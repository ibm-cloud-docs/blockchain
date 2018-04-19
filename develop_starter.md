---

copyright:
  years: 2017, 2018
lastupdated: "2018-3-16"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# Deploying a business network on Starter Plan
{: #deploying-a-business-networks-on-starter-plan}

Business networks can be developed and deployed to a Starter Plan environment by using the {{site.data.keyword.blockchainfull}} Platform: Develop developer environment and Hyperledger Composer developer toolset.

By using the developer environment, you can quickly model and test {{site.data.keyword.blockchain}} business networks and deploy them to an instance of the {{site.data.keyword.blockchainfull_notm}} Platform.

## Before you begin

Ensure that you read the [About Starter Plan](./starter_plan.html) and [Getting started with Starter Plan](./get_start_starter_plan.html). Also ensure that you have installed the [{{site.data.keyword.blockchainfull_notm}} Platform: Develop developer environment](./develop_install.html) and created an instance of the {{site.data.keyword.blockchainfull_notm}} Platform Starter Plan following the instructions in [Govern Starter Plan network](./get_start_starter_plan.html).


## Step One: Retrieve admin secret

1. From the Starter Plan overview screen, click **Connection Profile**.

2. Inside the connection profile is an **admin secret** property. Retrieve the secret and save a copy of it.

## Step Two: Creating a certificate authority card

The secret retrieved in the preceding step will be used to create a business network card for the certificate authority (CA). The CA card will then be imported and the card will be used to exchange the **admin secret** for valid certificates from the Starter Plan certificate authority.

1. Using the secret noted from step one, run the following command to create the CA business network card:

        composer card create -f ca.card -p ./config/connection-profile.json -u admin -s ${SECRET}

2. Import the card using the following command:

        composer card import -f ca.card -c ca

3. Now that the card is imported, it can be used to exchange the **admin secret** for valid certificates from the CA. Run the following command to request certificates from the certificate authority:

        composer identity request --card ca --path ./credentials

The `composer identity request` command creates a `credentials` directory that contains certificate `.pem` files.

## Step Three: Adding the certificates to the Starter Plan instance

The certificates must be added to the Starter Plan instance. For convenience, they can be added by using the {{site.data.keyword.blockchainfull_notm}} Platform API. The certificates must be added, then the peers must be stopped and restarted, and the certificates synced. The data that is required in the variables of the API calls can be retrieved from the **Connection Profile** in the Starter Plan Network Monitor.

1. Add the certificate by using the following API call:

        curl -X POST --header 'Content-Type: application/json' --header 'Accept: application/json' --basic --user ${USERID}:${PASSWORD} --data-binary <body> ${API_URL}/api/v1/networks/${NETWORKID}/certificates

    In the above example, the body is left blank, the following code is an example of the body of the API request:

        {
        "msp_id": "ExamplePeerOrg1",
        "adminCertName": "exampleAdminCert1",
        "adminCertificate": "-----BEGIN CERTIFICATE-----\nMIIBkzCCATqgAwIB...",
        "peer_names": [
          "example-fabric-peer-1234a"
        ],
        "SKIP_CACHE": true
        }

2. Stop the peers by using the following API call:

        curl -X POST --header 'Content-Type: application/json' --header 'Accept: application/json' --basic --user ${USERID}:${PASSWORD} --data-binary '{}' ${API_URL}/api/v1/networks/${NETWORKID}/nodes/${PEER}/stop

3. Restart the peers by using the following API call:

        curl -X POST --header 'Content-Type: application/json' --header 'Accept: application/json' --basic --user ${USERID}:${PASSWORD} --data-binary '{}' ${API_URL}/api/v1/networks/${NETWORKID}/nodes/${PEER}/start

4. Synchronize the certificates by using the following API call:

        curl -X GET --header 'Content-Type: application/json' --header 'Accept: application/json' --basic --user ${USERID}:${PASSWORD} ${API_URL}/api/v1/networks/${NETWORKID}/nodes/status

## Step Four: Creating an admin business network card

Now that the correct certificates have been synced with the peers, business network cards can be created which have the permissions to install the Hyperledger Composer runtime and start chaincode.

1. Create an admin card with the channel admin and peer admin roles by using the following command:

        composer card create -f adminCard.card -p ./config/connection-profile.json -u admin -c ./credentials/admin-pub.pem -k ./credentials/admin-priv.pem --role PeerAdmin --role ChannelAdmin

    This card will be used to deploy a business network to Starter Plan.

2. Import the card created in the previous step using the following command:

        composer card import -f adminCard.card -n adminCard

## Step Five: Installing and starting the business network

Next, the card created in the previous step can be used to install and start a business network. For this guide, we'll install the vehicle manufacturing network sample. The command to start a business network will also create a card. For Starter Plan, this card must be deleted, the example command given names this card `delete_me.card` so it can be easily distinguished.

1. Install the Hyperledger Composer runtime with the following command:

        composer runtime install -c adminCard -n vehicle-manufacture-network

2. Start the business network with the following command:

        composer network start -c adminCard -a vehicle-manufacture-network.bna -A admin -C ./credentials/admin-pub.pem -f delete_me.card

3. Delete the business network card called `delete_me.card`.

4. Create a new business network card and reference the certificates that are retrieved earlier with the following command"

        composer card create -n vehicle-manufacture-network -p ./config/connection-profile.json -u admin -c ./credentials/admin-pub.pem -k ./credentials/admin-priv.pem

5. Import the business network card with the following command:

        composer card import -f ./admin@vehicle-manufacture-network.card

The business network is now deployed to the Starter Plan instance.
