---

copyright:
  years: 2018, 2020
lastupdated: "2019-06-18"

keywords: Swagger APIs, authorize, service credentials, disable API access, IBM Cloud

subcollection: blockchain

---

{:external: target="_blank" .external}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:gif: data-image-type='gif'}

# Interacting with the network using Swagger APIs
{: #ibp-swagger}

{{site.data.keyword.blockchainfull_notm}} Platform exposes a number of REST APIs in Swagger that you can use to manage the nodes, channels, peers, and members of your network. Your applications can use these APIs to control important network resources without using the Network Monitor.

{:shortdesc}

Before you begin, you need to create a [{{site.data.keyword.blockchain}} Platform service instance](https://cloud.ibm.com/catalog/services/ibm-blockchain-5-prod){: external} on {{site.data.keyword.cloud_notm}}.


## Retrieving network credentials
{: #ibp-swagger-retrieving-network-credentials}

Enter the Network Monitor of your Blockchain network and open the "APIs" screen from the left navigator. You can see your network credentials for the REST APIs. You will later authorize the APIs by using the values of the "key" and "secret" displayed here, and run the APIs with the "network_id" as a parameter. Click **Show secret** to reveal the value of the secret field. Copy the values of the key, secret, and network_id fields, which you can used later in the Swagger UI.

**Figure 1** shows the "APIs" screen:
![APIs screen](../images/API_screen_starter.png "APIs screen"){: caption="Figure 1. APIs" caption-side="bottom"}

## Authorizing Swagger APIs
{: #ibp-swagger-authorizing-swagger}

Click the **Swagger UI** link on the "APIs" screen to open the Swagger UI.  

In the Swagger UI, click the **Authorize** button and the authorization window pops up. Enter the value of "key" and "secret" in your network credentials as username and password, and click **Authorize** then **Done**. Now you are ready to run the APIs. Note that if you refresh your browser, you need to reauthorize with your credentials.

Using Basic Auth authentication, any credentials that you specify in the Authorize window are stored after you click the **Authorize** and then **Done** buttons and are passed on each REST api call.

**Figure 3** shows the process to authorize Swagger APIs:

![Authorize APIs](../images/swaggerUIAuthorize.gif "Authorize APIs"){: caption="Figure 2. Authorize APIs" caption-side="bottom"}{: gif}


## Trying out APIs
{: #ibp-swagger-try-out}

Click the REST API you want to run and click the **Try it out** button.

**Figure 3** shows the "Try it out" button in the "Swagger UI":

![Try it out button in Swagger UI](../images/swaggerUITryItOut.png "Try it out button in Swagger UI"){: caption="Figure 3. *Try it out* button in the *Swagger UI* caption-side="bottom"}

After you click the **Try it out** button, you can enter required parameters to use the API. You can find `networkID` in your network credentials and find other parameters in your Network Monitor. After you enter the parameters, click t**Execute** to run the REST API call against your network.

**Figure 4** shows parameters in the "Swagger UI":

![Parameters in Swagger UI](../images/swaggerUIParams.png "Parameters in Swagger UI"){: caption="Figure 4. Entering parameters" caption-side="bottom"}  

After you click **Execute**, you can see the response of the API call against your network. You can also see a CURL command that can call the API directly from your command line.

**Figure 5** shows the API response body, URL, and CURL command:

![API response in Swagger UI](../images/swaggerUICurlResponse.png "API response in Swagger UI"){: caption="Figure 5. API response" caption-side="bottom"}    

## Disabling API access
{: #ibp-swagger-turn-off}

By default, all users with a non-Auditor role in {{site.data.keyword.cloud_notm}}, can view and use the **Network credentials** visible on the Swagger APIs' panel and can therefore manage your network using the APIs. However, if you prefer not to expose your Swagger API network credentials in the UI, you can copy and secure your existing key and secret values and generate new credentials that are not valid for use with the Swagger APIs. A flag, named resetCredentials, is provided and allows you to control the access by completing the following steps:

1. Follow the steps to generate a new network credential as described in the [Service credentials dashboard](/docs/blockchain?topic=blockchain-swagger-network#swagger-network-retrieve-id-token).
2. However, in the **Add Inline Configuration Parameters** box, paste in the following value:
   ```
   {
     "resetCredentials": true
   }
   ```
   {:codeblock}
3. Click **Add**.

Now, when any user accesses the Swagger APIs' panel from the UI, the **Network credentials** information in the UI will contain a generic key and secret value that is not valid for managing your network. Any API requests submitted using those credentials will not be processed.  

If you want to expose valid network credentials in the UI at a later time, simply repeat the steps above to generate a new credential, but this time in the **Add Inline Configuration Parameters** box, you can leave the box blank. You do not need to specify any parameters.

Now, the original valid credentials are visible in the **Network credentials** information in the UI and can be used to authenticate the Swagger APIs.

## Troubleshooting tips
{: #ibp-swagger-troubleshooting}

### 401 Unauthorized  
{: #ibp-swagger-401}

  Ensure that you have authorized the REST API by providing your network credentials. For more information, see [Authorizing Swagger APIs](/docs/blockchain?topic=blockchain-ibp-swagger#ibp-swagger-authorizing-swagger).

### 400 Error: Bad Request
{: #ibp-swagger-400}

  Some APIs might take an argument in the Body of the request which acts as a filter to show results only for a specific peer. A sample snippet is provided in the Body, which if used, needs to be edited to specify the peer or list of peers that you would like to filter on. To avoid this error, either edit the snippet to specify a peer in your network or remove the snippet entirely.
