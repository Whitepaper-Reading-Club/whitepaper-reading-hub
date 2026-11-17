# Transcript — New Constellation

- **Session:** [New Constellation — 2026-11-17](README.md)
- **Language:** English
- **Transcript type:** Edited. Re-transcribed from the audio; filler words and false starts are removed, and the wording is otherwise kept as spoken.
- **Review status:** Checked against the recording, the Apple transcript, and the paper.
- **Public sharing permission:** Attendee names are omitted. Author labels are best-effort, as described below.

## Editorial notes

Cleaned transcript of the 60:37 recording, re-transcribed from the audio and checked against the Apple transcript and the paper. Filler words and false starts are removed; wording is otherwise kept as spoken.

**Speaker labels are best-effort.** The two authors are labelled Roger and Quentin. I assigned the strategy and market-structure answers to Roger and the protocol-detail answers to Quentin from what each says, not from a reliable voice match, so the split between them is sometimes a guess; a bare Author label means I could not tell which. Everyone else is labelled Participant, so consecutive Participant turns are usually different people. A and B mark two voices that recur often. Names of attendees mentioned in the room are left out.

**Marks:** \[?\] follows a word or phrase I am not sure of; \[inaudible\] marks speech I could not recover; times are minutes:seconds into the recording.

## Transcript

### 00:00 Bundles and priority-fee ordering

*The recording starts mid-question.*

**Participant** (00:00): …proposers, someone sends in a certain transaction, that doesn't guarantee that priority-fee ordering. And secondly, with BAM it doesn't seem that's possible, because there's not one validator guaranteeing a sequence within a block. Am I mistaken with this assumption?

**Participant** (00:19): You should ask… \[inaudible\]

**Participant A** (00:22): I mean, there's things. If we do in-protocol ordering before this multiple-proposer thing, then we can basically wait to observe the order flow, and then you could manufacture a bundle by having the bundle all pay the same priority fee, and then you can do a bundle that way. But in a multiple-proposer model, where we don't see the flow from the other proposers, then ultimately the merge \[?\] can break it up. But there are weird things we can do with, again, using unique priority fees, even in this multiple-proposer model, because it seems the flow's going to be encrypted. They're not going to share the flow between them, right? So, I don't know, if I use some random u64 number and have these three transactions all pay the same priority fee, then statistically they probably would be included next to each other. So that's one way we can think about it. But this proposal is going to force… not force, but people are going to just start playing different games. That's an example of what might happen.

**Participant** (01:30): So it seems like the proposers have to agree to all opt into Jito in order to guarantee that the merged version abides by the bundle \[?\] rules.

**Participant A** (01:41): Well, not necessarily, but that also is sort of an outcome, right? If they all decide to opt in to run bundles, then that would make it easier, then we could actually statically include a bundle that way. So… but, I mean, we're starting with…

**Roger** (01:58): \[inaudible\] …bundles, but that is the biggest question somehow, right? It is one of the biggest questions, so it's fair to start here.

**Participant B** (02:09): Also, how do you encode certain properties that we want, like maker priority? Now, if you can just have a high priority fee, you can upset the whole design of what we're trying to achieve with both Harmonic and BAM of giving makers priority \[?\]. So I think we probably need some specific adjustments to accommodate for something like that. I mean, unless, you know, Toly doesn't care. \[laughter\] That's a different story.

**Participant** (02:39): I wish to give an example. So basically, why do Jito bundles exist? Because the end user has such a need. One use case that's very easy to understand is the back-running case. How back-running happens is: some user submits transaction A, right? Some searchers see an opportunity, because transaction A will move the market from a fair price in the wrong direction. So they want to fix the market; that's why they back-run it. The way Jito bundles support back-running is: the searchers put the user-signed transaction as transaction one in the bundle, then attach their transaction to fix the price right after, and Jito guarantees the atomic execution. That's why the market can be fixed immediately, and also the searchers get a profit. But without Jito bundles, this cannot be done. The true reason is that within the bundle there are multiple signers. The original transaction is signed by the customer, and the rest are signed by the searcher. So if we allow the priority-fee re-sorting, typically we will see the searcher's transaction put in the front, because they put a high fee, and the original user one put last. Therefore, even if all of them are executed, the market on chain is still broken. So there's no effect for back-running.

**Participant** (04:17): \[inaudible\]

**Participant A** (04:21): Yeah, if anything, you'd actually see value leak out of the protocol in that way, because block builders… there are, again, statistical things we could do to pack the block to capture that back-run in this model. But we have to respect the priority fee for the merge rules, so then it's just going to be a system transfer from the searcher, completely out of the protocol. Whereas now I think we're seeing a really nice market structure, even in Harmonic blocks and in BAM blocks too, where the value's driven through priority fees, right? Jito tips are down only \[?\], and priority fees have just been up and to the right, and it's because we've internalized that in the protocol. So what you're describing is, you're going to likely see a separation back to just out-of-protocol MEV capture, which is something that we will have to consider for sure.

**Participant** (05:17): Yeah, so maybe the new approach can rethink, basically do some similar concept, like atomic execution \[?\] for several transactions. Now it purely depends on what the transaction's priority fee is.

**Participant B** (05:34): Also, correct me if I'm wrong, for multisig stuff, I believe they are users of bundles, where you want to have your signed transaction and then some other transaction to actually execute that.

**Participant** (05:47): Yeah, I think Squads is using it.

**Participant B** (05:49): Yeah, exactly.

**Participant** (05:51): Or in other cases, where people do this flash-loaning \[?\], so they take a loan from one \[place\] and then provide liquidity using that, but done using a bundle.

**Participant A** (06:00): So I'm just curious, like what \[name\] was saying, the atomic batch execution: is that something that Anza would potentially look into? Because it looks like every other client kind of does. Firedancer does it, \[inaudible\] as well supports bundles. It's like 98% of stake on Solana that has bundle support. So Harmonic does it, Jito does it, \[two names unclear\] do it. So just curious.

**Roger** (06:25): I cannot speak for Anza anymore. I'm not with them anymore.

