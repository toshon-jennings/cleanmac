# CleanMac

<p align="center">
  <img src="https://raw.githubusercontent.com/toshon-jennings/cleanmac/main/og-image.png" alt="cleanmac disk space cleaner" width="900">
</p>

A simple, zero-dependency bash script to clean up developer caches, logs, and build artifacts on macOS.

## What it cleans

### Standard mode (default)
- **uv** — Python package manager cache
- **pip** — Python pip cache
- **npm** — Node.js package manager cache
- **bun** — Bun package manager cache
- **Homebrew** — Brew cleanup
- **Xcode** — DerivedData
- **Cargo** — Rust global cache
- **Docker** — Unused images, containers, volumes (only if Docker is running)

### Aggressive mode (`--aggressive`)
- **apps** — Application caches (`~/Library/Caches/*`) and safe cache subdirectories in `~/Library/Application Support/*` (Cache, Code Cache, GPUCache, Crashpad, CachedData, WebStorage, etc.)
- **logs** — Log files in `~/Library/Logs/*` and Crashpad folders
- **orphaned** — Data for uninstalled apps (Docker Desktop leftovers, app data without matching installed app)
- **downloads** — DMG/ZIP/PKG installers older than 30 days in Downloads
- **code-signing** — Orphaned code signing clone directories
- **xcode-archives** — Xcode archives in `~/Library/Developer/Xcode/Archives`
- **simulators** — iOS simulators and device support data (`~/Library/Developer/CoreSimulator`, `~/Library/Developer/Xcode/iOS DeviceSupport`)
- **gradle** — Gradle build cache (`~/.gradle/caches`)
- **pip-cache** — Modern pip cache (`~/.cache/pip`)

### Cache mode (`--cache`)
- **puppeteer** — Headless Chrome binaries
- **codex-runtimes** — OpenAI Codex runtimes
- **chroma** — ChromaDB vector database cache
- **whisper-models** — Whisper speech recognition models
- **prisma** — Prisma schema cache
- **go-build** — Go build cache (cleaned via `go clean -cache`)
- **electron** — Electron runtime cache
- **pnpm** — pnpm package manager cache
- And more (fontconfig, node, opencode, gh, etc.)

### AI mode (`--ai`)
- **huggingface** — HuggingFace model cache (`~/.cache/huggingface/hub`)
- **ollama** — Ollama local LLM models
- **torch** — PyTorch hub cache
- **conda** — Conda package cache
- **keras** — Keras model cache
- **pyenv** — pyenv build cache

## Safety features

- **Dry run mode** — Use `--dry-run` to preview before deleting
- **Browser data protection** — Never touches Chrome, Firefox, or Brave profile data (bookmarks, passwords, history)
- **System file protection** — Skips `com.apple.*`, `CloudKit`, and other system caches
- **Installed app protection** — Orphaned data detection skips reverse-domain named folders (com.*, org.*, etc.) and verifies against `/Applications/`
- **Confirmation prompts** — Aggressive mode asks before deleting orphaned app data
- **Graceful failures** — Permission-denied files are skipped, not fatal

## Install

### Homebrew

```bash
brew tap toshon-jennings/tap
brew install cleanmac
```

To upgrade to the latest version:

```bash
brew upgrade cleanmac
```

### Direct download

```bash
curl -sL https://raw.githubusercontent.com/toshon-jennings/cleanmac/main/cleanmac -o /usr/local/bin/cleanmac
chmod +x /usr/local/bin/cleanmac
```

## Usage

```bash
cleanmac                         # Clean developer caches (safe)
cleanmac --aggressive            # Deep clean (app caches, logs, orphaned data)
cleanmac --cache                 # Clean ~/.cache/ directory (puppeteer, playwright, codex, etc.)
cleanmac --ai                    # Clean AI/ML model caches (huggingface, ollama, torch, etc.)
cleanmac --dry-run --aggressive  # Preview what aggressive mode would delete
cleanmac --only npm,docker       # Clean only specific targets
cleanmac --skip xcode            # Clean everything except Xcode
cleanmac --silent                # Suppress output (for cron/scripts)
cleanmac --help                  # Show all options
```

## Options

| Flag | Description |
|------|-------------|
| `--dry-run`, `-n` | Show what would be deleted without deleting anything |
| `--silent`, `-y` | Suppress verbose output (cron-friendly) |
| `--only <targets>` | Comma-separated list of targets to clean (e.g., `docker,npm`) |
| `--skip <targets>` | Comma-separated list of targets to skip (e.g., `xcode,cargo`) |
| `--aggressive`, `-a` | Include app caches, logs, orphaned app data, old installers, code signing clones, simulators, xcode-archives, gradle, pip-cache |
| `--cache`, `-c` | Clean `~/.cache/` directory (puppeteer, playwright, codex, whisper, chroma, prisma, go-build, electron, etc.) |
| `--ai` | Clean AI/ML model caches (huggingface, ollama, torch, conda, keras) |
| `--standard`, `-s` | Explicit standard mode (same as default) |
| `--help`, `-h` | Show help message |

Available targets (standard): `uv`, `pip`, `npm`, `bun`, `brew`, `xcode`, `cargo`, `docker`

Available targets (aggressive): `apps`, `logs`, `orphaned`, `downloads`, `code-signing`, `simulators`, `xcode-archives`, `gradle`, `pip-cache`

Available targets (cache): `puppeteer`, `playwright`, `codex`, `whisper`, `chroma`, `prisma`, `go-build`, `electron`, `pnpm`

Available targets (ai): `huggingface`, `ollama`, `torch`, `conda`, `pyenv`

## Space Savings Summary

After each run, cleanmac prints a breakdown of space reclaimed per target and a total:

```
--- Space Savings Breakdown ---
  uv        882M
  npm       830M
  brew      1.4G
  apps      2.3G
  logs      45M
  orphaned  120M

Total space reclaimed: 5.1 GB
```

## Custom Paths

cleanmac respects standard environment variables for custom tool locations:

| Variable | Default | Used by |
|----------|---------|---------|
| `CARGO_HOME` | `~/.cargo` | Cargo |
| `NPM_CONFIG_CACHE` | `~/.npm` | npm |
| `DOCKER_CONFIG` | `~/.docker` | Docker |

## Automation

Drop it in a cron job or launch agent with `--silent`:

```bash
# Weekly cleanup at 2 AM
0 2 * * 0 /usr/local/bin/cleanmac --silent

# Monthly deep clean
0 2 1 * * /usr/local/bin/cleanmac --silent --aggressive

# Monthly AI/ML cache cleanup
0 3 1 * * /usr/local/bin/cleanmac --silent --ai

# Weekly ~/.cache/ cleanup
0 4 * * 0 /usr/local/bin/cleanmac --silent --cache
```

## Requirements

- macOS
- At least one of the supported tools installed (the script skips anything it doesn't find)

## License

MIT
