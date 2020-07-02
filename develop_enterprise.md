---

copyright:
  years: 2017, 2020
lastupdated: "2019-06-18"

keywords: business network, Enterprise Plan, developer environment, certificate authority card, admin business network card, BNA, business network archive

subcollection: blockchain

---

{:external: target="_blank" .external}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:gif: data-image-type='gif'}

# Deploying business networks to Enterprise Plan
{: #deploying-a-business-network}

{{site.data.keyword.IBM}} does not provide support for networks that use Hyperledger Composer in production, including the Composer CLI, JavaScript APIs, REST server, and Web Playground.
{:note}

Business networks can be developed and deployed to Enterprise Plan networks by using the {{site.data.keyword.blockchainfull_notm}} Platform developer environment and Hyperledger Composer developer toolset. By using the developer environment, you can quickly model and test blockchain business networks and deploy them to an Enterprise Plan network of the {{site.data.keyword.blockchainfull_notm}} Platform.
{:shortdesc}

{{site.data.keyword.blockchainfull_notm}} Platform's developer tools help you create a **Business Network Definition** that can then be packaged up into a business network archive (`.bna`). The developer environment allows you to deploy `.bna` files to a local or cloud {{site.data.keyword.blockchain}} for development and sharing.

This tutorial deals with the next step of a business network lifecycle, that is, to activate your business network by deploying the `.bna` to {{site.data.keyword.blockchainfull_notm}} Platform Enterprise Plan.

## Before you begin
{: #deploying-a-business-network-enterprise-plan-before-begin}

Ensure that you have installed the {{site.data.keyword.blockchainfull_notm}} developer environment and are comfortable with developing and deploying business networks. Guidance on writing business networks is available in the [Hyperledger Composer documentation](https://hyperledger.github.io/composer/latest/business-network/business-network-index).

You need access to an Enterprise Plan instance of {{site.data.keyword.blockchainfull_notm}} Platform, and to have created your peers beforehand. For more information about {{site.data.keyword.blockchainfull_notm}} Platform Enterprise Plan, see the [Enterprise Plan overview](/docs/blockchain?topic=blockchain-enterprise-plan-about#enterprise-plan-about).

## Step one: Create a connection profile for {{site.data.keyword.blockchainfull_notm}} Platform
{: #deploying-a-business-network-create-connection-profile}

1. Create a directory to store your connection details, for example:

    ```
    /Users/myUserId/.composer-connection-profiles/bmx-hlfv11
    ```
    {:codeblock}

    Each connection profile should contain a `connection.json` file. Create a new directory under the `.composer-connection-profiles`, in this instance `bmx-hlfv11`. This will be the name of the profile that you will use when working with Hyperledger Composer and {{site.data.keyword.blockchainfull_notm}} Platform.

    ```
    mkdir -p ~/.composer-connection-profiles/bmx-hlfv11
    cd ~/.composer-connection-profiles/bmx-hlfv11
    ```
    {:codeblock}

    You should now have the following directory structure:

    ```
    /Users/myUserId/.composer-connection-profiles/bmx-hlfv11
    ```

## Step two: Retrieve your connection profile
{: #deploying-a-business-network-retrieve-connection-profile}

1. From your {{site.data.keyword.blockchainfull_notm}} Platform dashboard, select **Overview**, then **Connection Profile** and the **Download** button to retrieve your connection profile.

2. Save the connection profile in the directory structure created in the preceding step. Name it **connection.json**.

## Step three: Add channel information
{: #deploying-a-business-network-add-channel-information}

1. Add the channel value in the `connection.json` to match the name of the channel that you plan to create and deploy your business network to. In the example connection profile template provided, the channels element is as follows: `"channels": {},`.

## Step Four: Prepare your peers
{: #deploying-a-business-network-prepare-peers}

This step **MUST** be performed before you create the channel to deploy a business network to. If this step is performed after channel creation, a deployed business network **might not start**.
{:note}

In the Connection Profile document under **certificateAuthorities** is an attribute **registrar** that contains attributes for **enrollId** and **enrollSecret** in the following format:

  ```
        "registrar": [
            {
                "enrollId": "admin",
                "enrollSecret": "PA55W0RD12"
            }
        ],
  ```

1. Create a business network card for the CA by using the following command:

    ```
    composer card create -f ca.card -p bmx-hlfv11 -u admin -s PA55W0RD12
    ```
    {:codeblock}

  Ensure that you're running the command in the same directory as your connection profile.

2. Import the business network card by using the following command:

    ```
    composer card import -f ca.card -c ca
    ```
    {:codeblock}

3. After the card has been imported, it can be used to acquire the certificates from the Certificate Authority using the following command:

    ```
    composer identity request --card ca --path ./credentials -u admin -s PA55W0RD12
    ```
    {:codeblock}

    The `composer identity request` command creates creates a credentials directory that contains the relevant certificate files.

4. Select **Members** from the navigation menu, then select the **Certificates** menu option and click the **Add Certificate** button.

5. Enter a unique name for this certificate in the **Name** field, this name cannot include dashes or hyphens.

6. Open the `admin-pub.pem` file created earlier and copy the contents of this file into the **Key** field and press the **Submit** button.

7. Use the user interface to stop the peers, then start the peers.

8. The certificate should now appear in the list of certificates.

## Step five: Create your channel
{: #deploying-a-business-network-create-your-channel}

1. Select **Channels** from the navigation menu on the left panel and click the **New Channel** button.

2. Enter a Channel Name and optional description and click **Next**. Note that the Channel Name must match the name that you specify in your connection profile for the channel attribute.

3. Give permissions as required and click **Next**.

4. Select the policy of the number of operators that need to accept channel updates and submit the request.

5. You should now be taken to the **Notifications** section where there should be a new request to review. Click the **Review Request** button.

6. Click the **Accept** button and you will be taken back to the **Notifications** section where you can see that the request can now be submitted. Click the **Submit Request** button to open the submission dialog and then click the **Submit** button. The new channel has been created.

7. Select **Channels** from the navigation menu. The new channel in the list of channels and should have “No peers added yet”. Click the actions menu next to it and select **Join Peers**. Check the check boxes next to the Peers you want to add and press **Add Selected**.

## Step six: Create a new identity to administer your business network
{: #deploying-a-business-network-add-new-identity-enterprise-plan}

Create business network card by using the requested certificates. This business network card will have the authority to install chaincode onto the peers that have your uploaded public certificate and will be an issuer for the certificate authorities.

1. To create the card, run the following command:

    ```
    composer card create -f adminCard.card -p bmx-hlfv11 -u admin -c ./credentials/admin-pub.pem -k ./credentials/admin-priv.pem --role PeerAdmin --role ChannelAdmin
    ```
    {:codeblock}

    Where `bmx-hlfv11` is the name of the profile that you previously created.

2. After creating the card, import it by using the following command:

    ```
    composer card import -f adminCard.card -c adminCard
    ```
    {:codeblock}

    Now we are ready to deploy your `.bna` file to the {{site.data.keyword.blockchainfull_notm}} Platform.


## Step seven: Deploy the business network
{: #deploying-a-business-network-deploy-business-network-enterprise-plan}

Now you can deploy your `.bna` file to the {{site.data.keyword.blockchainfull_notm}} Platform.

1. To use the card that is created in the previous step, you need to first install and then start the business network. Install the business network by using the following command:

   ```
   composer network install -c adminCard -a myNetwork.bna
   ```
   {:codeblock}

2. After you install the business network, start the business network by using the following command:

    ```
    composer network start -c adminCard -n myNetwork -V networkVersion -A admin -C ./credentials/admin-pub.pem -f delete_me.card
    ```
    {:codeblock}

3. To check that the business network has been deployed correctly, create an identity and associated business network card that can be used to ping the network.

    ```
    composer card create -f admin.card -p bmx-hlfv11 -u admin -n myNetwork -c ./credentials/admin-pub.pem -k ./credentials/admin-priv.pem
    ```
    {:codeblock}

4. Import the business network card by using the following command:

    ```
    composer card import -f admin.card
    ```
    {:codeblock}

5. Ping the network to check the network is running:

    ```
    composer network ping -c admin@myNetwork
    ```
    {:codeblock}
