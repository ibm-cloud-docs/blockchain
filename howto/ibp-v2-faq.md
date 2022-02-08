---

copyright:
  years: 2019, 2022
lastupdated: "2022-02-08"

keywords: FAQs, can I, upgrade, what version, peer ledger database, supported languages, why do I, regions

subcollection: blockchain
content-type: faq

---

{:external: target="_blank" .external}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:note: .note}
{:term: .term}
{:important: .important}
{:tip: .tip}
{:faq: data-hd-content-type='faq'}
{:support: data-reuse='support'}
{:pre: .pre}


# FAQs
{: #ibp-v2-faq}



**Hyperledger Fabric**
- [What is the value of using {{site.data.keyword.blockchainfull_notm}} Platform over native Hyperledger Fabric?](#ibp-v2-faq-v2-IBP-Overview-1-7)
- [What version of Hyperledger Fabric is being used with {{site.data.keyword.blockchainfull_notm}} Platform?](#ibp-v2-faq-v2-Hyperledger-Fabric-3-1)
- [Can {{site.data.keyword.blockchainfull_notm}} Platform components interoperate with Hyperledger Fabric components on the same network? And vice versa? And what is the support policy for networks that include both {{site.data.keyword.blockchainfull_notm}} Platform components and open source components?](#ibp-v2-faq-interoperability)

**Planning for your network**

- [Is {{site.data.keyword.blockchainfull_notm}} Platform HIPAA ready?](#ibp-v2-faq-hippa)
- [What ports are used by the {{site.data.keyword.blockchainfull_notm}} Platform?](#ibp-v2-ports)
- [How can I estimate the {{site.data.keyword.blockchainfull_notm}} Platform sizing requirements for my development, test, and production environments?](#ibp-v2-faq-sizing)
- [How does pricing work on {{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}}?](#ibp-v2-faq-cloud-pricing)
- [What are the limitations of the free {{site.data.keyword.blockchainfull_notm}} Platform using the  {{site.data.keyword.cloud_notm}} Kubernetes Service free cluster?](#ibp-v2-faq-free-cluster)
- [What regions or locations are available for the {{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}}?](#ibp-v2-faq-v2-IBP-Overview-1-9)
- [What persistent file storage does {{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}} use by default?](#ibp-v2-faq-cloud-storage)
- [Do I need multizone region storage for {{site.data.keyword.blockchainfull_notm}} Platform nodes?](#ibp-v2-faq-cloud-mzr-storage)


**Deploying the platform or upgrading**
- [What versions of Red Hat OpenShift are supported?](#ibp-v2-faq-ocp-versions)
- [Is there a trial option available for using a Red Hat OpenShift cluster on {{site.data.keyword.cloud_notm}}?](#ibp-v2-faq-ocp-trial)
- [Is it possible to deploy blockchain nodes to multiple clouds from a single blockchain console?](#ibp-v2-faq-multicloud)
- [How can I find what version of the {{site.data.keyword.blockchainfull_notm}} Platform that I am running?](#ibp-v2-faq-version)
- [How do I get the latest Fabric version and Fabric functionalities on my {{site.data.keyword.blockchainfull_notm}} Platform network?](#ibp-v2-faq-v2-fabric-upgrade)
- [I am currently using Hyperledger Fabric v1.4 and want to move to {{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}} or Multicloud. Can I continue to use Raft?](#ibp-v2-faq-migrate-raft)
- [Can I migrate the blockchain components on my {{site.data.keyword.IBM_notm}} Kubernetes service cluster to a Red Hat OpenShift cluster in {{site.data.keyword.cloud_notm}}?](#ibp-v2-faq-ocp-migrate)
- [What happens when I delete my {{site.data.keyword.blockchainfull_notm}} Platform service?](#ibp-v2-faq-v2-IBP-Overview-1-8)
- [Can I use my existing Kubernetes cluster on {{site.data.keyword.cloud_notm}}?](#ibp-v2-faq-v2-Infrastructure-4-2)


**Blockchain components**
- [What database do the peers use for their ledger?](#ibp-v2-faq-v2-IBP-Overview-1-3)
- [What types of off-chain databases are supported with the {{site.data.keyword.blockchainfull_notm}} Platform?](#ibp-v2-faq-offchain-db)
- [If service discovery is on, will an endorsement request be routed to any peer on the network?](#ibp-v2-faq-service-discovery)
- [Do ordering service Raft nodes use Transport Layer Security (TLS) for communication?](#ibp-v2-faq-raft-tls)
- [How can I back up and restore components and networks?](#ibp-v2-faq-backup-restore)
- [What benefits are available with the new smart contract lifecycle available on nodes and channels running on Fabric v2.x?](#ibp-v2-faq-new-lifecycle)

**Certificates**
- [Do you support using certificates from non-IBM Certificate Authorities (CAs)?](#ibp-v2-faq-v2-external-certs)
- [What is the recommended way to manage private keys?](#ibp-v2-faq-hsm)
- [Can I integrate my corporate LDAP server with the Certificate Authority (CA) in the {{site.data.keyword.blockchainfull_notm}} Platform?](#ibp-v2-faq-ldap)
- [What is the process for rotating certificates on a periodic basis?](#ibp-v2-faq-cert-mgmt)

**Developing applications and smart contracts**
- [What languages are supported for smart contracts?](#ibp-v2-faq-v2-IBP-Overview-1-4)
- [What version of the IBM Blockchain Platform works with the Ansible collection?](#ibp-v2-faq-ansible-version)
- [How do I get support for running the IBM Blockchain Platform Ansible playbook?](#ibp-v2-faq-ansible-support)
- [How can I find the examples and tutorials within the VSCode extension?](#ibp-v2-faq-vscode-tutorials)
- [Can the {{site.data.keyword.blockchainfull_notm}} Platform monitor the health of a client application?](#ibp-v2-faq-mon-client-app)

**Monitoring your network**
- [Where does {{site.data.keyword.IBM_notm}} store the customer's logs and how long does {{site.data.keyword.IBM_notm}} keep the audit logs for the blockchain platform service?](#ibp-v2-faq-customer-logs)
- [Do we have access to logging services and what logs are available to me?](#ibp-v2-faq-v2-Logging-and-Monitoring-11-6)  
- [Where can I see the price breakdown for {{site.data.keyword.cloud_notm}} Kubernetes Service, Storage, and Blockchain in my monthly invoice?](#ibp-v2-faq-cloud-invoice)
- [Is there a best practice for monitoring my blockchain resources?](#ibp-v2-faq-mon-res)

## What is the value of using {{site.data.keyword.blockchainfull_notm}} Platform over native Hyperledger Fabric?
{: #ibp-v2-faq-v2-IBP-Overview-1-7}
{: faq}

Hyperledger Fabric is a powerful, versatile, pluggable, open source, distributed ledger technology capable of addressing a wide variety of use cases across many industries. {{site.data.keyword.blockchainfull_notm}} Platform is {{site.data.keyword.IBM_notm}}'s commercial distribution of Hyperledger Fabric. A key benefit of the platform is that  {{site.data.keyword.IBM_notm}} tests the open source code for security vulnerabilities daily and provides 24x7x365 support with SLAs appropriate for production environments. The platform is the **commercial distribution of Hyperledger Fabric** and includes integrated tools that provide end to end features for developers and network operators to develop, test, operate, monitor, and govern Fabric components by using an intuitive console UI. Quickly deploy an instance and use the streamlined console UI to [build a network](/docs/blockchain?topic=blockchain-ibp-console-build-network), easily [deploy smart contracts](/docs/blockchain?topic=blockchain-ibp-console-smart-contracts), [govern your components](/docs/blockchain?topic=blockchain-ibp-console-govern-components), and [govern your channel](/docs/blockchain?topic=blockchain-ibp-console-govern). Interested in APIs? See the [{{site.data.keyword.blockchainfull_notm}} Platform API reference](https://cloud.ibm.com/apidocs/blockchain){: external}. With the {{site.data.keyword.blockchainfull_notm}} Platform, it is easy to extend a basic network, work with multicloud solutions, and receive {{site.data.keyword.IBM_notm}} worldwide support when needed. Finally, the {{site.data.keyword.blockchainfull_notm}} Platform provides additional security benefits that are essential for running an enterprise-grade production network.



## What version of Hyperledger Fabric is being used with {{site.data.keyword.blockchainfull_notm}} Platform?
{: #ibp-v2-faq-v2-Hyperledger-Fabric-3-1}
{: faq}


{{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}} uses Hyperledger Fabric v1.4.12 and v2.2.5.




## Can {{site.data.keyword.blockchainfull_notm}} Platform components interoperate with Hyperledger Fabric components on the same network? And vice versa? And what is the support policy for networks that include both {{site.data.keyword.blockchainfull_notm}} Platform components and open source components?
{: #ibp-v2-faq-interoperability}
{: faq}

Yes. Hyperledger Fabric networks consist of many distributed members owning one or more nodes. There are multiple deployment options:

* {{site.data.keyword.blockchainfull_notm}} Platform for IBM Cloud with console
* {{site.data.keyword.blockchainfull_notm}} Platform v2.x (Full Platform)
* {{site.data.keyword.blockchainfull_notm}} Images
* Open source Hyperledger Fabric images or a non-IBM product

Containers deployed from any of the above sources can be connected on a single channel and transact. You can join IBM Blockchain Platform peers to any network running Hyperledger Fabric components. Similarly, you can invite Fabric peers to join channels hosted on an ordering service deployed on the IBM Blockchain Platform. Note that you will need to use Hyperledger Fabric APIs or the CLI. For more information about what is supported, see [Support for IBM Blockchain Platform](https://www.ibm.com/support/pages/node/1072956){: external}. For instructions on how to configure interoperability see [Connect the IBM Blockchain Platform to Hyperledger Fabric components](https://developer.ibm.com/tutorials/hyperledger-fabric-and-ibm-blockchain-peer-interoperability/){: external}.




## Is {{site.data.keyword.blockchainfull_notm}} Platform HIPAA ready?
{: #ibp-v2-faq-hippa}
{: faq}

Because HIPAA readiness is only relevant when platform components process Personal Health Information (PHI) or Personally Identifiable Information (PII), the {{site.data.keyword.blockchainfull_notm}} Platform does not need to be HIPAA ready. Customers should not store PHI or PII on the ledger since it is immutable and therefore cannot be deleted. Instead, the recommendation is to store all PHI or PII off ledger in another database and simply reference it from the ledger.

The {{site.data.keyword.blockchainfull_notm}} platform gives customers total control over their deployments, certificates, and private keys. The console simplifies and accelerates the process of deploying components into a Kubernetes cluster on {{site.data.keyword.cloud_notm}} that is managed and controlled by the customer. As a reminder, because the customer owns the storage that is mounted to the containers, {{site.data.keyword.IBM_notm}} does not have access to or control over any of the data that the customer chooses to store in their ledger.

## What ports are used by the {{site.data.keyword.blockchainfull_notm}} Platform?
{: #ibp-v2-ports}
{: faq}

See the port information in the Security topic that addresses this for the [console](/docs/blockchain?topic=blockchain-ibp-security#ibp-security-ibp-ports) and the [customer Kubernetes cluster](/docs/blockchain?topic=blockchain-ibp-security#ibp-security-Kubernetes-ports).

## How can I estimate the {{site.data.keyword.blockchainfull_notm}} Platform sizing requirements for my development, test, and production environments?
{: #ibp-v2-faq-sizing}
{: faq}

After you understand how many CAs, peers, and ordering nodes are required, you can examine the default resource allocations table for  your [nodes](/docs/blockchain?topic=blockchain-ibp-console-adv-deployment#ibp-console-adv-deployment-allocate-resources) to get an approximate estimate of the CPUs (VPCs) required for your network. If you are purchasing {{site.data.keyword.blockchainfull_notm}} Platform on {{site.data.keyword.cloud_notm}}, you can estimate your cost through this method, but you also have the ability to scale dynamically if more sources are needed.


## How does pricing work on the {{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}}?
{: #ibp-v2-faq-cloud-pricing}
{: faq}
{: support}

{{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}} is priced based on the VPCs that you allocate to your blockchain nodes on the {{site.data.keyword.IBM_notm}} Kubernetes Service. For more information, see [Pricing for {{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}}](/docs/blockchain?topic=blockchain-ibp-saas-pricing#ibp-saas-pricing).


## What are the limitations of the free {{site.data.keyword.blockchainfull_notm}} Platform using the  {{site.data.keyword.cloud_notm}} Kubernetes Service free cluster?
{: #ibp-v2-faq-free-cluster}
{: faq}

* Preview the {{site.data.keyword.blockchainfull_notm}} Platform at no charge for 30 days when you link your {{site.data.keyword.blockchainfull_notm}} Platform service instance to an {{site.data.keyword.cloud_notm}} Kubernetes free cluster.
* Performance will be limited by throughput, storage, and functionality. Read more about the [limitations of free clusters](/docs/containers?topic=containers-cs_ov#cluster_types).
* {{site.data.keyword.cloud_notm}} will delete your Kubernetes cluster after 30 days.
* Only one blockchain console can be connected to a free cluster at a time.
* You cannot migrate any nodes or data from a free cluster to a paid cluster.

The following capabilities are only available on a paid cluster:

- Customizing resource allocation for a node during or after deployment.
- Using a Hardware Security Module (HSM) to secure the private key for a node.
- Configuring a Certificate Authority (CA) for high availability by using a PostgreSQL database and replica sets.
- Selecting a specific Kubernetes zone when deploying a node.
- Overriding node configuration during or after deployment by using the console or APIs.
- Adding or removing ordering nodes to an ordering service. The free offering only supports a single node Raft ordering service.  

See [Find out how to preview the platform free for 30 days](/docs/blockchain?topic=blockchain-ibp-saas-pricing#ibp-saas-pricing-free) for more information on how to get started.


## What regions or locations are available for the {{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}}?
{: #ibp-v2-faq-v2-IBP-Overview-1-9}
{: faq}

The available regions for {{site.data.keyword.blockchainfull_notm}} Platform are listed in [{{site.data.keyword.blockchainfull_notm}} Platform locations](/docs/blockchain?topic=blockchain-ibp-regions-locations). Note that you must create a Kubernetes cluster on {{site.data.keyword.cloud_notm}} in the same region as the blockchain service to recognize the cluster. Additional regions will be available soon.


## What persistent file storage does {{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}} use by default?
{: #ibp-v2-faq-cloud-storage}
{: faq}

By default {{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}} uses Classic file storage. You can find more information on the [{{site.data.keyword.cloud_notm}} File storage page](/docs/containers?topic=containers-file_storage#file_storage){: external}. For a complete list of storage options, see [Persistent storage considerations](/docs/blockchain?topic=blockchain-ibp-v2-deploy-iks#ibp-console-storage)



## Do I need multizone region storage for {{site.data.keyword.blockchainfull_notm}} Platform nodes?
{: #ibp-v2-faq-cloud-mzr-storage}
{: faq}

No. When the {{site.data.keyword.blockchainfull_notm}} Platform is configured with a multizone cluster in {{site.data.keyword.cloud_notm}} Kubernetes service, you can choose which zone a particular component (peer or ordering node) is deployed to, or you can let the console decide.  Then, when the node is subsequently deployed, Kubernetes "pins" the associated pod to the chosen zone. Pinning means that Kubernetes will not provision the pod in another zone in the event of a whole zone failure. And because the pods are pinned to specific zones, there is no need to access the same storage from another zone. Therefore, MZR storage is not required for {{site.data.keyword.blockchainfull_notm}} Platform nodes.



## What versions of Red Hat OpenShift are supported?
{: #ibp-v2-faq-ocp-versions}
{: faq}

Currently, {{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}} supports linking to Red Hat OpenShift Container Platform 4.5, 4.6 and 4.7 clusters.



## Is there a trial option available for using a Red Hat OpenShift cluster on {{site.data.keyword.cloud_notm}}?
{: #ibp-v2-faq-ocp-trial}
{: faq}

A free 30 day trial is available in the [Red Hat Marketplace.](https://marketplace.redhat.com/en-us/products/ibm-blockchain){: external}. See [Deploy from Red Hat Marketplace](/docs/blockchain-sw-252?topic=blockchain-sw-252-deploy-ocp-rhm) to learn more.


## Is it possible to deploy blockchain nodes to multiple clouds from a single blockchain console?
{: #ibp-v2-faq-multicloud}
{: faq}

You cannot currently deploy blockchain nodes to multiple hosted cloud providers. However, you can use your console to operate a distributed multicloud network by importing nodes deployed by using consoles on other clouds.

## How can I find what version of the {{site.data.keyword.blockchainfull_notm}} Platform that I am running?
{: #ibp-v2-faq-version}
{: faq}
{: support}

View the Support page by clicking the question mark icon <img src="../images/support-link.png" alt="Support link icon" width="30" style="width:30px; border-style: none"/> in the upper right corner of the page. The {{site.data.keyword.blockchainfull_notm}} Platform version is visible under the page heading.

## How do I get the latest Fabric version and Fabric functionalities on my {{site.data.keyword.blockchainfull_notm}} Platform network?
{: #ibp-v2-faq-v2-fabric-upgrade}
{: faq}

Depending on the contents of a Fabric release and {{site.data.keyword.blockchainfull_notm}} Platform, it might only be necessary to upgrade nodes to get access to the latest features. However, often it is necessary to update channel configurations with the latest capabilities in order to get access to the latest Fabric features. In these cases, it is important to upgrade components and channels in a particular order:

1. Upgrade nodes to the latest Fabric versions. Note that nodes are always backward compatible with earlier versions and earlier capabilities. For more information about upgrading nodes, see [Upgrading to a new version of Fabric](/docs/blockchain?topic=blockchain-ibp-console-govern-components#ibp-console-govern-components-upgrade).
2. Update channels with any new channel capabilities. If you update capabilities to a capability level (such as 2.0) before upgrading nodes to the Fabric version corresponding to a capability, the node may crash. For more information, see [Capabilities](/docs/blockchain?topic=blockchain-ibp-console-govern#ibp-console-govern-capabilities).

If you are moving from v1.4.x to v2.x, you may have to update your smart contracts to conform to new smart contract lifecycle. For more information, see [Upgrading to a new version of Fabric](/docs/blockchain?topic=blockchain-ibp-console-smart-contracts-v2#ibp-console-smart-contracts-v2).
{: tip}

## I am currently using Hyperledger Fabric v1.4.x and want to move to {{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}} . Can I continue to use Raft?
{: #ibp-v2-faq-migrate-raft}
{: faq}

Yes. The {{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}}  uses Raft consensus. All of the applications and smart contracts that you are using on Fabric v1.4.x are able to work on your {{site.data.keyword.blockchainfull_notm}} Platform network. However, no mechanism exists to migrate your ledger data from one network to another. Instead, you can reinstall your smart contract packages on your {{site.data.keyword.blockchainfull_notm}} Platform network. See also [Can IBM Blockchain Platform components interoperate with Hyperledger Fabric components on the same network?](/docs/blockchain-sw-252?topic=blockchain-sw-252-ibp-v2-faq#ibp-v2-faq-interoperability).

## Can I migrate the blockchain components on my {{site.data.keyword.IBM_notm}} Kubernetes service cluster to a Red Hat OpenShift cluster in {{site.data.keyword.cloud_notm}}?
{: #ibp-v2-faq-ocp-migrate}
{: faq}

No. There is currently no way to migrate existing components to a new Red Hat OpenShift cluster in {{site.data.keyword.cloud_notm}}.


## What happens when I delete my {{site.data.keyword.blockchainfull_notm}} Platform service?
{: #ibp-v2-faq-v2-IBP-Overview-1-8}
{: faq}

When you delete an {{site.data.keyword.blockchainfull_notm}} Platform service instance, all of the blockchain CAs, peers, smart contract pods (if using peers deployed with a Fabric 2.x image; smart contracts deployed on peers using a Fabric 1.4.x image are located inside the peer container), and ordering nodes are deleted along with their associated storage. If you have exported any nodes to other consoles, make sure to reach out to the administrators of those consoles to let them know that those nodes are no longer functioning, because deleting them in your console does not automatically delete them in theirs.


## Can I use my existing Kubernetes cluster on {{site.data.keyword.cloud_notm}}?
{: #ibp-v2-faq-v2-Infrastructure-4-2}
{: faq}
{: support}

Your existing Kubernetes cluster works with the {{site.data.keyword.blockchainfull_notm}} Platform if it satisfies the following conditions:
- It is running Kubernetes version v1.19 - v1.22.
- There are enough available resources in the cluster.


## What database do the peers use for their ledger?
{: #ibp-v2-faq-v2-IBP-Overview-1-3}
{: faq}
{: support}

You have the choice of either CouchDB or LevelDB when you configure your peer database. Because data is modeled differently in a Couch database than in a Level database, the peers in a channel must all use the same database type. See [LevelDB versus CouchDB](/docs/blockchain?topic=blockchain-ibp-console-adv-deployment#ibp-console-adv-deployment-level-couch) to decide what is best for your business needs.

## What types of off-chain databases are supported with the {{site.data.keyword.blockchainfull_notm}} Platform?
{: #ibp-v2-faq-offchain-db}
{: faq}

As a best practice it is recommended that you do not query the entire blockchain ledger for the purpose of aggregation or reporting. If you want to build a dashboard or collect large amounts of data as part of your application, you can query an off chain database that replicates the data from your blockchain network. This allows you to understand the data on the blockchain without degrading the performance of your network or disrupting transactions.

You can use block or smart contract events from your application to write transaction data to an off-chain database or analytics engine. For each block received, the block listener application would iterate through the block transactions and build a data store by using the key/value writes from each valid transaction's read-write set. The [Peer channel-based event services](https://hyperledger-fabric.readthedocs.io/en/release-2.2/peer_event_services.html#peer-channel-based-event-services) provide replayable events to ensure the integrity of downstream data stores. For an example of how you can use an event listener to write data to an external database, see the [Off chain data sample](https://github.com/hyperledger/fabric-samples/tree/release-1.4/off_chain_data) in the Fabric samples.

Blockchain solutions can use any RDBMS or NoSQL DB such as {{site.data.keyword.IBM_notm}} Cloudant for offchain data storage. Hyplerledger Fabric does not govern, interact with, or manage off-chain databases. In most cases, the off-chain database is used for reference data and non-transactional data. {{site.data.keyword.IBM_notm}} has successfully built blockchain products and solution accelerators with Hyperledger Fabric and NoSQL databases such as OrientDB.

## If service discovery is on, will an endorsement request be routed to any peer on the network?
{: #ibp-v2-faq-service-discovery}
{: faq}

It depends on whether your endorsement policy is set to "ANY", in which any peer can sign an endorsement request, or whether the policy is bound directly to an organization's peers. The service discovery information provided by the peer supplies two pieces of information, `Layouts` and `EndorsersByGroup`. With these two pieces of data, the SDK has the ability to send requests to peers in different organizations that meet the endorsement policy requirements. The Node.js SDK provides default code that uses the `Layouts` and `EndorsersByGroup` and sends the requests to the appropriate peers to meet the endorsement policy requirements. This existing logic can be customized to meet the business needs.

## Do ordering service Raft nodes use Transport Layer Security (TLS) for communication?
{: #ibp-v2-faq-raft-tls}
{: faq}

Yes. The Raft ordering service nodes are configured to use TLS communication. TLS is embedded in the trust model of Hyperledger Fabric. By default, server-side TLS is enabled for all communications using TLS certificates. TLS is used to encrypt the communication between your nodes and as well as between your nodes and your applications. TLS prevents man-in-the-middle and session hijacking attacks. All {{site.data.keyword.blockchainfull_notm}} Platform components use TLS to communicate with each other.

## How can I back up and restore components and networks?
{: #ibp-v2-faq-backup-restore}
{: faq}

As with anything that is deployed to a Kubernetes-based cluster, backups of components and networks in the {{site.data.keyword.blockchainfull}} Platform are a matter of backing up its [persistent volumes](https://kubernetes.io/docs/concepts/storage/persistent-volumes/){: external}. These volumes are where ledgers and other types of storage are mounted so they can be used by nodes.

As a result, "backing up" a component or a network is the process of saving a copy of the relevant persistent volumes, while "restoring" a component or network involves bringing up components and pointing them to these saved volumes. For more information, check out [Backing up and restoring components and networks](/docs/blockchain?topic=blockchain-backup-restore){: external}.

## What benefits are available with the new smart contract lifecycle available on nodes and channels running on Fabric v2.x?
{: #ibp-v2-faq-new-lifecycle}
{: faq}

The new smart contract lifecycle allows channel members to collaborate in the decision making process about smart contracts like never before. Where previously smart contracts were instantiated on a channel by a single channel member and other organizations only had the ability to choose whether to install the smart contract, the new lifecycle allows organizations to propose, approve, and commit smart contracts at an organizational level.

This separation of concerns opens exciting new opportunities for collaborating organizations. For example, different organizations can install smart contracts on their peers that contain only the code relevant to their business role and make minor updates to these smart contracts where necessary, without needing to seek new approvals from other organizations.

For a tutorial on how this process is handled by the console, check out [Deploy a smart contract using Fabric v2.x](/docs/blockchain?topic=blockchain-ibp-console-smart-contracts-v2).

For information about to take advantage of the new lifecycle when writing a smart contract, check out [Writing powerful smart contracts](/docs/blockchain?topic=blockchain-write-powerful-smart-contracts).

## Do you support using certificates from non-IBM Certificate Authorities?
{: #ibp-v2-faq-v2-external-certs}
{: faq}

Yes, you can bring your own certificates if they are issued by a CA that is X.509 compliant. The CA should sign by using ECDSA and the defaults should be set to use P256 curve. See this topic about [Using certificates from an external CA with your peer or ordering node](/docs/blockchain?topic=blockchain-ibp-console-adv-deployment#ibp-console-adv-deployment-third-party-ca).

## What is the recommended way to manage private keys?
{: #ibp-v2-faq-hsm}
{: faq}

Because private keys are not stored by the platform, users are responsible for downloading and securing their private key. Therefore, when a higher level of security is required for private keys, an HSM is recommended. An HSM is a hardware appliance that performs cryptographic operations and provides the capability to ensure that the cryptographic keys never leave the HSM. Hyperledger Fabric supports HSM devices that implement the PKCS #11 standard. PKCS #11 is a cryptographic standard for secure operations, generation, and storage of keys. See [Configuring a node to use a Hardware Security Module (HSM)](/docs/blockchain?topic=blockchain-ibp-console-adv-deployment#ibp-console-adv-deployment-cfg-hsm) to learn more.

## Can I integrate my corporate LDAP server with the Certificate Authority (CA) in the {{site.data.keyword.blockchainfull_notm}} Platform?
{: #ibp-v2-faq-ldap}
{: faq}

You cannot currently directly integrate your [LDAP](#x2481619){: term} server with the CA. However, you can use an external mechanism to generate X.509 certificates for the LDAP users. To use those certificates with a peer or ordering service, see these topics on [Using certs from an external CA for your peer or ordering service](/docs/blockchain?topic=blockchain-ibp-console-adv-deployment#ibp-console-adv-deployment-third-party-ca) and
[Manually building an organization MSP](/docs/blockchain?topic=blockchain-ibp-console-organizations#console-organizations-build-msp).  

Also, you cannot configure the blockchain console login authentication to use an LDAP user registry at this time.
{: note}

## What is the process for rotating certificates on a periodic basis?
{: #ibp-v2-faq-cert-mgmt}
{: faq}
{: support}

Similar to how passwords need to be regularly updated, identity certificates need to be renewed, a process also referred to as "certificate rotation". The platform displays certificate expiration dates for components throughout the console.  When a certificate expires, transactions on the network will fail because the identity can no longer be trusted.  It is your responsibility to monitor those expiration dates and manage your certificate renewal accordingly. The process varies depending on the type of certificate, when it was generated, and for organization admin certificates, whether Node OU support was enabled on the MSP when the identity was enrolled. The platform attempts to renew the peer and ordering node enrollment certificates 30 days before they expire. See [Managing certificates](/docs/blockchain?topic=blockchain-cert-mgmt) to learn more about the types of certificates that you need to monitor and how to renew them.

## What languages are supported for smart contracts?
{: #ibp-v2-faq-v2-IBP-Overview-1-4}
{: faq}
{: support}

The {{site.data.keyword.blockchainfull_notm}} Platform supports smart contracts that are written in Node.js, Golang (Go), JavaScript, and Java. The new Hyperledger Fabric programming model currently supports JavaScript, TypeScript, Java, and Go. If you are interested in preserving your existing application code, or by using Fabric SDKs for *Go*, you can still connect to your {{site.data.keyword.blockchainfull_notm}} Platform network by using the lower-level Fabric SDK APIs.

## What version of the {{site.data.keyword.blockchainfull_notm}} Platform works with the Ansible collection?
{: #ibp-v2-faq-ansible-version}
{: faq}

Versions 2.1.3 and 2.5.x of the {{site.data.keyword.blockchainfull_notm}} Platform can be used with the Ansible collection to deploy a Hyperledger Fabric network.

## How do I get support for running the {{site.data.keyword.blockchainfull_notm}} Platform Ansible playbook?
{: #ibp-v2-faq-ansible-support}
{: faq}

Ansible is an open source technology and this product is not officially supported by {{site.data.keyword.IBM_notm}}. For support related to the usage of the {{site.data.keyword.blockchainfull_notm}}  Platform and Ansible playbooks use the [GitHub repository](https://github.com/IBM-Blockchain/ansible-collection/issues){: external}.

## How can I find the examples and tutorials within the VSCode extension?
{: #ibp-v2-faq-vscode-tutorials}
{: faq}

The {{site.data.keyword.blockchainfull_notm}} Platform extension provides guided tutorials within VS Code to help you get started. You can find these tutorials on the extension home page when the extension is first installed. However, you can return to the tutorials and the home page by going to the extensions tab. For more information, see [Guided tutorials in VS Code](/docs/blockchain?topic=blockchain-develop-vscode#develop-vscode-guided-tutorials).

## Can the {{site.data.keyword.blockchainfull_notm}} Platform monitor the health of a client application?
{: #ibp-v2-faq-mon-client-app}
{: faq}

The {{site.data.keyword.blockchainfull_notm}} Platform console does not monitor the health of blockchain client applications, but {{site.data.keyword.cloud_notm}} does offer tooling such as [{{site.data.keyword.la_full_notm}}](/catalog/services/ibm-log-analysis){: external} and [{{site.data.keyword.mon_full_notm}}](/catalog/services/ibm-cloud-monitoring){: external} that can be used for their health monitoring.



## Where does {{site.data.keyword.IBM_notm}} store the customer's logs and how long does {{site.data.keyword.IBM_notm}} keep the audit logs for the blockchain platform service?
{: #ibp-v2-faq-customer-logs}
{: faq}

The logs are stored in the customer's Kubernetes cluster. {{site.data.keyword.IBM_notm}} does not have access to the logs and it is up to the customer to manage all of their log data including retention management.

## Do we have access to logging services and what logs are available to me?
{: #ibp-v2-faq-v2-Logging-and-Monitoring-11-6}
{: faq}

With {{site.data.keyword.blockchainfull_notm}} Platform, you can now directly access logs from your Kubernetes dashboard. It is recommend that you take advantage of the {{site.data.keyword.la_full_notm}} service that allows you to easily parse the logs in real time.


## Where can I see the price breakdown for {{site.data.keyword.cloud_notm}} Kubernetes Service, Storage, and Blockchain in my monthly invoice?
{: #ibp-v2-faq-cloud-invoice}

Actual cost breakdowns are visible from your Invoices in the {{site.data.keyword.cloud_notm}} Dashboard. For detailed steps, see the [Billing](/docs/blockchain?topic=blockchain-ibp-saas-pricing#ibp-saas-pricing-billing) section in the Pricing topic.


## Is there a best practice for monitoring my blockchain resources?
{: #ibp-v2-faq-mon-res}
{: faq}
{: support}

You are responsible for the health monitoring and resource allocation of the blockchain nodes in your Kubernetes cluster. While requests against the nodes are being actively processed, you should be monitoring for spikes in resource consumption to avoid problems. {{site.data.keyword.IBM_notm}} recommends that you configure {{site.data.keyword.mon_full_notm}} and set up alerts to track when blockchain nodes are reaching their limits. See the tutorial on [{{site.data.keyword.mon_full_notm}}](/docs/monitoring?topic=monitoring-getting-started) for more details.

You should be aware that JavaScript and TypeScript smart contracts require more resources than contracts written in Golang. Therefore, when you are allocating resources to your cluster, it is important to ensure adequate resources are available to your smart contract pods when they are deployed on a channel and during transaction processing. The pods containing the smart contracts will consume as much resources as they need to function.
{: tip}





