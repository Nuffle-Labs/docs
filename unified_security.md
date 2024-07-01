# A Unified Security Layer

*Authors: Don Dall, Firat Sertgoz*
## Abstract
We are seeing a Cambrian explosion of restaked security systems pioneered by EigenLayer. These systems improve the cryptoeconomic security of protocols and applications by **facilitating the attestation from networks verifying the blockchain itself.** Each restaking provider has different semantics specific to their network. These semantics are hard to understand for the higher-level purchaser or delegate, and sometimes they are irrelevant, where services only care about borrowing security to do some productive work. 

![GQ7_9xbXcAEm34A](https://hackmd.io/_uploads/rklNfFY8A.jpg)
*[Reference](https://x.com/gauntlet_xyz/status/1805662991708213452/photo/1)*

Cryptoeconomic security is already scarce. Different restaking providers across blockchains fragment cryptoeconomic security further. For restaking to become mainstream, applications need to share security subsystems, experiment with different providers, and potentially even combine the use of one provider with another to improve their cryptoeconomic security -- all depending on supported features, ecosystem alignment and appetite for risk.

In this blogpost, we outline the case and the design for a unified security layer that enables service providers to leverage any crypto-economic security from any restaking platform.

## The Case for Unification

The growing number of restaking protocols are fragmenting available cryptoeconomic security. Without unification,
participatipants in restaking will suffer a variety of problems. Unification allows new features that are otherwise impossible with a single restaking provider. Unification solves for the following problems: 

- **Not enough stake**: A service could require a large amount of borrowed security from many operators. The total required stake might not be present in one restaking protocol but might exists in multiple restaking protocols. Currently there is no way to combine these restaking protocols, thus services with large stake requirements cannot be onboarded.

- **Asset Variety**: An institutional operator generally has multiple native assets and are validators to many blockchain protocols. This opportunity represents that they can attest to many different systems and provide cryptoeconomic security all at once to a service. A unification layer would allow node-operating businesses to delegate once. The system, in this case, can support and attest all supported assets from all restaking providers. A service that operates in multiple chains could require different native assets to secure its service. A unification layer would allow defining multi-chain security strategies with native assets with already existing restaking protocols..

- **Missed opportunity**: Blockchain protocol without a native restaking layer miss the opportunity to provide cryptoeconomic security to decentralised services. Builders need to build expertise in each restaking system they want to support. This introduces opportunity cost, risk, and auditing requirements for the protocol. A unification layer would allow native delegates to lock up their stake and operators to lock their validators to a smart contract on the native chain. They can, then, delegate to the unification layer to attest for them based on their strategy. 

- **Semantics of Restaking Protocols**: Every restaking protocol defines its interface that service providers must adapt and integrate. This creates a bottleneck in value generation since service providers want to reach their customers as soon as possible with various assets. A unification layer significantly reduces the time to market for all decentralised services. 


## Design space

There are a few ways to support unified interoperability with the systems:

* **Provide a common interface** for the commissioning and consumption of security attestations for all providers. This approach is likely a long-term solution; however, since we are at an early stage, it would be helpful to have room for innovation and solve problems uniquely. 
* **Introduce a unification layer** for the agents in each security system to pool their attestations and aggregate them. This system would utilize the expertise of builders who understand the restaking paradigm and limit the burden on the external actors who want to purchase security, freeing them up to concentrate on their value generation. For operators, allowing them to become delegates means they can delegate all of their assets to a trustless operator set. Utilize account abstraction to ensure the attestation is delegated to the unification layer.

## Unified Security Layer

We introduce a unified security layer that enables service providers to leverage any crypto-economic security from any restaking platform. Service providers only need to provide the service binary, validation semantics of their service, and their configuration. Unified security layer abstracts the orchestration of the services with different restaking protocols.

**High-level Architecture Diagram:**

Below is our initial birds eye view of the architecture.

![final big](https://hackmd.io/_uploads/BJMezZ38R.png)

**Restaking platform interaction Diagram:**

The below diagram is zoomed in to represent the interaction flow for different actors.

![final_flow](https://hackmd.io/_uploads/BylGz-hIR.png)


## What is in it for existing users?

The answer to this question depends on the type of participant asking.

### Delegators:

Delegators can now delegate all their assets on one unified platform and interact in one place. There is no need to search for services and deal with operators to build trust. Operators can earn trust in a unified system, and you can transparently see an operator's reputation stake and network participation.

Delegators can set adaptive strategies and have power over their assets. As the system is an open market, they can access GTM-style applications and earn the most significant yield, which is usually reserved for Genesis operators.

### Service Providers:

Due to the competitive marketplace, an application can now determine its security parameters, the tokens involved, slashing logic, and rewards, and then be at ease with it. You do not need to delve deeper into the specific platform or hire protocol engineers for each re-staking solution. 
With this, you can set and forget it, ensure security, and continue innovating.

### Operators:

Consider becoming a delegator if the expenses of running all the validator software are unsustainable in the long term, or if they operate a node enterprise and aim to cut costs without depending on Infura.

If operators want to continue making money from their existing infrastructure, they have the option to run the infrastructure and also deploy a few components:

* An Application Oracle: Operators can provide this service and charge based on the number of requests. Other solo stakers may not run a service binary but still wish to act as a superdelegate.
* A Threshold Signing node: Operators may choose to operate a threshold node and implement various strategies based on their portfolio. They could selectively sign specific service outputs without relying on another oracle.
* A Keyper Guardian: They might be interested in becoming a keyper and participating in the community. Keypers ensure the system's liveness by running and signing on behalf of all services that the community has reviewed. As a keyper, they can earn a reputation and leverage their experience and influence within the community to guide the network.

They can also now use their other nodes' validator stake to participate in restaking subsystems. Keypers could delegate their Cardano Withdrawal Certificate to shared security, and we can bolster additional systems while continuing to stack yield. 

### Restaking Protocols:

Protocols that are new might not have enough liquidity in their ecosystem for a particular type of application and are struggling to bolster DeFI in the system. With this, protocols can now cherry-pick security from others while bootstrapping a budding DeFI space. 

Current customers have to choose a restaking protocols to run their services. This creates some lock-in in terms of asset variety and restaking protocols may lose out on the opportunity. If the service providers had the choice to have strategies that would allow using multiple restaking protocols, they would commit to both to increase their service security. With Unified security we aim to remove this 'vendor lock-in' for service providers and 'opportunity cost' for restaking protocols.

### End-Users

In a world where any restaked asset from any restaking platform can be utilised, users should have the oppurtunity to have a say in what the underlying asset is protecting the service they are using. This oppurtunity is akin to 'choosing a chain', but a lot more flexible since the strategy that secures the service can be changed while keeping the service still alive.

Open platforms bolster innovation. In a world where a service can choose from whatever restaking platform, the speed of innovation will increase, leading to many more dApps that the end-users can participate in. 

## What is next?

Nuffle Labs is actively researching on a Unified Security Layer that would benefit the whole industry. We aim to introduce new capital oppurtunities to users and rapidly expand the innovation space for builders. Nuff' Security,  Break the silos.

