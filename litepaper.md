

# Litepaper

*Authors: Firat Sertgöz, Donnovan Dall*
*Version: v0.1*
## Introduction

In the rapidly evolving blockchain ecosystem, there is an increasing demand for secure, fast, and trustless user interactions. With the proliferation of diverse blockchain networks, each with its unique features and capabilities, the ability to seamlessly interact and exchange value across these platforms has become a crucial necessity. Furthermore, as chain abstraction helps the ease of use of blockchain technologies and blurs the lines between different blockchain networks, it is of utmost importance to provide a safety mechanism that does not compromise the fast interaction within or between the different networks.

Borrowing cryptoeconomic security has been thoroughly discussed recently through restaking primitives. Eigenlayer introduced Actively Validated Services (AVSes), which use borrowed cryptoeconomic security to verify services that do not specifically live on-chain.

Nuff protocol uses and extends this primitive by introducing a trustless, dynamically adjusted cryptoeconomic security layer that actors can participate to insure against faults. 


## Problem Statement

As new blockchain networks and n-layer solutions on top of existing networks emerge in an incredible pace, users are faced with an increasingly difficult user experience. Users are required to maintain assets on different chains, wallets, find and use different bridging solutions and face many more user experience problems. 

On top of this, every security domain has a different objective truth. For users to interact with more than one security domain, users are expected to abide to the rules of the security domain that they are operating in. This introduces significant user experience problems in terms of latency and asynchronicity.

As the number of interactions and number of technologies increase, the possible attack surfaces also increase. It is unreasonable to expect users to assess the risks of the underlying platforms that they use every time they interact with a new system or with new actors.

Ideally, users should either be abstracted from these security semantics or given the opportunity to purchase enough cryptoeconomic security to protect against the risks of underlying security domains or the risks that other actors that they are interacting with impose.


## Nuff Protocol

Nuff protocol introduces a layer where actors can participate to purchase or provide cryptoeconomic security to hedge themselves against possible faults which may arise from interacting many distributed systems. The goal is to provide adaptable security to any interaction an actor wants to take through an underwriter which would cover the total capital at risk. In return, underwriters get a premium on the coverage that they provide based on the risk that they are taking. Through this mechanism, an actor would be fully collaterized for any fault that their interaction involves. 


The system offers the following advancements:

- **Unconditionally Safe Blockchain Interactions:** As long as the participants can find an underwriter that can provide insurance against the possible faults of their interactions, they can enjoy unconditionally safe interactions. 

- **Security Collateral Marketplace:** Users can purchase collateral for their faults from an open marketplace. Collateral can be provided from any chain that the protocol supports through escrow smart contracts. Risk underwriters compete for the premium that the user would give on the collateral based on the risk assessment. Users of the protocol who want to earn partial rewards from the premium can delegate their capital to risk underwriters as collateral.  

- **Fault Risk Model:** Nuff Protocol calculates a risk coefficient based on the possible faults that may arise on the action the user wants to purchase insurance for. This coefficient is the basis for the auction and is driven by multiple factors that we will go into detail in the later chapters.

### Security Collateral Marketplace 

There are two main participants in this market;
- Risk Underwriters: Parties who compete for the premium on the insurance purchase.
- Collateral buyers: Parties who want to buy collateral against their faults

Risk underwriters lock their funds *a priori* to an 'insurance auction' and provide proofs that they have enough funds on a certain chain ready to underwrite faults.

For each round, buyers, which could consist of DeFi traders, market makers, bridges or simply cross-chain message senders, provide an order that they want to get insured on. This order is then translated into a set of possible faults which can be independently insured, depending on the risk appetite of the user.

When the collateral request hits the mempool, a formula to calculate the risk coefficient is applied based on multiple factors for each interaction, such as:
* for PoS protocols: 
    * total stake
    * byzantine threshold: this is more complex than calculating 1/3 and is usually protocol-specific
* for PoW protocols: 
    * hash rate
    * centralization of mining power
* ease of verifying collusion: can an external observer of the protocol easily verify misbehaviour like double signing, double spending, etc
* ease of data retrieval: does the protocol allow retrieval of information directly from nodes or do they have to be provided by additional systems that are not subject to mechanism 
* data verification: does the protocols involved support any systems to verify the validity of any data provided?
* token volatility
* community sentiment: governance protocol to allow the community to vote and apply an additional factor based on the general sentiment of the protocol involved
* asset volatility
* tokenomics
* protocol reliability
* complexity of the interactions
* interaction validity verification
* (essentially this is CoC++)

