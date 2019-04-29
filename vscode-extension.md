---

copyright:
  years: 2017, 2019
lastupdated: "2019-03-05"

subcollection: blockchain

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:note: .note}
{:important: .important}
{:tip: .tip}
{:pre: .pre}

# Tools for smart contracts
{: #develop-vscode}

The {{site.data.keyword.blockchainfull}} Platform Visual Studio Code extension provides an environment within Visual Studio Code for developing, packaging, and deploying smart contract packages. The extension also includes commands to set up a preconfigured local instance of Hyperledger Fabric for simplified local smart contract development.

**Please note:** The {{site.data.keyword.blockchainfull_notm}} Platform extension is compatible with {{site.data.keyword.blockchainfull_notm}} Platform V2 networks, and with Hyperledger Fabric versions 1.4.x and later.

## Prerequisites
{: #develop-vscode-prerequisites}

Before installing the {{site.data.keyword.blockchainfull_notm}} Platform Visual Studio Code extension, complete the following prerequisites.

- **Install Yeoman**

  Yeoman is a generator tool used to create skeleton smart contract projects. Install Yeoman using the following command: `npm install -g yo`

- **Install Docker**

  To run the preconfigured instance of Hyperledger Fabric, ensure you have installed [Docker ![External link icon](images/external_link.svg "External link icon")](https://www.docker.com/).

- **Operating system requirements**

  Currently, this extension is compatible with Mac, Windows, and Linux.

- **Hyperledger Fabric version requirements**

  This extension is compatible with Hyperledger Fabric versions 1.4.0 and later.

## Installing the extension
{: #develop-vscode-installing-the-extension}

1. Navigate to the [Visual Studio Code extension marketplace page ![External link icon](images/external_link.svg "External link icon")](https://marketplace.visualstudio.com/items?itemName=IBMBlockchain.ibm-blockchain-platform) or search for **{{site.data.keyword.blockchainfull_notm}} Platform** in the extensions panel within Visual Studio Code.
2. Click **Install**.
3. Restart Visual Studio Code to complete installation of the extension.

## Visual Studio Code command palette
{: #develop-vscode-command-palette}

This extensions adds many commands to the Visual Studio Code command palette. Many of the operations detailed in this document can also be started by running the command from the Visual Studio Code command palette.

## Creating a project
{: #develop-vscode-creating-a-project}

To create a new project, complete the following steps:

1. Open the **{{site.data.keyword.blockchainfull_notm}}** and click the menu button in the smart contract packages section and click **Create Smart Contract Project**
2. Select the language you wish to create a smart contract in, currently the options are JavaScript, TypeScript, Go and Java.
3. Create a folder with the name of your project, and open it.
4. Select how to open your new project. The project folder should now open.

## Packaging a smart contract
{: #packaging-a-smart-contract}

When a smart contract is ready to be deployed, it must first be packaged. To package a smart contract, complete the following steps:

1. In Visual Studio Code, navigate to the **{{site.data.keyword.blockchainfull_notm}} Platform** panel.
2. In the **Smart Contract Packages** pane, click the + icon. If you have a smart contract project open in the file viewer, it will be automatically packaged, and will be shown in the **Smart Contract Packages** pane. If you have multiple smart contract folders open, you will be asked which one to package. If you have no smart contract folders open, you'll get an error message.

## Importing, Exporting or deleting a smart contract package
{: #develop-vscode-exporting-deleting-smart-contract-package}

After a smart contract has been packaged, it can be exported as a `.cds` file or deleted if it is no longer needed.

To delete a smart contract package:

1. In the {{site.data.keyword.blockchainfull_notm}} Platform extension panel, right-click the smart contract package to delete.
2. Select **Delete Package**.

The package will now be deleted and disappear from the list of smart contract packages.

To export a smart contract package:

1. In the {{site.data.keyword.blockchainfull_notm}} Platform extension panel, right-click the smart contract package to export.
2. Select **Export Package**.
3. Choose the directory to save your smart contract package file and click **Export**.

To import a smart contract package
1. In the **Smart Contract Packages** pane. Click the menu and select **import smart contract package**
2. Browse to the smart contract package you wish to import

## Connecting to and disconnecting from a network
{: #develop-vscode-connecting-and-disconnecting}

When you use this extension, you install and instantiate smart contract packages on peers and channels in your Hyperledger Fabric instance. This extension can initialise a preconfigured local instance of Hyperledger Fabric using Docker.

### Connecting to the preconfigured Hyperledger Fabric instance
{: #develop-vscode-connecting-to-preconfigured-Fabric-instance}

To connect to the preconfigured Hyperledger Fabric instance, first ensure Docker is running on your machine:

1. Open the **{{site.data.keyword.blockchainfull_notm}} Platform** tab in Visual Studio Code.
2. In the **{{site.data.keyword.blockchainfull_notm}} Platform** pane, click **local_fabric**. If Docker is running, the local Hyperledger Fabric instance should be downloaded and started. After starting a connection to the preconfigured local instance. 

## Exploring connections
{: #develop-vscode-exploring-connections}

After connecting to a Hyperledger Fabric instance, you can see a list of the available channels and peers in those channels in the **{{site.data.keyword.blockchainfull_notm}} Platform** pane. Spend a minute exploring your Hyperledger Fabric instance to get familiar with the structure. Listed first are the channels within a connection, under a channel is a list of the peers that are members of that channel, and a list of smart contract packages instantiated on that channel. Under each peer in the list is a list of all smart contract packages installed on that peer. You can also see the versions of smart contract packages that are installed or instantiated.

#### Restarting the preconfigured Hyperledger Fabric runtime
{: #develop-vscode-restarting-Fabric-runtime}

To restart the `local_fabric` runtime:

1. When the runtime is started; from the **Local Fabric Ops** pane click the menu
2. Select **Restart Fabric Runtime**.

The Hyperledger Fabric containers will be stopped and restarted.

#### Stopping the preconfigured Hyperledger Fabric runtime
{: #develop-vscode-restarting-Fabric-runtime}

To stop the `local_fabric` runtime:

1. When the runtime is started; from the **Local Fabric Ops** pane click the menu
2. Select **Stop Fabric Runtime**.

The Hyperledger Fabric containers will be stopped and restarted.

#### Teardown the Hyperledger Fabric runtime
{: #develop-vscode-teardown-Fabric}

To teardown the `local_fabric` runtime:

1. When the runtime is started; from the **Local Fabric Ops** pane click the menu
2. Select **Teardown Fabric Runtime**.

Performing a teardown of the `local_fabric` network will close all Hyperledger Fabric containers. **Please note**: This will result in the loss of the ledger and world state data.

### Exporting connection profile for the preconfigured Hyperledger Fabric runtime
{: #develop-vscode-exploring-connection-details}

The connection details required to connect to the `local_fabric` connection can be exported. Connection details for the `local_fabric` connection are useful for testing client applications intended to connect to or interact with your Hyperledger Fabric instance.

To export the `local_fabric` connection details:

1. Start the preconfigured Hyperledger Fabric runtime.
2. Expand **Nodes**, right click on the peer and select **Export Connection Profile**.

The connection details are saved to a directory called `local_fabric` contained in your current project directory.

## Installing smart contract packages
{: #develop-vscode-installing-smart-contract-packages}

After connecting to an instance of Hyperledger Fabric, you can install and instantiate smart contract packages on the peers.

1. In the **Local Fabric Ops** pane, click **Install Smart Contract**
2. Select the peer you want to install the Smart contract on.
3. Select the package you wish to install.

## Instantiating smart contract packages
{: #develop-vscode-instantiating-smart-contract-packages}

To begin running on a channel, an installed smart contract package must first be instantiated.

1. In the **Local Fabric Ops** pane, click **Instantiate Smart Contract**
2. Select the installed Smart contract to instantiate.
3. Optional - Enter the name of the instantiate function in your smart contract.
4. Optional - Enter any arguments required by your instantiate function.

## Upgrading an instantiated smart contract
{: #develop-vscode-upgrading-instantiated-smart-contract}

After a smart contract has been installed on a peer and instantiated on a channel, it can be upgraded to deploy a newer version of the smart contract.

1. Ensure that the smart contract you wish to upgrade is instantiated.
2. Install the new version of the smart contract to a peer on the same network.
3. Right-click on the instantiated smart contract, and select **Upgrade Smart Contract**.
4. Optionally, choose a transaction to run when the new smart contract is instantiated.

## Enabling development mode on the preconfigured Hyperledger Fabric runtime
{: #develop-vscode-enabling-development-mode-Fabric}

In normal operation, a peer will create and maintain a chaincode container to run instantiated smart contracts. By switching to development mode, the peer allows the chaincode container to be run manually. Running the chaincode container manually, on the command line or in a terminal, assists with iterative development and debugging of smart contracts.

To enable development mode with the preconfigured Hyperledger Fabric runtime:

1. When the runtime is started; from the **Local Fabric Ops** pane expand the **Nodes** section.
2. Right click on the peer and select **Toggle Development Mode**.

Development mode must be enabled in order to use the debugging feature of the extension.

## Creating an identity
{: #develop-vscode-create-identity}

1. From the **Local Fabric Ops** pane; expand nodes, right click on a peer and select **Create identity**
2. Give the identity a name. An identity will the be registered, enrolled, and added to the `Local Wallet`

## Connecting to your own Hyperledger Fabric instance
{: #develop-vscode-connecting-to-own-Fabric-instance}

By using this extension, you can connect to a preconfigured local instance of Hyperledger Fabric, or you can connect to your own Hyperledger Fabric instance. If you choose to connect to your own Fabric instance, it must be Fabric 1.4.0 or later.

To connect to your own Hyperledger Fabric instance, complete the following steps:

1. Open the **{{site.data.keyword.blockchainfull_notm}} Platform** tab in Visual Studio Code.
2. In the **{{site.data.keyword.blockchainfull_notm}} Platform** pane, click **+**.
3. Enter a name for the connection. This name will be displayed in the **{{site.data.keyword.blockchainfull_notm}} Platform** pane.
4. Browse to the connection profile
5. Select either an existing file system [wallet ![External link icon](images/external_link.svg "External link icon")](https://hyperledger-fabric.readthedocs.io/en/release-1.4/developapps/wallet.html "Wallet"), or create a new file system wallet by entering the filepaths of the certificate and private key for your fabric identity.
6. Your connection should appear in the connections list underneath `local_fabric`. Click the connection name to connect.
7. If the connection has not been associated with a wallet select the wallet you want to use
8. Select the identity you want to use to connect

## Associating a wallet with a gateway
{: #develop-vscode-associate-wallet}

A wallet can be associated with one or more gateways.

1. From the **Fabric Gateways** pane right click on a gateway and select associate wallet
2. Select the wallet you want to associate

## Disassociating a wallet with a gateway
{: #develop-vscode-disassociate-wallet}

1. From the **Fabric Gateways** pane right click on a gateway and select disassociate wallet

## Disconnecting from a network
{: #develop-vscode-disconnecting}

When connected to a network, you can close the connection by clicking the disconnect icon in the upper-right of the Blockchain Connections pane.

## Editing or deleting a connection
{: #develop-vscode-editing-or-deleting-connection}

Connections can be edited or deleted in the connections pane.

### Editing a connection
{: #develop-vscode-editing-connection}

By editing a connection, you can change the filepath for the connection profile, the name given to the connection, and the filepaths to identity certificates and private keys.

To edit a connection:

1. From the extension, right-click on the connection to edit in the lower-left, this opens a contextual menu with the options to add an identity, edit the connection, or delete the connection.
2. Select **Edit connection**.
3. The **User Settings** page will open, with the connection details highlighted.
4. Make any changes, and save the settings page.

### Deleting a connection
{: #develop-vscode-deleting-connection}

Connections can be deleted by:

1. From the extension, right-click on the connection to edit in the lower-left, this opens a contextual menu with the options to add an identity, edit the connection, or delete the connection.
2. Select **Delete connection**.
3. A dialogue box appears to confirm connection deletion. Click **Yes**.

The connection will be deleted.

## Submitting transactions
{: #develop-vscode-submitting-transactions}

After a smart contract has been installed and instantiated, the transactions can be submitted from the connections pane in the {{site.data.keyword.blockchainfull_notm}} Platform extension panel.

To submit a transaction:

1. Ensure that your smart contract is installed and instantiated, and that you're connected to the network.
2. In the connections pane, expand the **Instantiated Smart Contracts**.
3. Expand the smart contract that contains the transaction to be submitted.
4. Right-click on the transaction to submit, and select **Submit Transaction**.
5. Enter any arguments required by the transaction, and press **Enter**.

## Testing an instantiated smart contract
{: #develop-vscode-testing-instantiated-smart-contract}

Tests for a smart contract can be generated after the smart contract has been instantiated. The tests can be generated as either JavaScript or TypeScript, and run or debugged.

To generate smart contract tests:

1. Ensure that the smart contract has been instantiated.
2. Under **Instantiated Smart Contracts**, right-click on the smart contract to generate tests for.
3. Select **Generate Smart Contract Tests**.
4. Now select the language for the test file, either **JavaScript** or **TypeScript**. The {{site.data.keyword.blockchainfull_notm}} Platform extension will now install required npm modules and build the test file.

After the test file has been built, the tests can be run by clicking the **Run Tests** button in the file.

## Debugging a smart contract using the preconfigured Hyperledger Fabric runtime
{: #develop-vscode-debugging}

The ability to debug a smart contract locally helps a smart contract developer to iterate on their smart contract functions, and fix bugs before attempting to invoke the functions after instantiation. Debugging a smart contract allows you to run through the smart contract transactions with breakpoints and output, ensuring the transactions work as intended. To debug your smart contract:

1. Ensure you are connected to the `local_fabric` connection that it is in development mode.
2. Open your smart contract project.
3. Open the debug view in Visual Studio Code using the left-hand navigation bar.
4. Select the Debug Smart Contract configuration by using the dropdown in the upper-left.
5. Package and install the smart contract by clicking the **play** button.
6. Add breakpoints to the smart contract by clicking on the relevant line numbers in your smart contract files.
7. On the debug tool bar click the **Blockchain** icon to instantiate the smart contract
8. On the debug tool bar click the **Blockchain** icon to submit or evaluate transactions.

To make changes to your smart contract while debugging, click the **restart** button after making the changes to your smart contract. Restarting debugging means you don't need to instantiate the contract again.

## Add a Wallet
{: #develop-vscode-add-a-wallet}

1. From the **Fabric Wallets** pane click the + icon
2. Choose either to specify an existing file system wallet or create a new wallet and add an identity
3. If using an existing wallet browse to the wallet
4. If creating a new wallet enter a name for the wallet, a name for the identity, and the MSPID 
5. Choose either to add a certificate and private key or select a gateway and provide an enrollment id and secret
6. If using a certificate and private key browse to the certificate and private key
7. If using an enrollment ID and secret, choose the gateway to enroll with and enter the enrollment ID and secret

## Add an identity to a wallet
{: #develop-vscode-add-a-identity-wallet}

1. From the **Fabric Wallets** pane right click on a wallet and select **Add Identity**
2. Provide a name for the identity and an MSPID.
3. Choose either to use a certificate and private key or an enrollment id and secret.
4. If using a certificate and private key browse to the certificate and private key
5. If using an enrollment ID and secret, choose the gateway to enroll with and enter the enrollment ID and secret

## Delete a wallet
{: #develop-vscode-delete-a-identity-wallet}

From the **Fabric Wallets** pane right click on the identity you want to delete and select **Delete Identity**

## Remove a wallet
{: #develop-vscode-remove-wallet}

1. From the **Fabric Wallets** pane right click on the wallet you want to remove and select **Remove Wallet**
2. Choose whether to delete the wallet from the file system or not

## Edit a wallet
{: #develop-vscode-edit-wallet}

1. From the **Fabric Wallets** pane right click on the wallet you want to edit and select **Edit Wallet**
2. Update User settings with the details you wish to update.
