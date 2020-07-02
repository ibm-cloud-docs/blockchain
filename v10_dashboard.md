---

copyright:
  years: 2017, 2020
lastupdated: "2019-10-03"

keywords: Network Monitor, peer nodes, resources, channels, smart contract

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
{:gif: data-image-type='gif'}

# Using the Network Monitor
{: #ibp-dashboard}

{{site.data.keyword.blockchainfull}} Platform brings a Network Monitor to provide an overview of your blockchain environment, including network resources, members, joined channels, transaction performance data, and deployed chaincode. The Network Monitor also offers you the entry point to run Swagger APIs, develop a network with {{site.data.keyword.blockchainfull_notm}} Platform, and try sample applications.
{:shortdesc}

Use this tutorial to learn how to use your Network Monitor to operate an Enterprise Plan network.

## Left navigation pane
{: #ibp-dashboard-left-navigation}

The Network Monitor exposes the following screens in three sections. You can navigate to each screen from the left navigator in the Network Monitor.
- The **My network** section contains the "[Overview](/docs/blockchain?topic=blockchain-ibp-dashboard#ibp-dashboard-overview)", "[Members](/docs/blockchain?topic=blockchain-ibp-dashboard#ibp-dashboard-members)", "[Channels](/docs/blockchain?topic=blockchain-ibp-dashboard#ibp-dashboard-channels)", "[Notifications](/docs/blockchain?topic=blockchain-ibp-dashboard#ibp-dashboard-notifications)", "[Certificate Authority](/docs/blockchain?topic=blockchain-ibp-dashboard#ibp-dashboard-ca)", and "[APIs](/docs/blockchain?topic=blockchain-ibp-dashboard#ibp-dashboard-apis)" screens.
- The **My code** section contains the "[Develop code](/docs/blockchain?topic=blockchain-ibp-dashboard#ibp-dashboard-write-code)", "[Install code](/docs/blockchain?topic=blockchain-ibp-dashboard#ibp-dashboard-chaincode)", and "[Try samples](/docs/blockchain?topic=blockchain-ibp-dashboard#ibp-dashboard-samples)" screens.
- The "[Get help](/docs/blockchain?topic=blockchain-ibp-dashboard#ibp-dashboard-support)" screen shows support information as well as release notes for helios and Hyperledger Fabric (the code base the {{site.data.keyword.blockchainfull_notm}} Platform is based on).

The name of your blockchain network is at the top of the left navigation pane. You can [change the name of your network](/docs/blockchain?topic=blockchain-ibp-dashboard#ibp-dashboard-network-name) in the Network Monitor.

You can [check and configure network preferences](/docs/blockchain?topic=blockchain-ibp-dashboard#ibp-dashboard-network-preferences) from the drop-down menu on the upper right of the Network Monitor.

This tutorial describes each of the above screens and functions.

## Overview
{: #ibp-dashboard-overview}

The "Overview" screen displays real-time status information on your blockchain resources, including the orderer, CA, and peer nodes. Each resource is displayed under four distinct headers: **Type**, **Name**, **Status**, and **Actions**. During the creation of your blockchain network, three orderer nodes and two CA nodes are automatically created. The CAs are member-specific, whereas the orderers are common endpoints that are shared across the network.

**Figure 1** shows the "Overview" screen:

![Overview screen](images/myresources.png "Network overview"){: caption="Figure 1. Network overview" caption-side="bottom"}

### Node actions
{: #ibp-dashboard-node-actions}

The **Actions** header of the table provides buttons to start or stop your resources. You can also start or stop a group of nodes by selecting multiple nodes and then clicking the **Start Selected** or **Stop Selected** button. The **Start Selected** or **Stop Selected** button appears on top of the table when you select one or more nodes.

The Stop and Start actions are not available for an Orderer node. In general, there is no need to stop and start Peer or CA nodes on a network. The Stop and Start actions are provided in case you needed to restart a peer, for example to bring it up in a clean state.

You can also check component logs by clicking **View Logs** from the drop-down list under the **Actions** header. The logs expose the calls between the various network resources and are useful for debugging and troubleshooting. For more information on using your network logs, see [Monitoring a blockchain network](/docs/blockchain?topic=blockchain-monitor-blockchain-network#monitor-blockchain-network)

To understand the effects of starting and stopping a peer, you can experiment by stopping a peer and attempting to target it with a transaction, and you will see connectivity errors in the logs. When you restart the peer and attempt the transaction again, you will see a successful connection. You can also leave a peer down for an extended period of time as your channels continue to transact. When the peer is brought back up, you will notice a synchronization of the ledger as it receives the blocks that were committed when it was down. After the ledger is fully synchronized, you can perform normal invokes and queries against it.

### Connection Profile
{: #ibp-dashboard-connection-profile}

You can view the JSON file about low-level network information of each resource by clicking the **Connection Profile** button. The connection profile contains all the configuration information that you need for an application. However, because this file contains only the addresses for your specific components and the orderer, if you need to target additional peers, you need to obtain their endpoints. The header that contains "url" displays the API endpoint of each component. These endpoints are required in order to target specific network components from a client-side application and their definitions will typically live in a JSON-modeled configuration file that accompanies the app. If you are customizing an application that requires endorsement from peers that are not part of your organization, you need to retrieve the IP addresses of those peers from the relevant operators in an out-of-band operation. Clients must be able to connect to any peers from which they need a response.

### Add peers
{: #ibp-dashboard-peers}

Network members deploy [peers](/docs/blockchain?topic=blockchain-blockchain-component-overview#blockchain-component-overview-peer) to store their copies of network ledger and to run chaincode to query or update the ledger. If the endorsement policy defines a peer as an endorsing peer, the peer also returns endorsement results to applications.

Click the **Add Peers** button at the upper right to add peer nodes to your network. In the pop-up "Add Peers" panel, select the number and size of peer nodes you want to add. You can add more peers for your organizations based on your own requirements. You might be in different scenarios when you need more peers. For example, you might want multiple peers to join the same channel for redundancy. Each peer processes the channel's transactions and writes to their respective copies of the ledger. If one of the peers fails, the other peer (or multiple other peers) can continue processing transactions and application requests. You can also symmetrically load balance all application requests across the peers, or you could target different peers for different functions. For example, you can use one peer to query the ledger and use another peer to process endorsements for ledger updates.

## Members
{: #ibp-dashboard-members}

The "Members" screen contains two tabs to display network member information in the "Members" tab and certificate information in the "Certificates" tab.

### Add members to Enterprise Plan networks
{: #ibp-dashboard-members-tab}

**Figure 3** shows the initial "Members" screen that displays your network members in the "Members" tab:

![Members tab in Members screen](images/monitor_members.png "Network members"){: caption="Figure 3. Network members" caption-side="bottom"}

You can invite other members in the "Members" tab to add to those that are initially invited when you create the network. To invite a member to your network, enter the institution name and operator's email address and click **Add Member**. A network can have a total of 15 members (including the network initiator). To remove a member from your network, click the "remove" symbol at the end of the member row.


### Certificates
{: #ibp-dashboard-certificates}

**Figure 5** shows the initial "Members" screen that displays member certificates in the "Certificates" tab:

![Certificates tab in Members screen](images/monitor_certificates.png "Certificates"){: caption="Figure 5. Certificates" caption-side="bottom"}

Operators can manage the certificates for the members in the same institution in the "Certificates" tab. Click **Add Certificate** to open the "Add Certificate" panel. Give a name to your certificate, paste your client-side certificates in PEM format to the "Key" field, and click **Submit**. You need to restart your peers before the client-side certificates can take effect.

## Channels
{: #ibp-dashboard-channels}

Consisting of a subset of network members who want to transact privately, channels provide data isolation and confidentiality by allowing the members of a channel to establish specific rules and a separate ledger, which only channel members can access. Every network must have at least one channel for transactions to take place. Each channel has a unique ledger and users must be properly authenticated to perform read/write operations against this ledger. If you're not on a channel, you can't see any data.

**Figure 6** shows the initial dashboard screen displaying an overview of all channels in your network:

![Channels](images/channels.png "Channels"){: caption="Figure 6. Channels" caption-side="bottom"}

Creating a channel results in the generation of a channel-specific ledger. For more information, see [Creating a channel](/docs/blockchain?topic=blockchain-ibp-create-channel#ibp-create-channel).

You can also select an existing channel to view more precise details about the channel, membership, and active chaincode. For more information, see [Monitoring a network](/docs/blockchain?topic=blockchain-monitor-blockchain-network#monitor-blockchain-network).

If you have uploaded a new certificate to the platform by using the ["Certificates" tab](/docs/blockchain?topic=blockchain-ibp-dashboard#ibp-dashboard-certificates) of the "Members" screen, you can use this panel to add the certificate to a channel. Click **Sync Certificate** from the drop-down list under the **Actions** header next to the relevant channel. This allows you to operate the channel from a remote client, including being able to instantiate a chaincode on the channel. For more information, see [Uploading signing certificates to {{site.data.keyword.blockchainfull_notm}} Platform](/docs/blockchain?topic=blockchain-managing-certificates#managing-certificates-upload-certs) in the [Managing certificates](/docs/blockchain?topic=blockchain-managing-certificates#managing-certificates) tutorial.

## Notifications
{: #ibp-dashboard-notifications}

When you create a channel or are invited to a new channel, a notification appears in the Network Monitor. You can view and respond to these requests in the "Notifications" screen.

**Figure 7** shows the "Notifications" screen:

![Notifications](images/notifications.png "Notifications"){: caption="Figure 7. Notifications" caption-side="bottom"}

The requests are grouped into "All", "Pending", and "Completed" subtabs. Numbers after the subtab header indicate the number of requests in each subtab.
   * You can find all your requests in the "All" subtab.
   * Requests that you have not accepted or declined, or you have not viewed, are in the "Pending" subtab. Click the **Review Request** button to view the request, which includes the channel policy and members, and voting status. If you are a channel operator, you can either **Accept** or **Decline** the request, or handle it at another time by clicking **Later**. If the request is accepted by enough channel operators, you can click **Submit Request** to activate the channel update.
   * A submitted request will appear in the "Completed" subtab. You can click **Review Request** to view its details.

When you have a long list of requests, you can search for a request in the search field on the top.

Pending requests can be deleted by selecting the boxes in the front of them and clicking **Delete Request**. Note that a completed request cannot be deleted.

## Certificate Authority
{: #ibp-dashboard-ca}

The table on "Certificate Authority" (CA) screen displays all of the identities that have been registered with your organization, including your admin, peers, and client applications. You can also use this screen to register a new identity.

**Figure 8** shows the "Certificate Authority" screen:

![Certificate Authority](images/CA_screen.png "Certificate Authority"){: caption="Figure 8. Certificate Authority" caption-side="bottom"}

Click the **Generate Certificate** button next to your admin identity to get a new public certificate and private key from your CA. The **Certificate** field contains the public certificate, also referred to as the signCert or enrollment cert, just above the **Private Key**. You can click the copy icon at the end of each field to copy the value. This panel can be used an alternative way to generate a public and private key pair for a client application which uses the Fabric SDK. **Note** that {{site.data.keyword.blockchainfull_notm}} Platform doesn't store these certificates. You need to safely save and store them.

Click the **Add User** button to register a new identity to your organization. In the **Add User** pop-up window, complete the following fields and then click **Submit**.
  - **Enroll ID:** This will be the name of your new identity that is sometimes referred to as your `enroll ID`. **Save this Value** and you need to use it when you configure a remote peer or enroll a new application.
  - **Enroll Secret:** This will be the password to your identity that is sometimes referred to as your `enroll Secret`. **Save this Value** and you need to use it when you configure a remote peer or enroll a new application.
  - **Type:** Select the type of identity that you want to register, either peer or client application.
  - **Affiliation:** This will be the affiliation within your organization, such as `org1` for example, that the identity will belong to.
  - **Maximum Enrollments:** You can use this field to limit the number of times that your can enroll or generate certificates with this identity. If you leave the field blank, the value defaults to an unlimited number of enrollments.

You can learn more about your CA by visiting the [Managing certificates on {{site.data.keyword.blockchainfull_notm}} Platform](/docs/blockchain?topic=blockchain-managing-certificates#managing-certificates) tutorial.

## APIs
{: #ibp-dashboard-apis}

{{site.data.keyword.blockchainfull_notm}} Platform exposes a number of REST APIs in Swagger that you can use to manage the nodes, channels, peers, and members of your network. Your applications can use these APIs to control important network resources without using the Network Monitor.

**Figure 9** shows the "APIs" screen:

![APIs](images/API_screen.png "APIs"){: caption="Figure 9. APIs" caption-side="bottom"}

Click the **Swagger UI** link to open the Swagger UI. Note that you need to authorize the Swagger UI with your network credentials (which can be found on this APIs page) before you can run the APIs. For more information, see [Interacting with the network using Swagger APIs](/docs/blockchain?topic=blockchain-ibp-swagger#ibp-swagger).

## Develop Code
{: #ibp-dashboard-write-code}

{{site.data.keyword.IBM_notm}} does not provide support for networks that use Hyperledger Composer in production, including the Composer CLI, JavaScript APIs, REST server, and Web Playground.{:note}

Enterprise Plan provides a development environment with industry standard tools and technologies. After you develop a network, you can deploy it to your network.

**Figure 10** shows the "Develop code" screen:

![Develop code](images/write_code.png "Develop code"){: caption="Figure 10. Develop code" caption-side="bottom"}

For more information about developing and deploying your business networks, see [Deploying business networks on Enterprise Plan](/docs/blockchain?topic=blockchain-deploying-a-business-network#deploying-a-business-network).

## Install code
{: #ibp-dashboard-chaincode}

Chaincode, which is also known as "smart contract", is pieces of software that contains a set of functions to query and update the ledger. They are installed on peers and instantiated on a channel.

**Figure 11** shows the "Install code" screen:

![Install code](images/chaincode_install_overview.png "Install code"){: caption="Figure 11. Install code" caption-side="bottom"}

A chaincode is first installed on a peer's file system and then instantiated on a channel. For more information, see [Installing, instantiating, and updating a chaincode](/docs/blockchain?topic=blockchain-install-instantiate-chaincode#install-instantiate-chaincode).

## Try samples
{: #ibp-dashboard-samples}

Sample applications help you to get a better understanding of a blockchain network and application development. Follow the **View on GitHub** links to learn how to use the samples and deploy them to {{site.data.keyword.blockchainfull_notm}} Platform. For more information on how to develop and deploy your samples, see [Deploying Sample Applications](/docs/blockchain?topic=blockchain-deploying-sample-applications#deploying-sample-applications).

**Figure 12** shows the "Try samples" screen:

![Try samples](images/sample_overview_ep.png "Try samples"){: caption="Figure 12. Samples" caption-side="bottom"}

## Get help
{: #ibp-dashboard-support}

The "Get help" screen contains a "Support" tab that provides a list of resources for developers and a "Release Notes" tab that describes new functions on {{site.data.keyword.blockchainfull_notm}} Platform.

**Figure 13** displays the information in the initial "Support" tab:

![Support](images/support.png "Support"){: caption="Figure 13. Blockchain support" caption-side="bottom"}

### Blockchain resources and support forums
{: #ibp-dashboard-support-forums}

Use the resources in the "Support" tab to troubleshoot problems and get help from {{site.data.keyword.IBM_notm}} and the Fabric community. For more information about the links on the "Support" tab, see [Resources and support forums](/docs/blockchain?topic=blockchain-blockchain-support#blockchain-support-resources) in [Getting support](/docs/blockchain?topic=blockchain-blockchain-support#blockchain-support).

If you cannot debug your issue or ascertain an answer to your question, submit a support case in the {{site.data.keyword.cloud_notm}} Service Portal. For more information, see [Submitting support cases](/docs/blockchain?topic=blockchain-blockchain-support#blockchain-support-cases).

### Fabric release notes
{: #ibp-dashboard-release-notes}

The "Release Notes" tab displays the latest features of your network. The "Network Monitor UI" button lists new functions and bug fixes for the {{site.data.keyword.blockchainfull_notm}} Platform user experience. The "Hyperledger Fabric" button will direct you to the release notes for your network's version of Hyperledger Fabric and the Fabric Certificate Authority.

**Figure 14** displays the release notes for the Network Monitor UI.

![Release notes helios](images/releasenotes_helios.png "Release notes of Network Monitor UI"){: caption="Figures 14. Release notes for Network Monitor UI" caption-side="bottom"}

**Figure 15** displays the release notes for your networks version of Hyperledger Fabric and the Fabric Certificate Authority.

![Release notes Fabric](images/releasenotes_Fabric.png "Release notes of Fabric"){: caption="Figures 15. Release notes for Fabric" caption-side="bottom"}

## Network preferences
{: #ibp-dashboard-network-preferences}

Click the upper right corner and open the drop-down menu and then the **Network preferences**. The Network preferences window opens. The Network preferences window shows the basic information of your network, such as network name, Fabric version, network location in {{site.data.keyword.cloud_notm}}, and state database type.

**Enterprise Plan networks** that are created after May 15th, 2018 will run on Hyperledger Fabric v1.1.1. If you create networks after the upgrade, you can also manage web inactivity timeout and mutual TLS for your network in the Network preferences window. These settings can be changed by the network initiator only.

### Web inactivity timeout
{: #ibp-dashboard-web-inactivity-timeout}

**Note**: Only **network initiator** can change the web inactivity timeout setting. This is a network level setting and will affect all network members.

The web inactivity timeout is set to **Off** by default. If you turn the web inactivity timeout to **On**, any member of the network will be logged out automatically after 10 minutes of inactivity. When the web inactivity timer reaches 10 minutes, the web inactivity timeout function ends the inactive web sessions to ensure security of the network member's account. Clicking a link or refreshing the Network Monitor resets the web inactivity timer. Before reaching 10 minutes, closing the browser window or tab also ends the web session.

**Figure 16** shows the "Network preferences" window:

![Network preferences](images/network_preferences.gif "Network preferences"){: caption="Figure 16. Network preferences" caption-side="bottom"}{: gif}

### Mutual TLS (for Enterprise Plan networks)
{: #ibp-dashboard-mutual-tls}

**Enterprise Plan networks** offer you the ability to enable Mutual TLS to secure the communication between your application and your blockchain components.

**Note**: Only a **network initiator** can enable or disable the mutual TLS. This is a network level setting and will affect all network members.

The mutual TLS button is set to **Off** by default. If you enable mutual TLS, you need to update your applications to support this function. Otherwise, your applications will not be able to communicate with your network.

For a Fabric 1.1 Enterprise plan network, each organization has its own mutual TLS Certificate Authority (CA). The information required to connect to the mutual TLS CA is available in the [Connection profile](/docs/blockchain?topic=blockchain-ibp-dashboard#ibp-dashboard-connection-profile) accessible from your **Overview** screen in the Network Monitor by clicking the **Connection Profile** button. The connection profile contains the necessary information to connect to the CA and get the certificates you need to connect to your network.

In the Connection Profile, locate the `certificateAuthorities` section where you will find the following attributes that are necessary to enroll and get the certificates to communicate with your network using Mutual TLS.

- `url`: URL for connecting to the CA that can give out mutual TLS certificates
- `enrollId`: Enroll ID to use for getting a certificate
- `enrollSecret`: Enroll secret to use for getting a certificate
- `x-tlsCAName`: CA name to use for getting certificate that will allow the application to communicate with Mutual TLS.

For more information about updating your applications to support mutual TLS, see [How to configure mutual TLS](https://hyperledger.github.io/fabric-sdk-node/release-1.4/tutorial-mutual-tls.html){: external}.



**Figure 17** shows the "Network preferences" window:

![Network preferences](images/network_preferences_ep_tmp.png "Network preferences"){: caption="Figure 17. Network preferences" caption-side="bottom"}

## Update network name
{: #ibp-dashboard-network-name}

When you create an instance of Enterprise Plan, {{site.data.keyword.blockchainfull_notm}} Platform assigns a name to your network. However, you can update this network name at anytime in your Network Monitor.

On the top of the left navigator in the Network Monitor, click the network name and the field becomes editable. Type the new network name that you want to use and press the **Enter** key. Your network name will be updated in a few seconds.
