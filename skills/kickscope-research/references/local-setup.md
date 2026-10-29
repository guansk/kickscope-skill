# Local Agent setup

Use the Skill directory containing this file and `../scripts/install.cjs`; no source checkout or npm package is required. The installer is self-contained. Its generated files are validated against the source build before release.

## Check the environment

Confirm macOS, Google Chrome with KickScope 0.17.0+ installed and enabled, Node.js 18+, local file/command access, and the current Agent client's support for MCP stdio. Installation is local to the Mac running Chrome. Cloud-only agents cannot reach this local socket; do not attempt cloud installation or create a remote tunnel as part of this flow.

Get the actual extension ID from the user's **Connect Agent** page or Chrome extension details. It is 32 lowercase letters in a–p. Different unpacked/Store builds can have different IDs. Ask for a missing ID rather than guessing or substituting another build. If the extension is absent, ask the user to install it from their verified source. This Skill cannot infer the official Store listing from an ID.

If setup came from the extension, the copied request includes the ID and a versioned public GitHub Skill URL. Download that repository at the stated tag into a local working directory and read this Skill. Verify the installer SHA-256 using the repository's `release.json` before running it. Use the exact repository and version provided; do not guess another release URL. Installing the Skill into a client's supported Skill directory is optional when that client only supports MCP and can read the instructions directly.

## Install the companion

Run with actual absolute paths and the supplied ID:

```sh
node "/absolute/path/to/kickscope-research/scripts/install.cjs" ACTUAL_EXTENSION_ID --client generic
```

This writes:

- `~/.local/share/kickscope/`: `mcp.cjs`, `protocol.cjs`, `host.cjs`, diagnostic `call.cjs`, a launcher, and `mcp-config.json`. The directory is private to the current user.
- `~/Library/Application Support/Google/Chrome/NativeMessagingHosts/com.bifrost.kickscope.json`: Chrome host registration restricted to the specified extension ID.

Generic mode does not need Codex or write any Agent client's configuration. It emits the actual absolute Node executable and MCP script path in `mcp-config.json`. No sudo or cloud AI API key is needed. Node installation/path changes or companion updates require rerunning the installer. If Node is missing, explain the requirement rather than claiming installation succeeded.

Exit codes: `0` means the installer's Chrome connection check passed; `1` means environment/install/explicit configuration failed; `2` means the connection check failed even if local files were successfully installed. A generic-mode success does not prove client registration.

## Configure the current client

Read the generated `mcp-config.json`, then register its `kickscope` stdio entry using the actual client's supported mechanism. Back up any configuration file before editing it, preserving other MCP servers and permissions. Do not overwrite an entire configuration with this single entry. Resolve home paths; do not store literal `~` or `$HOME` where a client expects an executable path.

- **Claude Code:** use its own MCP registration mechanism; consult `claude mcp add --help` as needed. Example shape: `claude mcp add --transport stdio --scope user kickscope -- ABSOLUTE_NODE ABSOLUTE_MCP`. Existing entries should be inspected before updating. Do not write Codex settings for Claude Code.
- **Codex:** the explicit `--client codex` installer option can register, back up and verify its entry with the Codex CLI, including `CODEX_HOME`. Select it only when configuring Codex.
- **Other clients:** use their actual documented MCP stdio configuration. If unsupported, report the missing capability instead of installing an unrelated client.

For a client that supports installing Skills, put the Skill in its supported location without overwriting an unrelated existing Skill. For a client that only supports MCP, reading this Skill as setup instructions is sufficient; do not assume a universal Skill directory.

Official references: [Claude Code MCP](https://code.claude.com/docs/en/mcp), [Codex MCP](https://developers.openai.com/codex/mcp/). Client setup may require reloading tools or a new session.

## Verify and troubleshoot

Have the user keep Chrome running, enable KickScope and its **Connect Agent** switch, and click **Reconnect** when needed. Recheck with:

```sh
node "/absolute/path/to/kickscope-research/scripts/install.cjs" --check
```

`--print-config` emits the absolute MCP JSON without installing or checking. For protocol diagnostics, locate `call.cjs` in the installed companion directory using the current OS home directory, then run:

```sh
node "/absolute/home/.local/share/kickscope/call.cjs" kickscope_status
```

This verifies the companion MCP protocol, not that the current Agent client has loaded its tools. Complete setup by actually calling `kickscope_status` from the client's MCP interface. Report companion installation, client registration and Chrome connection separately. Confirm the end-to-end client connection only when its actual call returns `connected: true`.

A connected bridge does not prove Chrome AI image input is supported. Initial image permission/model download may need user action. Do not start OCR merely to install; once the user supplies a research project, follow the workflow in `SKILL.md`. If a step remains blocked, give the exact failed stage and the smallest user action required.
