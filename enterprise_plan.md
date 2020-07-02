---

copyright:
  years: 2018, 2020
lastupdated: "2019-09-24"

keywords: blockchain network, Enterprise Plan, production-ready network

subcollection: blockchain

---

{:external: target="_blank" .external}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:important: .important}
{:pre: .pre}

# About Enterprise Plan
{: #enterprise-plan-about}

{{site.data.keyword.blockchainfull}} Platform Enterprise Plan is a production-ready offering for organizations who would like to create or join a blockchain network for real businesses. This plan provides the key infrastructure along with tools and support to easily start a highly secure and production-ready network. Enterprise Plan is upgraded from Hyperledger Fabric V1.0 to V1.1 on May 15, 2018. All networks that are created after May 15, 2018 are at Fabric V1.1 level. However, networks that were created earlier will remain at Fabric V1.0 level.
{:shortdesc}

Enterprise Plan has been withdrawn. Therefore, new clients cannot provision Enterprise Plan networks at this time. Existing clients can continue to add new members and create new networks until December 31, 2019. However, no new Enterprise Plan networks can be created after that date. New users can make use of the latest user interface and features available now in the second generation of blockchain technology by visiting [{{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}}](/docs/blockchain?topic=blockchain-ibp-v2-deploy-iks).
{: important}

**Enterprise Plan** is a production environment that offers high levels of security and support. Deploying your network on Enterprise Plan allows you to take advantage of the following features:

* An ultra high security environment with many hardware, firmware, and software security features.
* Hardened architecture for scalability, resiliency, and availability.
* Optimized for performance and running on the world's fastest Linux compute.

Operating your network on the {{site.data.keyword.blockchainfull_notm}} Platform includes tools and capabilities to simplify administrative tasks:

* Dashboards for monitoring and managing the resources on the network.
* Seamless upgrades of the full code stack.
* 24/7 technical support that is integrated into the portal.
* Hardened security stack with no privileged access, malware and tamper resistance, 100% encryption, and many more features for networks with sensitive data in regulated industries.
* Enterprise networks are backed up off-site once every 24 hours. If a disaster happens, these networks can be restored to the same site or to an alternate site.

**Notes:**
- {{site.data.keyword.blockchainfull_notm}} Platform is a platform service on {{site.data.keyword.cloud_notm}} and all membership offerings follow the [{{site.data.keyword.cloud_notm}} Services terms](http://www-03.ibm.com/software/sla/sladb.nsf/sla/bm){: external} on service level agreements (SLAs). Enterprise Plan networks are provisioned in a data center in a single geography. For a list of available geographies, see  [{{site.data.keyword.blockchainfull_notm}} Platform locations](/docs/blockchain?topic=blockchain-ibp-regions-locations#ibp-regions-locations).

For members who are going to initiate the network, {{site.data.keyword.IBM_notm}} provides a graphical user interface to guide the network initiator through key steps to set up and provision the network. This includes inviting other members and setting the governance rules. For more information, see [Govern the Enterprise Plan network](/docs/blockchain?topic=blockchain-getting-started-with-enterprise-plan#getting-started-with-enterprise-plan). After the network is deployed, an interactive graphical user interface, the Network Monitor, is available to monitor health and activity of the network; manage key network activities that include new deployments, members addition or removal, chaincode lifecycle, and channel management; and seek technical support. For more information, see [Using the Network Monitor](/docs/blockchain?topic=blockchain-ibp-dashboard#ibp-dashboard).

Sign up now for your [{{site.data.keyword.blockchainfull_notm}} membership](https://cloud.ibm.com/catalog/services/ibm-blockchain-5-prod){: external}.

The {{site.data.keyword.blockchainfull_notm}} Platform is built with key Hyperledger Fabric components that include a Certificate Authority (CA) and at least 1 peer (max of 6).  Enterprise Plan also provides a crash fault tolerant (CFT) Kafka ordering service for the network members.

The Fabric CA is the Certificate Authority that is provided with the Enterprise plan. Two intermediate CAs are provided per member, which grant membership to the network. By using the CA, the member can also provide membership certificates to users of the network.

It’s important to understand that for a transaction to be appended to the ledger, there are three phases that are involved:
1. Transaction Simulation and Endorsement (peer)
2. Ordering (ordering service)
3. Validation and Commit (peer)

The Fabric peers owned by the members are the interface or gateway for applications to execute chain code, providing the business logic to execute transactions against the ledger. All transactions must be endorsed. The other members of the network do this endorsement. After endorsement, transactions are sent to an {{site.data.keyword.IBM_notm}}-provided ordering service.

Besides the core blockchain components, the Enterprise Membership option provides an infrastructure with secure data storage and communications (TLS), and high availability.  While Fabric networks share these infrastructure resources, isolation is provided for the Fabric component nodes in a network, and each node executes in a secure Docker container that protects the execution environment.

The sole aspect that must be determined is the size of the peers that the network requires. This decision is based on the number of channels that are required, plus the workload per channel, memory usage, and disk space (storage).

You should use Enterprise Plan for more stable, production, or almost production level deployments.



## Pricing
{: #enterprise-plan-about-pricing}

To use the Enterprise plan, network members must pay $1,000 per month as the membership fee and additionally $1,000 per month for each of their peers in the network.  The monthly fees are billed daily prorated.  For example, a member (associated membership fee of $1,000) of two peers (per peer fee of $1,000 X 2 peers) needs to pay $3,000 every month.  If the month has 30 days, the member pays $100 ($3,000/30) every day.  Note that if you need high availability (HA), you must double your required peer numbers to provide the HA capabilities.

Network members can pay their bill with their own {{site.data.keyword.cloud_notm}} accounts that contain the space to create the network instance. Alternatively, one network member can cover the bill for all members in the network. For more information about how to pay for the blockchain networks, see [Paying for the network](/docs/blockchain?topic=blockchain-paying-mode#paying-mode).
