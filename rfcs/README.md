# RFCs

An RFC (request for comments) proposes a larger change to Sesame Link and lets everyone discuss it
before anything is built. Sesame Link's source code is not public, so an RFC describes what people
would see and do, not how to implement it. If it is accepted, Sesame builds it.

## When to write one

Write an RFC for a change that needs a design, for example:

- a new workflow, such as reviewing a session's changes from a phone
- support for another coding tool or terminal
- a change to what remote clients or voice calls are allowed to do
- a new integration, such as notifications in another app

For a small improvement, such as a missing option or a clearer message,
[request a feature](https://github.com/SesameAILabs/link/issues/new?template=feature_request.yml)
instead. For a bug, [open a bug report](https://github.com/SesameAILabs/link/issues/new?template=bug_report.yml).

## How to propose one

1. Search [open RFC pull requests](https://github.com/SesameAILabs/link/pulls?q=is%3Apr+label%3Arfc)
   and [existing RFCs](.) for the same idea.
2. Fork this repository and copy [`0000-template.md`](0000-template.md) to
   `rfcs/0000-short-name.md`, where `short-name` describes the change in a few words.
3. Fill in the template. The problem section matters most: an RFC that explains a real problem
   well is useful even if the proposed solution changes.
4. Open a pull request that adds only that file. Then rename the file so `0000` becomes the pull
   request's number, padded to four digits, such as `0012-review-from-phone.md`.

## What happens next

Anyone can comment on the pull request. A maintainer replies within two weeks and may ask
questions or suggest changes. The RFC then ends in one of these states, recorded in its `Status`
line:

| Status | Meaning |
| --- | --- |
| Draft | Open for discussion. |
| Accepted | Merged. Sesame plans to build it, though not on a promised date. |
| Declined | The pull request is closed with the reason. |
| Withdrawn | The author closed it. |
| Shipped | Released. The `Shipped in` line names the release. |

An accepted RFC can still change while it is built. The [changelog](../CHANGELOG.md) records what
actually shipped.

## Before you submit

Don't include anything confidential, and only include content you have the right to share. By
opening a pull request, you agree that Sesame may use the ideas in it.
