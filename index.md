---

copyright:
  years: 2017, 2020
lastupdated: "2020-06-29"

keywords: IBM Blockchain Platform offerings, VS code extension, IBM Cloud

subcollection: blockchain

---

{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:note: .note}
{:important: .important}
{:tip: .tip}
{:term: .term}
{:pre: .pre}
{:external: target="_blank" .external}

# Getting started with {{site.data.keyword.blockchainfull_notm}} Platform
{: #get-started-ibp}

{{site.data.keyword.blockchainfull}} Platform provides a managed and full stack blockchain-as-a-service (BaaS) offering that allows you to deploy blockchain components in environments of your choice. Clients can build, operate, and grow their blockchain networks with an offering that can be used from development through production.
{:shortdesc}

Before you use an {{site.data.keyword.blockchainfull_notm}} Platform offering, read the technical and support information in the [Disclaimer](/docs/blockchain?topic=blockchain-disclaimer#disclaimer) section.
{: important}

## Which {{site.data.keyword.blockchainfull_notm}} Platform offering is right for your business?
{: #get-started-console-ocp-which-ibp}

{{site.data.keyword.blockchainfull_notm}} Platform provides different offerings that allow you to deploy your network in the environment of your choice. You need to decide if you want to deploy the {{site.data.keyword.blockchainfull_notm}} Platform 2.5 or if you want to use the {{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}}.

| |{{site.data.keyword.blockchainfull_notm}} Platform for anywhere (2.5) | {{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}} |
|----|---|----|
| Where do you want to deploy the platform?|  Multiple Kubernetes distributions on a private, public, or hybrid multicloud <br><br> See [Supported Platforms](/docs/blockchain-sw-25?topic=blockchain-sw-25-console-ocp-about#console-ocp-about-prerequisites) | A Kubernetes cluster on {{site.data.keyword.cloud_notm}} <br><br> See [Supported configuration](/docs/blockchain?topic=blockchain-ibp-console-overview#ibp-console-overview-supported-cfg) |  
| What are my deployment options? | <ul><li> Full platform </li> <li> [Only {{site.data.keyword.blockchainfull_notm}} images](#get-started-ibp-images) </li> </ul>| <ul><li> Full platform </li> </ul>
| How is it billed? |Contact us for [pricing](/docs/blockchain-sw-25?topic=blockchain-sw-25-ibp-sw-pricing) |[$0.29 USD per allocated CPU hour](/docs/blockchain?topic=blockchain-ibp-saas-pricing)  |
| Can I connect with Peers in other clouds? |  Yes| Yes |
| Can my data center be [on-premises](#x4561212){: term} and behind a firewall? | Yes| No |
| Can I use a console UI to deploy and manage my blockchain components? | Yes | Yes|
| Are APIs available for node management? | Yes | Yes|
| Does the product integrate with the {{site.data.keyword.blockchainfull_notm}} Platform VSCode extension to develop and test my smart contracts?| Yes | Yes|
| I am an experienced Fabric customer. Are peer, CA, orderer, and smart contract images available and supported? | Yes | No |
| Where can I learn more? |See [About {{site.data.keyword.blockchainfull_notm}} Platform 2.5](/docs/blockchain-sw-25?topic=blockchain-sw-25-console-ocp-about#console-ocp-about-offers)  | See [About {{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}}](/docs/blockchain?topic=blockchain-ibp-console-overview#ibp-console-overview-capabilities) |
{: caption="Table 1. Which offering is right for your business?" caption-side="bottom"}

### Developer Tools

- [**{{site.data.keyword.blockchainfull_notm}} Platform Developer Tools**](/docs/blockchain?topic=blockchain-develop-vscode#develop-vscode)
  Developers can start with a free VS Code extension IDE that provides an explorer and commands accessible from the command palette for developing smart contracts quickly. Install it locally or run it from the cloud by using Red Hat CodeReady Workspaces.

### {{site.data.keyword.blockchainfull_notm}} images
{: #get-started-ibp-images}

- Your purchase of {{site.data.keyword.blockchainfull_notm}} Platform 2.5 includes an entitlement to images for [peer](/docs/blockchain?topic=blockchain-glossary#glossary-peer), [Certificate Authority](#x2016383){: term}, [ordering service](#x9826021){: term}, and [smart contract](/docs/blockchain?topic=blockchain-glossary#glossary-smart-contracts) containers that are signed by IBM. The images are based on the open source Hyperledger Fabric code base and contain a number of enhancements for stability and serviceability. The images are bundled with support from IBM. The {{site.data.keyword.blockchainfull_notm}} images do not include the {{site.data.keyword.blockchainfull_notm}} management console or operator. This offering is intended for experienced Fabric users with existing Fabric deployments. For more information, see [{{site.data.keyword.blockchainfull_notm}} images for Hyperledger  Fabric](/docs/blockchain-sw-25?topic=blockchain-sw-25-blockchain-images#blockchain-images).


## Next steps
{: #get-started-ibp-next-steps}

After you deploy {{site.data.keyword.blockchainfull_notm}} Platform in the environment of your choice, you can set up the network with consortium, nodes, channels, smart contracts, and start transactions on it. For more information, see the task topics under the **HOW TO** section in the left navigation of this documentation.

## Getting support
{: #get-started-ibp-getting-support}

{{site.data.keyword.IBM_notm}} offers various support options on {{site.data.keyword.IBM_notm}}-implemented blockchain solutions. For more information about {{site.data.keyword.blockchainfull_notm}} Platform support, see [Getting support](/docs/blockchain?topic=blockchain-blockchain-support#blockchain-support).
