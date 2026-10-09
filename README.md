# Sesame Link

Control Claude Code, Codex, and Pi sessions running on your own computer from a browser, your
phone, or a Sesame voice call. Sessions keep running locally; Link relays them to you and does not
store session content off your computer.

This repository is where you report problems, ask questions, suggest ideas, and download releases.
Sesame Link's source code is not public.

- **Documentation:** [link.sesame.com/docs](https://link.sesame.com/docs/)
- **Web app:** [link.sesame.com](https://link.sesame.com)
- **What changed:** [CHANGELOG.md](CHANGELOG.md) and [Releases](https://github.com/SesameAILabs/link/releases)

## Requirements

- macOS (Apple Silicon or Intel), or x86-64 Linux with glibc 2.31 or newer
- At least one supported coding tool, installed as a CLI on `PATH` and signed in:
  - [Claude Code](https://code.claude.com/docs/en/setup#install-claude-code) (`claude`), plus
    tmux 3.4 or newer (`brew install tmux` on macOS)
  - [Codex CLI](https://developers.openai.com/codex/cli/) (`codex`), signed in with `codex login`
    (preview)
  - [Pi](https://pi.dev/), plus tmux 3.4 or newer (preview)

The Claude and Codex desktop apps do not meet these requirements.

## Install

```bash
curl -fsSL https://storage.googleapis.com/sesame-link/install.sh | sh
```

The script is [install.sh](install.sh) in this repository. It downloads the release for your
computer and checks it against the release's published checksums. On macOS 13 or newer it also adds
the Sesame Link menu-bar app.

Then, from the folder you want new sessions to start in:

```bash
sesame-link auth login
```

Sign in with your Sesame account in the browser, enter the code shown in your terminal, and accept
the prompt to start Link. Open [link.sesame.com](https://link.sesame.com) from any device to see
your sessions. To add another computer, install and sign in there with the same account.

macOS releases are signed with Sesame's Developer ID and notarized by Apple. Linux releases are
unsigned preview builds. The [installation guide](https://link.sesame.com/docs/installation/)
covers updating and uninstalling.

## Talk to your sessions by voice

In the Sesame app, add the **Sesame Link** app to the character you want to call. Then call that
character and ask what your sessions are doing, or ask it to start or steer one. See
[Voice agents](https://link.sesame.com/docs/use/voice-agents/).

## Common commands

| Command | Purpose |
| --- | --- |
| `sesame-link status` | Check this computer's connection. |
| `sesame-link doctor` | Find and repair setup problems. |
| `sesame-link start` / `stop` / `restart` | Start, stop, or restart Link. |
| `sesame-link logs --follow` | Watch Link's log. |
| `sesame-link sessions` | List sessions and attach this terminal to one. |
| `sesame-link claude` / `codex` / `pi` | Start a session here and attach to it. |
| `sesame-link update` | Install the latest release. |

All commands are in the [command reference](https://link.sesame.com/docs/reference/commands/).

## Get help

1. Run `sesame-link status` and `sesame-link doctor`. Together they fix most setup problems.
2. Read [Troubleshooting](https://link.sesame.com/docs/reference/troubleshooting/).
3. Search [existing issues](https://github.com/SesameAILabs/link/issues?q=is%3Aissue), then
   [open one](https://github.com/SesameAILabs/link/issues/new/choose).

Ask questions and suggest ideas in [Discussions](https://github.com/SesameAILabs/link/discussions).
Report security problems privately, as [SECURITY.md](SECURITY.md) describes, and never in a public
issue.