**Participant** (06:28): You're not privy to any internal details anymore? \[laughter\] All right.

**Roger** (06:33): No, I mean, what I want to say, first of all, is that this whole ordering of transactions is something which is a bit vague in the paper still, actually. We do say we have this priority ordering. You could do it that way, you could also do all kinds of things. And for me, the main input from this meeting is also to hear what other things people want to have. And I got a bit of feedback already yesterday from people. So it could be some sort of application-based sorting as well. And I understand that in the room there's validators here, right, which get essentially money through this sort of block-building business, and then people building blocks in the room, which also make money this way. And of course this is a bit, I would say, yeah, it's a bit dicey, you know.

**Participant A** (07:23): I think even if you get the priority ordering right, unless you have that atomic execution in place, you cannot respect bundles, right?

**Roger** (07:35): Yeah. I mean, you kind of said a trick how it could be done, right? By having, let's say, almost the same priority fee or something, then you can basically order it that way. And that should actually work, I would think.

**Participant B** (07:47): That seems to have the issue, though, that there's the tiebreakers, right? There's the hash and the compute units, the smaller compute units. That could mess you up if you all have the same priority fee.

**Quentin** (07:58): No, but you could have an epsilon, you know. You can tiebreak yourself by just increasing it. You pick one random number and then increase by one, increase by one.

**Participant** (08:09): Yeah, there's also another thing, where bundles don't really fail. You won't see a bundle that failed; it just doesn't land. So you don't pay for it if it doesn't land. So in this case where, let's say, you have multiple batches: the first batch gets executed, the second one has the bundle, but then this batch being executed before the bundle kind of invalidates its state. Then in theory you shouldn't include it. You cannot fail on chain.

**Participant B** (08:35): But also the hack with the epsilon, it's not exactly great, because I think you still lose the all-or-nothing property of bundles. If you have a partial execution, then you're kind of in trouble.

**Author** (08:50): \[inaudible\] …of the epsilon, yeah.

**Participant B** (08:55): Because that is one of the properties that people do often want. Not always, but often.

**Participant** (09:01): Bundles only exist because people want to do more atomically, right? Would the problem totally be solved if transaction v2 is just 5x \[the size\]?

**Participant B** (09:11): It wouldn't \[?\] be solved. You have some organizational issues of who signs what. If you only have certain things like a pre-signed \[?\] transaction and you need to have your own signed transaction afterwards, you can't do that with a transaction v2 still. Or the same thing, the multisig thing I was talking about.

**Participant** (09:27): But some future transaction \[format\] could technically allow it \[?\]. Maybe. Okay.

**Quentin** (09:39): The way I would put it is: if we want an atomic unit of execution on chain, then that should be the transaction. Something that's appendable, but only atomically executable. I mean, if I wanted to support that, then I would call that the new transaction format \[?\].

**Participant A** (09:56): Okay, I guess. Yeah, like an atomic batch, like two transactions or two sets, and then we would make it atomic at the protocol level.

**Participant** (10:06): Yeah, that makes sense. And it would also be a much simpler design, and you could pick the thing that's actually best at the ordering level, versus trying to accommodate all these band-aid patches that were done to support some of the use cases, larger things, signing and all. And you kind of split this out from the proposal.

**Participant** (10:30): Would there be the situation where, if it's appendable only, that would mess things up? If you need to put something before? I don't know.

**Author** (10:38): You would need to think this through.

**Participant** (10:41): Yeah, I don't have \[a take\] on this.

**Participant** (10:43): But it seems like this thing belongs fundamentally into a transaction, into something that the protocol natively supports, versus bundles, which give you some pseudo-support of, like, 'trust me, bro, I'll include it atomically'.

**Participant** (11:03): My question is, why do you need bundles?

**Participant** (11:07): Well, I mean, we just talked about it. \[crosstalk\]

**Participant B** (11:17): I mean, it's a good question. We just said there are cases where, at least currently, there's stuff that transaction v1 does not support. If you have something that's both larger than 4K and you want to have some number of transactions that all-or-nothing execute, that is impossible on chain right now. So either it's larger than what's possible right now, or you have this kind of organizational issue: you have a signed transaction and you want something else to execute with that, all or nothing. You just can't do that. Like \[name\] said, and also with multisigs.

**Participant** (11:51): Yeah, another reason is that so far the priority fee is more like a very static thing. So the end user actually cannot calculate \[?\] that precisely. But with Jito bundles there's, let's say, dynamic tips, so people can calculate \[?\] very precisely.

**Participant B** (12:12): There's also, if you have something that's speculative. Let's just say you're trying to do an arb, right, and you don't know if it will execute or if it will revert, like if somebody else got it first. If you're using priority fees, that will always be burnt. So if you had a five-SOL priority fee and you're trying to capture an eight-SOL opportunity, and maybe you have a 10% chance of winning it, that is not a winning business strategy. Straight up. So you want the all-or-nothing property, where it just fails to execute and you don't pay anything.

**Participant** (12:48): What about the spam that incentivizes?

**Participant B** (12:52): Yeah, for sure, it incentivizes spam. So they also work as an inclusion fee. Our solution right now is doing simulations. We simulate it locally, and we send a curated set of transactions that have been sorted to be highly likely to land. Through the block engine… or through BAM, we simulate much more tightly with what the validator does, in lockstep. When you're just using the block engine, you're essentially kind of racing transactions and bundles at the same time. Whereas with BAM there's a single stream of transactions to execute, and a very high percentage chance it will land as it was streamed out. But that's a different topic.

**Participant** (13:40): Yeah, I wanted to change the topic too.

### 13:40 Gossip vs on-chain, denial of service, duplicates and inclusion fees

**Participant** (13:44): So let's come back to… Also, if there's anyone who hasn't asked a question.

**Participant** (13:47): Oh, I have one. So, reading the actual white paper now: joining a hub requires you to make an on-chain transaction \[?\]. Is there a specific reason why that's the design, or is that kind of up in the air, like maybe it could be in gossip? My intuition was that it would be a gossip thing, since the way that validators coordinate with each other, as far as networking goes and stake goes, is gossip, right? So I'm sure there's a reason…

