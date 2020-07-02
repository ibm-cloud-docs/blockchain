
---

copyright:
  years: 2019, 2020
lastupdated: "2020-06-09"

keywords: high availability, CA, PostgreSQL, replica sets, zone

subcollection: blockchain

---

{:external: target="_blank" .external}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:note: .note}
{:important: .important}
{:tip: .tip}
{:pre: .pre}

# Building a high availability Certificate Authority (CA)
{: #ibp-console-build-ha-ca}




Because redundancy is required to achieve high availability (HA) networks, you can use the {{site.data.keyword.blockchainfull}} Platform console to configure a CA for high availability.
{:shortdesc}

**Target audience:** This topic is designed for network operators who are responsible for creating, monitoring, and managing the blockchain network.


If you are using {{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}}, HA CA configuration is only available in paid Kubernetes clusters on {{site.data.keyword.cloud_notm}}.
{: important}


## Configuring CA replica sets
{: #ibp-console-build-ha-ca-replica-sets}

When a Kubernetes pod becomes unavailable, Kubernetes immediately attempts to restart it. However, if your SLA requires zero downtime for your CA, you can configure replica sets for your CA. A replica set is a Kubernetes mechanism that is used to guarantee the availability of the pod. The use of replica sets ensures that multiple replicas of the pod are running at all times. If one of the CA replicas becomes unavailable, Kubernetes immediately switches to another CA replica, therefore there is no downtime while you wait for the pod to restart. The replica uses data from the same shared database so that the CA can continue to seamlessly process requests. If CA replica sets are required for your business, the {{site.data.keyword.blockchainfull_notm}} Platform requires the CA to use an instance of a `PostgreSQL` database. Thus, before you can configure CA replica sets, you are responsible for configuring an instance of the `PostgreSQL` database.

### Deploying a PostgreSQL database
{: #ibp-console-build-ha-ca-postgresql}

`PostgreSQL` is a widely available open source relational database management system. The CA uses the database to store certificates (public keys only) and identities that are registered with the CA. Customers need to provision their own PostgreSQL database instance in {{site.data.keyword.cloud_notm}} or from a third-party provider. When you deploy a new CA by using the blockchain console or APIs, you need to provide the connection information for the database. Each CA needs its own PostgreSQL database.

## Considerations
{: #ibp-console-build-ha-ca-considerations}

- CA replica sets are only possible when the PostgreSQL database is selected for the CA. The SQLite database is not supported for use with CA replica sets.
- Because you cannot switch a CA database from SQLite to PostgreSQL, only new CAs that use PostgreSQL can be configured to be HA. Likewise, after a CA is deployed and configured to use a PostgreSQL database, it cannot be reconfigured to use a different PostgreSQL instance or later configured to use SQLite.
- CA replica sets are automatically spread across your worker nodes in a _single zone_ according to available resources. Replica sets are not available across zones.
- Because creating a CA replica set results in a new CA pod, when you configure an additional replica set, the CPU and memory requirements for the CA are doubled. If three replica sets are required, then the total CPU and memory requirements triple, and so on. However, the storage allocation does not change because all of the replica sets use the same PostgreSQL database. When you configure your CA, you can specify how many replica sets to create.
- The number of replica sets can be scaled up or down over time according to your availability needs.
- Replica sets are only available when PostgreSQL is selected as the CA database.
- The ability to deploy a CA to a specific Kubernetes cluster zone is only available when SQLite is selected as the CA database.

## Before you begin
{: #ibp-console-build-ha-ca-before}

You need to configure an instance of a PostgreSQL database. There are two options available in {{site.data.keyword.cloud_notm}}:
  - [Databases for PostgreSQL](https://cloud.ibm.com/catalog/services/databases-for-postgresql){: external}. See the [Getting Started tutorial](/docs/databases-for-postgresql?topic=databases-for-postgresql-getting-started){:external} for information on provisioning an instance of the service.
  - [Hyper Protect DBaaS for PostgreSQL](https://cloud.ibm.com/catalog/services/hyper-protect-dbaas-for-postgresql){: external}. See the tutorial on [Getting started with IBM Cloud Hyper Protect DBaaS for PostgreSQL](/docs/hyper-protect-dbaas-for-postgresql?topic=hyper-protect-dbaas-for-postgresql-gettingstarted) for setup instructions.  

Use of a third-party PostgreSQL database is also supported.  If the third-party database is configured to use TLS, you will need to provide at least one server certificate. If SSL client authentication (mutual TLS) is enabled, you also need to provide a client certificate and a client key in .pem format.  More information on how to provide this information is provided in the steps that follow.

##  Creating the PostgreSQL connection file
{: #ibp-console-build-ha-ca-connx}

For all three PostgreSQL database options, you need to provide a file that contains the connection information to the PostgreSQL database when you create the CA. The process to generate the file depends on whether the database resides in {{site.data.keyword.cloud_notm}} or is from third-party provider.

- **{{site.data.keyword.cloud_notm}} Databases for PostgreSQL** If you are using a PostgreSQL database from {{site.data.keyword.cloud_notm}} Databases for PostgreSQL, you need to generate Service Credentials from the PostgreSQL resources page in your **{{site.data.keyword.cloud_notm}} Databases for PostgreSQL** dashboard by completing the process in the following clip:

   ![Service credentials](../images/service_credentials_postgresql.gif "Service credentials"){: gif}

   Save the credentials that you copied to a file of type JSON on your local system. When you create your CA, you need to provide this file.

- **{{site.data.keyword.cloud_notm}} Hyper Protect DBaaS for PostgreSQL** If you are using a PostgreSQL database from {{site.data.keyword.cloud_notm}} Hyper Protect DBaaS for PostgreSQL, then you need to manually build the JSON file by using the following template:

 ```json
 {
    "connection": {
       "postgres": {
          "hosts": [
             {
                "hostname": "db_hostname",
                "port": "db_port"
             }
          ],
          "database": "db_name",
          "authentication": {
             "username": "db_admin_username",
             "password": "db_admin_password"
          },
          "query_options": {
             "sslmode": "verify-full"
          },
          "certificate": {
             "certificate_base64": "ssl_root_cert"
          }
       }
    }
 }
 ```
 {: codeblock}

 Enter the corresponding `hostname`, `port`, `database`, `username`, `password`, and `certificate_base64` values for your PostgreSQL database:
   - `hostname` and `port` can be found in the **Connect to database** panel of the Hyper Protect DBaaS for PostgreSQL dashboard.
   - `database` is the value that you enter for the **Cluster name** field when you provision the service instance. Note that hyphen (`-`), space, and `.db` are not allowed in the name.
   - `username` and `password` are the values you enter for the **Database admin name** and **Database admin password** fields when you provision the service instance.
   - `certificate_base64` is the one-row string **base64** format of the certificate value in the CA file, which you can download from the **Connect to database** panel of the Hyper Protect DBaaS for PostgreSQL dashboard. Ensure to convert the certificate value to one row.

 Save this information to a JSON file on your local system so that is can be provided when you create your CA.

- **Third-party provider** If you are using a third-party provider for your PostgreSQL database, then you need to manually build the JSON file by using the following template:

  ```json
  {
     "connection": {
        "postgres": {
           "hosts": [
              {
                 "hostname": "db_hostname",
                 "port": 30000
              }
           ],
           "database": "db_name",
           "authentication": {
              "username": "db_user",
              "password": "db_password"
           },
           "query_options": {
              "sslmode": "verify-full"
           },
           "certificate": {
              "certificate_base64": "tls_certificate"
           },
           "client": {
              "certificate_base64": "client_tls_certificate",
              "private_key_base64": "client_tls_private_key"
           }
        }
     }
  }
  ```
  {: codeblock}

Enter the corresponding `hostname`, `port`, `database`, `username`, and `password` values for your PostgreSQL database.  

Valid values for `sslmode` are listed in the [Hyperledger Fabric CA PostgreSQL documentation](https://hyperledger-fabric-ca.readthedocs.io/en/release-1.4/users-guide.html#postgresql){: external}.

The `certificate` and `client` fields are required only if you are using TLS to encrypt communications to the PostgreSQL database. All certificate strings must be base64 encoded.

If you are not using TLS, you need to remove the `certificate` and `client` elements from your JSON file.  
{: important}

- If the third-party database is configured to use TLS, you will need to provide at least one server `certificate`.
- If SSL client authentication (mutual TLS) is enabled, you will need to provide the `certificate` as well as the client certificates,  `certificate_base64`  and `private_key_base64`.  

Save this information to a JSON file on your local system so that is can be provided when you create your CA.

## Creating an HA CA
{: #ibp-console-build-ha-ca-create}

Create a new CA by using the {{site.data.keyword.blockchainfull_notm}} Platform console.

1. Navigate to the **Nodes** tab on the left and click **Add Certificate Authority**.
2. Make sure that the option to **Create** a Certificate Authority is selected. Then click **Next**.
3. Give your CA a **display name** and enter your CA admin credentials by specifying a **CA administrator enroll ID and secret**.
4. Click the **Database and replica sets selection** checkbox under **Advanced deployment options** and click **Next**. If your network is on {{site.data.keyword.cloud_notm}}, this option is only available on paid clusters.
5. To use replica sets, select `PostgreSQL` as the CA database.
6. Click **Add file** and browse to the JSON file you created with the database connection information.
7. Choose the number of replica sets you need. Two replica sets ensure that if one CA replica becomes unavailable, the other is always immediately ready to process requests. Three replica sets provide even greater redundancy. If two of the three replica sets are unavailable, the third is ready to process requests. Because each additional replica set requires additional CPU and memory, you need to ensure you have adequate resources available to accommodate the number you choose. This value can be updated later as well.
8. You have the opportunity to configure resource allocation for the node. The resources that you specify here are used for each replica set.  If you want to learn more about how to allocate resources in {{site.data.keyword.cloud_notm}} for your node, see this topic on [allocating resources](/docs/blockchain?topic=blockchain-ibp-console-adv-deployment#ibp-console-adv-deployment-allocate-resources).
9. Review the Summary page, then click **Add certificate authority**.

