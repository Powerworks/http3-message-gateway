# Compounding series

This is **repo #10** (final) in the compounding sequence.

## Complete sequence

| # | Repo | Language/Stack | Core idea extracted |
|---|------|----------------|---------------------|
| 1 | [`c-message-passing`](https://github.com/Powerworks/c-message-passing) | C99 | Actor message passing, mailbox isolation |
| 2 | [`ada-contract-state-machine`](https://github.com/Powerworks/ada-contract-state-machine) | Ada 2012 | Design-by-contract, protected objects |
| 3 | [`soa-contract-first`](https://github.com/Powerworks/soa-contract-first) | C99 / HTTP/JSON | Service contracts as bounded boundaries |
| 4 | [`elixir-supervision-pool`](https://github.com/Powerworks/elixir-supervision-pool) | Elixir / OTP | Supervision trees, let-it-crash |
| 5 | [`swift-notes-offline-first`](https://github.com/Powerworks/swift-notes-offline-first) | Swift / SwiftUI | Value types, actors, offline-first UI |
| 6 | `react-native-promise-era` | JavaScript / React Native | Promises, async/await, JS↔Native bridge |
| 7 | `kotlin-tasks-multiplatform` | Kotlin Multiplatform | Structured concurrency, repository protocol |
| 8 | `wasm-actor-runtime` | C → WASM | Sandboxed message passing at bytecode level |
| 9 | `ebpf-observability` | C + eBPF | Programmable kernelspace tracing & networking |
| 10 | `http3-message-gateway` | C / HTTP/3 | QUIC-based message transport, datagram-first semantics |

Plus the healthcare track: `hl7-mirth-2019` (2019-era Mirth Connect + HL7 v2 + CDA v3 + FHIR DSTU2/STU3).

## The arc of the series

1. **Isolation** — C actors, mailbox isolation (no shared memory)
2. **Contracts** — Ada pre/postconditions, executable specs
3. **Boundaries** — SOA service contracts across processes
4. **Fault recovery** — Elixir OTP supervision trees
5. **Local-first** — Swift offline-first, value types, actors
6. **Structured async** — React Native promises → Kotlin coroutines
7. **Sandboxing** — WASM linear memory as isolation boundary
8. **Observability** — eBPF kernelspace tracing without app changes
9. **Transport** — HTTP/3 QUIC datagrams, message passing at the network layer

Each repo compounds into the next: contracts layer on message passing, boundaries layer on contracts, supervision layers on boundaries, and so on.

## Obsidian links

[[Programming/C|c-message-passing]]  
[[Programming/Ada|ada-contract-state-machine]]  
[[Programming/SOA|soa-contract-first]]  
[[Programming/Elixir|elixir-supervision-pool]]  
[[Programming/Swift|swift-notes-offline-first]]  
[[Programming/JavaScript|react-native-promise-era]]  
[[Programming/Kotlin|kotlin-tasks-multiplatform]]  
[[Programming/WebAssembly|wasm-actor-runtime]]  
[[Programming/eBPF|ebpf-observability]]  
[[Programming/HTTP3|http3-message-gateway]]
