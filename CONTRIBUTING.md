# Contributing to Mondoo

Welcome to Mondoo! We're glad to have you here and excited to help you contribute to our projects. This document gives a basic outline of our processes and how you can contribute to our projects.

## Reporting Security Findings

If you believe you've discovered a security issue within one of the Mondoo projects, please see our [Security](SECURITY.md) guide for information on responsible disclosure.

## Reporting Issues

If you find something that's not working as expected, please let us know by submitting a GitHub issue in our project repositories. Each repository includes project specific issue templates to gather everything our team needs to triage your issue. Please take the time to fill out all the fields so we can reproduce your issue and get back to you quickly. If you're not sure that your problem is necessarily a bug or you have questions feel free to reach out to us on [GitHub Discussions](https://github.com/orgs/mondoohq/discussions).

## Making Contributions

Whether you have a great new idea for one of our projects or you just found a typo you can't let go, we're here to help you with your first contribution to a Mondoo project.

Before jumping in to make your change, please read about our Developer Certification of Origin (DCO) requirement and the process for opening your first pull request below.

### Developer Certification of Origin (DCO)

Software licenses are an essential component of every open source project. Licenses outline the legal rights afforded to users of projects and the rights provided to project maintainers when a copyright holder contributes new code.

The DCO is an attestation attached to every contribution made by every developer, ensuring the contributor has the right to contribute under the project's license. 

In the commit message of the contribution, the developer adds a Signed-off-by statement and thereby agrees to the DCO as shown below.


```
Developer's Certificate of Origin 1.1

By making a contribution to this project, I certify that:

(a) The contribution was created in whole or in part by me and I
    have the right to submit it under the open source license
    indicated in the file; or

(b) The contribution is based upon previous work that, to the
    best of my knowledge, is covered under an appropriate open
    source license and I have the right under that license to
    submit that work with modifications, whether created in whole
    or in part by me, under the same open source license (unless
    I am permitted to submit under a different license), as
    Indicated in the file; or

(c) The contribution was provided directly to me by some other
    person who certified (a), (b) or (c) and I have not modified
    it.

(d) I understand and agree that this project and the contribution
    are public and that a record of the contribution (including
    all personal information I submit with it, including my
    sign-off) is maintained indefinitely and may be redistributed
    consistent with this project or the open source license(s)
    involved.
```

#### DCO Sign-Off Methods

The DCO requires a sign-off message in the following format appears on each commit in the pull request:

```
Signed-off-by: Ziggy Stardust <ziggy@mondoo.com>
```

The DCO text can either be manually added to your commit body, or you can add either `-s' or `--signoff` to your usual git commit commands. If you are using the GitHub UI to make a change you can add the sign-off message directly to the commit message when creating the pull request. 

If you forget to add the sign-off you can also amend a previous commit with the sign-off by running `git commit --amend -s'. If you've pushed your changes to GitHub already you'll need to force push your branch after this with `git push -f`.

### Contribution Process

With licensing understood, let's continue on to your contribution!

1. Fork the project repo to your GitHub account.
2. Create a topic branch for your contribution with a short name that summarizes the change as best you can.
3. Make your change and commit it to your branch, making sure to include the DCO sign-off text in your contribution.
4. Push the change to your fork.
5. Open a pull request in the GitHub interface to the Mondoo project. Make sure to fill out all the required fields in the pull request so we can quickly review your change.
6. Sit back and relax while we review your proposed change. How long will that take? We aim to review all changes and respond within the week. Once a review has been completed and any changes are made, our team will merge your change so it can be part of our next release.

## Need Help?

If you still have questions on contributing or would like to chat about Mondoo in general please reach out to us on [Mondoo Community Slack](https://mondoo.link/slack).