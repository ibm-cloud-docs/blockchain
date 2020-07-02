---

copyright:
  years: 2018, 2020
lastupdated: "2020-05-26"

keywords: blockchain components, ca, certificate authorities, peer, ordering service, orderer, channel, smart contract, applications

subcollection: blockchain

---

{:external: target="_blank" .external}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:note: .note}
{:important: .important}
{:tip: .tip}
{:pre: .pre}

# Blockchain component overview
{: #blockchain-component-overview}



The components and structure of the {{site.data.keyword.blockchainfull}} Platform are based on the underlying infrastructure and tools of [Hyperledger Fabric](https://hyperledger-fabric.readthedocs.io/en/release-1.4/){: external}, an open source permissioned blockchain solution to which {{site.data.keyword.IBM_notm}} is a major contributor. Networks based on Fabric include several standard components that can be deployed in a number of configurations to support a wide variety of use cases.

For a more comprehensive overview of Fabric networks and the interrelation of the components that comprise it, see [this document on the structure of a blockchain network](https://hyperledger-fabric.readthedocs.io/en/release-1.4/network/network.html) from the Fabric community documentation, which shows how a network can be started and matured.

For the purposes of this overview, we focus just on certificate authorities (CAs), orderers, peers, smart contracts, and applications. As you can see from the [Build a network tutorial](/docs/blockchain?topic=blockchain-ibp-console-build-network#ibp-console-build-network), this sequence is not arbitrary; it reflects the proper order in which components in a network based on Fabric are deployed.

## Peers
{: #blockchain-component-overview-peer}

At a conceptual level, a blockchain network is comprised mainly of organizations (as organizations decide on how a network is structured, as well as owning nodes and managing identities). At a physical level, however, a blockchain network is comprised primarily of peer nodes that are owned and administered by organizations. Peers are the fundamental elements of the network because they host ledgers and smart contracts (which are contained in ["chaincode"](https://hyperledger-fabric.readthedocs.io/en/release-1.4/developapps/chaincodenamespace.html){: external}), and are therefore where transactions are executed and validated.

More accurately, the peer hosts **instances** of the ledger, and **instances** of smart contracts. Because smart contracts and ledgers are used to encapsulate the shared processes and shared information in a network, these aspects of a peer make them a good starting point to understand what a Fabric network does.

To learn more about peers specifically, check out [Peers](https://hyperledger-fabric.readthedocs.io/en/release-1.4/peers/peers.html){: external} from the Fabric community documentation.

The {{site.data.keyword.blockchainfull_notm}} Platform console allows you to create peers, join them to channels, create anchor peers, install smart contracts, and seamlessly upgrade your peers.

## Certificate Authorities (CAs)
{: #blockchain-component-overview-ca}

You can think of a blockchain network as a series of managed interactions between nodes to serve a defined business use case. For these interactions to be verifiable (to ensure, in other words, who is who) requires identities and a system of permissions that can be checked during each interaction. You can think of the kind of identity that is needed as being similar to a credit card in that it identifies someone within a particular context. Credit cards identify an individual in terms of banking transactions, while Fabric identities allow users to be identified in a blockchain context.

In Hyperledger Fabric, as well as the {{site.data.keyword.blockchainfull_notm}} Platform, this component is the Certificate Authority (CA), which creates identities in the form of x509 certificates as well defining of an organization through the creation of a Membership Services Provider (MSP), which defines the permissions of identities at a component and channel level. These identities can include attributes about them, for example, by linking them to a particular organization or organizational unit (OU).

An organization MSP, for example, has an MSP subfolder called **admins**. Any user whose certificate is inside that admin folder is an admin of the organization. Because this MSP defines the organization, it is listed in the configuration on every channel of which the organization is a member. As a result, whenever an admin of the organization tries to perform an action, the signing certificate of the admin (which is attached to all of its interactions) is checked against the certificates listed in the MSP. Does the certificate match the one listed in the channel configuration? If it does, the other organizations will validate it and the action can be performed. If not, the request to execute the transaction is rejected.

{{site.data.keyword.blockchainfull_notm}} Platform CAs are based on the [Hyperledger Fabric CA](https://hyperledger-fabric-ca.readthedocs.io/en/release-1.4/){: external}, though it is possible to use another CA if it uses a PKI based on x.509 certificates. Because non-Fabric CAs are not configured to create properly formatted MSPs, users who want to use this kind of CA must create the MSP for themselves.

For more information about how certificate authorities are used to establish identity and membership, see [Hyperledger Fabric documentation on identity](https://hyperledger-fabric.readthedocs.io/en/release-1.4/identity/identity.html){: external} and on [membership](https://hyperledger-fabric.readthedocs.io/en/release-1.4/membership/membership.html){: external}.

## Ordering services
{: #blockchain-component-overview-orderer}

Many distributed blockchains, such as Ethereum and Bitcoin, feature systems in which any node can participate in the process where transactions are ordered and bundled into blocks. Because of this fact, these systems rely on probabilistic consensus algorithms that eventually guarantee ledger consistency to a high degree of probability, but are still vulnerable to divergent ledgers (also known as a ledger “fork”), where different participants in the network have a different view of the accepted order of transactions.

Hyperledger Fabric works differently. It features a kind of a node that relies on deterministic consensus algorithms that is called an “ordering node” or an "orderer", that does this transaction ordering. The collection of ordering nodes form the "ordering service".

In addition to promoting finality, separating the endorsement of smart contract execution (which happens at the peers) from ordering gives Fabric advantages in performance and scalability, eliminating bottlenecks that can occur when execution and ordering are performed by the same nodes.

The ordering service performs one other key function: it maintains what is known as the "system channel", a default channel that hosts the "consortium", the list of organizations that can create and join application channels. Because some aspects of the configuration of application channels are inherited by default from the system channel (certain capability levels, for example), it is the job of ordering service admins to establish these defaults. Note however that the ordering service organization can add additional administrating organizations to the system channel.

The {{site.data.keyword.blockchainfull_notm}} Platform uses an implementation of the Raft protocol, in which a leader node is dynamically elected among the ordering nodes in a channel (this collection of nodes is known as the “consenter set”), and that leader replicates messages to the follower nodes. Because the system can sustain the loss of nodes, including leader nodes, as long as there is a majority of ordering nodes (what’s known as a “quorum”) remaining, Raft is said to be “crash fault tolerant” (CFT). In the {{site.data.keyword.blockchainfull_notm}} Platform, users have the ability to select a single node ordering service or a five node ordering service, though it is possible to add or remove nodes from an ordering service and from channels later on. Similarly, it is possible for either one organization or multiple organizations to manage the ordering service and contribute nodes.

For more information about the ordering service, see [The Ordering Service](https://hyperledger-fabric.readthedocs.io/en/release-1.4/orderer/ordering_service.html){: external}.

## Channels
{: #blockchain-component-overview-channels}

A channel is a mechanism that provides a private layer of communication, allowing subsets of network members to create a separate ledger for their transactions. Channels are not just **private** in the sense that network members who are not a part of the channel are unable see the transactions, they are also **secret** in the sense that unless a network member is a part of the channel, they will not even know that the channel exists.

Multiple channels can be created between members, thus supporting one of the [many mechanisms to implement privacy](https://developer.ibm.com/tutorials/cl-blockchain-private-confidential-transactions-hyperledger-fabric-zero-knowledge-proof/){: external}. As discussed in the section on [peers](#blockchain-component-overview-peer), data in a blockchain network is stored on peers in a ledger. Each channel has a separate ledger that holds the transactions (which include configuration transactions) for that channel.

The {{site.data.keyword.blockchainfull_notm}} Platform allows channels to be easily created and managed. Channel configuration updates allow the members of a channel to edit channel parameters to fit their use case. For example, more members can be added to a channel, or the capabilities of a channel can be changed. Because changes to a channel must be approved by channel members, the {{site.data.keyword.blockchainfull_notm}} Platform provides a mechanism for the collection of necessary signatures.

For more information about channels and how to use them, see the [Hyperledger Fabric documentation](https://hyperledger-fabric.readthedocs.io/en/release-1.4/channels.html){: external}.

## Smart contracts
{: #blockchain-component-overview-smart-contracts}

Before businesses can transact with each other, a common understanding about rules and processes must be reached and defined. Taken together, these contracts lay out the "business model" that governs all of the interactions between business partners.

The same need exists in blockchain networks. The industry term for these business models on blockchain networks is "smart contracts". Fabric networks contain these contracts in a larger structure that is known as "chaincode", which includes not just the business logic but the underlying infrastructure that validates the identities of the users that attempt to invoke the smart contract.

Where contracts in the business world are signed and filed with law firms, smart contracts are installed on peers and "instantiated" on a channel.

For more information about smart contracts, see [Smart contracts](https://hyperledger-fabric.readthedocs.io/en/release-1.4/smartcontract/smartcontract.html){: external}.

## Applications
{: #blockchain-component-overview-applications}

Client applications in a Fabric-based network like {{site.data.keyword.blockchainfull_notm}} Platform leverage underlying infrastructures such as APIs, SDKs, and smart contracts to allow client interactions (invokes and queries) at a higher level of abstraction.

For a look at how applications interact with a network based on Fabric, check out the [Developing Applications](https://hyperledger-fabric.readthedocs.io/en/release-1.4/developapps/developing_applications.html){: external} topic in the Hyperledger Fabric documentation. You can also view the [creating applications](/docs/blockchain?topic=blockchain-ibp-console-app#ibp-console-app) topic to learn how to connect your applications to {{site.data.keyword.blockchainfull_notm}} Platform.

## An example network
{: #blockchain-component-overview-example-network}

**Figure 1** depicts an example of a deployed blockchain network that consists of four organizations, Org A, Org B, Org C, and Org D. Each organization has their own Certificate Authority that is responsible for distributing cryptographic identity material. There is also an ordering service with five Raft nodes that defines policies and network participants. Channel X includes all four organizations, but Channel Y is restricted to Org C and Org D.  Lastly, client applications in possession of a properly signed x509 certificate can send calls to their associated peers on the network.

![Blockchain Network](images/blockchain_network_2-01.svg "Example blockchain network"){: caption="Figure 1. An example blockchain network with four members that leverage channels to isolate data" caption-side="bottom"}