**Roger** (14:14): This is much easier to answer for me, I think, because it doesn't go into the market structure. So, I personally want to abolish gossip completely.

**Participant** (14:21): Oh!

**Roger** (14:22): It's a bullshit protocol. \[laughter\] So we have essentially a blockchain, right? And the blockchain is the source of truth of what happened, right? So why would we not use the blockchain for these things? That's the honest answer.

**Quentin** (14:35): Especially for things that relate to stake registration and membership and so on. And the leader schedule, essentially, right? Everyone has to calculate that to follow the blockchain.

**Participant** (14:49): Okay. So with that framing in mind, do you see a time when votes would get back into the transaction \[stream\]?

**Roger** (14:59): No.

**Participant** (15:01): Because votes are in gossip right now…?

**Roger** (15:03): For votes we have certificates. No, they don't do this anymore. So now we have certificates for these things. These go on chain \[?\], basically, and that gives you proof enough. So, yeah. As I think really the main topic is this kind of market structure: I'm not sure I would agree completely with this idea that you need to bundle things also, right? That was also mentioned by others. But I'm open for, you know, \[inaudible\]. But for this, I would say this goes on chain, and that's fine.

**Participant** (15:35): I had a quick question about denial of service for proposers, because Alpenglow has pretty well thought out \[?\] mitigations, such as the ParentReady window and so on, so if you're behind you can quickly catch up. So I'm kind of curious what proposers have to mitigate this denial-of-service vector. Since there's no global synchronicity, which obviously would be very hard to do, right, I'm kind of curious how they catch up, since it could be this large point of failure, right? Where if you can DoS \[?\]…

**Roger** (16:07): So you would denial-of-service some proposer so that they cannot send their proposal out, or?

**Participant** (16:13): Yeah, essentially all three proposers, potentially.

**Roger** (16:16): Sure. I would say as a proposer you don't need to receive much, right? The only thing you really need to receive is this hash from the other two proposers. At the point where you're proposing… okay, maybe you need to receive transactions, I guess, but this could be a different address. But usually I would distinguish between sending things out and receiving things. And I think the proposer is mostly a sender and less a receiver, so I'm a bit less worried about denial of service, actually. Because the only thing I really have to receive as a proposer is this hash from the other proposers to make the attestation, and maybe that you can filter out more easily \[?\].

**Quentin** (16:59): We know exactly who we want to receive these particular messages from: just the other proposers \[?\].

**Participant** (17:06): All right. Question?

**Participant B** (17:07): Yeah. So what is the incentive for users to not send to all n proposers every single time? Because I think that puts more load on the network. I mean, it's not exactly a magnification attack, but you're making more CPUs do work.

**Roger** (17:31): Ah, true. It could be what we discussed before, right? Let's say the three proposers have different building engines in the back end, and depending on your application you should send…

**Participant B** (17:38): But it doesn't cost more to send to all three, right? You just have to…

**Quentin** (17:42): It does. It does, yeah. You have to pay the inclusion fee for the proposer to include you. So if all three proposers see your transaction and include it, then you would need to pay the inclusion part, which is basically paying for the block space, right? This part you would pay to each of the proposers.

**Roger** (18:04): So for simple payments I wouldn't do it at all, right? Just send to one. If it doesn't work, send it again later on or something.

**Quentin** (18:11): If you don't need the censorship resistance as the user, I would just always send to one proposer and \[inaudible\], right?

**Participant** (18:21): So I have a question. If they send the same transaction to three proposers, why do they need to pay multiple inclusion fees? That's the same transaction, same signature.

**Roger** (18:33): Because it goes on chain, right?

**Quentin** (18:35): Because it wastes block space. Each proposer only has some limited capacity for including transactions, right?

**Participant** (18:44): Yeah, but the three proposers will forward to the leader, say, hey…

**Author** (18:48): There's no leader.

**Participant B** (18:50): So on your block explorer, you'll see the first one land and you'll see two failures after that? Is that what you'd see?

**Roger** (18:56): No, you would see all three transactions in the block, I would say. It's just the first one will get executed; the other ones will not be executed, because of this reason or something.

**Quentin** (19:07): And the explorer should probably show this as a special case: show that this transaction was included by all these proposers.

**Participant** (19:15): How is it now if you send the exact same transaction? Then of course the builder would just remove the dupe?

**Participant** (19:19): It would fail pre-flight. It would be a pre-flight failure. It'd be the exact same thing.

**Participant** (19:23): So this is actually a change of the Solana semantics for a transaction. Because previously, with the same signature… and now it appears twice in the blocks. Now we allow it to appear multiple times. It's changing the semantics.

**Participant** (19:40): And I think it's in sigverify that there's the deduping of the signatures. So how do you do that when three different proposers are doing sigverify, each independently unaware that they're each getting the same transaction? They can't dedup. They can only dedup if they receive the same signature twice on their end.

**Author** (20:03): Yeah, we have to do it during execution.

**Participant** (20:06): Ah, okay. So there needs to be another dedup.

**Roger** (20:09): I should say maybe for this: at least right now, block space is maybe not the main bottleneck that we have, somehow. So that's why you sort of don't care so much about this with so few proposers. If you had something like 20 proposers, then you have 20x of this, and maybe it starts to go wrong. But I think the main bottleneck we usually think about is execution, and you would only execute once, right?

**Participant** (20:31): I'm not sure, but in that case someone can copy this exact transaction and land it in the same block. Oh, sorry, in the next block. And then the same fee payer would pay for this next transaction that they didn't submit, because it's just a copy of something that landed on chain \[?\]. With these new rules of dedup \[?\], you can actually land it too.

**Roger** (20:52): Good point. I mean, it depends on… maybe you can say in which block it should land or something.

**Quentin** (21:06): Yeah, and not just that. You initially would probably indicate in the transaction which proposers you intend to send it to. So there would be proposer one, proposer two, proposer three, right? And the transaction, for example, would say either all three proposers can include it, or I intend to send it to proposer one.

