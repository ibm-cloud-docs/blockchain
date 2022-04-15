---

copyright:
  years: 2014, 2022
lastupdated: "2022-04-15"

keywords: release note, latest changes, Hyperledger Fabric

subcollection: blockchain

---



{{site.data.keyword.attribute-definition-list}}

# Release notes
{: #release-notes-saas-20}

Use these release notes to learn about the latest changes to {{site.data.keyword.blockchainfull}} Platform for {{site.data.keyword.cloud_notm}} built on Hyperledger Fabric [v1.4.12](https://github.com/hyperledger/fabric/releases/tag/v1.4.12) and [v2.2.5](https://github.com/hyperledger/fabric/releases/tag/v2.2.5).
{: shortdesc}



[Installing patches](/docs/blockchain?topic=blockchain-ibp-console-manage-console#ibp-console-manage-patch) provides instructions on how to apply patches to your existing blockchain nodes. Patches are cumulative, so select the latest available patch to include all earlier patches.

## 05 Apr 2022
{: #05-04-2022}

**Certificate Authority (CA) patch 1.5.2-8, Peer and ordering node patch 1.4.12-11, 2.2.5-3.**

Miscellaneous bug fixes and security patches.

Severity: medium

## 08 Mar 2022
{: #08-03-2022}

**Certificate Authority (CA) patch 1.5.2-7, Peer and ordering node patch 1.4.12-10, 2.2.5-2.**

Miscellaneous bug fixes and security patches.

Severity: medium

## 08 Feb 2022
{: #08-02-2022}

**Certificate Authority (CA) patch 1.5.2-6, Peer and ordering node patch 1.4.12-9, 2.2.5-1.**

Miscellaneous bug fixes and security patches.

Severity: medium

## 11 Jan 2022
{: #11-01-2022}

**Certificate Authority (CA) patch 1.5.2-5, Peer and ordering node patch 1.4.12-8, 2.2.4-5.**

Miscellaneous bug fixes and security patches.


## 14 Dec 2021
{: #14-12-2021}

**Certificate Authority (CA) patch 1.5.2-4, Peer and ordering node patch 1.4.12-7, 2.2.4-4.**

Miscellaneous bug fixes and security patches.


## 16 Nov 2021
{: #16-11-2021}

**Certificate Authority (CA) patch 1.5.2-3, Peer and ordering node patch 1.4.12-6, 2.2.4-3.**

Miscellaneous bug fixes and security patches.



**Deprecation notice for {{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}}**: Because Kubernetes v1.22 deprecates a number of APIs, nodes running Kubernetes v1.13, v1.14, or v1.15 will no longer be updated to the latest {{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}} patches. In addition, no Kubernetes cluster nodes will be updated if the master node is not reachable. Impacted clusters are identified via Kubernetes calls to the master node and calls to health check endpoints for blockchain components. Migration instructions for impacted clusters will be posted here soon. 

## 26 Oct 2021
{: #26-10-2021}

**Certificate Authority (CA) patch 1.5.2-2, Peer and ordering node patch 1.4.12-5, 2.2.4-2.**

Miscellaneous bug fixes and security patches.


## 05 Oct 2021
{: #05-10-2021}

**Certificate Authority (CA) patch 1.5.2-1, Peer and ordering node patch 1.4.12-3, 2.2.4-1.**

Miscellaneous bug fixes and security patches.


## 10 Aug 2021
{: #10-08-2021}

Miscellaneous bug fixes and security patches.

Export certificates in .pem format for easy management and maintenance in {{site.data.keyword.cloud_notm}} Certificate Manager. See [viewing and updating identities](/docs/blockchain-sw-253?topic=blockchain-sw-253-ibp-console-identities#ibp-console-identities-update-identities) to learn more details.





## 13 Jul 2021
{: #13-07-2021}

**Certificate Authority (CA) patch 1.5.0-1, Peer and ordering node patch 1.4.12-2, 2.2.3-2**

Miscellaneous bug fixes and security patches.

## 16 Jun 2021
{: #16-06-2021}

**Certificate Authority (CA) patch 1.4.9-8, Peer and ordering node patch 1.4.12-1, 2.2.3-1**

Miscellaneous bug fixes and security patches.

The platform has been updated to include support for Hyperledger Fabric v1.4.12 and v2.2.3.


## 05 May 2021
{: #05-05-2021}

**Certificate Authority (CA) patch 1.4.9-7, Peer and ordering node patch 1.4.11-2, 2.2.2-2**

Miscellaneous bug fixes and security patches.



## 13 Apr 2021
{: #13-04-2021}

**Certificate Authority (CA) patch 1.4.9-6, Peer and ordering node patch 1.4.11-2, 2.2.2-1** 



## 22 Feb 2021
{: #02-22-2021}

**Certificate Authority (CA) patch 1.4.9-5, Peer and ordering node patch 1.4.9-5 and 2.2.1-5**

## 12 Jan 2021
{: #01-12-2021}

**Certificate Authority (CA) patch 1.4.9-4, Peer and ordering node patch 1.4.9-4, 2.2.1-4**

Miscellaneous bug fixes and security patches.

### New logging configuration panel
{: #01-12-2021-logger-ui}

A new panel is available to override peer and ordering node log levels for specific component loggers. See [Configuring node logging](/docs/blockchain?topic=blockchain-ibp-console-manage-console#ibp-console-manage-logger) for more information.

## 08 Dec 2020
{: #12-08-2020}

**Certificate Authority (CA) patch 1.4.9-3, Peer and ordering node patch 1.4.9-3, 2.2.1-3**

Miscellaneous bug fixes and security patches.  


## 19 Nov 2020
{: #11-19-2020}

**Certificate Authority (CA) patch 1.4.9-2, Peer and ordering node patch 1.4.9-2, 2.2.1-2**

Miscellaneous bug fixes and security patches.  


When you link a new {{site.data.keyword.blockchainfull_notm}} Platform service instance to a Kubernetes cluster that uses [community Kubernetes Ingress](/docs/containers?topic=containers-ingress-types), the cluster Automatic Load Balancer (ALB) pods need to be restarted which can take five to ten minutes. If the restarts have not completed before you create your first blockchain Certificate Authority (CA), peer, or ordering node, the initial green status indicator on the new node can take slightly longer than you are accustomed to while the pods restart. Existing blockchain service instances are not impacted.



## 02 Nov 2020
{: #11-02-2020}


**Certificate Authority (CA) patch 1.4.9-1, Peer and ordering node patch 1.4.9-1, 2.2.1-1**

Miscellaneous bug fixes and security patches.  

If you are running **OpenShift Container Platform 3.11** in {{site.data.keyword.cloud_notm}}, it is recommended that you [upgrade your cluster to 4.4](https://docs.openshift.com/container-platform/4.4/migration/migrating_3_4/planning-migration-3-to-4.html){: external} now in order to fully take advantage of the new features. After you upgrade your cluster, follow instructions to [refresh your blockchain console](/docs/blockchain?topic=blockchain-ibp-console-manage-console#ibp-console-refresh) to experience the latest functionality in this release.
{: important}

### Fabric v2.x node upgrade
{: #11-02-2020-upgrade}

In addition to deploying new nodes based on the latest Fabric v2.2.1 image, you can upgrade your existing nodes to Fabric v2.x and the capabilities of your channels, allowing organizations to take advantage of the new smart contract lifecycle process.

For information about upgrading nodes, check out [Upgrading to a new version of Fabric](/docs/blockchain?topic=blockchain-ibp-console-govern-components#ibp-console-govern-components-upgrade). For information about updating the capabilities of your channels, check out [Capabilities](/docs/blockchain?topic=blockchain-ibp-console-govern#ibp-console-govern-capabilities).

### Support for Fabric v2.x smart contract lifecycle
{: #11-02-2020-lc}

The platform has been updated to include support for Fabric v2.x smart contract lifecycle process, which enables the distributed governance of smart contracts. Learn mode about how to [Deploy a smart contract using Fabric v2.x](/docs/blockchain?topic=blockchain-ibp-console-smart-contracts-v2).

### Improvements for HSM support
{: #11-02-2020-hsm}

A new [process](/docs/blockchain?topic=blockchain-ibp-console-adv-deployment#ibp-console-adv-deployment-hsm-build-docker) is available to configure an HSM for your blockchain nodes, delivering faster performance.

### Certificate renewal enhancements
{: #11-02-2020-cert-renew}

Customers can now use the console to renew certificates that have expired or are about to expire, including the Certificate Authority (CA) TLS certificate, peer and ordering node enrollment certificates, and peer and ordering node TLS certificates.

### Remove registered user from CA
{: #11-02-2020-delete-user}

You can now delete registered users from a CA.



## 1 Oct 2020
{: #10-01-2020}

**CA, Peer, and ordering node patch 1.4.7-3, 2.1.1-3**

Miscellaneous bug fixes and security patches.  

## 25 Aug 2020
{: #08-25-2020}

**CA, Peer, and ordering node patch 1.4.7-2, 2.1.1-2**

Miscellaneous bug fixes and security patches.  

Certificate expiration dates have been added throughout the component details, making it easier to monitor and track certificate expiration dates. See [Certificate Management](/docs/blockchain?topic=blockchain-cert-mgmt) to learn more about your responsibilities. In addition, it is now possible to enable Node OU support for your MSPs and channels through the console. Read more about [Node OU support](/docs/blockchain?topic=blockchain-cert-mgmt#cert-mgmt-nodeou), why this is important, and how to simplify certificate renewal for the MSPs on your network.

## 14 July 2020
{: #07-14-2020}

**CA, Peer, and ordering node patch 1.4.7-1, 2.1.1-1**  

Miscellaneous bug fixes and security patches.


## 18 June 2020
{: #06-18-2020}

**Peer and ordering node patch 1.4.7-0**

**{{site.data.keyword.IBM_notm}} considers this a critical patch that you should apply at your nearest opportunity.** Not applying this patch risks being susceptible to a data integrity issue if you experience a CouchDB or underlying storage crash on your peer. This patch updates the CouchDB `delayed_commits` configuration property to false. The prior setting could cause the peer's CouchDB database to be in an inconsistent state in the event of a CouchDB or underlying storage system crash. The new setting ensures that a peer will recover from crashes in a consistent state. As always, it is recommended to utilize an endorsement policy that requires multiple peers to endorse a transaction, to avoid an inconsistency from a single peer from impacting the overall blockchain network state.  

**To be certain that your peers have no database corruption**, you should reprovision your peers.

Miscellaneous bug fixes and security patches.
If you are running **OpenShift Container Platform 3.11** in {{site.data.keyword.cloud_notm}}, it is recommended that you [upgrade your cluster to 4.3](https://docs.openshift.com/container-platform/4.3/migration/migrating_3_4/planning-migration-3-to-4.html){: external} now in order to fully take advantage of the new features. After you upgrade your cluster, follow instructions to [refresh your blockchain console](/docs/blockchain?topic=blockchain-ibp-console-manage-console#ibp-console-refresh) to experience the latest functionality in this release.
{: important}


### Fabric peer and ordering node images
{: #06-18-2020-images}

The platform introduces the capability to deploy new peer and ordering nodes based on either Hyperledger Fabric v1.4 or v2.x. Deploying peer and ordering nodes with the latest Fabric images is recommended to ensure that you have access to current Fabric fixes and features. It is not currently possible to migrate existing nodes to Fabric v2 images.

### Elimination of Docker daemon dependency
{: #06-18-2020-docker}

Leveraging the Fabric v2 **external chaincode launcher** capability, when you deploy a peer based on the Fabric v2.1.1 image, smart contracts are deployed into their own pod rather than inside a container on the peer pod.

### Multizone-capable storage
{: #06-18-2020-Multizone}

If your Kubernetes cluster is configured to use multizone-capable storage, new peer and ordering nodes can be deployed that leverage multizone storage, effectively extending their high availability across cluster zones. See [Multizone-capable Storage](/docs/blockchain?topic=blockchain-ibp-v2-deploy-iks#ibp-console-storage-multizone) for more information.

### Kubernetes version upgrade
{: #06-18-2020-k8s}

{{site.data.keyword.blockchainfull_notm}} Platform requires **Kubernetes v1.15 - v1.18**. If your existing Kubernetes cluster is running v1.14 or lower, you need to upgrade your cluster before you can update your existing blockchain components to this latest release.

## 20 May 2020
{: #05-20-2020}

**CA, peer, and ordering node patch 1.4.6-2**

Miscellaneous bug fixes and security patches.


### Support for Intermediate Certificate Authorities (CAs)
{: #05-20-2020-ICA}

You now have the option to configure an intermediate CA using the console or APIs to override the default CA settings. See the tutorial on [Creating an intermediate Certificate Authority](/docs/blockchain?topic=blockchain-ibp-ica) to learn more.

### Updated connection profile
{: #05-20-2020-connx-profile}

The generated connection profile that client applications use to connect to the network has been streamlined and is now downloadable from the **Organizations** tab. See [Downloading a connection profile](/docs/blockchain?topic=blockchain-ibp-console-organizations#ibp-console-organizations-connx-profile) to learn how.

## 16 April 2020
{: #04-16-2020}

**CA, peer, and ordering node patch 1.4.6-1**  

### Support for AWS HSM
{: #04-16-2020-AWS-HSM}

If you plan to use AWS HSM, you must include the `immutable` and `AltId` parameters in your BCCSP configuration. See [Fabric documentation](https://hyperledger-fabric.readthedocs.io/en/release-2.2/hsm.html#example) for details.

**Miscellaneous bug fixes**

## 24 March 2020
{: #03-24-2020}

{{site.data.keyword.IBM_notm}} is in the process of migrating existing {{site.data.keyword.blockchainfull_notm}} Platform consoles to v2.1.3, therefore, the new features described in this list may not yet be available in your console. Unsure what version you are currently using? Click the question mark icon in the upper right corner of the console. The {{site.data.keyword.blockchainfull_notm}} Platform version is visible under the page heading. Existing customers will receive a Cloud notification with more details about when their console will be migrated.
{: note}

**Hyperledger Fabric v1.4.6**

All new nodes are deployed using Hyperledger Fabric v1.4.6. If you have an existing blockchain network, you should review the topic on [Capabilities](/docs/blockchain?topic=blockchain-ibp-console-govern#ibp-console-govern-capabilities) to understand how this new Fabric version can impact your network.

**Hardware Security Module (HSM) support for node identities**   

Full cryptographic HSM support is now available for HSMs that implement the PKCS #11 standard. Using an HSM provides on-demand encryption, key management, and key storage. When you deploy a CA, peer, or ordering node, you now have the option to store the private key for the node identity in an HSM. See [Configuring a node to use a HSM](/docs/blockchain?topic=blockchain-ibp-console-adv-deployment#ibp-console-adv-deployment-cfg-hsm) for more details.

**Support for adding and removing ordering nodes from an existing ordering service**  

Previously, an ordering service could only contain one or five ordering nodes and they all were contributed from the same organization. Now, the ordering service can be deployed across multiple organizations in a blockchain network, enabling individual organizations to add and remove individual ordering nodes as required. Multi-organizational transaction ordering improves the decentralized nature of a blockchain network.  Learn more about the process in the new [Adding and removing Raft ordering service nodes tutorial](/docs/blockchain?topic=blockchain-ibp-console-add-remove-orderer#ibp-console-add-remove-orderer).

**Ability to override default CA, peer, ordering node configuration**  

Hyperledger Fabric includes many configuration options for a CA, peer, or ordering node. A subset of those options for the [CA](/docs/blockchain?topic=blockchain-ibp-console-adv-deployment#ibp-console-adv-deployment-ca-customization), [peer](/docs/blockchain?topic=blockchain-ibp-console-adv-deployment#ibp-console-adv-deployment-peer-create-json) and [ordering node](/docs/blockchain?topic=blockchain-ibp-console-adv-deployment#ibp-console-adv-deployment-orderer-create-json) can now be overridden by using the console or the [APIs](/docs/blockchain?topic=blockchain-ibp-v2-apis#ibp-v2-apis-custom).

**Full Java smart contract development support**

In addition to smart contracts written in JavaScript, TypeScript, and Go programming languages, it is now possible to deploy Java smart contracts from the console. Moreover, Java can be freely mixed and matched with other application and smart contract programming languages, including JavaScript, TypeScript, and Go. This heterogeneous programming language support enables an organization to capitalize on the full range of its development skills.

In order to use Java chaincode, developers should be aware that:

- Java 11 is required to execute Java smart contracts.
- Gradle v4.x and Maven v3.x are used to build Java smart contracts.
- Custom Gradle versions can be used by using a Gradle wrapper.
- Java smart contracts require the fabric-chaincode-shim at v1.4.6 or later, as this version is the first version that includes support for Java 11.
- For an example of a Java smart contract, see the [FabCar Java smart contract](https://github.com/hyperledger/fabric-samples/tree/release-1.4/chaincode/fabcar/java){: external} from Fabric v1.4.

**v2 APIs available**

New {{site.data.keyword.blockchainfull_notm}} Platform console APIs using the route `/v2/` are now available. Use of the earlier `/v1/` APIs continues to be supported. See [{{site.data.keyword.blockchainfull_notm}} Platform APIs](https://cloud.ibm.com/apidocs/blockchain) for more information.

**High Availability Certificate Authority**

When a CA is deployed, replica sets can be configured to ensure High Availability of the node. Note that CA replica sets require a PostgreSQL database. Learn more in the [Building a high availability Certificate Authority (CA)](/docs/blockchain?topic=blockchain-ibp-console-build-ha-ca) tutorial.

## 6 February 2020
{: #02-06-2020}

**Update default resource allocation**

The default _memory_ allocation for the peer container has been increased from 0.4GB to 1GB. When deploying a new peer, the peer container will now default to 1GB memory, but you can adjust this value according to your use case. Increasing the memory alleviates some resource contention that could occur during smart contract instantiation.     

Existing peer containers are not impacted.

**Peer and ordering node patch 1.4.3-1**

The Ingress timeout for the gRPC web proxy has been increased to avoid an error during smart contract instantiation. It is recommended that you apply this patch to your existing peer and ordering nodes.

## 11 December 2019
{: #12-11-2019}

**Miscellaneous bug fixes**

## 15 November 2019
{: #11-15-2019}

**Service availability in two new regions**  

The {{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}} is now available to be provisioned in two new {{site.data.keyword.cloud_notm}} regions: **US East**, and **AP South**. Refer to the [locations](/docs/blockchain?topic=blockchain-ibp-regions-locations) information to see the data centers that are available in those regions.

## 06 November 2019
{: #11-06-2019}

**Simplified component creation flows**  

Peer, CA and ordering service creation has been streamlined. Now, when you create a node, you have to select a checkbox to configure advanced deployment options.

## 02 October 2019
{: #10-02-2019}

**Peer, CA, and ordering node patch 1.4.3-0**

**Zone selection for ordering nodes**

When multiple zones are configured in your Kubernetes cluster, you can now choose which zone to deploy your ordering node to. Spreading nodes across zones is useful for ensuring high availability of your blockchain network.

**Ability to set peer to be an anchor peer when the peer joins the channel**

Users can now choose to designate a peer to be an anchor peer when the peer is being joined to the channel, rather than after the peer has joined the channel. An anchor peer must exist for each organization in order for cross organizational gossip to work. [Anchor peers](/docs/blockchain?topic=blockchain-ibp-console-govern#ibp-console-govern-channels-anchor-peers) are also required for private data and service discovery to work. [Learn how](/docs/blockchain?topic=blockchain-ibp-console-build-network#ibp-console-build-network-join-peer).

**Ability to add a peer to a channel from the Channels tab**

A peer can now be joined to a channel directly from the Channels tab. See [Join a peer to a channel](/docs/blockchain?topic=blockchain-ibp-console-govern#ibp-console-govern-channels-join-peer) for more details.

**Export/Import all**

Allows for the bulk export and import of your blockchain network, including nodes, MSPs and identities, rather than having to export and import these components one at a time. See the topic on [Exporting and importing in bulk](/docs/blockchain?topic=blockchain-ibp-console-import-nodes#ibp-console-import-bulk-export-import) for more details.

**Hyperledger Fabric v1.4.3**

All new nodes are deployed using Hyperledger Fabric v1.4.3. If you have an existing blockchain network, you should review the topic on [Capabilities](/docs/blockchain?topic=blockchain-ibp-console-govern#ibp-console-govern-capabilities) to understand how this new Fabric version can impact your network.

## 21 August 2019
{: #08-21-2019}

**Peer node patch `1.4.1-3`**  
With this patch, you now have the ability to upload additional admin identities for a peer. See [Adding new peer admin certificates](/docs/blockchain?topic=blockchain-cert-mgmt) for more information.

**Select peer zone** ({{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}} only)  

If multiple zones are configured in your Kubernetes cluster on {{site.data.keyword.cloud_notm}}, you can now use the console UI to select the zone where the peer is deployed. This capability is important if you are configuring [multizone HA for peers](/docs/blockchain?topic=blockchain-ibp-console-ha#ibp-console-ha-multi-zone).

**Peer database support now includes LevelDB**

You now have the option to choose between CouchDB and LevelDB for your Peer database. For more information, see [LevelDB vs CouchDB](/docs/blockchain?topic=blockchain-ibp-console-adv-deployment#ibp-console-adv-deployment-level-couch).

**Certificate Authority (CA) updates**   

The console UI no longer stores the admin enroll id and secret that you provide when you create a CA. Therefore, after you create a CA, you are responsible for generating the admin certificates by using the new **Associate identity** and passing in the enroll id and secret. This change improves CA security. See [Associating the identity of the CA admin](/docs/blockchain?topic=blockchain-ibp-console-identities#ibp-console-identities-ca-identity) for more details.

## 12 August 2019
{: #08-12-2019}

**Beta trial of the {{site.data.keyword.blockchainfull_notm}} Platform ends**

The Beta trial of the {{site.data.keyword.blockchainfull_notm}} Platform, which began on February 12, 2019, has officially ended. All of the beta instances of your console and the components that you deployed into your Kubernetes cluster using the beta console will be deleted.

**Peer node patch `1.4.1-2`**

This patch contains a security fix for the peer couchDB container.

## 10 July 2019
{: #07-10-2019}

**Ordering node patch `1.4.1-2`**  

This patch fixes a problem that occurs when ordering nodes restart. The [genesis block](#x9076628){: term} is updated which prevents the ordering node from communicating with the other nodes in the ordering service. You can apply this patch to all existing ordering nodes in your ordering services by following these [instructions](/docs/blockchain?topic=blockchain-ibp-console-manage-console#ibp-console-manage-patch) to update each ordering node. All new ordering nodes that you create will automatically include this fix.

**Anchor peer removal**  

You now have the ability to remove anchor peers from a channel.

**Miscellaneous bug fixes**  

Bug fixes and translation updates.

## 24 May 2019
{: #05-24-2019}

**Raft consensus protocol**  

The five node Raft ordering service, recommended for production networks, is now available. In addition, for development and testing purposes, you can deploy a single node Raft ordering service.

## 9 May 2019
{: #05-09-2019}

**Introducing {{site.data.keyword.blockchainfull_notm}} Platform Console APIs**

APIs are now available to provision, edit, and delete peer, orderer, and CA nodes, making it possible to script the building of your blockchain network. Use the documentation in the [{{site.data.keyword.cloud_notm}} API documentation repository](/apidocs/blockchain#introduction){: external} to learn more about the APIs and try them out. In addition, see the topic on [Building a network with APIs](/docs/blockchain?topic=blockchain-ibp-v2-apis) for instructions on how to use the APIs to build out your network.  

**Channel governance**  

Channel governance updates allow policies to be reconfigured. You can also control which channel members need to sign a channel update and you can orchestrate signature collection.

## 17 April 2019
{: #04-17-2019}

**Ability to size and scale node resources**  

When you deploy a node you now have the ability to specify the amount of CPU, memory, and storage to your containers, where applicable. You can later scale their resources up or down at a later time according to usage patterns. For more information, see [Allocating resources](/docs/blockchain?topic=blockchain-ibp-console-adv-deployment#ibp-console-adv-deployment-allocate-resources).

**Use of {{site.data.keyword.cloud_notm}} Identity and Access Management (IAM)**  

IAM is used to control user access to the console UI as well as restricting the actions they can perform in the UI.  See this topic for information on how to [add and remove users from the console](/docs/blockchain?topic=blockchain-ibp-console-manage-console#ibp-console-manage-console-add-remove).

## 3 April 2019
{: #04-03-2019}

**Support for external CA**

When you add a peer or orderer node, you have the option to use certificates from an external CA, one that is not hosted by {{site.data.keyword.IBM_notm}}.  See this topic on [Using certificates from an external CA with your peer or ordering node](/docs/blockchain?topic=blockchain-ibp-console-adv-deployment#ibp-console-adv-deployment-third-party-ca) for more information.

**Tuning orderer performance**

New orderer tuning parameters are available in the console to give you more control over your orderer throughput and performance. See this topic on [Tuning your orderer](/docs/blockchain?topic=blockchain-ibp-console-govern#ibp-console-govern-orderer-tuning) for instructions on how to configure the parameters.



