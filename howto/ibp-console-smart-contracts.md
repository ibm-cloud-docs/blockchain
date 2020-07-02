---

copyright:
  years: 2019, 2020
lastupdated: "2020-07-01"

keywords: smart contract, private data, private data collection, anchor peer

subcollection: blockchain

---

{:external: target="_blank" .external}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:note: .note}
{:term: .term}
{:important: .important}
{:tip: .tip}
{:pre: .pre}

# Deploy a smart contract on the network
{: #ibp-console-smart-contracts}




A smart contract is the code, sometimes referred to as chaincode, that applications interact with to read and update data on the blockchain ledger. A smart contract can turn business logic into an executable program that is agreed to and verified by all members of a blockchain network. This tutorial is the third part in the [sample network tutorial series](#ibp-console-smart-contracts-structure) and describes how to deploy smart contracts to start transactions in the blockchain network.
{:shortdesc}

**Target audience:** This topic is designed for network operators who are responsible for creating, monitoring, and managing the blockchain network. Additionally, application developers might be interested in the sections that reference how to create a smart contract.

## Sample network tutorial series
{: #ibp-console-smart-contracts-structure}

You are currently on the third part of our three-part tutorial series. This tutorial guides you through the process of using the console to deploy a smart contract onto a channel in your {{site.data.keyword.blockchainfull_notm}} Platform network.

* [Build a network tutorial](/docs/blockchain?topic=blockchain-ibp-console-build-network#ibp-console-build-network) guides you through the process of hosting a network by creating an orderer and peer.
* [Join a network tutorial](/docs/blockchain?topic=blockchain-ibp-console-join-network#ibp-console-join-network) guides you through the process of joining an existing network by creating a peer and joining it to a channel.
* **Deploy a smart contract on the network** (Current tutorial) Provides information on how to write a smart contract and deploy it on your network.

You can use the steps in these tutorials to build a network with multiple organizations in one cluster for the purposes of development and testing. Use the **Build a network** tutorial if you want to form a blockchain consortium by creating an orderer node and adding organizations. Use the **Join a network** tutorial to connect a peer to the network. Following the tutorials with different consortium members helps you create a truly **distributed** blockchain network.

This final tutorial is meant to show how to create and package a smart contract, how to install the smart contract on a peer, and how to instantiate the smart contract on a channel.

Smart contracts are installed on peers and then instantiated on channels. **All members that want to submit transactions or read data by using a smart contract need to install the contract on their peer.** A smart contract is defined by its name and version. Therefore, both the name and version of the installed contract need to be consistent across all peers on the channel that plan to run the smart contract.

After a smart contract is installed on the peers, a single network member instantiates the contract on the channel. The network member needs to have joined the channel in order to perform this action. Instantiation updates the ledger with the initial data that is used by the smart contract, and then starts smart contract pods. The peers can then use the smart contract.

- **Only one network member needs to instantiate a smart contract.**
- **If a peer with a smart contract installed joins a channel where the same smart contract version has already been instantiated, the smart contract container starts automatically.**  

In this tutorial, we go through the steps to use the {{site.data.keyword.blockchainfull_notm}} Platform console to manage the deployment of smart contracts on your network:

- [Write and package your smart contract](/docs/blockchain?topic=blockchain-ibp-console-smart-contracts#ibp-console-smart-contracts-write-package).
- [Install smart contracts on your peers](/docs/blockchain?topic=blockchain-ibp-console-smart-contracts#ibp-console-smart-contracts-install).
- [Instantiate them on channels](/docs/blockchain?topic=blockchain-ibp-console-smart-contracts#ibp-console-smart-contracts-instantiate).
- [Specify endorsement policies](/docs/blockchain?topic=blockchain-ibp-console-smart-contracts#ibp-console-smart-contracts-endorse).
- [Upgrade the smart contract policies and code](/docs/blockchain?topic=blockchain-ibp-console-smart-contracts#ibp-console-smart-contracts-upgrade).


## Before you begin

Before you can install a smart contract, ensure that you have the following things ready.

- You must either [build a network](/docs/blockchain?topic=blockchain-ibp-console-build-network#ibp-console-build-network) or [join a network](/docs/blockchain?topic=blockchain-ibp-console-join-network#ibp-console-join-network) by using your {{site.data.keyword.blockchainfull_notm}} Platform console.
- Because smart contracts are installed onto peers, ensure that you [add peers](/docs/blockchain?topic=blockchain-ibp-console-build-network#ibp-console-build-network-create-peer-org1) to your console.  
- Smart contracts are instantiated on a channel. Therefore, you must [create a channel](/docs/blockchain?topic=blockchain-ibp-console-build-network#ibp-console-build-network-create-channel) or [join a channel](/docs/blockchain?topic=blockchain-ibp-console-join-network#ibp-console-join-network-join-peer-org2) by using your console.
- The {{site.data.keyword.blockchainfull_notm}} Platform supports smart contracts written in JavaScript, TypeScript, Java, and Go. When you are allocating resources to your peer node, it is important to note that JavaScript and TypeScript smart contracts require more resources than contracts written in Go.

## Step one: Write and package your smart contract
{: #ibp-console-smart-contracts-write-package}

The {{site.data.keyword.blockchainfull_notm}} console manages the *deployment* of smart contracts rather than development. If you are interested in developing smart contracts, you can get started using tutorials provided by the Hyperledger Fabric community and tooling provided by {{site.data.keyword.IBM_notm}}.

- To learn how smart contracts can be used to conduct transactions among multiple parties, see the [Developing applications topic](https://hyperledger-fabric.readthedocs.io/en/release-1.4/developapps/developing_applications.html){: external} in the Hyperledger Fabric documentation.
- When you are ready to start building smart contracts, you can use the [{{site.data.keyword.blockchainfull_notm}} Visual Studio code extension](/docs/blockchain?topic=blockchain-develop-vscode) to start building your own smart contract project. You can also use that extension to [connect directly to your network from Visual Studio Code](/docs/blockchain?topic=blockchain-develop-vscode#develop-vscode-connecting-ibp) and explore the inline tutorials.
- For a quick tutorial on developing smart contracts, see [Develop a smart contract with the IBM Blockchain Platform VS Code extension](https://developer.ibm.com/tutorials/ibm-blockchain-platform-vscode-smart-contract/){: external}.
- For a more in-depth end-to-end tutorial about using an application to interact with smart contracts, see [Hyperledger Fabric Commercial Paper tutorial](https://hyperledger-fabric.readthedocs.io/en/release-1.4/tutorial/commercial_paper.html){: external}.
- To learn about how to incorporate access control mechanisms into your smart contract, see [Chaincode for Developers](https://hyperledger-fabric.readthedocs.io/en/release-1.4/chaincode4ade.html#chaincode-access-control){: external}.

Because Fabric v2.x peers do not have a "shim" (the external dependencies that allowed smart contracts to run on earlier versions of Fabric), you will have to vendor the shim and then repackage any smart contracts written in Golang (go) that you installed on peers deployed using the 1.4.x Fabric image. Without this vendoring and repackaging, the smart contract will not run on a peer using a Fabric 2.x image. You will not need to do this on smart contracts that are written in Java or Node.js, nor for smart contracts written and packaged using the 2.0 package.
{: important}

When you are ready to deploy your smart contract to the {{site.data.keyword.blockchainfull_notm}} platform, the smart contract must be packaged into `.cds` format. For more information, see [Packaging smart contracts](/docs/blockchain?topic=blockchain-develop-vscode#packaging-a-smart-contract). Alternatively, you can use peer CLI commands to build the package. For v1.4.x commands, see [1.4.x peer cli commands](https://hyperledger-fabric.readthedocs.io/en/release-1.4/commands/peerchaincode.html#peer-chaincode-package){: external}. For v2.x commands, see [2.x peer cli commands](https://hyperledger-fabric.readthedocs.io/en/release-2.0/commands/peerchaincode.html#peer-chaincode-package){: external}.

### Vendoring smart contracts
{: #ibp-console-smart-contracts-write-package-vendor}

To vendor the shim for a go smart contract, navigate to your smart contract source folder. Then initialize the go module by issuing:

```
go mod init
```
{: codeblock}

Then add the following to your `go.mod` file:

```
require github.com/hyperledger/fabric v1.4.7
```
{: codeblock}

You can then get the necessary dependencies to install the go smart contract on a 2.x peer by issuing:

```
go mod tidy
go mod vendor
```
{: codeblock}

After you have completed these steps, you can repackage your smart contract using the process that is described in [Packaging smart contracts](/docs/blockchain?topic=blockchain-develop-vscode#packaging-a-smart-contract).

## Step two: Install a smart contract
{: #ibp-console-smart-contracts-install}

Use your console to perform these steps:

1. Click the **Smart contracts** tab to install one or more smart contracts.
2. Click **Install smart contract** to upload the smart contract package file in [.cds format](https://hyperledger-fabric.readthedocs.io/en/release-1.4/chaincode4noah.html#packaging){: external}. _The smart contract package file must be less than 25 MB in size._ You can use the {{site.data.keyword.blockchainfull_notm}} Visual Studio Code extension to [create a smart contract package](/docs/blockchain?topic=blockchain-develop-vscode#packaging-a-smart-contract). When you install the package from the **Smart contracts** tab, you can select one or more peer nodes to install the smart contracts on.

If only one peer exists in the console, the smart contract will be installed on it. You are not prompted to select a peer to install the smart contract on. You can navigate to the nodes tab and click a peer that is managed by your console to view the list of smart contracts installed on an individual peer.

You can return to the **Smart contracts** tab later to install the smart contract on additional peers, even after the smart contract was instantiated on the channel. If the version of the smart contract that you install is the same as the instantiated version, these additional peers can process transactions just like the existing peers.
{:tip}

**This console cannot be used to install Hyperledger Composer `.bna` files.**


## Step three: Instantiate a smart contract
{: #ibp-console-smart-contracts-instantiate}

Smart contracts are instantiated on a channel. Any console member with peers joined to a channel can instantiate a smart contract and specify the associated [endorsement policy](/docs/blockchain?topic=blockchain-glossary#glossary-endorsement-policy).

Use your console to perform these steps:

1. On the smart contracts tab, find the smart contract from the list of smart contracts that are installed on your peers and click **Instantiate** from the overflow menu on the right side of the row.
2. On the side panel that opens, select a channel to instantiate the smart contract on. You can select the channel, named `channel1`, which you created. Then, click **Next**.
3. Specify the [endorsement policy for the smart contract](/docs/blockchain?topic=blockchain-ibp-console-smart-contracts#ibp-console-smart-contracts-endorse), described in the following section. When multiple organizations are members of the channel, you have the opportunity choose how many organizations are required to endorse the smart contract transactions.
4. You also need to select the organization members to be included in the endorsement policy. If you are following along in the tutorial, that would be `org1msp` and possibly `org2msp` if you completed both the **Build a network** and **Join a network** tutorials.
5. On the Select peer panel, select a peer from the drop-down list that is from an organization that is a member of the channel.
6. If your smart contract includes Fabric private data collections, you need to upload the associated collection configuration JSON file, otherwise you can skip this step. For more information, see [private data](/docs/blockchain?topic=blockchain-ibp-console-smart-contracts#ibp-console-smart-contracts-private-data).
7. On the last panel, you are prompted to specify the name of the smart contract initialization function, along with the associated arguments to pass to that function.

You can view all of the smart contracts that are instantiated on a channel by clicking the channel icon in the left navigation, selecting a channel from the table, and then clicking the **Channel details** tab.

Be aware that if you use a free Kubernetes cluster on {{site.data.keyword.cloud_notm}}, instantiation can take significantly longer than in a paid cluster.Depending on the number of peers you have deployed in your cluster, the type of smart contract you are instantiating, and the platform you are running on, instantiation can take several minutes. If the instantiation fails with a timeout error, wait 5 minutes and then retry it again.
{: tip}

The combination of **installation and instantiation** is a powerful feature because it allows for a peer to use a single smart contract across many channels. Peers may want to join multiple channels that use the same smart contract, but with different sets of network members able to access the data. A peer can install the smart contract once and use it on any channel where it has been instantiated. This lightweight approach saves compute and storage space, and helps you scale your network.

## Step four: Send transactions by using your client applications
{: #ibp-console-smart-contracts-connect-to-SDK}

After a smart contract has been instantiated on a channel, you can use a client application to transact with other members of your network. Applications can invoke the business logic that is contained in smart contracts to create, transfer, or update assets on the blockchain ledger.

### Connect with SDK
{: #ibp-console-smart-contracts-connect-to-SDK-panel}

After you create an organization MSP definition, you can download a connection profile that can be used by a client application to connect to your network via one or more gateway peers. The gateway peers are the peers that are specified in the connection profile, and they are used to perform service discovery to find all of the endorsing peers in the network that will endorse transactions.

Click the **Organization MSP** tile for the organization that your client application interacts with. Click **Create connection profile** to open a side panel where you can build and download your connection profile.

  ![Create connection profile panel](../images/create-connx-profile.png "Create connection profile panel")

If you plan to use the client application to register and enroll users with the organization CA, you need to include the Certificate Authority in the connection profile definition.

Select the peers to include in the connection profile definition. When a peer is not available to process requests from a client application, service discovery ensures that the request is automatically sent to a different peer. Therefore, to accommodate for peer downtime during a maintenance cycle for example, it is recommended that you select more than one peer for redundancy. In addition to peers created by using the console or APIs, imported peers that have been imported into the console are eligible to be selected as well.

The list of channels that the selected peers have joined is also provided for your information. If a channel is missing from the list, it is likely because the peer joined to it is currently unavailable.

You can then download the connection profile to your local file system and use it with your client application to generate certificates and invoke smart contracts.

The connection profile that is downloaded from the {{site.data.keyword.blockchainfull_notm}} Platform console can only be used to connect to your network by using the Node.js (JavaScript and TypeScript) and Java Fabric SDKs.
{: note}

The generated connection profile only supports Fabric CAs. If you manually built your organization MSP with certificates from an external CA, the connection profile will not include any information in the "certificateAuthorities": section.


## Specifying an endorsement policy
{: #ibp-console-smart-contracts-endorse}

Every smart contract must have an endorsement policy, that is specified during instantiation. The endorsement policy specifies the set of organizations, the peers, on a channel that can execute the smart contract and independently validate the transaction output. For example, an endorsement policy can specify that a transaction is added to the ledger only if a majority of the members on the channel endorse the transaction. The organization that instantiates the smart contract can select from among the members who have installed the smart contract to become validators, and sets the endorsement policy for all channel members. You can update your endorsement policy by following the steps for [updating your smart contract](/docs/blockchain?topic=blockchain-ibp-console-smart-contracts#ibp-console-smart-contracts-upgrade).

When you follow the steps to [instantiate a smart contract](/docs/blockchain?topic=blockchain-ibp-console-smart-contracts#ibp-console-smart-contracts-instantiate), you can use the side panel to set a contract's endorsement policy after selecting the channel. Use this panel to specify a simple policy by selecting the peers that need to endorse the transaction from the list of peers that have installed the smart contract on the channel. You can use this method to specify an endorsement policy of all channel members, a majority of them, a single member, or a simple +1 preventing members from self-signing. The default endorsement policy is set to `1 of x`, meaning that only a single member of the channel is required to endorse a smart contract transaction.

Click the **Advanced** button if you want to specify a policy in JSON format. You can use this method to specify more complicated endorsement policies, such as requiring that a certain member of the channel must validate a transaction, along with a majority of other members. You can find additional [examples of advanced endorsement policies](https://hyperledger-fabric.readthedocs.io/en/release-1.4/arch-deep-dive.html#example-endorsement-policies){: external} in the Hyperledger Fabric documentation. For more information about writing endorsement policies in JSON, see [Hyperledger Fabric Node SDK documentation](https://hyperledger.github.io/fabric-sdk-node/release-1.4/global.html#ChaincodeInstantiateUpgradeRequest){: external}.

Endorsement policies are not updated automatically when new organizations join the channel and install a chaincode. For example, if the endorsement policy requires two of five organizations to endorse a transaction, the policy will not be updated to require two out of six organizations when a new organization joins the channel. Instead, the new organization will not be listed on the policy, and they will not be able to endorse transactions. You can add another organization to an endorsement policy by upgrading the relevant chaincode and updating the policy.

### What does the user type have to do with the smart contract endorsement policy?
{: #ibp-console-smart-contracts-endorse-user-type}

It's worth clicking the **Advanced** button to review the endorsement policy that will be used for the smart contract. Every organization that you select from the **Members** drop-down list on this panel will be included in the endorsement policy (represented by the role `mspId` parameter in the policy). The value of the role `name` parameter indicates the type of identity that is required to endorse transactions and can be any of  `client`, `peer`, `orderer`, `admin`, or `member`.  If it is set to `member`, then any of the other four roles will satisfy the criteria.

When a `peer` role is specified, you should ensure that any peers that will be submitting endorsement transactions were registered with the correct type. In the following endorsement policy example, peers who are members of `org1msp` can endorse transactions if their enroll id was registered with the type of `client`, `peer`, `orderer`, or `admin`. However, for peers who are members of `org2msp`, their enroll id must have been registered with a type of `peer` to be able to endorse transactions.

```json
{
    "identities": [
        {
            "role": {
                "name": "member",
                "mspId": "org1msp"
            }
        },
        {
            "role": {
                "name": "peer",
                "mspId": "org2msp"
            }
        }
    ],
    "policy": {
        "1-of-2": [
            {
                "signed-by": 0
            }
        ]
    }
}
```  
{: codeblock}

Unsure which `type` was selected when a peer identity was registered? From the console, you can open the CA where the peer identity was registered and view the list of registered users and their associated type.

## Upgrading a smart contract
{: #ibp-console-smart-contracts-upgrade}

You can upgrade a smart contract to change its code, endorsement policy, or private data collection while maintaining its relationship to the assets on the ledger. There are a variety of reasons why you may want to upgrade an instantiated smart contract.
1. You can upgrade the smart contract to add or remove functionality from its code and iterate on the logic of your business network.
2. Whenever a new member is added to a channel, the endorsement policy of the instantiated smart contracts *must* be updated to include the new channel member. In order to work with the new channel member, the smart contract must be repackaged with a new version number and instantiated on the channel, even if the smart contract itself is unchanged. Otherwise, transaction endorsement cannot succeed.
3. When a private data collection has changed, for example an organization is added or removed you need to upgrade your smart contract. Or, use this action whenever a new private data collection is added to the collection configuration JSON file.
4. The smart contract initialization arguments have changed.

{:important}
Before you upgrade an instantiated smart contract, the new version of the smart contract must be installed on all peers in the channel that are running the previous level of the smart contract.

### How to upgrade a smart contract
{: #ibp-console-smart-contracts-upgrade-howto}

To upgrade a smart contract, install the updated code by specifying the same smart contract name but by using and a new version number. If you have installed a newer version of a smart contract on any peer in the channel, notice that the original version now has the `Upgrade Available` button next to it in the **Instantiated smart contracts** table.

{:important}
When a new member that will run the smart contract joins the channel, it is mandatory to update the smart contract by installing a new version on all the peers and instantiating it on the channel with a modified endorsement policy that includes the new member.

- Navigate to the **Smart contracts** tab on the left.
- Scroll down to the **Instantiated smart contracts** table.
- In the **Instantiated smart contracts** table, locate the smart contract version and channel that you want to upgrade. It must have the **Upgrade Available** label next to it.
- Click the **overflow menu** on the right side of the smart contract row and click **Upgrade** to perform the following steps:  

 1. Select the smart contract version that you want to upgrade on the channel from the drop-down list.
 2. Update the endorsement policy by adding or removing channel members. You can also click **Advanced** to paste in a new JSON formatted string, which modifies the existing policy.
 3. On the Select peer panel you need to select a peer that can approve the proposal to upgrade the smart contract. Therefore, you need to select a peer from the drop-down list that is from an organization that was a member of the channel before the smart contract was last instantiated on the channel.
 4. If you want to associate a private data collection configuration file with the smart contract you can upload the JSON file. Or if you want to update an existing collection configuration, you can upload the JSON file.
 If the smart contract was previously instantiated with a collection configuration file, you **must** again upload the previous version or a new version of the collection configuration file during this step.
 5. (Optional) Modify the smart contract initialization argument values if the parameters have changed. If you are unsure about it, check with your smart contract developer. If they have not changed, you can leave this field blank.

After you upgrade the smart contract, you will change the version of the contract that is instantiated on the channel. If you are using private data collections, be sure you have configured anchor peers on the channel.

### Considerations when you upgrade smart contracts
{: #ibp-console-smart-contracts-upgrade-considerations}

1. Do I need to install the new version of the smart contract on all my peers?  

  When a smart contract is invoked on a peer, it attempts to run the version that is instantiated on the channel. If a previous version is running on the peer, it results in an error. Therefore, before you upgrade a smart contract on a channel, *it is a best practice to install the latest version of the smart contract on all peers that are running the previous version.*  

2. Can a subsequent smart contract version still process data on the ledger from a prior version of the smart contract?  

  Yes. As long as the new smart contract code addresses the data in a compatible way (by adding new JSON fields and not changing the semantics or type of the existing fields), any subsequent version of the smart contract can run against the data from a prior version.

3. What happens to my peers if I forget to upgrade them to the latest version before updating the smart contract on the channel?  

  After updating the channel to use the new version of the smart contract, if there are still peers on the channel running the previous version, those peers are no longer able to endorse transactions for the smart contract. Also, you risk not having enough endorsements for transactions to be committed to the ledger, depending on how the smart contract endorsement policy is defined. However, it is possible to install the new version of the smart contract on these peers later and they will again be able to endorse transactions, effectively catching up.

4. What happens when I remove an organization from my private data collection?

   The peers in that organization continue to store data in the private data collection until its ledger reaches the block that removes its membership from the collection. After that occurs, the peers will not receive private data in any future transactions, and _clients_ of that organization will no longer be able to query the private data via chaincode from any peer.

## Private data
{: #ibp-console-smart-contracts-private-data}

Private data is a feature of Hyperledger Fabric networks at version 1.2 or higher and is used to keep sensitive information private from other organization members **on a channel**. Data privacy is achieved by using [private data collections](https://hyperledger-fabric.readthedocs.io/en/release-1.4/private-data/private-data.html#what-is-a-private-data-collection "What is a private data collection?"){: external}. For example, several wholesalers and a set of farmers might be joined to a single channel. If a farmer and a wholesaler want to transact privately, they can create a channel for this purpose. But they can also decide to create a private data collection on the smart contract that governs their business interactions to maintain privacy over sensitive aspects of the sale, such as the price, without having to create a secondary channel. To learn more about when to use private data within a blockchain, visit the [Private Data](https://hyperledger-fabric.readthedocs.io/en/release-1.4/private-data/private-data.html#private-data "Private data"){: external} concept article in the Fabric documentation.

In order to use private data with {{site.data.keyword.blockchainfull_notm}} Platform, the following three conditions must be satisfied:  
1. **Define the private data collection.** A private data collection file can be added to your smart contract. Then, at run time, your client application can use private data-specific chaincode APIs to input and retrieve data from the collection. For more information about how to use private data collections with your smart contract, see the Fabric SDK tutorial on [Using private data](https://hyperledger.github.io/fabric-sdk-node/release-1.4/tutorial-private-data.html){: external} in the Fabric SDK documentation.  

2. **Install and instantiate the smart contract.** After you define the smart contract private data collection, you need to install the smart contract on the peers that are members of the channel. When you instantiate the smart contract on the channel by using the console, you need to upload the collection configuration JSON file. For more information on how to [create a collection definition JSON file](https://hyperledger.github.io/fabric-sdk-node/release-1.4/tutorial-private-data.html){: external} see the Fabric SDK documentation topic.

  Instead of using the console to install and instantiate your smart contract with a collection config file, you can also use the Fabric SDK. Those instructions are also available under [How to use private data](https://hyperledger.github.io/fabric-sdk-node/release-1.4/tutorial-private-data.html){: external} in the Node SDK documentation.  

  **Note:** A client needs to be an admin of your peer in order to install or instantiate a smart contract by using the SDK. Therefore, you need to download the certificates of the peer admin identity from your console wallet and pass the peer admin's signing certificate and private key to directly to the SDK instead of creating an application identity. For an example of how to pass a key pair to the SDK, see [Connecting to your network by using low-level Fabric SDK APIs](/docs/blockchain?topic=blockchain-ibp-console-app#ibp-console-app-low-level).  

3. **Configure anchor peers.** Because cross organizational [gossip](https://hyperledger-fabric.readthedocs.io/en/release-1.4/gossip.html){: external} must be enabled for private data to work, an anchor peer must exist for each organization in the collection definition. Refer to this information for [how to configure anchor peers](/docs/blockchain?topic=blockchain-ibp-console-govern#ibp-console-govern-channels-anchor-peers) on your network.

Your channel is now configured to use private data.

