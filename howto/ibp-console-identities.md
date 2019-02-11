---

copyright:
  years: 2019
lastupdated: "2019-02-08"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:important: .important}
{:note: .note}
{:pre: .pre}

# Creating and managing identities
{: #ibp-console-identities}

***[Is this page helpful? Tell us.](https://www.surveygizmo.com/s3/4501493/IBM-Blockchain-Documentation)***

The nodes of the {{site.data.keyword.blockchainfull_notm}} Platform are based on Hyperledger Fabric and builds permissioned blockchain networks. This means that all participants of the blockchain consortium need to have identities that are continuously verified by Public Key Infrastructure. The {{site.data.keyword.blockchainfull_notm}} Platform console allows you to create these identities by your organization's Certificate Authorities (CAs). You can then use the console wallet to store these identities and use them to operate your network.

## Managing Certificate Authorities
{: #ibp-console-identities-manage-ca}

A CA is similar to a publicly trusted notary that acts as an anchor of trust among multiple parties, with each organization in a consortium maintaining their own CA. Your CA creates the identities that belong to your organization and issue each identity a public and private key. These keys are what allow all of your nodes and applications to sign and verify their actions. For more details about how CAs are used to establish identity, visit [the identity topic ![External link icon](../images/external_link.svg "External link icon")](https://hyperledger-fabric.readthedocs.io/en/release-1.4/identity/identity.html "identity") in the Hyperledger Fabric documentation.

When you create a CA by using the {{site.data.keyword.blockchainfull_notm}} Platform console, two CAs are created with the same endpoint url: a root CA and a TLS CA. You can view both of these CAs under the same display name in the **Nodes** page of your console. The root CA provides keys to your nodes and applications. The TLS CA provides certificates used to protect the communication within your network. To learn more about TLS on your network, see [Using your TLS CA](/docs/services/blockchain/howto/ibp-console-identities.html#ibp-console-identities-tlsca).

When you create your nodes, you need to use your root CA to create the following identities that are required to deploy, operate, and interact with your network:
- **CA admin:** Your CA contains a default CA admin identity. This identity has an enroll ID and secret, which are analagous to a username and password, that you specify during the deployment of your CA. You can use this admin username and password to operate your CA and create new identities. If you created a CA by using the console, the CA admin of your root CA is also the admin of the TLS CA, unless you decide to create a separate TLS CA admin.
- **Peers or orderers:** You need to register an identity for each of the peers and orderers that belong to your organization. Your nodes will then use these identities during deployment to generate the keys they need to participate in the network. For security, create a unique enroll ID and secret for each node you deploy.
- **Peer or orderer admins:** {{site.data.keyword.blockchainfull_notm}} Platform nodes are deployed with the public keys of component administrators identities inside of them. These certificates allow the admins to operate the component from a remote client or by using the console.
- **Org admins:** When you join a consortium hosted by an ordering service, you provide the public keys of identities that will become the administrators for your organization. You can use these identities to create or edit channels.
- **Applications:** Your applications need to sign their transactions before submitting them to be validated by the network. You need to create identities you can use to sign transactions from your client applications.

You can use the console to create these identities by using the [registration process](/docs/services/blockchain/howto/ibp-console-identities.html#ibp-console-identities-register). After you register your admin identities, you need to issue each identity a public private key, provide the public key to your organization MSP definition, and add the identity to your console wallet. You can complete these steps for one admin identity when you [create your organization MSP](/docs/services/blockchain/howto/ibp-console-organizations.html#console-organizations-create-msp). You can use separate identities as org admins or node admins, or you can use one identity to do both tasks. The [Building a network tutorial](/docs/services/blockchain/howto/ibp-console-build-network.html#ibp-console-build-network) uses one identity to be an admin for each organization created in the tutorial.

## Setting a CA identity
{: #ibp-console-identities-ca-identity}

Before working with identities, you need to set the identity of the CA admin, either by using the admin identity created during the creation of the CA or by establishing a new one. Open the CA on the **Nodes** tab. The enroll ID of the currently active identity is visible next to the CA name at the top of the panel. You can use that admin identity to create other identities by using the **Register** button to register identities that will become org admins and node identities, or to use the **Enroll** button to generate an identity and export it to the wallet.

To set an identity as a CA admin, click the **Settings** icon and then click **Set Identity** in the slider. You can specify an identity that exists in the wallet using the **Existing identity** tab. Alternatively, you can use the **New identity** tab to either paste certificates for the admin, or to upload a JSON file containing the certificates.

Not all identities have the ability to register new users. For more information, including how to establish an additional CA admin, see [Creating new CA admins](/docs/services/blockchain/howto/ibp-console-identities.html#ibp-console-identities-ca-admin).
{: note}

## Registering identities
{: #ibp-console-identities-register}

The first step in creating an identity is known as **registration**. During registration, an enroll ID and secret is created which can then be used by a node or an org admin to **enroll** this identity by generating a public and private key.

You need to enter the following information when you register a new identity with your CA.

- **Enroll ID** and **Enroll Secret**: This identity has an ID and secret, which are analagous to a username and password, that you specify during the deployment of your CA. You can use this admin ID and secret to create certificates with this identity by using this console or the Fabric CA client.
- **Type**: When the CA was deployed, the administrator specified the types of identities issued by the CA. Common examples of identity types would include peer, orderer, and client (applications). Select the identity type for this user from the list of available types.
- **Affiliation**: The part of your organization that you want to affiliate with this user. For example, this could be a department or unit that operates an application or a peer. It is possible to limit a particular CA admin to only be able to register users within its own department with an organization.
- **Maximum Enrollments**: Optionally, enter the number of times that you can enroll or generate certificates by using this identity. Specifying a limited number of enrollments helps protect the security of your nodes and applications. It defaults to unlimited enrollments.
- **Attributes**: Optionally, you can specify any [attribute-based access control ![External link icon](../images/external_link.svg "External link icon")](https://hyperledger-fabric-ca.readthedocs.io/en/release-1.4/users-guide.html#attribute-based-access-control "Attribute-Based Access Control") attributes for the user. For example, you can use this section to [create another CA admin](/docs/services/blockchain/howto/ibp-console-identities.html#ibp-console-identities-ca-identity) with authority to register and enroll new identities. You can see a full list of available Fabric CA attributes in the [Registering a new identity ![External link icon](../images/external_link.svg "External link icon")](https://hyperledger-fabric-ca.readthedocs.io/en/release-1.4/users-guide.html#registering-a-new-identity "Registering a new identity") section of the Fabric CA users guide.

Click the **Register user** button on the CA overview panel to create a new user. Be sure that you have [set the identity](/docs/services/blockchain/howto/ibp-console-identities.html#ibp-console-identities-ca-identity)
by using an identity that has the ability to register new users before attempting this task. In general this is your `admin` user. If the button is gray, this implies that you have either not set an identity, or that the identity cannot create new identities.  

Clicking **Register user** will open a series of side panels:
1. On the first side panel, enter the **Enroll ID** and **Enroll Secret** of the new identity. **Save these values**, as they are not stored by the console.
2. On the second side panel, select the identity **Type**. The drop-down list contains the list of types that this CA supports. Then, select the **Affiliation** that the new identity will belong to. You can choose an existing affiliation from the drop-down list or type in a new one.
3. On the third side panel, enter the **Maximum Enrollments** allowed for this identity. If not specified, the value defaults to unlimited enrollments.
4. On the last side panel, add the **Attributes** of the identity you are creating.

After you click **Register**, the new identity will be added to the list of **Authenticated users** that have been created by your CA. The identities are listed by their **Enroll ID**, along with their **Type** and **Affiliation**. Clicking on an identity in the table will open a side panel that allows you to view the number of **Maximum Enrollments** and **Attributes** that were created during registration.

### Creating new CA admins
{: #ibp-console-identities-ca-admin}

By default, only the CA admin created during deployment has the ability to register new identities. You can also create other identities with the ability the register new users by using the **Attributes** panel of the registration process.

On Side panel 4, click the **Add Attribute** button. Provide an **attribute name** of `hf.Registrar.Roles`. Enter an **attribute value** of `*`. You can also use this panel to create an identity that can only register certain types of identity types, such as clients or peers, or within a certain affiliation. You can also create an identity that has the ability to revoke an identity and all the certificates that the identity has been issued. You can see a full list of the attributes in the [Registering a new identity ![External link icon](../images/external_link.svg "External link icon")](https://hyperledger-fabric-ca.readthedocs.io/en/release-1.4/users-guide.html#registering-a-new-identity "Registering a new identity") section of the Fabric CA users guide.

## Enrolling an identity
{: #ibp-console-identities-enroll}

You can generate the public and private key for each identity that has been registered with your CA. You can generate keys for one of your admin identities when you [create your organization MSP](/docs/services/blockchain/howto/ibp-console-organizations.html#console-organizations-create-msp), and provide additional certificates to the MSP for other admins that have already been enrolled using your CA.

Be sure that you have [set the identity](/docs/services/blockchain/howto/ibp-console-identities.html#ibp-console-identities-ca-identity) that the CA uses to create the keys. After setting the identity, you can click the **Enroll Identity** button, and displays the keys and display them in a side panel in base64 format.
  - The public key is displayed in the **Certificate** field. This certificate is also referred to as your enrollment certificate, signing certificate, or signCert.
  - You can find the corresponding private key in the **Private Key** field.

Each key pair created by clicking **Enroll Identity** is only generated once and not stored by the console or your browser. Clicking the **Enroll Identity** button will also be counted against the maximum number of enrollments that you have set for the CA admin. After enrollment, you should store the key pair by downloading the identity to your local file system or adding it to your console wallet. Enter a new name for this public and private key pair into the **Name** field in order to retrieve them.
  - Click **Export Identity** to download these certificates to your local file system in JSON format.
  - Click **Add Identity to Wallet** to add these certificates to the console wallet. You can then find the name and keys of this identity in a new tile in your [wallet tab](/docs/services/blockchain/howto/ibp-console-identities.html#ibp-console-identities-wallet)

You can also use the Fabric CA client or Fabric SDKs to enroll the identities you have created in the console. The console provides you with all of the information that will need to compete these steps. Ensure you have saved the **Enroll ID** and **Enroll Secret** that you specified during registration.

## Using your TLS CA
{: #ibp-console-identities-tlsca}

The communication within your network is secured by TLS certificates. TLS encrypts the communication between your nodes and between your nodes and your applications. Using TLS prevents attackers from disrupting the communication between your nodes or reading the transactions submitted from your applications. The keys and certificates used for TLS are different than the certificates used to sign and validate your transactions and are issued by a separate Certificate Authority.

Each CA created by the {{site.data.keyword.blockchainfull_notm}} Platform console contains a root CA and a TLS CA. You can view both of these CAs under the same display name in the nodes tab of your console. Click on your CA overview panel and then click **TLS Certificate Authority** to operate your TLS CA. Your TLS CA has the [same registration](/docs/services/blockchain/howto/ibp-console-identities.html#ibp-console-identities-register) and [enrollment process](/docs/services/blockchain/howto/ibp-console-identities.html#ibp-console-identities-enroll) as your root CA. Both CAs are deployed with the same CA admin ID and secret.

Each peer or orderer node that you deploy needs to generate a public TLS certificate. When creating a node using the console, you are asked to provide an enroll ID and secret of an identity registered with your TLS CA. The [Build your network tutorial](/docs/services/blockchain/howto/ibp-console-build-network.html#ibp-console-build-network) uses the CA admin identity for convenience. However, it is a best practice to register a unique identity with your TLS CA for each node. The node will use this identity to generate its TLS certificate during deployment. This certificate will be required by any application that needs to communicate with the orderer or peer. You can find the TLS certificate of a node by navigating to the node overview panel and clicking Settings. You can also find the TLS Certs of your peers and orderers in the connection profile that can be downloaded [from the smart contracts tab](/docs/services/blockchain/howto/ibp-console-smart-contracts.html#ibp-console-smart-contracts-connect-to-SDK).

When creating a peer or orderer using your console, you can also use the TLS CA to specify an additional domain name for each node. Enter the new domain name in the **TLS CSR hostname** field when deploying your orderer or peer. This hostname will be added to the list of common names in the TLS certificate issued to your node.


## Certificate expiration
{: #ibp-console-identities-expiration}

Certificates generated using {{site.data.keyword.blockchainfull_notm}} Platform 2.0 CAs will expire after one year. The expiration period is the same for certificates that are generated by using the Fabric SDKs, the Fabric CA client, or by using the console. If the certificates expire, your applications can no longer interact with your network and you will need to re-enroll to generate new certificates. If you have hit the enrollment limit of a user, you can register a new user and then enroll.

You can use your command line to check your certificates expiration date. First you will need convert certificates that are in base64 into PEM format by running the following command on your local machine:

```
export FLAG=$(if [ "$(uname -s)" == "Linux" ]; then echo "-w 0"; else echo "-b 0"; fi)
cat $HOME/<path-to-certificate>/cert.pem | base64 $FLAG
```
{:codeblock}

Run the following command to display the PEM encoded certificate in a human-readable form:
```
openssl x509 -in <certificate file> -text
```
{:codeblock}

The certificate will look similar to the example below:

```
Certificate:
Data:
    Version: 3 (0x2)
    Serial Number:
        20:3d:3e:c5:31:4f:85:7a:30:9f:b5:67:47:3d:b0:10:70:80:f6:18
Signature Algorithm: ecdsa-with-SHA256
    Issuer: C=US, ST=North Carolina, O=Hyperledger, OU=Fabric, CN=fabric-ca-server-org1CA
    Validity
        Not Before: Nov 28 19:18:00 2018 GMT
        Not After : Nov 28 19:23:00 2019 GMT
    Subject: C=US, ST=North Carolina, O=Hyperledger, OU=peer, OU=org1, CN=1peeradmin
    ...
    ...
```

You can find the expiration date in the **Validity** section and follows `Not After:` In this example, the certificate will expire on November 28, 2019.

## Storing identities in your console wallet
{: #ibp-console-identities-wallet}

The wallet stores the identities and keys that the {{site.data.keyword.blockchainfull_notm}} Platform console uses to operate the nodes of your network. You need to add your peer, orderer, and organization admins to this wallet before you can use the console to work with channels and smart contracts. You can also use the wallet to conveniently store the identities that will be used by your applications. You can use the wallet to export them at any time. Use the left navigation to browse to the wallet overview panel. You can add, update, and export identities from this wallet by using the overview screen.

The wallet is a component of the console and not a separate service. It stores your keys in the local storage of the browser that you use to access the console instead of your local file system. If you access your console from a different browser, or start a private browsing session, you will not be able to access the identities stored in the wallet. **It is recommended that you export your admin identities from the console and store them in a safe place**.
{:important}

### Adding identities
{: #ibp-console-identities-add}

You can add an admin identity to your wallet when you [create your organization MSP](/docs/services/blockchain/howto/ibp-console-organizations.html#console-organizations-create-msp). A CA managed by the console can also add an identity to your wallet during the [enrollment process](/docs/services/blockchain/howto/ibp-console-identities.html#ibp-console-identities-enroll).

You can use the **Add identity** button on the overview screen to import an identity directly into the wallet. Clicking this button will open up a side panel that allows you to add an identity's public and private key pair.
- **Name:** Enter an identity name that is used for your reference only.
- **Certificate:** Paste in the identity's public key which you generated by using your CA.
- **Private Key:** Paste in the identity's private key which you generated by using your CA.


You can also add an identity by uploading a JSON file in the format below. You can also use the **Upload JSON** button to upload multiple identities at once.

```
{
    "name": "peerAdminCerts",
    "private_key": "LS0tLS1CRUdJTiBQUklWQVRFIEtFWS0tLS0tDQpNSUdIQWdFQU1CTUdCeXFHU000OUFnRUdDQ3FHU000OUF3RUhCRzB3YXdJQkFRUWdIVk5Kc0tHYnl6eTVZWEQ2DQovaEhKOVZlR0QrZVhmenpxUi9PQWVZYnY5UWloUkFOQ0FBUmZ1WlB1OTRzNnJQSEVCMjJlWFhpSWozd0lKYkg4DQpRYVpaRkJlNFp5SnU5UllHbkxQWUdLRnhETkRVMUZkU1NxUGNLcnczUXpxT3hXNU1NZDVWbEtVdw0KLS0tLS1FTkQgUFJJVkFURSBLRVktLS0tLQ0K",
    "cert": "LS0tLS1CRUdJTiBDRVJUSUZJQ0FURS0tLS0tDQpNSUlCNURDQ0FZdWdBd0lCQWdJVUhhUFNNdlcxOEwyaGNTeGlJK1ZqT1d0WnlyZ3dDZ1lJS29aSXpqMEVBd0l3YTHJNM1o5TUZicGt3SGthYnB0MmZBekFLQmdncWhrak9QUVFEQWdOSEFEQkVBaUFmdUhjY2R6WExqZ3BLDQplVFhnNmNNcnRzRUs4ZVlKTVFMNlZLU0xwUXIvN3dJZ1hyK2ZuVjUrb2RHWnNRUU94SjZ1aDVTV0tJVkdZQ2ZGDQp1Ykw4VmJNdnRRZz0NCi0tLS0tRU5EIENFUlRJRklDQVRFLS0tLS0NCg=="
}
```
{:codeblock}

If you enrolled an identity by using the Fabric CA client or the Fabric SDKs, your keys need to be converted from PEM format into base64 format. You can convert certificates into base64 format by running the following command on your local machine:
```
export FLAG=$(if [ "$(uname -s)" == "Linux" ]; then echo "-w 0"; else echo "-b 0"; fi)
cat $HOME/<path-to-certificate>/cert.pem | base64 $FLAG
```
{:codeblock}

### Viewing and updating identities
{: #ibp-console-identities-update-identities}

From the **Wallet** tab, click a tile to view, update, or remove an identity from the wallet. It may be necessary to update your identities if their certificates have expired, and they need to be issued new keys from the CA. You can also use this tab to delete keys from your console and your local system.

Clicking an identity opens a side panel that displays its public and private keys in base64 format. Click **Export** to download the identity's certificates to your local file system. Click **Update** to change the identity name in the wallet or paste a new set of keys into the panel. Click **Remove** when you no longer need to use this identity and want to delete its keys.

## Associating identities
{: #ibp-console-identities-associate-admin}

You need to provide the public key of your organization and node admins [to your organization MSP definition](/docs/services/blockchain/howto/ibp-console-organizations.html#console-organizations-create-msp). The nodes or channels created by the console uses the certificates from the MSP definition to check who is a valid administrator. As a result, the same public private key pair that you used to add an admin cert to the MSP definition needs to be stored inside your console wallet.

When you use the console to create an orderer or peer, you will encounter an **Associate identity** panel. Select an identity in the wallet whose certificate is also inside your organization MSP definition. You will also need to select an admin identity in the **Associate identity** section when you create or edit a channel. This will allow your console to know which identity to use when it communicates with your peers, orderers, and ordering service consortium.
