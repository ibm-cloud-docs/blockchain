---

copyright:
  years: 2014, 2023
lastupdated: "2023-08-08"

keywords: smart contract, private data, private data collection, anchor peer

subcollection: blockchain

---



{{site.data.keyword.attribute-definition-list}}



# Deploy a smart contract
{: #ibp-console-smart-contracts}



**ATTENTION!! IBM Blockchain Platform SaaS Edition has been replaced by IBM Support for Hyperledger Fabric!!** IBM Blockchain Platform SaaS Edition will no longer be supported after **July 31, 2023**. Customers have been directed to migrate their networks by July 31, 2023. After this date, IBM Blockchain Platform SaaS networks that are not migrated to IBM Support for Hyperledger Fabric will be at risk for potential security vulnerabilities. A migration tool is provided from your console, and the disruption to your network is minimal. See [Migrating to IBM Support for Hyperledger Fabric](/docs/blockchain?topic=blockchain-migrating-to-hlf-support) for details.

A smart contract is the code, packaged as chaincode, that applications interact with to read and update data on the blockchain ledger. A smart contract turns business logic into an executable program that is agreed to and verified by all members of a blockchain network. This tutorial is the third part in the [sample network tutorial series](#ibp-console-smart-contracts-structure) and describes how to deploy smart contracts to start transactions in the blockchain network.
{: shortdesc}

**Target audience:** This topic is designed for network operators who are responsible for creating, monitoring, and managing the blockchain network. Additionally, application developers might be interested in the sections that reference how to create a smart contract.

## Sample network tutorial series
{: #ibp-console-smart-contracts-structure}

You are currently on the third part of our three-part tutorial series. This tutorial guides you through the process of using the console to deploy a smart contract onto a channel in your {{site.data.keyword.blockchainfull_notm}} Platform network.

* [Build a network tutorial](/docs/blockchain?topic=blockchain-ibp-console-build-network#ibp-console-build-network) guides you through the process of hosting a network by creating an orderer and peer.
* [Join a network tutorial](/docs/blockchain?topic=blockchain-ibp-console-join-network#ibp-console-join-network) guides you through the process of joining an existing network by creating a peer and joining it to a channel.
* **Deploy a smart contract on the network** (Current tutorial) Provides information on how to write a smart contract and deploy it on your network.

You can use the steps in these tutorials to build a network with multiple organizations in one cluster for the purposes of development and testing. Use the **Build a network** tutorial if you want to form a blockchain consortium by creating an orderer node and adding organizations. Use the **Join a network** tutorial to connect a peer to the network. Following the tutorials with different consortium members helps you create a truly **distributed** blockchain network.

Select the tutorial that corresponds to your channel configuration:
- ![version 2.x](images/2-x_Pill.png "version 2.x")*** [Channel application capabilities and peer Fabric images are 2.x](/docs/blockchain?topic=blockchain-ibp-console-smart-contracts-v2)
- ![version 1.4](images/1-4_Pill.png "version 1.4") [Channel application capabilities are 1.4 and peer Fabric images are 1.4 or 2.x](/docs/blockchain?topic=blockchain-ibp-console-smart-contracts-v14)

*** Fabric v2.0 introduced a new distributed process to manage the lifecycle of a smart contract that allows for decentralizing the governance of smart contracts on a channel. Whenever possible, it is recommended that customers should move to using the new smart contract lifecycle to avoid any interruption of service in later upgrades when Fabric no longer supports the v1.4 process for installing and instantiating smart contracts.
{: important}
