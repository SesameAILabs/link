# Changelog

What each Sesame Link release changes. The same notes are on the
[documentation's Changelog page](https://link.sesame.com/docs/changelog/) and attached to each
[GitHub release](https://github.com/SesameAILabs/link/releases).

## v0.1.37 — 2026-10-24

**Improved**

- When Sesame Link stops with an error, its error report names the step it stopped in, so
  repeated failures at start are easier to diagnose.
- When Sesame Link can't read your login shell's `PATH` at startup, its error report says why,
  such as a shell Link can't ask, like `nologin` or `tcsh`.

**Fixed**

- A Codex session no longer gets stuck after a turn fails with an error such as `Bad Request`: it
  returns to idle and sends your next message.
- A computer removed from your account while offline now stops trying to reconnect and reports
  that it was removed, instead of retrying every hour.
- `sesame-link start` and `sesame-link auth login` now refuse a `--state-directory` path that's
  too long, and say why, instead of restarting and failing silently.
- Stop is hidden in the web app whenever a Claude Code or Codex session can't be interrupted, such
  as while a message is still being sent.

**Staff preview**

- Cursor shell-command and MCP tool approvals can be answered with "always allow" from your phone
  or browser once you set `allow_cursor_persistent_rules` to true; it's off by default.
- Cursor's file deletion, web search, and MCP tool approvals are no longer missed after the agent
  edits a file, and MCP tool approvals can be answered remotely on Cursor 2026.10.22 or later.
- Cursor sessions no longer hold later messages behind one whose delivery couldn't be confirmed
  once a later message is delivered, and a message sent just as a background task finishes is no
  longer left unconfirmed.

## v0.1.36 — 2026-10-22

**New**

- `sesame-link config orca on` now also makes each Codex terminal Orca opens a Sesame Link
  session, and Orca keeps showing whether it's working, idle, or waiting.
- Pi asks whether to trust a new folder in the web app, the Sesame app, or `sesame-link pi`, and
  the session starts once you answer. Pi remembers your answer.

**Improved**

- You can answer Claude Code's "trust this folder?" prompt from your phone or browser even while a
  terminal is attached, as long as nobody is typing in it.
- With `sesame-link config claude-mod on`, interrupting from your phone or browser stops exactly
  the running turn on Claude Team and Enterprise accounts.
- Sesame Link's secure connections follow the latest certificate authority distrust decisions.

**Fixed**

- Claude Code sessions whose project folder under `~/.claude/projects` is a symbolic link no
  longer show "Stop hook error" after each reply, and can be compacted again.
- Claude Code 2.1.304 no longer shows a "hook was skipped" message in each session with
  `claude-mod` on.
- A Codex session's history stays readable after a turn is interrupted while one of its commands
  keeps running.
- Pi sessions start on Macs with account names longer than eight characters, instead of failing
  with "Link state directory is too long for a Pi socket".

**Staff preview**

- Cursor's Auto-review command prompts, web search approvals, and "Delete this file?" prompts can
  be answered from your phone or browser.
- A Cursor session waiting on something only the terminal can answer shows what the terminal
  shows, and a second message sent right after another is delivered in turn.
- Cursor sessions keep taking messages after Cursor summarizes the conversation.
- Cursor sessions on 2026.10.19 and earlier October builds no longer repeat their replies or end
  turns with "WritableIterable is closed"; they show no thoughts. Restart sessions started before
  the update.
- The menu-bar app lists Cursor with the other coding agents.

## v0.1.35 — 2026-10-20

**New**

- `sesame-link config orca on` makes each Claude Code terminal Orca starts a Sesame Link session,
  while Orca keeps showing whether it's working, idle, or waiting.
- `sesame-link claude "prompt"` sends the prompt once as the first message, and
  `sesame-link claude --resume <id>` reconnects to that conversation's session or continues it.
- A message stuck behind text left in Claude Code's input box can be released from your phone or
  browser; the cleared text is shown so you can copy it back.

**Improved**

- Up to 12 sessions run at once by default, up from 8, and stopped sessions no longer count
  toward the limit. A limit you set with `--max-sessions` is kept.
- A session waiting on a screen Sesame Link doesn't recognize, such as an MCP server approval,
  now shows that screen in your clients instead of sitting silently.
- `sesame-link start` says when this computer keeps Sesame Link offline while nobody is logged
  in, and on Linux offers to run `loginctl enable-linger`.
- Claude Code 2.1.303 is verified.

**Fixed**

- Sesame Link no longer stays offline after Sesame refuses its connection; it keeps retrying,
  and `sesame-link status` shows when.
- Two copies of Sesame Link signed in as the same computer no longer knock each other offline all
  day; `sesame-link status` says how to fix it.
- Messages sent from your phone or browser around a `/compact`, or while you type in the session's
  terminal, are now delivered or held for review correctly instead of getting stuck.
- Idle sessions with a terminal attached no longer make the web client say "Link couldn't confirm
  access to this session right now".
- Approving a plan from your phone or browser no longer fails while Claude is still writing it.

## v0.1.34 — 2026-10-18

**Improved**

- When a Codex session can't take a message right now, it says nothing was sent and what helps:
  waiting a minute, forking the session, or restarting Sesame Link.
- On macOS, Login Items in System Settings can list Sesame Link by name with its icon, instead
  of "Sesame AI, Inc.", when the menu-bar app is installed.

**Fixed**

- A Codex session no longer refuses every message until Sesame Link restarts after an earlier
  message's outcome was unclear; it recovers once the thread shows what happened.
- A fork of a compacted Claude Code conversation no longer holds every message after its first.
  Start a new fork to replace one that is already stuck.

## v0.1.33 — 2026-10-12

**New**

- You can fork a conversation into a new session, optionally on another model and in a new Git
  worktree, leaving the original untouched.
- Forks can start from an idle Claude Code or Pi session, a Codex session at any finished turn,
  an ended session, or a conversation you had outside Sesame Link.
- Forks need Claude Code 2.1.296, Pi 1.0.2, or Codex 0.154.0 or newer.

**Improved**

- `sesame-link restart` now also restarts Codex after you install a new Codex release, so
  features that need it, such as Codex forks, turn on.
- `sesame-link diagnostics` no longer overwrites an existing file; move it aside or pass another
  `--output`.

**Fixed**

- Messages no longer stay unconfirmed or hold later messages after you submit them yourself in a
  busy Claude Code session.
- Messages reach Claude Code more reliably while Claude has queued messages or has just compacted
  its context.
- A fork's first message no longer stays listed as queued after it appears.
- Workflow agents that were stopped, paused, or cut short are no longer listed as still working.
- `sesame-link uninstall` right after `stop` no longer occasionally fails with "Couldn't keep
  Sesame Link stopped".
- Session folders and slash commands are handled more safely.

**Staff preview**

- Cursor sessions say to run `agent login` when the Cursor CLI is logged out, in
  `sesame-link status`, the New session dialog, and at start.
- Cursor Plan mode shows the "Ready to build?" prompt as waiting on the terminal, and later
  messages are delivered after a plan you didn't build.
- Only the terminal can trust a Cursor workspace; remote clients can't answer that prompt.

## v0.1.32 — 2026-10-11

**Improved**

- When image storage fills up, new uploads replace the oldest images already sent instead of
  being refused; removed images stay in the message history, marked as removed.

**Fixed**

- `sesame-link update` no longer hangs on a build that doesn't answer `--version`; it gives up,
  reports why, and keeps the installed build.

## v0.1.31 — 2026-10-10

**Action needed:** Remote updates are now on by default, so browsers you link can update Sesame
Link without asking. Turn them off with `sesame-link config remote-update off`.

**New**

- `sesame-link auth login` fills in the sign-in code for you and starts Sesame Link as soon as
  you're signed in.
- Remote clients can show which sessions have unread results, even when no browser is connected.
- `sesame-link --help` and the menu-bar app link to the public issue tracker at
  https://github.com/SesameAILabs/link/issues.

**Improved**

- Sign-in and pairing codes are bold instead of reversed, so selecting one to copy it is visible.

**Fixed**

- Messages reach Claude Code sessions in more layouts, including don't-ask mode, custom status
  lines, and narrow panes.
- Messages no longer get stuck unsent after a browser terminal opens on a Claude Code session.
- A Codex error that Codex won't retry no longer leaves the session refusing new messages.
- On macOS, `sesame-link restart` and `start` no longer fail with "Bootstrap failed: 5" while the
  previous Sesame Link is still exiting.

**Removed**

- Sessions Sesame Link starts keep Claude Code's screen-reader mode off, and `--ax-screen-reader`
  is no longer accepted.

**Staff preview**

- Cursor sessions can start with Auto-review or Run Everything; Run Everything needs
  `"run_everything"` in `allowed_cursor_approval_modes` in `remote-access.json`.
- Messages reach Cursor sessions in Auto-review and Run Everything modes, and when the branch has
  a pull request open.
- Remote clients can answer Cursor's command prompts after the agent has edited files.
- Cursor sessions list their pull requests and can name themselves.

## v0.1.30 — 2026-10-08

**Fixed**

- Messages reach Claude Code sessions again when ultracode is on.
- Herdr tabs Sesame Link opens now close when their session ends.

**Staff preview**

- `sesame-link cursor` starts Cursor as a Sesame Link session that remote clients can start,
  message, and interrupt.
- `allowed_remote_runtimes` in `remote-access.json` limits which tools remote clients can start
  sessions with.

## v0.1.29 — 2026-10-08

**New**

- Sesame Link works with Herdr: `/open-sesame` continues in the same pane, sessions show their
  state in Herdr, and new sessions open as background Herdr tabs while Herdr is open.
- `sesame-link config claude-mod on` delivers remote messages through a Claude Code mod on Claude
  Code 2.1.293 or later. Off by default.
- Update notices link straight to the new version's changelog entry.

**Improved**

- `sesame-link start` keeps your `CLAUDE_CONFIG_DIR`, so sessions use that configuration and
  sign-in.
- Remote clients show the Claude Code, Codex, or Pi version each session actually runs.
- On macOS, restarting works better with Apple Terminal.

## v0.1.28 — 2026-10-07

**New**

- Send PNG and JPEG images to Claude Code, Codex, and image-capable Pi models.
- Install Sesame Link with only Pi installed; Pi sessions need tmux 3.4 or newer.
- `sesame-link status --json --watch` follows connection changes as they happen.

**Improved**

- Setup no longer offers Claude Code commands on computers without Claude Code.
- `--open-terminal` opens new session tabs without taking focus from what you're using.

**Fixed**

- Remote messages no longer stay queued after a Claude Code subagent finishes.

## v0.1.27 — 2026-10-06

**Action needed:** Run `sesame-link auth login` once after upgrading. Approving pairings and
links, or removing a machine, may now ask you to sign in again first.

**New**

- Sesame Link remembers whether it was running across restarts and starts at login through your
  service manager.
- Browser terminal access can open a shell in the session's folder; it closes with its browser
  panel.

**Improved**

- The web app moved to https://link.sesame.com.
- Error messages are in plain language and name the command to run next.
- `sesame-link sessions` describes each state in words, such as "waiting" or "needs attention".
- Sesame Link finds Claude Code, Codex, Pi, tmux, and your other tools even when your login shell
  is slow.
- The saved login renews itself; if a command says it was used elsewhere, sign in again.
- Removing a Codex session closes its terminal, and `sesame-link uninstall` also stops Codex's
  server.
- Browser terminals send every key to the session and no longer show tmux's status line.
- Remote clients get a fairer share of resources, and long sessions or huge output no longer slow
  Sesame Link down.

**Fixed**

- Sessions keep accepting messages after one joins a running Claude turn, and quick messages keep
  their order.
- A session refuses to start in a folder it can't read, such as Documents over SSH, and says how
  to grant access.
- Attaching works from terminals this computer doesn't know, such as Ghostty or kitty over SSH.
- `machine approve`, `machine deny`, `machine revoke`, and `connect approve` work again.
- Terminals no longer show `select-pane ... returned 127` after a remote message.
- `sessions attach` attaches only to the session it names.
- On Ubuntu, new sessions open in the desktop terminal, including over SSH.
- Pull requests opened with a heredoc description now show up on their session.
- Installs and updates work with BusyBox unzip.

**Removed**

- `sesame-link connect web` no longer opens the web app; `auth login` already signs your browser
  in.

## v0.1.26 — 2026-10-04

**New**

- `sesame-link update` verifies the release's signature itself before installing. The first
  update checked this way is the one after this release.
- Browsers you link can update Sesame Link; you'll be asked once whether to allow it.
- `sesame-link config` lists and changes this computer's settings.
  `sesame-link error-reporting` is now `sesame-link config error-reporting`.
- Remote clients can run `/compact`, `/clear`, `/context`, and `/reload-skills` in Claude Code
  sessions; change the list with `allowed_remote_slash_commands` in `remote-access.json`.
- Sessions list the GitHub pull requests they open or push to; turn lookups off with
  `sesame-link config pull-requests off`.
- Session histories include the model's thinking where available; turn it off with
  `sesame-link config thinking off`.

**Improved**

- Sessions find the same `node`, `python`, and other tools as your terminal.
- Pi sessions run stock Pi, with model and thinking choices, `/compact`, and streamed replies.
- The installer accepts `SESAME_LINK_ENVIRONMENT=staging`.
- Sesame Link uses less of your computer's resources.
- The web app starts new sessions on your last computer and runtime and works better on phones.

**Fixed**

- Queued messages no longer get stuck after a local Claude Code command such as `/mcp`.

**Removed**

- The experimental Ghostty backend for Claude sessions; Ghostty tabs still work.
- `sesame-link account bootstrap`, `account import`, `account rotate`, and
  `--account-credential-file`.

**Staff preview**

- Open a session's terminal in the web app after `sesame-link config terminal-access on`.

## v0.1.25 — 2026-10-02

**New**

- Clients approved with `sesame-link connect` reach only the machine that started the link; use
  `--all-machines` or `--machine` for more, and `--expires-in-hours` to limit access. Clients
  approved earlier keep the reach they were given.
- `sesame-link sessions` is an interactive list you can browse, filter, and attach from.
- `restart` and `update` offer to restart Codex when it runs an older build.

**Improved**

- Waits show a spinner and elapsed time, and `update` shows a download progress bar.
- Sessions nobody is watching no longer upload their activity.

**Fixed**

- Claude sessions no longer stay `running` with messages stuck in the queue.
- Subagents that end on an API error no longer stay listed as working.
- With `--open-terminal`, sessions started while the screen is locked get their tab after unlock.

## v0.1.24 — 2026-10-01

**New**

- New installations connect to production by default; existing installations keep their
  environment.
- `sesame-link` with no command shows a welcome screen with this computer's state and next steps.

**Improved**

- `status` opens with one line saying whether you're connected; `--verbose` and `--json` give the
  rest.
- Errors lead with what failed and the command to run.
- Codex installed with npm is supported.

**Fixed**

- Messages to Claude Code sessions with background tasks no longer stop for review.
- Stopped subagents and ending sessions no longer show as working.
- Codex sessions stay usable through brief terminal check failures.

## v0.1.23 — 2026-09-30

**New**

- Answer Codex permission requests and questions, MCP forms, and Codex app approvals from a remote
  client. Some request types still need the terminal.

**Improved**

- Agents no longer repeat their final answer in a tool call; restart running sessions to pick this
  up.

## v0.1.22 — 2026-09-30

**New**

- macOS binaries are signed with Sesame's Developer ID and notarized by Apple. The Linux binary is still unsigned.
- `sesame-link codex` starts Codex as a Sesame Link session.
- `sesame-link logs` prints the daemon log; `--follow` keeps printing it.

**Improved**

- `--help` groups commands, and `auth login` offers to start Sesame Link once you're signed in.
- Codex sessions report each tool by the name Codex gives it.

**Fixed**

- Codex sessions start again with Codex 0.157 and later.

## v0.1.21 — 2026-09-29

**New**

- Sign in once with `sesame-link auth login`; `auth status` and `auth logout` manage it.
- `sesame-link uninstall` stops Sesame Link, unpairs the machine, and removes the binary, keeping
  your history.

**Improved**

- Sesame Link finds `claude` and `codex` fresh on each start, so reinstalling either takes effect
  at the next restart.
- Ending a session keeps its history.
- With `--open-terminal` set to Ghostty or iTerm, new tabs open in your Sesame Link window.

**Fixed**

- Very large Codex tool results no longer break the session.
- Codex sessions keep accepting messages on a busy computer.

## v0.1.20 — 2026-09-27

**Action needed:** Run `sesame-link restart` after installing, and restart running Claude sessions
when you can. `--daemon-address` is removed.

**New**

- Sesame Link notices when Claude Code is logged out and tells you to run `claude /login`.
- Messages sent during a Codex turn go into that turn, as they do for Claude.
- `--open-terminal` also opens a terminal for new Codex sessions on macOS.

**Improved**

- Remote clients are held more closely to the limits you set. Remote messages can't start with `/`
  or `@`-mention files outside the session's folder.
- Saving a Claude "always allow" rule remotely is off unless you allow it in `remote-access.json`.
- Running subagents show in a session's activity, with what each is doing.
- Failed or interrupted turns are reported as such instead of as completed.
- Sessions that end on the computer end in remote clients right away.
- Claude Code 2.1.283 is verified.

**Fixed**

- Messages are no longer set aside for review after background tasks report back or after
  `/fork`.
- Messages reach sessions in narrow terminals.
- Sessions started from inside Claude Code keep their transcript.
- Sesame Link connects over IPv4 when IPv6 isn't working.
- Terminals opened with `--open-terminal` close when their session ends.

## v0.1.19 — 2026-09-25

**New**

- Sesame Link sends crash and error reports, never session content or file paths. Turn them off
  with `sesame-link error-reporting off`, then restart Sesame Link.
- New sessions offer folders you've worked in before.

**Improved**

- A message stuck behind something in Claude's terminal is reported, with a preview and the
  command to attach.
- Claude knows when a message comes from your remote client, and answers voice calls in a form
  that reads well aloud.
- With `--open-terminal`, Ghostty opens new sessions as tabs, and iTerm is supported.
- Sesame Link reconnects within a second after the service restarts.
- Commands check that your credentials are private and say how to fix them.

**Fixed**

- Claude's auto-mode and feedback prompts no longer hold up remote messages in Sesame Link
  sessions.
- Interrupting Claude before it replies marks the message interrupted instead of sent.
- Deleting a session that's still starting no longer leaves it running.

## v0.1.18 — 2026-09-24

**Action needed:** If Sesame Link won't start because of a retired Gateway profile, move the
configuration aside, run `sesame-link setup`, then `sesame-link start`.

**New**

- `/open-sesame` moves a Claude Code conversation into a Sesame Link session.
- `sesame-link diagnostics` writes a shareable archive with credentials removed.
- Messages sent while Claude is working join its queue right away (Claude Code 2.1.247 or later).

**Improved**

- Attached terminals are titled with the session name.

**Fixed**

- Running Claude sessions stay running after a Sesame Link restart.
- A crash or full disk no longer leaves Sesame Link unable to start.
- Sessions with non-ASCII paths recover correctly after a restart.

## v0.1.17 — 2026-09-23

**New**

- `sesame-link claude` starts Claude Code as a Sesame Link session with your arguments.
- On macOS, new sessions open in their own terminal window; choose the app with
  `--open-terminal`.
- Sessions you start on your computer appear in the web app and the Sesame app.
- Codex sessions can name themselves.

**Improved**

- Remote messages reach Claude more reliably, and a stuck tmux can no longer hold them forever.
- `sessions` shows a table, and `status` uses wide terminals better.
- Suggested commands keep your `--state-directory`.

**Fixed**

- Stop and delete never act on a similarly named session.

## v0.1.16 — 2026-09-22

**New**

- `sesame-link sessions attach` works for Codex sessions.

**Improved**

- Queued messages wait their turn and send in order instead of going to review.
- `sesame-link unpair` removes the machine from your account's lists.
- Installer output is easier to read.

**Fixed**

- `sesame-link machine revoke` works again.
- Codex terminals no longer reconnect every second.

**Removed**

- Sesame Link no longer opens terminals for remote clients; the web app's Terminal button copies
  the attach command instead.

## v0.1.15 — 2026-09-21

**Improved**

- Remote clients can show the text of messages other clients queued.

## v0.1.14 — 2026-09-20

**Action needed:** Codex sessions need Codex CLI 0.154.0 or newer.

**New**

- Stop and remove sessions, and archive or delete Codex history.
- Run up to eight sessions at once; new sessions are refused when memory is low.

**Improved**

- Codex sessions reconnect after interruptions without replaying work.
- Claude sessions keep their history when entering or leaving a worktree.

**Fixed**

- Queued messages no longer get stuck after a background notification.

## v0.1.13 — 2026-09-18

**New**

- `sesame-link doctor --environment production` selects the production service.

**Improved**

- Message delivery tracking survives restarts and gives fewer false warnings.
- Claude Code answers are shorter and voice-friendly, and session names are two to four words.

## v0.1.12 — 2026-09-17

**Fixed**

- Messages reach sessions whose Claude prompt shows an IDE file or selection.
- A trailing space no longer causes a false "delivery unconfirmed" warning.
- Model, effort, and permission changes survive a Claude Code update.

## v0.1.11 — 2026-09-17

**New**

- `sesame-link update` installs the latest release and offers to restart Sesame Link.

## v0.1.10 — 2026-09-16

**Action needed:** Run `sesame-link stop` then `sesame-link start` once after upgrading, so
`restart` can work.

**New**

- `sesame-link restart` restarts Sesame Link with the options it was started with.
- `sesame-link status` says when a newer release is out or the running build doesn't match.

**Improved**

- Command output is easier to scan and respects `NO_COLOR`.

**Fixed**

- Removed or failed sessions no longer leave folders behind.

## v0.1.9 — 2026-09-16

**Action needed:** `sesame-link sessions <session>` is now `sesame-link sessions attach
<session>`; update any scripts or aliases.

**New**

- `sesame-link sessions` has `attach`, `create`, and `delete`.

**Fixed**

- Queued messages reach Claude when its prompt shows an IDE file or selection.

## v0.1.8 — 2026-09-15

**Action needed:** Upgrade the approving machine before pairing a new one. Stop Sesame Link before
unpairing, and after pairing again run `sesame-link machine associate-session <id>` to restore
remote access to existing sessions.

**New**

- `sesame-link sessions` lists local sessions and attaches to them.

**Improved**

- Message delivery survives restarts and never resends uncertain text automatically.
- Claude sessions stay connected across `/clear` and `/resume`.
- Plan approvals check you're approving the plan you saw.
- Machine approval shows the requesting machine and how old the request is.
- The web app recovers live updates without losing drafts.

**Removed**

- Remote messages starting with `!` are refused.

## v0.1.7 — 2026-09-02

**New**

- Codex sessions: create, message, interrupt, and approve them remotely, and open them in the
  Codex terminal.
- The Sesame voice remote works with both Claude Code and Codex.

**Improved**

- New sessions show every setting explicitly and remember your last choices.
- The web app shows Codex settings and handles Codex approvals.
- The web app works better on phones.
- Shell commands show as terminal input and output.

**Fixed**

- No more duplicate "Delivery uncertain" messages, or false ones for messages ending in a newline.

## v0.1.6 — 2026-08-30

**Action needed:** Finish or end running sessions before upgrading; sessions already running are
left unmanaged.

**New**

- Create and message Codex sessions from a remote client (preview).
- Releases publish their own notes.

**Improved**

- Claude's workspace-trust dialog is recognized more reliably.
- New sessions no longer warn while waiting for their first message.

**Fixed**

- Interrupt recovery no longer closes a turn that's still running.
