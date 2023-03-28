---

copyright:
  years: 2017, 2023
lastupdated: "2023-03-28"

keywords: IBM Blockchain Platform offerings, VS code extension, IBM Cloud

subcollection: blockchain

---

{{site.data.keyword.attribute-definition-list}}

# Getting started with {{site.data.keyword.blockchainfull_notm}} Platform
{: #get-started-ibp}

{{site.data.keyword.blockchainfull}} Platform provides a managed and full stack blockchain-as-a-service (BaaS) offering that allows you to deploy blockchain components in environments of your choice. Clients can build, operate, and grow their blockchain networks with an offering that can be used from development through production.
{: shortdesc}

Before you use an {{site.data.keyword.blockchainfull_notm}} Platform offering, read the technical and support information in the [Disclaimer](/docs/blockchain?topic=blockchain-disclaimer#disclaimer) section.
{: important}

## Which {{site.data.keyword.blockchainfull_notm}} Platform offering is right for your business?
{: #get-started-console-ocp-which-ibp}

{{site.data.keyword.blockchainfull_notm}} Platform provides different offerings that allow you to deploy your network in the environment of your choice. You need to decide if you want to deploy the {{site.data.keyword.blockchainfull_notm}} Platform 2.5.4 or if you want to use the {{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}}.

| |{{site.data.keyword.blockchainfull_notm}} Platform for anywhere (2.5.4) | {{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}} |
|----|---|----|
| Where do you want to deploy the platform?|  Multiple Kubernetes distributions on a private, public, or hybrid **multicloud** <br><br> See [Supported Platforms](https://www.ibm.com/docs/en/blockchain-platform/2.5.4?topic=started-about-blockchain-platform-254#console-ocp-about-prerequisites)) | A Kubernetes cluster on {{site.data.keyword.cloud_notm}} <br><br> See [Supported configuration](/docs/blockchain?topic=blockchain-ibp-console-overview#ibp-console-overview-supported-cfg) |  
| What are my deployment options? | <ul><li> Full platform </li> <li> <a href="#get-started-ibp-images">Only {{site.data.keyword.blockchainfull_notm}} images</a> </li> </ul>| <ul><li> Full platform </li> </ul>
| How is it billed? |Contact us for [pricing](https://www.ibm.com/docs/en/blockchain-platform/2.5.4?topic=253-pricing) |[$0.29 USD per allocated CPU hour](/docs/blockchain?topic=blockchain-ibp-saas-pricing)  |
| Can I connect with peers in other clouds? |  Yes| Yes |
| Can my data center be [on-prem](#x4561212){: term} and behind a firewall? | Yes| No |
| Can I use a console UI to deploy and manage my blockchain components? | Yes | Yes|
| Are APIs available for node management? | Yes | Yes|
| I am an experienced Fabric customer. Are peer, CA, orderer, and smart contract images available and supported? | Yes | No |
| Where can I learn more? | See [About {{site.data.keyword.blockchainfull_notm}} Platform 2.5.4](https://www.ibm.com/docs/en/blockchain-platform/2.5.4?topic=started-about-blockchain-platform-254) | See [About {{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}}](/docs/blockchain?topic=blockchain-ibp-console-overview#ibp-console-overview-capabilities) |
| Ready to get started? | See [Step one: Install the IBM Blockchain Platform](https://www.ibm.com/docs/en/blockchain-platform/2.5.4?topic=started-getting-blockchain-platform-254#get-started-console-ocp-step-two-deploy-console) | See [Getting started with {{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}}](/docs/blockchain?topic=blockchain-ibp-v2-deploy-iks) |
{: caption="Table 1. Which offering is right for your business?" caption-side="bottom"}

Have questions and want to speak to an {{site.data.keyword.blockchainfull_notm}} Platform expert? [Schedule a consult](https://www.ibm.com/cloud/blockchain-platform/developer?schedulerform){: external} now to learn more about how blockchain can transform your business.


### {{site.data.keyword.blockchainfull_notm}} images
{: #get-started-ibp-images}

- Your purchase of {{site.data.keyword.blockchainfull_notm}} Platform 2.5.4 includes an entitlement to images for [peer](#x2033450){: term}, [Certificate Authority](#x2016383){: term}, [ordering service](#x9826021){: term}, and [smart contract](#x8888420){: term} containers that are signed by IBM. The images are based on the open source Hyperledger Fabric code base and contain a number of enhancements. The images are bundled with support from IBM. The {{site.data.keyword.blockchainfull_notm}} images do not include the {{site.data.keyword.blockchainfull_notm}} management console or operator. This offering is intended for experienced Fabric users with existing Fabric deployments. For more information, see [{{site.data.keyword.blockchainfull_notm}} images for Hyperledger  Fabric](/docs/blockchain-sw-254?topic=blockchain-sw-254-blockchain-images#blockchain-images).

## Next steps
{: #get-started-ibp-next-steps}

After you deploy {{site.data.keyword.blockchainfull_notm}} Platform in the environment of your choice, you can set up the network with consortium, nodes, channels, smart contracts, and start transactions on it. For more information, see the task topics under the **HOW TO** section in the left navigation of this documentation.

## Getting support
{: #get-started-ibp-getting-support}

{{site.data.keyword.IBM_notm}} offers various support options on {{site.data.keyword.IBM_notm}}-implemented blockchain solutions. For more information about {{site.data.keyword.blockchainfull_notm}} Platform support, see [Getting support](/docs/blockchain?topic=blockchain-blockchain-support#blockchain-support).
