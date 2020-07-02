---

copyright:
  years: 2018, 2020
lastupdated: "2019-05-31"

keywords: IBM Cloud accounts, IBM Blockchain Platform, pay your own fees, pay for others

subcollection: blockchain

---

{:external: target="_blank" .external}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Paying mode
{: #paying-mode}

{{site.data.keyword.blockchainfull}} Platform charges monthly with membership fees and peer fees. Network members can pay their bill with their {{site.data.keyword.cloud_notm}} accounts that contain the space to create the network instance. Alternatively, one network member can cover the bill for other members in the network, or even all members to pay for the entire network. The payer can also transfer the billings back to other members if necessary.
{:shortdesc}

Depending on the network plan that you choose and the amount of resources that you use, your payments are different. For more information about pricing, see [Pricing](/docs/blockchain?topic=blockchain-ibp-pricing#ibp-pricing).

Note that when several organizations build a consortium, it is recommended that the participating organizations use a functional email address to create their network. In this case, access to the network does not depend on any individual's availability.


## Prerequisites
{: #paying-mode-prereq}

Every network member must have a paid {{site.data.keyword.cloud_notm}} account, for example, a **Pay-As-You-Go** account, so that they can create {{site.data.keyword.blockchainfull_notm}} Platform service instances on {{site.data.keyword.cloud_notm}}. If you don't have an account, [register](https://cloud.ibm.com/registration) one and upgrade it to a paid account. To upgrade your account to a Pay-As-You-Go type, go to **Manage** > **Billing and Usage** > **Billing** in the {{site.data.keyword.cloud_notm}} console, and click **Add Credit Card**.


## Paying your own fees
{: #paying-mode-pay-your-own}

After you create or join any blockchain networks on {{site.data.keyword.blockchainfull_notm}} Platform, your {{site.data.keyword.cloud_notm}} accounts will be charged for your membership fee and peer fees.


## Paying for other network members
{: #paying-mode-pay-for-others}

One member in the network can cover the fees for one or more other network members in the same blockchain network. To pay in this mode, the payer and other network members must complete the following steps.

1. The payer creates separate Cloud Foundry spaces in {{site.data.keyword.cloud_notm}} for the members who the payer will pay for.
   1. Log in to {{site.data.keyword.cloud_notm}}.
   2. From the menu bar, click **Manage** > **Account** > **Cloud Foundry Orgs**.
   3. Click **View Details** of the Cloud Foundry Org that you will create the network. If you don't have an Org for the network, click the **Add a new Cloud Foundry Org** button to create one.
   4. Click the **Add a Cloud Foundry Space** button to create a space for another network member. Select the region for the space, give it a name, and click **Add**.  Note that only the space creator can access the created space.  
   **Note**: If the payer pays for more than one member, the payer needs to repeat this step to create separate spaces for each of the members who the payer will pay for.
2. The payer invites other members to the payer's {{site.data.keyword.cloud_notm}} account and give them access to their specific spaces. Ensure that each member can have access to only one space.
   1. From the menu bar, click **Manage** > **Account** > **Users**.  
   2. Click **Invite Users** and assign user access:
      1. Specify the email address of a single user that you will invite to your Cloud Foundry Org.
      2. In the **Cloud Foundry access** section, make the following selections:
         - **Organization**: The Cloud Foundry Org to create the network.
         - **Organization roles**: Auditor.
         - **Region**: The region where you created the space for this user.
         - **Space**: The space that you created for this user.
         - **Space roles**: Developer.
      3. Click **Invite Users**.  
   **Note**: If the payer pays for more than one member, the payer needs to repeat this step to invite other members and assign access for each of them.
3. The payer creates a blockchain network {{site.data.keyword.blockchainfull_notm}} Platform and invites other members to the blockchain network in the Network Monitor of the network. For more information about inviting network members, see [Members](/docs/blockchain?topic=blockchain-ibp-dashboard#ibp-dashboard-members).
4. Each member then receives a notification email that contains an invitation to join the network. The member can complete the following steps to join the network.
   1. Click the "Proceed" button in the notification email, which takes you to the blockchain service page in {{site.data.keyword.cloud_notm}}.
   2. Log in to {{site.data.keyword.cloud_notm}} and ensure that you use correct {{site.data.keyword.cloud_notm}} organization and space, which the payer adds you to.
      1. In the {{site.data.keyword.cloud_notm}} console, click the profile avatar in the upper right corner.
      2. Choose the payer's account from the **Account** drop-down list.  Note that the account and organization where you provision the blockchain service instance will be the one that gets charged.  
   3. In the {{site.data.keyword.cloud_notm}} catalog, search for **Blockchain** and create a service instance.
      1. In the **Service Name** field, type in a descriptive name for your future recognition.
      2. Verify that the organization and space are the ones that the payer adds you to.
      3. Select the membership plan with which to create your network instance. Your membership plan must be the same as the one that the payer create the network with.
      4. Click **Create**.
   4. After you create the blockchain service instance, follow the wizard to join the network.  For more information, see [Joining a network](/docs/blockchain?topic=blockchain-getting-started-with-enterprise-plan#getting-started-with-enterprise-plan-join-nw).

### Known limitations
{: #paying-mode-limitation}
- Because all members are inside of the payers {{site.data.keyword.cloud_notm}} account, the payer has access to all members' blockchain instances and can impersonate them. Therefore, this paying mode is best used for PoC environments, or in cases where the payer is handling all management of the {{site.data.keyword.blockchainfull_notm}} Platform and the members are being provided with only an application.  
- Any member in the payer's Cloud Foundry Org can see all the spaces in the organization.  However, they cannot edit or modify the organizations because they don't have accesses.  
- Be aware that any member in the payer's Cloud Foundry Org can see the {{site.data.keyword.cloud_notm}} IDs of other members in the same Cloud Foundry Org.  
- After adding all members to the payer's {{site.data.keyword.cloud_notm}} account and assigning them access to provision blockchain instances and join the network, the payer is also giving the members access to create other services, which could cause additional charges. All members might sign an agreement with trust that they will not create any unauthorized services in their accounts to cause extra cost to the payer's account.  

## Letting a member pay for you
{: #paying-mode-let-other-pay}

An alternative of adding other members' accounts to the payer's account, other members can also add the payer's account to their accounts as a **Billing Manager**. To pay in this mode, the payer and other network members must complete the following steps.

1. The payer creates a blockchain network {{site.data.keyword.blockchainfull_notm}} Platform with the payer's account.
2. Other members add the payer's account to their {{site.data.keyword.cloud_notm}} account as a **Billing Manager**.
  1. From the menu bar on the top of the {{site.data.keyword.cloud_notm}} console, click **Manage** > **Account** > **Users**.
  2. On the **Users** page, click the **Invite users** button.
  3. Add the payer's account under the **Email address** field.
  4. Under the **Cloud Foundry access** section, choose your organization that you want to add the payer' account to from the drop-down list.
  5. Choose **Billing manager** as the **Organization roles**.
  6. Configure other fields accordingly and then click **Invite users**.  
3. The payer invites other members to the blockchain network in the Network Monitor of the network. For more information about inviting network members, see [Members](/docs/blockchain?topic=blockchain-ibp-dashboard#ibp-dashboard-members).
4. Each member then receives a notification email that contains an invitation to join the network. The member can then join the network. For more information, see **step 4** under [Paying for other network members](/docs/blockchain?topic=blockchain-paying-mode#paying-mode-pay-for-others).

### Known limitation
{: #paying-mode-let-other-pay-limitation}

After adding the payer's {{site.data.keyword.cloud_notm}} account as billing manager to other members account and other members purchase any other services by using their account, the payer needs to cover those fees too. All members might sign an agreement with trust that they will not create any unauthorized services in their accounts to cause extra cost to the payer's account.  


## Transferring billings to other members
{: #paying-mode-transfer-billing}

While network members can have a member to pay for them, later on, other members can take over the billings. You can remove the payer's account from your accounts so that the payer can stop to pay for you.

1. From the menu bar on the top of the {{site.data.keyword.cloud_notm}} console, click **Manage** > **Account** > **Cloud Foundry Orgs**.
2. Click the organization that you add the payer's account to.
3. Switch to the **Users** tab.
4. Find the payer's account in the users table and click the **Remove from Org** link.

After you remove the payer's account from your account, your account will be charged for your blockchain network membership fee and peer fee.
