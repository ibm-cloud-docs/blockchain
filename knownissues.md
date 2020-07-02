---

copyright:
  years: 2017, 2020

lastupdated: "2019-09-24"

subcollection: blockchain

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}
{:note: .note}
{:important: .important}
{:tip: .tip}
{:external: target="_blank" .external}

# Known issues
{: #known-issues}

This page describes known issues that you might encounter when you use Enterprise Plan.
{:shortdesc}

The following issues are already reported:
- **Configuring an external CA in not supported on Enterprise Plan**. As an alternative, you can generate and upload admin certificates via the Network Monitor. For more information, see the description on the ["Certificates" tab of "Member" screen](/docs/blockchain?topic=blockchain-ibp-dashboard#ibp-dashboard-members) in the Network Monitor.
- **Chaincode containers can sometimes be stopped** by a background network issue and might need to be rebuilt and restarted after a user invokes the chaincode. If this happens it may take your chaincode a few minutes to respond.

For support and help with your {{site.data.keyword.blockchainfull_notm}} Platform network on {{site.data.keyword.cloud_notm}}, see [Getting support](/docs/blockchain?topic=blockchain-blockchain-support#blockchain-support).
