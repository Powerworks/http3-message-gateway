# System design — http3-message-gateway

## Scope

An HTTP/3 (QUIC) message gateway that routes datagram-first messages between actor-style services. QUIC replaces TCP: multiplexed streams, no head-of-line blocking, connection migration, and optional unreliable datagrams (HTTP/3 DATAGRAM frames).

This is message passing at the transport layer — the same send/receive semantics as repo #1 (C message passing), but carried over QUIC instead of process mailboxes.

## Components

### Gateway (`gateway/gateway.c` / `.h`)

- Registers actors with stream IDs and reliability mode (stream vs datagram)
- Routes incoming HTTP/3 messages to the correct actor based on `actor_id` in the frame
- Tracks per-actor stats (message count, bytes, last activity)

### Message framing (`gateway/msg_frame.c` / `.h`)

Length-prefixed, type-tagged binary frame:
- `type` — DATAGRAM, STREAM, or ACK
- `flags` — reliability and ordering hints
- `actor_id` — destination actor address
- `seq` — sequence number for ordering (stream mode)
- `len` + `payload` — variable-length body

### QUIC transport (conceptual)

- UDP-based, no head-of-line blocking across streams
- HTTP/3 DATAGRAM frames for unreliable, unordered messages
- HTTP/3 streams for reliable, ordered request/response
- 0-RTT for connection resumption
- Connection migration (IP change without reconnect)

## Transport model

| Property | TCP/HTTP/2 | QUIC/HTTP/3 |
|----------|------------|-------------|
| Head-of-line blocking | Yes (per connection) | No (per stream) |
| Connection setup | 1-RTT (TCP + TLS) | 0-RTT (QUIC) |
| Multiplexing | One stream | Many streams + datagrams |
| Connection migration | No | Yes |
| Unreliable datagrams | No | Yes (HTTP/3 DATAGRAM) |

## Decisions recorded

See `design/adr/0001-quic-over-tcp.md`.

## Diagrams

See `design/diagrams/http3-gateway-topology.mmd`.
