# Contributing

Sesame Link's source code is not public. You can help by:

- **Reporting bugs** with the [issue forms](https://github.com/SesameAILabs/link/issues/new/choose).
  A report with the version, operating system, coding tool version, and steps to reproduce is
  usually fixable without follow-up questions.
- **Confirming other people's bugs.** Add a 👍 reaction, or a comment if your setup differs.
  Please don't post "+1" comments.
- **Requesting features** with the
  [feature request form](https://github.com/SesameAILabs/link/issues/new?template=feature_request.yml).
  We read 👍 reactions as votes.
- **Proposing larger changes** as an [RFC](rfcs/README.md) pull request.

Pull requests are accepted only for RFCs. Pull requests that change anything else are closed.

## How issues are handled

An automated assistant reads each new issue first. It adds labels, points out likely duplicates,
and asks for anything the form is missing. A maintainer then confirms the bug or asks for more.
Labels show where an issue stands:

| Label | Meaning |
| --- | --- |
| `needs-triage` | Not yet looked at by a maintainer. |
| `needs-info` | Waiting on the reporter. Closed after 30 days without a reply. |
| `confirmed` | Reproduced and tracked for a fix. |
| `upstream` | Caused by Claude Code, Codex, or Pi rather than Link. |
| `rfc` | A pull request proposing an RFC. |
| `fixed-in-next-release` | Fixed. The fix ships in the next release. |
