# Litepaper

## Introduction

In the rapidly evolving blockchain ecosystem, there is an increasing demand for secure, fast and trustless user interactions. With the proliferation of diverse blockchain networks, each with its own unique features and capabilities, the ability to seamlessly interact and exchange value across these platforms has become a crucial necessity. Furthermore, as chain abstraction helps the ease of use of blockchain technologies and blurs the lines between different blockchain networks, it is utmost important to provide a safety mechanism that does not comprimise the fast interaction within or between the different networks.

Borrowing cryptoeconomic security has been research recently (provide references) through restaking primitives. Eigenlayer introduced Actively Validated Services, which uses this borrowed cryptoeconomic security to verify services that do not specifically live on-chain.

Nuff protocol uses and extends this primitive introducing a trustless, dynamically adjusted crypto-economic security layer that actors can participate to insure against attributable faults.


## Problem Statement

As new blockchain networks and n-layer solutions on top of existing networks emerge in an incredible pace, users are faced with an increasingly difficult user experience. Users are asked to maintain different wallets, assets on different chains, use different bridging solutions and face many more user experience problems. 

On top of this, every security domain has a different objectively attributable truth. For users to interact with more than one security domain, users are expected to abide to the rules of the security domain that they are operating in. This introduces significant user experience problems in terms of latency and asynchronicity (is that a word?).

As the number of interactions and number of technologies increase, the possible attack surfaces also increase. It is unreasonable to expect users to asses the risks of the underlying platforms that they use every time that they interact with a new system or with new actors.

Ideally, users should either be abstracted from underlying security semantics or given the opportunity to purchase enough cryptoeconomic security to protect against the risks of underlying security domains or the risks that other actors that they are interacting with create.


## Nuff Protocol

Nuff protocol provides a layer where actors can participate to purchase or provide cryptoeconomic security to hedge themselves against possible attributable faults. The goal is to provide adaptable security to any interaction an actor wants to take through undewriters covering the total capital at risk. In return underwriters get a premium on the coverage that they provide based on the risk that they are taking. Through this mechanism an actor would be fully collaterized for any attributable fault that their interaction involves. 



The system offers the following advancements:

- **Unconditionally Safe Blockchain Interactions:** As long as the participants can find a risk underwriter that can provide insurance against the possible attributable faults for their actions, they can enjoy unconditionally safe interactions. 

- **Security Collateral Marketplace:** Users can purchase collateral for their attributable faults from an open marketplace. Collateral can be provided from any chain that the protocol supports through escrow smart-contracts. Risk underwriters compete for the premium that the user would give on the collateral based on the risk assesment. Users of the protocol who want to earn partial rewards from the premium can delegate their capital to risk underwriters as collateral.  

- **Attributable Fault Risk Model:** Nuff Protocol calculates a premium coefficient based on the total attributable fault that the user wants to purchase insurance for. This coefficient is the basis for the auction and is driven by multiple factors that we will go into detail in the later chapters.

### Security Collateral Marketplace 

There are two main actors that participate in this market;
- Risk Underwriters: Parties who compete for the premium on the insurance purchase.
- Collateral buyers: Parties who wants to buy collateral against their attributable faults

Risk undewriters lock their funds a prio to an 'insurance auction' and provide proofs that they have enough funds on a certain chain ready to underwrite faults.

For each round users, which could consist of DeFi traders, market makers, bridges or simply cross-chain message senders, provide a attributable fault(s) that they want to get insured on. 

When the collateral request hits the mempool, a formula for risk calculation is applied based on multiple inputs such as:
* source chain
* destination chain
* assets involved in order
* total amount of security in the underlying chains
* (add more here)

This risk factor is attached to the collateral request and the collateral request gets broadcasted to underwriters. The insurance premium is derived by the risk factor and the current amount of collateral that is in the smart contracts. Risk underwriters check the current collateral state and provide additional collateral if the total attributable fault costs are undercollaterised.

If the total attributable fault cost for the action cannot be underwritten due to the fact that there is not enough collateral in the market, the premium on the action increases for the next round. Through this, collateral market would always have just enough collateral to cover for the faults. 

When the fault(s) are matched with a risk underwriter, the action that the purchaser wants to take is unconditionally safe for the period that they have purchased the insurance for. 

#### Risk Underwriters

Risk underwriters are on-chain agents that provide insurance on the chains that they operate for any interaction that a user wants to insure. Risk underwriters deploy capital on escrow smart contracts that live on supported chains. In return, they get dynamically adjusted rewards based on the premium that they are entitled to.

