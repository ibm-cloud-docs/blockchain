---

copyright:
  years: 2014, 2023
lastupdated: "2023-02-17"

keywords: IBM Blockchain, IBM Blockchain Platform, terms, Fabric, Raft, CouchDB, consortium, MSP

subcollection: blockchain

---



{{site.data.keyword.attribute-definition-list}}



# Glossary
{: #glossary}



This topic defines {{site.data.keyword.blockchainfull}} Platform-specific terms that appear in this documentation. For a deeper understanding of terms, and for a glossary of terms that relate to Hyperledger Fabric concepts, refer to the [Hyperledger Fabric glossary](https://hyperledger-fabric.readthedocs.io/en/release-2.2/glossary.html){: external}.
{: shortdesc}

## Approve
{: #glossary-propose}

Referring to a step in the Hyperledger Fabric v2.x smart contract lifecycle, a smart contract definition is approved by organizations on the channel. After the number of approvals that are specified in the channel lifecycle endorsement policy are satisfied, the smart contract definition can be committed to a channel, and the peers where it is installed can begin to process requests.

## Asset
{: #glossary-asset}

Tangible or intangible goods, services, or property that are represented as an item that is traded on the blockchain network.

## Block
{: #glossary-block}

An ordered set of transactions, which is cryptographically linked to the preceding block on a channel.

## CA
{: #glossary-CA}

An abbreviation of "Certificate Authority", this is the component that issues certificates to all the participating members. These certificates represent a member’s identity. All entities in the network (peers, ordering nodes, clients, and so on) must have an identity to communicate, authenticate, and ultimately transact. These identities are required for any direct participation in the blockchain network.

## Chain
{: #glossary-chain}

The ledger’s chain is a transaction log structured as hash-linked blocks of transactions. Peers receive blocks of transactions from the ordering service, mark the block’s transactions as valid or invalid based on endorsement policies and concurrency violations, and append the block to the hash chain on the peer’s file system.

## Chaincode
{: #glossary-chaincode}

Where **smart contracts** contain the business logic that governs the interactions for a set of assets, "chaincode" describes the larger set of packages and infrastructure that encompass the smart contract.

## Channel
{: #glossary-channel}

Consisting of a subset of network members who want to transact privately. Channels provide data isolation and confidentiality by allowing the members of a channel to establish specific rules and a separate ledger that only channel members can access. Peers, which are the nodes that function as transaction endpoints for organizations, are joined to channels.

## Client
{: #glossary-client}

The client represents the entity that acts on behalf of a user. It must connect to a peer for communicating with the blockchain. The client might connect to any peer of its choice. Clients create and thus invoke transactions. The client submits an actual transaction invocation to the endorsers, and broadcasts transaction proposals to the ordering service.

## Commit
{: #glossary-commit}

Referring to a step in the Hyperledger Fabric v2.x smart contract lifecycle, after the number of approvals that are specified in the channel lifecycle endorsement policy are satisfied, the smart contract definition can be committed to a channel, and the peers where it is installed can begin to process requests.

## Connection profile
{: #glossary-connection-profile}

A connection profile is used by the Fabric Client SDKs to connect to the network. A connection profile can be downloaded from the [Organizations tile](/docs/blockchain?topic=blockchain-ibp-console-organizations#ibp-console-organizations-connx-profile).

## Consensus
{: #glossary-consensus}

A collaborative process to keep the ledger transactions synchronized across the network. Consensus ensures that ledgers are updated only when the appropriate participants approve transactions, and that ledgers are updated with the same transactions in the same order. There are many different algorithmic ways of achieving consensus.

## Consenter set
{: #glossary-consenter}

The ordering service nodes actively participating in the ordering process on a channel. These nodes are also known as "consenters."

## Console
{: #glossary-console}

The name of the user interface in the {{site.data.keyword.blockchainfull_notm}} Platform. The console allows users to view, create, and manage their deployments. Because the public and private keys are only stored locally in the browser the console runs on, users maintain total control over their keys.

## Consortium
{: #glossary-consortium}

The group of non-orderer organizations listed on the orderer system channel. *The following lines do not apply to orderers that do not use a system channel:* These are the only organizations that can create channels. At channel creation time, all organizations added to the channel must be part of a consortium. However, an organization that is not defined in a consortium may be added to an existing channel. While a blockchain network can have multiple consortia, most blockchain networks have a single consortium.

## CouchDB
{: #glossary-couchdb}

A document store that can be used as the state database of your peers. Using CouchDB allows you to use rich queries and deploy indexes with your smart contract.

## Current state
{: #glossary-current-state}

The current state of the ledger represents the latest values for all keys that are ever included in its chain transaction log. Because current state represents all latest key values known to the channel, it is sometimes referred to as the **world state**. Smart contracts execute transaction proposals against current state data. The current state changes every time when the value of a key changes or a new key is added and is critical to a transaction flow because the latest key-value pair must be known before it can be changed. Peers commit the latest values to the current state of the ledger for each valid transaction in a block. The current state is stored the state database associated with a peer.

## Dynamic membership
{: #glossary-dynamic-memership}

A member can be dynamically added to the network by a user with **registrar** privilege. Members are also assigned roles and attributes, which control their access and authority on the network. Neither roles nor attributes can be assigned dynamically though. Hyperledger Fabric supports the addition or removal of members, peers, and ordering service nodes, without compromising the operations of the overall network. Dynamic membership is critical when business relationships adjust and entities need to be added or removed for various reasons.

## Endorsement
{: #glossary-endorsement}

The process by which transactions are validated. Endorsement rules are implemented by specifying endorsement policies, which define the organizations that must approve of a transaction.

## Endorsement policy
{: #glossary-endorsement-policy}

Defines the peer nodes on a channel that must execute transactions that are attached to a specific application, and the required combination of responses (endorsements). A policy could require that a transaction be endorsed by a minimum number of endorsing peers, a minimum percentage of endorsing peers, or by all endorsing peers that are assigned to a specific application. Policies can be curated based on the application and the desired level of resilience against misbehavior, whether deliberate or not, by the endorsing peers. A submitted transaction must satisfy the endorsement policy before it is marked as valid by committing peers. A distinct endorsement policy to deploy transactions is also required.

## Genesis block
{: #glossary-genesis-block}

The configuration block that initializes a blockchain network or channel, and also serves as the first block on a chain.

## Gossip
{: #glossary-gossip}

Hyperledger Fabric allows peers to gather important network information from each other without having to rely on the ordering service. The [gossip data dissemination protocol](https://hyperledger-fabric.readthedocs.io/en/release-2.2/gossip.html){: external} provides a secure, reliable, and scalable way for peers to exchange messages between each other. For example, if peers miss some blocks because of delays, network outages, or other reasons, they can sync up to the current ledger state by using gossip messaging to contact other peers in possession of these missing blocks.

## HSM
{: #glossary-hsm}

Hardware Security Module. Provides on-demand encryption, key management, and key storage as a managed service. HSM is a physical appliance that handles the resource-intensive tasks of cryptography processing and reduce latency to applications. For more information, see [Hardware Security Module](https://www.ibm.com/cloud/hardware-security-module){: external}.

## Hyperledger Fabric
{: #glossary-hyperledger-fabric}

[Hyperledger Fabric](https://hyperledger-fabric.readthedocs.io/en/release-2.2/){: external} is a business blockchain framework that the Linux Foundation hosts to serve as a foundation for developing blockchain applications or solutions with a modular architecture. Hyperledger Fabric components such as consensus and membership services are plug-and-play.

## Install
{: #glossary-install}

The process of placing a smart contract on a peer’s file system. You must install the smart contract on every peer that wants to access or change the data governed under the smart contract.

## Instantiate
{: #glossary-instantiate}

The process of starting and initializing a smart contract container on a channel with peers running Fabric v1.4.x and v1.4 capabilities. After a smart contract is installed on the peers and every peer has joined the channel, the smart contract must be instantiated on the channel. Instantiation performs any necessary initialization of the smart contract, which includes setting the key value pairs that comprise a smart contract's initial world state. After instantiation, peers that have the smart contract installed can interact with the data governed by the smart contract. Starting in Fabric v2.x, the instantiation process has been replaced with a [propose, approve, and commit model](/docs/blockchain?topic=blockchain-ibp-console-smart-contracts-v2).

## Kafka
{: #glossary-kafka}

A consensus plug-in implementation for Hyperledger Fabric that results in a cluster of ordering service nodes in the blockchain network. Kafka implementations and Raft implementations are intended for production networks. However, only Raft ordering service clusters are natively supported and can be created using the {{site.data.keyword.blockchainfull_notm}} Platform.

## Ledger
{: #glossary-ledger}

Comprised of a literal "chain of blocks" that store the immutable, sequenced record of transactions, as well as a state database to maintain current state. There is one ledger per channel, and updates to it are managed by the consensus process according to the policies of a particular channel.

## LevelDB
{: #glossary-leveldb}

A key-value store that can be used as an option for the state database for your peers. LevelDB stores the current state as key-value pairs, and does not support the use of indexes or rich queries.

## Lifecycle endorsement policy
{: #glossary-lifecycle-ep}

On a channel, the lifecycle endorsement policy specifies which organizations can approve a smart contract definition and how many of them are required to approve the definition or update. The default lifecycle endorsement policy requires a majority of all organizations on the channel to approve.

## Member
{: #glossary-member}

Also known as "organizations", members in a blockchain network, similar to the members of any group, form the structure of the network. A member can be as large as a multi-national corporation or as small as an individual. Members are enrolled into the network with a certificate that grants them permissions to use the network as either a service provider (for example, issuing certificates, validating/ordering transactions) or as a consumer. The former provides foundational blockchain services that include transaction validation, transaction ordering, and certificate management services. Consumer members use the network to invoke transactions against the distributed ledger. Members can have multiple Peers.

## MSP
{: #glossary-msp}

An abbreviation of **Membership Service Provider**, which provides the definition of an organization, including the root certificate of the CA issuing certificates for the entities associated with that organization, as well as the sign cert of the admin of that organization. MSPs also exist at the local level of a peer or ordering node, and are the authentication mechanism that verifies the admin users of the node. In the {{site.data.keyword.blockchainfull_notm}} Platform, MSPs can be exported from one console into another, allowing users to create an organization in one console, import it to another console, and operate it (for example, to create a channel). MSPs can also be imported into an ordering service, forming a "consortium", the list of organizations allowed to create and join channels.

## Network
{: #glossary-network}

An instance of an {{site.data.keyword.blockchainfull_notm}} Platform service on {{site.data.keyword.cloud_notm}}.

## Node
{: #glossary-node}

The communication entity of the blockchain. There are three types of nodes: CA, peer, and ordering node.

## Ordering node
{: #glossary-orderer}

The node that collects transactions from network members, orders the transactions and bundles them into blocks. Also known as orderer. These blocks are then distributed to peers, which then verify the blocks and add them to the ledgers on each channel. Ordering nodes contain the cryptographic identity material that is tied to each member and authenticate the identity of clients and peers to access the network. The overall function that an ordering node or collection of nodes provides is known as the **ordering service**.

## Organization
{: #glossary-organization}

See [Member](#glossary-member).

## Out of band
{: #glossary-out-of-band}

An expression used to refer to sharing network artifacts outside of the console UI, for example by email or some other file transfer mechanism. After submitting a smart contract proposal, the originator can share the smart contract package and package id with other channel members in an out of band operation by emailing this information to them.

## Peer
{: #glossary-peer}

A blockchain network resource that provides the services to execute and validate transactions, and maintain ledgers. The peer runs smart contract and is the holder of transaction history and the current state of assets on ledgers. They are owned and managed by organizations and are joined to channels.

## Propose
{: #glossary-propose}

Referring to a step in the Hyperledger Fabric v2.x smart contract lifecycle, a smart contract definition is proposed to a channel for approval by the organizations that are specified in the channel lifecycle endorsement policy. A smart contract proposal contains a smart contract definition name, version, package, endorsement policy, and optionally a private data collection.

## Quorum
{: #glossary-quorum}

In a Raft ordering service, a quorum represents the number of nodes that must be available for transactions to be processed. This number is a majority of the total number of nodes in the consenter set of the channel. In other words, if you have one node, you need that node available to have a quorum, because the majority of one is one. Similarly, if you have two nodes, you will need both available, since the majority of two is two (for this reason, a configuration of two nodes is discouraged; there is no advantage to a two node configuration). In a similar vein, the majority of three is two, the majority of four is three, the majority of five is three, and so on.

## Raft
{: #glossary-raft}

Raft is a crash fault tolerant (CFT) ordering service based on an implementation of [Raft protocol](https://raft.github.io/raft.pdf){: external} in `etcd`. Raft follows a “leader and follower” model, where a leader node is elected (per channel) and its decisions are replicated by the followers. Raft ordering services should be easier to set up and manage than Kafka-based ordering services and a cluster of these nodes can be created using the {{site.data.keyword.blockchainfull_notm}} Platform.

## SDK
{: #glossary-sdk}

Hyperledger Fabric Client Software Development Kits (SDKs) enable interaction between your client application and your blockchain network. Fabric supports three  SDKs: [Go](https://github.com/hyperledger/fabric-sdk-go){: external}, [Node](https://github.com/hyperledger/fabric-sdk-node){: external}, and [Java](https://github.com/hyperledger/fabric-sdk-java){: external}) that include documentation for the available APIs.

## Service credentials
{: #glossary-service-credentials}

Service credentials are in JSON format and contain the API endpoint information and enrollIDs/secrets for your network resources, that is, CAs, ordering nodes, and peers. Your application interacts with network resources through these API endpoints.

## Service discovery
{: #glossary-sd}

Fabric [service discovery](https://hyperledger-fabric.readthedocs.io/en/release-2.2/discovery-overview.html){: external} allows your client applications to dynamically find the peer and ordering endpoints of your network. If you do not configure service discovery in your application, the endpoint information of peer and ordering nodes on your channel needs to be added manually to your connection profile or your application.

## Shim
{: #glossary-shim}

When referring to smart contracts or chaincode, the shim represents a set of Hyperledger Fabric chaincode APIs that a smart contract can use to access state variables, transaction context, and call other smart contracts.

## SignCert
{: #glossary-sign-cert}

The certificate that any entities, whether organizations or admins, attach to their proposals or proposal responses. These signCerts are unique to an entity and are checked by the ordering service to make sure they match the signCert on file for that entity.

## Smart contracts
{: #glossary-smart-contracts}

See [Chaincode](#glossary-chaincode).

## Smart contract definition
{: #glossary-smart-contract-def}

Used by the Hyperledger Fabric v2.x smart contract lifecycle, the smart contract definition contains the elements that members of an organization agree to before a smart contract can be committed to a channel. The definition is composed of a smart contract definition name, version, endorsement policy, and private data collection. Changes to the version, endorsement policy, and private data collection must be approved by the organizations that are specified in the channel lifecycle endorsement policy before the updated version can become active on the channel.

## Smart contract endorsement policy
{: #glossary-transaction-ep}

See [Endorsement policy](#glossary-endorsement-policy).

## Smart contract package
{: #glossary-smart-contract-pkg}

Used by the Hyperledger Fabric v2.x smart contract lifecycle, the smart contract package contains the business logic that reads from or writes to the smart contract ledger. The package is attached to a smart contract definition proposal. Smart contract packages can only be installed on peers that are running a Fabric v2.x or higher image.

## State database
{: #glossary-state-database}

Current state data is stored in a database on the peers for efficient reads and queries from smart contracts. You can select to use either LevelDB or CouchDB as the state database of your peers.

## Transaction
{: #glossary-transaction}

Changes to the ledger are achieved by invoking "transactions", which either involve assets or changes to the configuration of a channel. "Asset" transactions usually involve a smart contract governing the rules of a transaction between counter-parties, while the rules governing configuration transactions are established in the configuration of the channel itself.

## User
{: #glossary-user}

A user is a participant in a blockchain network that has indirect access to the ledger through a trust relationship with a Certificate Authority.

## World state
{: #glossary-world-state}

See [Current state](#glossary-current-state).
