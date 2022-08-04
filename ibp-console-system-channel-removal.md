
copyright:
  years: 2022
lastupdated: "2022-08-04"

keywords: tutorials, how-to, learn

subcollection: blockchain-sw-253

---

{:external: target="_blank" .external}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:note: .note}
{:important: .important}
{:tip: .tip}
{:term: .term}
{:pre: .pre}


# System channel removal
{: #ibp-howto-remove-system-channel}

Hyperledger Fabric v2.2 introduced the capability to create application channels, using [Channel Participation APIs](https://github.com/hyperledger/fabric/blob/main/swagger/swagger-fabric.json). 
Prior to v2.2, all application channels were created with transactions on the system channel. 
Legacy system channels are therefore now obsolete, and can (and should) be removed for improved security and performance. 
The Fabric Operations Console has added support for system channel removal, as of version v1.0.3-25.

With the latest Fabric Operations Console you can:

- Manage ordering nodes (aka orderers) that are not on a system channel
- Manage application channels that were created with Channel Participation APIs
- Remove the system channel (and application channels) from ordering nodes (aka unjoin a channel)

System channel support remains, however removing the system channel is recommended.

**Attention** If you remove the system channel from an existing network, and you use Node.js chaincode,
you should upgrade in stages - see [Upgrading Node.js chaincode](#upgrade-nodejs-chaincode) below for details.

**Target audience:** This topic is designed for architects and system administrators who are responsible
for planning, configuring, and installing {{site.data.keyword.blockchainfull_notm}} 2.5.3. Application
channels are identical regardless of the creation method used.

## Benefits
{: #benefits-removal-system-channel}

Operating without a system channel has the following benefits:

**Increased privacy** - A system channel is a shared entity for all organizations on the network. It is therefore impossible to hide the existence of application channels, and their member organizations, from other members. By contrast, operating without a system channel obscures all channel names and their members from other member organizations.

**Increased performance** - A system channel consumes a small amount of CPU, networking and storage resources that can be instead utilized for application transactions. The capability to unjoin channels also enables optimizing ordering nodes, by recovering CPU, storage and networking resources from application channels that are no longer needed.

**Enhanced channel maintenance** - The channel participation APIs are lighter weight, easier to use, and deliver additional features, resulting in reduced code requirements for managing channel policies.

### Systemless orderers details
{: #create-systemless-orderers}

This section, for network administrators, describes the details of an ordering service **without** a system channel. 
When a system channel **is** used for creating an ordering service, each ordering node receives a configuration block that is generated prior to creating the orderer container. 
Each ordering node gets this configuration block to bootstrap it and bring it online. By contrast, when using systemless ordering nodes this process has changed.

With no system channel, an orderer is simply created and started. 
It does not wait for a genesis block before becoming online. 
The node will be online immediately and the channel participation APIs are available to use. 
Once an orderer has joined a channel via a channel participation API request it will be considered either a `consenter` or `follower` of the application channel. 
A `consenter` has its TLS certificate listed in the `consenters` field in the channel's configuration block, which authorizes the node to participate in the ordering of transactions. 
A `follower` ordering node has joined an application channel, but is not listed in the consenters field and cannot order transactions. 
Follower nodes can download the ledger to enable data redundancy without additional consenter networking and communication overhead. 
Note that a follower orderer can always be upgraded to a consenter orderer, and vice versa, by editing the application channel's config block.

Removal of the system channel means that the legacy concept of orderering nodes belonging to a centralized group (the consortium of network organizations) is obsolete. Orderering nodes without a system channel are a much more loosely grouped entity. 
They may or may not be members of the same channels, and each orderer can choose to join or unjoin any application channel. 
This also means that appending orderers to an existing ordering cluster, and creating the ordering cluster from scratch, now both follow the same path. 
Which is to create and start the orderers and then join them to an application channel.

## Removing an existing system channel
{: #remove-system-channel}

The process of removing an existing system channel is to remove the system channel configuration from each ordering node. 
You should first place the system channel into maintenance mode (step 2 below) to prevent any application channels from being created while the system channel is being removed. 
You will then unjoin the system channel from each orderer node (step 3 below) using the Fabric Operations console, as follows:

1. Ensure that each ordering node in the Ordering Service is running Fabric v2.4.x or higher
1. Place the system channel into maintenance mode, using the Fabric Operations console UI, as follows:
    - From the **Nodes** tab, click the Ordering cluster tile
    - Click the **Ordering nodes** tab
    - Click the **settings** (gear) icon
    - Click the **Maintenance** button
    - Follow the prompts to turn maintenance mode on
1. Remove the system channel from all ordering nodes, using the Fabric Operations console UI, as follows:
    - From the **Nodes** tab, click the Ordering cluster tile
    - Remain on the **Details** tab
    - Click the **unjoin (trash can)** icon on the system channel tile
    - Follow the prompts to unjoin each ordering Node.js from the system channel
1. Complete the process by upgrading the Node.js chaincode from Peer v1.4.x to v2.4.x

## Create a TLS identity
{: #create-tls-id}

After you click the ordering cluster tile you may see a warning about missing a TLS identity.
This identity is a different type than we've used in the past and is only need for orderers that do not have a system channel.
This identity is actually the same identity the orderer is using when it enrolls on startup against the TLS CA.
To get this identity:

1. From the **Nodes** tab, select the CA tile that this orderer cluster used (during create)
1. Find the row for the identity this orderer used and click the dot dot dot
1. On the enroll wizard select the * **TLS CA** * in the CA drop down (**its important to select the TLS CA**)
1. Type the enroll secret that was used earlier when creating the orderer
1. Follow any other directions in the enroll wizard and click submit
1. If you browse back to the nodes page and select the Orderer Cluster tile the TLS identity error should be gone

## Upgrading Node.js chaincode
{: #upgrade-nodejs-chaincode}

You may need to be cautions about upgrading peers and orderers to use the system channel removal features.
Hyperledger Fabric v2.4 updated its chaincode version of Node.js to v16 and removed support for the earlier Node.js v12. 
If you are upgrading your peers and orderers to use the channel participation APIs, and are using Node.js v12 based chaincode, you must also incrementally upgrade the Node.js chaincode, as follows:

1. Upgrade any v1.4.x peers to v2.2.x (note that earlier Node.js v12 chaincode will continue to work, because Fabric v2.2.x provides both Node.js environments (v12 and v16)
1. Update your chaincode locally to use a Node.js v16.14.2 (or later v16.x.x) environment
1. Perform a [Fabric chaincode upgrade](https://hyperledger-fabric.readthedocs.io/en/release-2.2/chaincode_lifecycle.html#upgrade-a-chaincode) on each channel where this chaincode is used
1. Upgrade your v2.2.x peers to Fabric v2.4.x
