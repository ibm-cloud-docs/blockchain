---

copyright:
  years: 2017, 2020
lastupdated: "2019-09-24"

keywords: blockchain network, Enterprise Plan, getting started tutorial

subcollection: blockchain

---

{:external: target="_blank" .external}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:important: .important}
{:pre: .pre}
{:tip: .tip}

# Getting started with Enterprise Plan
{: #getting-started-with-enterprise-plan}

{{site.data.keyword.blockchainfull}} Platform Enterprise Plan provides a blockchain network with high security, integrity, scalability, and performance. You can quickly provision a fully functional network and use the Network Monitor, which is a GUI dashboard, to immediately run [chaincode](/docs/blockchain?topic=blockchain-glossary#glossary-chaincode) and applications without having to design and configure a network from scratch.
{:shortdesc}

Enterprise Plan has been withdrawn. Therefore, new clients cannot provision Enterprise Plan networks at this time. Existing clients can continue to add new members and create new networks until December 31, 2019. However, no new Enterprise Plan networks can be created after that date. New users can make use of the latest user interface and features available now in the second generation of blockchain technology by visiting [{{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}}](/docs/blockchain?topic=blockchain-ibp-v2-deploy-iks).
{: important}

This tutorial introduces the prerequisites and steps you need to follow to get an Enterprise Plan network that is hosted in {{site.data.keyword.IBM_notm}}'s highly available and secure environment.

The following steps indicate the basic flow to launch an Enterprise Plan network with multiple network [members](/docs/blockchain?topic=blockchain-glossary#glossary-member):
1. A **network initiator**, as one special type of network member, creates the network and defines governance policies. The network initiator can then invite other [organizations](/docs/blockchain?topic=blockchain-glossary#glossary-organization) to join this network as network members. For more information, see [Creating a network](/docs/blockchain?topic=blockchain-getting-started-with-enterprise-plan#getting-started-with-enterprise-plan-create-network).
2. Invited **network members** receive an email notification that provides instructions for them to join a {{site.data.keyword.blockchain}} network. Besides the instructions in the email notification, you can also follow the steps in [Joining a network](/docs/blockchain?topic=blockchain-getting-started-with-enterprise-plan#getting-started-with-enterprise-plan-join-nw).
3. All **network members**, after creating or joining a network, can enter the Network Monitor to configure and manage their network resources. You can set up [channels](/docs/blockchain?topic=blockchain-glossary#glossary-channel) with a group of network members to execute private transactions on a channel-specific ledger, which only channel members can access. In the Network Monitor, you can also join your own peers to the channel, and then install and instantiate chaincode on them. For more information, see [Configuring network resources and environment](/docs/blockchain?topic=blockchain-getting-started-with-enterprise-plan#getting-started-with-enterprise-plan-config).
4. **Application developers**, after developing applications, enable the interaction between their applications and the network. For more information, see [Enabling applications to interact with the network](/docs/blockchain?topic=blockchain-getting-started-with-enterprise-plan#getting-started-with-enterprise-plan-enable-apps).
5. **Network operators** monitor transactions on their channels in the Network Monitor. For more information, see [Monitoring network resources](/docs/blockchain?topic=blockchain-getting-started-with-enterprise-plan#getting-started-with-enterprise-plan-monitor-resources).


## Creating a network
{: #getting-started-with-enterprise-plan-create-network}

Before you begin, you need to create a [{{site.data.keyword.blockchain}} Platform service instance](https://cloud.ibm.com/catalog/services/ibm-blockchain-5-prod){: external} on {{site.data.keyword.cloud_notm}}. You need to log in with your {{site.data.keyword.cloud_notm}} ID. If you do not have an ID, click the **Sign up to Create** button. Rename the service and credential names for your instance so you can recognize it easily in the future. Select the {{site.data.keyword.cloud_notm}} region, organization, and space, where you can deploy your {{site.data.keyword.blockchain}} network. Then, select **Enterprise Membership Plan** from the pricing plans table and click the **Create** button.

You can find your {{site.data.keyword.blockchain}} Platform service instance in your [{{site.data.keyword.cloud_notm}} service dashboard](https://cloud.ibm.com/resources){: external}.

If you are a network initiator, click the **Create Network** button to initiate a {{site.data.keyword.blockchain}} network. Follow the wizard to complete the basic configuration of your network and resources.
![Create Network wizard](images/create_network_name.png "Create Network wizard")

1. In the "Let's Get Started" screen, give a name to your network, choose the location of your {{site.data.keyword.cloud_notm}} organization, and add the name of your institution. When you invite other network members, they will look for this network name to join. Click **Next**.
2. (Optional) In the "Invite Members" screen, enter the institution name and the email address of the member who you want to invite to your network. The institution name that you designate is not an official title. It simply allows the institution to be easily recognized and can be changed when they join the network. Note that a network can have up to 15 members including yourself. This step is optional, and you can invite members to your network later in the Network Monitor. Click **Next**.
	Members that you invite will receive an email notification about your invitation after you complete all of the steps to create the network.
3. In the "Define Governance Rules" screen, establish the policies for membership, channel creation, and chaincode. By default, all network members can invite other members to join the network, create channels, and instantiate chaincode. Currently, your network uses the default governance policies. Click **Next**.
4. In the "Review Summary" screen, verify your network configuration. If you want to make modifications, click **Edit** beside the section header or click the **Previous** button to go back to previous screens. When you complete the network configuration, click **Done**.
5. In the "Network Created" screen, you will be notified that your network has been successfully created. You can click **Add [Peers](/docs/blockchain?topic=blockchain-glossary#glossary-peer)** to configure your network resources or click **Enter Monitor** directly to open the Network Monitor. You can also add peers later in the Network Monitor. For more information about peers, see [Add peers](/docs/blockchain?topic=blockchain-ibp-dashboard#ibp-dashboard-peers).

Now you successfully deploy a {{site.data.keyword.blockchain}} network that can support the following network resources:
* A member-specific Certificate Authority (CA)
* Default governance policies
* Up to 15 network members
* Three orderers and two intermediate CA nodes
* Up to three small peers for each member
* A crash fault tolerant ordering service
* Up to 150 channels
* Up to 10 instantiations of chaincode per member in Network Monitor


## Joining a network
{: #getting-started-with-enterprise-plan-join-nw}

Similar to creating a network, you need to create a [{{site.data.keyword.blockchain}} Platform service instance](https://cloud.ibm.com/catalog/services/ibm-blockchain-5-prod){: external} on {{site.data.keyword.cloud_notm}}. You need to log in with your {{site.data.keyword.cloud_notm}} ID. If you do not have an ID, click the **Sign up to Create** button. Rename the service and credential names for your instance so you can recognize it easily in the future. Select the {{site.data.keyword.cloud_notm}} region, organization, and space, where you can deploy your {{site.data.keyword.blockchain}} network. Then, select **Enterprise Membership Plan** from the pricing plans table and click the **Create** button.

You can find your {{site.data.keyword.blockchain}} Platform service instance in the [{{site.data.keyword.cloud_notm}} service dashboard](https://cloud.ibm.com/resources){: external}.

If you are an invited network member, click the **Pending Invite ->** button, select the network that you want to join from the drop-down list, and click the **Join Network!** button. Follow the wizard to view the network basic configuration of your network and configure your own network resources.
![Join Network wizard](images/join_network_name.png "Join Network wizard")

1. In the "Let's Get Started" screen, enter the name of your organization and click **Next**.
2. In the "Review Governance Rules" screen, review the network's governance policies of membership, channel creation, and chaincode. Click **Next**.
3. (Optional) In the "Add Peers" screen, choose the quantity of peers that you want to add. Click **Next**. Each member in a network can add up to three peers. This step is optional, and you can add your peers later in the Network Monitor. For more information about peers, see [Add peers](/docs/blockchain?topic=blockchain-ibp-dashboard#ibp-dashboard-peers).
4. In the "Review Network Summary" screen, verify the network configuration. If you want to make modifications, click the **Previous** button to go back to previous screens. After you complete the resource configuration, click **Done**. You will be notified that you have successfully joined the network. You can then click **Enter Monitor** to open the Network Monitor.




## Configuring network resources and environment
{: #getting-started-with-enterprise-plan-config}

1. Enter your Network Monitor after you create or join a {{site.data.keyword.blockchain}} network. The Network Monitor is a GUI dashboard where you can manage and track network status information. For more information, see [Network Monitor](/docs/blockchain?topic=blockchain-ibp-dashboard#ibp-dashboard).
2. Add your own peers to the network. If you have already added enough peers, skip this step. Peers run chaincode and they are the endpoint to interact with your applications. Click **Add Peers** in the "Overview" screen, and select the quantity and size of your peers. For more information, see [Overview](/docs/blockchain?topic=blockchain-ibp-dashboard#ibp-dashboard-overview).
3. Set up a channel. All members in the same channel are provisioned a channel-specific ledger, which delivers data isolation and confidentiality. For more information about how to create a channel, see [Creating a channel](/docs/blockchain?topic=blockchain-ibp-create-channel#ibp-create-channel-creating-a-channel). If you are a channel member who is invited to join a channel, you will receive an email notification with a link to the wizard that allows you to join the channel.
4. Join peers to the channel. Only peers that are associated with the channel can access its ledger. For more information, see [Channels](/docs/blockchain?topic=blockchain-ibp-dashboard#ibp-dashboard-channels).
5. Install and instantiate chaincode. All channel members need to install the same chaincode with the same name and version on every peer that will run the chaincode. After you install the chaincode, you need to instantiate it on the channel before you can use it. For more information, see [Installing, instantiating, and updating a chaincode](/docs/blockchain?topic=blockchain-install-instantiate-chaincode#install-instantiate-chaincode).

**Note**: To achieve high availability, each organization must purchase at least two peers, and within a channel, each participating member must join at least two peers.


## Retrieving network credentials and connection profile
{: #getting-started-with-enterprise-plan-retrieve-credentials}

After you create an Enterprise Plan network in {{site.data.keyword.cloud_notm}}, you can retrieve the network credentials and connection profile from the service instance page or in the Network Monitor.

### Retrieving from the service instance page
{: #getting-started-with-enterprise-plan-retrieve-svc-ins}
You are on the service instance page right after you create a service instance. You can also click your service in the [{{site.data.keyword.cloud_notm}} service dashboard](https://cloud.ibm.com/resources){: external} to open your service instance page.

Retrieve your service credentials with the following steps:
1. On the service instance page, click **Service credentials** in the left navigator to show the "Service credentials" screen.
2. Click **New credential** in the "Service credentials" screen.
3. In the "Add new credential" screen, give the credential a name and enter **{"type": "service_instance_token"}** in the "Add inline configuration parameter" field. Click **Add** and the new credential is added in the table. You can click **View credentials** under the "ACTIONS" column to view the credential details. This credential contains the API key and secret, which you can use to authorize APIs.

### Retrieving in the Network Monitor
{: #getting-started-with-enterprise-plan-retrieve-nw}
You can find the network credentials on the "APIs" screen in your Network Monitor. For more information about using APIs, see [Trying out APIs with Swagger](/docs/blockchain?topic=blockchain-ibp-swagger#ibp-swagger).

You can retrieve the connection profile on the "Overview" screen in your Network Monitor. Click the **Connection Profile** button on the "Overview" screen and the connection profile shows in a new page.


## Enabling applications to interact with the network
{: #getting-started-with-enterprise-plan-enable-apps}
Applications leverage the SDK APIs to interact with your {{site.data.keyword.blockchain}} network resources. You need to add the API endpoint information of your network resources in your application so that the application can ultimately target your peers with transaction requests. You can then add the API endpoint information from the Network Monitor. Applications can be hosted on your local file system or on {{site.data.keyword.cloud_notm}}.

## Monitoring network resources
{: #getting-started-with-enterprise-plan-monitor-resources}
After a transaction is triggered from your application, you can view transaction status information in the Network Monitor. For more information about network monitoring, see [Monitoring a network](/docs/blockchain?topic=blockchain-monitor-blockchain-network#monitor-blockchain-network).

## Leaving a network
{: #getting-started-with-enterprise-plan-leave-nw}
If you want to leave a network, delete the blockchain service instance from your {{site.data.keyword.cloud_notm}} dashboard.

Before you leave a network, ensure that you are not a member in any channels of the network. Otherwise, you will get errors when you leave the network. A channel member can remove you from the channel using the channel update process. For more information about the channel update process, see [Updating a channel](/docs/blockchain?topic=blockchain-ibp-create-channel#ibp-create-channel-updating-a-channel).
{:note}
