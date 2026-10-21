# http3-message-gateway

HTTP/3 (QUIC) message gateway — datagram-first message transport for actor-style services.

**Thesis:** HTTP/3 over QUIC treats messages as datagrams with built-in multiplexing, connection migration, and 0-RTT — no more head-of-line blocking from TCP. This is message passing at the transport layer: actor-style send/receive semantics carried over QUIC streams and datagrams, with the gateway as the routing hub.

This is **repo #10** (final) in the compounding series on `github.com/Powerworks`:

```
C message passing → Ada contracts → SOA services → Elixir actors → Swift mobile → React Native async → Kotlin mobile → WASM actors → eBPF observability → HTTP/3 gateway
                                                     └── (healthcare track: hl7-mirth-2019)
```

## What's inside

| Path | Purpose |
|------|---------|
| `gateway/` | C HTTP/3 gateway: QUIC listener, datagram router, stream handler |
| `client/` | Example HTTP/3 client: send/receive messages via QUIC |
| `proto/` | Message framing schema (length-prefixed protobuf-style or JSON) |
| `design/` | System design narrative, ADRs, Mermaid diagrams |
| `architecture/` | Principal-level architecture extract (Obsidian-friendly) |
| `docs/` | Compounding links to previous/next projects |

## Build & run

Requires **clang/gcc**, **nghttp3** or **quiche** (QUIC implementation), **OpenSSL 3.x** (with QUIC support).

```bash
# Build gateway
cc -O2 -o gateway/gateway gateway/gateway.c -lquiche -lcrypto -lssl -lpthread

# Run gateway (listens on UDP 4433)
./gateway/gateway --cert cert.pem --key key.pem --bind 0.0.0.0:4433

# Run client
cc -O2 -o client/client client/client.c -lquiche -lcrypto -lssl -lpthread
./client/client --connect 127.0.0.1:4433 --msg "hello"
```

## Design summary

- **QUIC transport** — UDP-based, multiplexed streams, no head-of-line blocking, connection migration
- **HTTP/3 DATAGRAM frames** — unreliable, unordered datagrams for fire-and-forget messages
- **HTTP/3 streams** — reliable, ordered message exchange for request/response
- **Gateway router** — routes incoming messages to actor mailboxes based on message type/address
- **0-RTT** — resumable connections send data immediately on reconnect

See `design/README.md` for the full system design and `architecture/ARCHITECTURE.md` for the principal-level extract.
