# mint-otlp-token

Mints a GitHub Actions OIDC JWT for `otlp.mondoo.love` and exposes it as env
vars to subsequent steps. After this action runs, any OTel SDK / `otel-cli` /
`curl` command in the same job can push traces, metrics, and logs to the
Mondoo LGTM stack with no shared secret.

## Quick start

```yaml
jobs:
  ci:
    runs-on: ubuntu-latest
    permissions:
      id-token: write   # required — without this the mint fails
      contents: read
    steps:
      - uses: mondoohq/.github/actions/mint-otlp-token@main

      # OTEL_EXPORTER_OTLP_ENDPOINT and OTEL_EXPORTER_OTLP_HEADERS
      # are now in $GITHUB_ENV; SDKs auto-discover them.
      - run: ./scripts/run-my-instrumented-app
```

## With otel-cli (wrap arbitrary commands in spans)

```yaml
- uses: mondoohq/.github/actions/mint-otlp-token@main

- name: Install otel-cli
  run: |
    curl -fsSL https://github.com/equinix-labs/otel-cli/releases/download/v0.4.5/otel-cli_0.4.5_linux_amd64.tar.gz \
      | sudo tar -xz -C /usr/local/bin otel-cli

- env:
    OTEL_SERVICE_NAME: my-build
  run: otel-cli exec --name "build" --kind client -- make build

- env:
    OTEL_SERVICE_NAME: my-build
  run: otel-cli exec --name "test"  --kind client -- make test
```

Result: one span per wrapped step in Grafana → Explore → Tempo,
queryable by `service.name = my-build`.

## With curl (manual metric push)

```yaml
- uses: mondoohq/.github/actions/mint-otlp-token@main

- name: Push a counter
  run: |
    NOW_NS=$(date +%s%N)
    curl --fail-with-body -sS -X POST "${OTEL_EXPORTER_OTLP_ENDPOINT}/v1/metrics" \
      -H "Authorization: Bearer ${OTLP_JWT}" \
      -H 'Content-Type: application/json' \
      --data-binary "$(jq -n --arg ts "$NOW_NS" --arg svc "${{ github.workflow }}" '
        {resourceMetrics:[{
          resource:{attributes:[{key:"service.name",value:{stringValue:$svc}}]},
          scopeMetrics:[{metrics:[{
            name:"my_deploys",
            sum:{aggregationTemporality:2,isMonotonic:true,
                 dataPoints:[{asInt:"1",timeUnixNano:$ts,startTimeUnixNano:$ts}]}
          }]}]
        }]}')"
```

## Inputs

| Input | Default | When to override |
|---|---|---|
| `audience` | `otlp.mondoo.love` | Only if the collector audience is renamed |
| `endpoint` | `https://otlp.mondoo.love` | Local testing against a different collector |

## What it sets in `$GITHUB_ENV`

| Var | Value |
|---|---|
| `OTLP_JWT` | The raw signed JWT (masked) |
| `OTEL_EXPORTER_OTLP_ENDPOINT` | `https://otlp.mondoo.love` |
| `OTEL_EXPORTER_OTLP_HEADERS` | `Authorization=Bearer <jwt>` |

## Caveats

- **Tokens are short-lived (~5 min).** Mint per job. If a job's push happens
  more than 5 minutes after the mint, re-invoke this action before pushing.
  In practice rarely a concern — most CI work fits in one token's lifetime.
- **Only `mondoohq/*` repos can push.** Caddy enforces
  `repository_owner=mondoohq` at the proxy layer; tokens from other orgs
  return `HTTP 403: Forbidden: repository_owner not in allowlist`.
- **`permissions: id-token: write` is mandatory.** Without it the runner
  doesn't inject the `ACTIONS_ID_TOKEN_REQUEST_*` env vars and the mint
  fails with a clear error message. `permissions:` defaults to
  `contents: read` only — easy to miss.
- **Works on both `ubuntu-latest` and the mondoohq self-hosted runners.**
  The OIDC issuer is the same in both.

## Where the telemetry lands

| Signal | Backend | How to query |
|---|---|---|
| Traces | Tempo | Grafana → Explore → Tempo → `service.name = ...` |
| Metrics | VictoriaMetrics | `<metric>_total{service_name="..."}` (counter convention) |
| Logs | VictoriaLogs | `{service_name="..."}` |
