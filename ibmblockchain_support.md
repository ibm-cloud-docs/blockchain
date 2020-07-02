---

copyright:
  years: 2017, 2020
lastupdated: "2020-06-19"

keywords: IBM Blockchain Platform, support case, Hyperledger Fabric Community, Cloud tickets, Rocket Chat, dWAnswers

subcollection: blockchain

---

{:external: target="_blank" .external}
{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:note: .note}
{:important: .important}
{:tip: .tip}
{:pre: .pre}

# Getting support
{: #blockchain-support}

{{site.data.keyword.blockchainfull}} Platform provides several avenues for troubleshooting problems and getting support, which depend on the {{site.data.keyword.blockchainfull_notm}} Platform offerings that you use.
{:shortdesc}

For all {{site.data.keyword.blockchainfull_notm}} Platform offerings, it is recommended to first use [free digital support resources](/docs/blockchain?topic=blockchain-blockchain-support#blockchain-support-resources) to troubleshoot problems and get help from {{site.data.keyword.IBM_notm}} and the Hyperledger Fabric Community. If you use an Enterprise plan network, you can navigate to the "Support" screen in your Network Monitor, which contains links to these resources.

If your problem cannot be solved by any of the [free digital support resources](/docs/blockchain?topic=blockchain-blockchain-support#blockchain-support-resources), consider the following approaches to get support for the offerings that you use.

- **{{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}}**
  Submit a support case through {{site.data.keyword.cloud_notm}}. For more information, see [Submitting support cases](/docs/blockchain?topic=blockchain-blockchain-support#blockchain-support-cases).

- **{{site.data.keyword.blockchainfull_notm}} Platform Visual Studio Code extension**
  Submit any issues in the [{{site.data.keyword.blockchainfull_notm}} Platform Visual Studio Code extension](https://github.com/IBM-Blockchain/blockchain-vscode-extension/issues){: external}.

- **Enterprise Plan on {{site.data.keyword.cloud_notm}}**
  Submit a support case through {{site.data.keyword.cloud_notm}}. For more information, see [Submitting support cases](/docs/blockchain?topic=blockchain-blockchain-support#blockchain-support-cases).

## Resources and support forums
{: #blockchain-support-resources}

**{{site.data.keyword.blockchainfull_notm}} Service docs**
  [{{site.data.keyword.blockchainfull_notm}} Service docs](/docs/blockchain?topic=blockchain-get-started-ibp#get-started-ibp), provides guidance on how to start with {{site.data.keyword.blockchainfull_notm}} Platform. You can find corresponding topics from the table of contents or search any term from the search bar.

**Hyperledger Fabric resources**  
  [Hyperledger Fabric documentation](https://hyperledger-fabric.readthedocs.io/en/release-1.4/){: external}, the [Fabric community wiki](https://wiki.hyperledger.org/display/fabric){: external}, and the [Fabric Jira dashboard](https://jira.hyperledger.org/secure/Dashboard.jspa?selectPageId=10104){: external} provide more details about the Fabric stack.

  You can also talk to a Fabric expert in [Rocket.Chat](https://chat.hyperledger.org/channel/fabric){: external} to get answers to questions about Fabric.

  You can also post questions or check if your question was answered in [Stack Overflow](https://stackoverflow.com/questions/tagged/hyperledger-fabric){: external}.

**Developer resources**  
  [IBM Developer](https://developer.ibm.com/technologies/blockchain/) provides tutorials, code patterns, and videos for blockchain developers. Application developers can use the [Blockchain Design patterns](https://developer.ibm.com/technologies/blockchain/articles/getting-started-with-blockchain-design-patterns) to learn about common patterns and best practices for building applications that communicate with blockchain networks.

  Because {{site.data.keyword.IBM_notm}} does not support using Hyperledger Composer for production implementation, you are encouraged to engage with [Hyperledger Composer community](https://chat.hyperledger.org/channel/composer){: external} and [Stack Overflow](https://stackoverflow.com/questions/tagged/hyperledger-composer){: external} when you work on demos and proofs of concept.

## Submitting support cases
{: #blockchain-support-cases}

For issues that are related to {{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}}, you can submit a support case in the [{{site.data.keyword.cloud_notm}} Service Portal](https://cloud.ibm.com/unifiedsupport/supportcenter){: external}.

If you need help with Hyperledger Fabric or your applications, you can make use of the community resources or engage [{{site.data.keyword.blockchainfull_notm}} Services](https://www.ibm.com/blockchain/services){: external}. It is recommended to use Hyperledger Composer solely for demos and proofs of concept. {{site.data.keyword.IBM_notm}} does not support networks that use Hyperledger Composer in production, including the Composer CLI, JavaScript APIs, REST server, and Web Playground.
{:note}

The response to your support case depends on your [{{site.data.keyword.cloud_notm}} support plan](/docs/get-support?topic=get-support-support-plans#support-plans){: external}.

- If you purchased **Premium Support** or **Advanced Support**, you can specify a severity level on your ticket from Sev-1 to Sev-4. Sev-1 is the highest level of severity. Support cases with higher severity levels indicate more urgency and are responded to more quickly by the support team. Ensure that the ticket severity is  assigned based on the published criteria that is defined in[Case severity and response times](/docs/get-support?topic=get-support-escalation#support-case-severity){: external}.  For more information, see [{{site.data.keyword.cloud_notm}} support plan offerings](/docs/get-support?topic=get-support-support-plans#support-plans){: external}.  
- If you do not purchase support, your {{site.data.keyword.cloud_notm}} Pay-As-You-Go or Subscription account comes with a free **Basic Support** plan. In this case, your support case is automatically registered as Sev-4.

Before you open a support ticket, you need to [gather your logs](/docs/blockchain?topic=blockchain-ibp-console-manage-console#ibp-console-manage-logs).
{:  important}

Follow these steps to submit a support case.

1. Log in to [{{site.data.keyword.cloud_notm}} Service Portal](https://cloud.ibm.com/unifiedsupport/supportcenter){: external} with your {{site.data.keyword.IBM_notm}} ID.
2. Under **Need more help?** on the right of the page, click **Create a Case**.
3. Fill the **Create Case** form with your information at least for the following fields.
  - Choose **Technical** as your case type.
  - In the **Category** drop-down list, select **Blockchain**.
  - In the **Subject** field enter a summary of your issue.
  - In the **Description** field, describe your issue.
  - Attach any relevant logs or files to demonstrate your issue.
  - Click **Email me updates** to receive updates on the status of the ticket.
4. Click the **Submit** button.  You will receive an email notification in a few minutes for this case.

You can find your previously submitted cases by clicking **My Cases** in the {{site.data.keyword.cloud_notm}} Service Portal. Click and open a case to check its status or provide additional information.
