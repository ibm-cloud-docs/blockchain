---

copyright:
  years: 2019, 2020
lastupdated: "2020-06-02"

keywords: IBM Blockchain Platform, blockchain

subcollection: blockchain

---

{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:note: .note}
{:important: .important}
{:javascript: data-hd-programlang="javascript"}
{:java: data-hd-programlang="java"}
{:tip: .tip}
{:pre: .pre}
{:external: target="_blank" .external}

# Upgrading to the {{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}}
{: #enterprise-upgrade}

You can use the {{site.data.keyword.blockchainfull_notm}} Platform upgrade tool to migrate your Enterprise Plan network to {{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}} (referred to as {{site.data.keyword.blockchainfull_notm}} Platform 2.0). The upgrade tool allows you to create new nodes on the {{site.data.keyword.blockchainfull_notm}} Platform 2.0 side by side with your Enterprise Plan network. Your new nodes are joined to your existing channels and contain the same ledger data as your old network. You can continue to use your Enterprise Plan network throughout the upgrade process, providing you the ability to migrate your applications without any downtime. When you can successfully process transactions with your nodes on the {{site.data.keyword.blockchainfull_notm}} Platform 2.0, you can safely delete your network on Enterprise Plan.

Before you start using the upgrade tool, review Before you begin](#enterprise-upgrade-considerations). You can then review the [Upgrade overview](#enterprise-upgrade-overview) to learn about the recommended steps for migrating a blockchain consortium to the new platform.

## Before you begin
{: #enterprise-upgrade-considerations}

If possible, Enterprise Plan customers are encouraged to migrate to the {{site.data.keyword.blockchainfull_notm}} Platform 2.0 right away. If you do not need to preserve your ledger data on Enterprise Plan, you are encouraged to skip the upgrade process and create a new network on the {{site.data.keyword.blockchainfull_notm}} for {{site.data.keyword.cloud_notm}}. If you are ready to create a new network, go to [Getting Started with {{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}}](/docs/blockchain/reference?topic=blockchain-ibp-v2-deploy-iks#ibp-v2-deploy-iks).

It is helpful to become familiar with the {{site.data.keyword.blockchainfull_notm}} Platform 2.0 console before you start the upgrade process. See [Getting Started with {{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}}](/docs/blockchain/reference?topic=blockchain-ibp-v2-deploy-iks#ibp-v2-deploy-iks) to learn how to deploy an instance of {{site.data.keyword.blockchainfull_notm}} Platform 2.0. You can then use the [Build a network tutorial](/docs/blockchain/reference?topic=blockchain-ibp-console-build-network#ibp-console-build-network) to learn how to use the console to operate your new network.

You can only use the upgrade tool to migrate your network to the {{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}}. You cannot use the tool to migrate your network to {{site.data.keyword.blockchainfull_notm}} Platform v2.1.
{:important}

## Upgrade Overview
{: #enterprise-upgrade-overview}

The migration process involves steps that need to be completed by {{site.data.keyword.IBM_notm}} and the organizations in your network.

1. **Scheduled by the network founder and then completed by IBM:** [Upgrade your Enterprise Plan network to Fabric v1.4.3](#enterprise-upgrade-overview-one)
2. **Done by all network members:** [Update your applications to use service discovery](#enterprise-upgrade-overview-two)
3. **Done by all network members:** [Use the upgrade tool to create new nodes on {{site.data.keyword.blockchainfull_notm}} Platform 2.0](#enterprise-upgrade-overview-three)
4. **Done by all network members:** [Migrate your chaincode to the {{site.data.keyword.blockchainfull_notm}} Platform 2.0](#enterprise-upgrade-overview-four)
5. **Done by all network members:** [Download a new connection profile from {{site.data.keyword.blockchainfull_notm}} Platform 2.0](#enterprise-upgrade-overview-five)
6. **Done by all network members:** [Delete your Enterprise Plan network](#enterprise-upgrade-overview-six)

All of the members of your blockchain network can use these steps to migrate to the new platform without experiencing network downtime or making extensive changes to their client applications.

If your network uses an application that was built with Hyperledger Composer, you need to follow a different set of steps to upgrade to the new platform. For more information, see [Upgrading a network that uses Hyperledger Composer](#enterprise-upgrade-composer).

After you start the upgrade process for your Enterprise network, you cannot use the {{site.data.keyword.cloud_notm}} Kubernetes Service, or the {{site.data.keyword.blockchainfull_notm}} Platform 2.0 console to manage the new nodes until the entire network migration is successfully completed.
{: important}

## Step one: Have your Enterprise Plan network upgraded to Fabric v1.4
{: #enterprise-upgrade-overview-one}

If you choose to upgrade your Enterprise Plan network to {{site.data.keyword.blockchainfull_notm}} Platform 2.0, {{site.data.keyword.IBM_notm}} will send the founder of your network an email to schedule the date that your network is upgraded. Make sure that the network owner monitors the email address that is associated with their {{site.data.keyword.cloud_notm}} account.

On the day that you have scheduled your upgrade, {{site.data.keyword.IBM_notm}} will upgrade your Enterprise Plan network from Fabric v1.1 to Fabric v1.4.3. {{site.data.keyword.IBM_notm}} can then migrate your ordering service from Kafka to RAFT consensus. Upgrading to RAFT allows you to create new ordering nodes on {{site.data.keyword.blockchainfull_notm}} Platform 2.0. You can learn more about the features of Fabric v.1.4.3 by going to the [Fabric documentation](https://hyperledger-fabric.readthedocs.io/en/release-1.4/whatsnew.html){: external}.

You will not be able to submit transactions to your network while your ordering service is being migrated from Kafka to RAFT. The length of downtime will depend on the number of channels in your Enterprise Plan network. The upgrade of your network to Fabric v.1.4.3 might cause breaking changes that will require updates to your application. For more information, see [Prepare for the breaking changes from Fabric v1.4](/docs/blockchain?topic=blockchain-enterprise-upgrade-applications#enterprise-upgrade-applications-one).

## Step two: Update your applications to use service discovery
{: #enterprise-upgrade-overview-two}

When your Enterprise Plan network is running on Fabric v1.4.3, you can update your client application to take advantage of the [Service Discovery Feature](https://hyperledger-fabric.readthedocs.io/en/release-1.4/discovery-overview.html){: external} of Hyperledger Fabric. Using service discovery makes it easier to upgrade to {{site.data.keyword.blockchainfull_notm}} Platform 2.0:
- Applications can find all of the peers and ordering nodes that are created on the new platform, eliminating the need to coordinate when your network members shut down your Enterprise Plan peers.
- You can complete and test all of the required application updates before you start using the upgrade tool.
- Using service discovery prepares your application for long-term use of the new platform.

For these reasons, it is recommended that all of the organizations in your network update their applications to use service discovery before they start using the upgrade tool. For guidance on how you can update your applications, see [Updating your applications](/docs/blockchain/reference?topic=blockchain-enterprise-upgrade-applications#enterprise-upgrade-applications).

## Step three: Get started with the upgrade tool
{: #enterprise-upgrade-overview-three}

After your Enterprise Plan network is upgraded to Fabric v1.4.3 and a RAFT ordering service, you can start using the upgrade tool to create nodes on the {{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}}. For more information on how to use the tool and how the tool works, see [Getting started with the upgrade tool](/docs/blockchain/reference?topic=blockchain-enterprise-upgrade-tool#enterprise-upgrade-tool).

Each organization that is a member of an enterprise network will get one instance of the upgrade tool. For example, if you have a staging network and a production network with two members each, then your blockchain network will use four instances of the upgrade tool.

If you are operating the upgrade tool on behalf of multiple organizations, you need to make sure that you are using the instance of the tool that corresponds to the organization that you are migrating. You need to keep the identities that are generated by each upgrade tool separate and upload those identities to the instance of {{site.data.keyword.blockchainfull_notm}} Platform 2.0 that you created for that organization.
{: important}

You cannot use the {{site.data.keyword.blockchainfull_notm}} Platform 2.0 console or APIs to add or remove nodes from your network until the upgrade is complete. If you want to remove upgraded nodes for any reason, such as a deployment failure, you need to remove the nodes using the upgrade tool. Otherwise, your Enterprise Plan network and your network on {{site.data.keyword.blockchainfull_notm}} Platform 2.0 will become out of synch.
{:note}

## Step four: Migrate your chaincode to {{site.data.keyword.blockchainfull_notm}} Platform 2.0
{: #enterprise-upgrade-overview-four}

After all of the organizations in your network have used the upgrade tool to create peer and ordering nodes on the {{site.data.keyword.blockchainfull_notm}} Platform 2.0, you can use the upgrade tool to migrate your chaincode. The chaincode that was instantiated on your channels by the Enterprise Plan Network Monitor or the Swagger APIs cannot be invoked from peers on the new platform. You can use the upgrade tool to install a new version of the chaincode that can be invoked to endorse transactions from either platform.

After all the members of your channel have migrated their chaincode to the new platform using the upgrade tool, you can upgrade the chaincode on the channel using the {{site.data.keyword.blockchainfull_notm}} Platform console. After you upgrade the chaincode on your channels, you can start submitting transactions to your new nodes on {{site.data.keyword.blockchainfull_notm}} Platform 2.0. If any channel members have not used the upgrade tool to install the new chaincode version on their peers, they will no longer be able to endorse transactions after the upgrade. For more information, see [Migrating your smart contracts](/docs/blockchain/reference?topic=blockchain-enterprise-upgrade-tool#enterprise-upgrade-tool-smart-contracts).  


## Step five: Download a new connection profile from {{site.data.keyword.blockchainfull_notm}} Platform 2.0
{: #enterprise-upgrade-overview-five}

After you use the upgrade tool to migrate your chaincode, you can start using your applications to submit transactions to your new nodes on the {{site.data.keyword.blockchainfull_notm}} Platform 2.0. If you have updated your applications to use service discovery, you can download a new connection profile from the {{site.data.keyword.blockchainfull_notm}} Platform 2.0 console and import it into your application. If you cannot update your applications, follow the instructions for [updating your applications manually](/docs/blockchain/reference?topic=blockchain-enterprise-upgrade-applications#enterprise-upgrade-applications-manual).

The {{site.data.keyword.blockchainfull_notm}} Platform 2.0 connection profile can be downloaded from the **Organization MSP** tile on the **Organizations** panel. You can use the connection profile for all of your channels if you joined the same peers to each channel. For more information, see [Connect with SDK](/docs/blockchain/reference?topic=blockchain-ibp-console-smart-contracts#ibp-console-smart-contracts-connect-to-SDK-panel). Chaincode is referred to as smart contracts on the {{site.data.keyword.blockchainfull_notm}} Platform 2.0 console UI.

After you import the new connection profile, you need to use the Certificate Authority on the {{site.data.keyword.blockchainfull_notm}} Platform 2.0 to enroll a new identity for your application. Your application can then use service discovery to submit transactions to your nodes on the new platform and the nodes that remain on Enterprise Plan.

## Step six: Delete your Enterprise Plan network
{: #enterprise-upgrade-overview-six}

When your applications can add transactions to the ledger by using the nodes on {{site.data.keyword.blockchainfull_notm}} Platform 2.0, you can safely delete your network on Enterprise Plan. Before you delete your network, you must ensure that the other organizations in your consortium have updated their applications to use service discovery or that they have updated their applications to target the endpoints of your new nodes. If other applications continue to target your peers on Enterprise Plan, their transactions might fail to meet the chaincode endorsement policy and might not be committed to the ledger.

Before you delete your network, you should use the Network Monitor to stop each of your peers to confirm that they are not being used to endorse transactions. If no problems are detected after you stop your Enterprise network peers, you can remove the peers by using the Network Monitor. You can then delete your Enterprise Plan network.

## Upgrading a network that uses Hyperledger Composer
{: #enterprise-upgrade-composer}

Hyperledger Composer is not supported on the {{site.data.keyword.blockchainfull_notm}} Platform 2.0. As a result, you need to port your application to use the new Fabric programming model to upgrade your Enterprise Plan network. You can use the following steps to port your application and upgrade your network:

1. You can start developing new smart contracts and applications right away by creating a test environment on Hyperledger Fabric v1.4. Fabric v1.4 provides a new programing model that makes it easier to write smart contracts and submit transactions to the network. The new model allows you to port an application from a Hyperledger Composer business network to an application that uses the APIs provided by Fabric smart contracts and the Fabric SDKs. The programming model is not available on lower versions of Fabric, requiring you to develop and deploy your application on Fabric v1.4. You can use the [{{site.data.keyword.blockchainfull_notm}} extension for VS Code](/docs/blockchain/reference?topic=blockchain-develop-vscode#develop-vscode) development tool to write and test your new smart contracts.

  You can also use your test environment to practice the process of porting from Composer to your new application. You can deploy a [Hyperledger Composer Business Network at 0.20.8](https://github.com/hyperledger/composer/releases/tag/v0.20.8){: external} to a network that runs on Fabric v1.4 and create sample transaction data. You can then port your sample Composer 0.20.8 application to the application that you developed using the new programming model.

  You can create a test network on Fabric 1.4 by deploying a local instance of [Hyperledger Fabric](https://hyperledger-fabric.readthedocs.io/en/release-1.4/getting_started.html){: external}. If you have a test network on Enterprise Plan, {{site.data.keyword.IBM_notm}} can upgrade that network to Fabric v1.4.3. When you are ready to get started, go to the [Hyperledger Composer Porting Guide](https://davidkel.github.io/docs/Porting/TOC.html){: external}. If you want assistance for porting your application, engage [{{site.data.keyword.blockchainfull_notm}} services](https://www.ibm.com/blockchain/services){: external}.

2. {{site.data.keyword.IBM_notm}} will upgrade your Enterprise Plan network from Fabric v1.1 to Fabric v1.4.3. In addition to the new programming model, Fabric v1.4.3. provides important features that will help you upgrade your Enterprise Plan network to the next generation of the {{site.data.keyword.blockchainfull_notm}} Platform such as Raft ordering services and service discovery. You can learn more about the features of Fabric v1.4.3 by going to the [Fabric documentation](https://hyperledger-fabric.readthedocs.io/en/release-1.4/whatsnew.html){: external}. {{site.data.keyword.IBM_notm}} will send you an email to schedule the date that your network is upgraded.

3. When your Enterprise Plan network is running on Fabric v1.4.3, you need to upgrade your Business Network to use Composer v0.20.8. To learn how to upgrade your application, see [Migrating from Composer 0.19.x to Composer 0.20.8](https://davidkel.github.io/docs/0.19to0.20/doc.html){: external}. Because Hyperledger Composer v0.19 supports only Fabric v1.1, you might encounter problems running Composer v0.19 on Fabric v1.4. You should update your application as soon as your Enterprise Plan network runs Fabric v1.4.3. It is recommended that you test upgrading to Composer v0.20.8 on a local instance of Fabric before your Enterprise Plan network is upgraded to Fabric v1.4.3.

4. After you upgrade to Composer v0.20.8, you can port your application from Composer your new application. You can then continue to use your new application to submit transactions to your Enterprise Plan network until you upgrade to the {{site.data.keyword.blockchainfull_notm}} Platform 2.0.

5. When you are finished porting from Composer to the new programing model, you can start using the upgrade tool to create new nodes on {{site.data.keyword.blockchainfull_notm}} Platform 2.0. The new programming model takes advantage of the service discovery feature of Hyperleder Fabric. This allows your application to find all of the peers and ordering nodes that are created on the new platform, eliminating the need to coordinate the creation of new nodes between the members of your network. For more information on how to use the tool and how the tool works, see [Getting started with the upgrade tool](/docs/blockchain/reference?topic=blockchain-enterprise-upgrade-tool#enterprise-upgrade-tool).

## Support
{: #enterprise-upgrade-support}

If you encounter an issue during the upgrade process, open a support ticket by using the {{site.data.keyword.cloud_notm}} Service Portal. For more information, see [Submitting support cases](/docs/blockchain?topic=blockchain-blockchain-support#blockchain-support-cases).
