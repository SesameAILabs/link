# Security

Report security problems privately. Do not open a public issue or discussion for them.

## How to report

Use **Report a vulnerability** on this repository's
[Security tab](https://github.com/SesameAILabs/link/security/advisories/new). Only Sesame's
maintainers can read the report, and we reply there.

Include:

- the Sesame Link version (`sesame-link --version`) and your operating system
- what an attacker can do, and what they need first, such as network access, a stolen login, or
  access to your computer
- steps to reproduce

Don't include real session content, tokens, or other people's data. If you need to share logs, say
so in the report and we'll arrange a private way to send them.

## What's in scope

- the `sesame-link` CLI and background service, the installer, and the macOS menu-bar app
- the web app at [link.sesame.com](https://link.sesame.com) and the service behind it
- the Sesame Link app in the Sesame voice app

Problems in Claude Code, Codex, or Pi themselves belong with their own projects.

## Supported versions

Fixes ship in the next release. Run `sesame-link update` to install it; the
[changelog](CHANGELOG.md) says what changed.
