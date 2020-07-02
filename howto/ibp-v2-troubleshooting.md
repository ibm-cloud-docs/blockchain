---

copyright:
  years: 2019, 2020
lastupdated: "2020-06-30"

keywords: troubleshooting, debug, why, what does this mean, how can I, when I

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
{:tsSymptoms: .tsSymptoms}
{:tsCauses: .tsCauses}
{:tsResolve: .tsResolve}
{:support: data-reuse='support'}
{:troubleshoot: data-hd-content-type='troubleshoot'}

# Troubleshooting
{: #ibp-v2-troubleshooting}



General problems can occur when you use the console to manage nodes, channels, or smart contracts. In many cases, you can recover from these problems by following a few easy steps.
{:shortdesc}

This topic describes common issues that can occur when you use the {{site.data.keyword.blockchainfull_notm}} Platform console.  

**Issues with the Console**
- [Why are my console actions failing in my Chrome browser Version 77.0.3865.90 (Official Build) (64-bit)?](#ibp-v2-troubleshooting-chrome-v77)
- [When I hover over my node, the status is `Status unavailable`, what does this mean?](#ibp-v2-troubleshooting-status-unavailable)
- [When I hover over my node, the status is `Status undetectable`, what does this mean?](#ibp-v2-troubleshooting-status-undetectable)
- [Why am I getting the error `Unable to get system channel` when I open my ordering service?](#ibp-troubleshoot-ordering-service)
- [Why did my smart contract installation, instantiation or upgrade fail?](#ibp-console-smart-contracts-troubleshoot-entry1)
- [Why is the smart contract that I installed on the peer not listed in the UI?](#ibp-console-build-network-troubleshoot-missing-sc)
- [My channel, smart contracts, and identities have disappeared from the console. How can I get them back?](/docs/blockchain?topic=blockchain-ibp-v2-troubleshooting#ibp-v2-troubleshooting-browser-storage)
- [Why am I getting the error `Unable to authenticate with the enroll ID and secret you provided` when I create a new organization MSP definition?](#ibp-v2-troubleshooting-create-msp)
- [Why am I getting the error `An error occurred when updating channel` when I try to add an organization to my channel?](#ibp-v2-troubleshooting-update-channel)
- [When I log in to my console, why am I getting a 401 unauthorized error?](#ibp-v2-troubleshooting-console-401)
- [Why am I getting a `Cluster linking is taking too long` error when I try to link my Kubernetes cluster in {{site.data.keyword.cloud_notm}} to my {{site.data.keyword.blockchainfull_notm}} Platform service instance?](#ibp-v2-troubleshooting-console-helm-reset)
- [Why am I getting an error “all SubConns are in TransientFailure” on the console?](#ibp-console-transientfailure)

**Issues with your Nodes**  

- [Why is my first invoke of a smart contract returning the following error: no suitable peers available to initialize from?](#ibp-v2-troubleshooting-smart-contract-anchor-peers)
- [Why are my node operations failing after I create my peer or ordering service?](#ibp-console-build-network-troubleshoot-entry1)
- [Why does my peer or ordering node fail to start?](#ibp-console-build-network-troubleshoot-entry2)
- [What is the proper way to clean up a failed node deployment?](#ibp-v2-troubleshooting-cleanup)
- [How can I view my smart contract container logs?](#ibp-console-smart-contracts-troubleshoot-entry2)
- [Why is my CA, peer, or ordering node that is configured to use HSM not working?](#ibp-v2-troubleshooting-hsm-proxy)
- [Why are my transactions returning an endorsement policy error: signature set did not satisfy policy?](#ibp-v2-troubleshooting-endorsement-sig-failure)
- [Why are the transactions I submit from VS Code failing with a No endorsement plan available error?](#ibp-v2-troubleshooting-anchor-peer)
- [Why are the transactions I submit from VS Code failing with an endorsement failure?](#ibp-v2-troubleshooting-endorsement)
- [How do I delete a peer pod?](#ibp-troubleshooting-delete-peer)
- [How can I recover a contract after a failed upgrade of the smart contract container?](#ibp-troubleshooting-contract-fail)

**Issues on {{site.data.keyword.cloud_notm}}**  

- [My Kubernetes cluster on {{site.data.keyword.cloud_notm}} expired. What does this mean?](#ibp-v2-troubleshooting-cluster-expired)
- [After I deploy a node, I'm seeing a message in my Kubernetes cluster on {{site.data.keyword.cloud_notm}} reporting that the pod has unbound immediate persistent volume claims. Is this an error?](#ibp-v2-troubleshooting-unbound-persistent-volume-claim)
- [After I deploy a node, I'm seeing a message in my Kubernetes cluster on {{site.data.keyword.cloud_notm}} reporting that the pod has hit a crash loop backoff. Is this an error?](#ibp-v2-troubleshooting-crash-loop-backoff)

**Issues with upgrading your Enterprise Plan network**  

- [How can I retry a chaincode migration?](#ibp-v2-troubleshooting-upgrade-tool)



## Why are my console actions failing in my Chrome browser Version 77.0.3865.90 (Official Build) (64-bit)?
{: #ibp-v2-troubleshooting-chrome-v77}
{: troubleshoot}

The console has been working successfully, but requests have started to fail. For example, after I create an ordering service and open it I see the error: `Unable to get system channel. If you associated an identity without administrative privilege on the ordering service node, you will not be able to view or manage ordering service details.`
{: tsSymptoms}

This problem can be caused by a [bug](https://bugs.chromium.org/p/chromium/issues/detail?id=1006243){: external} introduced by the Chrome browser `Version 77.0.3865.90 (Official Build) (64-bit)` that causes actions from the browser to fail.
{: tsCauses}

To resolve this problem, open the console in a new browser tab in Chrome. Any identities that you saved in your console wallet will  persist in the new browser tab. To avoid this problem you can upgrade your Chrome browser version. Ensure you have downloaded all of your wallet identities to your local machine before closing your browser. 
{: tsResolve}



## When I hover over my node, the status is `Status unavailable` or `Status unknown`, what does this mean?
{: #ibp-v2-troubleshooting-status-unavailable}
{: troubleshoot}

A CA, peer, or ordering node has a grey status box, meaning the status of the node is not available. Ideally, when you hover over any node, the node status should be `Running`.
{: tsSymptoms}

This problem can occur if the node is newly created and the deployment process has not completed. If the node is a CA and the status has been grey for more than a few minutes, then it is likely that the deployment process has failed. Peers and ordering nodes take longer to deploy, but this condition can also occur when the health checker that runs against the peer or ordering nodes cannot contact the node. The request for status can fail with a timeout error because the node did not respond within a specific time period, the node could be down, or network connectivity is down.
{: tsCauses}

If this is a new node, wait a few more minutes for the deployment to complete. You can try reloading the page in your browser to refresh the status. If the node is not new,
[examine the associated node logs](/docs/blockchain?topic=blockchain-ibp-console-manage-console#ibp-console-manage-console-node-logs)  for errors to determine the cause.
{: tsResolve}

## When I hover over my node, the status is `Status undetectable`, what does this mean?
{: #ibp-v2-troubleshooting-status-undetectable}
{: troubleshoot}

The node status in the tile for the peer or ordering node is yellow, meaning the status of the node cannot be detected. Ideally, when you hover over any node, the node status should be `Running`.
{: tsSymptoms}

This condition only occurs on peer and ordering nodes that were *imported* to the console and the health checker cannot run against the node. This status happens because an `operations_url` was not specified when the node was imported. An operations url is required for the node health checker to run. The node itself is likely `Running`, but because the operations url was not specified, its status cannot be determined.
{: tsCauses}

You can resolve this problem by performing the following steps:
 1. Click the node tile to open it.
 2. Click the **Settings** icon.
 3. Click **Associate identity**, view and make note of the identity that is associated with this node. Click **Cancel** to close this panel.
 4. Click **Export** to generate a `JSON` file for the node.
 5. Edit the generated `JSON` file and enter the `operations_url` for the node. The value of the `operations_url` depends on how it was configured as well as various network settings. This value needs to be provided by the network administrator who deployed the node.
 6. Click **Delete**. This step removes the imported node from the console, but does not delete the actual node.
 7. From the **Nodes** tab, click **Add Peer** or **Add ordering service** followed by **Import an existing peer** or **Import an existing Ordering service**.
 8. Click **Upload JSON** and browse to the JSON file you just edited. Click **Next**.
 9. Associate the same identity you noted in step three.
 10. Click **Add peer** or **Add ordering service**.
The health checker can now run against the node and report the status of the node.
{: tsResolve}


## Why am I getting the error `Unable to get system channel` when I open my ordering service?
{: #ibp-troubleshoot-ordering-service}
{: troubleshoot}

After you create an ordering service, the status is `Running`. But when you open the ordering service you see the error:
{: tsSymptoms}

```
Unable to get system channel. If you associated an identity without administrative privilege on the ordering service node,
you will not be able to view or manage ordering service details.
```

This problem can occur when the console has lost contact with your Kubernetes cluster on {{site.data.keyword.cloud_notm}}. This can happen if your console has not recently communicated with your cluster, or if you have made changes to your cluster that have overridden the settings used by the {{site.data.keyword.blockchainfull_notm}} platform.
{: tsCauses}

You can renew the communication between your console and the {{site.data.keyword.IBM_notm}} Kubernetes service cluster on {{site.data.keyword.cloud_notm}} by clicking on the service instance of your console in your **Resource list** and clicking the **Refresh cluster** button. When the link between your cluster and your console has been refreshed, click the **Launch the IBM Blockchain Platform** button.
{: tsResolve}


## Why did my smart contract installation, instantiation or upgrade fail?
{: #ibp-console-smart-contracts-troubleshoot-entry1}
{: troubleshoot}
{: support}

It is possible you may experience an error when installing, instantiating or upgrading a smart contract. For example, when you try to install a smart contract on a peer, it fails with the error `An error occurred when installing smart contract on peer.`
{: tsSymptoms}

You may receive this error if this version of the smart contract already exists on the peer, or if your peer is out of resources.
{: tsCauses}

- Open your Kubernetes dashboard and ensure the peer status is `Running`.
- Open the peer node and ensure the smart contract version does not already exist on the peer and try again with the proper version.
- If you are still experiencing problems after the node is up, [check your node logs](/docs/blockchain?topic=blockchain-ibp-console-manage-console#ibp-console-manage-console-node-logs)  for errors.
{: tsResolve}



## Why is the smart contract that I installed on the peer not listed in the UI?
{: #ibp-console-build-network-troubleshoot-missing-sc}
{: troubleshoot}

A smart contract was installed on a peer but when you click on the **Smart contracts** tab, it is not listed.
{: tsSymptoms}

This issue can occur when another user or application installs the smart contract onto the peer and you do not have the peer admin identity in your browser wallet.
{: tsCauses}

In order to view the smart contracts installed on a peer, you need to be a peer admin. Ensure that the peer admin identity exists in your browser wallet. If it does not, you need to import it into your console wallet.
{: tsResolve}

## My nodes, channels, smart contracts, and identities have disappeared from the console. How can I get them back?
{: #ibp-v2-troubleshooting-browser-storage}
{: troubleshoot}
{: support}

Nodes, identities, channels, or smart contracts (or some combination of these) that I successfully deployed into the console are no longer available.
{: tsSymptoms}

One of the features of {{site.data.keyword.blockchainfull_notm}} Platform is that you are now responsible for securing and managing your certificates. Therefore, they are only persisted in the browser local storage to allow you to manage the component. If you are using a private browser window and then switch to another browser or non-private browser window, the identities that you created will be gone from your wallet in the new browser session. Therefore, it is required that you export the identities from the wallet in your private browser session to your file system. You can then import them into your non-private browser session if they are needed. Otherwise, there is no way to recover them.
{: tsCauses}

- Whenever you create a new organization MSP definition, you generate keys for an identity that is allowed to administer the organization. Therefore, during that process you must click the **Generate** and then **Export** buttons to store the generated identity in your wallet and then save it to your file system as a JSON file.
- To resolve this problem in your browser, you need to import those identities and associate them with the corresponding node:
  - In the browser where you are experiencing the problem, click the **Wallet** tab followed by **Add identity** to import the JSON file into your wallet.
  - Click **Upload JSON** and browse to the JSON file you exported using the **Add files** button.
  - Click **Submit**.
  - Now open the peer or ordering service node that this identity was originally associated to, and click on the **Settings** icon.
  - Click the **Associate identity** button.
  - Select the identity you just imported to your console wallet from the drop-down list.
  - Click **Associate**.
- Repeat this process for each identity that was in the wallet of the original browser.
{: tsResolve}

This problem can also occur when the console has lost contact with your Kubernetes cluster on {{site.data.keyword.cloud_notm}}. This can happen if your console has not recently communicated with your cluster, or if you have made changes to your cluster that have overridden the settings used by the {{site.data.keyword.blockchainfull_notm}} platform.
{: tsCauses}

You can renew the communication between your console and the {{site.data.keyword.IBM_notm}} Kubernetes service cluster on {{site.data.keyword.cloud_notm}} by clicking on the service instance of your console in your **Resource list** and clicking the **Refresh cluster** button. When the link between your cluster and your console has been refreshed, click the **Launch the IBM Blockchain Platform** button.
{: tsResolve}

## Why am I getting the error `Unable to authenticate with the enroll ID and secret you provided` when I create a new organization MSP definition?
{: #ibp-v2-troubleshooting-create-msp}
{: troubleshoot}
{: support}

When you attempt to create a new organization MSP definition from the Organizations tab, you experience the error `Unable to authenticate with the enroll ID and secret you provided`.
{: tsSymptoms}

This error occurs when the value that you specified for the enroll secret is not valid for the enroll ID that you selected in the `Generate organization admin certificate` section of the panel.
{: tsCauses}

Verify that you have selected the correct organization admin enroll ID from enroll ID drop down list and enter the correct value for the enroll secret.
{: tsResolve}

## Why am I getting the error `An error occurred when updating channel` when I try to add an organization to my channel?
{: #ibp-v2-troubleshooting-update-channel}
{: troubleshoot}

When you attempt to add another organization to a channel, the update fails with the message `An error occurred when updating channel`.
{: tsSymptoms}

This error occurs when the selected **Channel Updater MSP ID** on the **Update channel** panel is not an admin of the channel.
{: tsCauses}

On the **Update channel** panel, scroll down to the **Channel Updater MSP ID** and select the MSP ID that was specified when the channel was created or specify the MSP ID that is the admin of the channel.
{: tsResolve}

## When I log in to my console, why am I getting a 401 Unauthorized error?
{: #ibp-v2-troubleshooting-console-401}
{: troubleshoot}
{: support}

When I try to log in to my console, I am unable to access the console from my browser. If I check the browser logs, I can find the error 401 unauthorized.
{: tsSymptoms}

Your browser console session times out after **8 hours** of inactivity. If a session becomes inactive, the console will prevent the inactive user from performing any actions.
{: tsCauses}

If your session has become inactive, you can try simply refreshing your browser. If that does not work, close the browser including **all** tabs and windows. Open the URL again. You will be required to log in.

As a best practice, you should have already stored your certificates and identities on your file system. If you happen to be using an incognito window, all the certificates are deleted from the browser local storage when you close the browser. After you log in again you will need to re-import your identities and certificates.
{: note}

## Why am I getting a `Cluster linking is taking too long` error when I try to link my Kubernetes cluster in {{site.data.keyword.cloud_notm}} to my {{site.data.keyword.blockchainfull_notm}} Platform service instance?
{: #ibp-v2-troubleshooting-console-helm-reset}
{: troubleshoot}
{: support}

After attempting to link my Kubernetes cluster on {{site.data.keyword.cloud_notm}} to my {{site.data.keyword.blockchainfull_notm}} Platform service instance, it fails with the error `Cluster linking is taking too long`.
{: tsSymptoms}

This issue can occur when your cluster is busy processing other requests and does not respond to the linking request in a timely matter.
{: tsCauses}

To resolve this problem you can run the `helm reset` command to delete the tiller and then try to link your cluster again. The tiller is the helm mechanism that the blockchain deployer uses to set up components on your cluster.
{: tsResolve}
Run the following command from your {{site.data.keyword.cloud_notm}} CLI terminal:

```
bx api cloud.ibm.com
bx login
bx cs clusters
$(bx cs cluster-config <cluster_name> --export)
kubectl get deployments #test that the connection is working
helm reset
```
{: codeblock}

Attempt to link your cluster again. If it continues to fail, you should [Contact Support](/docs/blockchain?topic=blockchain-blockchain-support#blockchain-support-cases).

## Why am I getting an error “all SubConns are in TransientFailure” on the console?
{: #ibp-console-transientfailure}
{: troubleshoot}

The following error appears on the console: "All SubConns are in TransientFailutre."
{: tsSymptoms}

An Out of Memory (OOM) situation can cause this error.
{: tsCauses}

To resolve this problem, you need to resize the peers and CouchDB containers to add more memory, such as 2000 MB memory each. After resizing the memory, [delete the peer pods](#ibp-troubleshooting-delete-peer) so they will be re-created. Then try the scenario again. See [Considerations when reallocating resources](/docs/blockchain?topic=blockchain-ibp-console-govern-components#ibp-console-govern-components-reallocate-resources) for more information.
{: tsResolve}

## Why is my first invoke of a smart contract returning the following error: no suitable peers available to initialize from?
{: #ibp-v2-troubleshooting-smart-contract-anchor-peers}
{: troubleshoot}
{: support}

When I try to invoke a smart contract from the Fabric SDK, the transaction fails and returns the following error:
{: tsSymptoms}

```
error: [Network]: _initializeInternalChannel: no suitable peers available to initialize from Failed to submit transaction: Error: no suitable peers available to initialize from
```

This error occurs if you have not configured an anchor peer on your channel. Unless you have manually updated your connection profile, your application needs to use the [Service Discovery](https://hyperledger-fabric.readthedocs.io/en/release-1.4/discovery-overview.html){: external} feature to learn about the peers it needs to submit the transaction to.
{: tsCauses}

Use the following steps to [configure anchor peers on your channel](/docs/blockchain?topic=blockchain-ibp-console-govern#ibp-console-govern-channels-anchor-peers).
Also, you should verify that you are submitting the transactions against the correct channel and organization MSP id.
{: tsResolve}

## Why are my node operations failing after I create my peer or ordering service?
{: #ibp-console-build-network-troubleshoot-entry1}
{: troubleshoot}

It is possible you may experience an error when managing an existing node. When that occurs, it is often useful to consult the node logs.

For example, when you try to operate the node, the action might fail.
{: tsSymptoms}

After creating a new peer or ordering service, depending on your cluster storage configuration, it may take a few minutes for the nodes to be ready for operation.
{: tsCauses}


Check your Kubernetes dashboard and ensure the peer or node status is `Running`. Then try your action again. If you are still experiencing problems after the node is up, [check your node logs](/docs/blockchain?topic=blockchain-ibp-console-manage-console#ibp-console-manage-console-node-logs) for errors.  
{: tsResolve}




## Why does my peer or ordering node fail to start?
{: #ibp-console-build-network-troubleshoot-entry2}
{: troubleshoot}

It is possible to experience this error under a variety of conditions.

The peer log includes:
{: tsSymptoms}
```
[main] InitCmd -> ERRO 001 Cannot run peer because cannot init crypto, folder “/certs/msp” does not exist`
```
or the ordering node log contains:

```
Failed to initialize local MSP: admin 0 is invalid [The identity does not contain OU [CLIENT], MSP: [orderermsp],The identity does not contain OU [ADMIN], MSP: [orderermsp]]
```

- This error can occur under the following conditions:
  - When you created the peer or ordering service organization MSP definition, you specified an enroll ID and secret that corresponds to an identity of type `peer` and not `client` or `admin`. It must be of type `client` or `admin`.
  - When you created the peer or ordering service organization MSP definition, you specified an enroll ID and secret that does not match the enroll ID or secret of the corresponding organization admin identity.
  - When you created the peer or ordering service, you specified the enroll ID and secret of an identity that is not type 'peer' or 'orderer'.
  - When you created the peer or ordering service, you associated an identity that does not have the `admin` role.

{: tsResolve}
- Open your peer or ordering service CA node and view the registered identities listed in the **Registered Users** table.
- Delete the peer or ordering service and recreate it, being careful to specify the correct enroll ID and secret of a user that has the `peer` or `orderer` role and associate an identity that has a role of `admin` with the node.
- Note that before you create the peer or ordering service, you need to register an organization admin user, with a type `admin`. Be sure to specify that same id as the enroll ID when you create the organization MSP definition. See these instructions for [registering peer identities](/docs/blockchain?topic=blockchain-ibp-console-build-network#ibp-console-build-network-use-CA-org1) and these instructions for [registering orderer identities](/docs/blockchain?topic=blockchain-ibp-console-build-network#ibp-console-build-network-use-CA-orderer).

## What is the proper way to clean up a failed node deployment?
{: #ibp-v2-troubleshooting-cleanup}
{: troubleshoot}

Sometimes a node can fail to deploy, for example, due to lack of resources in your Kubernetes cluster. After you understand the cause of the node deployment failure, you need to clean up the failed node in your cluster.
{: tsSymptoms}

Do not attempt to use Kubernetes commands to remove the node. Instead, it is extremely important that you use the {{site.data.keyword.blockchainfull_notm}} Platform console or the APIs to remove the failed node to ensure that the associated metadata and storage are also cleaned up.
{: tsResolve}

## How can I view my smart contract container logs?
{: #ibp-console-smart-contracts-troubleshoot-entry2}
{: troubleshoot}

You may need to view your smart contract, or chaincode, container logs to debug a smart contract issue.
{: tsSymptoms}




Follow these instructions to view your smart contract container logs on [{{site.data.keyword.cloud_notm}}](/docs/blockchain?topic=blockchain-ibp-console-manage-console#ibp-console-manage-console-container-logs).
{: tsResolve}


## Why is my CA, peer, or ordering node that is configured to use HSM not working?
{: #ibp-v2-troubleshooting-hsm-proxy}
{: troubleshoot}

This problem can happen when you try to deploy a CA, peer, or ordering node that is configured with HSM and the deployment fails. Or it can surface when a node that is configured for HSM stops working. The node, client, or SDK logs contain the following error:
{: tsSymptoms}
```
{"code":1000,"message":"Private key not found [pkcs11: 0x30: CKR_DEVICE_ERROR]"}"
```
or
```
{"code":1000,"message":"Private key not found [pkcs11: 0xB3: CKR_SESSION_HANDLE_INVALID]"}"
```


This problem happens when the PKCS #11 proxy that is associated with the HSM is unreachable due to a network problem or if the proxy restarts after the node has connected to it.
{: tsCauses}

To re-establish communications between the node and the proxy, restart the failing node by deleting the pod associated with the node. A new pod will be created and the connection with the PKCS #11 proxy is restored. Use the following steps to restart the failing node:
{: tsResolve}
- List the pods: `kubectl get pods -n <NAMESPACE>`
- Delete the pod: `kubectl delete pod -n <NAMESPACE> <PODNAME>`  

Replace:
- `<NAMESPACE>` with the namespace where the associated pods are deployed.
To find the namespace, open any CA node in your console and click the Settings icon. View the value of the Certificate Authority endpoint URL. For example: https://n2734d0-paorg10524.ibpv2-cluster.us-south.containers.appdomain.cloud:7054.
The namespace is the first part of the url beginning with the letter n and followed by a random string of six alphanumeric characters. So in this example, the value of the namespace is `n2734d0`.
- `<PODNAME>` with the **Name** of the failing pod that is visible in the list of pods returned by the previous command.

## Why are my transactions returning an endorsement policy error: signature set did not satisfy policy?
{: #ibp-v2-troubleshooting-endorsement-sig-failure}
{: troubleshoot}

When I invoke a smart contract to submit a transaction, the transaction returns the following endorsement policy failure:
{: tsSymptoms}

```
returned error: VSCC error: endorsement policy failure, err: signature set did not satisfy policy
```

If you have recently joined a channel and installed the smart contract, this error occurs if you have not added your organization to the endorsement policy. Because your organization is not on the list of organizations who can endorse a transaction from the smart contract, the endorsement from your peers is rejected by the channel. If you encounter this problem, you can change the endorsement policy by upgrading the smart contract. For more information, see [Specifying an endorsement policy](/docs/blockchain?topic=blockchain-ibp-console-smart-contracts#ibp-console-smart-contracts-endorse) and [Upgrading a smart contract](/docs/blockchain?topic=blockchain-ibp-console-smart-contracts#ibp-console-smart-contracts-upgrade).
{: tsCauses}

## Why are the transactions I submit from VS Code failing with a No endorsement plan available error?
{: #ibp-v2-troubleshooting-anchor-peer}
{: troubleshoot}

Transactions submitted from VS Code fail with an error similar to:
{: tsSymptoms}

```
Error submitting transaction: No endorsement plan available for {"chaincodes":[{"name":"hello-world"}]}
```

This error occurs if you are using the Fabric Service Discovery feature but did not configure any anchor peers on your channel.
{: tsCauses}

Follow step three of the [private data topic](/docs/blockchain?topic=blockchain-ibp-console-smart-contracts#ibp-console-smart-contracts-private-data) in the Deploy a smart contract tutorial to configure your anchor peers.
{: tsResolve}

## Why are the transactions I submit from VS Code failing with an endorsement failure?
{: #ibp-v2-troubleshooting-endorsement}
{: troubleshoot}

My smart contract endorsement proposals from my peer are failing from my client application with the endorsement error `error: [Channel.js]: Channel:<channel_name> received discovery error:failed constructing descriptor for chaincodes:<name:"chaincode-name">` or `[ERROR] Error submitting transaction: No endorsement plan available for {"chaincodes":[{"name":"MyAssetContract"}]}`
{: tsSymptoms}

Also in the endorsing peer logs I can see the error:
```
UTC [discovery] chaincodeQuery -> ERRO 23c Failed constructing descriptor for chaincode chaincodes:<name:"chaincode-name">,: cannot satisfy any principal combination
```

This error occurs when the peer's enroll id type does not match the smart contract endorsement policy that was configured when the smart contract was instantiated on the channel.
{: tsCauses}

The only way to resolve this error is to delete the peer and create a new one with an enroll id that has the correct type `peer`. You can use the enroll id and secret from an existing user of type `peer` from the peer's CA or register a new user with type `peer`. Follow the instructions in the [Build a network tutorial](/docs/blockchain?topic=blockchain-ibp-console-build-network#ibp-console-build-network-create-peer-org1) to create a new peer identity with the correct type and peer.
{: tsResolve}

## How do I delete a peer pod?
{: #ibp-troubleshooting-delete-peer}
{: troubleshoot}

You are looking for the commands to delete a peer.
{: tsSymptoms}

Use the following CLI command to identify and then delete and restart the failing peer:
{: tsResolve}

* List the pods: `kubectl get pods --all-namespaces`
* Delete the pod: `kubectl delete pod -n <NAMESPACE> <PODNAME>`  

Replace `<NAMESPACE>` with the name of your Kubernetes namespace or your OpenShift project.

## How can I recover a contract after a failed upgrade of the smart contract container?
{: #ibp-troubleshooting-contract-fail}
{: troubleshoot}

All contracts were lost after the procedure to upgrade the smart contract container crashed.
{: tsSymptoms}

[Delete all the peer pods](#ibp-troubleshooting-delete-peer). This deletion triggers the peer to be created again and restarts the proxy.
{: tsResolve}


## My Kubernetes cluster on {{site.data.keyword.cloud_notm}} expired. What does this mean?
{: #ibp-v2-troubleshooting-cluster-expired}
{: troubleshoot}

I received an e-mail that my {{site.data.keyword.IBM_notm}} Kubernetes service cluster is about to expire or its status is `Expired`. Or, you are not able to access the console after 30 days.
{: tsSymptoms}

Free Kubernetes clusters are only valid for 30 days.
{: tsCauses}

It is not possible to migrate from a free cluster to a paid cluster. After 30 days you cannot access the console and all of your nodes and certificates are deleted. See this topic on [Kubernetes cluster expiration](/docs/blockchain?topic=blockchain-ibp-console-manage-console#ibp-console-manage-console-cluster-expiration) for information on what is happening and what you can do.
{: tsResolve}

## After I deploy a node in the console, I'm seeing a message in my Kubernetes cluster on {{site.data.keyword.cloud_notm}} reporting that the pod has unbound immediate persistent volume claims. Is this an error?
{: #ibp-v2-troubleshooting-unbound-persistent-volume-claim}
{: troubleshoot}
{: support}

I see what looks like an error in my Kubernetes cluster on {{site.data.keyword.cloud_notm}} reporting that a pod has unbound immediate persistent volume claims and am worried that my node has failed to deploy.
{: tsSymptoms}

More time is needed for the node to come up.
{: tsCauses}

Unbound immediate persistent volume claims are a normal part of the deployment process for any node, reflecting that the persistent volume claim has been made but that the node itself has not yet deployed. Wait a few minutes for the node to deploy.
{: tsResolve}

## After I deploy a node, I'm seeing a message in my Kubernetes cluster on {{site.data.keyword.cloud_notm}} reporting that the pod has hit a crash loop backoff. Is this an error?
{: #ibp-v2-troubleshooting-crash-loop-backoff}
{: troubleshoot}

I see an error in my Kubernetes cluster on {{site.data.keyword.cloud_notm}} reporting that the pod has hit a crash loop backoff and am worried that my node has failed to deploy.

This node has failed to deploy.
{: tsCauses}

The node has failed to deploy. There can be several reasons for this, but you must go to your console, delete the node, and attempt to redeploy it. Make sure you are using the correct MSP, enroll ID, and secret.
{: tsResolve}


## How can I retry a chaincode migration?
{: #ibp-v2-troubleshooting-upgrade-tool}
{: troubleshoot}

When I used the {{site.data.keyword.blockchainfull_notm}} Platform 2.0 console to upgrade my chaincode, I encountered an error starting the chaincode container. How can I retry the chaincode migration?
{: tsCauses}

Complete the following steps with the upgrade tool to retry a failed chaincode migration:
{: tsResolve}

1. Fix your chaincode source code to resolve the problem that caused the migration failure. You can test your chaincode by installing and instantiating it on a test network to confirm that the chaincode can build correctly.

2. Install and instantiate the updated chaincode on your Enterprise Plan network with a different **version**.

3. After you have installed the updated chaincode, you can refresh the migration tool in your browser to see the new chaincode in the **Migrate Chaincode** panel. You can then use the upgrade tool to install the updated chaincode on your peers on {{site.data.keyword.blockchainfull_notm}} Platform 2.0.

If you cannot change the chaincode version, you need to use the upgrade tool to delete the upgraded peer on the new platform and then use the tool to create a new peer. After you have fixed the chaincode source code and installed and instantiated it on your Enterprise Plan network, you can use the tool to install a fixed version of your chaincode on the new peer.


