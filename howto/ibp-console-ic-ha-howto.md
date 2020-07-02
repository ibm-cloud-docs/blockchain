---

copyright:
  years: 2019, 2020
lastupdated: "2020-05-20"

keywords: HA, highly availability, multiregion

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

# Setting up multiregion High Availability (HA) deployments for peers
{: #ibp-console-hadr-mr}



Multiregion HA configuration provides the highest degree of HA coverage that is possible. Deploying peers across multiple geographic regions ensures that if any one region becomes unavailable, the peers in other regions can continue to transact. Note that multiregion HA support for CAs and the ordering service is not currently available.

## Overview
{: #ibp-console-hadr-overview}

To set up multiregion HA support for peers, you need to complete the following tasks:
- Create multiple blockchain service instances that are each bound to a Kubernetes cluster in a different region.
- Use the blockchain console or APIs to create your blockchain nodes in the different regions.
- Use the node export and import actions to manage the nodes from a single console.

## Configuration steps
{: #ibp-console-hadr-config}

To configure multiregion HA by creating redundant peers for each organization, complete the following steps when you configure your blockchain network:

1. Create three Kubernetes clusters in {{site.data.keyword.cloud_notm}}  in the regions you prefer. These clusters can be located in any region you want, although for high performance they should be relatively close together. For example, the regions, East Coast US, and West Coast US, and Canada are better than the regions, West Coast US, London, and Tokyo.
2. Deploy a new {{site.data.keyword.blockchainfull_notm}} Platform instance on the cluster in one of the regions. If you are using a Kubernetes cluster on {{site.data.keyword.cloud_notm}}, you need to link the service instance to the cluster.  Repeat these steps in the second and third regions. When you are finished, you have three separate {{site.data.keyword.blockchainfull_notm}} Platform instances linked to three separate clusters, each in a different region, and three separate consoles.

This tutorial assumes that an ordering service exists with a channel defined that the peers can join.
{: important}

### Step one: Create the peer organization's CA and metadata in cluster one
{: #ibp-console-hadr-peerCA}

1. Deploy a CA in the first cluster by following the instructions in the Build a network tutorial for [Creating your peer organization's CA](/docs/blockchain?topic=blockchain-ibp-console-build-network#ibp-console-build-network-create-CA-org1CA). Make note of the values of the CA **enroll ID** and **secret** because you need to provide those values when you import the CA into other clusters.
2. After the CA tile status indicator is green, `Running`, follow the instructions to [Register identities with your CA](/docs/blockchain?topic=blockchain-ibp-console-build-network#ibp-console-build-network-use-CA-org1). In those instructions, you create two identities, one for the peer and another for the peer's organization admin identity.
3. [Create the peer's organization MSP definition](/docs/blockchain?topic=blockchain-ibp-console-build-network#ibp-console-build-network-create-peers-org1) for the peer's organization in the first cluster.
4. [Create a peer](/docs/blockchain?topic=blockchain-ibp-console-build-network#ibp-console-build-network-peer-create) in the first cluster.
5. [Install your smart contract](/docs/blockchain?topic=blockchain-ibp-console-smart-contracts#ibp-console-smart-contracts-install) on your peer.

Before you can instantiate the smart contract on the channel, you need to follow these steps to join the peer to the channel on the ordering service:
- [Export the peer's organization definition](/docs/blockchain?topic=blockchain-ibp-console-join-network#ibp-console-join-network-add-org2-remote) and share it with an ordering service admin.
- The ordering service admin needs to follow the steps to [import the peer's organization](/docs/blockchain?topic=blockchain-ibp-console-join-network#ibp-console-join-network-import-remote-msp) and add it to the consortium. They will also need to complete the steps in those instructions to export the ordering service to a JSON file.
- [Import the ordering service JSON file](/docs/blockchain?topic=blockchain-ibp-console-join-network#ibp-console-join-network-import-remote-orderer) to your console.
- [Join your peer to the channel](/docs/blockchain?topic=blockchain-ibp-console-join-network#ibp-console-join-network-join-peer-org2).
- Finally, if you use service discovery, private data, and peer gossip, the ordering service admin needs to [configure anchor peers](/docs/blockchain?topic=blockchain-ibp-console-govern#ibp-console-govern-channels-anchor-peers) on the channel. For HA, it is recommended that you add each redundant peer as an anchor peer. That way if one of the peers is unavailable, gossip between organizations can continue.   

Now that the peers are joined to the channel, you can [instantiate the smart contract](/docs/blockchain?topic=blockchain-ibp-console-smart-contracts#ibp-console-smart-contracts-instantiate) on the channel.

### Step two: Export the metadata and identities from cluster one
{: #ibp-console-hadr-export-meta1}

1. Export the CA definition to a JSON file.
   - Open the CA in the **Nodes** tab.
   - Click the download icon to generate the CA JSON file from your browser session.
2. Export the peer's organization MSP definition to a JSON file.
   - Open the MSP definition in the **Organizations** tab.
   - Click the download icon on the tile.
3. Export the peer's organization admin identity from your wallet.
   - Navigate to the **Wallet** tab.
   - Click the peer's organization admin identity and then click **Export identity**.
   - A JSON file that contains the organization admin certificates is created. Make note of the file name and secure the file, because it is required when you create additional peers in your other clusters.

### Step three: Import the metadata and identities in to cluster two and three
{: #ibp-console-hadr-import-meta23}

1. In clusters two and three, [import the CA JSON file](/docs/blockchain?topic=blockchain-ibp-console-import-nodes#ibp-console-import-ca) that you exported from cluster one.  
2. If you will need to use the CA to register other identities, you need to open the imported CA and associate the CA administrator enroll ID and secret that you used when you deployed the CA on cluster one.
3. Import the peer organization MSP definition JSON file that you exported from cluster one.
   - In the **Organizations** tab click **Import MSP definition**.
   - Click **Add file** to upload the JSON file.
   - Click **I have an administrator identity for the MSP definition** checkbox to allow you to use this MSP definition when you create new peers in other zones.
   - Click **Import MSP definition**.
4. Import the organization admin identity JSON file that you exported from cluster one into the console wallet on clusters two and three.
5. In clusters two and three, [import the ordering service JSON file](/docs/blockchain?topic=blockchain-ibp-console-join-network#ibp-console-join-network-import-remote-orderer) to your console.

### Step four: Create new peers in cluster two and three and join a channel
{: #ibp-console-hadr-create-new-peers}

1. In clusters two and three, use the CA to register a new peer user, following the same steps that you took when you registered the peer identity in cluster one. You only need to create the peer users, you do not need to re-create the organization admin identity because you will import that in the next step.
2. Create new peers, by using the CA you that imported from cluster one as the peer's CA. Use the peer organization MSP that you imported from cluster one as the peer organization MSP definition.
3. In cluster two and three, you can now repeat the steps that you ran to join the peers to the same channel as the peer in cluster one. 
4. After you install the smart contract on these redundant peers, they are able to transact and query the ledger.
5. Again, if you plan to use service discovery, private data, and peer gossip, you need to make each redundant peer an anchor peer.  

Your network is now configured such that a failure in any single region will not affect the peer workload.  

To maximize your HA even further, consider the following additional options:
- Export the peers that you created in clusters two and three and import them into the console in cluster one. Then, everything can be managed from a single cluster.
- However, cluster one can fail. Therefore, to further account for a cluster failure, you can import all your peers into each of the three consoles. Then if a cluster containing any one console fails, everything can still be managed from the consoles in the other two clusters.