Ideally, risk underwriters provide collateral on the same denominator for the fault that is being insured. For example, if the insurance is for a swap between ETH/BTC and the fault is that the market maker does not provide the agreed upon quote, the collateral would be in ETH since the actor who purchased the insurance initially had ETH. 

Another idea is to provide the collateral in NUFF token no matter what the fault is. For this, there has to be an LP for NUFF/Fault currency for each chain that Nuff protocol escrow contract is deployed on. This version might be open to arbitrage possibilities.

#### Escrow Contracts

Escrow contracts are the main component that the collateral providers interact with. Escrow contracts are deployed on networks that Nuff protocol supports. Any participant that wants to provide collateral for risk undewriters to utilise can provide liquidity into these contracts. 

In the initial phase these contracts will also be used to check if the attributable faults were realised or not through inclusion or non-inclusion proofs. 

Contract state would hold a jellyfish merkle tree as its state and emit events of certain actions happening. Deposits and withdrawal from the contract would be tracked through these events which are recorded as state in the [jellfish merkle tree](https://developers.diem.com/papers/jellyfish-merkle-tree/2021-01-14.pdf).

Since it's cheaper to prove this contract's state through light clients, participants who use these contracts to deposit and withdraw funds would face lowered premium costs.

//TODO: Think more around the economics of this, think about AVS cost/reward, collateral provider premium/AVS reward ratio.

### Composable Attributable Faults

//TODO: Categorise the faults

Every action in a blockchain has possible attributable faults. Even a simple transfer throughout its lifecycle could face attributable faults such as;
- Transaction not being finalized.
- Canonical chain that the finalized transaction is included getting hard forked.
- Transaction block being double signed. 
- Transaction not being included in the canonical chain that has social consensus on.

All of these attributable faults can be insured against since all of them can be proven on-chain. One important aspect of these faults are that they are 'time-bound'. 
//TODO: Something about T_Rev and T_Fin here -- 

Each action a user would like to take can get be broke down into possible attributable faults that it can face. After these are broken into granular faults, advanced users can select which faults they want to get insured on. If users don't want to be bothered with the details of the faults, they would buy insurance on the composition of all faults (maybe not on the fork?) and pay a premium accordingly.

Until now, we only talked about 'objectively attributable faults', one interesting extension to the protocol would be 'intersubjectively attributable faults' through EIGEN token (or EIGEN style tokens). Users would purchase insurance based on 'intersubjetively attributable faults'. Premium for these would be higher since proofs of these would involve `token forks`.



//TODO: Explain the `token fork`case


### Light Client Verification

Nuff Protocol consumes the outputs of satelitte chains that the escrow contract lives and the satellite chains that the faults are expected to happen. The deposits/withdrawals on the contracts, collateral being locked for faults, and realisation of faults happening and not happening has to be attested through the protocol to provide unconditional safety for the consumers of the protocol. 

Nuff Protocol validators would attest to the state roots of the satelitte chains, deposit events for the escrow contracts and inclusion or non-inclusion of insured actions. 

To avoid the network bootstrapping problem and provide cryptoeconomic security on the validator attestations, we propose validators being AVSes on Eigenlayer ecosystem. 

We separate the actions that AVSes need to take into two:

*Sequences*:

- Generated by the operators in a distributed manner, there's no centralised coordinator.
- Stored off-chain DA Layer (from Eigenlayer perspective), since it's not cost and speed-effective to store them all individually on-chain.
- Verified on demand, messages are not always verified on-chain - they should be available for verification at all times, but it's not necessary that the attestation is submitted on-chain.


*Checkpoint Tasks*:
- Generated by the Task Manager on-chain, which must determine the work that should be done and expect an answer from the operators.
- Stored on-chain, Tasks are stored directly on the AVS contracts, and a response is expected to be submitted on-chain in a certain time range.
- Always verified on-chain, Operators response to Task are verified on-chain. Failed verification of a Task represents a protocol fault and slashable offence.

A sequence involves abovementioned attributes, the rationale behind separating these two primitives is due to achieve faster finality on the sequences and lowering operational costs. 

Keep in mind that we are still posting the sequences on a DA layer, which we will go later into detail how that enables trust minimized protocol, so it is available for participants to consume. 

``@Don is this correct? Or do we consider Soverign proof verification as the AVS task?``

In order to allow for the implementation of slashing and payment processes, the AVS Task was defined as a Checkpoint Task.

A Checkpoint Task is, essentially, comprised of the submission of a merkle root of the attested Sequences during a time range. Checkpoint Task are submitted at a regular cadence. Checkpoint Task not only provides a safe ledger to the AVS state, but also allows for establishing slashing and payment processes while keeping the SFFL cost of operation cheap.

To facilitate the Checkpoint Task, the operators must then agree on all the Sequences sent in that time period e.g. daily, and aggregate them into a Sparse Merkle Tree (SMT). Anyone that has a copy of this SMT, can generate proofs of membership and non-membership for any Sequence. This way, any Sequence that should've been attested can be verified and any message that shouldn't have been attested can also be verified - leading to both punishments and also liveness tracking. (Taken from SFFL)

#### Light Client Liveness and Security

As can be seen in the chapters above, AVSes which are running light clients to attest to satelite chain states are crucial to the liveness and security of Nuff protocol. 

There are multiple strategies, as of now, that the AVSes can follow:

- [Undonditionally Safe Light Clients](https://arxiv.org/pdf/2405.01459): AVSes could leverage Insured Light Client protocol to provide attestations on the state roots. This would require additional actors, but would be considered outside of Nuff Protocol.
- Soft DAS through trusted full-nodes: AVSes which participate as the validators of Nuff protocol could sample state data from multiple full-nodes and sample them. When they reach a certain DAS treshold, they would provide attestations on the state roots. 
- Running a full-node: The most extreme case would be running a full-node and provide state roots through executing the chain. 

It is up to do the AVSes to define what strategy they would chose. Nuff Protocol will provide reference implementations of the light clients and provide detailed explanation on how these strategies can be applied to avoid AVS getting slashed. 



Talk about insurance buying, auctions, intents, reversion period and risk analysis


Talk about Escrow contracts, multi-chain aspect, relayer and unlocks

### Sovereign Rollup








### Sequence structure






### Example Use Cases

Nuff Protocol unlocks bunch of different use-cases based on attributable faults and collateral marketplace.

#### Trustless fast bridging

Current bridging solutions provide either a trusted setup with low latency or a trustless setup with high cost and high latency. Through insuring against consensus faults (hard fork, transaction not being in a finalised, even social consensus faults) bridges can protect themselves against.

[STAKESURE](https://arxiv.org/html/2401.05797v1) talks about secure confirmation rules for Bridges and CEXs. Essentially, what Nuff Protocol would offer is to change the confirmation rule based on the insurance purchased on the attributable faults that a capital would risk. 

A fully insured bridge would buy enough insurance for the total capital at risk for the duration of the total finalization period for both networks perpetually (depending on the networks also social consensus). This would allow the bridge to move as fast as the insurance confirmation on any escrow contract that the Nuff protocol supports. 

This would allow lightning fast bridges with no safety risk that is fully trustless.

#### Trustless near-instant L2 <-> L1 withdrawal

Currently, users of optimistic L2s have to wait for ORU period and ZK L2s need to wait for the proving period to withdraw their funds. 

For optimistic L2s, this is due to the fact that the sequencer might lie about the state transition and a fraud proof might come in the ORU period. For ZK proofs, its just waiting for the Zk proof for the state transition for that specific sequence. 

In a world where both of these attributable faults can be insured against, a user of these rollups who wants to withdraw quicker than these period could ask for another party that is on L1 to front the withdrawal amount. In return, the person who is fronting the amount would hedge itself against the possible attributable faults and buy insurance from the collateral marketplace. Even if there is a fault for the withdrawals, the fronting party would be unconitionally secured and could claim the insurance.

This also implies that there is unconditional security for L2 to L2 withdrawals too. 

#### Insure against consensus risk of a network with collateral from another network

Perhaps one of the most important next steps for the industry is Chain abstraction movement. Chain abstraction is the idea that web3 users should not and will not care what the underlying system of their favorite application is. This creates a comparable or better experience than web2 applications. The caveat here is that as we blur the lines between networks, the underlying security assumptions also blur out. 


#### Trustless fully collaterized intent marketplace

Intents in its current form is in its infancy with multiple research teams spending a lot of time to mature them. We beleive that in the future, agents or humans would be interacting with intent marketplaces where they define the state that they want to achive instead of defining the execution path to achieve that state. 

While intent systems are evolving, users who are using these systems could be at risk to the attributable faults of the system.


- Collaterizing consensus risk of a network with borrowed collateral from another network
- Trustless fully collaterized intent marketplaces
- Fully collaterized DeFi products (talk about this in detail, borrow/lend, options, futures)





### Rewards

Talk about rewards structure, tokenomics, introduction of new assets through governance



## Conclusion
