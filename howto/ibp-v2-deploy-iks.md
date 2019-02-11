---

copyright:
  years: 2019
lastupdated: "2019-02-08"


---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:note: .note}
{:important: .important}
{:tip: .tip}
{:pre: .pre}

# Getting started with {{site.data.keyword.blockchainfull_notm}} Platform free 2.0 beta
{: #ibp-v2-deploy-iks}

***[Is this page helpful? Tell us.](https://www.surveygizmo.com/s3/4501493/IBM-Blockchain-Documentation)***

{{site.data.keyword.blockchainfull}} Platform 2.0 is a free beta release that includes the {{site.data.keyword.blockchainfull_notm}} Platform console, a GUI that can simplify and accelerate your journey to deploy and manage blockchain components. This tutorial describes how to get started with  {{site.data.keyword.blockchainfull_notm}} Platform 2.0 and use the console to deploy and manage blockchain components in your {{site.data.keyword.IBM_notm}} Kubernetes Service cluster on {{site.data.keyword.cloud_notm}}.
{:shortdesc}

 After you deploy the {{site.data.keyword.blockchainfull}} Platform to your Kubernetes cluster, you can launch the console to create and manage your blockchain components. By using an {{site.data.keyword.IBM_notm}} Kubernetes Service cluster to deploy the {{site.data.keyword.blockchainfull_notm}} Platform 2.0, you experience the following important benefits:

- **Control:** You control and manage your blockchain components and certificates from one central console. Deploy only the components needed for your business and add more as your need grow.
- **Flexible Kubernetes based deployment:** You can take advantage of the compute (CPU, memory, storage) options for your Kubernetes cluster and leverage built-in HA and DR options.

## Considerations
{: #ibp-v2-deploy-iks-considerations}

Before you deploy the console, ensure that you understand the following considerations:

- {{site.data.keyword.blockchainfull_notm}} Platform free 2.0 beta is built by using Hyperledger Fabric v1.4.
- All peers deployed using the free 2.0 beta use CouchDB as their state database.
- You have the option to use a free Kubernetes cluster for evaluation of the beta offering, however capacity and performance are limited, none of your data can be migrated, and the cluster is deleted after 30 days.
- You are responsible for the management of health monitoring, security, and logging  of your Kubernetes cluster. See this [information ![External link icon](../images/external_link.svg "External link icon")](https://console.bluemix.net/docs/containers/cs_responsibilities.html#your-responsibilities-by-using-ibm-cloud-kubernetes-service "Cluster management responsibilities") for details on what {{site.data.keyword.cloud_notm}} manages and what you are responsible for.
- You are also responsible for monitoring the resource usage of your Kubernetes cluster by using the Kubernetes dashboard. If you need to increase storage capacity or performance of your cluster, see this information on how to [modify your existing volume ![External link icon](../images/external_link.svg "External link icon")](https://console.bluemix.net/docs/containers/cs_storage_file.html#change_storage_configuration "Changing the size and IOPS of your existing storage device").
- You are responsible for managing and securing your certificates, your public and private keys. IBM does not store your certificates in the Kubernetes cluster.
- The free 2.0 beta offering is available in only the **Dallas** region of {{site.data.keyword.cloud_notm}}. Therefore, all blockchain components will reside in the Dallas data center. They are not deployed elsewhere.
- While the beta offering is free, you still need to pay for your Kubernetes cluster, if you choose a paid cluster.

## Before you begin
{: #ibp-v2-deploy-iks-prereq}

Before you begin:

- Ensure that you have an [{{site.data.keyword.cloud_notm}} paid account ![External link icon](../images/external_link.svg "External link icon")](https://console.cloud.ibm.com/catalog/services/ibm-blockchain-platform-free-20-beta "Catalog"). If you do not have an account:
1. Click the **Sign up** button.
2. After you create a free trial account, upgrade it to a **Pay-As-You-Go** type by going to **Manage** > **Billing and Usage** > **Billing** in the {{site.data.keyword.cloud_notm}} console, and clicking **Add Credit Card**.

When you plan to use the service instance in the context of a broader organization-wide solution, it is recommended that the participating organizations use a functional email address to create their network. In this case, access to the network does not depend on any single individual's availability.
{:tip}  

- If you plan to use an existing {{site.data.keyword.cloud_notm}} Kubernetes Service cluster, check the Kubernetes version and upgrade it to 1.11 or higher, if necessary. For more information about how to determine what version of Kubernetes your cluster is running and how to upgrade the version, see [Updating the Kubernetes version of your cluster](/docs/services/blockchain/howto/ibp-v2-deploy-iks.html#ibp-v2-deploy-iks-updating-kubernetes).

### Browsers
{: #ibp-v2-deploy-iks-browsers}
The following list specifies the minimum required browser software for the {{site.data.keyword.blockchainfull_notm}} Platform console:

- Chrome: latest version for your operating system
- Firefox: latest regular (non-ESR) versions for your operating system
- Safari: latest version for Mac

### Resources required
{: #ibp-v2-deploy-iks-resources-required}

To deploy the {{site.data.keyword.blockchainfull_notm}} Platform console into an {{site.data.keyword.cloud_notm}} Kubernetes Service cluster, you need to ensure that your Kubernetes cluster meets the minimum hardware resource requirements:

|Kubernetes cluster type | Use case | CPU | RAM | Worker nodes |
|-----------|------|-----|-----------------------|
|Standard (Recommended) | Suitable for MVPs | 4 (Shared) | 16 GB (Shared)|multiple|
|Free | Suitable for evaluation | 2 | 4 GB | 1 |  

These resources are sufficient for testing and experimentation. If you use a free Kubernetes cluster, be aware that the cluster will be deleted after the 30 day trial and all associated assets are removed. Performance is significantly slower in a free cluster.
{:note}

## Step one: Create a service instance in {{site.data.keyword.cloud_notm}}
{: #ibp-v2-deploy-iks-create-service-instance}

Use the following steps to create a service instance of {{site.data.keyword.blockchainfull_notm}} Platform 2.0 on {{site.data.keyword.cloud_notm}}.

1. Locate the [Blockchain service ![External link icon](../images/external_link.svg "External link icon")](https://console.cloud.ibm.com/catalog/services/blockchain-platform-20) in {{site.data.keyword.cloud_notm}} Catalog, or search for `Blockchain` in your {{site.data.keyword.cloud_notm}} Catalog page.
2. You can optionally rename the **Service name** for your instance so you can recognize it easily in the future.
3. For Beta, **Dallas** is the only available region and it cannot be changed.
4. You can leave the resource group and tags fields unchanged.
5. Click **Create** to provision the service instance.

## Step two: Deploy {{site.data.keyword.blockchainfull_notm}} Platform 2.0
{: #ibp-v2-deploy-iks-steps}

You can follow the guidance to deploy {{site.data.keyword.blockchainfull_notm}} Platform 2.0 immediately after you create the service instance.

1. The **Welcome & prerequisites** step. If you already have an existing {{site.data.keyword.IBM_notm}} Kubernetes Service cluster in the **Dallas** region, select the check box and **If you have an IBM Kubernetes Service cluster and want to use it for your Blockchain service, you can skip the next step. But be sure the Kubernetes version is at v1.11 or higher**. Click **Continue**.
2. The **Create cluster** step. If you select the check box in step 1 to use an existing Kubernetes cluster, this step is skipped. Otherwise, click **Create a new cluster**, which launches the {{site.data.keyword.cloud_notm}} Kubernetes dashboard to create a cluster. For more information, see [Getting started with {{site.data.keyword.cloud_notm}} Kubernetes Service ![External link icon](../images/external_link.svg "External link icon")](https://cloud.ibm.com/docs/containers/container_index.html). Allow extra time for this process to complete.
  - Regardless of the cluster type you choose, you must select the Kubernetes cluster location of **Dallas** for the Beta release.
  - Choose **Standard cluster (recommended):**  If you need a longer term option that includes multiple nodes for high availability and can be migrated from the Beta offering to the GA offering when it is available.
  - Choose **Free cluster:** If you plan to use the cluster for less than 30 days. **Note** that it is not possible to migrate from a free cluster to a paid cluster. The free type of cluster offers limited storage and transaction throughput.
  - For more information about the differences between the free and paid Kubernetes clusters on {{site.data.keyword.cloud_notm}}, see [Comparison of free and standard clusters ![External link icon](../images/external_link.svg "External link icon")](https://console.cloud.ibm.com/docs/containers?topic=containers-cluster_types#cluster_types "Comparison of free and standard clusters").  

   You must return to this tab in your browser after you create the cluster so that you can complete the {{site.data.keyword.blockchainfull_notm}} Platform 2.0 deployment process.  
   {:important}  

  You need to wait until your cluster is deployed successfully. Then, click the **I Have a Cluster** button.
3. Your Kubernetes version running in your cluster must be at version 1.11 or higher. Follow these [steps](/docs/services/blockchain/howto/ibp-v2-deploy-iks.html#ibp-v2-deploy-iks-updating-kubernetes) to check your cluster version and upgrade it if necessary. Then come back and proceed with these instructions.
4. The **Deploy onto cluster** step. Select the Kubernetes cluster that you want to deploy {{site.data.keyword.blockchainfull_notm}} Platform 2.0 into from the drop-down list and click **Deploy to cluster**.

  You might be in the following situations in which you cannot find your Kubernetes cluster in the drop-down list:
  - The cluster creation process can take about 10 minutes to complete. If you created a cluster, allow some extra time until the status of your cluster needs to be **Normal**.
  - Clusters that are outside the **Dallas** region are not visible and cannot be used.
  - Check to make sure you are not using the ESR version of Firefox. If you are, switch to another browser such as Chrome and retry.

5. The **Launch console** step. After your {{site.data.keyword.blockchainfull_notm}} Platform 2.0 has been successfully deployed, click **Launch the {{site.data.keyword.blockchainfull_notm}} Platform** to open the {{site.data.keyword.blockchainfull_notm}} Platform console. It may take a few minutes for the button to become enabled while the console is being provisioned.

## (Optional) Add additional users to the console
{: #ibp-v2-deploy-iks-add-users}

By default, the console uses [{{site.data.keyword.IBM_notm}} ID ![External link icon](../images/external_link.svg "External link icon")](https://console.bluemix.net/docs/iam/index.html#iamoverview "IBM Cloud Identity and Access Management") as the identity service provider. Your {{site.data.keyword.blockchainfull_notm}} Platform console is provisioned by configuring the email address of the {{site.data.keyword.IBM_notm}} owner as the Administrator of the console. As an Administrator, this user is authorized to grant other users access to the console via their email addresses.  

### How to add a new user
{: #ibp-v2-deploy-iks-add-users-howto}

Click the **Users** icon in the left navigation to view the list of users who can log in to the console. If you are logged as a console administrator, you can click **Add new users** to authorize new users to the console and grant them **Admin** or **General** access. Users with the **Admin** role can add or delete users.

## Next steps
{: #ibp-v2-deploy-iks-next-steps}

Now that your console is ready to use, you can go ahead to [build a network](/docs/services/blockchain/howto/ibp-console-build-network.html#ibp-console-build-network "Building a network tutorial").

## Updating the Kubernetes version of your cluster
{: #ibp-v2-deploy-iks-updating-kubernetes}

If you use an existing {{site.data.keyword.cloud_notm}} Kubernetes Service cluster, ensure that the Kubernetes version is at version 1.11 or higher.

You can check the Kubernetes version of your cluster in the [Kubernetes clusters page ![External link icon](../images/external_link.svg "External link icon")](https://console.cloud.ibm.com/containers-kubernetes/clusters) on {{site.data.keyword.cloud_notm}}, which lists all your clusters in a table.

If the Kubernetes version is not at version 1.11 or higher, you need to complete the following steps to update the Kubernetes version of your cluster.

1. Click the overflow menu icon at the end of the row and select **Update version**. This process takes about one hour to complete. If the version is updated successfully, you can see the updated version of your cluster in the **Kubernetes version** column.  
2. Select a version higher than 1.11 from the Kubernetes version drop-down list and click **Update**.
3. Click your cluster and go to the **Worker Nodes** tab. Select the check box before the work node that you want to update and click **Update Kubernetes** from the pop-up menu bar. If your cluster contains multiple work nodes, all of them must be updated.

  Updates to worker nodes can cause downtime for your apps and services. Your worker node machine is reimaged, and data is deleted if not [stored outside the pod ![External link icon](../images/external_link.svg "External link icon")](https://cloud.ibm.com/docs/containers/cs_storage_planning.html#persistent_storage_overview).
  {:important}

![Update Kubernetes version](../images/update_k8s_version.gif)

For more information about updating Kubernetes version for an {{site.data.keyword.IBM_notm}} Kubernetes Service cluster and work nodes, see [Updating clusters, worker nodes, and add-ons ![External link icon](../images/external_link.svg "External link icon")](https://cloud.ibm.com/docs/containers/cs_cluster_update.html#update).  

You must wait for the update to complete before you can [resume the {{site.data.keyword.blockchainfull_notm}} Platform 2.0 deployment](/docs/services/blockchain/howto/ibp-v2-deploy-iks.html#ibp-v2-deploy-iks-steps).
