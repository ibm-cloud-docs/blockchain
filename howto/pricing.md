---

copyright:
  years: 2017, 2020
lastupdated: "2020-06-04"

keywords: Enterprise Plan, peer fee, membership fee

subcollection: blockchain

---

{:external: target="_blank" .external}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:deprecated: .deprecated}
{:important: .important}
{:pre: .pre}

# Pricing
{: #ibp-pricing}

This guide helps you understand pricing for Enterprise plan, and how much you will pay as you develop and grow your blockchain network.
{:shortdesc}

Enterprise Plan has been withdrawn. Therefore, new clients cannot provision Enterprise Plan networks at this time. Existing clients can continue to add new members and create new networks until December 31, 2019. However, no new Enterprise Plan networks can be created after that date. New users can make use of the latest user interface and features available now in the second generation of blockchain technology by visiting [{{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}}](/docs/blockchain?topic=blockchain-ibp-v2-deploy-iks).
{: important}

Enterprise Plan charges monthly membership and peer fees to organizations who build blockchain networks.

| Pricing elements | Enterprise Plan cost per month |
|-----|-----|
| Membership fee | $1000 |
| Peer fee | $1000 |

*Figure 1. {{site.data.keyword.blockchainfull_notm}} Platform pricing overview*

The monthly fee is billed daily prorated. For example, a member (associated membership fee of $1,000) of two peers (per peer fee of $1,000 X 2 peers) needs to pay $3,000 every month. If the month has 30 days, the member pays $100 ($3,000/30) every day. For more information about how to pay for your networks, see [Paying mode](/docs/blockchain?topic=blockchain-paying-mode#paying-mode).


## Network basic components
{: #ibp-pricing-components}

To understand pricing, we need to start with an introduction to basic components of a network. {{site.data.keyword.blockchainfull_notm}} Platform enables the creation of a blockchain network that is based on Hyperledger Fabric. At a high level, a Fabric blockchain network consists of the following basic components:

-	**Organizations** – Any entity who needs to maintain a copy of the blockchain ledger and needs to validate transactions. There can be multiple blockchain organizations for a single company.
-	**Peers** – The node associated with an organization that contains the blockchain ledger and validates transactions. Peers are associated with an individual blockchain organization.
-	**Ordering service** – Composed of a single ordering node or a collection of ordering nodes. The ordering service sequences transactions, creates blocks, and sends blocks to peers for validation.
-	**Certificate Authority (CA)** –Issues digital certificates for identification purposes to any interactive network component.

Enterprise Plan provides you with highly available CAs and peers with a crash fault tolerant ordering service.

## Key elements of pricing
{: #ibp-pricing-key-elements}

Enterprise Plan has two pricing elements:

- **Membership fee** – Covers organization creation, access to ordering service and CA, and is charged on a **per-instance** basis. Included in this pricing element, {{site.data.keyword.blockchainfull_notm}} Platform handles the ordering service and the CA on your network’s behalf. This fee is required to have access to a network built on {{site.data.keyword.blockchainfull_notm}} Platform.	Enterprise allows for a single organization per membership. Because Enterprise is designed for pilot and production environments, you are linked to your specific organization.

-	**Peer fee** – Covers an organization’s peers and is charged on a **per-peer** basis. This fee is required only if you would like to have a peer to maintain a copy of the ledger and validate transactions.

## Enterprise Plan pricing
{: #ibp-pricing-enterprise-plan}

{{site.data.keyword.blockchainfull_notm}} Platform doesn't provide default configuration for an Enterprise Plan network. You can choose the configuration that you would like to start with. When you are ready to use Enterprise Plan, you should have a good understanding of what your network configuration should be. As a high availability best practice, we strongly recommend a minimum of two peers per organization to ensure that your organization does not experience a network outage.

If you are in an Enterprise Plan network with the other network member, and each of you add two peers for your organization, each of your bill is reflected in Figure 2.

| Pricing components | Cost per month |
|-----|----------------|
| Membership fee | $1000 |
| Peer fee | $1000 |
| Peer fee | $1000 |
| End of month charge | $3000 |

*Figure 2. Charge of a basic Enterprise Plan network*

### Example Enterprise Plan pricing
{: #ibp-pricing-enterprise-plan-pricing}


#### Additional peers
{: #ibp-pricing-enterprise-additional-peers}

Continue with the example above, if you add another two peers to your organization. You bill increases by $2000 per month. Your bill is reflected in Figure 3:

| Pricing components | Cost per month |
|-----|----------------|
| Membership fee | $1000 |
| Peer fee | $1000 |
| Peer fee | $1000 |
| Peer fee | $1000 |
| Peer fee | $1000 |
| End of first month charge | $5000 |

*Figure 3. Charge of an Enterprise Plan network with four peers*

If the other member keeps the same network configuration, the member’s bill looks the same as before, which is reflected in Figure 2.

#### Additional networks
{: #ibp-pricing-enterprise-additional-networks}

Enterprise Plan also does not restrict the number to network instances that you can provision or join. If you create a second Enterprise Plan network with the same basic network configuration, that is, one single organization with two peers, your bill is reflected in Figure 11. You might find yourself in this scenario when you have created multiple networks, joined multiple networks, or a combination of the two.

| Pricing components | Cost per month |
|-----|----------------|
| Membership fee network 1 | $1000 |
| Peer fee | $1000 |
| Peer fee | $1000|
| Membership fee network 2 | $1000 |
| Peer fee | $1000 |
| Peer fee | $1000|
| Total | $6000 |

*Figure 4. Charge of two Enterprise Plan networks both with basic network configuration*

If the other member uses only one Enterprise Plan network and keeps the same network configuration, the member’s bill looks the same as before, which is reflected in Figure 4.
