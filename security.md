---

copyright:
  years: 2014, 2023
lastupdated: "2023-03-07"

keywords: security, encryption, storage, tls, iam, roles, keys

subcollection: blockchain

---



{{site.data.keyword.attribute-definition-list}}



# Security
{: #ibp-security}

{{site.data.keyword.blockchainfull}} Platform provides a scalable, highly reliable platform that helps customers deploy applications and data quickly and securely. This document provides information about securing your {{site.data.keyword.blockchainfull_notm}} Platform service instance, where the blockchain console runs, and best practices for securing the linked Kubernetes cluster where the blockchain nodes are deployed.
{: shortdesc}

## Security on the {{site.data.keyword.blockchainfull_notm}} Platform console
{: #ibp-security-ibp}

**Audience:** Tasks in this section are typically performed by **blockchain network operators**.  

Configuration of an {{site.data.keyword.blockchainfull_notm}} Platform network includes deploying the blockchain console and then linking it to either a new or existing customer Kubernetes cluster. The blockchain console can then be used to create blockchain nodes that reside in the customer Kubernetes cluster.   

Considerations include:
- [IAM (Identity and Access Management)](#ibp-security-ibp-iam)
- [Ports](#ibp-security-ibp-ports)
- [Key management](#ibp-security-ibp-keys)
- [Membership service providers (MSPs)](#ibp-security-ibp-msp)
- [Access control lists (ACLs)](#ibp-security-ibp-acls)
- [API authentication](#ibp-security-ibp-apis)

### IAM (Identity and Access Management)
{: #ibp-security-ibp-iam}


Identity and access management allows the owner of a console to control which users can access the console and their privileges within it. Identity and access management on {{site.data.keyword.cloud_notm}} is controlled by using the `IAM` service. Every user that accesses the blockchain console must have an IBMid account and be assigned an access policy in the IAM service. This policy determines what actions the user can perform within the console. Blockchain-specific permissions (for example, which users can create components) are based on the IAM roles that are mapped to blockchain permissions in this [Role to permissions mapping table](/docs/blockchain?topic=blockchain-ibp-console-manage-console#ibp-console-manage-console-role-mapping).

When the {{site.data.keyword.blockchainfull_notm}} Platform console is provisioned, the email address of the {{site.data.keyword.cloud_notm}} account owner is set as the console administrator, known in IAM as the **Manager** role. All new access requests will be sent to this user. This console administrator can then grant other IBMid users access to the console by using the IAM UI. For more information about IAM on {{site.data.keyword.cloud_notm}}, see [What is IAM](/docs/account?topic=account-iamoverview){: external}. For the steps required to add new users, see [Adding and removing users from the console](/docs/blockchain?topic=blockchain-ibp-console-manage-console#ibp-console-manage-console-add-remove).




### Ports
{: #ibp-security-ibp-ports}


If you are using a client application to send requests to the console, either via the blockchain APIs or the Fabric SDKs, the associated `HTTPS` console port needs to be exposed in your firewall. In {{site.data.keyword.cloud_notm}} use the default port `443`.




### Key management
{: #ibp-security-ibp-keys}

The {{site.data.keyword.blockchainfull_notm}} Platform network is based on trusted identities. Customers use the Certificate Authorities (CAs) in the console to generate the identities and associated certificates that are required by all members to transact on the network. The generated public and private keys are `ECDSA` with Curve `P256`. These keys are stored in the browser when they are added to the member's blockchain wallet so that the console can use them to manage blockchain components. However, it is recommended that customers export these keys and import them into their own key management system in case they clear their browser cache or switch browsers. Customers are responsible for the storage, backup, and disaster recovery of all keys that they export.

Because these public and private key pairs are essential to how the {{site.data.keyword.blockchainfull_notm}} Platform functions, **key management** is a critical aspect of security. If a private key is compromised or lost, hostile actors might be able to access your data and functionality. Although you use the {{site.data.keyword.blockchainfull_notm}} Platform console to generate the certificates and private keys, they are not _permanently_ stored by the browser or the cloud database. Public and private keys are temporarily stored in the browser and added to the member's wallet so that the console can use the private key to digitally sign transactions. Customers are ultimately responsible for exporting the keys and managing their storage, backup, and disaster recovery.

If a private key is lost and cannot be recovered, you will need to generate a new private key by registering and enrolling a new identity with your Certificate Authority. You should also then remove and replace your signCert in any components or organizations where you had used the lost or corrupted identity. See [Updating an organization MSP definition](/docs/blockchain?topic=blockchain-ibp-console-organizations#ibp-console-govern-update-msp) for detailed steps.

In order to secure private keys in a production environment, the {{site.data.keyword.blockchainfull_notm}} Platform includes the option to configure a [Hardware Security Module](#x6704988){: term} (HSM) to generate and store the private keys for a node. An HSM is an optional hardware appliance that performs cryptographic operations and ensures that the cryptographic keys never leave the HSM. You are responsible for configuring an HSM device that implements the [PKCS #11 standard](http://docs.oasis-open.org/pkcs11/pkcs11-base/v2.40/os/pkcs11-base-v2.40-os.html){: external}.  PKCS #11 is a cryptographic standard for secure operations, generation, and storage of keys. You will also need to configure a PKCS #11 proxy to communicate with your HSM. The platform supports ECDSA Sign and Verify cryptographic controls and EC Key generation. When an HSM is implemented for a node, the private key for the node is not stored in the browser wallet. Rather, the private key is accessed from the HSM via the proxy. When you register other node admin or client application identities with a CA by using the console, their private keys are not stored inside the HSM because they need the private key to transact on the network. For instructions on how to use HSM with the {{site.data.keyword.blockchainfull_notm}} Platform, see [Configuring a node to use a Hardware Security Module (HSM)](/docs/blockchain?topic=blockchain-ibp-console-adv-deployment#ibp-console-adv-deployment-cfg-hsm).

You also have the option to bring your own certificates from your own non-{{site.data.keyword.blockchainfull_notm}} Platform CA when you create a peer node or ordering service. If you use your own certificates, you will need to manually build the peer or ordering service MSP definition file that includes those certificates and import the file into the console **Organizations** tab. See [Using certificates from an external CA with your peer or ordering node](/docs/blockchain?topic=blockchain-ibp-console-adv-deployment#ibp-console-adv-deployment-third-party-ca) for the steps required.

### Membership Service Providers (MSPs)
{: #ibp-security-ibp-msp}

Whereas Certificate Authorities generate the certificates that represent identities, turning these identities into roles in the {{site.data.keyword.blockchainfull_notm}} Platform is done through the creation of Membership Service Providers (MSPs) in the console. These MSPs, which structurally are comprised of folders containing certificates, are used to represent organizations on the network. Every organization will have one and only one MSP and will always contain at least one **admincert** that identifies an administrator of the organization. When an MSP is associated with a peer, for example, it denotes that the peer belongs to that organization. Later on in the flow for creating a peer (or any node), this same administrator identity can be used to serve as the administrator of the peer as well. In order to perform some actions on a node, an administrator role is required. For example, to be able to install a smart contract on a peer, your public key must exist in the `admincerts` folder of the peer's organization MSP, which therefore makes you an administrator of the peer organization.

MSPs also identify the root CA that generated the certificates for the organization and any other roles beyond administrator that are associated with the organization (for example, members of a sub-organizational group), as well as setting the basis for defining access privileges in the context of a network and channel (e.g., channel admins, readers, writers).

MSP folders for organization members are based on a Fabric defined structure and are used by Fabric components. For more information about Fabric MSPs and their structure, see the  [Membership](https://hyperledger-fabric.readthedocs.io/en/release-2.2/membership/membership.html){: external} and [Membership Service Provider Structure](https://hyperledger-fabric.readthedocs.io/en/release-2.2/membership/membership.html#msp-structure){: external} topics in the Hyperledger Fabric documentation. The Fabric CA establishes this structure by creating the following folders inside the MSP definition:

| MSP folder name | Description |
|-------------------------|-------------|
| `cacerts` | Contains the certificate of the root CA of your network.|
| `intermediatecerts` | These are the certificates of any intermediate CAs in your chain of trust (leading back to a root CA or CAs). Because intermediate CAs are currently not supported by the {{site.data.keyword.blockchainfull_notm}} Platform, this field will be blank.|
| `keystore` | Contains the private key that was generated alongside your public key. This key is used to generate signatures by creating a cryptographic hash that can be verified using the public key known to other users. This key is never shared, and you are responsible for securing and managing it. If this key becomes compromised, it can be used to impersonate your identity, making it crucial to keep this key safe. |
| `signCerts`| Contains the public key that was generated alongside your private key. It is also known as a "signing certificate" because it is used to verify signatures generated by other users.|
| Many Fabric components contain additional information inside their MSP folder. For example, a peer, includes the following folders: ||
| `admincerts` | Contains the admin certificates for this organization or component. |
| `tls` | Contains the TLS certs that you store for communicating with other network components. |
{: caption="Table 1. MSP folders" caption-side="bottom"}

Note that organization MSPs are stored in browser storage and must be exported to a local file system and secured by the customer.

### Access control lists (ACLs)
{: #ibp-security-ibp-acls}

Hyperledger Fabric allows for finer grained control over user access to specified resources through the use of access control lists (ACLs). ACLs allow access to a channel resource to be restricted to an organization and a role within that organization. The available set of ACLs are from the underlying Fabric architecture and are selected during channel creation or update. Note that access control lists are restrictive, rather than additive. If access to a resource is specified to an organization, it means that **only that organization** will have access to the resource. For example, if the default access to a particular resource is the Readers of all organizations, and that access is changed to the Admin of Org1, then only the Admin of Org1 will have access to the resource. Because access to certain resources is fundamental to the smooth operation of a channel, it is highly recommended to make access control decisions carefully. If you decide to limit access to a resource, make sure that the access to that resource is added, as needed, for each organization.

You can use the blockchain console to select which ACLs to apply to resources on a channel. See this information under [Creating a channel](/docs/blockchain?topic=blockchain-ibp-console-build-network#ibp-console-build-network-channels-create) for instructions on how to configure access control for a channel.

### API authentication
{: #ibp-security-ibp-apis}

In order to use the blockchain [APIs](https://cloud.ibm.com/apidocs/blockchain){: external} to create and manage network components, your application needs to be able to authenticate and connect to your network. See this topic on [API Authentication on {{site.data.keyword.cloud_notm}}](/docs/blockchain?topic=blockchain-ibp-v2-apis#ibp-v2-apis-authentication) 

## Best practices for security on the customer Kubernetes cluster
{: #ibp-security-Kubernetes}

**Audience:** Tasks in this section are typically performed by **Kubernetes infrastructure managers**.

The {{site.data.keyword.blockchainfull_notm}} Platform console allows you to deploy and manage nodes on a Kubernetes cluster that you operate. The previous section addressed the security of the console. The following sections detail the best practices you can use to secure your Kubernetes cluster and the nodes of your network:

- [Kubernetes cluster security](#ibp-security-Kubernetes-security)
- [Network security](#ibp-security-Kubernetes-network)
- [Internet Ports](#ibp-security-Kubernetes-ports)
- [Cluster and Operating System (OS)](#ibp-security-Kubernetes-container-os)
- [Keys and cluster access information](#ibp-security-Kubernetes-keys)
- [Membership Service Providers (MSPs)](#ibp-security-kubernetes-msp)
- [Storage](#ibp-security-kubernetes-storage)
- [Data privacy](#ibp-security-kubernetes-privacy)
- [GDPR](#ibp-security-kubernetes-gdpr)

### Kubernetes cluster security
{: #ibp-security-Kubernetes-security}

The best place to start is to learn about the security features of the underlying Kubernetes infrastructure. The open source documentation provides a review of recommended practices for [securing a Kubernetes cluster](https://Kubernetes.io/docs/tasks/administer-cluster/securing-a-cluster/){: external}.


If you are using {{site.data.keyword.cloud_notm}}, you can review the topic on Security for the [{{site.data.keyword.cloud_notm}} Kubernetes Service](/docs/containers?topic=containers-security){: external} or [Red Hat OpenShift on {{site.data.keyword.cloud_notm}}](/docs/openshift?topic=openshift-security).




### Network security
{: #ibp-security-Kubernetes-network}


{{site.data.keyword.cloud_notm}} provides the underlying network, including the networks and routers, over which customers’ VLAN resides. The customer configures their servers and uses gateways and firewalls to route traffic between servers to protect workloads from network threats. Protecting your cloud network by using firewalls and intrusion prevention system devices is imperative for protecting your cloud-based workloads.





#### Firewall configuration
{: #ibp-security-Kubernetes-network-firewall}

Kubernetes clusters should be secured by a firewall to protect the network from unauthorized access from internet traffic. By default {{site.data.keyword.cloud_notm}} Kubernetes service clusters are preconfigured with a Calico network plug-in that secures the public network interface of every worker node in the cluster. By configuring Kubernetes and Calico network policies you can easily control the inbound and outbound network traffic. For more information, see [Controlling traffic with network policies](/docs/containers?topic=containers-network_policies). In addition, you can also configure Edge worker nodes on your {{site.data.keyword.cloud_notm}} Kubernetes service clusters to restrict the worker nodes that are exposed to the public internet. See [Restricting network traffic to edge worker nodes](/docs/containers?topic=containers-edge) to learn more.  If your configuration includes a network firewall, there are ports that need to be exposed to allow network traffic through. The following section describes how to find the ports that can be exposed and what they are for.

### Internet Ports
{: #ibp-security-Kubernetes-ports}

{{site.data.keyword.blockchainfull_notm}} Platform exposes certain ports associated with each node type that are used by client applications, for example, when sending transactions to peers or the ordering service. If you have configured a firewall, you will need to expose these ports in your network for the transactions to reach their destination and for the nodes to be able to respond to the requests. {{site.data.keyword.blockchainfull_notm}} Platform supports the `HTTPS` IP Security protocol.

If your {{site.data.keyword.blockchainfull_notm}} Platform instance is linked to an **OpenShift** cluster in {{site.data.keyword.cloud_notm}}, port `443` needs to be exposed.  

If your {{site.data.keyword.blockchainfull_notm}} Platform instance is linked to an **{{site.data.keyword.cloud_notm}} Kubernetes service** cluster, use the following table to identify the ports that need to be exposed:

| Port | Number | Description |
|----------|---------|-------|
| **CA ports**|||
| Fabric CA | 7054 |Port 7054 is exposed from the CA container. This is exposed all the way to the customer application via the service.|
| **Peer ports**|||
| Peer gRPC API | 7051 | This is the port used by the clients to communicate with the peer. |
| Smart contract containers | 7052 | This is the port used by the smart contract containers to communicate with the peer.|
| Peer Operations | 9443 | This port is used to get to the health check endpoint and to the metrics endpoint of the peer.|
| gRPC Web proxy | 7443 | This port is used by the UI to communicate with the peer via gRPC web APIs. |
| **Ordering service ports** ||||
| Orderer gRPC API | 7050 | This is the port used by the clients to communicate with the orderer. |
| Orderer Operations | 8443 | This port is used to get to the health check endpoint and to the metrics endpoint of the Orderer.|
| gRPC Web proxy | 7443 | This port is used by the UI to communicate with the orderer via gRPC web APIs.|
{: caption="Table 2. Ports on {{site.data.keyword.cloud_notm}}" caption-side="top"}


### Cluster and Operating System security
{: #ibp-security-Kubernetes-container-os}

- **Sensitive data:** Cluster configuration data is stored in the `etcd` component of your Kubernetes master. Data in `etcd` is stored on the local disk of the Kubernetes master and is backed up to {{site.data.keyword.cos_full_notm}}. Data is encrypted during transit to {{site.data.keyword.cos_full_notm}}. 


- **UBI Linux:** The Fabric Docker images use [Red Hat UBI images](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/8/html-single/building_running_and_managing_containers/index#how_are_ubi_images_different/){: external}, which is a smaller, lighter, and more secure version of Linux.

### Keys and cluster access information
{: #ibp-security-Kubernetes-keys}


| Type | Description |Storage | Access |
|------|-------------|--------|--------|
|**Customer Kubernetes cluster access information**| In order for the service components to access and manage the components that are deployed into this cluster, the service components generate a Kubernetes secret for **cluster** access. Each Kubernetes secret is tied to that specific customer cluster. | After generation, these secrets never leave the IBM Kubernetes cluster where the service components are running and are deleted whenever a customer deletes the associated cluster. | Accessed only by the service component code and only in response to customer requests to deploy or manage components via the blockchain console. Developers of the service do not have access to the information in the secrets. |
{: caption="Table 3. Types of keys and information used to access the customer Kubernetes cluster" caption-side="top"}

### Membership Service Providers (MSPs)
{: #ibp-security-kubernetes-msp}

Organizations in a blockchain network are represented by [MSP](/docs/blockchain?topic=blockchain-glossary#glossary-msp) definitions. You can use the blockchain console to add a new organization to the network by creating a new MSP definition in the **Organizations** tab. If you are the admin of an ordering service, you can use the console to add that organization MSP to a consortium using the **Ordering service** tab. Finally, if you are an administrator of a channel, you can add that organization to an existing channel so the organization can transact on the network using the **Channels** tab (this task might require the signature of other organizations). See the topic on [Join the consortium hosted by the ordering service](/docs/blockchain?topic=blockchain-ibp-console-build-network#ibp-console-build-network-add-org) for detailed steps.

### Storage
{: #ibp-security-kubernetes-storage}

When the blockchain console deploys a node, storage is dynamically provisioned from the default storageclass for that node from persistent storage. You have the option of encrypting the persistent volume but there may be some performance implications with encryption to consider.  

Customers are responsible for encrypting their own storage and the encryption must occur before any blockchain components are deployed to the cluster.
{: important}


The default persistent storage type is File storage, also known as Endurance storage. For more information about encryption on all of the {{site.data.keyword.cloud_notm}} storage options:
- [{{site.data.keyword.cloud_notm}} File storage Provider managed encryption-at-rest](/docs/FileStorage?topic=FileStorage-mng-data#provisioning-storage-with-encryption){: external}
- [Portworx encrypting volumes](https://docs.portworx.com/reference/cli/encrypted-volumes/){: external}
- [{{site.data.keyword.cloud_notm}} data encryption and key management](https://www.ibm.com/cloud/architecture/architectures/data-security-archt){: external}




### Data privacy
{: #ibp-security-kubernetes-privacy}

When you have data privacy requirements, [Private Data collections](https://hyperledger-fabric.readthedocs.io/en/release-2.2/private-data/private-data.html#what-is-private-data){: external} provide a way to further isolate specific data from the rest of the channel members. The combination of the use of channels and private data offer various solutions for achieving [Data Residency](/docs/blockchain?topic=blockchain-console-icp-about-data-residency).

### GDPR
{: #ibp-security-kubernetes-gdpr}

In order to be GDPR compliant, it is recommended that you store PII data off chain.

## Hyperledger Fabric Security
{: #ibp-security-kubernetes-fabric}

Because {{site.data.keyword.blockchainfull_notm}} Platform is based on Hyperledger Fabric, you can leverage the secure features included in a Fabric network.  

- **TLS v1.2 communications** TLS is embedded in the trust model of Hyperledger Fabric. By default, server-side TLS is enabled for all communications using TLS certificates. TLS is used to encrypt the communication between your nodes and between your nodes and your applications. TLS prevents man-in-the-middle and session hijacking attacks. All {{site.data.keyword.blockchainfull_notm}} Platform components use TLS to communicate with each other.

- **Transaction integrity:** Fabric uses the cryptographic ECDSA standard to guarantee transaction integrity. With ECDSA, the transaction originator, such as a client application, signs their message by using their private key, and the recipient, such as a peer, uses the originator’s public key to verify the authenticity of the message. If a transaction is tampered with on its way to the recipient, the signature verification fails.

- **Channels:** While Fabric channels are a powerful mechanism for partitioning and isolating data, they also provide the primary foundation for data privacy. Only members of the same channel can access the data of this channel. To ensure channel security, the channel configuration update policy is configured to define the number of channel operators who need to agree on the channel update request before a channel can be updated. Therefore, a clear process must exist for defining the organizations that are allowed to join and update channel.

- **Ledger data:** Implicit in the blockchain permissioned network is the notion that an agreed upon policy of multiple endorsers is required to sign (approve) a transaction before it can be committed to the ledger. Before any information can be added to the ledger, a clear and well-established process for defining the ledger information must exist. Data on the ledger is immutable.

- **Smart contracts:** All smart contracts should be reviewed by channel members before they are installed and executed on peers in their organization. Likewise, all updates to smart contract should be reviewed before the updates are applied to a peer. See this topic on [Upgrading a smart contract](/docs/blockchain?topic=blockchain-ibp-console-smart-contracts-v14#ibp-console-smart-contracts-upgrade) for the steps that are required.

- **HSM integration:** After you have configured an HSM for your environment, you have the option of configuring your nodes to use the HSM to generate and store private keys. See [Configuring a CA, peer, or ordering node to use the HSM](/docs/blockchain?topic=blockchain-ibp-console-adv-deployment#ibp-console-adv-deployment-cfg-hsm) for more information.

## Enable network policy
{: #ibp-security-enable-network-policy}

Enable network policy will automatically install when the console is created. In the operator deployment specification, the operator needs to add an environment variable `IBPOPERATOR_CONSOLE_APPLYNETWORKPOLICY` and set its value to `true`.

```
kubectl edit deploy ibp-operator -n <offering-namespace>
```

In the environment section under `spec.containers`, add the following:

```
- name: IBPOPERATOR_CONSOLE_APPLYNETWORKPOLICY
   value:"true"
```

When the option above is enabled, the operator will install two network policies during the console installation. These network policies are meant to give basic defensive security mechanism, and opens only the ports that the offering uses. This by no means is a policy that can replace a firewall. The policies applied provide ingress security, as blockchain network may require to call npm, gradle, maven servers to get chaincode modules as well as to communicate with the orderers/peers/applications running outside the cluster on internet we do not provide egress policies.

The following two policies are applied:

1. Deny-all-ingress.

This policy denies all ingress (`ingress: []`) network traffic to the pods (`podSelector: {}`) in the namespace it applies to. This ensure only the needed traffics go through.

```
    kind: NetworkPolicy
    apiVersion: networking.k8s.io/v1
    metadata:
      name: networkpolicy-denyall
      namespace: <>
      labels:
        type: "ibp-console"
        app.kubernetes.io/name: "ibp"
        app.kubernetes.io/instance: "ibp-console"
        app.kubernetes.io/managed-by: "ibp-operator"
        release: "operator"
        helm.sh/chart: "ibp"
    spec:
      podSelector: {}
      ingress: []
```

2. Ingress.

This ingress network policy applies to network traffic coming from anywhere (`from: [])`. It applies to all pods that labels `app.kubernetes.io/name: "ibp"` which are all the offering related pods. This policy only opens the ports that are required for the blockchain components and the available management console from outside for them to connect to each other.

```
    kind: NetworkPolicy
    apiVersion: networking.k8s.io/v1
    metadata:
      name: networkpolicy-ingress
      labels:
        type: "ibp-console"
        app.kubernetes.io/name: "ibp"
        app.kubernetes.io/instance: "ibp-console"
        app.kubernetes.io/managed-by: "ibp-operator"
        release: "operator"
        helm.sh/chart: "ibp"
    spec:
      ingress:
      - from: [] # everywhere
        ports:
        - port: 7051 # peer-api
          protocol: TCP
        - port: 9443 # peer-operations / ca-operations
          protocol: TCP
        - port: 7443 # peer-grpcweb / orderer-grpcweb
          protocol: TCP
        - port: 7052
          protocol: TCP # peer-chaincode
        - port: 3000 # optools
          protocol: TCP
        - port: 7050 # orderer-grpc
          protocol: TCP
        - port: 8443 # orderer-operations
          protocol: TCP
        - port: 22222 # fileserver #check install/invoke chaincode
          protocol: TCP
        - port: 11111 # grpc #check install/invoke chaincode
          protocol: TCP
        - port: 7054 # ca
          protocol: TCP
      podSelector:
        matchLabels:
          app.kubernetes.io/name: "ibp"
      policyTypes:
        - Ingress
```
