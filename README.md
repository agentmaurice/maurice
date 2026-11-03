# Maurice

Maurice is one `maurice` executable: the AgentMaurice command-line tools and the free local runtime, with no account and no Docker. It runs on your computer, for personal and professional work.

This repository publishes the installers and the release archives of Maurice. There is a single release per version, with the same bytes for every way of installing it.

## Install with your coding agent

Paste this into a coding agent that has a terminal on your machine (Claude Code, Codex, Cursor, Grok Bot, Hark or any MCP client):

```text
Install Maurice by following https://agentmaurice.ai/en/install.md, then verify that you can use it.
```

French: `Installe Maurice en suivant https://agentmaurice.ai/fr/installer.md, puis vérifie que tu peux l'utiliser.`

## Install from a terminal

Download the installer into a temporary directory, read it, then run it:

```sh
install_tmp="$(mktemp -d)"
curl -fsSL https://agentmaurice.ai/install.sh -o "$install_tmp/install.sh"
sh "$install_tmp/install.sh" --install-dir "$HOME/.local/bin" --no-autostart --data-dir "$HOME/.maurice/one"
```

The `install.sh` in this repository is the readable source of that installer. It reads the stable release from `https://get.agentmaurice.app` (never the GitHub API), downloads the archive and its SHA-256 from that site, verifies the checksum and the archive contents, and on macOS the Developer ID signature. It installs `maurice` and its bundled Viewer without sudo, starts Maurice, and prints a one-use pairing prompt for the coding agent on the same machine.

Then check:

```sh
"$HOME/.local/bin/maurice" version --json
"$HOME/.local/bin/maurice" doctor --data-dir "$HOME/.maurice/one" --json
```

`doctor` reports `running`, `health_ok` and `storage_ok`.

Platforms: macOS on Apple Silicon (Developer ID signed), Linux amd64 and arm64 (SHA-256 checksums) and Windows amd64. On Windows, run `install.ps1` in PowerShell without administrator rights: `.\install.ps1 -AddToPath` installs the current stable version, and `-Version <x.y.z>` pins one.

## Command-line only

`scripts/install_mauricecli.sh` and `scripts/install_mauricecli.ps1` are the readable sources of the command-line installers published with each release. They install the same `maurice` executable:

```sh
curl -fsSL https://github.com/agentmaurice/maurice/releases/latest/download/install_mauricecli.sh | bash
```

```powershell
iwr https://github.com/agentmaurice/maurice/releases/latest/download/install_mauricecli.ps1 -UseBasicParsing | iex
```

Maurice and MauriceCLI are the same binary at the same version. Releases up to v2.2.12 were published in the former repositories `agentmaurice/one` and `agentmaurice/mauricecli`, which stay readable; their links keep working. Every later release is published here.

## Release files

Each release lists, for every platform, the archive, its SHA-256 and, for the command-line archives, a minisign signature. `sha256sums.txt` and `mauricecli-latest.json` describe the command-line archives.

## Update

Run the installer again. It keeps the same or a newer version, and replaces an older one in place: it stops the old process and starts Maurice again on the same data directory `~/.maurice/one`, without deleting it and without a new pairing prompt.

`maurice update check` and `maurice update install` update this same binary and its Viewer. Maurice never installs an update by itself: the background check only reports a newer version, which `maurice status` shows; install it with `maurice update install --yes`.

## Installation count

The installer counts an installation only with `--report-installation`, or when you answer yes to its prompt. It then sends a random installation ID, the version, the operating system and the CPU architecture to get.agentmaurice.app and PostHog EU. Nothing else is sent.

## Report an issue

Open an [issue](https://github.com/agentmaurice/maurice/issues/new) with the output of `maurice version --json` and `maurice doctor --data-dir "$HOME/.maurice/one" --json`. Issues are public: do not attach raw logs, tokens, pairing links or personal data.

Maurice is proprietary software by AgentMaurice. Releases are listed under [Releases](https://github.com/agentmaurice/maurice/releases).
