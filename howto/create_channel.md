---

copyright:
  years: 2017
lastupdated: "2017-08-14"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Creating or updating a channel

Channels are an incredibly powerful mechanism for partitioning and isolating data, and they provide the primary foundation for data privacy.  Only members of the same channel can access the data of this channel.
{:shortdesc}

To ensure channel security, the channel update policy is configured to define the number of channel operators who need to agree on the channel creation or update request before a channel is created or updated. 

## Creating a channel
Click the **New Channel** button in the "Channels" screen of your Network Monitor, and complete the following steps to submit a channel creation request:  
1. Choose a name reflective of the channel's business objective, add a description if you want, and click **Next**.  The channel name must be unique in a Blockchain network.  It must start with a letter and can contain only lowercase characters, numbers, or dashes.  
  ![Create channel 1](../images/create_channel.png "Create a channel panel 1")  
    
2. Invite any combination of your network members by selecting network members and clicking the **Add Member** button.  Customize permissions by assigning roles for each of the invited members, and click **Next**.  
  ![Create channel 2](../images/create_channel_2.png "Create a channel panel 2")  
  
    * A channel operator can query or update the channel ledger.  Channel operator has the authority to **Accept** or **Decline** a channel create request, and to submit a channel update request. There must be at least one **Operator** in each a channel.  
    * A channel writer can update the channel ledger.
    * A channel reader can only query the channel ledger.
  
3. Configure the channel update policy by selecting the number of channel operators to approve channel update request, and click **Submit Request**.   
  ![Create channel 3](../images/create_channel_3.png "Create a channel panel 3")  

The invited members will receive an invitation email.  They can also find the request with "Vote Pending" status in the **Notifications** screen of the Network Monitor:  
* Members who are currently channel operators can click the **Review Request** button to review the channel configuration, and then **Accept** or **Decline** the channel update request.  If the channel update request gets enough **Accept** votes from channel operators, its status changed to "Vote Accepted".  Any channel operator can then click the **Submit Request** button to complete the channel update.  
* Members who are invited as channel writers or readers can click the **Review Request** button to review the channel configuration.

As a special type of channel update request, when enough channel operators agree on the channel creation request and a channel operator submits the request, the new channel is created.  All channel members can find the channel in the "Channels" screen of their Network Monitor.

## Updating a channel
If you want to modify the configuration of a channel, for example, add or remove channel members, or change the channel update policy, you can submit a channel update request.  In the "Channels" screen of your Network Monitor, locate the channel that you want to modify and select **Edit Channel** from the drop down list under the **Action** header.  Complete the same steps as you submit a channel creation request to submit a channel update request.

All channel members will receive email notification on the channel update request:
* The newly invited members receive email notifications to invite them to join the channel.  They can also find the request with "Vote Pending" status in the **Notifications** screen of their Network Monitor:  
    * Members who are currently channel operators can click the **Review Request** button to review the channel configuration, and then **Accept** or **Decline** the channel update request.  If the channel update request gets enough **Accept** votes from channel operators, its status changed to "Vote Accepted".  Any channel operator can then click the **Submit Request** button to complete the channel update.  
    * Members who are invited as channel writers or readers can click the **Review Request** button to review the channel configuration.
* The removed members receive email notifications about the channel change.
* Existing channel operators also receive email notification on channel update.  They can also find the request with "Vote Pending" status in the **Notifications** screen of their Network Monitor for them to **Accept** or **Decline**.

When enough channel operators agree on the channel update request and any channel operator submit the request, the channel is updated.  All channel members can find the updated channel in the "Channels" screen of their Network Monitor.
