# cleanmac

A simple, zero-dependency bash script to clean up developer caches, logs, and build artifacts on macOS.

## What it cleans

- **uv** — Python package manager cache
- **pip** — Python pip cache
- **npm** — Node.js package manager cache
- **bun** — Bun package manager cache
- **Homebrew** — Brew cleanup
- **Xcode** — DerivedData
- **Cargo** — Rust global cache
- **Docker** — Unused images, containers, volumes (only if Docker is running)

## Install

### Homebrew

```bash
brew tap toshon-jennings/tap
brew install cleanmac
```

### Direct download

```bash
curl -sL https://raw.githubusercontent.com/toshon-jennings/cleanmac/main/cleanmac -o /usr/local/bin/cleanmac
chmod +x /usr/local/bin/cleanmac
```

## Usage

```bash
cleanmac                  # Clean everything
cleanmac --dry-run        # Preview what would be deleted (no changes)
cleanmac --only npm,docker  # Clean only specific targets
cleanmac --skip xcode     # Clean everything except Xcode
cleanmac --silent         # Suppress output (for cron/scripts)
cleanmac --help           # Show all options
```

## Options

| Flag | Description |
|------|-------------|
| `--dry-run`, `-n` | Show what would be deleted without deleting anything |
| `--silent`, `-y` | Suppress verbose output (cron-friendly) |
| `--only <targets>` | Comma-separated list of targets to clean (e.g., `docker,npm`) |
| `--skip <targets>` | Comma-separated list of targets to skip (e.g., `xcode,cargo`) |
| `--help`, `-h` | Show help message |

Available targets: `uv`, `pip`, `npm`, `bun`, `brew`, `xcode`, `cargo`, `docker`

## Space Savings Summary

After each run, cleanmac prints a breakdown of space reclaimed per target and a total:

```
--- Space Savings Breakdown ---
  uv        882M
  npm       830M
  brew      1.4G

Total space reclaimed: 3.1 GB
```

## Custom Paths

cleanmac respects standard environment variables for custom tool locations:

| Variable | Default | Used by |
|----------|---------|---------|
| `CARGO_HOME` | `~/.cargo` | Cargo |
| `NPM_CONFIG_CACHE` | `~/.npm` | npm |

## Automation

Drop it in a cron job or launch agent with `--silent`:

```bash
# Weekly cleanup at 2 AM
0 2 * * 0 /usr/local/bin/cleanmac --silent
```

## Requirements

- macOS
- At least one of the supported tools installed (the script skips anything it doesn't find)

## License

MIT
