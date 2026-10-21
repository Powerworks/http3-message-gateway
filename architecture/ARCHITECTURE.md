# Architecture extract — http3-message-gateway

**Level:** Principal  
**Purpose:** Extract architecture-relevant bits that compound into the next projects in this series (and the series capstone).  
**Format:** Obsidian-friendly markdown with `[[wikilinks]]`.

## Signal (carry forward)

| Pattern | Observation | Why it matters |
|---------|-------------|----------------|
| **QUIC as transport** | UDP-based, multiplexed, no head-of-line blocking. | Message passing at the transport layer — actors communicate over datagrams and streams. |
| **HTTP/3 DATAGRAM** | Unreliable, unordered messages. | Fire-and-forget actor messages without reliability overhead. |
| **Message framing** | Length-prefixed, type-tagged binary frame with `actor_id`. | Same contract discipline as SOA (repo #3) and Ada contracts (repo #2), but binary and compact. |
| **Gateway as router** | Central frame router dispatches to actors by `actor_id`. | The gateway is the mailbox server; actors are pure functions of their inbound frames. |

## Noise (deliberately skipped at this layer)

- **TLS 1.3 / QUIC crypto details** — transport security is important but orthogonal to the message-passing architecture.
- **QUIC connection migration state machine** — complex; not needed for the reference.
- **HTTP/3 QPACK header compression** — optimization, not architecture.

## Principal takeaway

> Message passing doesn't need processes, mailboxes, or even reliable streams — QUIC datagrams give you actor-style send/receive with connection migration and zero head-of-line blocking. The gateway is the final evolution of the mailbox: a network-addressable router that delivers framed messages to logical actors.

This completes the series: from C processes (repo #1) to WASM sandboxes (repo #8), eBPF observability (repo #9), and now network-native message passing (repo #10).

## Compounds into

- [[../../c-message-passing/architecture/ARCHITECTURE|C message passing]] — same send/receive semantics, QUIC instead of OS mailboxes
- [[../../ebpf-observability/architecture/ARCHITECTURE|eBPF observability]] — eBPF can trace QUIC/UDP syscalls for gateway latency and context switches
- [[../../wasm-actor-runtime/architecture/ARCHITECTURE|WASM actors]] — WASM actors can run inside the gateway as per-connection actors
