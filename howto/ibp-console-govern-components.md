---

copyright:
  years: 2019, 2020
lastupdated: "2020-06-16"

keywords: network components, IBM Cloud Kubernetes Service, allocate resources, batch timeout, reallocate resources, LevelDB, CouchDB

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

# Managing deployed components
{: #ibp-console-govern-components}



After creating CAs, peers, and ordering nodes, you need to monitor the resources used by the nodes and potentially reallocate resources.
{:shortdesc}

**Target audience:** This topic is designed for network operators who are responsible for creating, monitoring, and managing their components in the blockchain network.

## Considerations when reallocating resources
{: #ibp-console-govern-components-reallocate-resources}

Resizing a node requires the containers to be rebuilt, which can cause a delay in the functioning of the node.
{:important}

We recommend using the [{{site.data.keyword.cloud_notm}} Sysdig](https://www.ibm.com/cloud/sysdig){: external} tool in combination with your {{site.data.keyword.cloud_notm}} Kubernetes dashboard to monitor your Kubernetes resource usage. When you determine that a worker node is running out of resources, you can add a new larger worker node to your cluster and then delete the existing working node.
{:note}




While it takes less effort to deploy enough resources to your Kubernetes cluster from the start and therefore be able deploy and expand resources without having to increase the resources in your cluster, the bigger the deployment, the more it will cost. Users need to consider their options carefully and recognize the tradeoffs that they are making regardless of the option that they choose.


You can scale your cluster manually by monitoring your nodes and either adding more nodes or larger nodes. While this process can be labor intensive, it has the advantage of allowing the user to always be certain what is being charged to their cloud account.

A Kubernetes cluster on {{site.data.keyword.cloud_notm}} includes the ability to use an **autoscaler** that can scale your worker nodes up or down in response to your pod spec settings and resource requests. For more information about the autoscaler and how to set it up, see [Scaling Kubernetes clusters](/docs/containers?topic=containers-ca#ca){: external} or [Scaling OpenShift clusters](/docs/openshift?topic=openshift-ca){: external} in the {{site.data.keyword.cloud_notm}} documentation. Note that allowing the autoscaler to adjust your resources will result in charges to your {{site.data.keyword.cloud_notm}} account that will vary automatically with your usage.

To scale manually in the console, click the node that you want to adjust on the **Nodes** page and then click the **Usage** tab. You can see a button called **Reallocate**, which will launch a **Resource allocation** tab that is very similar to the one that you saw when you created the node. If you want to lower the amount of available resources, simply provide lower values and click **Reallocate resources** on that tab and the resulting **Summary** page.

If you want to increase the CPU and memory for a node, use the **Resource allocation** tab in the console to increase the values. The white box at the bottom of the page will add up the new values. After clicking **Reallocate resources**, the **Summary** page will translate this value into a **VPC** amount, which is used to calculate your bill. You'll then need to navigate to your Kubernetes cluster to make sure your cluster has sufficient resources for this reallocation. If it does, you can click **Reallocate resources**. If sufficient resources are not available, you will need to increase the size of your cluster.

The method you will use to increase storage will depend on the storage class you chose for your cluster.  Refer to the [Kubernetes](/docs/containers?topic=containers-kube_concepts#kube_concepts){: external} or [OpenShift](/docs/openshift?topic=openshift-kube_concepts) storage options in the {{site.data.keyword.cloud_notm}} documentation. If you are about to exhaust the storage on your peer or ordering node, you might need to deploy a new peer or ordering node with a larger file system and let it sync via your other components on the same channels.

In {{site.data.keyword.cloud_notm}}, CPU and memory can be increased using the console if you have resources available in your Kubernetes cluster on {{site.data.keyword.cloud_notm}}. However, storage must be increased using the {{site.data.keyword.cloud_notm}} CLI. For a tutorial on how to do this, see Changing the size and IOPS of your existing storage device on your [Kubernetes ](/docs/containers?topic=containers-file_storage#file_change_storage_configuration){: external} or [OpenShift](/docs/openshift?topic=openshift-block_storage#block_change_storage_configuration){: external} cluster.




Note that you do not need to adjust the CPU, memory, or storage for your smart contract pods. These pods will automatically use as many resources as they need to function efficiently. In cases where your smart contracts are struggling because of insufficient resources, you will have to address this at the cluster level.
{: tip}


### Monitoring file storage
{: #ibp-console-govern-components-monitor-storage}

To view your consumption of file storage, navigate to your Kubernetes cluster on {{site.data.keyword.cloud_notm}}. Click on the menu button in the upper left hand corner. Then click on **Classic infrastructure**, **Storage**, and then **File Storage**. This will display the capacity and usage for each persistent volume claim (PVC). This usage can be mapped to your {{site.data.keyword.blockchainfull_notm}} Platform nodes by clicking on the cell in the **Notes** column.

You will see something that looks like this:

```
PVC {"plugin":"ibm-file-plugin-77497497cc-5qm58","region":"us-south","cluster":"05ccfca248dc4c389978d074524e0a8e","type":"Endurance","ns":"n4c817f","pvc":"n4c817forg1ca-fabric-ca-pvc","pv":"pvc-63074ac8-8b9b-11e9-88db-222bd59fb4bc","storageclass":"default","reclaim":"Delete"}
```

To see the same information from the command line, log in to your cluster and enter this command:

```
ibmcloud sl file volume-list --column id --column notes
```

This will allow you to map the output from the pods to the nodes you have deployed.

### Adding storage
{: #ibp-console-govern-components-add-storage}

As you monitor your pods and notice that more storage is needed, you can increase storage when additional storage is available. The following links provide more information about how to increase storage after your network is deployed.

- [IBM file storage](/docs/FileStorage?topic=FileStorage-expandCapacity)
- [Portworx](https://docs.portworx.com/portworx-install-with-kubernetes/storage-operations/create-pvcs/resize-pvc/)
- [Block storage](/docs/BlockStorage?topic=BlockStorage-expandingcapacity#expandingcapacity)

Users can allocate more storage to their running network by resizing the existing storage PVCs or by deploying nodes with new PVCs.


## Deleting components
{: #ibp-console-govern-components-delete}

The best practice for deleting components is to delete them using the console. This will also delete all of the artifacts associated with a node including your ledger data in persistent storage and the keys that are stored as secrets. Deleting a peer will not, however, delete any smart contract pods associated with it. These must be deleted separately. Deleting a component is usually achieved by logging onto the console where a component was created or installed, clicking on the component and finding the related **trash can** icon. You will typically be prompted to type the name of the component and to confirm your decision. You can also delete nodes by using the {{site.data.keyword.blockchainfull_notm}} Platform APIs.

However, there are cases in which this type of deletion will not be successful. For example, occasionally when a node fails to deploy it will not be possible to delete it using the console. The same can be true if the console loses connection with the cluster for some reason.

In these cases, it will be necessary to use the Kubernetes dashboard or delete the node or relevant pods manually. If you attempt to delete pods that contain deployments such as peers, CAs, or ordering nodes, the pod will automatically restart and not be permanently deleted. 

Because smart contracts installed on a 2.x peer are deployed into their own pods and not directly into the peer container, they will not be deleted when a peer is deleted. They will have to be deleted either using the UI of your cluster or by issuing kubectl commands. Smart contracts installed on a v1.4.x peer will be deleted when the peer is deleted.
{: important}




Because deployments are organized in Kubernetes by their "namespace", you need to know your Kubernetes cluster namespace before you can delete components using the kubectl CLI. From the console, open any CA node and click the **Info and Usage** icon. View the value of the API URL. For example: https://nf85a2a-soorg10524.ibpv2-cluster.us-south.containers.appdomain.cloud:7054. The namespace is the first part of the url beginning with the letter `n` and followed by a random string of six alphanumeric characters. So in the example above the value of the namespace is `nf85a2a`.


If you want to delete all of your smart contract pods, you can issue this command:


```
kubectl get po -n <NAMESPACE> | grep chaincode-execution | cut -d" " -f1 | xargs -I {} kubectl delete po {} -n <NAMESPACE>
```
{:codeblock}




If you want to delete a single smart contract pod, you will first have to figure out the name of your smart contract pod.


First, get a list of all of the smart contract pods running in your cluster:

```
kubectl get po -n <NAMESPACE> | grep chaincode-execution | cut -d" " -f1 | xargs -I {} kubectl get po {} -n <NAMESPACE> --show-labels
```
{:codeblock}

Replacing `<NAMESPACE>` with the name of your cluster namespace.

You should see results similar to:

```
NAME                                                       READY   STATUS    RESTARTS   AGE   LABELS
chaincode-execution-0a8fb504-78e2-4d50-a614-e95fb7e7c8f4   1/1     Running   0          14s   chaincode-id=javacc-1.1,peer-id=org1peer1
NAME                                                       READY   STATUS    RESTARTS   AGE   LABELS
chaincode-execution-f3cc736f-94ef-454d-8da3-362a50c653d9   1/1     Running   0          4m    chaincode-id=nodecc-1.1,peer-id=org1peer1
```

Your smart contract name and version is visible next to the chaincode-id.





To delete a single pod, issue this command, substituting the `<POD_NAME>` for the name of your pod, for example the smart contract pod `chaincode-execution-0a8fb504-78e2-4d50-a614-e95fb7e7c8f4`, as well as your `<NAMESPACE>`:

```
kubectl delete pod <POD_NAME> -n <NAMESPACE>
```
{:codeblock}





You can also use kubectl commands to delete all of the nodes in your cluster by issuing commands to delete each type of node. First, set the namespace where the nodes you want to delete are located:

```
kubectl config set-context --current --namespace=<NAMESPACE>
```
{:codeblock}




Then run the following commands to delete all of your blockchain nodes:

```
kubectl delete ibpca --all
kubectl delete ibppeer --all
kubectl delete ibporderer --all
```
{:codeblock}

You may also choose to only delete all of a single type of node within a namespace, for example, by only issuing `kubectl delete ibppeer --all`.

Note that if you delete your entire namespace, your smart contract pods will also be deleted. Your smart contract pods will also be deleted, along with your nodes, if you delete your service instance using the Kubernetes dashboard in {{site.data.keyword.cloud_notm}}.
{: tip}


