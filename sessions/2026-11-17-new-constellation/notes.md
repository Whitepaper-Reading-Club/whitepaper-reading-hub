# Reading notes — New Constellation

- **Session:** [New Constellation — 2026-11-17](README.md)
- **Contributors:** Not named in the source summary.
- **Status:** Reading summary prepared for the 17 November 2026 session.

These notes are the club’s summary of the paper. Claims below are the summary’s account of New Constellation version 1.0. The room’s questions and the authors’ replies are in the [transcript](transcript.md), not here.

## Summary

New Constellation brings multiple concurrent proposers (MCP) to Solana: three co-located proposers build every 50 ms slot side by side and keep their blocks sealed until all have committed, so no single party can censor, front-run, or sandwich users.

Whoever leads a Solana block today sees every pending transaction before anyone else and decides what makes it in. That position enables sandwiching, front-running, and quiet censorship. New Constellation aims to make fast, neutral inclusion the default, so liquidations, swaps, and other applications can run onchain without having to trust the block producer.

## Key ideas from the paper

New Constellation breaks up the leader’s temporary monopoly over transaction flow. No proposer gets to see what the others are including before its own block is locked, so nobody has a last look at other people’s orders. A transaction gets in as long as one of the proposers it was sent to is honest; the only way to keep it out is to skip the slot for everyone, which is visible. The final order follows a fixed priority-fee rule that every validator computes for itself. It runs on a 50 ms tick, with no synchronised clocks and no extra network hop.

### How ordering works today

Each Solana leader runs a scheduler that sorts transactions roughly by priority fee per compute unit. That is convention, not a protocol rule. The leader’s real edge is information: it sees every pending transaction and its fee, so it can slot its own trade, or sell the spot to a searcher, just ahead of a user’s.

Alpenglow, by Kniep, Sliwinski, and Wattenhofer, is the consensus protocol underneath. Its voting half, Votor, went live on mainnet on 21 October 2026. Its other half, Rotor, spreads erasure-coded pieces of each block through one layer of relays and has no release date yet.

ParentReady is the Alpenglow event that starts a leader window. A validator emits it once it holds a notarisation certificate for an earlier block plus skip certificates for every slot in between. It needs no finality and no clock.

The first Constellation paper (March 2026) used proposers and attesters with synchronised clocks, and a leader that still assembled the block. A new design was published on 14 November 2026, called New Constellation.

### One slot, end to end

Each window has *k* = 3 proposers, all within a small delay Δlocal of each other (10 ms proposed). A proposer starts its window on ParentReady and then produces 16 blocks back to back. A slot runs like this:

1. The proposer generates a fresh symmetric key, packs transactions for 50 ms, and streams the encrypted block through Rotor as it goes.
2. With the last piece it sends a signed hash of the block, which also binds the key, straight to its co-proposers.
3. Once it has their hashes, or after waiting 2Δlocal at most, it sends every validator a signed attestation listing which co-proposers were on time and what they committed to, together with its own key.
4. Validators decrypt the accepted blocks, merge them, and execute.

Each proposer times that wait on its own clock, so nothing needs synchronising. Attestations go directly to validators while blocks take Rotor’s relay hop, so both arrive at about the same time: the commit-and-reveal step adds no latency.

Validators vote once every 4 slots. Those 4 slots form one consensus block (cblock), which is all Alpenglow sees: a 16-slot window is an ordinary leader window of four 200 ms blocks. The vote covers the resulting state hash.

### When proposers fail

A proposer that is down from the start of its window costs little. Its two co-proposers both mark it late, their attestations agree, and the window carries on with two blocks per slot. Validators hold their first vote for a bounded time in case the missing attestation is still on its way, which also stops two proposers from attesting only each other to squeeze out an honest third.

If the attestations for a slot conflict, say because one proposer saw a hash in time and another did not, validators vote skip. That discards the cblock and, as in Alpenglow, the rest of the window. A single lost hash can cause this, so hashes are sent several times. A malicious proposer can also burn a window on purpose by sending its hash to only some co-proposers.

Accountability comes from signatures. A proposer marked late by its peers is identifiable and can be sanctioned, and one that signs two different blocks or attestations for a slot can be proven guilty and slashed. This is also why slots cannot be stretched: under Alpenglow a leader can run late up to the global timeout to gather more flow, whereas here the geographically close peers cap lateness at a few milliseconds. With *k* = 2 a single crash leaves no majority, so 3 is the proposed minimum.

### What is hidden, and from whom

Block hiding protects users from the other proposers. A key is only released inside an attestation, and by then its sender has seen everyone’s commitments, so no proposer can build its block around a rival’s contents.

Transaction hiding protects users from the proposers they send to. A user picks *n* proposers and a threshold *t*. With *t* = 1 the plain transaction goes to each of them and duplicates are removed in the merge. With *t* ≥ 2 the user encrypts the transaction, erasure-codes the ciphertext into *n* fragments, and splits the key into *n* secret shares, so any *t* fragments rebuild it and fewer reveal nothing. Validators execute it in the slot where the *t*-th fragment lands. Each fragment carries a public header signed by the fee payer (fee payer, compute limit, writable accounts, priority fee, target slot), enough to check validity and place the transaction. Because the public header shows which accounts it touches and what it bids, information is still leaked.

