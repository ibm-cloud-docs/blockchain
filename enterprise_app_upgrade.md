---

copyright:
  years: 2019, 2020
lastupdated: "2020-05-27"

keywords: IBM Blockchain Platform, blockchain

subcollection: blockchain

---

{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:note: .note}
{:important: .important}
{:java: data-hd-programlang="java"}
{:javascript: data-hd-programlang="javascript"}
{:tip: .tip}
{:pre: .pre}
{:external: target="_blank" .external}

Select **Java** or **Node** depending on the Fabric SDK that you use.
{:note: .note}

# Updating your applications
{: #enterprise-upgrade-applications}

Upgrading from Enterprise Plan to {{site.data.keyword.blockchainfull_notm}} Platform 2.0 has important implications for your applications. It is recommended that you update your applications before you start using the upgrade tool. Complete the following steps to update your applications:

1. [Prepare for breaking changes from the upgrade of your Enterprise Plan network to Fabric v1.4](#enterprise-upgrade-applications-one)
2. [Upgrade your version of Fabric SDK](#enterprise-upgrade-applications-two)
3. [Update your application to use service discovery](#enterprise-upgrade-applications-three)
4. [Download a new connection profile from {{site.data.keyword.blockchainfull_notm}} Platform 2.0](#enterprise-upgrade-applications-four)

You can use these steps to begin the upgrade process to the {{site.data.keyword.blockchainfull_notm}} Platform 2.0 without making large code changes or experiencing application downtime. For more information, review each step below. You need to review these steps before you start using the upgrade tool.

If your network uses an application that was build with Hyperledger Composer, you need to follow a different set of steps to upgrade to the new platform. For more information, see [Upgrading a network that uses Hyperledger Composer](/docs/blockchain/reference?topic=blockchain-enterprise-upgrade#enterprise-upgrade-composer).

## Step one: Prepare for the breaking changes from Fabric v1.4
{: #enterprise-upgrade-applications-one}

The peer-based EventHub has been removed in Fabric v1.4.3 and replaced with a new channel-based event service. If you are still using the old peer-based based service, you must update your applications to use channel events. Because you can use channel events on Fabric v1.1, you can update your application before your network is migrated. For more information, see [How to use the channel-based event service](https://hyperledger.github.io/fabric-sdk-node/release-1.4/tutorial-channel-events.html){: external} in the Node SDK documentation.
{: javascript}

You might need to update your application before your Enterprise Plan network is upgraded from Fabric v1.1 to Fabric v1.4.3. The peer-based EventHub has been removed in Fabric v1.4.3. If you are still using the old peer-based based service, you must update your applications to use the new channel-based event service. Because you can use channel events on Fabric v1.1, you can migrate your application at any time.
{: java}

## Step two: Upgrade your version of the Fabric SDK
{: #enterprise-upgrade-applications-two}

When your Enterprise Plan network is running on Fabric 1.4.3, you can upgrade to version 1.4.5 of the Fabric Node SDK. You need to upgrade the SDK version before you can update your application to use service discovery. You can go to the [Node Fabric SDK documentation](https://hyperledger.github.io/fabric-sdk-node/release-1.4/index.html) for more information.
{: javascript}

When your Enterprise Plan network is running on Fabric 1.4.3, you must upgrade your application to the 1.4.5 version of the Fabric Java SDK. You need to upgrade the SDK version before you can update your application to use service discovery. Version 1.4.5 is the minimum version that is required to run the sample code provided in this documentation. You can find the Maven definition for version 1.4.5 below:
{: java}

```
<dependency>
   <groupId>org.hyperledger.fabric-sdk-java</groupId>
   <artifactId>fabric-sdk-java</artifactId>
   <version>1.4.5</version>
</dependency>
```
{: codeblock}
{: java}

See the [Java Fabric SDK documentation](https://hyperledger.github.io/fabric-gateway-java/) for more information.
{: java}

## Step three: Update your applications to use service discovery
{: #enterprise-upgrade-applications-three}

After you upgrade the Fabric SDK, you can update your application to take advantage of the [Service Discovery Feature](https://hyperledger-fabric.readthedocs.io/en/release-1.4/discovery-overview.html){: external} of Hyperledger Fabric. Using service discovery makes it easier to upgrade to {{site.data.keyword.blockchainfull_notm}} Platform 2.0. As a result, it is recommended that you update your application before you start using the upgrade tool.
{: javascript}

After you upgrade the Fabric SDK to version 1.4.5, you can update your application to take advantage of the [Service Discovery Feature](https://hyperledger-fabric.readthedocs.io/en/release-1.4/discovery-overview.html){: external} of Hyperledger Fabric. Configuring service discovery makes it easier to upgrade to {{site.data.keyword.blockchainfull_notm}} Platform 2.0. As a result, it is recommended that you update your application before you start using the upgrade tool.
{: java}

Service discovery allows your applications to dynamically find the peer and ordering endpoints of your network. If you do not use service discovery, the endpoint information of peer and ordering nodes on your channel needs to be added manually to your connection profile or your application. You would need to edit your connection profile or update your application each time a node is added or removed from your network.

You can start using service discovery by updating your application code before the upgrade process. After you use the upgrade tool to create nodes on {{site.data.keyword.blockchainfull_notm}} Platform 2.0, you can download a connection profile from the new platform and import it into your application using the SDK. Your application will use service discovery to find all of the peers and ordering nodes on the {{site.data.keyword.blockchainfull_notm}} Platform 2.0 and Enterprise Plan. Because the connection profile provided by the {{site.data.keyword.blockchainfull_notm}} Platform 2.0 console assumes that your application uses service discovery, updating your application prepares your application for long-term use of the new platform.

You can use one of two ways to update your application:
- If you have the time to make thorough updates to your application, you can use the high-level SDK APIs provided by the new Fabric programming model. The new APIs are built to take advantage service discovery and reduce the number of steps that are required to submit a transaction. For more information, see [Update your application to use the new Fabric SDK programming model](#enterprise-upgrade-applications-new-apis).
- If you want to make minimal updates to your application before using the upgrade tool, you can use the low-level Fabric SDK APIs to use service discovery. For more information, see [Patch your applications to use service discovery with the low-level APIs](#enterprise-upgrade-applications-patch).

If you cannot update your applications to use service discovery, you need to [manually update your application or connection profile](#enterprise-upgrade-applications-manual) during the upgrade.

To configure your client application to use Service Discovery, when you start your gateway with the `gateway.connect()` call, you need to set the queryHandlerOptions to `strategy: DefaultQueryHandlerStrategies.MSPID_SCOPE_ROUND_ROBIN`. This configuration ensures that requests from the client application are distributed across available peers. See [DefaultQueryHandlerStrategies](https://hyperledger.github.io/fabric-sdk-node/release-1.4/module-fabric-network.html#.DefaultQueryHandlerStrategies__anchor) in the Node SDK documentation for more information.
{: tip}

### Option one: Update your application to use the new Fabric SDK programming model
{: #enterprise-upgrade-applications-new-apis}

Starting with Fabric 1.4, you can take advantage of a new, simplified Fabric SDK programing model. The new model introduces high-level APIs that take advantage of service discovery and reduce the number of steps that are required to submit a transaction. Updating your applications to use the new programming model makes it easier to use the new platform and is the recommended path for your applications in the long term.

To learn more about the new programming model, see the [Creating applications](/docs/blockchain/reference?topic=blockchain-ibp-console-app#ibp-console-app) tutorial or the [Developing applications topic](https://hyperledger-fabric.readthedocs.io/en/release-1.4/developapps/developing_applications.html){: external} in the Hyperledger Fabric documentation. If you need additional updating your applications to use the new programming model, engage [{{site.data.keyword.blockchainfull_notm}} services](https://www.ibm.com/blockchain/services){: external}.
{: javascript}

To learn more about the new programming model, see the [Developing applications topic](https://hyperledger-fabric.readthedocs.io/en/release-1.4/developapps/developing_applications.html){: external} in the Hyperledger Fabric documentation. For documentation on the new APIs, see the [Hyperledger Fabric Gateway SDK for Java](https://hyperledger.github.io/fabric-gateway-java/). If you need additional updating your applications to use the new programming model, engage [{{site.data.keyword.blockchainfull_notm}} services](https://www.ibm.com/blockchain/services){: external}.
{: java}

### Option two: Patch your applications to use service discovery with the low-level APIs
{: #enterprise-upgrade-applications-patch}

If you want to start using the new platform without large changes to your application code, you can update your applications to use service discovery while continuing to use the low-level APIs. If you are connecting to your Enterprise Plan network by using your connection profile, you need to make the following updates to your application:
- You need to create a new channel object with the SDK instead of loading a channel from your configuration file.
- You need to enable service discovery when you initialize the channel connection. You also need to add a peer from your organization to the channel. The SDK uses this peer to get information about your channel by using service discovery.

You can learn more about how to patch your application by going to the example below. If you are using the Node SDK, you can also learn more by going to the [How to use Service Discovery](https://hyperledger.github.io/fabric-sdk-node/release-1.4/tutorial-discovery.html) tutorial in the NodeSDK documentation.
{: javascript}

You can learn more about how to patch your application by going to the example below.
{: java}

#### Patching a sample application

You can use the following example to learn how to update an application to use service discovery. Because the sample does not include features of production code such as error handling or listening for events, this code should be used as an example only.

The example below can connect to an Enterprise Plan network by using a connection profile. The sample application loads the connection profile from your local file system and uses it to create an instance of the Fabric Client. The application then creates a channel object by using a channel that is defined in the connection profile. When the application initializes the channel connection, it connects to all of the peer and ordering nodes that are added to the channel in the connection profile.
{: javascript}

```javascript
const NodesdkClient = require('fabric-client');

// load the connection profile (location defined in ccpFile)
const buffer = fs.readFileSync(ccpFile);
const ccp = JSON.parse(buffer.toString());

// create client instance
const client = NodesdkClient.loadFromConfig(ccp);

// create a channel instance from the connection profile
const channel = client.getChannel('mychannel');

// initialize the channel connection
await channel.initialize();
```
{: codeblock}
{: javascript}

The example below can connect to an Enterprise Plan network by using a connection profile. The sample application creates an instance of the Fabric Client and uses the Client to parse a connection profile that is loaded from your local file system. The application can then create a channel instance from a channel that is defined in the connection profile. When the application initializes the channel connection, it connects to all of the peer and ordering nodes that are added to the channel in the connection profile.
{: java}

```java
// Create a client instance
HFClient client = HFClient.createNewInstance();

// load the connection profile
File configFile = new File(ccpFile);
NetworkConfig conf = NetworkConfig.fromJsonFile(configFile);

// create a channel instance from the connection profile
Channel channel = client.loadChannelFromConfig("mychannel", conf);

channel.initialize();
```
{: codeblock}
{: java}

You can update this sample code to use service discovery by replacing the last two lines of code. Edit the line that gets a channel from your connection profile:
{: javascript}

```javascript
// create a channel instance from the connection profile
const channel = client.getChannel('mychannel');
```
{: javascript}

Replace it with a line that creates a new channel object with a name that you provide:
{: javascript}

```javascript
const channel = client.newChannel('mychannel');
```
{: codeblock}
{: javascript}

You can then edit the line that initializes the channel connection without service discovery:
{: javascript}

```javascript
// initialize the channel connection
await channel.initialize();
```
{: javascript}

Replace this line with the block of the code below:
{: javascript}

```javascript
// get the list of peers in your organization
const orgPeers = client.getPeersForOrg();
// define the initialization options, specify the
// first peer in the list of your organization peers
const initOptions = {target: orgPeers[0], discover: true, asLocalhost: false};
// initialize the channel using the required options
await channel.initialize(initOptions);
```
{: codeblock}
{: javascript}
Instead of getting all of the peers and ordering nodes that are defined on the channel, the code above uses the connection profile to get one peer from your organization. When the channel connection is initialized, your peer is passed the channel and service discovery is enabled by setting `discover: true`. The SDK then uses service discovery to receive the list of peers and ordering nodes on the channel that need to endorse a transaction and commit it to the ledger. If you have multiple peers in your organization for high availability, you can edit the code block above to add additional peers to the channel. You need to call `channel.initialize` inside an asynchronous function because the call returns a promise.
{: javascript}

You can update this code sample to use service discovery. Edit the line that creates a channel from your connection profile:
{: java}

```java
Channel channel = client.loadChannelFromConfig("mychannel", conf);
```
{: java}

You can replace this line with a call to a new method that creates a channel with service discovery enabled. You will add this method to your application in the next step.
{: java}

```java
Channel channel = createChannel(client, conf, "mychannel");
```
{: codeblock}
{: java}

You can then add the method that creates the channel:
{: java}

```java
/*
 * create the named channel for use with a connection profile
 */
private static Channel createChannel(HFClient client, NetworkConfig networkConfig, String channelName) throws InvalidArgumentException {
    // create a Channel instance
    Channel channel = client.newChannel(channelName);

    // get a list of all the peers in the client organization
    List<String> peerNames = networkConfig.getClientOrganization().getPeerNames();

    // build a Peer instance of each of these peers with appropriate
    // properties and add it to the channel instance
    for (String name: peerNames) {
        String url = networkConfig.getPeerUrl(name);

        // retrieve any explicit properties defined for that peer
        Properties props = networkConfig.getPeerProperties(name);
        Peer peer = client.newPeer(name, url, props);

        // ensure the peer has all roles including service discovery
        PeerOptions peerOptions = PeerOptions.createPeerOptions()
            .setPeerRoles(EnumSet.allOf(PeerRole.class));
        channel.addPeer(peer, peerOptions);
    }
    return channel;
}
```
{: codeblock}
{: java}

Instead of getting all of the peers and ordering nodes on the channel, the method above uses the connection profile to get the list of peers from your organization. Service discovery is enabled by setting all of the roles for each peer and then adding them to the channel. The peers added to the channel can then receive updates by using service discovery. You can initialize the channel instance by using the same line of code that was in the original sample:
{: java}

```java
channel.initialize();
```
{: codeblock}
{: java}

The SDK can then use service discovery to receive the list of peers and ordering nodes on the channel that need to endorse a transaction and commit it to the ledger.
{: java}

## Step four: Download a new connection profile from {{site.data.keyword.blockchainfull_notm}} Platform 2.0
{: #enterprise-upgrade-applications-four}

After you update your application to take advantage of service discovery, you can get [started with the upgrade tool](/docs/blockchain/reference?topic=blockchain-enterprise-upgrade-tool#enterprise-upgrade-tool) and start creating nodes on the new platform. You can continue to use your Enterprise Plan connection profile and submit transactions to your Enterprise Plan network.

After all of the organizations in your network have created nodes on the new platform, and you have used the tool to migrate your chaincode, you can use the {{site.data.keyword.blockchainfull_notm}} Platform 2.0 console to download a new connection profile. The {{site.data.keyword.blockchainfull_notm}} Platform 2.0 connection profile can be downloaded from the **Organization MSP** tile on the **Organizations** panel. You can use the connection profile for all of your channels if you joined the same peers to each channel. For more information, see [Connect with SDK](/docs/blockchain/reference?topic=blockchain-ibp-console-smart-contracts#ibp-console-smart-contracts-connect-to-SDK-panel). Chaincode is referred to as smart contracts on the {{site.data.keyword.blockchainfull_notm}} Platform 2.0 console UI.

After you import the new connection profile, you need to use the Certificate Authority on the {{site.data.keyword.blockchainfull_notm}} Platform 2.0 to enroll a new identity for your application. Your application can then use service discovery to submit transactions to your nodes on the new platform and the nodes that remain on Enterprise Plan.

## If you cannot update your application
{: #enterprise-upgrade-applications-manual}

If you cannot update your application to use service discovery before the upgrade process, you need to manually update your application or edit your Enterprise Plan connection profile while you are using the upgrade tool. For each new peer or orderer node that you create on the {{site.data.keyword.blockchainfull_notm}} Platform 2.0, you need to add the node endpoint information to your application or connection profile. You can find a template for the connection profile used by Enterprise Plan in YAML format in the [How to use a common connection profile](https://hyperledger.github.io/fabric-sdk-node/release-1.4/tutorial-network-config.html) tutorial.

You can use the {{site.data.keyword.blockchainfull_notm}} Platform 2.0 console to find the endpoint URL and the TLS certificate for each node:
1. Log in to your {{site.data.keyword.blockchainfull_notm}} Platform 2.0 console.
2. Click the peer or orderer node in the **Node** tab.
3. On the peer or orderer page, click the **Settings** icon besides the peer or orderer name.
4. In the side panel, click **Export** to save the peer or orderer JSON file.
5. You can find the endpoint URL for your application in the `"api_url"` field of the JSON file. You can find the node TLS certificate in the `"pem"` field.

In order to add the certificate to the connection profile, the certificates need to be decoded from base64 into PEM format. You can decode the certs by running the following command on your local system:
```
export FLAG=$(if [ "$(uname -s)" == "Linux" ]; then echo "-w 0"; else echo "-b 0"; fi)
echo <base64_string> | base64 --decode $FLAG > <key>.pem
```
{:codeblock}
