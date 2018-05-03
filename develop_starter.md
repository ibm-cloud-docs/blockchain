---

copyright:
  years: 2017, 2018
lastupdated: "2018-5-3"

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

Ensure that you read the [About Starter Plan](./starter_plan.html) and [Getting started with Starter Plan](./get_start_starter_plan.html). Also, ensure that you have installed the [{{site.data.keyword.blockchainfull_notm}} Platform: Develop developer environment](./develop_install.html) and created an instance of the {{site.data.keyword.blockchainfull_notm}} Platform Starter Plan following the instructions in [Govern Starter Plan network](./get_start_starter_plan.html). Ensure you have Node v8.9 or higher and npm v5.x. You will need a business network (also known as .bna file) ready to deploy for this guide. If you have no business network (.bna) file, check the [Develop the network](./develop.html) guide to develop one. Lastly, you will need Composer-cli version 0.18.1. Uninstall your current version of the CLI:

`npm uninstall -g composer-cli`

and install version 0.18.1:

`npm install -g composer-cli@0.18.1`


## Step One: Retrieve admin secret

1. From the Starter Plan overview screen, click **Connection Profile** and then download. Rename this file to 'connection-profile.json'. 

2. Move this file to be in the same directory as your .bna file.

3. Inside the connection profile, go all the way down until you see 'registrar'. Inside 'registrar', under 'enrollId' there is an **enrollSecret** property. Retrieve the secret and save a copy of it.

![D8KBag](https://i.makeagif.com/media/4-12-2018/D8KBag.gif)



## Step Two: Creating a certificate authority card

The secret retrieved in the preceding step will be used to create a business network card for the certificate authority (CA). The CA card will then be imported and the card will be used to exchange the **enrollSecret** for valid certificates from the Starter Plan certificate authority.

1. Using the **enrollSecret** noted from step one, run the following command to create the CA business network card:

        composer card create -f ca.card -p connection-profile.json -u admin -s ${enrollSecret}

2. Import the card using the following command:

        composer card import -f ca.card -n ca

3. Now that the card is imported, it can be used to exchange the **enrollSecret** for valid certificates from the CA. Run the following command to request certificates from the certificate authority:

        composer identity request --card ca --path ./credentials

The `composer identity request` command creates a `credentials` directory that contains certificate `.pem` files.

## Step Three: Adding the certificates to the Starter Plan instance

The certificates must be added to the Starter Plan instance. For convenience, they can be added by using the {{site.data.keyword.blockchainfull_notm}} Platform UI. The certificates must be added, then the peers must be restarted, and then the certificates must be synced on the channel. The certificate needed is the `admin-pub.pem` file that was generated from the previous command, which is in the `credentials` directory.

1. Back in the blockchain service, click on the 'Members' tab, then to the right of 'Members', you should see 'Certificates'. Click on 'Certificates' and then on 'Add Certificate'. Go to your `credentials` directory, and copy and paste the contents of the `admin-pub.pem` file in the certificate box. Submit the certificate and restart the peers. Note: restarting the peers takes a minute.

![jlEb2y](https://i.makeagif.com/media/4-12-2018/jlEb2y.gif)

2. Next, the certificates must be synced on the channel. From our blockchain service, under ‘my network’ click on ‘Channels’ and then the three-dot button. Then click ‘Sync Certificate’.

![E-sVV5](https://i.makeagif.com/media/4-12-2018/E-sVV5.gif)

## Step Four: Creating an admin business network card

Now that the correct certificates have been synced with the peers, business network cards can be created which have the permissions to install the Hyperledger Composer runtime and start chaincode.

1. Create an admin card with the channel admin and peer admin roles by using the following command:

        composer card create -f adminCard.card -p connection-profile.json -u admin -c ./credentials/admin-pub.pem -k ./credentials/admin-priv.pem --role PeerAdmin --role ChannelAdmin

    This card will be used to deploy a business network to Starter Plan.

2. Import the card created in the previous step using the following command:

        composer card import -f adminCard.card -n adminCard

## Step Five: Installing and starting the business network

Next, the card created in the previous step can be used to install and start a business network. For this guide, we'll install the vehicle manufacturing network sample. The command to start a business network will also create a card. For Starter Plan, this card must be deleted, the example command given names this card `delete_me.card` so it can be easily distinguished.

1. Install the Hyperledger Composer runtime with the following command:

        composer runtime install -c adminCard -n vehicle-manufacture-network

2. Start the business network with the command below. If you get an error, wait a minute and try again.

        composer network start -c adminCard -a vehicle-manufacture-network.bna -A admin -C ./credentials/admin-pub.pem -f delete_me.card

3. Delete the business network card called `delete_me.card`.

4. Create a new business network card and reference the certificates that are retrieved earlier with the following command:

        composer card create -n vehicle-manufacture-network -p connection-profile.json -u admin -c ./credentials/admin-pub.pem -k ./credentials/admin-priv.pem

5. Import the business network card with the following command:

        composer card import -f ./admin@vehicle-manufacture-network.card

The business network is now deployed to the Starter Plan instance. You can now view your chaincode logs from your 
installed business network. Back in your blockchain instance, you can go into channels, click on 'defaultchannel' or what
 you named the channel
and then on the small '>' symbol. Then, under 'Actions', click on the three-dot symbol
and 'view details'. This is your chaincode log. You will be able to see all relevant details for your business network here. Your network is now instantiated!

![fN-Yuj](https://i.makeagif.com/media/4-13-2018/fN-Yuj.gif)

## Step Six: Testing the business network

This section will go over how to test your network with a local version of the composer-rest-server. We need composer-rest-server v0.18.1. Uninstall your current version of the composer-rest-server:

`npm uninstall -g composer-rest-server`

and install version 0.18.1:

`npm install -g composer-rest-server@0.18.1`
  
1. Start up the server with the command below. Make sure you are in the same directory as your `connection.profile.json` file. Note: this guide will show the logs from the vehicle-manufacture-network, but you can use any .bna file you want. The process is the same. 

        composer-rest-server -c admin@vehicle-manufacture-network -n never -w true

2. In your browser, go to http://localhost:3000/explorer

3. Click on 'Person' then POST and in the data field, enter:

        {

"$class": "org.acme.vehicle_network.Person",

"username": "username",

"email": "email@gmail.com"

}


4. Under the data field, click the 'Try it out!' button.

5. In your response body, you should see the data that you entered above. In your blockchain service, go back into your channel logs, as shown in Step 5 part 5, and you should see that your logs reflect the response body of the POST request.

![aK59y7](https://i.makeagif.com/media/4-30-2018/aK59y7.gif)

Nice job! You successfully ran a local version of your composer-rest-server and submitted transactions that have been logged to the IBM Blockchain Starter Plan. 

