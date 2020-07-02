---

copyright:
  years: 2019, 2020
lastupdated: "2020-06-09"

keywords: network components, IBM Cloud Kubernetes Service, batch timeout, channel update, channels, Raft, channel configuration, access control

subcollection: blockchain

---

{:external: target="_blank" .external}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:note: .note}
{:important: .important}
{:tip: .tip}
{:pre: .pre}

# Advanced channel deployment and management
{: #ibp-console-govern}




When you create a channel, there are a number of advanced options that allow you fine tune the configuration of your channel to fit your use case. In this topic we'll discuss how to edit those options as part of creating a channel or when editing a channel after it has been created.
{:shortdesc}

**Target audience:** This topic is designed for network operators who are responsible for creating, monitoring, and managing channels.

The roles played by the various participants in channel creation, as well as the underlying method used to create and update the channel configuration, is inherited from the [Hyperledger Fabric processes for creating and editing a channel](https://hyperledger-fabric.readthedocs.io/en/release-1.4/configtx.html){: external}.
{:tip}

The parameters that govern how channels function, from the organizations on the channel to their permissions to the policies, are all contained in the channel configuration. When you create a channel, you are asked to decide on a number of these parameters. Some options, such as the channel name, cannot be changed after a channel has been created, while other options can only be changed after the channel has been created.

## General options
{: #ibp-console-govern-update-channel-available-parameters-general}

For an overview of the channel creation process that ignores the advanced options, see [Step four: Create a channel](/docs/blockchain?topic=blockchain-ibp-console-build-network#ibp-console-build-network-create-channel).

* **Channel name**. Channels should be given distinctive names relevant to the purpose of the channel. Each ordering service can only have a single channel with a given name. If you see an error that a name has already been given to a channel on that ordering service, you will have to select a new name.

* **Ordering service**. Channels are hosted on a single ordering service and cannot be migrated from one ordering service to another.

* **Organizations**. This section of the panel is how organizations are added or removed from a channel. Organizations that can be added can be seen in the drop-down list. Note that an organization must be a member of the consortium of the ordering service before it can be added to a channel. For more information about how to add an organization to the consortium, see [Add your organization to list of organizations that can transact](/docs/blockchain?topic=blockchain-ibp-console-build-network#ibp-console-build-network-add-org).

  You can also update an organization's level of permission on the channel:

   - A channel **operator** has permission to create and sign channel configuration updates. There must be at least one operator in each a channel.
   - A channel **writer** can update the ledger by invoking a smart contract. A channel writer can also instantiate a smart contract on a channel.
   - A channel **reader** can only query the ledger, by invoking a read only function in smart contract for example.

* **Update policy**. The update policy of a channel specifies how many organizations (out of the total number of operators in the channel), who must approve of an update to the channel configuration. To ensure a good balance between collaborative administration and efficient processing of channel configuration updates, consider setting this policy to a majority of admins. For example, if there are five admins in a channel, choose `3 out of 5`.

## Advanced options
{: #ibp-console-govern-update-channel-available-parameters-advanced}

By clicking the box under advanced options, users can access parameters that users should take caution in updating. Note that if you are attempting to change any parameter that requires the signature of ordering service admins (for example, the **Block cutting parameters** or **ordering service consenters**) and are one of the ordering service admins on this channel, you will see a field for the ordering service organization. Select the MSP of the relevant ordering service organization from the drop-down list (if you are not an ordering service admin, the MSP will have to be exported to you). If you are not an admin of the ordering service organization, you can still make a request to change one of the block cutting parameters, but the request will be sent, and will need to be signed, by an ordering service admin.

If the signature of an ordering service org admin is required, you will not see a pending tile for the channel in your console until the ordering service org has signed the request. When you see the tile, you can join your peer to it.
{:tip}

* **Capabilities**. If you're unfamiliar with capabilities, see [Channel capabilities](https://hyperledger-fabric.readthedocs.io/en/release-1.4/capabilities_concept.html){: external} in the Fabric documentation. Currently, only **application** and **orderer** capabilities can be set during channel creation. For information about **channel** capabilities (which are a level of capabilities that span both ordering nodes and peers and do not refer to "capabilities on the channel") and how to change them, check out the [Capabilities](#ibp-console-govern-capabilities) section below.

* **Block cutting parameters**. These fields determine the conditions under which the ordering service cuts a new block. For information on how these fields affect when blocks are cut, see the [Block cutting parameters](/docs/blockchain?topic=blockchain-ibp-console-govern#ibp-console-govern-orderer-tuning-batch-size) section below. Note that changing these parameters will require the signature of an ordering service admin. If you are an ordering service admin, you can sign this update using the **Ordering service organization** panel.

* **Ordering service administrator** (only available when updating a channel). This section shows the ordering service administrator organizations for this channel. By default, all ordering organizations that are administrators are added to an application channel at channel creation time. If an ordering service organization was added as an administrator of the system channel after this application channel was created, it must be added here before any nodes belonging to it can be added as application channel consenters. Note: if your console is at a build before `2.1.3-104`, you will not see this option. To see the version of your build, click on the support icon in the upper right  corner (it resembles a question mark). The version will be listed below **IBM Blockchain Platform version** on the upper left.

* **Consenter set**. The ordering service nodes on a particular channel are known in a Raft consensus mechanism as a "consenter set". For this reason, the ordering nodes in a consenter set are sometimes referred to as "consenters". The orderer system channel will always contain all of the possible consenters available in a network (its consenter set is "all consenters"), while application channels might have all consenters or some subset of consenters. It is possible to add or remove particular nodes from this consenter set during both the creation of a channel and through a channel update (for example to add newly created nodes, which are first added to the system channel, to the consenter set of an application channel). To add a node to the consenter set, first ensure that the organization that owns the consenter is an admin of the ordering service of the application channel through the **Ordering service administrator** panel. You can then add the consenter through this **Consenter set** panel by opening the drop-down list, clicking on a node, and clicking **Add**. For information about creating a new ordering node and adding it to a consenter set, see the [Adding and removing ordering service nodes](/docs/blockchain?topic=blockchain-ibp-console-add-remove-orderer#ibp-console-add-remove-orderer) tutorial. If you modify the consenter set, you also have the ability to change the size of the ordering service Snapshot. For more information about Snapshots, see [Snapshots](https://hyperledger-fabric.readthedocs.io/en/release-1.4/orderer/ordering_service.html#snapshots){: external} in the Fabric documentation.

* **Ordering service signature**. If changes to the configuration are made that require the signature of an ordering service org admin (for example, to the block cutting parameters, orderer capability, or consenter set), this panel will allow you to send the change to the relevant ordering service admin organization. The console where the ordering service organization MSP was created will see a signature notification. After the ordering service organization signs the channel update request, it is sent back to the organization that requested the channel update to be submitted.

* **Access control lists (ACLs)**. To specify a finer grained control over resources, you can restrict access to a resource to an organization and a role within that organization. For example, setting access to the resource `ChaincodeExists` to `Application/Admins` would mean that only the admin of an application would be able to access the `ChaincodeExists` resource. Users have the option to edit the access control list one at a time or by uploading a JSON containing the list. For more information about Access Control, see [Access Control Lists (ACLs)](https://hyperledger-fabric.readthedocs.io/en/release-1.4/access_control.html){: external} in the Fabric documentation.

  If you restrict access to a resource to a particular organization, be aware that only that organization will be able to access the resource. If you want other organizations to be able to access the resource, you will have to add them one by one. As a result, consider your access control decisions carefully. Restricting access to certain resources in certain ways can have a highly negative effect on how your channel functions.
  {:important}

## Updating a channel configuration
{: #ibp-console-govern-update-channel}

While creating a channel and updating a channel have the same goal, giving users the ability to ensure that the configuration of their channel is as well suited as possible to their use case, the two processes are in fact very different **as tasks** in the console. Creating a channel is a process undertaken by a **single organization**. As long as an organization is a member of the consortium of the ordering service, they can create the channel in any way they want. They can give it any name, add any organizations (as long as they are a member of the consortium), assign those organizations permissions, set access control lists, and so on.

The other organizations have the choice of whether to participate in this channel (for example, whether to join peers to it), but it is assumed that the collaborative process of deciding on the channel configuration will happen out of band, before an organization creates the channel.

Updating a channel is different. Rather than an organization acting alone, channel updates follow the collaborative governance procedures that are fundamental to the way the {{site.data.keyword.blockchainfull_notm}} Platform functions. This collaborative process involves sending the channel configuration update requests to operator organizations that have an administrative role in the channel.

Because selecting [Anchor peers](#ibp-console-govern-channels-anchor-peers) and [Joining a peer to a channel](#ibp-console-govern-channels-join-peer) are actions that only require the signature of the organization performing the action, they are updated through the **Channel details** panel inside a channel. To update other parameters, you must click on the **Settings** button next to the channel name at the top of the page and initiate a channel configuration update transaction. A panel will appear that looks very similar to the panel you use to create a channel.

### Signature collection flow
{: #ibp-console-govern-update-channel-signature-collection}

For signatures to be verified, all of the organizations on a channel (including the ordering service organizations) should export the MSPs representing their organizations to the other organizations on the channel. To export an MSP, click the download button on your MSP (on the **Organizations** tab), then send it to other organizations out of band. When you receive an MSP JSON file, import it using the **Organizations** panel. For more information about exporting and importing, including information about to export and import in bulk, see [Importing nodes](/docs/blockchain?topic=blockchain-ibp-console-import-nodes).
{: important}

Whenever a channel request is made that requires signatures (whether it is during channel creation, as when an ordering service admin signature is required, or during a channel update, when channel operators will likely need to sign), it will be sent to the organizations in the channel whose signatures are required. For example, if there are five operators (channel admins) in a channel, an update request will be sent to all five. For the channel request to be approved, the number of channel operators listed in the **channel update policy** must be satisfied. If this policy says `3 out of 5`, the new channel configuration update will take effect after three organizations have signed it and it has been submitted.

This process of knowing when you have an update to sign, as well as signing it, is handled through the **Notifications** button (which looks like a bell) in the top right of the console. When you see a blue dot on the **Notifications** button, it means that you either have a pending request to evaluate or are being notified of a channel-related event.

When you click on the **Notifications** button, you may have one or more actions you have the ability to take:

* **Needs attention**: a request needs to be signed or submitted (if all required signature have already been collected). The request might be to either your peers or ordering service, depending on the organizations in your console.
* **Open**: includes everything that **needs attention** as well as requests that have been signed by the user but still need to be signed by one or more other channel members.
* **Closed**: requests that have been submitted. No actions to be taken on these items. They can only be viewed.
* **All**: includes both open and closed requests.

If a channel configuration update request has been made, you have the ability to click on **Review and update channel configuration** and see the changes to the channel configuration update that are being proposed or have been made (if the new channel configuration has been approved). If you are an operator on the channel, and not enough signatures have been gathered to approve the channel configuration update request, you will have the ability to sign the update request.

You are not required to sign a channel configuration update, however note that there is no way to sign **against** a channel update. If you do not approve of a channel configuration update, you can simply close the panel and reach out to other channel operators out of band to voice your concerns. However, if enough operators in the channel approve of the update to satisfy the channel update policy, the new configuration will take effect.
{:note}

### Channel configuration parameters you can update
{: #ibp-console-govern-update-channel-available-parameters}

It's possible to change many, but not all, of the configuration parameters of a channel after the channel has been created. The **Channel name**, for example, cannot be edited, nor can the ordering service where the channel is hosted.

First thing you will see is the **Organization updating channel** panel. Here you will specify the organization (MSP) and admin identity that will be submitting this channel configuration update. From here, you can navigate down to the parameter you want to change, which includes all of the same parameters you specified when creating the channel other than the **channel** capabilities and the **ordering service administrator**, which are only available when updating a channel.

If your console is at a build before `2.1.3-104`, you will not see the **ordering service administrator** option. To see the version of your build, click on the support icon in the upper right corner (it resembles a question mark). The version will be listed below **IBM Blockchain Platform version** on the upper left.
{: tip}

If you are attempting to change any parameter that requires the signature of ordering service admins (for example, the **Block cutting parameters** or **ordering service consenters**) and are one of the ordering service admins on this channel, you will see a field for the ordering service organization. Select the MSP of the relevant ordering service organization from the drop-down list. If you are not an admin of the ordering service organization, you can still make a request to change these fields, but the request will need to be signed by an ordering service admin.

When you are done making your updates, the **Review channel information** panel will allow you to review your changes and then submit them.

#### Anchor peers
{: #ibp-console-govern-channels-anchor-peers}

Because cross organizational [gossip](https://hyperledger-fabric.readthedocs.io/en/release-1.4/gossip.html){: external} must be enabled for service discovery and private data to work, each organization should specify at least one [anchor peer](https://hyperledger-fabric.readthedocs.io/en/release-1.4/gossip.html#anchor-peers){: external} on each channel. This anchor peer is not a special **type** of peer. It is just the peer that bootstraps cross organizational gossip.

Note that while anchor peers are a channel configuration parameter, they are not set through the normal process of creating and editing a channel configuration.

To configure a peer to be an anchor peer, click the **Channels** tab and open the channel where the smart contract was instantiated.
 - Click the **Channel details** tab.
 - Scroll down to the Anchor peers table and click **Add anchor peer**.
 - Select at least one peer from each organization in collection definition that you want to serve as the anchor peer for the organization. For redundancy reasons, you can consider selecting more than one peer from each organization in the collection.

#### Join a peer to a channel
{: #ibp-console-govern-channels-join-peer}

The ordering service and the peers known to this console that are joined to this channel are listed in the **Nodes** field at the top of the channel page. It is possible to add peers to this channel by clicking the **Join peer** button. On the panel that opens, you can specify the peers you want to join this channel, as well as whether you want this peer to bootstrap gossip communication between your organization and other organizations by making this peer an anchor peer. If you already have specified an anchor peer for this channel, you do not need to make this peer an anchor peer (only one anchor peer is needed to bootstrap communication between organizations), though it is sensible to make anchor peers HA by making sure you have at least two anchor peers in each channel.

As with the process for joining any peer to any channel, make sure that the [database type](/docs/blockchain?topic=blockchain-ibp-console-adv-deployment#ibp-console-adv-deployment-level-couch) of this peer is compatible with the database type of the channel. Similarly, ensure that the Fabric level of this peer is at the [application and channel capability](#ibp-console-govern-update-channel-available-parameters-advanced) levels of the channel.

Note that after a peer is removed from a channel, it might still show as being joined to the channel. This is because the ledger information of the channel up to the point the peer was removed is still available on the peer. Removing the peer from the channel means that the peer will receive no new channel updates, including the update that indicates that it is no longer a part of the channel. If you have any other peers joined to the channel, you can check the ledger height of those peers as compared to the peer that was removed to confirm that the removed peer is not receiving new blocks.

#### Capabilities
{: #ibp-console-govern-capabilities}

As noted in the [Channel capabilities](https://hyperledger-fabric.readthedocs.io/en/release-1.4/capabilities_concept.html){: external} topic in the Fabric documentation, there are three levels of capabilities: for the **orderer**, **application**, and **channel**.

While all capabilities can be edited as part of a channel configuration update request, you have the opportunity to edit these capabilities in several places:

  * **Channel and orderer**: in the [system channel](/docs/blockchain?topic=blockchain-ibp-console-govern#ibp-console-govern-capabilities-system-channel) by ordering service admins.
  * **Application and orderer**: during [channel creation](/docs/blockchain?topic=blockchain-ibp-console-govern#ibp-console-govern-capabilities-application-channels).

Note that what you see in this section will depend on the configuration of your channel and the Fabric level of your peers and ordering nodes, as **only valid and possible capability upgrades will appear**. For example, if your channel, orderer, and application capabilities are already at the most recent level, the capabilities section in the channel update will be hidden. Similarly, you will not see potential capability updates if your nodes in the channel are not at a sufficient Fabric level to process the capability.

To ensure that you will always be able to see and propose updates to the latest capabilities, **it is a best practice to upgrade your peers and ordering nodes to the latest Fabric version as soon as it is available in the console**. From a channel management perspective, it is also a best practice to discuss capability updates with other organizations before proposing a change. This will allow organizations to update nodes, as necessary, before the channel update request is submitted.
{:important}

#### Capabilities in the system channel
{: #ibp-console-govern-capabilities-system-channel}

Because the ordering service is involved in the validation of the orderer and channel capabilities, these capability levels exist in the system channel maintained by the ordering service. By default, any channel that is created on this ordering service inherits these capability levels. Because only the orderer capability (and not the channel capability) is apparent when creating a channel, it is important to communicate the **channel** capability level to consortium members so they can ensure that the level of their peers is at the Fabric version of the capability level or higher.

In order to edit the orderer or channel capabilities in the system channel, you must be an ordering service admin (your MSP can be added as an ordering service admin through the tile representing the ordering service on the **Nodes** tab). Note that capability versions can only advance. **You cannot go back to a previous capability or downgrade from a default capability level to a lower version**.

To edit these capabilities, click on the **Settings** button inside the ordering service. Then click **Capabilities**. Note that the channel and orderer capabilities, as well as the application capabilities, can also be edited through a channel configuration update. However, an ordering service admin will have to sign any configuration update that edits the orderer or channel capabilities.

#### Capabilities in application channels
{: #ibp-console-govern-capabilities-application-channels}

Application capabilities define the way transactions are handled exclusively by the peers. As a result, these capabilities are not inherited from the system channel (which is managed by the ordering service) and you will see the full list of capabilities, starting with `1.1`, when creating a channel. Note that the Fabric version of all peers in the channel must be at least at the level of the application capability level and the channel capability level inherited from the ordering service. When creating a channel, you might see that the default application capability is lower than the highest available level. This is done in cases where a new Fabric version with a new application capability has been released but it is not expected that most peers will be at the new Fabric version.

Like the orderer and channel capabilities, the application capability level can be edited through a channel configuration update. The orderer capability can also be specified during the creation of a channel, but will require the approval of the ordering service.

If you are using the SDK to create or edit a channel, take caution to not submit a configuration update with an invalid application capability. Because application capabilities are not validated by the ordering service, an invalid application capability will not be flagged and the configuration update will be approved. Because the peers cannot process an application capability that does not exist, the peers will crash when attempting to read the configuration block containing the invalid capability. Because the peers will be unable to progress beyond this configuration block, it will not be possible to reverse this configuration block and submit another one to "fix" the problem. A channel in this state would be unrepairable.
{:important}

## Tuning your ordering service
{: #ibp-console-govern-orderer-tuning}

Performance of a blockchain platform can be affected by many variables such as transaction size, block size, network size, as well as limits of the hardware. The orderer node includes a set of tuning parameters that together can be used to control orderer throughput and performance.  You can use these parameters to customize how your orderer processes transactions depending on whether you have many small frequent transactions, or fewer but large transactions that arrive less frequently. Essentially, you have the control to decide when the blocks are cut based on your transaction size, quantity, and arrival rate.

The following parameters are available in the console by clicking the orderer node in the **Nodes** tab and then clicking its **Settings** icon. Click the **Advanced** button to open the **Advanced channel configuration** for the orderer.

### Block cutting parameters
{: #ibp-console-govern-orderer-tuning-batch-size}

The following three parameters work together to control when a block is cut, based on a combination of setting the maximum number of transactions in a block as well as the block size itself.

- **Absolute max bytes**
  Set this value to the largest block size in bytes that can be cut by the orderer.  No transaction may be larger than the value of `Absolute max bytes`. Usually, this setting can safely be two to ten times larger than your `Preferred max bytes`.
  **Note**: The maximum size permitted is 99MB.
- **Max message count**
  Set this value to the maximum number of transactions that can be included in a single block.
- **Preferred max bytes**
  Set this value to the ideal block size in bytes, but it must be less than `Absolute max bytes`. A minimum transaction size, one that contains no endorsements, is around 1KB.  If you add 1KB per required endorsement, a typical transaction size is approximately 3-4KB. Therefore, it is recommended to set the value of `Preferred max bytes` to be around `Max message count * expected averaged tx size`. At run time, whenever possible, blocks will not exceed this size. If a transaction arrives that causes the block to exceed this size, the block is cut and a new block is created for that transaction. But if a transaction arrives that exceeds this value without exceeding the `Absolute max bytes`, the transaction will be included. If a block arrives that is larger than `Preferred max bytes`, then it will only contain a single transaction, and that transaction size can be no larger than `Absolute max bytes`.

Together, these parameters can be configured to optimize throughput of your orderer.

### Batch timeout
{: #ibp-console-govern-orderer-tuning-batch-timeout}

Set the **Timeout** value to the amount of time, in seconds, to wait after the first transaction arrives before cutting the block. If you set this value too low, you risk preventing the batches from filling to your preferred size. Setting this value too high can cause the orderer to wait for blocks and overall performance to degrade. In general, we recommend that you set the value of `Batch timeout` to be at least `max message count / maximum transactions per second`.

When you modify these parameters, you do not affect the behavior of existing channels on the orderer; rather, any changes you make to the orderer configuration apply only to new channels you create on this orderer.
{:important}

