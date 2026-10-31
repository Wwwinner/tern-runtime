# Tern Runtime

An agent skill for controlling Tern terminal workspaces through the ordinary `tern` CLI.

The skill provides instructions for choosing commands, resolving workspace targets, and reporting observed results. It does not require a plugin or an additional runtime script.

## Features

- Inspect sessions, tabs, panes, layouts, and associated processes.
- Create sessions, tabs, and splits in a chosen working directory.
- Launch terminal programs, including shells, Codex, Claude, scripts, tests, and build commands.
- Focus, move, rename, float, and dock panes.
- Send command lines, text, keys, and multiline pastes.
- Capture terminal text, scrollback, ANSI, HTML, and Surface text.
- Wait for a launched program to exit and retain its output.
- Open files at a line and column, including split, tab, preview, and editor-wait modes.
- Monitor live JSON events, including filtered subscriptions.
- Control pane retention and close selected panes or sessions.

## Requirements

- Tern installed and running, with the `tern` CLI available to the agent. On Windows, the console entry point is typically `tern.com`.
- An agent that can load `SKILL.md` skills and execute local commands.
- Any program you want to launch installed separately and available in the target environment.

The documented workflows were smoke-tested with **Tern 0.9.8 on Windows**.

## Installation

Copy `SKILL.md` and the `agents` directory into a folder named `tern-runtime` in your agent's skills directory. Keep `SKILL.md` at the root of that folder.

### Magpie on Windows

From a checkout of this repository, run PowerShell:

```powershell
$skillDir = Join-Path $HOME ".config/magpie/library/skills/tern-runtime"
New-Item -ItemType Directory -Path (Join-Path $skillDir "agents") -Force | Out-Null
Copy-Item -LiteralPath ".\SKILL.md" -Destination (Join-Path $skillDir "SKILL.md") -Force
Copy-Item -LiteralPath ".\agents\openai.yaml" -Destination (Join-Path $skillDir "agents/openai.yaml") -Force
```

For other agents, use their configured skills directory and the same folder structure. Follow the agent's normal skill discovery or reload procedure after installation.

## Usage

Ask your agent to use the `tern-runtime` skill, then describe the workspace operation you want. For example:

- "Show my Tern sessions, tabs, and panes."
- "Split this terminal to the right and open a shell in the same directory."
- "Open a new tab in this repository and start Codex."
- "Run the build in a new pane, keep its output, and wait for it to exit."
- "Read the output from the test pane."
- "Move this pane into a new tab."
- "Float the log pane over the terminal, then dock it again."
- "Open README.md in a preview block."
- "Watch for layout changes until I stop the subscription."

Target discovery is on demand: use known pane IDs directly, start unknown-pane discovery with compact `tern ls`, and request `inspect --json` only when required details are unavailable from narrower commands. Reuse IDs returned by creation commands. See [SKILL.md](SKILL.md) for the full command reference and execution workflow.

## Repository Layout

```text
README.md             Project overview and installation
SKILL.md              Skill instructions and CLI reference
agents/openai.yaml    Display metadata and default prompt
.gitattributes        Consistent line endings for Markdown and YAML
```

## Verification

The CLI smoke checks covered workspace inspection, program launch, sessions and tabs, splitting, process lookup, terminal input and multiline paste, output capture, pane movement, floating and docking, renaming, pane retention, exit and timeout handling, file opening with editor-wait behavior, live and filtered events, and resource closure.

The checks used an isolated session. Temporary resources were removed and the original workspace focus was restored. YAML metadata and command examples were also validated.
