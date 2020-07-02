---

copyright:
  years: 2017, 2020
lastupdated: "2019-10-03"

keywords: TLS, TLS certificates, client applications, digital certificates, certificate authority, intermediate certificate, client-side certificate, generate certificates, manage certificates

subcollection: blockchain

---

{:external: target="_blank" .external}
{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}


# Managing certificates on {{site.data.keyword.blockchainfull_notm}} Platform
{: #managing-certificates}

{{site.data.keyword.blockchainfull}} Platform is based on Hyperledger Fabric and builds permissioned blockchain networks. Participants are known as members of the network, and their activities and access to network resources are verified continuously. A participant's identity is provided in the form of a trusted x509 digital certificate. The verification is provided by an underlying public key infrastructure and sign/verify operations that secure transactions and management within the network.
{:shortdesc}

{{site.data.keyword.blockchainfull_notm}} Platform manages most of the certificate operations without users needing to handle their certificates. However, there are times when you will have to manage the certificates that allow you to communicate with the network, such as developing applications or expanding your network with a remote peer. The following is a short guide to your Certificate Authority, and the underlying certificate infrastructure. This tutorial can help you understand the certificates that you will be working with and the tasks you need to perform.

## Certificate authorities
{: #managing-certificates-network-ca}

Certificate authorities (CAs) provide identity on the network. A CA can be considered as a publicly trusted notary that acts as an anchor of trust among multiple parties. All entities in the network are given a certificate signed by a root CA that encapsulates their digital identity. This certificate is the root of trust for all the sign and verify operations that are performed on the network. For more details about how certificate authorities are used to establish identity, see [Hyperledger Fabric documentation](https://hyperledger-fabric.readthedocs.io/en/release-1.2/identity/identity.html){: external}.

Each member in the network possesses their own CA. Your organization CA signs requests for all of the entities and components that you own, such as your admin, peers, or applications. If you want to add a remote peer or new application to your network, you need to register the new identity with your Certificate Authority (registration). Then, the CA can provide the new entity with the certificates that the entity needs to interact with the network (enrollment).

### Registration with the Network Monitor
{: #managing-certificates-ca-panel}

You can use the {{site.data.keyword.blockchainfull_notm}} Platform Network Monitor to view the identities that are registered with your CA and add new ones. Navigate to the "Certificate Authority" panel of the Network Monitor. This panel displays all the identities that have been registered with your CA, including your organization admin, peers, and client applications. To register a new identity in your organization and click the **Add User** button on the panel. A pop-up window opens and displays the following fields that are necessary to register a new identity.
  - **Enroll ID:** This will the name of your new identity, sometimes referred to as your `enroll ID`. **Save this value** for when you configure a remote peer or enroll a new application.
  - **Enroll Secret:** This will be the password to your identity, sometimes referred to as your `enroll Secret`. **Save this value** for when you configure a remote peer or enroll a new application.
  - **Type:** Select the type of identity that you want to register, either peer or client application.
  - **Affiliation:** This will be the affiliation within your organization, such as `org1` for example, that the identity will belong to.
  - **Maximum Enrollments:** You can use this field to limit the number of times your can enroll or generate certificates using this identity. If you leave the field blank, the value defaults to an unlimited number of enrollments.

### Generating client-side certificates (enrollment)
{: #managing-certificates-enrollment}
Before you can connect a third-party client to {{site.data.keyword.blockchainfull_notm}} Platform, you need to be authenticated. The process of generating the necessary certificates, your private key and your certificate (also known as your enrollment cert or signCert), is called enrollment. These certificates will be needed anytime your client communicates with the network. Any client that submits calls to the network need to sign payloads by using a private key and attach a properly signed x509 certificate.

If you enroll using the Fabric SDKs, the SDK will generate a private key and signCert. The private key and signCert form a user context that can be used to operate the SDK.

You can also generate certificates from the command line by using the [Fabric CA client](/docs/blockchain?topic=blockchain-managing-certificates#managing-certificates-enroll-register-caclient). The Fabric CA client returns more complete set of certificates inside a Membership Service Provider (MSP) folder. This folder contains the root certificate that the CA signed, intermediate certificates, a private key, and your signCert. For more information about MSP and what the MSP folder contains, see [Membership Service Providers](/docs/blockchain?topic=blockchain-managing-certificates#managing-certificates-msp).

You can generate certificates with only identities that have been registered with your Certificate Authority, by using that identity's name and secret. By default, an **admin** identity is already registered with your CA, and is listed on the "Certificate Authority" screen. You can find the secret of the admin identity in your connection profile by clicking the **Connection Profile** button on the "Overview" screen of your Network Monitor. You can also register a new identity by clicking the [Add User](/docs/blockchain?topic=blockchain-managing-certificates#managing-certificates-ca-panel) button on the "Certificate Authority" screen of the Network Monitor, and then generate certificates with the name and secret of the new identity.

**Note:** If you follow the instructions below to generate certificates by using the Fabric CA client, you start by enrolling using the admin identity. You then use those certificates to register a new client identity with your CA. Then, you have the option of enrolling again by using the client identity. Now you can use those certificates to submit transactions to the network.

### Generating certificates using the Network Monitor
{: #managing-certificates-certs-panel}

You can use the Network Monitor to generate certificates using the admin identity, and then pass those certificates directly to the SDK. Click the **Generate Certificate** button next to your admin identity to get a new signCert and private key from your CA. The **Certificate** field contains the signCert, just above the **Private Key**. You can click the copy icon at the end of each field to copy the value. You then need to save these certificates in a place where you can then import them into your application. **Note** that {{site.data.keyword.blockchainfull_notm}} Platform doesn't store these certificates. You need to safely save and store them.

### Uploading signing certificates to {{site.data.keyword.blockchainfull_notm}} Platform
{: #managing-certificates-upload-certs}

An application requires only a valid signCert to submit transactions to the network. However, if a client wants to operate the network, by installing chaincode on peers or joining peers to channels, for example, the client needs to be recognized as an administrator. Each component recognizes a set of signCerts that are owned by an admin. If you need to operate your network from a client, you need to upload your signCert and have it added to the list of admin certs. You can do this on the platform by uploading your signCert in the **Certificates** tab of the ["Overview" panel](/docs/blockchain?topic=blockchain-ibp-dashboard#ibp-dashboard-members) of your Network Monitor. Sync this certificate to your peers by hitting the restart button that comes up after uploading. Afterward, your client will be able to operate the network. You can also upload your signCert using the [Swagger API](/docs/blockchain?topic=blockchain-ibp-swagger#ibp-swagger) to add an admin certificate.

Channels also recognize a set of admin certs from the identities that are allowed to operate the channel, including being able to instantiate a chaincode on the channel. If you use a new signCert with a remote client, you must sync the certificate to the channel before you can instantiate a chaincode. Perform the following steps in the Network Monitor to add the certificate to the channel:

1. Navigate to the **Certificates** tab of the "Members" screen. Click the **Add Certificate** button to upload the signCert to the platform.
2. Navigate to the "Channels" screen and find the name of relevant channel.
3. Click **Sync Certificate** from the drop-down list under the **Actions** header to add the new certificate to the list of channel admin certs.

**Note:** Certificates that are generated by using the admin identity of your organization are not automatically considered admin certs. A signCert and private key that are generated by using the **Generate Certificates** button will not let your SDK operate the network. They have been generated by the Fabric CA client, the SDK, or the Network Monitor, and have no connection to the pre-existing list of admin certs that network components recognize. You need to upload the signCert in the Network Monitor before your SDK can operate the network.

### Certificate expiration
{: #managing-certificates-expiration}

Certificates that CAs generate on the {{site.data.keyword.blockchainfull_notm}} Platform will expire after one or three years. The expiration period is the same for certificates that are generated by using the Fabric SDKs, the Fabric CA client, or by using the Network Monitor. If the certificates expire, your applications can no longer interact with your network. You need to re-enroll to generate new certificates. If you have hit the enrollment limit of a user, you can register a new user and then enroll. You also need to upload the new certificates to the platform if you used the old certificates to operate your network.

You can use your command line to check your certificates expiry date. Run the following command to display your certificates in a human-readable form:
```
openssl x509 -in <certificate file> -text
```
{:codeblock}

Your client certificate will look similar to the example below:

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

You can find the expiry date in the **Validity** section and follows `Not After:` In this example, the certificate will expire on Nov 28 2019.

## Using TLS Certificates
{: #managing-certificates-tls}

[Transport Layer Security](https://www.ibm.com/support/knowledgecenter/en/SSFKSJ_7.1.0/com.ibm.mq.doc/sy10660_.htm) (TLS) is embedded in the trust model of Hyperledger Fabric. All components on {{site.data.keyword.blockchainfull_notm}} Platform use TLS to authenticate and communicate with each other. Therefore, you need to attach a TLS certificate issued by the platform to your calls in order to validate and encrypt your communication. The other certificates discussed in this tutorial protect your ability to transact with and manage the network. TLS certs are used to secure your calls to the network.

TLS certificates are issued publicly by the platform and are the same for all your network components. You can download the TLS certificates with the following links, depending on your membership plan and cloud location. You can also find the TLS certificates in your [credentials profile](/docs/blockchain?topic=blockchain-ibp-dashboard#ibp-dashboard-connection-profile). This certificate can reside anywhere, as long as you can reference it from your application or command line.

- [TLS Cert for Enterprise Plan](https://public-certs.us-south.ibm-blockchain-5-prod.cloud.ibm.com/3.secure.blockchain.ibm.com.rootcert){: external}

All {{site.data.keyword.blockchainfull_notm}} Platform networks use server-side TLS, in which the network needs to authenticate your clients. Enterprise Plan networks can also enable mutual TLS, in which the client and the server authenticate each other, to further secure your applications. Client-side TLS certificates (for mutual TLS) are issued by the client CA and are unique to your network. If you use an Enterprise Plan network, it is recommended that you enable mutual TLS. For more information about mutual TLS, see these [mutual TLS instructions](/docs/blockchain?topic=blockchain-ibp-dashboard#ibp-dashboard-mutual-tls).

### Retrieving domain name from TLS certificates
{: #managing-certificates-retrieve-domain}

When you communicate with a Hyperledger Fabric component that is located outside {{site.data.keyword.blockchainfull_notm}} Platform, your call must be submitted by using the correct domain name. If a call is submitted to a component's IP address without resolving to the component's domain name, the call will be rejected and returns with an error.

You can find the full URL and domain name of components that are hosted on the platform in your connection profile. The following example shows the full URL of a peer, which is concatenated with the peer's domain name, `us01.blockchain.ibm.com`.
```
grpcs://n7413e3b503174a58b112d30f3af55016-org1-peer1.us01.blockchain.ibm.com:31002"
```

You can also find a component's domain name from its TLS certificate.

1. Download one of the root TLS certs from the list above and save it on your local machine.
2. In the same directory as the root TLS certs, run the following command. This command can display the certificate in a human-readable form in your command line. You can then find important information, such as the domain name, in the command line.
  ```
  openssl x509 -in <certificate file> -text
  ```
  {:codeblock}

For example, if you download the first certificate listed, and issue the following command, `openssl x509 -in us01.blockchain.ibm.com.cert -text`, you can see the following code as part of the output.

```
Certificate:
    Data:
        Version: 3 (0x2)
        Serial Number:
            05:5c:42:fb:90:b9:cb:3d:60:7c:e4:ec:7f:7f:8e:38
    Signature Algorithm: ecdsa-with-SHA256
        Issuer: C=US, O=DigiCert Inc, CN=DigiCert ECC Secure Server CA
        Validity
            Not Before: Jun 11 00:00:00 2018 GMT
            Not After : Jun 19 12:00:00 2019 GMT
        Subject: C=US, ST=New York, L=Armonk, O=International Business Machines Corporation, CN=*.us01.blockchain.ibm.com
    ...
    ...
```

The domain name is listed in the subject line as `CN=*.us01.blockchain.ibm.com`. You can also find alternative domain names that are listed farther down in the certificate.

Retrieving a component's domain name from TLS certificates can be useful if you communicate with a remote peer or a Fabric component that is hosted outside {{site.data.keyword.blockchainfull_notm}} platform. You can then add the domain name to an SSL override when you use the SDKs, or add the domain name and corresponding IP address to your `etc.hosts` file.

## Membership Service Providers (MSPs)
{: #managing-certificates-msp}

Components of {{site.data.keyword.blockchainfull_notm}} Platform consume identities via Membership Service Providers (MSPs). MSPs associate the certificates that the CAs issue with network and channel roles. For more information about MSPs, see [Membership concept topic in Hyperledger Fabric documentation](https://hyperledger-fabric.readthedocs.io/en/release-1.2/membership/membership.html){: external}.

The MSP folders within Fabric have a defined structure. When you enroll using the Fabric CA client, the client stores the certificates in an MSP folder on your local file system with the following subfolders:

- **cacerts:** This folder contains the root certificate of the root CA of your network.
- **intermediatecerts:** These are certificates of the intermediate CAs of your network. These intermediate CAs are linked to the root CA and form a chain of trust. Each Enterprise Plan organization has two intermediate CAs for failover and high availability.
- **signcerts:** This folder contains your signing certificate, which is also called your signCert or enrollment certificate. This certificate is attached to your calls to the network (a chaincode invoke, for example) when you reference your MSP directory from the command line or build a user context object with the SDKs. You can upload this certificate to the platform if you want to operate a network from the SDK or command line.
- **keystore:** This folder contains your private key. This key is used to sign your calls to the network when you reference your MSP directory from the command line or build a user context object with the SDKs. Keep this key safe to protect your network and your data.

You can also build an MSP folder that the Fabric CA client can reference by using the Network Monitor and the Swagger APIs.

- **cacerts** and **intermediatecerts**: You can fetch these certificates with the [Swagger APIs](/docs/blockchain?topic=blockchain-ibp-swagger#ibp-swagger) by issuing a Get request to the MSP API.
- **signcerts** and **keystore**: You can generate these certificates by clicking the **Generate Certificates** button on the "Certificate Authority" panel. A pop-up window opens with two certificates listed. Copy and store the **Certificate** in signcerts and the **Private key** in keystore. Keep these certificates in a safe place because they are not stored on the platform.

Many Fabric components contain additional information inside their MSP folder. For example, if you operate a remote peer, you might see the following folders:

- **admincerts:** This folder contains the list of administrators for this organization or component. You need to upload your signCert to this folder if you are operating a remote peer from the command line or the SDKs. If you use the Fabric CA client, you also need an admincerts folder in your MSP containing the corresponding signCerts to be recognized as administrator certs.
- **tls:** This is a folder where you store TLS certs that are used for communicating with other network components.

For more information about the structure of MSPs, see [Membership](https://hyperledger-fabric.readthedocs.io/en/release-1.2/membership/membership.html){: external} and [Membership Service Providers](https://hyperledger-fabric.readthedocs.io/en/release-1.2/msp.html){: external} in Hyperledger Fabric documentation.

## Enrollment and registration by using the Fabric CA client
{: #managing-certificates-enroll-register-caclient}

You can also use the Fabric CA client to generate certificates and register a new identity with the Certificate Authority. The instructions below generate certificates by using your admin identity, then use those certificates to register a new client. For more information about enrolling by using the Fabric CA client and generating certificates, see [Membership Services Providers](/docs/blockchain?topic=blockchain-managing-certificates#managing-certificates-msp).

### Enrolling by using the Fabric CA client
{: #managing-certificates-enroll-app-caclient}

1. Download the [Fabric CA images](https://github.com/hyperledger/fabric-ca/releases/tag/v1.4.4) to your local machine and extract them, then move them to a folder on such as `$HOME/fabric-ca-platform/`. Change to the directory where you moved your client images so that you can reference it directly in your commands.
    ```
    cd $HOME/fabric-ca-platform/
    ```
    {:codeblock}

2.  Set the path where the client can create your keys. Ensure that you remove the configuration material that past attempt create. If you don't run the `enroll` command before, the `msp` folder and the `.yaml` file do not exist.
    ```
    export FABRIC_CA_CLIENT_HOME=$HOME/fabric-ca-platform
    rm -rf $FABRIC_CA_CLIENT_HOME/fabric-ca-client-config.yaml
    rm -rf $FABRIC_CA_CLIENT_HOME/msp
    ```
    {:codeblock}

3. Download the TLS certs from {{site.data.keyword.cloud_notm}} depending on the service plan, location, and cluster that you use. You can find your service plan based on the URL of your Certificate Authority.
  - [TLS Cert for Enterprise Plan](https://public-certs.us-south.ibm-blockchain-5-prod.cloud.ibm.com/3.secure.blockchain.ibm.com.rootcert){: external}

  Save the contents to a folder, for example ``$HOME/tls``. This step allows the data flowing to be encrypted on the wire.

4. Open the **Connection Profile** JSON file from your "Overview" panel in the Network Monitor, and find the following variables:
  * URL for CA: ``url`` under `certificateAuthorities`
  * Admin user ID: ``enrollId``
  * Admin password: ``enrollSecret``
  * CA Name: ``caName``

5. You can use the Fabric CA client to send an `enroll` call to the Certificate Authority by passing in the TLS certs path and the four strings above with the following command:
  ```
  ./fabric-ca-client enroll -u https://<enroll_id>:<enroll_password>@<ca_url_with_port> --caname <ca_name> --tls.certfiles <tls_cert_path>
  ```
  {:codeblock}

  You need to run this command in the directory where you move the Fabric CA client to. A real call might look similar to the following example command:
  ```
  ./fabric-ca-client enroll -u https://admin:dda0c53f7b@n7413e3b503174a58b112d30f3af55016-org1-ca.us3.blockchain.ibm.com:31011 --caname org1CA --tls.certfiles $HOME/tls/us2.blockchain.ibm.com.cert
  ```

6. Find your admin certificate in `$FABRIC_CA_CLIENT_HOME/msp/signcerts/cert.pem`. You can then upload the admin certificate to your blockchain network from the Network Monitor. For more information about adding certificates, see [the "Certificates" tab of "Member" panel](/docs/blockchain?topic=blockchain-ibp-dashboard#ibp-dashboard-members) in the Network Monitor.

  You can also find CA root certificate and admin private key in the following directories:
  * CA root certificate: `$FABRIC_CA_CLIENT_HOME/msp/cacerts/--<ca_name>.pem`
  * The admin private key: `$FABRIC_CA_CLIENT_HOME/msp/keystore/<>_sk file`

### Registering by using the Fabric CA client
{: #register-app-caclient}

1. Issue the following command to find your affiliation and your organization name in your blockchain network.
  ```
  ./fabric-ca-client affiliation list --tls.certfiles <tls_cert_path>
  ```
  {:codeblock}

  You should see information that is similar to the following example:
  ```
  affiliation: ibp
      affiliation: ibp.PeerOrg1
  ```

  Make a note of the second **affiliation** value, for example, `ibp.PeerOrg1`. You need to use this value in the command below.

2. Run the following command to register the peer.
  ```
  ./fabric-ca-client register --id.name <name> --id.affiliation <affiliation> --id.secret <password> --tls.certfiles <tls_cert_path>
  ```
  {:codeblock}

  Specify a name and password for your peer and replace `name` and `password` with your peer name and password. Make a note of this information. You need to need it when you configure your peer. For example:
  ```
  ./fabric-ca-client register --id.name user1 --id.affiliation ibp.PeerOrg1 --id.secret userpw  --tls.certfiles $HOME/tls/us2.blockchain.ibm.com.cert
  ```

  You can only register an identity once. If you experience a problem, try a command with a new user name and password.

  When the command completes successfully, you should see information that is similar to the following example:
  ```
  2018/06/18 16:53:00 [INFO] Configuration file location: /fabric-ca-platform/admin/fabric-ca-client-config.yaml
  2018/06/18 16:53:00 [INFO] TLS Enabled
  2018/06/18 16:53:00 [INFO] TLS Enabled
  Password: userpw
  ```
