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

### Composable Attributable Faults








### Light Client Verification

Nuff Protocol consumes the outputs of satelitte chains that the escrow contract lives and the satellite chains that the faults are expected to happen. The deposits/withdrawals on the contracts, collateral being locked for faults, and realisation of faults happening and not happening has to be attested through the protocol to provide unconditional safety for the consumers of the protocol. 

Nuff Protocol validators would attest to the state roots of the satelitte chains, deposit events for the escrow contracts and inclusion or non-inclusion of insured actions. 

To avoid the network bootstrapping problem and provide cryptoeconomic security on the validator attestations, we propose validators being AVSes on Eigenlayer ecosystem. 

We separate the actions that AVSes need to take into two:

*Messages*:

- Generated by the operators in a distributed manner, there's no centralised coordinator.
- Stored off-chain DA Layer (from Eigenlayer perspective), since it's not cost and speed-effective to store them all individually on-chain.
- Verified on demand, messages are not always verified on-chain - they should be available for verification at all times, but it's not necessary that the attestation is submitted on-chain.


*Checkpoint Tasks*:
- Generated by the Task Manager on-chain, which must determine the work that should be done and expect an answer from the operators.
- Stored on-chain, Tasks are stored directly on the AVS contracts, and a response is expected to be submitted on-chain in a certain time range.
- Always verified on-chain, Operators response to Task are verified on-chain. Failed verification of a Task represents a protocol fault and slashable offence.




Validators of the system (EigenLayer AVS) attest to the deposit events, state roots of the sattelite chains that Nuff protocol supports and 

When the order is executed, NUFF protocol attests to the deposit events inclusion or non-inclusion on the destination chain. Depending on the proof, either locked collateral gets unlocked or the risk merchants are slashed to pay for user's insurance.


Talk about insurance buying, auctions, intents, reversion period and risk analysis


Talk about Escrow contracts, multi-chain aspect, relayer and unlocks


### Chain 

Talk about transactions, block structure, light clients

### Consensus

Talk about how we achieve consensus on the blocks (3 step process)


### Example Use Cases

Nuff Protocol unlocks bunch of different use-cases based on attributable faults and collateral marketplace.

#### Trustless fast bridging

Current bridging solutions provide either a trusted setup with low latency or a trustless setup with high cost and high latency. 



- Trustless low latency fully collaterized bridging
- Trustless near-instant L2 <-> L1 withdrawal
- Collaterizing consensus risk of a network with borrowed collateral from another network
- Trustless fully collaterized intent marketplaces
- Fully collaterized DeFi products (talk about this in detail, borrow/lend, options, futures)





### Governance and Rewards

Talk about rewards structure, tokenomics, introduction of new assets through governance



## Conclusion
