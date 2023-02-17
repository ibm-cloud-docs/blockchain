---

copyright:
  years: 2014, 2023
lastupdated: "2023-02-17"

keywords: admin certificate, Node OU, admin identity, expiration

subcollection: blockchain

---



{{site.data.keyword.attribute-definition-list}}

# Managing certificates
{: #cert-mgmt}

{{site.data.keyword.blockchainfull}} Platform is based on Hyperledger Fabric and builds permissioned blockchain networks. Participants are known as members of the network, and their activities and access to network resources are verified continuously. Learn how to manage a member's identity in the form of a trusted x509 digital certificate.
{: shortdesc}

**Target audience:** This advanced topic is designed for system administrators or operators who are responsible for registering users, enrolling identities, and administering ordering services or channels. You should be familiar with Membership Service Providers (MSPs) and how they are created.

{{site.data.keyword.blockchainfull_notm}} Platform manages most of the certificate operations without users needing to handle their certificates. However, there are instances when you need to manage the certificates that allow you to communicate with the network, such as for developing applications or renewing certificates before they expire. This topic is a short guide to your Certificate Authority and the underlying certificate infrastructure. This information can help you understand the certificates that you will be working with and the tasks you are responsible for.

For details on Hyperledger Fabric certificates, including certificate names, types, and locations, see the [Certificates Management Guide](https://hyperledger-fabric.readthedocs.io/en/latest/certs_management.html).

## Certificate Authorities (CAs)
{: #cert-mgmt-network-ca}

Certificate authorities (CAs) provide identity on the network. A CA can be considered as a publicly trusted notary that acts as an anchor of trust among multiple parties. All entities in the network are given a certificate that is signed by a root CA that encapsulates their digital identity. This certificate is the root of trust for all of the sign and verify operations that are performed on the network. For more details about how certificate authorities are used to establish identity, see [Hyperledger Fabric documentation](https://hyperledger-fabric.readthedocs.io/en/release-2.1/identity/identity.html){: external}.

Each organization in the network has their own CA. Your organization CA signs requests for all of the entities and components that you own, such as your admin, peers, or applications. If you want to add a node or new application to your network, you need to register the new user with your Certificate Authority (registration). Then, via the enrollment process, the CA generates a public and private key pair.  The "enrollment certificate", also known as the signed certificate or public key, is shared with the network.

## Overview
{: #cert-mgmt-overview}

The following diagram shows all of the certificates that need to be managed and where they reside on your network.

![Certificate overview diagram](images/cert-expiration-all.png "Certificate overview diagram"){: caption="Figure 1. Certificate overview diagram" caption-side="bottom"}

The left column includes the certificates for the blockchain CA, peer, and ordering nodes. There are two certificates that are included on the CA node, the CA root certificate and the CA TLS cert. The root certificate is included here for completeness, but because it expires in 15 years, update instructions are not provided. The peer and ordering nodes are similar in that they both contain an **enrollment certificate** (the public key or signed certificate), their **TLS certificate** that enables them to transact with other nodes in their organization, and their **MSP admin certificate**, which represents the admin identity that is allowed to administer the node.  

The {{site.data.keyword.blockchainfull_notm}} Platform  can automatically renew the enrollment certs for the peer and ordering nodes and the TLS certificate for the peer. But the MSP admin identities have to be manually renewed. If the MSP is enabled for Node Organizational Units (Node OUs), no further action is required. More information about Node OU support and how to determine whether the MSP is enabled for it is provided in this topic.

The right column shows the certificates that are relevant to the system channel and the application channel. When the MSP is not enabled for Node OU support, additional steps are required to update these certificates on the system and application channels and are provided here.

Because the certificate renewal process is different for peers and ordering nodes, the certificate renewal tooling described in this topic cannot be used on networks where the peers and ordering nodes share the same organization MSP definition.
{: important}

## Node OU support
{: #cert-mgmt-nodeou}

When you register a user with the CA, you need to select a user **Type** of `client`, `peer`, `orderer`, or `admin`, that is used to confer a "role" onto the identity. Roles are referred to as **Organizational Units (OU)** inside a certificate and MSPs can be configured to recognize these roles. The `admin` and `orderer` types were added to the {{site.data.keyword.blockchainfull_notm}} Platform when Fabric v1.4.3 was included and contains support for **Node OUs** in MSPs and channels.

This new capability means that when the MSP admin identity is enrolled during MSP creation, the generated signed certificate includes the admin type as an OU inside the certificate. This admin designation means that the signed cert of the identity does not need to be included in the MSP defining the organization. You can learn more about the benefits of Node OUs in the Fabric documentation on the [Membership Service Provider](https://hyperledger-fabric.readthedocs.io/en/release-2.2/membership/membership.html#node-ou-roles-and-msps){: external}.  

Because this functionality was not yet available when the {{site.data.keyword.blockchainfull_notm}} Platform service was initially offered, all MSP admin identities that were enrolled before Node OU support was added to the platform in December 2019, do not contain the `admin` OU in their signed certificate. Instead, when the MSP was created, the signed cert, or certificate, for the MSP admin was placed in the `admins` section. The platform supports both patterns, but if the Node OU configuration was not enabled when the organization MSP was created, additional steps are required after certificate renewal to update the MSP and any channels that the MSP is part of. Therefore, if your MSPs currently are not configured with Node OU support, it is recommended that you add Node OU support now, to avoid the extra updates steps that are required in one year when the certificates expire again.

If your certificates have already expired, you are not able to complete these steps, but should come back and do this after you renew the certificates.
{: note}

**Prerequisite:** Before you can take advantage of Node OU support, ensure that your CAs are at Fabric v1.4.3 or higher. Open the CA tile to view the Fabric version. If the CA is not at v1.4.3 or higher, you should [upgrade](/docs/blockchain?topic=blockchain-ibp-console-manage-console#ibp-console-manage-patch) it now.

![View CA Fabric version](images/ca-fabric-version.png "View CA Fabric version"){: caption="Figure 2. View CA Fabric version" caption-side="bottom"}

Enabling Node OU support for an MSP definition is a two-part process. First, you have to update the MSP definition itself, then you have to update the same MSP definition on the channel.

### Part one: Enable Node OU support for an MSP definition

1. Open the **Organizations** tab to view the Node OU status for each MSP:
    ![View Node OU status on MSP tile](images/msp-nodeou.png "View Node OU status on MSP tile"){: caption="Figure 3. View Node OU status on MSP tile" caption-side="bottom"}
2. Click the **Settings** icon for the MSP.  The checkbox to add Node OU is selected by default. You do not have to add a new JSON file, you can simply click **Update MSP definition** to enable Node OU support for the MSP.  
    ![Node OU checkbox](images/nodeou-checkbox.png "Node OU checkbox"){: caption="Figure 4. Node OU checkbox" caption-side="bottom"}

The MSP is also automatically updated on any peer or ordering nodes that are configured to use this MSP. There is a brief downtime when the node is automatically restarted to use the updated MSP.

### Part two: Enable Node OU support for the MSP on a channel

1. Open the **Channels** tab, select the channel that you want to update and click **Channel details**.
3. Scroll down and click the channel member MSP definition that you just enabled Node OU support for.  The checkbox to enable Node OU support is selected by default.
3. Select the existing MSP definition and the associated identity and click **Update MSP definition**.

Because you likely still have admin certificates in the MSP that will expire, you still need to follow the manual steps in this topic to update them. But after you complete that process, you do not have to follow the manual steps again next year after the certificates expire, since you have now enabled the Node OU support on the MSP.
{: important}

## Certificate renewal and expiration
{: #cert-mgmt-renew-expiration}

By default, most certificates that are generated by {{site.data.keyword.blockchainfull_notm}} Platform CAs expire after one year. However, the CA root certificate, CA TLS certificate, and the ordering node TLS certificates have longer periods before expiration. The default expiration period is the same for certificates that are generated by using the Fabric SDKs, the Fabric CA client, or by using the console. The expiration date for all certificates that expire within five years is visible in the console, and how to view the actual date is described later in this topic.

### Automatic certificate renewal
{: #cert-mgmt-auto-renewal}

Use the following topics to set up automatic certificate renewal.

#### Enrollment and TLS certificates
{: #cert-mgmt-auto-renewal-ecerts}

Because the console automatically enrolls identities for peer or ordering nodes when the nodes are deployed, the {{site.data.keyword.blockchainfull_notm}} Platform **automatically renews the enrollment (signing) certificates for the peer and ordering nodes and the peer TLS certificate**.  If the certificates expire within 30 days, the platform automatically attempts to contact the CA and renew the certificates. After these certificates are automatically renewed, there is no further action that needs to be taken.

If the CA is offline, unreachable, or the CA TLS certificate has expired, the certificates cannot be automatically renewed. The peer or ordering node displays a warning message indicating that certificate expiration is approaching or has occurred and you need to take action.

Peer and ordering node enrollment certificates can be renewed from the console, by opening the node tile and clicking the **Settings** icon. Click **Update certificates** to renew either the enrollment or TLS certificate. See detailed instructions for the [peer](/docs/blockchain?topic=blockchain-cert-mgmt#cert-mgmt-enroll-tls-peer) and [ordering node](/docs/blockchain?topic=blockchain-cert-mgmt#cert-mgmt-enroll-tls-ordering-node) enrollment and TLS certificates. Note that when a certificate is updated, the node is restarted.

#### Admin certificates
{: #cert-mgmt-auto-renewal-admin}

Organization MSP admin certificates cannot be automatically renewed. Instead, you need to follow the manual certificate renewal process, included in this topic, to update these certificates. If you have a large number of channels on your network, Ansible playbooks, also described later in this topic, are available to update admin certificates that have not yet expired across the channels on your network in a bulk action.

### Certificate types and actions
{: #cert-mgmt-cert-types}

When certificates expire, nodes can no longer process transactions and your applications can no longer interact with your network. Therefore, before a certificate expires, you need to **enroll the identity again to generate a new certificate**, a process referred to as "certificate renewal".   

The following tables describe the types of certificates that you need to manage, how to view their expiration date, and how to maintain them.

#### CA TLS certificate

|  Certificate | Description| How generated | Default expiration | How to view expiration | How to renew | Impact if expires | What to do if expired |
|-----------------|-----------------|-----------------|-----------------|-----------------|-----------------|-----------------|-----------------|
| **CA TLS certificate** | Used to trust the CA server. Contains the public key that must be shared with all members in the organization that want to transact with any node in the organization. When any client or node submits a transaction to another node, it must include this certificate as part of the transaction to prevent “man in the middle” attacks. | Generated when the CA is first started because TLS is enabled.| **2.5.x:** 10 years[^how-to-view1]<br><br>**2.1.x:** 1 year  | If it expires within five years, the expiration date is visible from the console. Open CA node and view **TLS Cert Expiration** field[^how-to-view2]   | See [Renew the CA TLS certificate](#cert-mgmt-ca-tls).| Transactions continue.<br><br>- Cannot register or enroll new identities, but transaction traffic does not stop.<br><br>- Automatic certificate renewal fails.<br><br>- Cannot renew other certificates without fixing CA TLS certificate first. | See [Renew the CA TLS certificate](#cert-mgmt-ca-tls). |
{: caption="Table 1. How to manage the CA certificates" caption-side="top"}

[^how-to-view1]: If a certificate expires in more than five years, the expiration date is not visible from the console.

[^how-to-view2]: If a certificate expires in more than five years, the expiration date is not visible from the console.

#### Peer certificates

| Certificate | Description | How generated | Default expiration| How to view expiration | How to renew| Impact if expires| What to do if expired|
|-----|-----|-----|-----|-----|-----|-----|-----|
| **Peer enrollment certificate (signcert)** | The public key that is used to verify that the signatures that nodes attach to communications were generated by the node's private key.| Generated when the peer is deployed based on the peer enroll ID and secret that is provided.| 1 year | Open console peer node and view **Enrollment Cert Expiration** field.  |  [Automatic renewal](#cert-mgmt-auto-renewal) is attempted 30 days before expiration. <br><br> If **automatic renewal** fails, [manually update the peer certificates](#cert-mgmt-enroll-tls-peer). | **Production outage** <br><br> Peers can no longer connect to orderers or to each other. |[Manually update the peer certificates](#cert-mgmt-enroll-tls-peer). |
|**Peer TLS certificate (signcert)** | When TLS is enabled on a network, each node must register a user and enroll an identity with the TLS CA. This is the TLS certificate for the peer from that process and is required for the peer to start.| Generated when the peer is deployed.| **2.5.x:** 15 years[^how-to-view3]<br><br>**2.1.x:** 1 year | Open console peer node and view **TLS Cert Expiration** field.  | [Automatic renewal](#cert-mgmt-auto-renewal) is attempted 30 days before expiration. <br><br> If **automatic renewal** fails, [manually update the peer certificates](#cert-mgmt-enroll-tls-peer).| **Production outage** <br><br> SDK does not allow connection when TLS certificates are expired.|[Manually update the peer certificates](#cert-mgmt-enroll-tls-peer).|
{: caption="Table 2. How to manage the peer certificates" caption-side="top"}

[^how-to-view3]: If a certificate expires in more than five years, the expiration date is not visible from the console.

#### Orderer certificates

| Certificate | Description | How generated | Default expiration| How to view expiration | How to renew| Impact if expires| What to do if expired|
|-----|-----|-----|-----|-----|-----|-----|-----|
| **Orderer enrollment certificate (signcert)** | The public key that the orderer uses to sign transactions. | Generated when the ordering node is deployed based on the ordering service enroll ID and secret that is provided.| 1 year | Open console ordering node and view **Enrollment Cert Expiration** field. | [Automatic renewal](#cert-mgmt-auto-renewal) is attempted 30 days before expiration. <br><br> If **automatic renewal** fails, see [Manual certificate renewal](#cert-mgmt-enroll-tls-ordering-node)| **Production outage** <br><br>Orderers can no longer work. | [Manual certificate renewal](#cert-mgmt-enroll-tls-ordering-node) |
|**Orderer TLS certificate (signcert)** | When TLS is enabled on a network, each node must register a user and enroll an identity with the TLS CA. This is the TLS certificate for the ordering node from that process and is required for the ordering node to start.| Generated when the ordering node is deployed.| **2.5.x:** 15 years[^how-to-view4]<br><br>**2.1.x:** 1 year | Open console ordering node and view **TLS Cert Expiration** field. | [Automatic renewal](#cert-mgmt-auto-renewal) is attempted 30 days before expiration. <br><br> If **automatic renewal** fails, see [Manual certificate renewal](#cert-mgmt-enroll-tls-ordering-node) | **Production outage** <br><br>Ordering nodes are no longer allowed to participate in cluster.  Quorum is lost, environment goes down.| [Manual certificate renewal](#cert-mgmt-enroll-tls-ordering-node) |
{: caption="Table 3. How to manage the orderer certificates" caption-side="top"}

[^how-to-view4]: If a certificate expires in more than five years, the expiration date is not visible from the console.


#### Peer admin certificate

The following diagram summarizes the process for updating peer admin certificates on your network. Click a circle to view the instructions.

<img usemap="#home_map" border="0" class="image" id="image_ztx_crb_f1b" src="images/peer-admin-nodes.png" width="750" alt="Peer admin certificate renewal process." style="width:750px;" />
<map name="home_map" id="home_map">
<area href="/docs/blockchain?topic=blockchain-cert-mgmt#cert-mgmt-manual-renewal" alt="View certificate expiration date" title="View certificate expiration date" shape="rect" coords="2, 290, 68, 390" />
<area href="/docs/blockchain?topic=blockchain-cert-mgmt#cert-mgmt-manual-enroll" alt="Enroll a new identity" title="Enroll a new identity" shape="rect" coords="118, 210, 196, 300" />
<area href="/docs/blockchain?topic=blockchain-cert-mgmt#ibp-console-identities-expired-certs-admin" alt="Register a new user" title="Register a new user and enroll a new identity" shape="rect" coords="113, 379, 202, 503" />
<area href="/docs/blockchain?topic=blockchain-cert-mgmt#cert-mgmt-manual-update-msp" alt="Update MSP" title="Update MSP" shape="rect" coords="269, 218, 335, 313" />
<area href="/docs/blockchain?topic=blockchain-cert-mgmt#cert-mgmt-manual-update-node-identity" alt="Associate admin identity" title="Associate admin identity" shape="rect" coords="354, 218, 428, 338" />
<area href="/docs/blockchain?topic=blockchain-cert-mgmt#cert-mgmt-manual-update-node-identity" alt="Associate admin identity" title="Associate admin identity" shape="rect" coords="264, 382, 347, 492" />
<area href="/docs/blockchain?topic=blockchain-cert-mgmt#cert-mgmt-manual-update-channel-peer" alt="Update MSP on application channel" title="Update MSP on application channel" shape="rect" coords="440, 218, 510, 315" />
<area href="/docs/blockchain?topic=blockchain-cert-mgmt#cert-mgmt-export-msp" alt="Share MSP with consortium members" title="Share MSP with consortium members" shape="rect" coords="530, 217, 605, 326" />
<area href="/docs/blockchain?topic=blockchain-cert-mgmt#cert-mgmt-import-msp" alt="Import MSP JSON" title="Import MSP JSON" shape="rect" coords="523, 125, 607, 196" />
<area href="/docs/blockchain?topic=blockchain-cert-mgmt#cert-mgmt-manual-update-os-channel-member" alt="Update ordering service channel member" title="Update ordering service channel member" shape="rect" coords="630, 125, 703, 237" /></map>


| Certificate | Description | How generated | Default Expiration| How to view expiration | How to renew|  Update MSP | Update node admin | Update channel | Share with network | Impact if expired | What to do if expired|
|-----|-----|-----|-----|-----|-----|-----|-----|-----|-----|-----|-----|
| **Peer organization admin certificate** | The admin certificate that is used to administer the peer and install smart contracts on the peer. It can also be configured to manage the application channel that the peer belongs to. The associated admin user is registered with the organization CA before the peer and organization MSP are created.   | Generated when you create the peer organization MSP and provide the admin identity enroll ID and secret. In addition to being part of the peer node configuration, this MSP certificate can also be included in the channel that the peer joins as a channel member or channel administrator. | 1 year | **Wallet tab:** Click the peer organization admin identity tile to view the expiration date of the certificate and private key. <br><br> **Peer node:** If Node OU support is not enabled for the peer MSP, open peer tile and view the **Admin Cert Expiration** in the left column.  <br><br>**MSP tab:** Open peer organization MSP tile, the admin certificate expiration date is listed in left column.<br><br>**Channel details tab:** Open channel and click member tile to view expiration date.| [Enroll new identity](#cert-mgmt-manual-enroll)  |   No action is required when Node OU is enabled for the MSP.<br><br> Otherwise, [Append the admin certificate to the MSP definition](#cert-mgmt-manual-update-msp).| No action is required when Node OU is enabled for the MSP.<br><br> Otherwise, [Associate new admin identity on peer](#cert-mgmt-manual-update-node-identity). | No action is required when Node OU is enabled for the MSP.<br><br> Otherwise, [Update channel](#cert-mgmt-manual-update-channel-peer) <br> <br>or <br><br>[Bulk admin certificate update with Ansible playbooks](#cert-mgmt-bulk-ansible)| All consortium members and ordering service admin need to import the updated MSP into their Organizations tab. <br><br> Ordering service admin needs to [Update ordering service channel member](#cert-mgmt-manual-update-os-channel-member). | Transactions can continue to work successfully. <br><br>But you are unable to install new smart contracts from a client application or manage the peer from the console. |See [Expired certificates](#ibp-console-identities-expired-certs).|
{: caption="Table 4. How to manage the peer organization admin certificate" caption-side="top"}

#### Ordering service MSP admin certificate

The following diagram summarizes the process for updating orderer organization admin certificates on your network. Click a circle to view the instructions.

<img usemap="#home_map2" border="0" class="image" id="image_ztx_crb_f13" src="images/orderer-admin-nodesv2.png" width="750" alt="Orderer organization admin certificate renewal process." style="width:750px;" />
<map name="home_map2" id="home_map2">
<area href="/docs/blockchain?topic=blockchain-cert-mgmt#cert-mgmt-manual-renewal" alt="View certificate expiration date" title="View certificate expiration date" shape="rect" coords="0, 287, 73, 388" />
<area href="/docs/blockchain?topic=blockchain-cert-mgmt#cert-mgmt-manual-enroll" alt="Enroll a new identity" title="Enroll a new identity" shape="rect" coords="112, 210, 179, 290" />
<area href="/docs/blockchain?topic=blockchain-cert-mgmt#ibp-console-identities-expired-certs-admin" alt="Register a new user" title="Register a new user and enroll a new identity" shape="rect" coords="103, 369, 189, 490" />
<area href="/docs/blockchain?topic=blockchain-cert-mgmt#cert-mgmt-manual-update-msp" alt="Update MSP" title="Update MSP" shape="rect" coords="248, 219, 312, 296" />
<area href="/docs/blockchain?topic=blockchain-cert-mgmt#cert-mgmt-manual-update-node-identity" alt="Associate admin identity" title="Associate admin identity" shape="rect" coords="330, 219, 394, 331" />
<area href="/docs/blockchain?topic=blockchain-cert-mgmt#cert-mgmt-manual-update-node-identity" alt="Associate admin identity" title="Associate admin identity" shape="rect" coords="246, 375, 315, 485" />
<area href="/docs/blockchain?topic=blockchain-cert-mgmt#cert-mgmt-manual-update-os-admin" alt="Update OS admin on system channel" title="Update OS admin on system channel" shape="rect" coords="552, 215, 612, 297" />
<area href="/docs/blockchain?topic=blockchain-cert-mgmt#cert-mgmt-export-msp" alt="Share MSP with consortium members (out of band)" title="Share MSP with consortium members (out of band)" shape="rect" coords="626, 216, 693, 306" />
<area href="/docs/blockchain?topic=blockchain-cert-mgmt#cert-mgmt-import-msp" alt="Import MSP JSON" title="Import MSP JSON" shape="rect" coords="620, 123, 687, 187" />
<area href="/docs/blockchain?topic=blockchain-cert-mgmt#cert-mgmt-manual-update-channel" alt="Update orderer member MSP on channel" title="Update orderer member MSP on channel" shape="rect" coords="700, 122, 759, 227" />
<area href="/docs/blockchain?topic=blockchain-cert-mgmt#ibp-console-identities-expired-certs-orderer-override" alt="Override NoExpirationChecks on each ordering node" title="Override NoExpirationChecks on each ordering node" shape="rect" coords="449, 304, 540, 387" /></map>


| Certificate | Description | How generated | Default expiration| How to view expiration | How to renew|  Update MSP | Update node admin | Update system channel | Share with network | Impact if expired | What to do if expired|
|-----|-----|-----|-----|-----|-----|-----|-----|-----|-----|-----|-----|
| **Ordering service organization admin certificate** | The admin certificate that is used to administer the ordering service and submit or approve channel updates. The associated admin identity is enrolled with the organization CA before the node and organization MSP are created.   | Generated when you create the organization MSP and provide the admin identity enroll ID and secret. In addition to being part of the ordering service configuration, this MSP certificate can also be included in the ordering service channels. | 1 year | **Wallet tab:** Click the ordering service admin identity tile to view the expiration date of the certificate and private key. <br><br>**MSP tab:** Open the ordering service organization MSP tile and the admin certificate expiration date is listed in left column.| [Enroll new identity](#cert-mgmt-manual-enroll) |   No action is required when Node OU is enabled for the MSP.<br><br> Otherwise, [Append the admin certificate to the MSP definition](#cert-mgmt-manual-update-msp).| No action is required when Node OU is enabled for the MSP.<br><br> Otherwise, [Associate new admin identity on orderer](#cert-mgmt-manual-update-node-identity)   | No action is required when Node OU is enabled for the MSP.<br><br> Otherwise, [Update ordering service admin](#cert-mgmt-manual-update-os-admin)  <br><br> or <br><br>[Bulk admin certificate update with Ansible playbooks](#cert-mgmt-bulk-ansible).| All consortium members need to import the updated MSP into their Organizations tab. <br><br> One consortium member needs to submit the request to [update the orderer member MSP on the application channel](/docs/blockchain?topic=blockchain-cert-mgmt#cert-mgmt-manual-update-channel).| Transactions can continue to work successfully. <br><br>But you are unable to modify channels from a client application or manage the orderer from the console. | See [Expired certificates](#ibp-console-identities-expired-certs). |
{: caption="Table 5. How to manage the orderer organization admin certificate" caption-side="top"}

## Manual certificate renewal
{: #cert-mgmt-manual-renewal}

While the platform automatically renews the certificates for peer and ordering nodes if the associated CA is available, customers are responsible for managing certificate expiration and renewal of the following certificates:

- CA TLS certificate
- Peer and ordering node enrollment and TLS certificates, only when automatic renewal is not successful
- MSP admin certificates
- Certificates from an external CA.

If the admin certificates have been stored or imported into the console wallet, you can monitor the exact date of expiration for each identity. Click an admin identity tile to view the expiration date of the certificate and private key.

![Admin Certificate expiration](images/cert-expiration.png "Admin Identity certificate expiration"){: caption="Figure 5. Admin identity certificate expiration" caption-side="bottom"}

Otherwise, if certificates expire within five years, their expiration date is visible from the peer, ordering node, MSP, and channel tiles.

To manually renew certificates, see:
- [CA TLS certificate](#cert-mgmt-ca-tls)
- [Peer enrollment and TLS Certificates](#cert-mgmt-enroll-tls-peer)
- [Ordering node enrollment and TLS certificates](#cert-mgmt-enroll-tls-ordering-node)
- [MSP Admin certificate](#cert-mgmt-manual-renewal-admin)
- [Certificates from an external CA](#cert-mgmt-manual-ext-ca)

## Renew the CA TLS certificate
{: #cert-mgmt-ca-tls}

Because automatic renewal of peer and ordering node enrollment certificates is not possible when the CA TLS certificate expires, it is important to keep your CA TLS certificate current.  Transactions on the network can continue to be successfully processed even if the CA TLS certificate expires, but you are not able to enroll new identities, which is required when certificates expire.

The process to renew the CA TLS certificate is same regardless of whether it has expired or not, although it is not available on an imported CA. In that case, the update needs to be performed from the console where the CA was deployed.

1. Open the CA tile and click the **Settings** icon.
2. Click **Renew TLS certificate**. The certificate is replaced and the CA is restarted.
3. After the CA is restarted, click the **Refresh certificates** icon next to the CA **Settings** icon to update the console.
4. For your network to work, it is required that you update all of the associated nodes that are currently using the original TLS certificate with the new TLS certificate to secure their communications. Click the **Settings** icon again and this time click **Update associated nodes**. All of the nodes in the list of associated nodes need to be updated.

This update requires a brief restart of the associated nodes. For zero downtime and to ensure nodes are still available to process transactions, it is recommended that the updates are performed on one node at a time.
{: important}

Because the console only shows CA TLS certificate expiration dates if they are within five years, it is possible that the expiration date of the CA TLS certificate is not visible when you open the CA tile. If you've just successfully renewed the certificate, the expiration date is no longer visible on the page.
{: tip}

## Renew Peer enrollment and TLS Certificates
{: #cert-mgmt-enroll-tls-peer}

When the peer enrollment certificate expires, the peer can no longer communicate with orderers or other peers in the organization. Likewise, when the peer TLS certificate expires, client applications that use the Fabric SDK are no longer able to send transaction proposals to the peer.  The automatic certificate renewal process attempts to re-enroll the peer enrollment and TLS certificates 30 days before they expire, and when successful, no further action is required on your part. But if the automatic certificate renewal fails because the CA is down or unreachable when the renewal attempt is made, or if the associated CA TLS certificate is expired, you have to manually update these certificates. When automatic renewal fails, a warning is displayed on the peer tile.

While you can use the following steps to renew certificates before or after they expire, to avoid an interruption of service, it is best to renew the certificates before they expire. Check the expiration date of your peer enrollment and TLS certificates by opening the peer node.

![Peer enrollment and TLS certificate expiration](images/peer-cert-expiry.png "Peer enrollment and TLS certificate expiration"){: caption="Figure 6. Peer enrollment and TLS certificate expiration" caption-side="bottom"}

1. Open the peer and click the **Settings** icon.
2. Click **Update certificates**. You can renew either the `Enrollment certificate` or the `TLS certificate` or both, based on their expiration dates.
3. In the drop-down box for each certificate type, select **Re-enroll certificate** if the certificate has not expired, or **Enroll certificate** if it has already expired.
4. When you click **Update certificates**, the peer is restarted. Verify the renewal is successful by clicking the refresh icon and examining the updated enrollment and TLS certificate expiration dates.

    ![Certificate refresh button](images/cert-refresh.png "Certificate refresh button"){: caption="Figure 7. Certificate refresh button" caption-side="bottom"}

While the console does not allow you to set custom expiration dates for certificates, notice that the Peer TLS certificate expiration date is in 15 years, so that renewal of this TLS certificate is not required again next year.  However, the peer enrollment certificate expires again in one year, so you will need to renew it again before that date.
{: note}

An alternative to renewing these certificates is to simply deploy a new peer, and then delete the peer with expired certificates. You would need to configure the new peer on the network in the same way the existing peer is configured, namely:
- Join it to the same channel or channels.
- Make this peer an [anchor peer](/docs/blockchain?topic=blockchain-ibp-console-build-network#ibp-console-build-network-join-peer), if the previous peer was an anchor peer.
- Install the same smart contracts on it.
- Download a new [connection profile](/docs/blockchain?topic=blockchain-ibp-console-organizations#ibp-console-organizations-connx-profile) that includes the new peer for client applications to address the new peer.

## Renew ordering node enrollment and TLS certificates
{: #cert-mgmt-enroll-tls-ordering-node}

If any single ordering node enrollment certificate expires, the entire ordering service goes down. When the ordering node TLS certificate expires, the node can no longer participate in the Raft cluster, which can cause  consenter [quorum](/docs/blockchain?topic=blockchain-glossary#glossary-quorum) to be lost, and the blockchain network goes down. It’s also not possible to upgrade any node in the ordering service if one of ordering nodes in the orderer consenter set is down. The automatic certificate renewal process attempts to re-enroll the ordering node enrollment and TLS certificates 30 days before they expire, and when successful, no further action is required on your part. But if the automatic certificate renewal fails because the CA is down or unreachable when the renewal attempt is made, or if the associated CA TLS certificate is expired, you have to manually update these certificates. When automatic renewal fails, a warning is displayed on the ordering node tile. While you can use this process to renew certificates before or after they expire, to avoid an interruption of service, it is best to renew the certificates before they expire.  Check the expiration date of your ordering node enrollment and TLS certificates by opening the ordering node.  

![Ordering node enrollment and TLS certificate expiration](images/ordering-node-cert-expiry.png "Ordering node enrollment and TLS certificate expiration"){: caption="Figure 8. Ordering node enrollment and TLS certificate expiration" caption-side="bottom"}

Because this certificate renewal process restarts the ordering node, and in the case of a TLS certificate renewal, the node can no longer participate in the application channel [consenter set](/docs/blockchain?topic=blockchain-glossary#glossary-consenter), you need to pay particular attention to maintaining quorum on the application channel throughout this process. Before attempting these steps, and to avoid any interruption of service, **ensure that you still have enough ordering nodes available to maintain quorum** while this node is restarted or offline, in the case of a TLS certificate update.
{: important}

The process to renew the enrollment certificate is the same for the ordering node as it is for the peers. But when you renew the ordering node TLS certificate, additional steps are required. Before proceeding, if you have a single-node ordering service and need to renew the TLS certificate, you should review [Considerations for a single node ordering service](#cert-mgmt-manual-renewal-os1).

#### Renew the certificates

1. Open the ordering service and click the **Ordering nodes** tab. As ordering node certificates approach expiration, a warning icon is visible on their tile.
2. Open the ordering node you want to update. If automatic certificate renewal failed for the node, you should see a warning. Click the **Settings** icon.
3. Click **Update certificates**. You can renew either the `Enrollment certificate` or the `TLS certificate` or both, based on their expiration dates.
4. In the drop-down box for each certificate type, select **Re-enroll certificate** if the certificate has not expired, or **Enroll certificate** if it has already expired.
5. When you click **Update certificates**, the ordering node is restarted.  Verify renewal is successful by clicking the certificate refresh button next to the **Settings** icon and examining the updated enrollment and TLS certificate expiration dates.
    Notice that the ordering node TLS certificate expiration date is in 15 years, so that renewal of this TLS certificate is not required again next year.  However, the enrollment certificate expires again in one year, so you will need to renew it again before that date.
    {: note}

6. If you only updated the ordering node enrollment certificate then you are done. But as mentioned earlier, if you updated the ordering node TLS certificate, additional steps are required to update the consenter set on the ordering service system and application channels with the new TLS certificate.

**Update the consenter set on the ordering service system channel (TLS certificate renewal only)**  

When you update the TLS certificate, you also need to update the consenter set on the ordering service system channel. This action ensures that any new application channels which include this ordering node as a consenter will use the updated TLS certificate.

In order to update the ordering service, you must have the ordering service organization admin identity in your wallet.
{: note}

1. Go back to the **Ordering service** page and scroll down to the **Ordering service consenters** table.
2. Click the action menu ![action menu](../../icons/overflow-menu.svg) next to the URL, and click **Update orderer certificates**.
3. After you copy and paste the name of the ordering service consenter set, displayed at the top of the side panel, and click **Update consenter**, the base-64 encoded TLS certificate is added to the consenter set of the ordering service system channel.  

**Update the consenter set on the application channel (TLS certificate renewal only)**

Next we have to update the TLS certificate for this ordering node on all application channels that include this ordering node as a consenter. Until you complete these steps and enough channels members approve the generated channel update request, the ordering node is not able to act as a consenter on the channel. Therefore, ensure there are enough other ordering nodes available in the consenter set to maintain quorum while this node is unavailable.

In order to update the application channel, you need to have the channel member admin identity in your wallet.
{: note}

1. From the **Channels** tab, open the application channel that you need to update and click the **Channel details** tab.
2. Scroll down to the **Ordering service consenters** table and click the action menu ![action menu](../../icons/overflow-menu.svg), next to the URL and click **Update orderer certificates**.
3. Copy and paste the name of the consenter set, displayed at the top of the side panel.  
4. To submit this update request to the channel members, you need to select the ordering service MSP from the drop-down list and then the associated orderer organization admin identity from your wallet. When you click **Update consenter** a channel update notification is sent to the consortium members for approval.
5. After enough channel members approve this update according to the channel update policy, the ordering node can resume participation in the consenter set.
6. Repeat these steps for every channel that includes this ordering node as a consenter.

### Considerations for a single node ordering service
{: #cert-mgmt-manual-renewal-os1}

If you are using a single-node ordering service and need to renew the ordering node TLS certificate, additional steps are required. The **Re-enroll** button for the ordering node TLS certificate is only visible when the node is running a Fabric version greater than or equal to v1.4.9, or in the case of a Fabric v2.x node, the version must be greater than or equal to v2.2.1. Likewise, the **Enroll** button is only visible when the Fabric version is greater than v1.4.9 or in the case of a Fabric v2.x image, it must be greater than v2.2.1.

- **Ordering node TLS certificate not expired -** If the ordering node TLS certificate has not expired, you should upgrade the Fabric v1.4.x node to 1.4.9 or Fabric v2.x node to 2.2.1.  Then use the reenroll button which is now visible in the console and follow the normal steps to [Renew ordering node enrollment and TLS certificates](#cert-mgmt-enroll-tls-ordering-node).

To avoid problems, do not mix Fabric v1.4 and v2.x nodes in your Raft cluster. All ordering nodes should be running v1.4.x or all should be running v2.x.
{: important}

- **Ordering node TLS certificate expired -** If the ordering node TLS certificate has expired then the **Enroll** button is blocked. Open a support ticket to address the problem.

## Renew MSP Admin certificate
{: #cert-mgmt-manual-renewal-admin}

Use the following steps to renew an MSP admin certificate.

### Step one: Enroll new identity
{: #cert-mgmt-manual-enroll}

Before an organization admin certificate expires, you must enroll the identity again to generate a new certificate and public key by using the associated CA. These instructions assume that you have the original enroll ID and secret that was specified when the user was originally registered. If the certificates were generated with the console, you can use the following instructions:
1. Open the CA tile. ***
2. Locate the enroll ID for the identity in the table and click **Enroll identity** from the action menu.
3. After you verify that you have selected the correct CA, **Root Certificate Authority**, or **TLS Certificate Authority**, according to the type of certificate that you are renewing, provide the enroll ID and secret that was specified when the user was initially registered. If the enroll ID and secret are not available, follow the same steps for an expired [admin certificate](/docs/blockchain?topic=blockchain-cert-mgmt#ibp-console-identities-expired-certs-admin).
4. Because the certificate and private key are never stored by the console, you must download them and store them securely and **export** the identity.
5. Click **Add identity to wallet**. Wallet identities cannot be replaced,  you need to either delete the existing wallet identity or give this one a different name.

If the certificates were generated from the Fabric CA client or Fabric SDKs, then they need to be renewed where they were generated. You can now use the updated certificates when you follow instructions in subsequent steps.  

If you reach the enrollment limit for an identity, simply use the console to register a new user and enroll an identity and then use the generated certificate in subsequent steps.
{: tip}

**If the CA admin identity expires, you are not able to update any of the identities in the CA table.** And when you open the CA node, there is a message that indicates the users cannot be listed. To fix this problem, click **Associated identity for root CA**. In the side panel that opens, click the **Enroll ID** tab to enroll a new admin identity for the CA admin by providing the original **Enroll ID** and **secret** and a new **display name** for the identity, then click **Associate identity**. You can now proceed with the steps to enroll a new identity for other users in the table by clicking **Enroll identity** from their action menu.

### Step two: Update organization MSP
{: #cert-mgmt-manual-update-msp}

If this is an MSP **admin** identity that was generated after Node OU support was enabled for the MSP, the admin role is inserted directly into the generated admin certificate, and therefore the MSP does not need to be updated. You can skip this step.

If this is an MSP **admin** identity, and the associated MSP Node OU support was disabled when the identity was originally enrolled, then you need to perform the following additional steps:

Before attempting these steps, be aware that these actions trigger a restart of the peer or ordering nodes that are configured with this MSP.
{: important}

1. Open the **Nodes** tab in your console.
2. Locate the node tile that requires the admin certificate update. The MSP name is listed on the tile under the node name. Make a note of the MSP name.
3. Open the **Organizations** tab.
4. Locate the MSP tile for the organization MSP from step two and click the **Export** icon.
5. Open the downloaded MSP JSON file in a text editor.
6. Edit the `admins` element by appending the new base64-encoded certificate string that you generated in the previous section to the end of the list of comma-separated admin certificates. If the identity was enrolled from the console, ensure that the **Root Certificate Authority** was selected. Open the identity JSON file that you exported, and copy the string from the `cert` field.
7. Save your changes.
8. In the **Organizations** tab, open the MSP tile for the peer and click the **Settings** icon.
9. In the side panel, click **Add file** and select the updated MSP JSON file.
10. Notice that the `Node OU` checkbox is selected for you, which means that Node OU support will be enabled on the MSP and that you will not have to repeat this process again _in another year_ when the certificates expire. For now though, you still need to complete all of the steps in this manual renewal process.
    ![Node OU checkbox](images/nodeou-checkbox.png "Node OU checkbox"){: caption="Figure 9. Update MSP with Node OU enabled" caption-side="bottom"}
11. Click **Update MSP definition**. All Peer and ordering nodes in this console that include this MSP as their node admin are automatically updated with the new MSP and restarted.
12. **Export this updated MSP**, and in an out of band action, share the file with all of the members of the network who must import it into their console. It is important for them to import the updated MSP, so when they create a new channel, they are using the MSP definition with the latest admin certificate.  

To verify that the update worked, open the MSP tile and view the updated expiration date.

### Step three: Associate new admin identity on peer or ordering service
{: #cert-mgmt-manual-update-node-identity}

If the updated certificate is an admin certificate for a peer or ordering service, you need to update the node admin identity. Open the associated peer or ordering service and click the **Associate identity for peer** or **Associate identity for ordering service** and browse to the newly created identity from the wallet.

To verify that this update worked successfully on the peer, after you associate the identity, you should be able to see the list of channels that the peer is on and list the smart contracts that are installed on the peer.
{: tip}

### Step four: Update peer organization MSP on application channel  
{: #cert-mgmt-manual-update-channel-peer}

As with all application channel updates, this update needs to be performed by a channel operator and the change will follow the channel update approval process according to the policy that was configured when the channel was created.
{: important}

If the peer organization MSP was not originally created with the Node OU configuration enabled, you also need to update all application channels that include the MSP so that it uses the new admin certificate. When the Node OU configuration is enabled, the MSP uses the value of the OU inside the certificate to determine if the identity is an admin in addition to checking the admin section of the MSP definition.

Complete the following steps for a **peer admin certificate**:

1. Open the channel to be updated in the console.
2. Click **Channel details** and scroll down to **Channel members**.
3. Click the channel member that you want to update.
4. On the **Update MSP definition** panel, select the updated MSP and the **original identity** and click **Update MSP definition**. You must use the original identity for this step because it's currently the only one with permission to make channel updates.  This action replaces the existing peer organization admin MSP definition with the updated MSP that includes the new admin certificate. Now this channel has the newest organization admin certificate.

### Step five: Update channel member on ordering service system channel
{: #cert-mgmt-manual-update-os-channel-member}

*If your cluster does not use a system channel, skip this step. To verify if a cluster uses a system channel, click the cluster tile and check the `Orderer Type`.*

If this is a peer admin MSP that is an ordering service consortium member, the ordering service admin needs to update the ordering service system channel. Because the ordering service system channel serves as a template for new application channels, taking this action keeps it current as the associated MSPs are updated. Before attempting these steps, the ordering service admin should have already imported the updated MSP from step two into their console.

1. From the **Nodes** tab, the ordering service admin opens the ordering service tile.
2. Delete the existing consortium member that contains the original MSP definition, and then add the consortium again with the new admin certificate by clicking **Add organization**.  Browse to the new MSP JSON file from [step two](#cert-mgmt-manual-update-msp).
    ![Update Ordering service consortium member](images/del-add-node.png "Update Ordering service consortium member"){: caption="Figure 10. Ordering service admin MSP update" caption-side="bottom"}

### Step six: Update ordering service admin on ordering service system channel
{: #cert-mgmt-manual-update-os-admin}

*If your cluster does not use a system channel, skip this step. To verify if a cluster uses a system channel, click the cluster tile and check the `Orderer Type`.*

If the updated certificate is for the ordering service MSP admin, you need to update the ordering service.  

1. From the **Nodes** tab, open the ordering service tile.
2. Under **Ordering service administrators** click the **Settings** icon on the ordering service organization MSP tile.
    ![Ordering service admin MSP update](images/os-msp-gear-icon.png "Ordering service admin MSP update"){: caption="Figure 11. Ordering service admin MSP update" caption-side="bottom"}
3. From the **Existing MSP ID** tab, select the MSP that needs to be updated and then select the **original identity** to sign the request. You must use the original identity for this step because it's the only one with permission to make channel updates. This action replaces the existing MSP definition for the ordering service admin with the updated MSP with the new admin certificate.

### Step seven: Update orderer organization MSP on channel  
{: #cert-mgmt-manual-update-channel}

Similar to step four, this update needs to be performed by a channel operator and the change will follow the channel update approval process according to the policy that was configured when the channel was created.
{: important}

If the orderer organization MSP was not originally created with the Node OU configuration enabled, you also need to update all application channels that include the MSP so that it will use the new admin certificate. When the Node OU configuration is enabled, the MSP uses the value of the OU inside the certificate to determine if the identity is an admin in addition to checking the admin section of the MSP definition.

Complete the following steps for an **orderer organization admin certificate:**

Orderer organization admin identities cannot update an application channel. Therefore, a consortium member needs to import the updated orderer organization admin MSP into their console and then complete the following steps:

1. Open the application channel to be updated in the console.
2. Click **Channel details** and scroll down to **Orderer members**.
3. Click the orderer member that you want to update.
4. On the **Update MSP definition** panel, select the updated MSP and click **Update MSP definition**. An update notification request is generated.
5. The orderer organization admin needs to approve the request and submit it to the channel.

## Certificates from an external CA
{: #cert-mgmt-manual-ext-ca}

Some customers prefer to use a third-party, or "external" CA to generate their peer and ordering node enrollment and TLS certificates, as well as the organization admin certificates. But, similar to certificates generated by a Fabric CA, they do expire, and their expiration date needs to be monitored to avoid a network outage caused by their expiry. And like certificates generated by a Fabric CA, expiration dates of certificates from an external CA are visible throughout the console. Before the certificates expire you need to request new certificates from your certificate provider, and then use the console to update them across your network. The process to update them using the console depends on the certificate type.

### Peer and ordering node enrollment and TLS certificates
{: #cert-mgmt-manual-ext-ca-enrollment-tls}

If the certificates are for peer or ordering node enrollment or TLS certificates, they can be updated by completing the following steps:

1. Open the peer or ordering node tile and click the **Settings** icon and then **Update certificates**:
    ![How to update certificates from an external CA](images/ext-ca-certs-update.png "How to update certificates from an external CA"){: caption="Figure 12. How to update certificates from an external CA" caption-side="bottom"}
2. Click **Add certificate** and browse to the updated certificate `.pem` files. You do not have to replace all of them, but if you provide an enrollment certificate, you also need to provide the associated key file. And likewise, if you provide a TLS certificate you also need to provide the associated key.
3. When you click **Update certificates**, the peer or ordering node is restarted. Verify the renewal is successful by examining the updated enrollment and TLS certificate expiration dates.

If you updated the ordering node TLS certificate, the following additional steps are required:  

**Update the consenter set on the ordering service system channel (TLS certificate renewal only)**  

When you update the TLS certificate, you also need to update the consenter set on the ordering service system channel. This action ensures that any new application channels which include this ordering node as a consenter will use the updated TLS certificate.

In order to update the ordering service, you must have the ordering service organization admin identity in your wallet.
{: note}

1. Go back to the **Ordering service** page and scroll down to the **Ordering service consenters** table.
2. Click the action menu ![action menu](../../icons/overflow-menu.svg) next to the URL, and click **Update orderer certificates**.
3. After you copy and paste the name of the ordering service consenter set, displayed at the top of the side panel, and click **Update consenter**, the base-64 encoded TLS certificate is added to the consenter set of the ordering service system channel.  

**Update the consenter set on the application channel (TLS certificate renewal only)**

Next we have to update the TLS certificate for this ordering node on all application channels that include this ordering node as a consenter. Until you complete these steps and enough channels members approve the generated channel update request, the ordering node is not able to act as a consenter on the channel. Therefore, ensure there are enough other ordering nodes available in the consenter set to maintain quorum while this node is unavailable.

In order to update the application channel, you need to have the channel member admin identity in your wallet.
{: note}

1. From the **Channels** tab, open the application channel that you need to update and click the **Channel details** tab.
2. Scroll down to the **Ordering service consenters** table and click the action menu ![action menu](../../icons/overflow-menu.svg), next to the URL and click **Update orderer certificates**.
3. Copy and paste the name of the consenter set, displayed at the top of the side panel.  
4. To submit this update request to the channel members, you need to select the ordering service MSP from the drop-down list and then the associated orderer organization admin identity from your wallet. When you click **Update consenter** a channel update notification is sent to the consortium members for approval.
5. After enough channel members approve this update according to the channel update policy, the ordering node can resume participation in the consenter set.
6. Repeat these steps for every channel that includes this ordering node as a consenter.

### Peer and ordering node organization admin certificates
{: #cert-mgmt-manual-ext-ca-admin}

When organization admin certificates are generated by an external CA, these certificates are [manually added](/docs/blockchain?topic=blockchain-ibp-console-organizations#console-organizations-build-msp) to the organization MSP JSON file. And when they are updated, the certificate inside the JSON file also needs to be updated. Certificate expiration dates depend on the certificate provider but are also visible from the MSP tile in the **Organizations** tab. Follow the same processes that are described in steps [2](#cert-mgmt-manual-update-msp) - [7](#cert-mgmt-manual-update-channel) to update the admin certificate in the MSP, peer, orderer, and channel. If any of the channel member or channel orderer organization admin certs have expired, then you also need to [override the orderer configuration to allow expired certs for channel updates](#ibp-console-identities-expired-certs-orderer-override).

## Bulk admin certificate renewal with Ansible playbooks
{: #cert-mgmt-bulk-ansible}

If your network contains a large number of channels that need to be updated, or if you prefer to automate the process of updating the peer organization MSP or orderer organization MSP on the ordering service, system channel, and application channel, a set of Ansible playbooks is available to script this process. **These playbooks can only be used if the certificates have not expired**. See the playbooks under [Adding an administrator certificate](https://ibm-blockchain.github.io/ansible-collection/tasks/addadmincert.html){: external} for instructions.

## Expired certificates
{: #ibp-console-identities-expired-certs}

When certificates expire, it is likely that peer, orderer, and channel operations will fail, but it is still possible to update the certificates. The process to address them depends on whether they are MSP admin certificates or enrollment certificates.

### How to fix expired organization admin certificates
{: #ibp-console-identities-expired-certs-admin}

If the peer or orderer organization **admin** certificates have expired, you need to generate certificates for a new admin identity. The overall process depends on whether Node OU is enabled for the MSP that the admin certificate belongs to.  

Register a new user and enroll the identity for a new peer organization or orderer organization admin identity with the same CA that the existing peer or orderer organization admin was registered with.

1. Open the peer or orderer organization CA.
2. Click **Register user** and specify a new enroll ID and secret.
3. Select a **Type** of `admin`.
4. Configure the rest of the settings as you did for the original admin identity and click **Register user**.
5. Right-click the action menu for the new user and select **Enroll identity**.
6. Ensure the **Root Certificate Authority** is selected and provide the enroll ID and secret that you specified when you registered the new user.
7. Specify a unique display name for this new identity and export the identity to your wallet.
8. If Node OU support was not enabled when the identity was originally enrolled, then you should use the new exported certificate and follow the manual update steps [2](#cert-mgmt-manual-update-msp) - [7](#cert-mgmt-manual-update-channel) to update the admin certificate in the MSP, peer, orderer, and channel. **Before attempting these steps, review the next section that describes the ordering node override that needs to be configured if any of the channel member or channel orderer organization admin certs have expired.**

#### Override orderer configuration to allow expired certs for channel updates
{: #ibp-console-identities-expired-certs-orderer-override}

After the admin certificate is updated in the MSP, any channels that the MSP belongs to also have to be updated manually. But, if any of the channel member certificates or channel orderer organization admin certificates have expired, you need to override the ordering service configuration to temporarily allow expired certificates.  

Fabric includes a [setting](https://hyperledger-fabric.readthedocs.io/en/release-2.1/raft_configuration.html#certificate-expiration-related-authentication){: external} that allows the channel to ignore expiration checks in order for channel updates to be applied to fix expired certificates. You need to override the `NoExpirationChecks` setting on each ordering node in the ordering service.
1. Open the ordering service and click the **Ordering nodes** tab.
1. Open the first ordering node and click the **Settings** icon.
1. In the side panel, click **Edit configuration JSON (Advanced)**
1. Under **Configuration updates** paste in the following snippet:
    ```
    {
    "General": {
        "Authentication": {
        "NoExpirationChecks": true
        }
    }
    }
    ```
    {: codeblock}

1. Click **Update ordering node**.  
1. Repeat these steps for each ordering node in the ordering service.

Now you are able to update the expired certificates in the channel, see steps [4](#cert-mgmt-manual-update-channel-peer) - [7](#cert-mgmt-manual-update-channel) for details. When you are finished, you need to remove the override by using the same process but this time setting the override snippet to:

    ```json
    {
    "General": {
        "Authentication": {
        "NoExpirationChecks": false
        }
    }
    }
    ```
    {: codeblock}

## Using the command line to view certificate expiration
{: #ibp-console-identities-cli-expiration}

You can also use the command line to check your certificates expiration date. If your certificate was not generated by the console, for example it was generated by using the Fabric CA client or Fabric SDKs, you need to convert certificates that are in `base64` format into PEM format by running the following command on your local system:

```
export FLAG=$(if [ "$(uname -s)" == "Linux" ]; then echo "-w 0"; else echo "-b 0"; fi)
echo <base64_string> | base64 --decode $FLAG > <key>.pem
```
{: codeblock}

Run the following command to display the PEM encoded certificate in a human-readable form:
```
openssl x509 -in <certificate .pem file> -text
```
{: codeblock}

The certificate looks similar to the following example:

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

You can find the expiration date in the **Validity** section and follows `Not After:`. In this example, the certificate expires on November 28, 2019.

## Export an MSP
{: #cert-mgmt-export-msp}

MSPs can be exported from the console to your local file system. Navigate to the **Organizations** tab and click the MSP that you want to export. Click the **Export** icon to download the MSP to a JSON file on your local system.

![How to export MSP](images/export-msp.png "How to export MSP"){: caption="Figure 13. How to export MSP" caption-side="bottom"}

Share the JSON file with all of the members of the network who must import it into their console. It is important for them to import the updated MSP, so that when they create a new channel, they are using the MSP definition with the latest admin certificate.

## Import an MSP
{: #cert-mgmt-import-msp}

When another organization member shares their MSP with you, importing it into your console is important to ensure you are using the latest version of their organization MSP.
1. Navigate to the **Organizations** tab and click **Import MSP definition**.
2. Browse to the MSP JSON file that was shared with you and click **Import MSP definition**.

If you previously imported the MSP into your console, you need to update the definition.
1. On the **Organizations** tab and click the MSP tile.
2. Click the **Settings** icon and then **Add file** to browse to the new JSON file.
3. Click **Update MSP**.  If you created any nodes in your console that use this MSP, they are updated with the new MSP and restarted.
