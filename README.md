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
cleanmac
```

That's it. No flags, no config. Run it whenever your disk needs breathing room.

## Requirements

- macOS
- At least one of the supported tools installed (the script skips anything it doesn't find)

## License

MIT
