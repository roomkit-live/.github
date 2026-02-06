<h1 align="center">RoomKit</h1>

<p align="center">Pure async Python library for multi-channel conversations.</p>

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
import asyncio
from roomkit import (
    ChannelCategory, InboundMessage, MockAIProvider,
    RoomKit, TextContent, WebSocketChannel,
)
from roomkit.channels.ai import AIChannel

async def main():
    kit = RoomKit()

    # A customer on WebSocket, a support agent on another, and an AI assistant
    kit.register_channel(WebSocketChannel("customer-ws"))
    kit.register_channel(WebSocketChannel("agent-ws"))
    kit.register_channel(AIChannel("assistant", provider=MockAIProvider(
        responses=["I found order #1234 — it shipped yesterday and is expected tomorrow."]
    )))

    # One room bridges all three
    await kit.create_room(room_id="support-42")
    await kit.attach_channel("support-42", "customer-ws")
    await kit.attach_channel("support-42", "agent-ws")
    await kit.attach_channel("support-42", "assistant", category=ChannelCategory.INTELLIGENCE)

    # Customer sends a message → stored, broadcast to agent, AI responds
    await kit.process_inbound(InboundMessage(
        channel_id="customer-ws",
        sender_id="customer-1",
        content=TextContent(body="Where is my order #1234?"),
    ))

    # Unified conversation timeline across all channels
    for event in await kit.store.list_events("support-42"):
        print(f"[{event.source.channel_id}] {event.content.body}")
    # [customer-ws] Where is my order #1234?
    # [assistant]   I found order #1234 — it shipped yesterday and is expected tomorrow.

asyncio.run(main())
```

## Links

- [Website](https://www.roomkit.live)
- [Documentation](https://www.roomkit.live/docs/)
- [PyPI](https://pypi.org/project/roomkit/)