`Research Note: Some of the faults outline here are subjective and cannot be easily observed by an external system, thus there is a need for adaptation based on retroactive analysis, community sentiment and other dynamic means of signalling a change in the environment over time. How we might facilitate these modification are yet to be explored.`

This risk factor is attached to the collateral request and the collateral request gets broadcasted to underwriters. The insurance premium is derived by the risk factor and the current amount of collateral that is in the smart contracts. Risk underwriters check the current collateral state and provide additional collateral if the total fault costs are undercollaterised.

`Research note: determine how the factor is attached, shouldnt be user. Probably applied at the beginning of sequence being built. We spoke yesterday around treating that as "gas" for our system and refunding to the payee. Although since we settle everywhere its not so trivial.`

If the total fault cost for the action cannot be underwritten due to the fact that there is not enough collateral in the market, the premium on the action increases for the next round. Through this, collateral market would always have just enough collateral to cover for the faults. 

When the fault(s) are resolved, the action that the buyer wants to take is unconditionally safe for the period that they have purchased the insurance for. 

#### Risk Underwriters

Risk underwriters are on-chain agents that provide insurance on the chains that they operate for any interaction that a user wants to insure. Risk underwriters deploy capital on escrow smart contracts that live on supported chains. In return, they get dynamically adjusted rewards based on the premium that they have provided.

Ideally, risk underwriters provide collateral on the same denominator for the fault that is being insured. For example, if the insurance is for a swap between ETH/BTC and the fault is that the market maker does not fulfill the agreed upon quote, the collateral would be in ETH since the buyer purchased the order with ETH. 

`Research Note: Another idea is to provide the collateral in NUFF token regardless of the fault. For this, there has to be an LP for NUFF/Fault currency, for each chain that Nuff protocol escrow contract is deployed on. This version might be open to arbitrage possibilities, although can be facilitated with Threshold decryption, ZKP, state channels or other private auctioning systems.`

#### Escrow Contracts

Escrow contracts are the main component that the collateral providers interact with. They are deployed on networks that Nuff Protocol supports. Any participant that wants to provide collateral for risk undewriters to utilise can provide liquidity into these contracts. 

In the initial phase, these contracts will also be used to check if the faults were realised or not through inclusion or non-inclusion proofs. 

