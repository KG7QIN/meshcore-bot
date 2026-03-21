# TODO

Task list for meshcore-bot development. Auto-updated sections are regenerated
by running `python scripts/update_todos.py` (see [Auto-Update](#auto-update)).

**Last updated:** 2026-03-21 — 2,167 passed / 29 skipped; coverage 36.72%;
target 40%; CI matrix fixed (Python 3.9 removed, ruff/mypy/ShellCheck green)

---

## Pending Work

### Test Coverage

Coverage is at **36.72%** (`fail_under=35`). Target is **40%**. Hardware-dependent
modules cap the realistic ceiling at around 40–42%.

**Tier 1 — High impact:**

- [ ] T1-E: `feed_manager.py` — polling loop still needed
- [ ] T1-F: `web_viewer/app.py` (41%, ~2,269 uncovered) — greeter, bans, packets/messages endpoints

**Tier 2 — Medium impact:**

- [ ] T2-A: `utils.py` (60%, ~403 uncovered) — `format_keyword_response`, `calculate_path_distances`
- [ ] T2-B: `graph_trace_helper.py` (2%, ~159 uncovered) — pure graph/trace, zero hardware deps
- [ ] T2-C: `db_manager.py` (54%, ~147 uncovered) — `AsyncDBManager` async methods, write queue
- [ ] T2-D: `discord_bridge_service.py` (31%, ~239 uncovered) — message formatting, webhook dispatch
- [ ] T2-E: `telegram_bridge_service.py` (36%, ~195 uncovered) — message relay, topic routing
- [ ] T2-F: `greeter_command.py` (15%, ~557 uncovered) — greeting detection, per-channel greetings

**Tier 3 — Smaller commands:**

- [ ] T3-C: `channels_command.py` (86%, ~33 uncovered) — remaining paths
- [ ] T3-I: `trace_runner.py` (24%, ~50 uncovered) — trace execution, path assembly
- [ ] T3-J: `earthquake_service.py` (16%, ~119 uncovered) — alert threshold, message format
- [ ] T3-L: `hacker_command.py` (17%, ~101 uncovered) — text transform logic
- [ ] T3-M: `sports_command.py` (16%, ~325 uncovered) — score formatting, schedule display
- [ ] T3-O: `repeater_command.py` (10%, ~363 uncovered) — repeater list/info formatting
- [ ] T4-A: `multitest_command.py` (33%, ~220 uncovered) — multi-channel test sequences

**Tier 4 — API/hardware heavy (skip for now):**

`wx_command.py`, `weather_service.py`, `solar_conditions.py`, `solarforecast_command.py`,
`packet_capture_service.py`, `map_uploader_service.py`, `airplanes_command.py`,
`aqi_command.py`, `alert_command.py`, `prefix_command.py`, `packet_capture_utils.py`

### Other Open Tasks

- [ ] MQTT: Add packet content parser tests using fixture data (decode raw hex, validate payload types)

---

## Planned Features

### Bridges

- [ ] Two-way Discord bridge — receive messages from Discord and relay to MeshCore
- [ ] Two-way Telegram bridge — relay Telegram messages back into MeshCore channels
- [ ] Telegram `message_thread_id` support — route bridged messages to forum topics
- [ ] Bridge DM support — optional, opt-in bridging of DMs (requires consent mechanism)

### Web Viewer

- [ ] Mobile-responsive improvements — optimize layout for small screens

### Commands and Features

- [ ] `!wx` non-US improvement — promote `wx_international.py` to default with US fallback

---

## Completed

### Test Coverage (2026-03-15 – 2026-03-21)

- [x] (2026-03-21) Extended `test_message_handler.py` — `TestHandleNewContactOutPathHashMode` (4 tests)
- [x] (2026-03-21) Extended `test_scheduler_logic.py` — interval advert and scheduled message timeout tests (8 tests)
- [x] (2026-03-21) Extended `test_core.py` — `TestProbeRadioHealth` (10 tests)
- [x] (2026-03-16) T1-G: `web_viewer/integration.py` — `tests/test_web_viewer_integration.py`
- [x] (2026-03-16) T1-D: `scheduler.py` — extended (106 total tests)
- [x] (2026-03-16) T1-C: `repeater_manager.py` — extended (131 total tests)
- [x] (2026-03-16) T1-B: `message_handler.py` — extended (139 total tests)
- [x] (2026-03-16) T1-A: Realtime viewer panels blank bug fixed (third pass, BUG-029)
- [x] (2026-03-16) T2-G: `rate_limiter.py` — extended to 98%
- [x] (2026-03-16) T2-H: `stats_command.py` — extended to 61% (66 total tests)
- [x] (2026-03-16) T2-I: `i18n.py` — `tests/test_i18n.py` (98% coverage)
- [x] (2026-03-16) T3-A: `trace_command.py` — `tests/test_trace_command.py` (88%)
- [x] (2026-03-16) T3-B: `announcements_command.py` — `tests/test_announcements_command.py`
- [x] (2026-03-16) T3-D: `help_command.py` — `tests/test_help_command.py`
- [x] (2026-03-16) T3-E: `aurora_command.py` — `tests/test_aurora_command.py`
- [x] (2026-03-16) T3-F: `joke_command.py` — `tests/test_joke_command.py`
- [x] (2026-03-16) T3-G: `dadjoke_command.py` — `tests/test_dadjoke_command.py`
- [x] (2026-03-16) T3-K: `moon_command.py` — `tests/test_moon_command.py`
- [x] (2026-03-16) `tests/test_channel_manager.py` — 47 new tests
- [x] (2026-03-16) `tests/test_web_viewer.py` — 19 new tests (220 total)
- [x] (2026-03-16) Fixed `test_weekly_on_wrong_day_does_not_run` — patching bug (BUG-027)
- [x] (2026-03-15) `tests/test_enums.py` — enum values and flag combinations
- [x] (2026-03-15) `tests/test_models.py` — MeshMessage dataclass
- [x] (2026-03-15) `tests/test_transmission_tracker.py` — full TransmissionTracker
- [x] (2026-03-15) `tests/test_message_handler.py` — path parsing, cache, message routing
- [x] (2026-03-15) `tests/test_repeater_manager.py` — role detection, ACL, device type
- [x] (2026-03-15) `tests/test_core.py` — config loading, radio settings, reload
- [x] (2026-03-15) `tests/test_feed_manager.py` — queue insert, deduplication, interval due-check
- [x] (2026-03-15) `tests/test_scheduler_logic.py` — scheduled message dispatch, advert setup
- [x] (2026-03-15) `tests/test_command_manager.py` — command dispatch, keyword matching
- [x] (2026-03-15) `tests/test_channel_manager_logic.py` — cache lifecycle, fetch-all, connectivity guard

### MQTT Test Framework

- [x] (2026-03-15) `tests/test_mqtt_live.py` — schema validation + live MQTT integration tests
  - Live: `pytest tests/test_mqtt_live.py -m mqtt`
  - Offline fixtures: `pytest tests/test_mqtt_live.py -m "not mqtt"`
  - Collect fixtures: `python tests/test_mqtt_live.py --collect-fixtures`
- [x] (2026-03-15) `tests/mqtt_test_config.ini` — broker/topic/timeout config
- [x] (2026-03-15) `tests/fixtures/mqtt_packets.json` — 8 real packets from SEA region

### Bug Fixes

- [x] (2026-03-21) Fixed `OverflowError` in contact path update (`fe3a1c2`)
- [x] (2026-03-21) Fixed blank error log in interval advert send (`cfdcf4f`)
- [x] (2026-03-21) Fixed blank error log in scheduled message send (`122997b`)
- [x] (2026-03-21) Fixed `_send_interval_advert_async` silently swallowing failures (`2b7ab12`)
- [x] (2026-03-21) Fixed `_send_scheduled_message_async` hanging on unresponsive radio (`38e9354`)
- [x] (2026-03-21) Added `_probe_radio_health()` zombie-connection detector (`6055b54`)
- [x] (2026-03-17) Alias refactor — per-command `aliases =` key replaces global `[Aliases]` section (`164dbae`)
- [x] (2026-03-17) Discord bridge test fix — `ConfigParser` lowercase key (`f971e97`)
- [x] (2026-03-16) BUG-029 fix (third pass) — SocketIO manager race and db_path resolution (`26d18c1`)
- [x] (2026-03-15) BUG-025 — `send_channel_message` retry on `no_event_received`
- [x] (2026-03-15) BUG-026 — help/long response truncation; response chunking added
- [x] (2026-03-15) BUG-024 — DB backup scheduler firing every second
- [x] (2026-03-15) BUG-023 — realtime command stream blank on load
- [x] (2026-03-15) BUG-022 — asyncio `IndexError` crash suppressed at DEBUG
- [x] (2026-03-15) BUG-015 to BUG-021 — scheduler timeouts, web viewer, config TUI
- [x] (2026-03-15) BUG-001 to BUG-003 — web viewer auth, DB migrations, geocoding log level

### Feature Work

- [x] (2026-03-15) Web viewer authentication — `web_viewer_password` in `[Web_Viewer]`
- [x] (2026-03-15) Radio reboot/connect/disconnect buttons in web UI
- [x] (2026-03-15) Live packet streaming (Live Activity panel, SocketIO feed)
- [x] (2026-03-15) Real-time message and log monitoring
- [x] (2026-03-15) Interactive contact management — star, purge inactive, export to CSV/JSON
- [x] (2026-03-15) Configuration tab (`/config`) — SMTP, maintenance, log rotation, DB backup
- [x] (2026-03-15) Command aliases — per-command `aliases =` config key
- [x] (2026-03-15) Scheduled message preview (`!schedule` DM command)
- [x] (2026-03-15) Inbound webhook service (`POST /webhook`, bearer token auth)
- [x] (2026-03-15) Per-channel rate limiting (`[Rate_Limits]` config section)
- [x] (2026-03-15) Nightly maintenance email with digest report
- [x] (2026-03-15) DB backup scheduling with configurable retention
- [x] (2026-03-15) Log rotation configurable via `[Logging]` section
- [x] (2026-03-15) `!path` geographic scoring toggle
- [x] (2026-03-15) Discord and Telegram one-way bridges
- [x] (2026-03-15) Multi-byte prefix (2-byte) support throughout
- [x] (2026-03-15) `!schedule` command (DM-only, shows times/channels/messages)

### Infrastructure / CI

- [x] (2026-03-21) CI: ruff, mypy, ShellCheck all green (PR #123)
- [x] (2026-03-21) CI: Python 3.9 removed from test matrix (PR #122)
- [x] (2026-03-15) ruff CI gate and mypy incremental strict mode
- [x] (2026-03-15) HTML/JS test framework (ESLint + HTMLHint), `lint-frontend` CI job
- [x] (2026-03-15) ShellCheck CI gate, all `.sh` files at `--severity=warning`
- [x] (2026-03-15) Docker multi-arch build (amd64 + arm64 + armv7), SBOM + provenance
- [x] (2026-03-15) .deb packaging and systemd service (`make deb`)
- [x] (2026-03-15) Database migration versioning (`MigrationRunner`, 5 migrations)
- [x] (2026-03-15) aiosqlite async DB (`AsyncDBManager`)
- [x] (2026-03-15) pytest-timeout and coverage threshold (`fail_under=35`)
- [x] (2026-03-15) Context checkpoint system (cron + SESSION_RESUME.md)
- [x] (2026-03-15) ncurses config TUI (`make config`)
- [x] (2026-03-15) Makefile (`make install/dev/test/lint/fix/deb/config/clean`)
- [x] (2026-03-15) Structured JSON logging (Loki/Elasticsearch/Splunk compatible)
- [x] (2026-03-15) APScheduler migration (`BackgroundScheduler` + `CronTrigger`)
- [x] (2026-03-15) Werkzeug WebSocket fix (`_apply_werkzeug_websocket_fix()`)

---

## Backlog

- [ ] Evaluate moving web viewer to a separate installable package
- [ ] Repeater auto-purge dry-run mode — log what would be purged without acting
- [ ] Feed manager: add support for JSON API feeds (not just RSS/Atom)
- [ ] Mobile-responsive web viewer improvements — optimize layout for small screens

---

## Auto-Update

The **Inline TODOs** section below is auto-generated by scanning source files for
`# TODO`, `# FIXME`, and `# HACK` markers. Regenerate it with:

```bash
python scripts/update_todos.py
```

The script also updates the `**Last updated:**` date at the top of this file.

Or run it as part of a pre-commit hook by adding to `.pre-commit-config.yaml`:

```yaml
- repo: local
  hooks:
    - id: update-todos
      name: Update TODO.md inline scan
      language: python
      entry: python scripts/update_todos.py
      pass_filenames: false
```

---

## Inline TODOs (auto-generated)

> _Last scanned: 2026-03-21. No `# TODO`, `# FIXME`, or `# HACK` markers
> found in `modules/` or `tests/`. Run `python scripts/update_todos.py` to refresh._

