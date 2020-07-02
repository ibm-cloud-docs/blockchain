---

copyright:
  years: 2019, 2020
lastupdated: "2020-06-22"

keywords: Pricing, pricing examples

subcollection: blockchain

---

{:external: target="_blank" .external}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:important: .important}
{:codeblock: .codeblock}
{:note: .note}
{:term: .term}
{:tip: .tip}
{:download: .download}

# Detailed pricing scenarios
{: #ibp-detailed-pricing}

This topic includes additional detailed pricing and sizing scenarios to help you learn more about the potential costs of various {{site.data.keyword.blockchainfull}} Platform for  {{site.data.keyword.cloud_notm}} network configurations.
{: shortdesc}

**Target audience**: This advanced topic is designed for architects and system administrators who are responsible for planning and sizing the cost of an {{site.data.keyword.blockchainfull_notm}} Platform network on {{site.data.keyword.cloud_notm}}.

The costs that are provided in this topic are based on the usage of **{{site.data.keyword.cloud_notm}} Kubernetes service clusters**. The costs are guidelines and do not represent actual costs. They represent a starting point for estimates of costs that would be incurred in environments with a similar configuration. Actual costs can vary by geography.  The prices reflected here are based on the resources required by **Fabric v1.4** peer images and actual prices as of October 16, 2019. It is possible the prices can change. The performance of your configuration depends on your smart contracts and the queries you are running. To determine your actual costs, it is important that you test the configuration to verify that the performance satisfies your requirements.
{: important}

If you need to first learn more about the basic elements that factor into the cost a blockchain network, review the topic on [Pricing for {{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}}](/docs/blockchain?topic=blockchain-ibp-saas-pricing).
{: note}

## Scenario comparisons
{: #ibp-pricing-details-tabs}

The following table includes configuration and cost information for three types of environments:
- **Functional Test/Demo**
- **Pilot**
- **Production or Pre-Production**  

Click a tab to view the details for a configuration. Each tab contains two options:

- **Cost to host** This column represents the cost to host a blockchain network, therefore the configuration includes an ordering service, CAs, and peers.
- **Cost to join** This column represents the cost to join an existing blockchain network by contributing peers or an new organization with peers to the network. This configuration includes only CA and peer nodes.

For example, consider the scenario where a group of _organizations_ want to transact on a blockchain network. The network might include buyers, suppliers, distributors, and banks. If you are **hosting** a network, you could host the ordering service and some organizations such as the buyers, and suppliers. Or, if you are a distributor, you might prefer to simply **join** the existing network. The tables below includes the costs for each type of scenario.

Storage costs are calculated based on {{site.data.keyword.cloud_notm}} File Storage costs.<br>
For {{site.data.keyword.blockchainfull_notm}} Platform cost estimation purposes, **1 VPC = 1 CPU = 1 vCPU = 1 Core**.

| | Assumptions | Cost to Host | Cost to Join |
|:-|:-----------------|:-----------------|:-----------------|
| **Goal:** A simple test or demo environment. Larger peers for more robust integration testing. <br><br> You can join the peers to multiple channels to better simulate a real environment or put them all in one account. <br><br> This network would typically include 2-3 organizations. | Configurations include: <br><br><br><br>CouchDB<br><br>File Storage (Bronze)<br><br>Not included: HA, Backup, [HSM](#x6704988){: term}, LogDNA, Sysdig, Dedicated hardware/HW</li></ul> | **Total estimated cost per month: $606 USD** <br><br> Includes: <br><br>**Single node Raft ordering service**<br><br>0.35 vCPU/0.7GB RAM/100GB Storage<br><br>**2 CAs**<br><br>2 x (0.1 vCPU/0.2GB RAM/20 GB Storage)<br><br>**1 Peer** <br> 1.1 vCPU/2.8 GB RAM/100 GB Storage<ul><li> **Peer container:** 0.2 vCPU/1GB RAM/50GB Storage</li><li>**Couch container:** 0.2 vCPU/0.4GB RAM/50GB Storage</li><li>**Smart contract container:** 0.5 vCPU/1GB RAM/0GB Storage</li><li> **Logging/gRPC Web container:** 0.2 vCPU/0.4GB RAM/0GB Storage</li></li></ul> | **Total estimated cost per month: $368 USD** <br><br> Includes: <br><br>**1 CA**<br><br> 0.1 vCPU/0.2GB RAM/20 GB Storage<br><br>**1 Peer**<br> 1.1 vCPU/2.8 GB RAM/100 GB Storage<ul><li> **Peer container:** 0.2 vCPU/1GB RAM/50GB Storage</li><li> **CouchDB container:** 0.2 vCPU/0.4GB RAM/50GB Storage</li><li> **Smart contract container:** 0.5 vCPU/1GB RAM/0GB Storage</li><li>**Logging/gRPC Web container:** 0.2 vCPU/0.4GB RAM/0GB Storage</li></li></ul>|
| **Total Resources** |  | **vCPU:** 1.65<br> **RAM:** 3.9GB<br> **Storage:** 240GB|  **vCPU:** 1.2<br> **RAM:** 3.0GB<br> **Storage:** 120GB|
| **Approximate Total Cost** <br> (per hour) | |**IBP:** 1.65vCPU x .29/hr = $0.48 USD  <br> **IKS***:** **$0.31 USD <br> **Storage:** $0.05 USD** <br><br>** **Total:** 0.84 USD/hr <br> <br> ***IKS 4x16 single node cluster with IP allocation (Shared hardware)| **IBP:** 1.2vCPU x .29/hr = $0.35 USD  <br> **IKS***:** **$0.13 USD <br> **Storage:** $0.03 USD <br><br> **Total:** 0.51 USD/hr  <br> <br> ***IKS 2x4 single node cluster with IP allocation (Shared hardware)|
| **Approximate Total Cost** <br> (per month) | |**IBP:** $346 USD  <br> **IKS:** $224 USD <br> **Storage:**$36 USD <br><br> **Total Base Cost:** $606 USD|  **IBP:** $252 USD  <br> **IKS:** $94 USD <br> **Storage:**$22 USD <br><br> **Total Base Cost:** $368 USD | |
{: caption="Table 1. Pricing Scenarios" caption-side="top"}
{: #simpletabtable5}
{: tab-title="Functional Test/Demo"}
{: tab-group="IAM-simple"}
{: class="simple-tab-table"}

| | Assumptions | Cost to Host | Cost to Join |
|:-|:-----------------|:-----------------|:-----------------|
|**Goal:** A relatively large test environment for evaluating an overall solution for eventual production.  <br>  <br> Includes multiple organizations and slightly increased resources for HA/Performance, but not meant for production. <br> <br> HA protection from node failure. If one node goes down, for maintenance for example, the network can still be fully operational. If zero downtime is not important for this Pilot environment, you could use a smaller configuration, such as a single zone, 4 node 4 x 16 cluster. <br><br>This network would typically include 3-5 organizations.| Configuration includes: <br><br>CouchDB<br><br>File Storage (Bronze)<br><br>HA via node redundancy <br><br>Optional: Backup Tests,<br> PostgreSQL Database,<br>File Storage (Silver),<br> [HSM](#x6704988){: term},<br> LogDNA/Sysdig,<br> Dedicated hardware</li></ul> | **Total estimated cost per month: $3,737 USD** <br><br>Includes:<br> <br><br>**5 node Raft ordering service**<br><br>5 x (1 vCPU/1GB RAM/500GB Storage) <br><br>**2 CAs**<br><br>2 x (0.1 vCPU/0.2GB RAM/20 GB Storage) <br><br>**2 Peers** <br><br>2 x (2.2 vCPU/4.4GB RAM/ 750 GB Storage)  </li><ul><li> **Peer container:** 0.5 vCPU/1GB RAM/250GB Storage</li><li> **CouchDB container:** 0.5 vCPU/1GB RAM/500GB Storage</li><li> **Smart contract container:** 1 vCPU/2GB RAM/0GB Storage</li><li> **Logging/gRPC Web container:** 0.2 vCPU/0.4GB RAM/0GB Storage</li></li></ul></ul> |**Total estimated cost per month: $1,816 USD** <br><br>Includes: <br><br><br> **1 CA**<br><br>0.1 vCPU/0.2GB RAM/20 GB Storage<br><br> **2 Peers** <br><br> 2 x (2.2 vCPU/4.4GB RAM/ 750 GB Storage)  <ul><li> **Peer container:** 0.5 vCPU/1GB RAM/250GB Storage</li><li>**CouchDB container:** 0.5 vCPU/1GB RAM/500GB Storage</li><li>**Smart contract container:** 1 vCPU/2GB RAM/0GB Storage</li><li>**Logging/gRPC Web container:** 0.2 vCPU/0.4GB RAM/0GB Storage</li></li></ul></ul>|
| **Total Resources** |  | **vCPU:** 9.6<br> **RAM:** 14.2GB<br> **Storage:** 4040GB|  **vCPU:** 4.5<br> **RAM:** 9GB<br> **Storage:** 1520GB|
| **Approximate Total Cost** <br> (per hour) | |**IBP:** 9.6vCPU x .29/hr = $2.79 USD  <br> **IKS***:** **$1.59 USD <br> **Storage** (Bronze): $0.81 USD <br><br> **Total:** $5.19 USD/hr <br> <br> *** IKS 1 zone x 3 node - 8 x 32 cluster with IP allocation (Shared hardware)| **IBP:** 4.5vCPU x .29/hr = $1.31 USD  <br> **IKS***:** **$0.90 USD <br> **Storage** (Bronze): $0.31 USD <br><br> **Total:** 2.52 USD/hr <br><br> *** IKS 1 zone x 3 node - 4 x 16 cluster with IP allocation (Shared hardware)|
| **Approximate Total Cost** <br> (per month) | |**IBP:** $2,008 USD  <br> **IKS:** $1,145 USD <br> **Storage** (Bronze):$584 USD  <br><br>**Total Base Cost:** $3,737 USD|  **IBP:** $944 USD  <br> **IKS:** $648 USD <br> **Storage** (Bronze):$224 USD <br><br> **Total Base Cost:** $1,816 USD |
| **Optional upgrades** | | **HA CA (PostgreSQL):** +$40 USD/month <br><br> **HSM (when available):** +$1,250 USD/month <br><br> **Storage (Silver):**+$224 USD/month <br> <br> **LogDNA:** $1.50/GB/Month (7 day search) <br><br> **Sysdig:** 0.035/hour/node (up to 20 containers)|**HA CA (PostgreSQL):** +$40 USD/month <br><br> **HSM (when available):** +$1,250 USD/month <br><br> **Storage (Silver):**+$105 USD/month  <br><br> **LogDNA:** $1.50/GB/Month (7 day search) <br><br> **Sysdig:** 0.035/hour/node (up to 20 containers) | |
{: caption="Table 1. Pricing Scenarios" caption-side="top"}
{: #simpletabtable5}
{: tab-title="Pilot"}
{: tab-group="IAM-simple"}
{: class="simple-tab-table"}


| | Assumptions | Cost to Host | Cost to Join |
|:-|:-----------------|:-----------------|:-----------------|
|**Goal:** Production environment. Pre-production should match production as closely as possible in terms of size, performance, HA, and capabilty.  <br><br> This network would typically include 5 or more organizations. | Configuration includes: <br><br>CouchDB<br><br>File Storage (Silver) <br><br>HA with multi-zone region ([MZR](#x9774820){: term})<br><br>Backup (Backup test in pre-prod, Run backups regularly in Prod)<br><br> HA CA PostgreSQL Database (2 CAs with 2 replica sets) <br><br>  Optional: <br> [HSM](#x6704988){: term}, <br>LogDNA/Sysdig, <br> Dedicated hardware</ul> | **Total estimated cost per month: $5,778 USD** <br><br>  Includes: <br><br>**5 node Raft ordering service**<br><br>5 x (1 vCPU/1GB RAM/500GB Storage) <br><br>**4 CAs**<br><br>4 x (0.1 vCPU/0.2GB RAM) + (2 x 20 GB shared storage) <br><br>**2 Peers** <br><br>2 x (3.7 vCPU/8.4GB RAM/ 750 GB Storage)<br>  </li><ul><li> **Peer container:** 1 vCPU/2GB RAM/250GB Storage</li><li> **CouchDB container:** 1 vCPU/2GB RAM/500GB Storage</li><li> **Smart contract container:** 1.5 vCPU/4GB RAM/0GB Storage</li><li> **Logging/gRPC Web container:** 0.2 vCPU/0.4GB RAM/0GB Storage</li></li></ul></ul> | **Total estimated cost per month: $3,467 USD** <br><br> Includes: <br><br> **2 CAs**<br><br>2 x (.1 vCPU/2 GB RAM) + (20 GB shared storage) <br><br>**2 Peers** <br><br>2 x (3.7 vCPU/8.4GB RAM/ 750 GB Storage) <br><ul><li> **Peer container:** 1 vCPU/2GB RAM/250GB Storage</li><li>**CouchDB container:** 1 vCPU/2GB RAM/500GB Storage</li><li>**Smart contract container:** 1.5 vCPU/4GB RAM/0GB Storage</li><li>**Logging/gRPC Web container:** 0.2 vCPU/0.4GB RAM/0GB Storage</li></ul></ul>|
| **Total Resources** |  | **vCPU:** 12.8<br> **RAM:** 22.6GB<br> **Storage:** 4040GB|  **vCPU:** 7.6<br> **RAM:** 17.2GB<br> **Storage:** 1520GB|
| **Approximate Total Cost** <br> (per hour) | |**IBP:** 12.8vCPU x .29/hr = $3.71 USD  <br> **IKS***:** **$2.74 USD <br> **Storage** (Silver): $1.21 USD  <br><br> **Total:** $7.67 USD/hr <br> <br> *** IKS 3 zone x 3 node - 4x16 cluster with IP allocation and load balancer (Shared hardware)| **IBP:** 7.6vCPU x .29/hr = $2.20 USD  <br> **IKS***:** **$1.85 USD <br> **Storage** (Silver): $0.46 USD <br><br> **Total:** $4.51 USD/hr <br><br> *** IKS 3 zones x  2 node - 4 x16 cluster with IP allocation and load balancer (Shared hardware)|
| **Approximate Total Cost** <br> (per month) | **HA CA PostgreSQL config:** <br>5 GB disk, 1 GB RAM, 10GB for backups, 1 vCPU|**IBP:** $2,671 USD  <br> **IKS:** $1,972 USD <br> **Storage** (Silver):$871 USD <br> **HA CA (PostGreSql):** $40 USD <br>**Storage costs for backups:**$224 USD <br><br>**Total Base Cost:** $5,778 USD <br><br> **LogDNA:** $1.50/GB/Month (7 day search)<br> <br> **Sysdig:** 0.035/hour/node (up to 20 containers)|  **IBP:** $1,584 USD  <br> **IKS:** $1,332 USD <br> **Storage**(Silver):$331 USD <br> <br> **HA CA (PostGreSql):** $40 USD <br>**Storage costs for backups:**$180 USD <br><br> **Total Base Cost:** $3,467 USD <br><br> **LogDNA:** $1.50/GB/Month (7 day search) <br><br> **Sysdig:** 0.035/hour/node (up to 20 containers) |
| **Optional upgrades** | | **HSM (when available):** +$1,250 USD/month <br><br> **Dedicated  IKS hardware:** +$1,848 USD/month <br>|**HSM (when available):** +$1,250 USD/month <br><br> **Dedicated  IKS hardware:** +$1,236 USD/month  | |
{: caption="Table 1. Pricing Scenarios" caption-side="top"}
{: #simpletabtable5}
{: tab-title="Pre-Production/Production"}
{: tab-group="IAM-simple"}
{: class="simple-tab-table"}