Contract state would hold a [Jellyfish Merkle Tree](https://developers.diem.com/papers/jellyfish-merkle-tree/2021-01-14.pdf)(JMT) as its state and emit events of certain actions happening. 
Deposits and withdrawal from the contract would be tracked through these events which are recorded as state in the JMT.
Since it's cheaper to prove this contract's state through light clients, participants who use these contracts to deposit and withdraw funds would face lowered premium costs.

`Research note: After the initial phase there needs to be a way to prove arbitrary inclusion or non-inclusion proofs for faults. One idea is to dynamically adjust AVS attestations and rewards based on the complexity of the inclusion/non-inclusion proofs.`

### Composable Faults

Every interaction with a blockchain has possible faults.

[EIGEN Whitepaper](https://github.com/Layr-Labs/whitepaper/blob/master/EIGEN_Token_Whitepaper.pdf) classifies faults into three categories:
- **Objectively Attributable Faults**: These are the faults that an observer of the system can unilaterally agree on. Such faults can be resolved through an on-chain dispute. We can think about these as cryptographically resolvable faults. 
- **Intersubjective Attributable Faults**: These faults require the observers of the system to come together and agree on the fault. EIGEN whitepaper discusses these in detail and offers an on-chain solution to resolve these types of faults. Examples of these type of faults could be data witholding attacks, oracles not providing correct prices and censorship. 
- **Non-attributable Faults**: Last category is non attributable faults. These faults are non-distinguishable to the observer or subjective to the observer. Examples would be faults happen in car-sharing reputation reports, food tasting scorings, music competitions.  

A simple transfer throughout its lifecycle could face  faults such as;
- Transaction not being finalized.
- The Canonical chain that the finalized transaction is included getting hard forked.
- Transaction block being double signed. 
- Transaction not being included in the canonical chain that has social consensus on.

All of these faults can be insured against since all of them can be proven on-chain. One important aspect of these faults are that they are 'time-bound'. Time-boundness is an important property of faults since it effects the insurance premium. If a fault requires capital to be locked for a long time, the premium would increase in return. 

Each interaction a user would like to take can be broke down into possible faults that it can face. After these are broken into granular faults, advanced users can select which faults they want to get insured on. If users don't want to be bothered with the details of the faults, they would buy insurance on the composition of all faults and pay a premium accordingly.

`Research note: Until now, we only talked about 'objectively attributable faults', one interesting extension to the protocol would be 'intersubjectively attributable faults' through EIGEN token (or EIGEN style tokens). Users would purchase insurance based on 'intersubjetively attributable faults'. Premium for these would be higher since proofs of these would involve intersubjective token forks. EIGEN Whitepaper talks about the mechanism to implement such a token. What Nuff protocol would do to support intersubjective faults is to track possible forks on the tokens that the AVS is attesting to and insure against the possible forks. The exact mechanism is a topic of research.`




### Light Client Verification

Nuff Protocol considers the external chains in the system as 'satellite' chains. The validators of the system verify the escrow state involved for each interaction input and the validity of the satellite with regards to the interaction outputs. Each satellite contains an escrow contract and represents the actors' balances in Nuff. The deposits/withdrawals on the contracts, underwriting collateral lockup, and realisation of faults happening and not happening has to be attested through the protocol to provide unconditional safety for the consumers of the protocol.

Nuff Protocol validators would attest to the state roots of the satellite, containing deposit events input data for the escrow and membership/non-membership of insured actions.

To avoid the network bootstrapping problem and provide underlying cryptoeconomic security on the validator attestations, we propose Nuff validators as AVSes within Eigenlayer ecosystem. 

### Attestation Responsibilities

*Sequencing*:
- Collects transaction orders according to the protocols ordering policy // TODO @don add notes on fair ordering mechanism in the first approach, leader bond, slashing signals, etc
- Ordering protocol is simple, yet needs to consider the total underwriting availability (not capacity) of the system, and the Time to Attest to each checkpoint. We have considerations of how to approach lagging, overlapping and non-relevant order attestations in the lattice design.
- Submits Sequence for interpretation by executors to the DA layer, and potentially AVS service manager.
- Orders and the Checkpoints within them must contain a unique identifier.

A Sequencer borrows trust from the participants with orders in the mempool. As such, the ordering protocol should be objectively verified. This is to ensure proper handling of transactions and that the Sequencer is liable to slashing of the bond on the DA layer/AVS task for misconducting their duties to the system.


`Note: Sequencing is currently centralised, although there are many systems being built which support decentralised sequencing.`

`Note: as at the current view of the system, the sequencer semantics, such as ordering and compression are as standard, and any nonstandard information is as above. See below around sovereign sdk semantics for in depth views.`

*Execution: Checkpoint Attestation*:
- Fetches the Sequence, and determines the checkpoint ordering.
- Validates the Sequence follows the ordering protocol. If a sequence does not follow the protocol then the validator must decide on the signal to emit, depending on the severity of the violation. The validator is able to refuse an order, but must risk being challenged for doing so.
- Instructs the satellite light client protocol to sync, and to report state at the checkpoints.
- Determine the state root of the satellite at the checkpoint.
- Retrieve/Build either:
    - Proof of membership, showcasing the escrow state contains the required checkpoint details at index `i`
    - Proof of non-membership, demonstrating the state of the escrow does not contain a checkpoint at index `i`
- Verify the membership, to witness the verification of the attestation.
- Verify the correctness of the information. This can be facilitated by Data Availability Sampling, or some other means. Assuming there is no objectively verifiable Data Validity Protocol for the Satellite, this process is subjective and requires the AVS to be dilligent about the data it attests to, it is expected the AVS would query many node providers for the information it needs to reach a risk threshold they decide. For further investigation, see [Light Client Liveness and Security](https://link-below)
- Commit the state of each checkpoint, pruning any previous states that are not required for future execution.
- Signs and broadcasts the executed state to either: DA layer, Relayer or some other mechanism for proposing the new information containing the validators signature, checkpoint roots, and execution witness.

A validator is free to continue operating the light client sync protocol for the satellites to aid in liveness scenarios and to protect itself from any weak-subjectivity related issues that may arise from poor liveness in a satellite system, or little demand for orders relating to the satellite. 

`Research Note: Decide if a proposer/relayer agent is required on behalf of the validating AVS. With sovereign, the data is posted lazily to the DA layer and verified elsewhere.`

`Research Note: Determine the specific mechanisms around liveness related issues when a node refuses to execute on a checkpoint. In the case of Sequencer-related faults then we must reach a game theoretical equillibrium such to avoid issues arising out of refusal to execute/poor sequencing.`

`Research Note: A Signal can be made across some another network protocol to gossip information such as proofs, DAS related commitments and fulfillment of satellite orders. This has been considered but will be investigated in further design approaches.`

`Note: Need to decide AVS task flow, in the beginning, it's a longer term than specific checkpoints, although further exploration of Checkpoint-based AVS tasks might be interesting, where the Sequence is posted to the Task Manager, and that purchases/reserves the chain insurance. Potentially the comissioning of an operator could be on a per-sequence basis, however we risk losing underwriting capacity`

A sequence involves abovementioned attributes, the rationale behind separating these two primitives is due to achieve faster finality, further decentralisation on the sequences and lowering operational costs. 

#### Slashing conditions

In order facilitate the economic mechanisms for comissioning the duties of an AVS, there must be some verification to the objective, subjective and nontrivial effort the AVS has to undertake.

// don: to explain the contract of the task with respect to eigenlayer and the sequences(s), but the checkpoint involves some registry
// don: note on slashing and guardians (stakesure training wheels for retroactive subjective issues)

When a validator is executing the sequence, every step in the State Transition Function (STF) for each checkpoint is a witnessable action. It appends a log of the information they received and the actions taken with respect to the Sequence and the Checkpoints therein. A validator can append additional information to their transcript which can support its claim that the information it attested to satisfied the correctness properties of the protocol. This log is in the form of a Zero-Knowledge proof, which becomes their Attestation once they have executed the checkpoints within the sequence.

The Attestation is in the form: `(signature, (state root, (outputs, witness))`

Note on FRI-like semantics, ZKP execution reqs, etc, optimisation.

The fork-choice rule of the DA is not enforced eagerly at the time of relaying attestation, this is facilitated with a lazy operation as described in [Sovereign SDK](below).


### Protocol Guarantees

Verification of the Attestation requires a number of steps to satisfy the properties of *correctness*, *safety* and *liveness*.

#### Safety:

#### Correctness:

Correctness can be objectively verified. A validator can either attest to conflicting information or information that correct assuming objectively proper application of the protocol. For this reason, we expect validators to attest without any reasonable suspicion of tampering on the data they receive. 

When verifying the proof of execution, the proof contains a transcript of the actions taken by the validator. If an action was not in accordance with the rules of the protocol, or violates some environmental mechanisms of the node software, this will produce an invalid proof. Which will demonstrate that the validator did not satisfy the protocol.
Futhermore, the wrongful execution of information can be verified objectively by a difference in the state roots posted to the DA layer. A difference in state roots would inform the network participants that some actors are misbehaving and to investigate further.

Objective faults in Nuff protocol:
- Execution validity, if the execution is modified then the state would not be deterministic
- Double signing, If a node signs two conflicting outcomes, this is proven onchain.

With the exposure to subjective systems such as RPC, external Consensus mechanisms and network latency, there is a subjective element to their participation. If the offence committed was intersubjective, there are additional training wheels provided in the form of [EigenToken](^999). These Guardians can interpret the slashing offence and be comissioned by the validator to appeal, notifying that they have acted dilligently and the social consensus provided by the Guardians will respond, although this is a latent procedure.

Since the system is retroactive by design, we can verify faults which are normally intersubjective. One example is Data Availability, if a validator had access to a DAS-style commitment to reasonable data, and publishes that commitment with its' attestation, then we can attribute that the validator was dilligent in its handling of the data.

We can also attribute Censorship issues due to the ordering protocol, and the application of such deterministically.


`Note: Further technical exploration into how a validator can signal that a chain is objectively colluding by Double Signing and modifying execution validity should be explained here. One approach is unilaterally abstaining, and the guardians can recover from any issues arising out of such. However the likelyhood of abstaining in relation to a chain fault would usually be deterministic and the validators would reach consensus in abstenance here.`

`Note: Expand on potential use of Naysayer proofs.`

#### Liveness:

Liveness properties can be retroactively verified, and as such we benefit of leveraging a lazy structure since the reward is not paid out upon completion, but upon verification. Accounting for the aggregation of the attestations for the epoch the validator was comissioned for at withdrawal time.

> With this property, validators who are objectively colluding can be slashed eagerly, but they can benefit from the laziness property, allowing them to continuously participate in the system without recommissioning of their services every epoch.

 The validator must attest for the commissioning of the task. If it does not provide an attestation then the underwriting  of sequences can be deemed `void` for their participating stake. Note, there are some conditional reasons a validator may not sign, or miss the attestation period. As such, this requires thorough analysis of these mechanics to ensure fairness.

#### Verification process

// TODO: @don this guy

A Checkpoint Task is comprised of the submission of a merkle root of the attested Sequences during a time range. Checkpoint Task are submitted at a regular cadence. Checkpoint Task not only provides a safe ledger to the AVS state, but also allows for establishing slashing and payment processes while keeping the SFFL cost of operation cheap.

To facilitate the Checkpoint Task, the operators must then agree on all the Sequences sent in that time period e.g. daily, and aggregate them into a Sparse Merkle Tree (SMT). Anyone that has a copy of this SMT, can generate proofs of membership and non-membership for any Sequence. This way, any Sequence that should've been attested can be verified and any message that shouldn't have been attested can also be verified - leading to both punishments and also liveness tracking. (Taken from SFFL)

// TODO: @don depth and mechanics around DA, agg proof, relation to standardised l2 actors, consumption of batches, slashing and forced inclusion, notes on lattice based security

#### Light Client Liveness and Security

As can be seen in the chapters above, AVSes which are running light clients to attest to satelite chain states are crucial to the liveness and security of Nuff protocol. 

There are multiple strategies, as of now, that the AVSes can follow:

- [Undonditionally Safe Light Clients](https://arxiv.org/pdf/2405.01459): AVSes could leverage Insured Light Client protocol to provide attestations on the state roots. This would require additional actors, but would be considered outside of Nuff Protocol.
- Soft DAS through trusted full-nodes: AVSes which participate as the validators of Nuff protocol could sample state data from multiple full-nodes and sample them. When they reach a certain DAS treshold, they would provide attestations on the state roots. 
- Running a full-node: The most extreme case would be running a full-node and provide state roots through executing the chain. 

It is up to do the AVSes to define what strategy they would chose. Nuff Protocol will provide reference implementations of the light clients and provide detailed explanation on how these strategies can be applied to avoid AVS getting slashed. 

### Claims

The claims process is trustless and can be facilitated in a few ways without interaction with a set of provers.

Since each interaction represents a checkpoint containing a height `H` in which a satellite must reach for the fault to be deemed passed, a validator can witness the state at each checkpoint and determine set membership or non membership for an order-related fault in the satellite at `H`. With this state, a claimant can retrieve a proof where the state within the store for the fault either contains the fillers action or it was not filled within the checkpoint period. 

For the purposes of set inclusion each satellite contains a smart contract which is backed by a data structure allowing for efficient set membership lookups, as well as non-membership. Preliminarily, we are considering a JMT. To observe that an interaction has been fulfilled, the smart contract can forward to the destination while appending a log of the input data and transaction semantics in the underlying store.  whilst passing the call through to the destination. 

For simple orders which native to the Nuffle Ecosystem, an infrastructure builder can implement the deposit into the escrow containing a memo for the order for the purposes of the fulfillment. This would enable the validators to attest to the deposit natively without any additional mechanics other than the memo and the proxy address. 

`Research Note: Since the claim can be made non-interactive and settled via the checkpoint mechanism, we can reduce a step for the user to unlock immediately, sending the funds to the buyer, assuming we have knowledge of the destination.`

For more complex faults, this can be determined based on the validators of Nuff and the Guardians.

// TODO: notes on retroattributable faults and intersubjective faults

### Sovereign Rollup 

For Nuffle Protocol, it is clear that we are attesting to the consensus of external systems and providing additional cryptoeconomic garuantees around the system, providing primitives that enable systems to reason about consensus in jurisdictions that have so far been 2-Dimentional[^bridge trilemma]. However, it is true that when this system is also a blockchain with its own set of rules and consequences, it would be paradoxical to base the cryptoeconomic security of such external systems on a single consensus mechanism itself. 

//TODO: wording

With the goal of also being able to secure against faults on Ethereum without leveraging existing stake in-flight. This leverage of the ecosystem should be utilised responsibly and in a fair way without compromising other protocols and the underlying protocol, as well as minimising the leverage possibilities which further reduce the CoC on Ethereum. 

With a goal of existing across many blockchains, the initial architecture is facilitated with Nuff a a Sovereign Rollup, built with the [Sovereign SDK](https://github.com/Sovereign-Labs/sovereign-sdk). The Sovereign SDK enables some properties which are particularly relevant for Nuff Protocol.

#### "Free ZK" State transition functions

The validator nodes execute a set of satellite light client protocols, witnessing checkpoints and logging any further information that will be useful in the case of a claim. There are multiple hand-rolled light clients that exist for some of the top consensus mechanisms, such as zkBridge on Eth, Lagrange's State Committees, NEAR's own light client[^]. The complexity arises due to the shifting technology and experience in the ZK space, systems become outdated quite quickly. This becomes even more cumbersome for the case where many systems should be objectively verified via a light client protocol. It is additionally non-trivial without some expertise to expand on such systems with newer features, given the constraints within the circuits. 

With Sovereign, the ability to support new protocols by writing standard rust code, providing a module and the community agreeing to introduce the new system with the current attestation process. This allows us to innovate with light clients further than how they are used today. One example might be embedding a protocol with DAS enabled features, by the protocol developers providing a module which can be imported in rust whilst also not intruding on other protocols. Eventually this modularisation of such systems can be driven with even more granularity such as binary loaders and with technologies like WASM.

`Research Note: We have additional ideas for execution, and user-provided fault blueprints. This is to be expanded in further revisions of the litepaper.`

#### Lazy proof verification: 

Validators have to provide witness transcript showcasing their steps as they action the checkpoints in the protocol. They must also execute this many times and much of the computation becomes cryptographic waste to reach the same output. Whilst this is a side effect of the cost of safety, we aim to reduce their complexity and costs associated with running the validator software to as close to zero as possible. With this in mind, validators post their transcripts, proofs and additional information to a DA layer. The DA layer verifies the signatures according to the sequence, and exposes a small commitment to submission. Once the attestation round has reached a reasonable threshold, a prover will come and aggregate those proofs and they will be verified by the settlement protocols.

#### State

Since there is no execution of smart contracts within a runtime (yet). The protocol state is a thin composition of information that exists externally:
- Escrow state for all satellites
- Constant state for checkpoints in the sequence

#### Settlement

The settlement of this system can be interpreted across a few steps:
- When the faults within an order have passed, this can be deemed as `EXECUTED`. An additional event may be emitted by an individual validator for consumption by other systems.
- When enough validators have submitted their attestations and consensus can be observed, this can be seen as `OPTIMISTIC` finality.
- Once the aggregation proof has been submitted to the settlement systems and verified the threshold. This state can be deemed as `SETTLED`.
- Once finalization for the settlement transaction(s) can be deemed `FINAL` as per the consensus of the systems. At this point the settlement contract can account for any rewards, and any eager slashing for misconduct.

#### Risk-Weighted Attestation Graph DA (lol)
Settlement, even in the context of a sovereign rollup has an interesting distribution of opinions, with some forming the opinion "L2s dont exist" to "L2s inherit security from L1". This complexity is also further enhanced since Nuff protocol does not necessarily "settle" anything other than signal that the AVS validators have fulfilled their part to play and the reached consensus on the faults in the sequence. 

This distinction does not apply in practice, since, if you provide the state to a DA layer that has a lower Cost of Corruption(CoC) than the underwriting parties, the system can be seen as secure as the DA. This can be approached by modelling the settlement of the chain as a function of the Risk/CoC.

wipmath:

Let `G = (V, E)` be a graph where:
* `V` represents the set of vertices, corresponding to a satellite `S`
* `E` represents the set of edges, corresponding to a risk coefficient `ρ`
* Neighbouring vertices favour other vertices with similar `ρ`
* There should be at least 3 edges, where more is possible for higher risk satellites. This enables redundancy as mentioned below.

> TODO: explain how E is resolved, more precision around implementation and quantification

Let:
* `Σ` be a sequence to be built
* `N` be the set of supported satellites
* `Φ` be the set of faults to be observed in the system

When `Σ` is constructed, the sequencer removes the orders from the mempool and translates them to create `Φ`. For all `V`, the partitions are formed into the set of chunks `C`, where `C = Σ_i^n`, according to the ordering protocol.
Once built, `for all i in C`, `C_i` is broadcasted to `N_i`. 
A validator then executes each `C_i`, attesting to the protocol for `N_i`. 
At the time of proposal for the execution results, the validator posts the results to the neighbouring edges `E` responsible for observing `N_i`, broadcasting the commitment to observers such as the buyer and other consumers.
This enables the aggregation prover to take the `C_i` and prove it without knowledge of `Φ`, the proof is then verified by the observing edges `E`.
An observer witnesses the verification of `C` for all `N`, and can action once the conditions of the order have met the threshold.
The execution of `C_i` can be deemed final once the aggregation proof for it has been verified by `E` and the finality of `N_i` has passed. `Note: This can be verified in the next round if a sequence needs to be built in the meantime`

Advantageous Properties:
1. Decentralised Assurance:
    * The nuff protocol is not reliant on a single DA system and the properties of consensus within those. If a claim is to be made, a buyer can effectively submit a request to a neighbour without fear of collusion from the system. Since the neighbours of the system can also be testified against by the neutral participating neighbour, it becomes nontrivial to circumvent 3+ consensus mechanisms.
2. Avoiding Releveraging:
    * Releveraging stake provided by the AVS from an Ethereum beacon validator whilst also using the same stake to retain the state of the Nuff Protocol would introduce some recursive mechanism for censorship. Introducing a neutral system would build on the trust of other systems, such as Bitcoin, thus mitigating another dimension of the trilemma.
3. Economic Incentives:
    * The protocol enables an interesting economy of scale around `ρ`, if satellites with a less-than-ideal score prefer to be secured by systems with favourable results, they are incentivised to improve on the protocol and mindshare to have access to additional incentives for their users.

Future Work and Considerations:

Verification:
- Add more depth around verification, joining the chunks back into the sequence, and building from the last sequence.
- Challenge why it doesn’t matter once the granularity of Φ is verified, essentially removing the lagging latency problem. Note that the sequencer doesn’t signal to slash; the aggregation proof does.

Additional Notes:
- Native cryptography for each satellite
- Releveraging stake
- Cost of Corruption (CoC)
- Chokepoint
- Sov SDK

#### Aggregation proof structure

> TODO: later showcasing outcomes and how they can be consumed

### Example Use Cases

Nuff Protocol unlocks bunch of different use-cases based on faults and collateral marketplace.

#### Trustless fully insured fast bridging

Current bridging solutions provide either a trusted setup with low latency or a trustless setup with high cost and high latency. Through insuring against consensus faults (hard fork, transaction not being in a finalised, even social consensus faults) bridges can protect themselves against faults.

[STAKESURE](https://arxiv.org/html/2401.05797v1) talks about secure confirmation rules for Bridges and CEXs. Essentially, what Nuff Protocol would offer is to change the confirmation rule based on the insurance purchased on the faults that a capital would risk. 

A fully insured light client bridge would buy enough insurance for the total capital at risk for the duration of the total (source and destination chain) finalization period for both networks perpetually (depending on the networks also is secured aginst social consensus). This would allow the bridge to move as fast as the insurance confirmation on any escrow contract that the Nuff protocol supports. 

This would allow lightning fast bridges with no safety risk that is fully trustless.

#### Trustless near-instant L2 <-> L1 withdrawal

Currently, users of optimistic L2s have to wait for ORU period and ZK L2s need to wait for the proving period to withdraw their funds. 

For optimistic L2s, this is due to the fact that the sequencer might lie about the state transition and a fraud proof might come in the ORU period. For ZK proofs, its just waiting for the Zk proof for the state transition for that specific sequence. 

In a world where both of these faults can be insured against, a user of these rollups who wants to withdraw quicker than these period could ask for another party that is on L1 to front the withdrawal amount. In return, the person who is fronting the amount would hedge itself against the possible faults and buy insurance from the collateral marketplace. Even if there is a fault for the withdrawals, the fronting party would be unconitionally secured and could claim the insurance.

This also implies that there is unconditional security for L2 to L2 withdrawals too. 

A primary consumer of Nuffle Protocol would be NFFL (Near Fast Finality Layer). NFFL would purchase insurance on the total capital for each transaction that would span L2 <-> L2. Through this NFFL would hedge itself against a sequencer fault (fraudalant or software fault). 

#### Insure against consensus risk of a network with collateral from another network

Perhaps one of the most important next steps for the industry is Chain abstraction movement. Chain abstraction is the idea that web3 users should not and will not care what the underlying system their favorite application is running on. This creates a comparable or better experience than web2 applications. The caveat here is that as we blur the lines between networks, the underlying security assumptions also blur out. Uninformed users might end up in chains that do not have enough cryptoeconomic security to provide a reliable experience. 

Another situation could be that a user trusts a certain security domain but still wants to interact with other networks that have less cryptoeconomic security. User would want to insure these interactions somehow to hedge themselves against possible consensus faults.

With Nuff Protocol, regular users of a certain security domain could purchase insurance on the network that they trust for the actions that they would like to take on another network as long as there is a risk underwriter that would provide collateral for the faults that the action could create. 


#### Trustless fully collateralized intent marketplace

Intents in its current form is in its infancy with multiple research teams spending a lot of time to mature it. We believe that in the future, agents or humans would be interacting with intent marketplaces where they define the state that they want to achive instead of defining the execution path to achieve that state. 

While intent systems are evolving, users who are using these systems could be at risk to the faults of the system. The faults could be anywhere from solvers not providing timely execution of the solved intents, to solvers not providing the agreed upon exchange rate of the swaps. As the intent systems/marketplaces mature, we will see many more possible faults that the users can and will want to insure against.

Participants on both side, users who are defining the intents and solvers who are finding the best path for their intents can purchase insurance to cover themselves for faults that their actions might create. 


`Research Note: There will be more use-cases as we discover more ways to leverage Nuff Protocol. We invite the community to foster new ideas around Nuff Protocol and its capabilities.`


## Economics and Rewards
`A complete description of Nuff Protocol tokenomics will follow in the later editions of this document.`

### Overview
Nuff protocol's economics is designed to be fair to all participants. Nuff protocol aims to achieve an economic equilibrium between the services that participants are providing and the consumers utilising those services. 

Here is a summary of the key ideas that derive the economics of the system:

**Epoch Rewards**: AVSes are validators of Nuff protocol and get payed per checkpoint. Initially, the rewards would be in ETH but eventually would be a combination of ETH and NUFF. This could be achieved through dual staking.  

**Insurance Premium**: Every participant who wants to buy insurance for their individual or composed faults has to pay a premium. This premium drives from the risk coefficient calculation that was discussed in the previous chapters. The majority of the premium goes to the risk underwriter who wins the auction to underwrite, and some of it goes to the AVS that attest to the faults happening or not happening. 

**DA Costs**: As explained in Lattice DA section, Data availability layer costs are determined by the level of security based on the interaction taking place and the faults that the AVSes need to attest to. This cost will be taken from the --@don

**Sequencer Fees**: Sequencer gets a fee for the ordering and the risk calculation per sequence that it is commiting to the DA layer. These fees will be included into the transaction fees.

**Protocol Treasury**: TBD



`Research note: We are currently researching inheriting gas fees and execution from the satelite chains that the escrow contracts are deployed on. One idea is to have users prepay NUFF on the satelite chains. After the premium auction is finalized and the AVS attest to the purchase, some NUFF would be transferred to the risk merchant internally on the contract. Furthermore, users who might want to buy insurance later on, could already buy some NUFF and delegate their NUFF to risk merchants, in return getting some rewards. These rewards could also fund their transactions and premiums later when they want to purchase premium.`




### Resources Provided

This is an overview of the resources provided by the participants of the protocol:

- **Cryptoeconomic security**: Every fault is validated through underlying cryptoeconomic security provided by the AVSes. On top of that DA Lattice provides cryptoeconomic guarantees around Sequencer commitments based on the Checkpoints involved in a specific sequence.
- **Data Availability**: DA Latice provides a trustless way to verify sequencer commitments. On top of the cryptoeconomic guarantees provided by DA Lattice, DA Lattice also provides storage of the sequences for a certain window.
- **Risk Collateral**: Capital that provides collateral against faults that an interaction might create. From a protocol standpoint, this is where the users of the system would interact with the value flow.  

`Research Note: Governance on which networks to support on Nuff Protocol will be decided by the token holders eventually. This would determine the light client protocols that the AVSes need to run to attest, risk coefficients that need to be calculated and the location of the escrow smart contracts`



## Future

`Note: extend the future and vision`

We believe in a world where any person or agent can freely participate in an open, fair, and safe crypto economic ecosystem. We think this can only be achieved in a world where trustlessness and sovereignty are the primary principles. To achieve such an ecosystem, systems that form it should provide comparable security guarantees and the user should be able to hedge the possible security risks with cryptoeconomic guarantees. When this guarantee is achieved, then we can build user experiences where the users do not have to worry about their interactions. Verifiable and cryptoeconomically secure interactions would create better experiences on web3 than their web2 counterparts. Nuff Protocol embraces these values in its design principles, providing  

As Nuffle Labs, we are commited to this vision and we invite web3 communities, researchers, economists, mathematicians and everyone who believes in these principles to join us with their products and ideas to move towards this vision. 
