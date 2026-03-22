# BUGS

Known bugs and fixed issues in meshcore-bot.

---

## Outstanding Issues

### High Priority

| ID      | Module          | Description                                                  | Workaround                                   |
|---------|-----------------|--------------------------------------------------------------|----------------------------------------------|
| BUG-028 | message_handler | `decode_meshcore_packet()` except-handler raises             | Init `byte_data = b""` before the `try`      |
|         |                 | `UnboundLocalError` on invalid hex input                     | block in `message_handler.py`                |

### Medium Priority

| ID      | Module                  | Description                                              | Workaround                                   |
|---------|-------------------------|----------------------------------------------------------|----------------------------------------------|
| BUG-004 | message_handler         | RF correlation (SNR/RSSI) can miss messages if the RF    | Increase `rf_data_timeout` in `[Bot]`        |
|         |                         | log event arrives more than `rf_data_timeout` (15 s)     |                                              |
| BUG-005 | scheduler               | On Pi Zero 2 W, bot + web viewer together use ~300 MB    | Disable web viewer or set                    |
|         |                         | RAM, leaving little headroom under load                  | `graph_startup_load_days = 7`                |
| BUG-006 | feed_manager            | Stale rows in `feed_message_queue` from an old install   | `DELETE FROM feed_message_queue`             |
|         |                         | can cause repeated queue-processing errors               | `WHERE sent_at IS NULL`                      |
| BUG-007 | discord_bridge_service  | Discord webhook rate limit is 30 req/min; excess         | Keep bridged channels low-traffic            |
|         |                         | messages are dropped, not queued                         |                                              |
| BUG-008 | telegram_bridge_service | `message_thread_id` (forum topics) not implemented;      | All messages go to the main group only       |
|         |                         | all messages go to the main group channel                |                                              |

### Low Priority

| ID      | Module                 | Description                                              | Notes                                        |
|---------|------------------------|----------------------------------------------------------|----------------------------------------------|
| BUG-009 | message_handler        | DMs are never bridged to Discord or Telegram             | By design — DMs are private                  |
| BUG-010 | wx_command             | Weather alerts and NOAA data are US-only                 | Use `wx_international.py` alternative        |
| BUG-011 | repeater_manager       | MeshCore hard-limits contacts to 300; auto-purge         | Tune `auto_purge_threshold` and enable       |
|         |                        | (20 at a time) may not keep up on busy meshes            | `auto_manage_contacts`                       |
| BUG-012 | plugin_loader          | Local plugins with the same name as a built-in are       | Rename your local plugin to a unique name    |
|         |                        | silently skipped; no override-by-name is possible        |                                              |
| BUG-013 | core.py                | Some older firmware versions don't support `get_time`    | Upgrade firmware; no impact on messaging     |
|         |                        | or `set_name`; bot logs a warning and continues          |                                              |
| BUG-014 | packet_capture_service | Packet hash calculation silently falls back to a         | Low impact; affects deduplication only       |
|         |                        | default value on failure                                 |                                              |

---

## Fixed Bugs

| Commit      | Bug(s)  | Module                  | Fix                                                          |
|-------------|---------|-------------------------|--------------------------------------------------------------|
| `6055b54`   | —       | core                    | Added `_probe_radio_health()` zombie-connection probe        |
| `38e9354`   | —       | scheduler               | Wrapped `send_channel_message` with `asyncio.wait_for`       |
| `2b7ab12`   | —       | scheduler               | Check `Event` result in `_send_interval_advert_async`        |
| `122997b`   | —       | scheduler               | Log `type(e).__name__` in `send_scheduled_message`           |
| `cfdcf4f`   | —       | scheduler               | Log `type(e).__name__` in `send_interval_advert`             |
| `fe3a1c2`   | —       | message_handler         | Reset `out_path_hash_mode = 0` in both path update branches  |
| `e0eae09`   | —       | CI                      | Fixed ruff, mypy, and ShellCheck failures (PR #123)          |
| `92c5910`   | —       | CI                      | Removed Python 3.9 from test matrix (PR #122)                |
| `164dbae`   | —       | command_manager         | Moved aliases to per-command `aliases =` config key          |
| `f971e97`   | —       | tests                   | Fixed `ConfigParser` lowercase key in Discord bridge test    |
| `26d18c1`   | 025–029 | web_viewer / scheduler  | Send retry, response chunking, test fix, db_path + SocketIO  |
| `ab72be9`   | 015–024 | scheduler / web_viewer  | Scheduler timeouts, web viewer history replay, config TUI    |
| `1264f49`   | —       | repeater_manager        | Auto-purge now respects `auto_manage_contacts`; stale data   |
| `5c8ee35`   | —       | utils                   | Timezone fix in `format_elapsed_display` (issue #75)         |
| `1cc41bc`   | —       | repeater_manager        | Repeater usage and web viewer formatting (PR #67)            |
| `1474174`   | —       | trace_command           | Fixed `TraceCommand` path truncation                         |
| `1a576e8`   | —       | trace_command           | Fixed reversed path nodes in `TraceCommand`                  |
| `5a96dec`   | —       | trace_command           | Fixed incorrect hop labeling in `TraceCommand`               |
| `2178a80`   | —       | core                    | Fixed log spam during shutdown                               |
| `e9f17ec`   | —       | core                    | Fixed incomplete shutdown (scheduler + disconnect)           |
| `217d2a4`   | —       | db_manager              | Fixed database connection handling across modules            |
| `d084c6b`   | —       | prefix_command          | Fixed multi-byte hex prefix lookups                          |
| `6c81513`   | —       | mesh_graph              | Fixed `MeshGraph` edge promotion logic                       |
| `36a8a67`   | —       | core                    | Fixed 1-byte to 2-byte prefix transition handling            |
| `0c060a5`   | —       | command_manager         | Fixed chunked message race with rate limiter                 |
| `58deb12`   | —       | repeater_manager        | Fixed `auto_manage_contacts = false` being ignored           |

---

## Design / Implementation Notes

| Module          | Description                                              | Notes                                        |
|-----------------|----------------------------------------------------------|----------------------------------------------|
| message_handler | Keyword-dispatched responses sent as a single message    | Mesh-friendly by design; use                 |
|                 | (no auto-chunking); long responses may be truncated      | `send_response_chunked()` if needed          |

---

## Reporting New Bugs

Open an issue at the project repository. Include:
- Bot version (`git describe --tags`)
- Relevant section of `config.ini` (redact keys/tokens)
- Log output (`logs/meshcore_bot.log`) around the time of the issue
- Steps to reproduce