**Participant** (21:26): And you cannot do it in a later slot, right? You can only do it in the same slot and send it to other proposers. Otherwise the status cache \[?\] would deal with it. It would also be part of replay. You cannot have something from a later slot in this model.

**Roger** (21:43): I would agree that you can solve this somehow, but maybe it doesn't make sense \[inaudible\].

### 21:49 Skipping, slashing and how hubs are formed

**Participant** (21:49): One other concern, sorry: now there could be an incentive for a proposer to misbehave and have the slots skipped, right? So at some point, within the range of the recent blockhash being valid, I control all three proposers, in the worst case. And until then, I control one proposer. So there's some super juicy MEV opportunity that I want to sandwich, whatever. I skip, skip, skip, skip, and then do my MEV.

**Roger** (22:24): Yeah. That can happen. That would already happen nowadays, I would say, with a single leader somehow, right? It's not different from that. I mean, then you really control already everything.

**Participant** (22:36): With the extra impact that the chain is not making \[progress\]. Everything else gets skipped, and the chain is not including anything.

**Roger** (22:46): Yeah, this is true. There are many of these cases, right? Even the case that just somebody right now controls all three proposers, right? And they basically control the chain. Then they can do MEV, right? But now people are incentivized to skip…

**Participant** (23:00): To skip first, and then… Right now it really doesn't exist, because when you're a leader you really want to produce your block in \[inaudible\] cases to get the rewards. But now you could maybe get more rewards later, especially if blocks aren't very full.

**Participant** (23:17): Could there be a penalty for skipping?

**Roger** (23:19): I mean, there should maybe be a little bit of a penalty \[?\], but…

**Participant** (23:27): That's also commensurate with what's the value that you extract, right? That's your economic security, just based on that.

**Roger** (23:34): I mean, we know who is responsible for skipping in some sense, right? We have this attestation, so we know who is the person who didn't deliver in time, for instance.

**Participant** (23:44): Or delivered different hashes.

**Roger** (23:46): Or this. It is a bit tricky, right? In Solana we haven't done it so far, really. There's many things you can do even right now and there's no real punishment. So here it gets a bit worse even, because, let's say, if you're in a small hub with only a few other potential co-proposers, and there's this one evil guy who always is with you, essentially, then you don't want to be with this guy anymore somehow. So we have to maybe dance around this a little bit.

