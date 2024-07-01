

# A Unified Security Layer

*Authors: Don Dall, Firat Sertgoz*
## Abstract
We are seeing a Cambrian explosion of restaked security systems pioneered by EigenLayer. These systems improve the cryptoeconomic security of protocols and applications by leveraging restaked assets. Each restaking provider has different semantics specific to their network. These semantics are hard to understand for the higher-level purchaser or delegate, and sometimes they are irrelevant, where services only care about borrowing security to do some productive work. 

![GQ7_9xbXcAEm34A](https://hackmd.io/_uploads/rklNfFY8A.jpg)
*[Reference](https://x.com/gauntlet_xyz/status/1805662991708213452/photo/1)*

Cryptoeconomic security is already scarce. Different restaking providers across blockchains fragment cryptoeconomic security further. For restaking to become mainstream, applications need to share security subsystems, experiment with different providers, and potentially even combine the use of one provider with another to improve their cryptoeconomic security -- all depending on supported features, ecosystem alignment and appetite for risk.

In this blogpost, we outline the case and the design for a unified security layer that enables service providers to leverage any crypto-economic security from any restaking platform.

## The Case for Unification

The growing number of restaking protocols are fragmenting available cryptoeconomic security. Without unification,
participatipants in restaking will suffer a variety of problems. Unification allows new features that are otherwise impossible with a single restaking provider. Unification solves for the following problems: 

- **Stake exclusivity**: A service may require exclusivity to the slashable stake to provide economic security garuantees over a Proof-of-Custody protocol. This exclusivity requirement would be problematic since the total available security to purchase could be reduced to a single service and rest of the service in the restaking protocol might starve.  In EigenLayer, tasks requiring stake are purchased for a finite period, and this introduces some form of 'fair use' policy, as we currently see through whitelist mechanisms. Unification layer could address this by allowing the service to divide the exclusive stake claim into multiple restaking protocols.

- **Asset Variety**: An institutional operator generally has multiple native assets and are validators to many blockchain protocols. This opportunity represents that they can attest to many different systems and provide cryptoeconomic security all at once to a service. A unification layer would allow node-operating businesses to delegate once. The system, in this case, can support and attest all supported assets from all restaking providers. A service that operates in multiple chains could require different native assets to secure its service. A unification layer would allow defining multi-chain security strategies with native assets with already existing restaking protocols..

- **Missed opportunity**: Blockchain protocol without a native restaking layer miss the opportunity to provide cryptoeconomic security to decentralised services. Builders need to build expertise in each restaking system they want to support. This introduces opportunity cost, risk, and auditing requirements for the protocol. A unification layer would allow native delegates to lock up their stake and operators to lock their validators to a smart contract on the native chain. They can, then, delegate to the unification layer to attest for them based on their strategy. 

- **Semantics of Restaking Protocols**: Every restaking protocol defines its interface that service providers must adapt and integrate. This creates a bottleneck in value generation since service providers want to reach their customers as soon as possible with various assets. A unification layer significantly reduces the time to market for all decentralised services. 

## The Unified Security Layer
We introduce a unified security layer that enables decentralised services to leverage any crypto-economic security from any restaking platform. Services only need to provide the service binary, validation semantics of their service, and their configuration. This layer abstracts the orchestration of the services with different restaking protocols. It provides a common interface for the commissioning and consumption of security attestations for all providers. 

The unification layer allows for agents in each security system to pool their attestations and aggregate them. This system would utilize the expertise of builders who understand the restaking paradigm and limit the burden on the external actors who want to purchase security, freeing them up to concentrate on their value generation. For operators, allowing them to become delegates means they can delegate all of their assets to a trustless operator set. They can even utilise account abstraction to ensure the attestation is delegated to the unification layer.

**High-level Architecture Diagram:**

Below is our initial birds eye view of the architecture.

![final big](https://hackmd.io/_uploads/BJMezZ38R.png)

**Restaking Platform Interaction Diagram:**

The below diagram is zoomed in to represent the interaction flow for different actors.

![final_flow](https://hackmd.io/_uploads/BylGz-hIR.png)

## Actors

### Delegators:

Delegators delegate all their assets on one unified platform and interact in one place. There is no need to search for services and deal with operators to build trust. Operators earn trust in a unified system, allows others to see an operator's reputation stake and network participation. Delegators can set adaptive strategies, have power over their assets, and access application that can generate more significant yield. 

### Service Providers:

An application can now determine its security parameters, the involved tokens, logic, and rewards. They no longer need to study the specific platform in detail or hire protocol engineers for each restaking provider. The unified security layer allow applications to purchase security from anywhere ensuring improved security, while they focus on building their core service. 

### Operators:

Operators are actors that opt to provide a range of services. If expenses of running the software themselves becomes unsustainable in the long term, they can consider becomingg a delegator. 

There is opportunity for operators to earn additional income from their existing infrastructure. They have the option to continue running their infrastructure and deploy a few more components: 

- **An Application Oracle:** Operators can provide this service and charge based on the number of requests. Other solo stakers may not run a service binary but still wish to act as a superdelegate.

- **A Threshold Signing Node:** Operators may choose to operate a threshold node and implement various strategies based on their portfolio. They could selectively sign specific service outputs without relying on another oracle.

- **A Keyper Guardian:** They might be interested in becoming a Keyper and participating in the community. Keypers ensure the system's liveness by running and signing on behalf of all services that the community has reviewed. As a Keyper, they can earn a reputation and leverage their experience and influence within the community to guide the network.

They can also now use their other nodes' validator stake to participate in restaking subsystems. Keypers could delegate their Cardano Withdrawal Certificate to shared security, and we can bolster additional systems while continuing to stack yield. 

### Restaking Protocols:

New protocol might lack enough liquidity in their ecosystem for a particular type of application. They usually struggle to bolster decentralised finance (DeFi) in the system. The unified security layer now allows builders to cherry-pick security from other protocols to bootstrap a DeFi ecosystem. 

Current customers have to choose a restaking protocols to run their services. This creates some lock-in in terms of asset variety and restaking protocols may lose out on the opportunity. If the service providers had the choice to have strategies that would allow using multiple restaking protocols, they would commit to both to increase their service security. The unified security layer aims to remove this 'vendor lock-in' for service providers and 'opportunity cost' for restaking protocols.

### End-Users

In a world where any restaked asset from any restaking platform can be utilised, users have the opportunity to have a say in what the underlying asset is protecting the service they are using. This opportunity is akin to 'choosing a chain', but a lot more flexible since the strategy that secures the service can be changed while keeping the service still alive.

Open platforms bolster innovation. In a world where a service can choose from whatever restaking platform, the speed of innovation will increase, leading to many more dApps that the end-users can participate in. 

## What is next?

Nuffle Labs is actively researching on a Unified Security Layer that would benefit the whole industry. We aim to introduce new capital oppurtunities to users and rapidly expand the innovation space for builders. 

'Nuff Security,  Break the silos.
