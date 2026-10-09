# Changelog

What each Sesame Link release changes. The same notes are on the
[documentation's Changelog page](https://link.sesame.com/docs/changelog/) and attached to each
[GitHub release](https://github.com/SesameAILabs/link/releases).

## v0.1.30 — 2026-10-08

- Messages from remote clients reach Claude Code sessions again when Claude Code labels the line
  above its message box, as it does with ultracode on. They used to wait in the queue and never
  arrive, because the label stopped Sesame Link recognizing the message box.
- A Herdr tab Link opens for a new session now closes when the session ends, as Ghostty and
  Terminal windows do, instead of leaving a shell behind.
- Staff preview: `sesame-link cursor` starts the installed Cursor CLI in the current folder as a
  Sesame Link session, with `--mode`, and attaches its terminal; Cursor keeps its own model setting.
  `sesame-link status` lists Cursor, and remote clients can start, message, and interrupt Cursor
  sessions; Cursor's own prompts still decide what runs. A new `allowed_remote_runtimes` setting in
  `remote-access.json` lists the tools remote clients may start sessions with, all by default.

## v0.1.29 — 2026-10-08

- On macOS, improve restart support via Apple Terminal
- Sessions work with Herdr:
  - `/open-sesame` in a Claude Code running in a Herdr pane continues the conversation in that
    same pane.
  - A session attached in a Herdr pane shows its working, idle, or needs-you state in Herdr and is
    attached again after Herdr restarts.
  - While Herdr is open, new Claude Code and Codex sessions, including ones started from your phone
    or browser, open as a background Herdr tab in the workspace already in the session's folder,
    or in a new one. `--open-terminal herdr` uses only Herdr.
- `sesame-link start` keeps the `CLAUDE_CONFIG_DIR` it runs with, so Claude Code sessions use that
  configuration directory and its sign-in, and Sesame Link reads their history from it.
- Remote clients see the Claude Code, Pi, or Codex version each session actually runs, rather than
  the version installed since it started.
- `sesame-link config claude-mod on|off` (off by default) delivers remote messages and cancels
  through a Claude Code mod on Claude Code 2.1.293 or later, keeping who sent each message.
  Messages fall back to the terminal when the mod is unavailable or a message needs Claude Code's
  own file expansion.
- Update notices and `sesame-link update` link directly to the new version's changelog entry.

## v0.1.28 — 2026-10-07

- Install Sesame Link on computers with only Pi installed. Pi sessions require tmux 3.4 or newer;
  the installer checks it before downloading.
- On computers without Claude Code, setup and the welcome screen no longer offer Claude Code
  commands, and the installer reports which runtime is missing without assuming only Codex can run.
- Send PNG and JPEG images to Claude Code, Codex, and image-capable Pi models. Images stay in
  private session storage on your computer and appear in remote conversation history.
- Remote messages no longer stay queued after a Claude Code subagent finishes.
- With `--open-terminal`, new session tabs and windows open without taking focus from the app or
  terminal tab you are using.
- Use `sesame-link status --json --watch` to follow connection changes as they happen.

## v0.1.27 — 2026-10-06

- The web app moved to `https://link.sesame.com`. Setup, sign-in, `status`, and the docs links now
  send you there.
- Commands explain problems in plain language and name the command to run next:
  - Session commands cover the session limit, low memory, a folder macOS blocks, and a missing
    Claude Code or tmux. `sesame-link sessions` shows each state in words ("waiting", "needs
    attention", "stopped") and says "No sessions yet." when there are none.
  - Sign-in, pairing, connect, unpair, and uninstall messages do the same, and the installer no
    longer prints runtime notes that don't apply or a restart reminder on a fresh install.
  - `status` and other commands tell a computer that isn't signed in to run
    `sesame-link auth login`, `status` keeps reporting when Sesame Link stops answering, and
    status, start, stop, restart, update, logs, config, and doctor describe problems the same way.
  - `sesame-link --help` describes every command and option, including ones that had no
    description, such as `--listen` and `pi --model`.
- Sesame Link finds your tools when your login shell is slow:
  - It waits up to a minute for your login shell's `PATH` when it starts, and if the shell still
    has not answered, keeps asking in the background and makes Claude Code, Codex, Pi, and tmux
    available to new sessions once it does. `sesame-link status --verbose` says `asking again`
    while it waits.
  - A session whose folder's login shell is slow to report its `PATH` uses the one Sesame Link
    read when it started, so `node`-based hooks such as the Vercel plugin's keep working.
  - Session terminals find Homebrew, mise, and other installed tools as a new terminal does. A
    terminal server already running keeps its `PATH` until it ends.
  - Terminals no longer show `'tmux -L 'sesame-link' select-pane ...' returned 127` after a remote
    message is typed into them.
- Sesame Link remembers whether it was started or stopped across computer restarts, reconnects
  after login through your macOS or Linux service manager, and picks up its sessions again without
  replaying work or discarding local history. Without a usable service manager, such as in a
  container or over SSH on Linux without a user systemd, `start` and `restart` run Sesame Link in
  the background instead and say it won't start at login.
- A session no longer starts in a folder its sessions can't read. On macOS that includes
  Documents, Desktop, and Downloads when Sesame Link was started from an app without access to
  them, such as over SSH: the refusal says which access sessions use and how to give it, instead
  of the session failing with "Operation not permitted".
- Attach to sessions from terminals whose type this machine does not know, such as Ghostty, kitty,
  or WezTerm reached over SSH: `sesame-link sessions` and the `claude`, `codex`, and `pi` commands
  attach as `xterm-256color` and say so instead of failing with "missing or unsuitable terminal".
- Remote messages:
  - A session keeps accepting remote messages after one joins a running Claude turn; previously it
    could stop taking any more messages.
  - Messages sent in quick succession keep their order across Claude Code, Codex, and Pi. When the
    conversation changed in between, Sesame Link asks you to review them first.
  - Locally started Pi sessions show the correct agent name and controls in remote clients.
- Codex:
  - An older Codex runtime restarts automatically when it is safe to, and asks first when Sesame
    Link cannot tell.
  - Removing a Codex session from Link closes its Codex terminal once no window shows it. A
    terminal open in a window stays open until you close it.
  - `sesame-link uninstall` also stops Codex's server. Running Codex turns end and open Codex
    windows disconnect; the conversations stay in Codex.
- Reliability:
  - Remote clients are held more closely to their own share of Sesame Link's resources, so one
    cannot crowd out the others or clients on this computer.
  - Long-running sessions and very large command output no longer slow Sesame Link down.
  - Remote messages that could stall Sesame Link are refused.
  - Installation and self-updates work with BusyBox unzip.
- The saved account login is more secure:
  - It renews itself regularly. If a command says the login was used elsewhere, run
    `sesame-link auth login` to sign in again.
  - Approving a pairing or a browser link, or removing a machine, may ask you to sign in with
    Sesame again first. `sesame-link` opens the browser for you, then finishes the action. This
    applies to logins made with this release, so run `sesame-link auth login` once after
    upgrading.
- Account commands such as `machine approve`, `machine deny`, `machine revoke`, and
  `connect approve` work again, instead of failing with "the Management API response for account
  read was invalid".
- Browser terminals:
  - Browser terminal access can open a shell: your login shell in the session's working folder,
    which ends when its browser panel closes, together with every job it started. The
    `terminal-access on` warning says so.
  - Browser terminals send every key, including the tmux prefix, to the session's program, so they
    can no longer open or switch tmux windows, and they no longer show the tmux status line. Drag
    to select text, since tmux no longer captures the mouse there.
  - A terminal access setting Link can't use leaves access off, and
    `sesame-link config terminal-access` says why. It no longer stops the daemon from starting.
- `sesame-link connect web` no longer opens the web client: `auth login` already leaves its browser
  signed in there, and `sesame-link` and `status` show the web app address. `connect web` now runs
  the advanced link ceremony like any other client name.
- `sesame-link sessions attach` attaches only to the session it names. If that session has just
  ended, the attach fails instead of joining another session whose name starts the same way.
- On Ubuntu, new sessions open attached in the preferred desktop terminal, including when Link
  starts over SSH while a graphical desktop is logged in.
- A session that opens a pull request with `gh pr create` after writing its description through a
  heredoc now lists that pull request; Claude sessions also use Claude's own report of the pull
  request it created.
- A message or session start carrying another runtime's settings is refused with an error that
  names both runtimes, and a Pi message that acknowledges unresolved deliveries is refused rather
  than having the acknowledgement silently dropped.
- Smaller CLI fixes:
  - The welcome screen marks the connection as `status` does (a reconnecting machine shows `!`, a
    revoked machine or refused configuration a red `✗`), and the `sessions` table colors each state
    as the session picker does.
  - Pressing Ctrl-D at the `/open-sesame` install offer after `setup` no longer installs the
    command, and every yes-or-no question shows its answers as `(y/N)` or `(Y/n)`.
  - Spinners no longer hide the terminal cursor, so an interrupted command can never leave it
    hidden.

## v0.1.26 — 2026-10-04

- Updates are now verified by the binary you already run. `sesame-link update` downloads the
  release itself and checks its manifest's signature against the release key built into the
  binary, the archive against that manifest, and on macOS Sesame's Developer ID signature, instead
  of running the channel's installer. Linux update archives are verified against the signed
  manifest too. This release is the first to publish signed manifests, so the first update checked
  this way is the one after it.
- Sesame Link can be updated from the web app. Enrolling a computer, and the next `start`,
  `restart`, or `status` on one enrolled earlier, asks whether browsers you link may update it
  (default yes); until you answer, they cannot. The web app offers updates once browsers are
  linked with the new permission, which happens after this release is out.
- `sesame-link config` lists this computer's settings, and `sesame-link config <setting> on|off`
  changes one: `error-reporting`, `remote-update`, `thinking`, `terminal-access`, and
  `pull-requests`. `sesame-link error-reporting` is now `sesame-link config error-reporting`.
- Sessions find the same `node`, `python`, and other version-managed tools as a terminal:
  - Claude and Pi sessions run with the PATH your login shell sets up in the session's workspace,
    read in the background at startup for recently used workspaces. If your shell does not answer
    within five seconds, sessions keep the PATH they inherit.
  - The Codex runtime starts with your login shell's PATH. A runtime already running keeps its old
    PATH until you restart it.
  - Sesame Link finds Claude Code, Codex, and Pi, and checks their versions, with that PATH. An
    installed Pi that cannot run is reported unavailable, and `sesame-link status` says what to
    fix; `status --verbose` shows which PATH runtimes were found with.
- Pi sessions (preview) are now stock Pi with native terminal access, remote prompts, streamed
  responses, interruption, and history:
  - Remote messages are confirmed once Pi writes them into its history, show as sent without a
    false warning, and finish when Pi settles.
  - Choose Pi's model and thinking level when creating a session, and switch either while it runs;
    each model offers only the levels Pi supports for it.
  - Remote clients can run `/compact` in a Pi session, and `/reload` when the computer's owner
    adds `reload` to `allowed_remote_slash_commands`.
  - Long histories stay readable beyond 32 MiB, and a malformed Pi state response reports the
    integration unavailable instead of replacing the session's identity.
- Remote clients can run built-in slash commands in a Claude Code session by sending the command as
  the whole message: `/compact` (with optional instructions), `/clear`, `/context`, and
  `/reload-skills` by default. The computer's owner changes the list with
  `allowed_remote_slash_commands` in `remote-access.json`, for example adding `autocompact` or
  `reload-plugins`, and can let messages starting with `!` run as shell commands with
  `allow_remote_bash_commands`, which is off by default. A command is refused when a workspace,
  personal, or plugin command or skill of the same name would run instead, and refusals name the
  setting to change. The daemon tells clients which commands it allows, so the web app suggests
  them as you type.
- Queued messages no longer get stuck after Claude Code finishes a local command, such as `/mcp`
  authentication.
- Sessions remember the GitHub pull requests they open or push to, and remote clients list them
  beside each session. Their status is looked up through this computer's own `gh` when a session
  acts or a client refreshes, never on a timer; `sesame-link config pull-requests off` turns lookups
  off.
- Session histories include the model's thinking where the runtime kept readable text: Pi's
  thinking, Claude Code's thinking summaries, and Codex reasoning summaries (which no longer appear
  as a tool named "reasoning"). Voice never reads it; `sesame-link config thinking off` leaves it
  out.
- Browser terminals (staff preview): attach to an existing Claude or coordinated Codex session's
  terminal from the web app after turning it on with `sesame-link config terminal-access on`, which
  warns what it grants. Output is bounded and viewers are cleaned up automatically.
- The installer accepts `SESAME_LINK_ENVIRONMENT=staging`, which the staging web app and docs set,
  to select staging before sign-in. A computer already paired with production keeps it; the
  installer says why and lists the step to select staging later.
- The daemon reports background shell tasks still awaiting their outcome, whether the computer runs
  macOS or Linux, and the installed Pi, Claude Code, and Codex versions, so remote clients can show
  them.
- After a Sesame Link service deploy, the daemon reconnects at a random moment within a window the
  service names instead of in the same second as every other computer. On a large fleet,
  reconnecting can take up to a couple of minutes.
- Lighter on the computer: sessions sharing a working directory reuse Git observations, Codex
  history reads take fewer round trips, and Pi history reads reuse unchanged work.
- When a session recovered after a daemon restart takes more than five seconds to show an empty
  composer, the daemon log records what Link read on its pane and how long it waited, without the
  pane's text.
- Removed: the experimental native Ghostty backend for Claude sessions (Claude runs in tmux, and
  Ghostty attachment tabs still work), and the retired `sesame-link account bootstrap`,
  `account import`, and `account rotate` commands and the `--account-credential-file` option. Owner
  operations use the login from `sesame-link auth login`.
- The web app, deployed alongside this release:
  - New session starts on the computer and runtime of your most recent session, on any device;
    offers every available runtime in one row, with the Pi preview last; and opens the new session
    with its message box focused, including on a phone.
  - A start whose outcome is uncertain closes the dialog onto that session with the warning, so a
    second Create cannot start a duplicate.
  - On phones: full-screen sheets and their pages get their own Back steps; Back from a session
    returns to the list at its previous scroll position; a session that cannot open returns to the
    list with the reason; the header and composer stay visible while the keyboard is up; and form
    fields no longer make iOS Safari zoom.
  - Decision cards read Deny then Allow, with full touch targets on phones. Reply settings have
    Cancel and Done, and the queued follow-ups confirmation reads Don't send and Send anyway.

## v0.1.25 — 2026-10-02

- A client approved with `sesame-link connect` now reaches only the machine that started the
  link, instead of every machine on the account. Pass `--all-machines` to let it reach every
  machine on the account, including machines paired later, or `--machine <machine_id>` (repeat it
  to name several) to choose machines, and `--expires-in-hours <hours>` to end its access
  automatically. Clients approved before this release keep the reach they were given.
- A Claude session no longer stays `running`, with remote messages stuck as queued, after Claude
  leaves one of its own background notifications in its input queue.
- A Claude Code background subagent whose run ends on an API error, such as during a network
  outage, no longer stays listed as working in the web client; a resumed subagent shows as
  working again.
- With `--open-terminal`, a session started while the Mac's screen is locked now gets its
  terminal tab once the screen unlocks, and a tab is logged as opened only after it actually
  attaches to the session.
- The daemon sends a session's activity to the Sesame Link service only while a remote client is
  watching that session, instead of streaming every session all the time. Remote clients see the
  same events as before; sessions nobody has open stop using upload bandwidth.
- `sesame-link restart` and `sesame-link update` ask whether to restart the Codex runtime too when
  it runs an earlier Link build, so Codex picks up the new build without stopping processes by
  hand. The question names the active Codex turns and pending approvals the restart ends; open
  Codex windows disconnect, and sessions come back from their history. Pass `--restart-codex` to
  `restart`, or with `--restart` to `update`, to answer yes in advance; without a terminal, or
  with `update --restart` alone, nothing is asked and Codex keeps its build.
- The CLI is easier to follow while it works:
  - `sesame-link sessions` on a terminal is an arrow-key list: move with the arrow keys or
    `j`/`k`, press Enter to attach, type `/` to filter by name, ID, or directory, press `n` for
    the command that creates a session, and Esc or `q` to leave. Piped output still prints the
    table, and a `TERM=dumb` terminal keeps the typed prompt.
  - Waits show a spinner with the elapsed time, and the sign-in and browser waits show when the
    code expires: `auth login`, `start`, `claude`, `codex`, `diagnostics`, and `connect`.
    `connect` shows one heading per step and sets the verification code apart. Piped output and
    terminals without UTF-8 show no motion.
  - `sesame-link update` shows a progress bar while it downloads the release, then a line for
    each verification the installer passes, and asks to restart the daemon as `(y/N)`.
  - Sign-in and recovery guidance is corrected, web connection and machine enrollment help is
    clearer, and the retired account credential file option is rejected everywhere.

## v0.1.24 — 2026-10-01

- New installations now connect to the production Sesame Link service by default. An installation
  that already holds a login or paired machine keeps the environment it was set up with, so
  upgrading and restarting do not move it. `sesame-link doctor --environment staging` still
  selects staging explicitly.
- `sesame-link` output is calmer and easier to scan:
  - Running `sesame-link` with no command shows a welcome screen with this machine's sign-in,
    daemon, and session state and the commands to run next, instead of a usage error.
  - `status` opens with one line that says whether this machine is connected and names the web
    app to open on any device, then one row each for the daemon, the Gateway, every runtime, and
    the release, and at most one next step. `status --verbose` adds connection, build, and
    installation details, and `status --json` prints the whole report for scripts.
  - Errors lead with what failed and put the command to run on its own line, `doctor` is a
    checklist, and `start` reports how long it took and ends with the web app address.
  - The installer, `auth login`, `machine pair`, `setup`, the welcome screen, and `--help` show
    the web app's address, such as `https://link.sesameai.app`, so you can reach your sessions
    from any device. The welcome screen's docs link follows the machine's environment.
  - Sign-in, `setup`, and a finished `update` open with the Sesame Link logo drawn in braille,
    and `--help` opens with a one-line header. The art appears only on a wide enough interactive
    terminal outside CI; `--quiet` leaves it out.
  - Commands in messages are bold in a color terminal and keep their backticks when piped, and
    marks such as `✓` fall back to ASCII on terminals that are not UTF-8.
- Messages sent from a remote client to a Claude Code session with busy background tasks no
  longer stop for review when Claude records a background notification or a subagent's report
  without its prompt hook. A prompt typed at the terminal still sends queued messages to review.
- A Claude Code subagent stopped from its parent session no longer stays listed as working in the
  web client, and ending or removing a session no longer briefly shows its rail row as working:
  the row reads "Ending…" and stays in its group until the session leaves the rail.
- Codex installed with npm is supported, and Link shows repair guidance when the runtime fails to
  launch.
- Codex sessions stay usable through temporary terminal inspection failures, and Link keeps
  watching for the terminal to exit instead of permanently disabling messages.

## v0.1.23 — 2026-09-30

- Answer more of a coding agent's requests from a remote client instead of at the terminal:
  - Codex permission requests, including a command's additional permissions, and Codex questions.
    Additional permissions have been verified with Codex 0.159.2 only.
  - MCP forms with simple fields from Codex and Claude, and URL requests from Codex.
  - Codex MCP app approvals, such as "Allow Computer Use to use Google Chrome?": allow once,
    decline, or cancel. Allowing for the session or permanently is offered only when Codex offers
    it.

  Whichever answer arrives first wins, from the terminal or a remote client. Saved and session
  app grants, filesystem additions, and unrestricted network access each stay within the machine
  owner's limits. Claude URL requests, forms with nested or referenced fields, and request
  shapes Link does not recognize still have to be answered at the terminal.
- Managed Claude and Codex sessions no longer get the remote-update tool, and agents no longer
  repeat their final answer in a tool call. Remote clients follow progress from the agent's
  ordinary messages and tool activity, and read the final answer from the transcript. Sessions
  that were already running keep the old tool until they restart.

## v0.1.22 — 2026-09-30

- macOS binaries are now signed with Sesame's Developer ID and notarized by Apple, and the
  installer refuses a macOS binary from this release onward that does not carry that signature.
  Earlier releases stay installable as unsigned previews. The Linux binary remains unsigned.
- `sesame-link codex` starts Codex in the current directory as a Link session and attaches to its
  terminal once the session has a first message: the command's prompt, one typed when asked, or
  one sent from a client. `--model`, `--reasoning-effort`, `--sandbox`, and `--ask-for-approval`
  set the session's configuration.
- `sesame-link logs` prints the end of the daemon's log, and `sesame-link logs --follow` keeps
  printing it as the daemon writes, continuing across daemon restarts.
- Getting started is clearer. `sesame-link --help` lists commands in groups — sessions, daemon,
  account and machine, and maintenance — and its getting-started steps begin with
  `sesame-link auth login`. That login makes the browser code stand out in the terminal and, when
  run interactively, offers to start the daemon once you are signed in; otherwise it prints the
  command that starts it.
- Codex sessions:
  - They start again with Codex 0.157 and later.
  - They report each tool by the name Codex gives it, such as `commandExecution` and
    `fileChange`, instead of a name Link chose.

## v0.1.21 — 2026-09-29

- Sign in once with your Sesame account. `sesame-link auth login` keeps a persistent sign-in in
  private local credential files, `auth status` shows it, and owner commands reuse it. `auth logout`
  disconnects the machine while preserving its local sessions. Terminal sign-in and
  `connect web` open Sesame sign-in on the Sesame Link web-app domain, and the `auth` commands'
  output is easier to read.
- `sesame-link uninstall` stops the selected daemon, unpairs the machine, and removes the
  executable you ran, after confirmation or with `--yes`. It keeps your account credentials and
  session history.
- The daemon now launches the `claude` and `codex` your shell runs, found again at every start
  instead of pinned to the executable it first found, so reinstalling, moving, or updating either
  takes effect at the next `sesame-link restart`. A mise shim resolves to your own tool from your
  home directory, or to the next one on PATH when no active mise tool provides it, and a project's
  mise pin no longer leaks into the daemon. `--claude-executable` and `--codex-executable` still
  name one explicitly, and restarts keep it. When Claude Code cannot be found or does not run,
  `sesame-link status` says what to do instead of reporting only that the CLI is unavailable.
  `Latest release` now shows the version the same way `Running build` does, without the `v`.
- Ending a session now preserves its history: a Claude Code session closes its managed terminal,
  and a Codex session stops observed work and archives its thread before releasing its Link
  session slot.
- With `--open-terminal` set to Ghostty or iTerm, a new session's tab now opens in the window that
  already holds your other Sesame Link sessions, rather than whichever window you last used.
- Claude session recovery quarantines a recovery record that contains operations from another
  runtime, and still recovers valid sessions and older records that name no operation runtime.
- Codex sessions are more resilient:
  - A very large Codex tool result, such as a big browser-tool page capture, no longer stops
    the Codex coordinator or leaves the Link session degraded and unable to recover its history.
  - A session no longer stops accepting remote messages when a busy machine briefly delays Link's
    check on its terminal; only failures that persist mark the session degraded.
  - When a session cannot finish starting, a failure to clean up its thread or recovery manifest is
    reported alongside the original startup error instead of replacing it.

## v0.1.20 — 2026-09-27

- Restart the daemon after installing. `sesame-link` commands and the Claude sessions this build
  starts now connect to the daemon more securely, and the `--daemon-address` option is removed.
  Run `sesame-link restart` before using the commands that read the daemon. Claude sessions
  started before the update keep the earlier connection until they are started again, so start
  them again when you can.
- Remote clients are held more closely to the limits you set. The terminal and the loopback API
  are unchanged:
  - A remote message that begins with `/` is refused, as one beginning with `!` already is. A
    remote prompt that begins with a path needs a word in front of it.
  - Remote messages can `@`-mention only files and directories inside the session's working
    directory. Write `@src/main.rs#L10`, not `@src/main.rs:10`, for a line.
  - A remote client can no longer apply a Claude "always allow" suggestion that Claude would save
    to its settings files, such as a rule in a project's local settings or an extra working
    directory, unless the owner sets `allow_claude_persistent_rules` or
    `allow_claude_persistent_directories` to `true` in the daemon's `remote-access.json` and
    restarts Sesame Link. Suggestions that last only for the running session still work remotely,
    and a saved permission mode is always refused remotely.
  - Permission and question dialogs rejected at the terminal are recognized more reliably, and a
    queued message is never typed while a dialog is waiting.
- The daemon refuses to start a session in a directory whose path contains control or invisible
  characters.
- A session keeps records only of its most recently settled messages; the status of a forgotten
  message answers not found. Queued, in-flight, and unresolved messages are always kept.
- Claude sessions:
  - Link now notices when Claude Code on your machine is logged out. `sesame-link status` and remote
    clients say to run `claude /login` there, remote Claude starts are refused until you do, and
    the web client explains a turn that failed because Claude was logged out.
  - Claude Code 2.1.283 is reported as `verified` in `/v1/status` and at daemon startup, instead of
    `assumed`, after the scripted suite passed end to end on it.
  - A message whose turn the model API refused or failed now reports its operation `failed` (reason
    `turn_failed`), and one whose turn was interrupted reports `interrupted`, instead of
    `completed`. When another message was waiting in Claude's queue at a remote interrupt, that
    message now completes with the turn Claude runs from it.
  - Message reads report, on each user message, the Link operation whose prompt started its turn,
    and `unknown` for input that joined a running turn, input typed at the terminal, and every
    message read after the daemon restarts.
  - Remote messages are no longer set aside for review after a background task or subagent reported
    back, or, on Claude Code 2.1.283, after `/fork` is typed in the terminal. Remote messages also
    reach a session whose narrow terminal cuts off the end of Claude's footer, instead of staying
    queued until the window is widened.
  - A session Link launches in tmux starts as a top-level Claude session even when the daemon was
    started from inside Claude Code: Link removes markers such as `CLAUDECODE` and
    `CLAUDE_CODE_CHILD_SESSION` first, so Claude keeps saving the transcript Link reads.
  - Subagents that keep running after their turn ends appear in a session's activity, so remote
    clients show the session as working, and each running subagent carries its launch description,
    how many tools it has used, and when it was last active. A running session starts reporting
    them once it is relaunched.
- Codex sessions:
  - A remote message sent while a Codex turn is running now goes into that turn, as it does for
    Claude. If the turn ends just before the message reaches it, the message starts the next turn;
    it is never lost or sent twice. Codex session snapshots report their capabilities, including
    `midTurnInput`.
  - Codex sessions start with the same remote-client guidance Claude sessions receive, and can
    report progress and a handoff with `report_remote_update`, so a voice client can narrate their
    milestones.
  - A Codex sub-agent's own approval request reaches remote clients as the session's interaction,
    and sub-agents that keep running after their turn ends appear in the session's activity with
    the command each is running.
  - Message reads report, on each user message, the Link operation that delivered it when Codex
    recorded that operation's id, and `unknown` otherwise.
  - On macOS, `--open-terminal` also opens a terminal for each new Codex session once its first
    message is sent, and an attached terminal is titled with the session's Link name followed by
    `· Sesame`, following each rename.
  - Codex sessions no longer inherit the markers of a Claude Code session `sesame-link start` was
    run from, so a `claude` that Codex runs as a tool keeps its own transcript. A Codex runtime
    already running keeps its environment until it restarts.
  - Deleting a Codex session, or a Codex create that fails or is refused, removes the session's
    state directory even when an interrupted write left part of its records behind.
  - `sesame-link status` says when the Codex coordinator is still running an earlier Link build. The
    coordinator and its app-server keep running across daemon restarts and updates, so changes to
    them apply only after you restart both, as the Codex runtime document describes.
- Remote clients see the machine more accurately:
  - A session that ends on the machine — deleted or stopped with `sesame-link`, or whose Claude Code
    or Codex process exits — ends in remote clients as soon as the daemon observes it, and a
    stopping Sesame Link tells the service first, so clients show it was stopped rather than that
    its connection was lost.
  - The daemon tells the Link Session Service when a turn starts or ends, so clients can order
    sessions by when they were last worked in. The report names the session and nothing else.
  - A remote start for a session this machine is already running is refused instead of launching a
    second Claude Code or Codex process, as a retried start used to.
  - A remote command whose answer is too large for the Gateway connection is reported as having an
    unknown outcome instead of as refused; a start answered this way is no longer returned to
    pending while its session runs.
  - When two local sessions claim the same service session, the inventory names only the one with
    the lower Link session ID and reports the other as `serviceBindingRefusal` `duplicate_session`,
    instead of one contested session taking the whole machine offline remotely. When the service
    refuses one session's binding, the daemon logs it and reports it as `serviceBindingRefusal` in
    that session's status until accepted; the other sessions carry on, and a
    `recovery_state_conflict` close no longer stops redialling.
  - The daemon reaches the Gateway over IPv4 when the machine has an IPv6 address but no working
    IPv6 route, instead of timing out on every attempt.
  - Session lists no longer run Git for every quiet session on every read: a session that has
    published nothing since its last Git observation reuses it for up to a minute.
- A terminal tab or window that `--open-terminal` opened now closes when its session is stopped,
  forgotten, or deleted. A detach keeps it open until Return is pressed; Apple Terminal and iTerm
  close it according to their profile's exit setting.

## v0.1.19 — 2026-09-25

- The daemon now sends crash and error reports to the Sesame Link maintainers, so failures on your
  machine are visible without sending a diagnostics bundle. A report names where in Link the
  failure happened, the release, and Link's own identifiers for the command or session involved;
  it never includes log messages, file paths, your hostname, or anything from a session. Run
  `sesame-link error-reporting off` to stop sending them, and restart the daemon to apply it now.
- The daemon and `sesame-link` commands check that their local credentials are private to you, and
  say how to fix them when they are not.
- Remote messages in Claude sessions:
  - Claude sessions that Sesame Link starts no longer show Claude's "Teach auto mode about your
    environment?" offer or its "How is Claude doing this session?" survey, either of which could
    hold a remote message at "waiting for the terminal" until someone answered it at the machine.
    In these sessions `/auto-mode-setup` is unavailable; run it in a Claude session started outside
    Sesame Link. Your other `skillOverrides` and `env` settings still apply.
  - A remote message that waits more than a few seconds behind something in Claude's terminal that
    Sesame Link cannot read — a new Claude dialog, a draft, an unfamiliar layout — is now reported
    instead of sitting silently queued. The web app shows a read-only preview of the terminal and
    the command to attach to it; the message still waits until the terminal is clear.
  - If you interrupt Claude before it starts replying to a remote message, Claude puts that message
    back in the terminal's input box. Sesame Link now clears it from there, marks the message
    interrupted instead of sent, and returns the session to idle, so the remote client is no longer
    left waiting on a message the session never ran.
  - Claude is now told when a prompt is a message you sent from one of your remote clients, rather
    than text pasted from somewhere else. When the prompt relays a voice call, Claude is also asked
    to answer in a form that reads well aloud: the answer first, at most three numbered questions,
    a progress report before any long step, and a check before multi-step changes while you are
    still discussing a design.
- New sessions are easier to start where you have worked before. The daemon remembers the
  directories its sessions launched in, the web client's creation form prefills the most recent and
  offers the others as one-click choices ahead of typed completions, and the voice agent can start
  a session in one of them instead of only the default workspace.
- With `--open-terminal`, Ghostty now opens each new session in a new tab of its front window
  instead of a new window, and opens a window only while it has none. `--open-terminal iterm`
  opens sessions in iTerm the same way. Apple Terminal still opens a new window for each session.
- The daemon reconnects within about a second of the Link service coming back from a planned
  restart, instead of waiting out a growing backoff. When the service refuses the daemon as
  speaking a protocol it no longer supports (`protocol_unsupported`), `sesame-link status` now
  says to run `sesame-link update` and restart the daemon.
- Command-line checks and messages:
  - `sesame-link machine approve`, `machine deny`, `machine revoke`, and the `connect` subcommands
    that take an identifier refuse a malformed one before contacting the service. Every
    Management refusal, including one to
    `machine rotate-credential` or to the notice `unpair` sends, names the service's error code
    beside its HTTP status, and an `unpair` notice whose answer was lost is reported as possibly
    recorded rather than as never sent.
  - Every command that needs a running daemon, the `connect` subcommands included, says
    `Sesame Link is not running, so ...; start it with ...`.
  - The installer, `sesame-link status`, and `sesame-link update` accept only release tags in the
    published form, such as `v0.1.19`; any other `SESAME_LINK_VERSION` is refused before anything
    is downloaded.
  - `sesame-link start` and `sesame-link serve` given a `--listen` address other than `127.0.0.1`
    refuse with "--listen must be a 127.0.0.1 address: the Link API binds only to loopback". A hook
    event Link does not handle is refused as `unsupported hook event: <name>`, and the daemon log
    records each launch as `started Claude session`, still carrying `claude_session_id`. None of
    these messages names an internal development stage any more.
- Local API changes:
  - `GET /v1/status` and the message pages of `GET /v1/sessions/{id}/messages` no longer carry a
    `stage` field, and the published result schemas in `protocol/schemas/v1` no longer list it.
    Read `apiVersion` and `daemon.version` for what a daemon speaks and which release it is.
  - Every `session.state_changed` event carries both the session's `lifecycle` and the `reason` it
    changed. A newly launched Claude session's `starting` event names its reason (`pane_launched`,
    or `native_launch_started` for a Ghostty launch), and a Codex session that reconnects to its
    app-server reports the lifecycle it resumes in.
  - A remote session start whose payload is not a valid session creation request is refused as a
    bad request naming what is wrong, instead of reaching the client as Link's internal error. An
    explicit `null` in an optional configuration field of a remote start or message is read as the
    field omitted, as the published request contract states: a start takes the machine's remote
    default permission mode, and a model-only restart keeps the session's admitted mode.
- Reliability and diagnostics:
  - Deleting a Claude session that is still starting waits for its tmux session to exist and then
    stops it, instead of reporting the deletion before the session had launched. If tmux refuses to
    stop it, the session stays listed so the delete can be retried rather than left running
    unowned.
  - The Codex coordinator keeps its own owner-only log, `codex-runtime/coordinator.log` in the
    state directory, recording why it refused a Codex request, closed a native or Link connection,
    could not settle a held write, or stopped. Clients still receive the same refusal codes.
    `sesame-link diagnostics` includes this log.

## v0.1.18 — 2026-09-24

- The daemon no longer connects on the retired preview Gateway profile. A remote access
  configuration that names any profile other than `production_management_v1` stops the daemon
  from starting, and its error names that profile and the fix: move the file aside, run
  `sesame-link setup` to enroll the machine, then `sesame-link start`.
- Claude Code's `/open-sesame` command, installed by `sesame-link install-claude-command` or
  offered by `sesame-link setup`, moves the conversation of a Claude Code session Link does not
  manage into a Link session on the tmux backend, continuing that Claude's model, effort, and
  permission mode.
- `sesame-link diagnostics` writes one archive of daemon logs, status, and session state, with
  credentials removed, to share when asking for help.
- A remote message sent while Claude is working is delivered into Claude's own queue within
  moments, as the same text typed at the terminal would be, instead of being held until the turn
  ends. Claude folds it into the running turn or runs it next; a draft in the prompt box still
  holds it. Requires Claude Code 2.1.247 or later.
- After a Link restart, a Claude session whose transcript shows the turn still open stays running,
  instead of an empty composer being treated as idle and queued messages being sent into the
  running turn.
- A terminal attached to a Link session is now titled with the session's name, or the title Claude
  or Codex sets until it has one, followed by `· Sesame`.
- Native Ghostty input is now withheld on Claude releases before 2.1.119, whose editing mode the
  session settings cannot select, as well as on 2.1.245. The daemon's startup log reports that
  decision with the other Claude capability decisions and why, and refusing a Ghostty session on a
  withheld Claude release names the reason.
- Reliability:
  - The paired machine's credential and identity, the connector configuration, the Management
    environment, and the Codex runtime marker are saved by atomic replacement, so a crash or full
    disk while one is written leaves the previous file instead of a truncated one that stops the
    daemon from starting or Codex from reconnecting.
  - When the daemon runs without a UTF-8 locale, such as under launchd, session paths and launch
    arguments outside ASCII are read back from tmux exactly, so a restart re-adopts those Claude
    sessions with the model and working directory they were launched with instead of quarantining
    them or reporting `_` in place of each such character.

## v0.1.17 — 2026-09-23

- `sesame-link claude [claude args] [-- prompt]` starts Claude Code in the current directory as a
  Sesame Link session with those arguments and attaches to it. Link keeps the arguments when a
  remote message switches model, effort, or permission mode, and sends text after `--` once as the
  first message.
- On macOS, every new tmux-backed Claude session now opens in its own attached terminal window,
  whether it was started from the web client, the voice agent, or `sesame-link sessions create`;
  `sesame-link claude` still attaches the terminal it runs in. Choose the app with
  `sesame-link start --open-terminal auto|ghostty|terminal|off`: `auto`, the default, uses Ghostty
  while it is running with scripting enabled and Apple Terminal otherwise, `ghostty` opens nothing
  while Ghostty is not running, and `off` restores the earlier behavior. The first window in each
  app may ask for macOS Automation permission, and while a window is attached a workspace trust
  prompt has to be answered in that window.
- Sessions started on your machine with `sesame-link claude` or `sesame-link sessions create` now
  appear in the web client and the Sesame app, as remotely started sessions do. This takes effect
  once the Link service supports it, and applies only while remote access is on.
- An opt-in macOS Ghostty backend for daemon-created Claude sessions, selected with
  `--claude-terminal ghostty` and kept across daemon restarts; tmux remains the default. Ghostty
  sessions confirm delivery through Claude's hooks and transcript, are recovered with their
  original process after a daemon restart, are stopped under local supervision without signaling
  recovered process IDs, and are treated as ended when their terminal is confirmed closed, with
  ambiguous observations handled conservatively. Remote messages no longer leave a stray `.` in
  Claude's input box. Native Ghostty input is refused on the known-incompatible Claude Code
  2.1.245 and on unparseable Claude versions, while recovered sessions keep their stop authority;
  tmux sessions are unaffected. Existing session records are migrated on upgrade to record their
  terminal ownership and backend explicitly, preserving their identities and recovery evidence.
- Remote message delivery to Claude is more reliable:
  - Enter is pressed only after Claude has drawn the message in its composer, so a busy or freshly
    restarted Claude no longer leaves it unsent.
  - A second Enter is not sent when Claude's transcript shows new native queue activity or
    messages, or when that evidence cannot be checked.
  - A wedged tmux server can no longer hold delivery indefinitely: every paste and Enter gives up
    after a bounded wait, reports the message's delivery as unknown rather than sent or unsent,
    and hands the pane back to its user.
  - Completed Claude turns are recognized even when transcript records arrive out of order, so a
    delayed user record no longer blocks remote input after a daemon restart.
  - Terminal input and configuration restarts are bound to one session target.
- Stop, delete, and cleanup act on the exact managed tmux session and never fall back to a
  similarly named session after the original exits.
- Codex sessions now carry Sesame Link's `set_session_name` tool, so a Codex agent names its
  session with a short voice-friendly title the way a Claude session does; a name a person chose
  still stands, and no experimental Codex capability is needed. While a Codex session is open in a
  native terminal, the daemon keeps reporting a complete session inventory, so the Link service
  can again retire sessions that ended during that time.
- Commands no longer open with a title banner and blank margins, `sesame-link sessions` lists
  sessions as a table fitted to the terminal width, `sesame-link status` places its sections side
  by side on wide terminals, and confirmation prompts stand on their own line with the accepted
  answers highlighted.
- Every command Sesame Link suggests keeps an explicit `--state-directory`, so following a
  suggestion from `status`, `unpair`, `sessions`, `connect`, `machine pair`,
  `machine rotate-credential`, or `update` acts on the installation that was reported on rather
  than the default one. `setup` accepts `--account-credential-file` and carries it into the
  account commands it suggests, and `account import` accepts `--state-directory`.
- The daemon always records its own lifecycle and remote command records, including remote
  `session.stop`, `session.forget`, `session.archive`, and `session.delete_history`, even under a
  quieter inherited `RUST_LOG` such as `warn`; `RUST_LOG` can still add detail. A remote answer
  larger than 1 MiB is no longer reported as an internal error: it is relayed when it fits the
  connection's frame limit and otherwise refused with Link's `payload_too_large` code, and an
  answer the daemon cannot read carries Link's own `internal_error` code.

## v0.1.16 — 2026-09-22

- The daemon no longer opens a terminal on its own machine for a remote client: the
  `--terminal-launcher` flag and the bundled macOS Terminal launcher are gone, and the installer
  removes the launcher an earlier release installed. Attach from the machine instead — the web
  client's Terminal button copies the `sesame-link sessions attach <id>` command to run there.
- `sesame-link sessions attach` now works for Codex sessions as well as Claude sessions. The
  daemon prepares the session's native Codex terminal on the coordinated app-server socket and
  the command attaches to it; an empty Codex thread is still refused until its first message.
  That terminal also stays connected now: the coordinator relays the provider's multi-megabyte
  plugin and app catalogue replies instead of dropping the connection on each of them, which made
  the terminal reconnect and redraw every second.
- Remote Codex sessions use the `workspace_write` sandbox and `on_request` approvals, and the
  machine owner's Codex approval settings are reported to remote clients so they can explain
  unavailable saved approvals before you submit.
- Remote Claude sessions launch with an explicit allowed permission mode and stop when Claude
  reports an unknown or disallowed mode, including after daemon recovery.
- Messages you send from a remote client arrive more reliably. A message queued behind an earlier
  one stays queued and sends in order once the earlier one is accepted, instead of being set aside
  as `needs_review` with an Edit prompt; only a prompt typed at the terminal, or one Link did not
  observe, still sends queued messages to review. A message Claude drew in its composer but never
  submitted — seen right after a permission-mode or restart respawn under load — is submitted by
  pressing Enter once more rather than reported as an unknown outcome after ten seconds, and only
  while the pane still shows exactly the pasted text, unchanged since the paste.
- `sesame-link machine revoke` works again: it accepts the machine record the service reports
  today, instead of refusing every review with "the Management API response for machine review was
  invalid".
- `sesame-link unpair` now tells the service the machine left its account, so the web client and
  other linked clients stop listing it. Unpair still completes locally when the service cannot be
  reached, and then names the `sesame-link machine revoke` command the account owner can run.
- Installer output is easier to scan, with consistent progress steps, readable platform names, and
  concise installation and daemon restart guidance.

## v0.1.15 — 2026-09-21

- Remote clients can show the text of a message another client queued, such as one the voice
  agent sent, instead of a placeholder. Session snapshots and the queue-admission event carry the
  first 2 KiB of the queued text, marked when cut. The preview comes from the daemon's memory and
  is never written to the recovery manifest.
- Session configuration options now name a default on the legacy top-level Claude model, effort,
  and permission-mode lists, matching the runtime-specific lists, so a remote client sees the same
  permission-mode default in both places.

## v0.1.14 — 2026-09-20

- Follow-up messages reach Claude more reliably. A queued message is no longer left waiting when a
  background notification or system continuation starts a new prompt, and prompt tracking now
  survives a daemon restart, so a delayed transcript write does not hold back later messages.
- Claude sessions keep their conversation history when entering or leaving a worktree, and their
  worktree context survives a configuration restart. History cursors issued before this upgrade
  refresh once.
- Claude Code's Link instructions are shorter, and private artifacts are now reserved for
  substantial standalone reports, designs, and detailed plans rather than routine responses.
- Codex sessions survive interruption. Link reconnects automatically after a dropped connection or
  an unavailable startup, confirms it owns the socket it reattached to, and reconciles each thread
  before accepting new work, so a submission whose outcome was uncertain is never replayed. A
  native Codex terminal and remote messages can now drive the same task, with separate next-turn
  and explicit steering actions and durable recovery of uncertain writes.
- Codex conversation history loads and recovers through bounded provider pages, so one oversized
  history read no longer disconnects other sessions. This release requires Codex CLI 0.154.0 or
  newer.
- Remote Codex work always uses the sandbox and approval settings chosen for remote sessions.
- Sessions can be stopped and removed, and Codex sessions can also be archived or have their
  provider history permanently deleted; both of those refuse a Claude session. Removal follows the
  runtime: a Link-owned Claude session is terminated, while a Codex session is detached rather than
  killed. These controls report the provider history result they actually observed.
- Link now admits up to eight sessions across Claude Code and Codex by default, and refuses to
  start a new one while the machine reports less than one gibibyte of memory available. That
  memory guard is best-effort protection against a launch pushing the host into swap;
  `serve --min-available-memory-mib` adjusts or withdraws it.

## v0.1.13 — 2026-09-18

- Remote message tracking is more reliable. Confirmed delivery survives background updates and
  daemon restarts; a later turn interrupting completion tracking no longer turns an observed
  submission into unknown delivery; and completion uncertainty no longer blocks later messages.
  Claude's pasted-content envelope is recognized when confirming an instruction, avoiding false
  delivery warnings for voice and other remotely pasted messages. Claude turn identities are now
  exposed so clients can group messages correctly.
- Claude Code sessions are guided toward concise, self-contained answers for voice, while explicit
  requests for detail still take precedence and lengthy supporting material can live in a
  referenced document. Their existing remote handoff can include structured outcomes, key facts,
  and next steps for clearer result cards without another model call. Coding-agent session names
  are now limited to two to four plain words and 40 characters so they remain voice-friendly.
- Monitor notifications preserve their summaries and event details even when Claude reports no
  originating tool or completion status.
- The daemon log now names the identifiers needed to trace a remote request to the work it caused:
  a completed remote command records both the local Link session and the durable service session a
  remote client addressed, and a started Claude session records the Claude session id that names its
  transcript, which remains available after the session itself is deleted.
- `sesame-link doctor --environment production` selects the production Link service for a state
  directory, alongside `staging` (still the default) and `local`. Each environment is pinned to the
  immutable identity its deployment publishes, so a state directory paired against one of them
  refuses the other; a paired state directory is never retargeted.

## v0.1.12 — 2026-09-17

- Remote messages reach their session, and resolve correctly, in three cases that previously went
  wrong. A message now reaches a session whose Claude prompt carries an IDE context chip — the
  editor file or selection Claude shows in the composer — instead of waiting for a terminal draft
  that is not there. Claude removing leading or trailing whitespace from a message, including the
  trailing space a phone keyboard adds, no longer produces a false “delivery unconfirmed” hold;
  whitespace-only messages are now rejected before submission instead. A later terminal turn no
  longer falsely completes an earlier remote message whose turn completion was never observed, and
  that earlier operation stays unresolved rather than being reported as delivered.
- Model, effort, and permission-mode changes now continue across an in-place Claude Code update,
  when the installed executable preserves the session's existing integration capabilities.

## v0.1.11 — 2026-09-17

- New `sesame-link update` installs the latest published release over the running binary and
  offers to restart the daemon, so activating an update no longer needs the install one-liner
  followed by a separate restart. It names the release, its notes, and the installer it will run
  before asking; `--yes` skips the question and `--restart` restarts without asking. The installer
  is the published one pinned to that release, and it installs into the running binary's
  directory. The command refuses a source checkout, and refuses a published version it cannot
  order against the running one. The update notice in `status`, `start`, and `restart` now names
  this command instead of the install one-liner.

## v0.1.10 — 2026-09-16

- New `sesame-link restart` stops the daemon and starts it again with the launch options it
  recorded at startup (listen address, executables, session ceiling, default workspace, and the
  remote access configuration path), so activating an update no longer needs the original command
  line. The record is `daemon-launch.json` in the state directory. A daemon started by an older
  build, including v0.1.9, has no record, so `restart` refuses it and names `sesame-link stop` then
  `sesame-link start` instead of guessing defaults; do that once after upgrading. A record that
  does not describe the running daemon, or names a workspace or remote access configuration that
  is gone, is refused before the daemon is stopped.
- `sesame-link status` now reports when the running daemon is not the build of the binary that ran
  the command, saying whether that binary is newer or older than the daemon or a different build
  of the same version, and names `sesame-link restart`. The installer's closing message points to
  this check after an upgrade.
- `sesame-link status` now reports the latest release on the public channel and, when it is newer
  than the binary running the command, names the install command, the restart that activates it,
  and the release notes. For a release build, `sesame-link start` and `sesame-link restart` add
  the same notice when a check made within the last day knows of a newer release. The check is an
  anonymous, bounded read of the channel's `latest` pointer that never fails the command; its
  record lives in `release-check.json` under the data directory, and `SESAME_LINK_NO_UPDATE_CHECK`
  turns it off.
- The daemon removes a Claude session's generated state directory when the session is refused by
  the session ceiling, fails to launch, or is deleted, instead of leaving orphaned directories
  behind. A create refused by the ceiling no longer writes anything, and daemon startup removes
  leftover empty or Link-generated session directories without probing each one through tmux, so
  recovery no longer slows down as leftovers accumulate.
- Command output is easier to scan: grouped status details, aligned fields, readable uptime,
  compact session records, clearer next steps, and terminal-aware color that respects
  `NO_COLOR`. Connection commands' JSON output is unchanged.

## v0.1.9 — 2026-09-16

- The `sesame-link sessions` command now has explicit `attach`, `create`, and `delete` subcommands
  for managing local daemon sessions. The old positional `sesame-link sessions <session>` attach
  form is removed, so update any scripts or aliases that depend on it.
- Queued messages now reach Claude when its prompt looks empty but carries an IDE file or selection
  context chip. That context is preserved, and unsent terminal drafts remain protected.
- Claude sessions are now asked to include exact PR and deliverable URLs in remote handoffs, and to
  publish substantial design docs and research as private Claude artifacts where those are
  available.

## v0.1.8 — 2026-09-15

- Message delivery and retries are now durable across daemon restarts. Link uses exact Claude
  transcripts or Codex history to resolve uncertain sends, never automatically resends uncertain
  text, and lets clients acknowledge an unresolved message before continuing once the session is
  observably safe. Keyed mutations replay their stored result after restart; the web client
  automatically looks up lost responses, while the Sesame remote exposes the key for deliberate
  recovery.
- Claude sessions now stay connected across native `/clear` and `/resume`, preserve their selected
  executable and observed capabilities across Link restarts, and refuse configuration replacement
  when that runtime identity cannot be verified. A corrupt session record no longer prevents healthy
  sessions from recovering, and session files and working directories are handled more safely.
- Remote interactions are safer and more predictable. Plan approval checks that the plan still
  matches the one you were shown, workspace-trust dialogs still require explicit consent, and the
  web client disables repeated approval clicks while a decision is pending. Remote messages that
  begin with `!` are refused.
- Linking and pairing are more secure, and machine approval shows the requesting machine and how
  old the request is. Pairing a new machine requires the approving machine to be upgraded too;
  older approvers refuse the new request.
- Unpairing now requires Link to be stopped and preserves native sessions and history. After
  pairing again to the same account, run
  `sesame-link machine associate-session <link-session-id>` while Link is stopped to restore remote
  access; existing sessions from older Link versions also require this explicit association before
  remote access resumes. A different account keeps those sessions local-only.
- Remote connections hold up better under heavy load. Closing a congested machine connection may
  briefly interrupt its other remote clients.
- The web client now recovers live updates after subscription failures, reconnects, and foreground
  changes without resending work or losing drafts. Background timers have explicit lifecycles,
  activity groups update in place, and remote position signals no longer steal focus or open
  details. On machines without a terminal launcher, Terminal copies the
  new `sesame-link sessions <id>` command; it needs v0.1.8 on `PATH` and a daemon using its default
  state directory, so upgrade an older daemon before using it.
- The new `sesame-link sessions` command lists local sessions and can attach a terminal to a
  selected Claude Code session. Daemon logs, `sesame-link status`, and private installer receipts
  now include build, run, uptime, timestamp, version, and checksum details for restart and upgrade
  diagnosis.
- The Sesame voice remote now binds a message to the conversation revision it was briefed on,
  pausing for fresh review if native or remote activity changes that context. Disconnecting the app
  revokes its upstream installation and access when possible and reports explicit cleanup guidance
  when revocation cannot be confirmed.

## v0.1.7 — 2026-09-02

- The web client no longer leaves a duplicate “Delivery uncertain” message after the exact prompt
  has reached the durable transcript, and reconnects reliably on iOS instead of stranding the send
  receipt.
- New Claude Code and Codex sessions now send an explicit value for every configuration field
  instead of offering an unknowable machine “Default”. The web client remembers the newest choices
  separately for each provider, reads Codex models and reasoning levels from that machine's own
  catalog, and offers Codex's “Ask me” and “Approve for me” reviewers while keeping “Ask me” as the
  explicit default.
- The web client now answers Codex command and file approvals using Codex's exact indexed choices.
  It no longer shows Claude-style deny or input-amendment controls that Codex cannot accept, and an
  unfamiliar Codex choice stays visible without becoming an approval control.
- The web client now shows a Codex session's running configuration: the composer states the model,
  reasoning effort, sandbox policy, and approval policy as read-only text, and the session details
  dialog fills those rows and the configuration status instead of reporting them unavailable.
- The web client home view now keeps the session workspace empty, puts creation guidance in the
  session list only when that list is empty, and links the Sesame Link header back home. Mobile
  status controls share one aligned header background, sorting uses a quieter compact treatment,
  and manual ordering reveals larger movement controls only while Reorder is enabled.
- The mobile web composer now stays inside iPhone safe areas, grows with multiline drafts up to a
  bounded height, and treats Enter as a newline while the on-screen Send button submits.
- The web client renders CLI bash-mode exchanges as a shell exchange instead of raw
  `<bash-input>`/`<bash-stdout>` wrapper text: the `!` command reads as the person's terminal
  input, and the captured stdout and stderr appear as literal, separately tinted output blocks.
- The web client now keeps each session header to two compact lines, uses provider icons in the
  header and session sidebar, and moves Terminal into the session menu. Active turns put Stop in
  the composer action slot; typing a follow-up restores Send so queued messages remain available.
- Remote Claude messages ending in a newline no longer become “Delivery uncertain” when Claude
  reports the submitted prompt without that final newline. Link still refuses broader text
  mismatches, and transcript recovery now checks only the first user message after the submission
  began instead of accepting a later coincidental match.
- The Sesame voice remote can now create, list, monitor, summarize, and control both Claude Code
  and Codex sessions with provider-qualified cards and spoken updates. Codex creation uses its own
  model, reasoning, sandbox, and approval fields; approvals preserve Codex's exact indexed choices;
  interruptions, failures, unknown delivery, and requested-versus-observed outcomes remain distinct.
- The daemon, CLI, installer, and web client now treat Claude Code and Codex as independently
  available runtimes. Session creation offers a runtime selector when both are ready, shows each
  runtime's own configuration fields, and limits choices to the machine owner's remote policy.
  `sesame-link status` reports provider-qualified readiness and Codex login guidance; installation
  now works with either an installed Claude Code plus tmux setup or Codex alone.
- Codex sessions can now surface command and file-change approval prompts to remote clients and can
  be interrupted remotely. Approval responses are selected from Codex's exact choices, dangerous
  persistent, filesystem, and network grants remain disabled unless the machine owner opts in, and
  Link waits for Codex's item/turn events before reporting an observed result. Experimental
  permission, user-input, and MCP response paths remain visible but unavailable until their live
  compatibility checks qualify them.
- Materialized Codex sessions now survive a Link daemon restart and can open the native Codex TUI
  on their exact thread. Link suspends remote message admission while that TUI owns the thread,
  reconciles terminal-originated work before reopening it, and refuses stale or mismatched runtime
  containers. Shared-home Codex entries are titled `Sesame Link · …` and follow Link renames so
  they are not mistaken for Codex-owned tasks. Ending the Link session now deletes its persistent
  Codex thread; if app-server cannot confirm that deletion, the Link session remains available for
  retry instead of leaving a phantom history entry.

## v0.1.6 — 2026-08-30

- Claude's workspace-trust dialog is recognized more reliably, and a remote Allow answers only the
  dialog it was shown for.
- Interrupt recovery no longer closes a turn that is still running. Where recovery is withheld,
  the session stays degraded and the terminal can still resolve it.
- Codex app-server threads are projected into Link as sessions, with runtime-tagged identity,
  bounded history and live updates, and Codex capabilities reported through the daemon's status
  endpoint. Where the Codex runtime is available you can create a Codex session and send it
  messages from a remote client. Interrupting one, attaching a native terminal to it, and changing
  its configuration after creation are refused rather than approximated, and it raises no approval
  prompt for a remote client to answer — choose its approval and sandbox policy when you create it.
- Releases now publish the notes written for their version instead of one fixed sentence, both on
  the GitHub release and as `RELEASE_NOTES.md` beside the archives in the public channel.
- Session hooks authenticate to Link more securely, and a session keeps serving the transcript it
  started with.
- A new session no longer warns that its state could not be read while it waits for the first
  message. Link names the no-transcript-yet refusal with its own error code
  (`409 transcript_not_created`), and the web client reads that state as the observed empty
  conversation it is; every other refusal still surfaces as a warning.
- Upgrading leaves sessions that were already running unmanaged: they keep running in tmux, but the
  daemon reports rather than adopts them. Finish or end a session before upgrading to keep managing
  it.
