# Getting help

## Fix it yourself first

```bash
sesame-link status
sesame-link doctor
```

`status` says whether this computer is connected and ends with the next command to run when
something needs fixing. `doctor` checks your setup and repairs what it can. If neither helps, find
your symptom in [Troubleshooting](https://link.sesame.com/docs/reference/troubleshooting/).

## Where to ask

| You have | Go to |
| --- | --- |
| Something broken | [Open an issue](https://github.com/SesameAILabs/link/issues/new/choose) after searching [existing ones](https://github.com/SesameAILabs/link/issues?q=is%3Aissue) |
| A question about how something works | [Ask a question](https://github.com/SesameAILabs/link/issues/new?template=question.yml) |
| A small improvement | [Request a feature](https://github.com/SesameAILabs/link/issues/new?template=feature_request.yml) |
| A larger change that needs a design | [Write an RFC](rfcs/README.md) |
| A mistake in the documentation | [Report a docs problem](https://github.com/SesameAILabs/link/issues/new?template=docs_problem.yml) |
| A security problem | Report it privately, as [SECURITY.md](SECURITY.md) describes |

Problems with the Sesame voice app outside the Sesame Link app go to Sesame's own support.

## What we need in a bug report

The issue form asks for these. Have them ready:

- `sesame-link --version`
- your operating system and version
- the coding tool and its version, such as `claude --version`
- where you saw the problem: the web app, a phone browser, a voice call, the menu bar, or the
  terminal
- the output of `sesame-link status`

Don't paste raw logs. Link's logs and your sessions can contain code, file paths, and prompts. Copy
only the lines that show the problem, and remove anything private.
