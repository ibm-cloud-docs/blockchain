---

copyright:
  years: 2017, 2020
lastupdated: "2020-06-02"

keywords: Swagger APIs, create a network, join a network, authentication credential

subcollection: blockchain

---

{:external: target="_blank" .external}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:gif: data-image-type='gif'}

# Creating or joining a network with Swagger APIs
{: #swagger-network}

{{site.data.keyword.blockchainfull}} Platform exposes a number of REST APIs that you can use to create or join a blockchain network on {{site.data.keyword.cloud_notm}}. You can try these APIs by using the [Swagger UI](/docs/blockchain?topic=blockchain-ibp-swagger#ibp-swagger) that is associated with your network.
{:shortdesc}


## Retrieving basic auth credential for API
{: #swagger-network-retrieve-id-token}

Before you begin, you need to create an [{{site.data.keyword.blockchainfull_notm}} Platform service instance](https://cloud.ibm.com/catalog/services/ibm-blockchain-5-prod){: external} with Enterprise Plan in {{site.data.keyword.cloud_notm}}.

To use Swagger APIs to create or join a network, you need a basic authentication credential to ensure that you have access to the service instance in {{site.data.keyword.cloud_notm}}.

1. In your [{{site.data.keyword.cloud_notm}} dashboard](https://cloud.ibm.com/resources){: external}, open the service instance that you created.
2. Click **Service credentials** from the left navigator.
3. Click the "New Credential" button on the **Service credentials** page to create a new credential.
    1. Give the credential a name, for example, *CreateJoin*.
    2. Enter **{"type": "service_instance_token"}** in the "Add inline configuration parameter" field.
    3. Click the **Add** button.
    ![Retrieve service credentials](../images/service_credentials.gif "Retreive service credentials"){: gif}
4. After the new credential is created, click **View credentials** under the **ACTIONS** header of this credential. The contents of the credential looks similar to the following example:

    ```
    {
      "service_instance_id": "60cc757d-1234-7866-9893-491cec2d0eaa",
      "service_instance_token": "T8-W0rHv_vEya63P8KcVUwxAXXbDmihGcDRD5AcHVXGn6S2jbxx2LikPz8w26c3k",
      "description": "This token can be used with the IBP APIs to create or join a network. It can only be used once."
    }
    ```

    The `service_instance_id` serves as the basic auth user id and the `service_instance_token` serves as the basic auth password. Use these values as the basic authentication credential when you call the **Create network** or **Join network** APIs.

    If you create a blockchain network before you follow the preceding steps to create a service credential, the contents of the credential also includes the network information and looks similar to the following example:

    ```
    {
      "org1": {
        "url": "https://ibp-sp.us-south.ibm-blockchain-5-prod.cloud.ibm.com",
        "network_id": "n5edf19260c998940c5933daca2be76974a",
        "key": "org1",
        "secret": "A8YeuUuNvKVbN5cVGOlwprpaaHCVxYiP0uEN0fviQB2W9_ldJLtMSOSHqkUpkqysMA"
        }
    }    
    ```

    **Note**: For the **Invite** API, the `key` serves as the basic auth user id and the `secret` serves as the basic auth password.


## Checking available network locations
{: #swagger-network-check-location}

You can use APIs to create blockchain networks in only available network locations. Before you create a network, use the following API to get a current list of available network locations. No credentials are required to run this API.

```
https://ibp-ep.us-south.ibm-blockchain-5-prod.cloud.ibm.com/api/v1/network-locations/available
```
{:codeblock}

A list of available network locations is returned that is similar to the following example:

```
{
  "DAL": {
    "location_id": "DAL",
    "description": "Dallas",
    "status": "available"
    "swagger_url": "https://ibmblockchain-v2-dal.4.secure.blockchain.ibm.com/api-docs"
  },
  "FFT": {
    "location_id": "FFT",
    "description": "Frankfurt",
    "status": "available"
    "swagger_url": "https://ibmblockchain-v2-fft.2.secure.blockchain.ibm.com/api-docs"
  },
  "TOR": {
    "location_id": "TOR",
    "description": "Toronto",
    "status": "available"
    "swagger_url": "https://ibmblockchain-v2-tor.1.secure.blockchain.ibm.com/api-docs"
  }
}
```

If you plan to create a network, select the location where you would like to create your network from the list returned by the API. Note the ``location_id`` and ``swagger_url`` are associated with that location.

If you plan to join a network, make a note of the ``swagger_url`` associated with the ``location_id`` specified in your invitation e-mail.

The ``swagger_url`` represents the api endpoint that you will use when you create or join a network using the APIs below.


## Creating a network

If you use Enterprise Plan, you need to complete two steps to create a network with APIs.

1. Create a blockchain service instance on {{site.data.keyword.cloud_notm}} with Enterprise Plan.  Retrieve your service instance ID and token as the basic auth username and password. For more information, see [Retrieving basic auth credential for API](/docs/blockchain?topic=blockchain-swagger-network#swagger-network-retrieve-id-token).

2. Call the **Create network** API using these service credentials. Issue this API against the api `swagger_url` retrieved from [Checking available network locations](/docs/blockchain?topic=blockchain-swagger-network#swagger-network-check-location). Navigate to the ``swagger_url link`` to use the Swagger UI to issue the Create Network API, or programmatically issue the command using the URL address without ``/api-docs``. For example,

    ```
    https://ibmblockchain-v2-tor.1.secure.blockchain.ibm.com/api/v1/networks
    ```
    {:codeblock}

**Parameters**:
- `location_id`: The ID of an available network location. Specify the value of the `loation_id` noted from [Checking available network locations](/docs/blockchain?topic=blockchain-swagger-network#swagger-network-check-location).
- `company_name`: Your identifier as a member in the network.
- `email`: Your email address to receive notifications.
- `peers`: : Number of peers that you want to create for this member. Valid values are 0 - 6. You can also create peers for your member later in the Network Monitor UI.
- `ledger_type`: The type of ledger of this network. The valid values are levelDB and couchDB. **levelDB** is the default value.


## Inviting new members to a network

After you create a blockchain network, you can invite other members to join your network. You need to specify the ID of the network that you want to invite new members to join. The basic auth credential that is required to invite a member is different than the one that is used in the **Create network** API.  You can get the credential to invite a member with the **Retrieve network credentials** API on the [Swagger UI](/docs/blockchain?topic=blockchain-ibp-swagger#ibp-swagger-retrieving-network-credentials) or [retrieve basic auth information for API](/docs/blockchain?topic=blockchain-swagger-network#swagger-network-retrieve-id-token) from your service instance in {{site.data.keyword.cloud_notm}}.

```
/networks/{networkID}/invite
```
{:codeblock}

**Parameters**:
- `email`: The email address of the member to invite to the network.
- `company_name`: The identifier you define for the member to join the network. Invited members can change their identifiers when they join the network.

The invited member will receive an email invitation with instructions on how to join your network.


## Joining a network

If you are invited to join a blockchain network, you will receive a network invitation email which includes the `location_id` and `network id`.

1. Before joining the network, you need to create an {{site.data.keyword.blockchainfull_notm}} Platform service instance and retrieve your service instance ID and token as the basic auth username and password. For more information, see [Retrieving basic auth information for API](/docs/blockchain?topic=blockchain-swagger-network#swagger-network-retrieve-id-token).

2. [Check the available network locations](/docs/blockchain?topic=blockchain-swagger-network#swagger-network-check-location) to get the `swagger_url` for the `location_id` in your invitation e-mail. It will look something like:

    ```
    https://ibmblockchain-v2-tor.1.secure.blockchain.ibm.com/api-docs
    ```
    {:codeblock}

3. Navigate to your `swagger_url` to use the Swagger UI to issue the Join API, or programmatically submit the Join request using the `swagger_url`. Replace `/api-docs` with  ``/api/v1/networks/[network_id]]/join``
and fill in the `network_id` by using the value from your invitation email. The resulting url would look similar to:

    ```
    https://ibmblockchain-v2-tor.1.secure.blockchain.ibm.com/api/v1/networks/56102acee0e4487889ef09db681bada0/join
    ```
    {:codeblock}

    **Note**: For the **Join** API, use the service instance ID and token that you retrieve in step 1 as the basic auth username and password.

**Parameters**:
- `company_name`: Your identifier as a member in the network. This will replace the name the inviter assigned.
- `email`: Your email address to receive notifications.  This should match the email address in the invite notification.
- `peers`: Number of peers that you want to create for this member. Valid values are 0 to 6. You can also create peers for your member later in the Network Monitor.