**Participant** (24:19): Do you want to create a blacklist? Like, other validators you \[don't\] want to associate with?

**Roger** (24:24): Yeah. But, you know, this will be new territory for Solana, right? Because right now not even a leader who proposes two different blocks for the same slot… this is in some sense the worst possible thing you can do right now, and there's no punishment for that, right? So it can be done. It was not done in the past because \[inaudible\] the protocol works even so, okay? So maybe, you know, now with Alpenglow \[?\] we can start doing this, but…

**Participant B** (24:55): And then also, curious: the 16 slots that you're proposing for, right? That is now a much longer monopoly if your users only send to you. If they are, let's just say, frugal, they don't want to spend for more inclusion fees, then you can play some more of these lagging games that \[name\] was talking about, right? You skip for a while, you store them up, you hope for more divergence of real-world versus on-chain prices, and then you land them later.

**Quentin** (25:34): The time frame is still the same, right? Because the slot time reduces to a fourth \[?\].

**Participant B** (25:38): Oh, okay, okay.

**Quentin** (25:40): So overall it's the same \[?\].

**Participant B** (25:43): Yeah, okay, never mind. I thought it was still 200 milliseconds, so 16 times 200.

**Participant** (25:48): There are also plans to shorten the leader window anyway.

**Participant** (25:53): Like, the leader rotation?

**Participant** (25:55): No, the leader window. Every leader gets four slots at the moment.

**Participant** (25:59): Yeah, from four to two, right?

**Participant** (26:00): They want to do four to two. But because of the leader handoff issue, they didn't advance on that.

**Participant** (26:04): I think it will come after this… after geo-aware \[inaudible\].

**Participant** (26:09): Yeah, geo-aware \[inaudible\] and all that.

**Participant** (26:11): Any other questions?

**Participant** (26:14): Just to confirm, so what we just said is we actually allow slashing for a late proposer? Wouldn't we have this trade-off of collusion between two proposers and a late \[one\]… \[inaudible\]

**Roger** (26:27): The honest answer is we don't quite know, right? We don't do slashing at all right now. And to introduce it for this, we'd better introduce it already now, I would say, right? Take the worst offences that we can prove, and do slashing right now of some kind, and then maybe we can carry it over. But I would be against: now this Constellation starts, and then we also need to do slashing. That's risky.

**Participant** (26:53): But it could also be a weaker form of slashing, right? Because it's not strictly provable. It's not like a cryptographic proof \[?\].

**Roger** (27:00): Yeah, I fully, fully agree. I think it's very risky.

**Participant** (27:04): Can you share a bit more?

**Author** (27:06): We have pieces of evidence that there is… \[inaudible\]

**Participant** (27:09): On the proposer schedule, what's the thinking here? Because you have these latency dependencies, right? So are you thinking of a fixed set of proposers in each location, and we just move between a set of locations? Is that the thinking, or…?

**Roger** (27:28): What do you mean with a fixed set?

**Participant** (27:30): Like, who defines what is considered a set of proposers for a location? You have this strict latency…

**Roger** (27:39): You can choose it. So as a validator, you choose which region you want to go in. The proposers, or the validators, have to organize into these hubs \[?\]. They have to basically sign the pledge that they are within this latency bound and that they can fulfil their proposer \[role\].

**Participant** (27:58): Okay, okay, so you self-select.

**Roger** (28:00): And if they can't, then they will not get any… \[inaudible\]

**Participant** (28:04): And how do you then respect the cap of three? You have a cap of three per location, right? So you have exactly three?

**Roger** (28:15): No, no, no. The selection is just a random selection by stake \[?\], three every time.

**Participant** (28:22): Okay. Yeah.

**Participant A** (28:24): Okay. So then how do you account for a malicious co-proposer? Let's say you have a roster of them that are in a certain region, and say one of them keeps skipping. How do you reorganize around that?

**Roger** (28:38): As I said before, we haven't thought about it much. I mean, okay, if you want a naive fast answer, I would say: if I'm in a region where there is a bully, okay, then maybe I move out of the region. That would be my silly answer to this. But I think it's kind of self-organizing a little bit.

### 29:02 Malicious proposers, detectability and hub stake

**Participant A** (29:06): Yeah, I was just going to ask a question. If you had a malicious proposer that observed a transaction it wants to front-run or sandwich, could they still submit front-runs to the other proposers? Because they know the ordering downstream. So they could set priority fees ahead of the one they've just observed.

**Roger** (29:29): So, to understand: a proposer gets a transaction, wants to be malicious about it, and sends something to the other proposers to attack it somehow?

**Participant A** (29:37): Yeah. Because you do have some kind of privileged information, right?

**Roger** (29:44): But why would you send something to the other proposers?

**Participant A** (29:47): To have plausible deniability.

**Roger** (29:49): You could do it yourself… \[inaudible\]

**Participant A** (29:53): But plausible deniability is something, right? You don't want to sign with your validator's pubkey or whatever.

**Roger** (29:59): Well, I could also say it's from a different user.

**Participant A** (30:03): Exactly, yeah.

**Roger** (30:04): I would say it's not much different from now. Many things are at least not worse than now \[?\].

**Participant B** (30:10): I would say it's worse than now, because now, if you try and do that, you can always see it was you who did it. Whereas with this new design, you can kind of smurf it, or you can hide behind somebody else. You can be much more sinister about it. Or if you were just holding stuff back… I don't know. I think it's much easier to proxy now, for sure. Because you have parallel streams. Before, there's just a single shred \[?\] stream. There's nobody else to blame.

**Roger** (30:41): Well, sure, I can also say naively: I just included whatever this user sent me, and this user is doing this transaction. What can I do? I'm just a leader.

**Participant B** (30:51): But right now people can always see it's you. There's your stream. What I'm trying to say is, if you look at what sandwiched.me does, they're trying to see, oh, what percentage of sandwiches come on Galaxy? What percentage of sandwiches come on Jupiter, on Forward Industries, on blah, blah, blah. They have these big charts. It's really obvious what slots have sandwiches, associated with what leader.

**Author** (31:21): \[inaudible\] …they still show up.

**Participant B** (31:24): But you can hide with others. You can hide amongst three. So it's not so obvious.

**Quentin** (31:30): But you would still see the victim was included here and he was exploited, and then the next time you're a proposer with some other co-proposers, the same happens.

**Participant B** (31:38): But it's a longer chain, that's what I'm saying.

**Participant** (31:41): You can still correlate: whenever I submit to this guy, I get front-run. And then you can run experiments, like A/B testing with the other two proposers.

**Participant B** (31:48): It's going to take longer to detect, is what I'm saying. It's more complex.

**Participant** (31:51): Yeah, so for me, I'm not sure I completely understand everything, but it seems like there will still always be this economic incentive for proposers to coordinate, because they can always extract more value and insert their transactions. And I feel that as long as it exists, they will just do that, and the MEV players will just adapt to the game.

**Roger** (32:14): This is true. I agree. This is similar to now, right? Now many validators run the same type of software, which all sits in the room, right? And that's sort of coordination, I think.

**Participant B** (32:28): One question now: is there a new form of uncle attack \[?\] because you have these parallel streams? If you have three proposers, one gets skipped, and the others try and snatch those up and do stuff with them? Because they're still leader, they have the power to include it.

**Author** (32:48): No… \[inaudible\]

**Participant B** (32:51): Oh, everybody skips?

**Author** (32:53): Yeah.

**Participant** (32:54): Okay, we have another question there.

**Participant** (32:59): Yeah, so I have a question around the hubs. You say it's permissionless, so anyone can create a hub. And then let's say I have 1% of the network staked. I can actually create my own hub and split that across a few validators \[inaudible: '200 to 300K', '33'\], and then I'm actually able to do the whole thing like we do now.

**Roger** (33:22): Yes. Very much so. If you want to do that, you can do that. We have no check for that. That's also why we don't want the hubs to be too small, right?

**Participant** (33:34): I can create my own…

**Roger** (33:36): Well, if you have 10% stake, then maybe you can do it.

**Quentin** (33:39): Yeah, there's a minimum size: a minimum number of validators, and a minimum stake requirement. And we will probably start with pretty low numbers for these, but we could ramp them up over time, I think, so that there are large hubs which each have a significant amount of stake, and it's not easy to completely control, or even have a disproportionately large stake within, a single hub.

**Roger** (34:08): But you could still do what he said somehow, right? You can say, well, turns out we're both from Frankfurt and we are in this hub now… \[inaudible\]

**Participant B** (34:17): If you're Helius with 13% of stake, you can probably go do this for a long time.

**Participant** (34:23): One of you guys, did you want to comment?

**Participant** (34:25): Just a quick one. There should probably be some adjustment of the VAT to maximally discourage people from splitting their stake between multiple validators, or multiple proposers, right? Whatever right now is the extra money you can gain from splitting your stake into multiple things, that should just be paid by the VAT.

**Roger** (34:49): So you would have to pay the VAT if you split up in multiple roles?

**Participant** (34:52): Yes, but you would probably balance this so that there's as little gain as possible.

**Participant** (34:59): \[Partly inaudible exchange, apparently on whether the VAT is too cheap right now compared with the value that can be extracted.\]

**Participant** (35:07): \[inaudible\]

### 35:16 What MCP is for: regional inclusion, hiding and MEV

**Participant A** (35:16): All right. When I think about trying to steelman MCP and the motivations for it, the biggest advantage that MCP can offer, that nothing else offers, is regional inclusion. So you have proposers… it's actually the opposite of co-location, right? You're purposely putting proposers around the globe. And that's the vision that Toly has been shilling for years. When he talks about MCP, he talks about regional inclusion. So with this proposal, and it also seems like we've come to consensus in this room that this protocol is not going to solve MEV. So if it's not solving MEV, and it's not offering what MCP can offer that a single leader can't, which is regional inclusion, then what we're optimizing here is, we're basically optimizing CR, censorship resistance. So this is the CR proposal, right? And it's like, do we have a censorship problem right now on chain? And if we do, then we can very cheaply improve CR by just squashing slot times, right? As you compress the slot times, you also are maximizing CR. So I guess I'm kind of curious: if it's not the regional inclusion thing, it's not the MEV thing, is it CR? Is that the motivation? And do we have a problem with it right now?

**Quentin** (36:43): I mean, I would agree that censorship resistance was basically the main property that we wanted from this proposal. But on the regional inclusion point I would actually disagree. I would say that this proposal is the first step in exactly this direction. If we wanted to have this in the future, then the hubs would be exactly the thing that we need to achieve regional inclusion guarantees. Because if we have hubs where a large amount of stake is represented, then you can get a fast inclusion guarantee that actually means something.

**Roger** (37:24): I mean, it's even written in the paper, actually. There's this one section about future things we would like to do.

**Participant A** (37:30): Yeah. So the other side, the straw man for the regional inclusion thing, and this proposal has the same issue, is that, sure, you can get fast acks from the proposer, or maybe the proposer's not going to leak that information for other reasons, but ultimately, look, I know I'm going to be included in this slot, but I don't know what my execution quality is until the end of the slot, right? So you have these 50-millisecond ticks that each proposer's running, so you might be included there. It's kind of a 'trust me bro' situation where they're going to include you in that tick, or whatever. So that's fine. But ultimately, as someone who's subscribing to shreds and building my own version of the bank and trying to update my models and my strat, I'm not going to know about the execution quality or what happened until the slot's over. Whereas right now we're approaching this HFT scenario. I mean, again, I'm shilling Harmonic here, but that's what their whole thing is, right? It's this whole HFT thing: you have a single leader and you get this continuous block building. And so as a subscriber, if I'm a trader, if I'm running a strat, I'm almost getting, HFT-style, not just the acknowledgement, the ack that I was included, but actual understanding of what the current bank is, right? So with regional inclusion, you get these Singapore versus London versus New York markets, and that is super novel. But what you are giving up with that model is the understanding of the execution quality. And then in the short term here, with this proposal, you're also giving that up, right? Because you can't leak the information, because then the model breaks. Is that right?

**Quentin** (39:26): Yeah, I mean, it depends mostly on the ordering and the hiding guarantees. Whether you can see the execution state basically live is not really related to the local inclusion, and it's not really related to the multiple proposers. It's the delayed release for the hiding, and it would potentially be a delay for the ordering, depending on the ordering rule, right?

**Participant A** (39:59): So is there a version of this that works without the hiding property, or is this sort of all or nothing \[?\]?

**Quentin** (40:05): I mean, you can just remove the hiding property if you don't want the hiding property. You still get all the censorship resistance.

**Participant A** (40:13): I kind of understood the hiding property when I was trying to grok the paper \[inaudible\], but I kind of forget now. What is the hiding property? Why is it that each proposer has this delayed release?

**Quentin** (40:29): I mean, the hiding property that we achieve is maybe slightly weaker than in some of the most extreme previous proposals, where we had this threshold encryption and many rounds. There we sometimes had this property that until consensus finality everything is hidden. And in this new design we have a slightly weaker property, where as long as this current block is being built by the proposers, the contents are hidden, and only once all of the proposers are locked in is the block basically revealed.

**Participant A** (41:05): What is the motivation for it? Why would I… because I don't want to leak that to the other proposers?

**Roger** (41:12): It comes for free. That's maybe the motivation.

**Quentin** (41:17): Yeah. I mean, one thing is, the ordering and the hiding: if we have one, the other one is basically for free, because they both need the delay.

**Roger** (41:29): I also would like to push back a little bit on your premise that MEV is not solved with this. Some part of it is solved with it. \[inaudible\]

**Participant** (41:39): Censorship is a form of MEV, right? You exclude theirs and include yours.

**Participant A** (41:43): Sure, yeah. It seems that, yeah, you can clean up parts of MEV.

**Participant** (41:52): Isn't a large part of MEV currently happening… if you look at what's happening on chain, that's one thing, but I think there are intermediaries in the transaction submission, even before a single transaction would reach a leader or a proposer. So the whole topic of order-flow monetization: you have several intermediaries that see the order flow.

**Roger** (42:20): That's the elephant in the room, essentially.

**Participant** (42:22): Yeah, it is, right? And the only thing that would really, truly solve it, in my naive understanding, is if, from the moment a user signs a transaction, the contents are hidden until inclusion and ordering are guaranteed. Then it would be solved. But I don't know if that's realistically solvable. It seems like this would be a scenario where you could say it's truly solved in a way that you can't mess with it. \[crosstalk\] Even if we get MCP perfectly right, currently what's happening is that there are RPC providers and other intermediaries that see the order flow. They can delay certain transactions, and if they know with MCP that there's deterministic ordering, they can also know exactly how they need to price things to get it in that order. So it kind of seems like this is, as you said, an elephant in the room that should be…

**Roger** (43:25): It is a bit like that, right? I mean, some people do make money, as you say, in the current system, and one could argue all validators make money, actually, as it is currently, right? Because they kind of take a tax for these things. How to get them to vote for a proposal in the end which does not have this income stream is a big question, right? So you would have to find a way to make it profitable \[?\]…

**Participant** (43:56): Yeah, for example, from a validator perspective, I think even if you say 'I want to be maximally honest and pro-user', it's really hard to do sometimes, because even before you see a transaction there are a bunch of intermediaries that look at the flow and decide whatever to do with it. \[inaudible\]

**Participant A** (44:17): The more I hear this now, the more I think that bundles are possible. I mean, okay, it basically turns into an order-flow game, right? Like an RPC game: I have the apps send the transactions to me, and then I send it to exactly one proposer with, again, some sort of random u64 priority fee, and then it gets included atomically. So we move the whole game just upstream, and then all the value gets captured out of the protocol. Because ultimately you want to show that opportunity to multiple searchers and have them bid for it, but if they can't bid for it via an in-protocol priority fee, because that's going to mess the ordering up \[?\], then they're just going to pay me for it out of protocol. And then we'll just send the validator pennies, and, you know…

**Participant** (45:04): Anyone else?

### 45:08 Proposer schedule, k, the last-revealer attack and why reduce MEV

**Participant** (45:08): I would like to understand how the schedule works: how the validators and proposers are selected or assigned. Maybe that's somehow said here, but I haven't followed. Who defines who is the proposer, and how does the schedule run with that?

**Roger** (45:30): Yeah, it's not well described in the paper, I would say, but generally you have these hubs in the end, and basically by hub stake you choose how often it's a hub's time to do something like this. And then within a hub, you choose the proposers with the same mechanism, right? Let's say if you have 10% of that hub, then with three proposers you get 30% of the windows of that hub, basically.

**Participant** (45:58): So proposers will be assigned, kind of, into the protocol, that they are offering their proposer functionality?

**Roger** (46:07): Yeah, they have to register for a hub. That would be the idea. You go on chain, you say, 'I'm with this hub', and then in the leader schedule, or in the proposer schedule, later on you would just be included.

**Participant** (46:18): So then there is a proposer schedule and a schedule for the validators?

**Roger** (46:22): Ah, the validators are always there.

**Participant** (46:25): Okay, I see.

**Roger** (46:26): And you could imagine that you could split the roles a little bit more as well. I mean, already now we think of validators maybe having multiple proposer roles in different areas, something like this. So that's a bit \[inaudible\].

**Participant** (46:45): So what I like about this proposal is that… previous versions of MCP did add the CR property, but they kind of increased latency, or just kept the same latency, kept the slot time. But now with this proposal, you do have CR but you also have the slot time reduction. It's now 50 milliseconds. But the number k, right, determines how many proposers can propose at a given time. As you increase k, you do increase the \[inaudible\]. It becomes a lot chattier. So there's a certain k beyond which the 50-millisecond slot time will not hold.

**Quentin** (47:33): I don't think k is the limiting factor. I think the limiting factor for the 50 milliseconds is mostly how small do we make the hubs, how close the validators are \[?\]. Because they have to communicate, so we have at least one hop within the hub, and that… it's separate from the 50 milliseconds, but it's somehow the same time \[scale\]… \[inaudible\]

**Roger** (48:02): And with this proposal, k must be small. It has to be small. We're not going to do seven or eight. Two, three or four, I think those are the only reasonable values \[?\].

**Participant** (48:15): But that's not my question. My question is: is it going to be constant per proposer window, or is it going to be floating?

**Author** (48:21): k?

**Participant** (48:22): k. Will it be three in one window and then five in the next window, or…?

**Roger** (48:26): It's always the same. Because if you would change it… similarly for how big the hub sizes are, right? You might think some hubs might be bigger than other hubs, because, let's say, in Frankfurt you have a lot of stake, you can make a very small hub. But then you would rather be in a bigger hub somewhere, and then you would start to lie about where you are and whatnot. So these have to be constants, all of them.

**Participant B** (48:50): Question. So there's the attestation phase where, let's just say you have A, B and C, A and B share their keys for decrypting, and C can be a malicious validator, where they can inspect the transactions from A and B at that point and choose whether or not they want to release their key. So if you want to delay a liquidation, or if you want to kind of pause time in the world for some reason, you have that power.

**Quentin** (49:31): Yeah, that's basically exactly the thing that \[name\] was trying to prevent with the heavy threshold-encryption machinery. If you delay the reveal until after consensus finality, that's basically the only way to prevent exactly this.

**Participant B** (49:51): So we're accepting this as an attack, or we want to design something to fix it?

**Roger** (49:56): I think you now made the assumption all three of them are malicious somehow.

**Participant B** (49:59): Well, I'm just making the assumption that C is. But any one of them… the last relayer \[or 'revealer'\] has the most information, right?

**Roger** (50:05): But how does C see the things of A and B?

**Participant B** (50:08): I thought they were all communicating at the end, were they not?

**Roger** (50:11): Yeah, but only a hash, right? It's not that they can see anything. I mean, A and B would have to send the stuff to C for C to see it. \[inaudible\] …then A and C are both malicious at that point \[?\].

**Participant B** (50:24): Or maybe you don't need to see the transactions. If you're just inspecting real-world events and you just want to delay, you don't have to see the contents.

**Roger** (50:36): Sure, but that's the idea of this local hub: they have to be very fast in terms of the attestation. \[inaudible\]

**Participant** (50:50): I'm a bit confused, but from my understanding MCP pushes \[?\] MEV somewhat down from validators to RPCs. What I want to ask is: why is it so important to reduce MEV? Because from my understanding it hurts retail users the most. However, it's quite demand-inelastic, because each trade in the end is a small difference in price, whilst validators and RPCs make the most profit out of it. So if you minimize MEV, it could disincentivize validators to operate, which means that it can hurt the ecosystem.

**Roger** (51:39): Again, this is the big topic somehow; we came at it from a different direction. In my opinion the best answer is something like this: we want Solana to win. And it wins if the market of what we have on chain is much, much bigger than it is now, right? So that's the trophy here. If we can manage to have ten times more valuable transactions than we have now, then it's better for everybody, right? Then everybody makes more profit. And if we cannot solve this MEV problem, then maybe somebody else will solve it, and all the traffic will go there, and then we are dead, okay? So that's my honest opinion about this elephant-in-the-room problem, so to speak. But still, I want to keep the current community, right? It's not like all the validators have to go. That makes no sense. So we want, of course, to somehow get there without destroying the ecosystem as it is right now. But good question.

### 52:45 Random shuffling, transaction privacy and performance

**Participant** (52:45): So what's the biggest argument against random shuffling? Like, a cutoff at the lowest priority fee and then just random shuffling. That would solve so many of the MEV sandwiching problems, right?

**Roger** (52:58): So, just random shuffling of the transactions, without priority fee, so to speak?

**Participant** (53:04): Yeah. Everybody pays the same, but it's kind of determined by the market.

**Roger** (53:08): I mean, you could then have some sort of auction \[?\], but that seems too complicated, right? \[inaudible\] I mean, there are people who do mind, they don't like this.

**Participant B** (53:18): But can you have the situation where, okay, you know that the first n transactions have some fixed number of CUs, and then you just choose that much to include, and then you shuffle the ordering from there? Yes, obviously there will be something that will revert, and you didn't fully fill the block. But that way people can't play that many front-running games, if it's randomized.

**Participant** (53:45): But then the incentive to pay the fees disappears, which is very bad for validators.

**Participant B** (53:50): Not totally, right? You still have a cutoff. You still want to remain above that cutoff.

**Author** (53:55): Yeah, the inclusion fee would still exist, and the priority fee would go away, I think.

**Roger** (54:01): I mean, there are chains who are doing funny stuff, right? They say they want to have much, much less throughput, much fewer transactions per time unit than they have right now, so that the price goes up to get in, and stuff like that. We can always do that, but then we won't win, right? We want all credit card transactions on chain, right? And people don't want to pay more than they pay for Visa.

**Participant** (54:24): Is your argument that we should just shuffle all the transactions \[inaudible\], regardless of priority fee?

**Participant** (54:33): Yeah, kind of. Instead of the priority fee, have a cheap, order-independent, pseudo-random thing. Doesn't need to be super good, but yeah.

**Participant** (54:42): Because priority fees are not the only piece of information in scheduling. You also have your CUs. They want to make the runtime fast \[?\], so they have to… \[inaudible\]

**Participant** (54:54): Just a point on this: if you just randomly shuffle, then the incentive for you to spam the network will be way higher, because you just spam it and then maybe some land in front, some land in the back, and then you sandwich or whatever. You'd just stress the network way more. Also, from a trading point of view, you want very predictable rules for landing a transaction. Usually you try to minimize latency jitter. You want to really have a reasonable explanation for why you landed here, not there. Which is why either first-come-first-served has been used, or the priority fee thing. But if you're telling me your block execution is random, there's no incentive for me as a trading firm to even try to land there, because I'm kind of wasting my time. It's like a gambling game. There's no way to explain deterministically, am I going to land here or there?

**Roger** (55:47): No, you'd put a lot of spam in, that's what he says, right? You put hundreds of transactions in, and then the transactions just check whether they are profitable, and if not, they don't execute. Maybe spam is good for us, then. \[laughter\]

**Participant** (55:59): But isn't that just wasting block space?

**Author** (56:04): Yeah. It would drive up the inclusion fee… \[inaudible\]

**Participant** (56:08): Is it possible to achieve transaction privacy at the start of the transaction lifecycle?

**Roger** (56:17): Yeah, this is in here.

**Quentin** (56:20): Yeah, I mean, we propose this scheme where you would basically Shamir-secret-share the transaction in a way where you need some threshold of proposers to include it, and no single proposer can inspect the transaction. And you could do that on the user side.

**Participant** (56:47): Would that solve the monetization problem \[?\]?

**Author** (56:51): For the most part, probably \[?\].

**Participant** (56:54): Would this be optional?

**Quentin \[?\]** (56:56): So basically, I think in the current proposal there are some issues, in that the header of that secret share reveals… what does it reveal? The accounts it writes to and the priority fee. So if it's a liquidation, for instance, you can tell without seeing the transaction. This is definitely bad. But I think that can probably be fixed.

**Participant** (57:18): And then it includes the inclusion price, but not the accounts?

**Quentin** (57:21): No, it has to include the accounts for the scheduling, because otherwise you cannot…

**Participant** (57:26): You need the accounts because, remember, \[inaudible\] they run it in parallel.

**Roger** (57:33): No, but I mean, if we have the reveal… \[crosstalk\] Ah, no, true. No, I don't think you need to reveal the accounts. You need to reveal the money that's in it for you \[?\], which is already revealing a bit. But if it doesn't reveal the accounts, then it would be fine.

**Participant** (57:53): Yeah. And then if you require n-of-n, like you said, t equal to n, then you achieve that. The only problem is if a proposer goes down, you don't get included. They can basically force a skip of the block, to see everyone else's share of the secret, and then reconstruct it later. But then if you slash them, you also solve that. So I think there's no incentive to censor that way.

**Author** (58:22): You solved it. Thanks. \[laughter\]

**Participant** (58:26): Which is an incredible achievement. I think this protocol is the first one that really has some really, really interesting properties. I think it's quite amazing.

**Roger** (58:38): If we can get it through a governance vote, I think it should be good.

**Participant B** (58:42): Is there any expected performance improvement, at least in theoretical TPS, if there was no CU limit and other bullshit?

**Roger** (58:52): So we need again something like a CU limit here, probably. Actually, they're thinking about removing the CU limit for Alpenglow completely, right? And for Alpenglow this perfectly works, because the leader has an incentive to make sure the block is executable. But if you have three proposers, they still have an incentive, but each still wants to be the guy who pushes most transactions. So you have to think about that. And it's very hard.

**Quentin** (59:19): They have an incentive that the block isn't skipped, but no one has the information to ensure that.

**Participant** (59:24): Yeah, they can't coordinate directly.

**Participant B** (59:28): So you're essentially now doing some of the verification work in parallel. But at the end of the day, the validators who are voting have to do the deduplication and the actual execution. So would you say the amount of time taken to produce a vote would be longer now?

**Roger** (59:51): To produce the vote? Yes. It's a little bit longer at the end of the window, I would say. Maybe generally a little bit longer, because so far you can execute while transactions are coming in, right? And with this, you have to do the ordering first. You have to wait for all the transactions to come in, then do the ordering, then the execution. So with, let's say, 50-millisecond blocks, it gives you a 50-millisecond \[?\] penalty. You can make the blocks shorter, then maybe it's a 20-millisecond penalty. So it is a bit slower, but it's not much slower, right, compared to the other proposals we had earlier, which really had a high penalty with extra hops \[?\] and whatnot.

*The recording ends here.*
