# tgnotify

[![CI](https://github.com/rezhajulio/tgnotify/actions/workflows/ci.yml/badge.svg)](https://github.com/rezhajulio/tgnotify/actions/workflows/ci.yml)
[![GitHub release](https://img.shields.io/github/v/release/rezhajulio/tgnotify)](https://github.com/rezhajulio/tgnotify/releases)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)

📣 Send Telegram notifications from your terminal, scripts, and cron jobs — with a single command.

`tgnotify` is a tiny, dependency-free CLI that delivers messages to any Telegram chat through the Bot API. No config files, no daemons, no SDKs: just a bot token, a chat ID, and a message.

## Why tgnotify?

- **Zero dependencies** — one static binary built on Go's standard library. Nothing to `pip install`, nothing to `npm i`, nothing to break on update day.
- **Rich text out of the box** — messages go out with `parse_mode: HTML`, so `<b>bold</b>`, `<i>italic</i>`, `<code>code</code>`, and `<a href="...">links</a>` just work.
- **Script-friendly** — the token comes from `TELEGRAM_BOT_TOKEN` (never from argv, so it stays out of shell history and `ps`), and the exit code tells your script exactly what happened. Made for cron, CI pipelines, and alerting hooks.
- **Cross-platform** — stripped binaries for Linux, macOS, and Windows on every release.

## Quick start

1. Talk to [@BotFather](https://t.me/BotFather) and create a bot to get a token.
2. Find your chat ID — message [@userinfobot](https://t.me/userinfobot), or open `https://api.telegram.org/bot<token>/getUpdates` after messaging your bot.
3. Grab the binary for your platform from [Releases](https://github.com/rezhajulio/tgnotify/releases).

```sh
export TELEGRAM_BOT_TOKEN="123456:ABC-def..."
tgnotify <chat_id> "deploy <b>finished</b> ✅"
# ok: message sent
```

## Usage

```
tgnotify <chat_id> <message...>
```

Words after `<chat_id>` are joined with spaces, so quoting is only needed for special characters.

### Exit codes

| Code | Meaning                                               |
| ---- | ----------------------------------------------------- |
| `0`  | Message sent                                          |
| `1`  | Telegram API rejected the request (details printed)   |
| `2`  | Usage error: token missing/empty, or arguments missing |

### Examples

```sh
# cron alert with hostname and rich text
TELEGRAM_BOT_TOKEN=xxx tgnotify -1001234567890 "backup <b>FAILED</b> on $(hostname)"

# CI pipeline notification with a link
tgnotify "$CHAT_ID" "build #${GITHUB_RUN_NUMBER} <a href=\"${RUN_URL}\">passed</a>"
```

## Install from source

You need the [Lisette](https://lisette.run) toolchain (it bundles Go):

```sh
curl -fsSL https://lisette.run/install.sh | sh
git clone https://github.com/rezhajulio/tgnotify.git
cd tgnotify
lis check && lis build
./target/.lisette/bin/tgnotify <chat_id> "hello from source"
```

## How it works

`tgnotify` is written in [Lisette](https://lisette.run) — a Rust-flavored language that compiles to readable Go — and talks to the Telegram Bot API with a plain HTTPS `POST`. No third-party libraries on either side of the fence: `sendMessage` is just JSON over HTTPS, and Go's standard library is all it takes.

## License

MIT — see [LICENSE](LICENSE).
