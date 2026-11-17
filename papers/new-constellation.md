# New Constellation

- **Authors:** Quentin Kniep and Roger Wattenhofer
- **Published:** 14 November 2026
- **Original source:** [wattenhoferSOL/SolanaMCP](https://github.com/wattenhoferSOL/SolanaMCP)
- **Version read:** Version 1.0, 14 November 2026 ([New Constellation v1.0.pdf](https://github.com/wattenhoferSOL/SolanaMCP/blob/main/New%20Constellation%20v1.0.pdf))
- **Topics:** Solana, Alpenglow, multiple concurrent proposers, MEV, transaction ordering

## Why read this?

New Constellation (NewCon) puts multiple concurrent proposers on Solana’s Alpenglow consensus. Instead of one leader, several co-located proposers each build a block for the same slot, and validators merge those blocks into one order. The design aims to remove the leader’s temporary monopoly on inclusion and ordering, which today lets a block producer sandwich, front-run, or quietly censor users.

## Reading guide

The paper’s own name for the protocol is New Constellation, or NewCon. Earlier drafts called it Spectral, and an earlier Constellation design (Legacy Constellation) is a different protocol. The club materials also call this work Solana MCP, for multiple concurrent proposers.

The [reading summary](../sessions/2026-11-17-new-constellation/notes.md) walks through one slot, block and transaction hiding, hubs, and how the design compares with PBS, Jito BAM, and Harmonic. The paper states the properties it is aiming for: no single party controls inclusion or ordering, a transaction needs only one honest proposer among the slot’s proposers unless the slot is skipped, ordering is a deterministic priority-fee merge, and blocks stay hidden until every proposer has committed.

## Reading sessions

- [17 November 2026](../sessions/2026-11-17-new-constellation/README.md) — discussion with the authors, plus the reading summary and transcript.
