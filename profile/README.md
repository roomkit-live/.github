# RoomKit

Pure async Python library for multi-channel conversations.

One abstraction — the **room** — to orchestrate messages across SMS, RCS, Email, WhatsApp, Messenger, Voice, WebSocket, HTTP webhooks, and AI channels.

```
Inbound ──► Hook pipeline ──► Store ──► Broadcast to all channels
                                             │
        ┌──────────┬──────────┬────────┬─────┼─────┬────────┬────────┬────────┐
        ▼          ▼          ▼        ▼     ▼     ▼        ▼        ▼        ▼
     SMS/RCS   WhatsApp    Email    Teams  Msgr   Voice     WS       AI    Webhook
```

## Repositories

| Repo | Description |
|---|---|
| [**roomkit**](https://github.com/roomkit-live/roomkit) | Python library — `pip install roomkit` |
| [**roomkit-docs**](https://github.com/roomkit-live/roomkit-docs) | Technical documentation (MkDocs) |
| [**roomkit-website**](https://github.com/roomkit-live/roomkit-website) | Landing page — [roomkit.live](https://www.roomkit.live) |
| [**roomkit-specs**](https://github.com/roomkit-live/roomkit-specs) | Protocol specs and RFCs |

## Quick Start

```bash
pip install roomkit
```

```python
from roomkit import RoomKit

kit = RoomKit()

@kit.on_event
async def handle(ctx):
    await ctx.broadcast()

await kit.run()
```

## Links

- [Website](https://www.roomkit.live)
- [Documentation](https://www.roomkit.live/docs/)
- [PyPI](https://pypi.org/project/roomkit/)