In this *t*-of-*n* setup, larger *n* buys censorship resistance, a higher *t* buys privacy, and *n*-of-*n* gives each proposer a veto. A receiver can hold back its fragment until the others are revealed, decrypt the transaction, and front-run it in a later block. If a slot is skipped after fragments are out, the transaction is exposed without having executed.

A plaintext transaction is still readable by its receiving proposer, which can trade ahead of it in the same slot only by outbidding its priority fee.

### Validators take over the leader’s job

With no leader, every validator assembles the slot itself. After removing duplicates, it sorts all transactions by priority fee per compute unit, then by smaller compute limit, then by hash. It then walks down that list, filling a virtual schedule of 4 parallel tracks (slots) until the CU limit is reached, skipping everything else.

Validator bandwidth usage does not increase with proposer count, because slot capacity limits it. But proposers pack blind, so duplicates and transactions that do not fit use up some extra I/O. Every included copy or fragment pays an inclusion fee to the proposer that carried it, on top of the usual base and priority fees, so redundancy has a price. The fee mechanism itself is left to future work.

### Hubs

A hub is a group of validators within Δlocal of each other, typically one city. Hubs are permissionless: anyone can register one onchain, and it becomes active once enough validators and stake have joined. Validators outside any hub only vote.

Every window goes to one active hub: half the windows are assigned in proportion to hub stake and half uniformly, so a big hub cannot dominate. Within the hub the 3 proposers are drawn by stake, with each member capped at a third so nobody holds two seats. Suggested starting hubs are New York, Frankfurt, Tokyo, and London. One operator could still run several validators in a hub; the stated deterrent is that each pays Alpenglow’s voting fee.

## Discussion and interpretations

This comparison is part of the reading summary. It is an editorial framing, not a table in the session transcript.

Ethereum’s proposer-builder separation (PBS) accepts MEV as inevitable and auctions it off. MCP tries to remove the monopoly that creates it.

| Approach | Who orders | User protection | Rests on |
| --- | --- | --- | --- |
| Ethereum PBS (MEV-Boost) | Winner of a per-slot builder auction | None against ordering MEV | Builder competition |
| Jito BAM | Node in a TEE, for the current leader | Private until execution | Hardware vendor, node operators |
| Harmonic | Leader picks the best of several competing builders’ blocks | Vetted searchers may back-run but not front-run or sandwich | Leader’s choice of builder; still one leader |
| New Constellation | Priority-fee merge by every validator | Block hiding, optional *t*-of-*n* transaction hiding, redundancy | One honest proposer per hub window |

## Limitations and disagreements

- **Censorship resistance over liveness.** Unanimous attestations mean no transaction can be excluded, but one malicious proposer can force a skip. The alternative is for the proposers to run one Byzantine fault tolerant voting round among themselves on which blocks to accept. More than two-thirds of them could then always produce a block, even if the rest are malicious, but that same two-thirds could also agree to leave out a block they want to censor.
- **Colocation risk.** Three proposers in one city make 50 ms slots possible, but for 800 ms all inclusion runs through three validators in one city, exposed together to a local outage or regulator. Concurrent hubs are left as future work.
- **Information leakage.** The header of the *t*-of-*n* fragments carries a lot of information about the transaction. It is easy to spot a profitable liquidation transaction based on the accounts it touches and the fee it pays.

## Open questions

- How the inclusion fee is charged is left to future work. The summary only says that every included copy or fragment pays the proposer that carried it, on top of the usual base and priority fees.
- Concurrent hubs, so that inclusion is not confined to one city for a whole window, are left as future work.
- Whether unanimous attestations should stay, given that one malicious proposer can force a skip, or whether proposers should instead vote among themselves on which blocks to accept.

## References and further reading

- Quentin Kniep and Roger Wattenhofer, New Constellation: Multiple Concurrent Proposers for Alpenglow, version 1.0, 14 November 2026. [Repository](https://github.com/wattenhoferSOL/SolanaMCP) and [PDF](https://github.com/wattenhoferSOL/SolanaMCP/blob/main/New%20Constellation%20v1.0.pdf).
- Alpenglow, by Kniep, Sliwinski, and Wattenhofer, is the consensus protocol underneath. The summary says Votor, its voting half, went live on mainnet on 21 October 2026, and Rotor has no release date yet.
- The first Constellation paper (March 2026) is a different design: proposers and attesters, synchronised clocks, and a leader that still assembled the block.

### People in the summary

- **Roger Wattenhofer.** Head of Research at Solana Foundation. Runs ETH Zurich’s Distributed Computing Group. Co-authored *Halting the Solana Blockchain with Epsilon Stake* (2024), on liveness weaknesses in TowerBFT, then co-designed Alpenglow and both Constellation papers.
- **Quentin Kniep.** Senior Researcher at Solana Foundation. PhD graduate from ETH Zurich. Co-authored the papers mentioned above.
