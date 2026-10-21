# ADR 0001 — QUIC/HTTP/3 over TCP/HTTP/2

## Status

Accepted

## Context

HTTP/2 over TCP solves multiplexing but still suffers head-of-line blocking: packet loss stalls all streams. QUIC (HTTP/3) is UDP-based, giving true per-stream multiplexing, faster connection setup (0-RTT), connection migration, and unreliable datagrams.

We considered:
1. **TCP + HTTP/2** — widely deployed, but head-of-line blocking limits message throughput.
2. **Raw UDP** — no reliability, ordering, or congestion control; too much to re-implement.
3. **QUIC + HTTP/3** — multiplexed, datagram-capable, connection-migrating, modern.

## Decision

Use QUIC/HTTP/3 as the transport. Actor messages ride on QUIC streams (reliable) or DATAGRAM frames (unreliable), depending on the message type.

## Consequences

### Positive

- **No head-of-line blocking** — packet loss stalls only the affected stream, not the whole connection.
- **0-RTT** — resumed connections send data immediately.
- **Connection migration** — client can change IP without dropping the connection (mobile roaming).
- **Unreliable datagrams** — fire-and-forget messages don't need reliability overhead.

### Negative

- **UDP blocking** — some corporate networks block UDP 443.
- **Implementation maturity** — QUIC libraries (quiche, nghttp3) are newer than OpenSSL/TLS stacks.
- **Middlebox traversal** — UDP is sometimes rate-limited or inspected.

## Related

- [[../architecture/ARCHITECTURE]] — principal extract
- [[../../c-message-passing/architecture/ARCHITECTURE|C message passing]] — same send/receive semantics, QUIC instead of process mailboxes
- [[../../ebpf-observability/architecture/ARCHITECTURE|eBPF observability]] — eBPF can trace QUIC/UDP syscalls for gateway latency
