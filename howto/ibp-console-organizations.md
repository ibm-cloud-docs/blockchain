---

copyright:
  years: 2019
lastupdated: "2019-02-08"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Managing organizations
{: #ibp-console-organizations}

***[Is this page helpful? Tell us.](https://www.surveygizmo.com/s3/4501493/IBM-Blockchain-Documentation)***

You can use the {{site.data.keyword.blockchainfull}} Platform console to create a formal organization definition known as a Membership Services Provider (MSP). Your organization's MSP definition allows other members of the blockchain consortium to verify the identity of your nodes and applications. Your MSP definition also contains your organization's admin certificates.

You can use the console to manage which organizations are members of your network. The administrator of the ordering service can use the organizations tab to add members to the blockchain [consortium](/docs/services/blockchain/glossary.html#glossary-consortium). Members of the consortium can then use the console to add members to new or existing channels.

## Understanding MSPs
{: #console-organizations-about-msp}

The {{site.data.keyword.blockchainfull_notm}} Platform is based on Hyperledger Fabric and builds permissioned blockchain networks. Participants need to be known to the network before they can submit transactions and interact with the assets on the ledger. Fabric recognizes identity through a group of invited organizations, known as the consortium. Organizations in the consortium are able to issue valid credentials to their members and let them become participants in the network. The participants can then operate blockchain nodes and submit transactions from client applications.

Each organization in the consortium needs to operate its own Certificate Authority, known as the root CA. This Certificate Authority (or its intermediate CAs) creates all the identities belonging to your organization and issues each identity a public and private key. These keys are signed by the CA and used by the members of your organization to sign and verify their actions. Joining the consortium allows other organizations to recognize your CAs signature, and verify that your peers and applications are valid participants. For more information about membership in Hyperledger Fabric, see the [Membership concept topic ![External link icon](../images/external_link.svg "External link icon")](https://hyperledger-fabric.readthedocs.io/en/release-1.4/membership/membership.html "Membership") in the Fabric documentation.

Before your organization can join a consortium, it needs to create an organization definition known as a **Membership Services Provider (MSP)**. The MSP contains the following information:
- A certificate signed by your **root Certificate Authority**. This certificate is used to verify the identity of your nodes, channels, and applications.
- A certificate signed by your **TLS CA**. A root TLS certificate allows your peers to participate in cross organization gossip, which is necessary to take advantage of the [**Private data** collections](/docs/services/blockchain/howto/ibp-console-smart-contracts.html#ibp-console-smart-contracts-private-data) and [Service Discovery ![External link icon](../images/external_link.svg "External link icon")](https://hyperledger-fabric.readthedocs.io/en/release-1.4/discovery-overview.html "Service discovery") features of Hyperledger Fabric.
- The **MSP ID**. The MSP ID is the formal name of your organization within the consortium. You need to remember the MSP ID for your nodes and applications.
- **Admin certificates** from your **Peer admin** and **Org Admin** identities. These certificates are passed to the Ordering Service and are used to verify which identities in your organization are allowed to create or edit channels. When you use your console to create an orderer or peer, the admin certificates inside the MSP are deployed within the new node. These certificates can then be used to operate the peers or orderers from your console or a client application.

## Managing MSPs in the console

Navigate to the **Organizations** tab. You can use this tab to [create an MSP definition](/docs/services/blockchain/howto/ibp-console-organizations.html#console-organizations-create-msp) by using a Certificate Authority that exists in your console. You can also use this tab to [import an MSP](/docs/services/blockchain/howto/ibp-console-organizations.html#console-organizations-import-msp) that was created by another organization.

You can view all of the MSPs that you have created or imported under **Available Organizations**. You can use the MSP definitions in the organizations tab for important functions within your console:
- If you are creating peer or orderer nodes, the MSP of your organization is used to provide admin certificates to the new node.
- If you are the admin of an ordering node, you can use the MSPs to [add new organizations to the consortium](/docs/services/blockchain/howto/ibp-console-organizations.html#console-organizations-add-consortium).
- If you are a member of the consortium, you can import the MSPs of other consortium members into your console and then add the members to new or existing channels.

## Creating an MSP for your organization
{: #console-organizations-create-msp}

Use the **Organizations** tab to generate an MSP definition for your organization. When you click **Create MSP**, a side panel opens that allows you to enter all the necessary information: your Root CA, the MSP ID, and your organization administrators.

- Use the **MSP details** section of the panel to select your organizations MSP ID. This ID is the formal name that other members of the network will use to refer to your organization. **Save your MSP ID.**

- Use the **Root Certificate Authority details** section to select your organization's root CA. This is the CA that you used (or will use) to create all of the node and application identities that belong to your organization. If you use intermediate CAs, this is the CA that you used to create those CAs. Select your root CA from the list of CAs managed by using your console. If you created a CA using the console, selecting a root CA will will also create a root TLS Certificate.

- You can also use the **Root Certificate Authority details** section to generate one of your organizations admin certificates. Before creating your organization MSP definition, you need to register your org and node admins with your root CA. You then need to complete the following steps in order to use these identities to operate your network:

  1. Create a public and private key pair for each admin identity.
  2. Provide the public key (certificate) of each admin identity in the MSP definition.
  3. Add the identity to your console wallet. The nodes or channels created by the console uses the certificates from the MSP to check who is a valid administrator. As a result, the same public private key pair that you used to add an admin cert to the MSP needs to be stored inside your console wallet.

  You can use the **Create MSP definition** panel to complete these actions for one of your admin identities. After you select your root CA, complete the following steps in the **Generate organization admin certificate** section:
  1. Enter the Enroll ID and Enroll secret of an admin identity registered with your root CA. After you enter the Enroll ID and Enroll secret, choose a name to display the identity in your console wallet.
  2. Click **Generate**. This will generate a new set of public and private keys and automatically add the keys to your console wallet. You can then find your admin identity in your wallet by using the name you selected on this panel. These keys are only stored in your browser local storage, therefore if you change browsers, they will not be in your console wallet. You will need to export the identity from your original browser and import them into the console wallet of your new browser.
  3. Then click **Export** to download the key pair to your file system and secure them.

- The **Administrators (Optional)** section of the side panel contains the public keys of your admins. The certificate you generated by using the **Generate organization admin certificate** section can be found first row of the **admin certificate** field. If you want to use multiple admin identities to operate your network, you can paste the certificates of additional node or organization admins into **admin certificate** fields.

Because your admin certs are passed to your nodes and channels by using the MSP definition, you need ensure that each of your node and org admin certificates are stored in the MSP. When you use the console to create an orderer, peer, or channel, you need to **Associate** one of the identities you exported in your console wallet with the admin certificates that were provided to the MSP definition. When you encounter an **Associate identity** section or panel, select an identity you generated and saved to the wallet when creating the MSP definition.

After you have selected your root CA, MSP ID, and created your admin certs, click **Create MSP definition** to create the MSP definition. It is now listed as an organization in the Organizations tab. You will use the MSP definition when you deploy your nodes and join a blockchain consortium.

## Importing an MSP
{: #console-organizations-import-msp}

Only the orderer admin can add new organizations to the consortium. If you are the orderer admin, you will need to collect the MSP definitions of all the organizations who have been invited to the consortium and import the MSPs into the console. You can then add the MSPs to the ordering service, by using the orderer node.

After an administrator creates an MSP definition, they can use the Organizations tab to download the MSP in JSON format to their local filesystem. They can then send you the MSP JSON file in an out of band operation. Navigate to the **Organizations** tab and use **Import MSP Definition** to import the MSP file into your console. Once an MSP definition is visible in the **Available organizations** section, you can then navigate to your orderer node to [add the organization to the consortium](docs/services/blockchain/howto/ibp-console-organizations.html#console-organizations-add-consortium).


## Adding an organization to a consortium
{: #console-organizations-add-consortium}

The consortium of organizations is hosted by the ordering service.

If you are the administrator of the ordering service, you can use the console to add an organization to the consortium. Navigate to the **Nodes** tab and click on the ordering node. On the ordering node panel, under **Consortium members**, click **Add organization**. This will open a side panel that will allow you to select from the list of available MSP definitions that you have [imported into your organizations tab](/docs/services/blockchain/howto/ibp-console-organizations.html#console-organizations-import-msp). You can also use the **Upload JSON** option to import the MSP definition file created by another org directly.

## Creating and editing a channel

Once an organization has been added to the consortium, they can use the ordering service to create a new channel or be added to a channel. The information that allows you to participate in a channel, such joining your peers to the channel, instantiating smart contracts, and submitting transactions, is provided by using the MSP definitions.

After an organization has been added to a consortium, they can create a channel by using the following steps:

1. Import the ordering node that hosts the consortium into their console. They do not need to be an administrator of the ordering node. But their console needs to have the orderer name and endpoint information.
2. Import the MSPs of organizations they want to add to the new channel into their console using the organizations tab. **Note** that organizations need to be added to the consortium before they can be added to a channel.
3. Navigate to the **Channels** tab and click **Create channel**. This will open a side panel that will allow you to specify the channel name, membership, and channel policies. You can add any organizations that have been added to the consortium to the new channel.

For more information about these steps, see [creating a channel](/docs/services/blockchain/howto/ibp-console-build-network.html#ibp-console-build-network-create-channel1) in the **Building a network** tutorial.
