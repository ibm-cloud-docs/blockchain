---

copyright:
  years: 2020
lastupdated: "2020-06-16"

keywords: intermediate CA, root CA, parent server, Certificate Authority

subcollection: blockchain

---

{:external: target="_blank" .external}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:note: .note}
{:important: .important}
{:help: data-hd-content-type='help'}
{:support: data-reuse='support'}
{:tip: .tip}
{:pre: .pre}

# Creating an intermediate Certificate Authority (CA)
{: #ibp-ica}
{: help}
{: support}



For customers who prefer to include intermediate CAs in their network, the {{site.data.keyword.blockchainfull}} Platform offers this configuration option when you deploy a CA. This tutorial describes the process for creating an intermediate CA.
{: shortdesc}

## Why would I want to use an intermediate CA with my {{site.data.keyword.blockchainfull_notm}} Platform network?
{: #ibp-ica-why}

Intermediate CAs are optional. Because an intermediate CA has its root certificate issued by a root CA or another intermediate authority, a “chain of trust” is established for any certificate that is issued by all CAs in the chain. Having one or more intermediate CAs provides added security and allows you to protect your root of trust. This ability to track back to the root CA not only allows the function of CAs to scale while still providing security, it also limits the exposure of the root CA, which, if compromised, would endanger the entire chain of trust. If an intermediate CA is compromised, on the other hand, there will be a much smaller exposure. A key benefit is that after the intermediate CAs are up and running, the root CA can be effectively turned off, limiting its vulnerability even more.

Another reason to include an intermediate CA would be when you have a very large organization with multiple departments and you don’t want a single CA to generate certificates for all of the departments. Intermediate CAs provide a mechanism for scoping the certificates that a CA manages to a smaller department or sub-group. This pattern though incurs significant overheard if there will only be a small number of members in the organization.

As an alternative to deploying multiple intermediate CAs, you can limit the scope of a CA by [overriding the CA configuration](/docs/blockchain?topic=blockchain-ibp-console-adv-deployment&locale=en#ibp-console-adv-deployment-ca-customization-why) to specify available `affiliations` (similar to departments) instead. When a CA is configured with affiliations, the affiliation of the registrar must be equal to or a prefix of the affiliation of the identity being registered. See the Fabric CA topic on [registering identities](https://hyperledger-fabric-ca.readthedocs.io/en/release-1.4/users-guide.html#registering-a-new-identity){: external} for more details.

The following diagram illustrates how the root CA issues the certificates for the intermediate CA and the intermediate CA then issues the certificates for the nodes and users in the organization.
![Intermediate CA](../images/int_ca.svg "Intermediate CA"){: caption="Figure 1. Intermediate CA in the network." caption-side="bottom"}

## Can I convert an existing CA to be an intermediate CA?
{: #ibp-ica-convert}

No. Because the CA signed certificate cannot be regenerated, you cannot convert an existing CA to become an intermediate CA. If you did, all of the existing certificates that were issued by the CA would no longer be valid. Therefore, if you want to have an intermediate CA in your network, you need to create a new CA and configure it to be an intermediate CA during the deployment process.

## Limitations
{: #ibp-ica-limitations}

Currently, {{site.data.keyword.blockchainfull_notm}} Platform only supports a single level of intermediate CAs. In other words, an intermediate CA cannot be a parent server to another intermediate CA.

## Process overview
{: #ibp-ica-overview}

The configuration process begins with the root CA. If one does not already exist, you need to create the root CA and then register two identities that are needed by the intermediate CA. When you create an intermediate CA you need to provide a JSON that overrides the default CA settings with the intermediate CA configuration. After your intermediate CA is active, you can use it for all subsequent identity register and enrollment requests for the organization, and then effectively turn off the root CA. At a high level, the following steps need to be performed:

- [Part One: Actions you perform from the root CA](#ibp-ica-part-one)

  1. Deploy a root CA if one does not already exist.
  2. Register the intermediate CA admin identity with the root CA.
  3. Register the intermediate TLS CA admin identity with the root CA.
  4. Export the root CA to a JSON file.

- [Part Two: Build the intermediate CA JSON override](#ibp-ica-part-two)

  Edit the example JSON override file by providing the root CA TLS signing cert and the intermediate CA identity enroll id and secrets to the JSON override file.

- [Part Three: Actions you perform on the intermediate CA](#ibp-ica-part-three)

  1. Create a new CA which will serve as the intermediate CA.
  2. Provide the intermediate CA admin enroll ID and secret that you registered with the root CA.
  3. Provide the CA JSON override file.

After completing these steps, when you create organization MSPs for your peers or orderers, you can point to the intermediate CA instead of the root CA.

This process can be performed using the console or the APIs, if you are an experienced API user. The overall process is the same regardless of which approach you choose. For clarity, this tutorial uses the console.
{: note}

## Part One: Actions you perform from the root CA
{: #ibp-ica-part-one}

1. **Deploy a root CA.** Before you can deploy an intermediate CA, a root CA must already exist in your {{site.data.keyword.blockchainfull_notm}} instance. See the [Build a network](/docs/blockchain?topic=blockchain-ibp-console-build-network#ibp-console-build-network-create-CA-org1CA) tutorial for instructions on how to create a CA.

2. **Register the intermediate CA admin identity with the root CA.** In order for an intermediate CA to be able to register and enroll identities, the intermediate CA admin identity must be registered with the root CA.
  - Click the root CA tile to open it and click **Register user**.
  - Enter the enroll ID and secret that you will use for the intermediate CA admin identity. For purposes of this tutorial we will use `icaadmin` and `icaadminpw`, but you can use any values suitable for your use case.
  - Leave the identity type as `client` and optionally specify a value for **maximum enrollments** if you want to limit the number of times you can generate certificates for this admin identity. Otherwise, you can leave it blank.
  - On the next panel, because this identity will be for the intermediate CA admin, you need to add the attribute `hf.IntermediateCA` and set its value to `true`. Don't forget to click **Add**.
  - Click **Register user** when you are finished.

3. **Register the intermediate TLS CA admin identity with the root CA.** Because TLS communications are enabled on the network, whenever you deploy a CA, a TLS CA is automatically deployed along side the CA, and uses the same endpoint address. Therefore, you also need to register an identity that will serve as the intermediate TLS CA admin. Repeat the actions that you performed in the previous step to register the admin identity, but this time specify a different enroll ID and secret. For purposes of the tutorial, we use `itlscaadmin` and `itlscaadminpw`.

4. **Export the root CA to a JSON file.** Now that you have the intermediate CA admins registered with the root CA, there is one last piece of information we need: the root CA TLS signed certificate. This certificate is shared with each node in the organization and is used to secure the communications between the nodes. Therefore, in order for the intermediate CA to communicate with the root CA, it has to be included in the intermediate CA configuration. To get the root CA TLS signed certificate, simply export the root CA to a JSON file. Open the root CA and click the **Export** icon.
  ![Export root CA](../images/export-root-ca.png "Export root CA"){: caption="Figure 2. Export root CA" caption-side="bottom"}

  - Open the downloaded JSON file and locate the value corresponding to the `tls_cert` element. You will need to copy and paste the certificate in a later step. It will resemble:

    ```
    "LS0tLS1CRUdJTiBDRVJUSUZJQ0FURS0tLS0tCk1JSUNvRENDQWtXZ0F3SUJBZ0lRWXZYRUgzUktMUVNiUnZTdnRLWUJDVEFLQmdncWhrak9QUVFEQWpDQnB6RUwKTUFrR0ExVUVCaE1DVlZNeEZ6QVZCZ05WQkFnVERrNXZjblJvSUVOaGNtOXNhVzVoTVE4d0RRWURWUVFIRXdaRQpkWEpvWVcweEREQUtCZ05WQkFvVEEwbENUVEVUTUJFR0ExVUJVaHAKVkM1VE5OVzNqRzJkVVZwUUlkSmEvMWtDSVFEMGE0QWRzazJ0RlhDRFp1RFB0VG5lTkNZWnBDSzMxL2MrUTFLTQozLzVGdEE9PQotLS0tLUVORCBDRVJUSUZJQ0FURS0tLS0tCga1",
    ```

  - Also make note of the value of the `api_url` in the JSON file. You will use the value of this URL to build the parent server URL for the intermediate CA. It will resemble:

    ```
    "api_url": "https://nb535a5-rootca.org1Cluster.us-south.containers.appdomain.cloud:7054",
    ```

    To build the parent server URL, edit the URL and insert the intermediate CA admin enroll ID and secret using the following format:
    ```
    https://<INTERMEDIATE-CA-ADMIN>:<INTERMEDIATE-CA-ADMIN-PW>@<DOMAIN>:<PORT>
    ```

    For example:
    ```
    https://icaadmin:icaadminpw@nb535a5-rootca.org1Cluster.us-south.containers.appdomain.cloud:7054
    ```

    Save the value of this URL for the next section.

## Part Two: Build the intermediate CA JSON override
{: #ibp-ica-part-two}

Anytime that you deploy a CA, you can override the default CA settings by pasting a JSON that contains the parameters that you want to override. Because we are deploying an intermediate CA, we need to override some of the default settings in order to configure it to be an intermediate CA. For example, you need to configure the `intermediate` block of the JSON to point to your root CA as the parent server. Therefore, copy the following text to an editor where you can modify the values:

```json
{
 	"ca": {
 		"debug": true,
 		"registry": {
 			"maxenrollments": -1,
 			"identities": [{
 				"name": "<INTERMEDIATE-CA-ADMIN>",
 				"pass": "<INTERMEDIATE-CA-ADMIN-PW>",
 				"type": "client",
 				"attrs": {
 					"hf.Registrar.Roles": "*",
 					"hf.Registrar.DelegateRoles": "*",
 					"hf.Revoker": true,
 					"hf.IntermediateCA": true,
 					"hf.GenCRL": true,
 					"hf.Registrar.Attributes": "*",
 					"hf.AffiliationMgr": true
 				}
 			}]
 		},
 		"intermediate": {
 			"parentserver": {
 				"url": "<PARENT-CA-SERVER-URL>",
 				"caname": "ca"
 			},
 			"tls": {
 				"enabled": true,
 				"certfiles": ["<TLS-CERT-FILE>"]
 			}
 		}
 	},
 	"tlsca": {
 		"debug": true,
 		"registry": {
 			"maxenrollments": -1,
 			"identities": [{
 				"name": "<INTERMEDIATE-TLS-CA-ADMIN>",
 				"pass": "<INTERMEDIATE-TLS-CA-ADMIN-PW>",
 				"type": "client",
 				"attrs": {
 					"hf.Registrar.Roles": "*",
 					"hf.Registrar.DelegateRoles": "*",
 					"hf.Revoker": true,
 					"hf.IntermediateCA": true,
 					"hf.GenCRL": true,
 					"hf.Registrar.Attributes": "*",
 					"hf.AffiliationMgr": true
 				}
 			}]
 		},
 		"intermediate": {
 			"parentserver": {
 				"url": "<PARENT-TLS-CA-SERVER-URL>",
 				"caname": "tlsca"
 			},
 			"tls": {
 				"enabled": true,
 				"certfiles": ["<TLS-CERT-FILE>"]
 			}
 		}
 	}
 }
```
{: codeblock}

Notice there are settings for both the intermediate CA and the intermediate TLS CA. Replace the following parameters:

- `<INTERMEDIATE-CA-ADMIN>` - with the enroll ID that you specified in Part One, step 2 when you registered the intermediate CA admin identity.
- `<INTERMEDIATE-CA-ADMIN-PW>` - with the enroll secret that you specified in Part One, step 2 when you registered the intermediate CA admin identity.
- `<PARENT-CA-SERVER-URL>` - with the value of the parent server that you built in Part One, step 4.
- `<TLS-CERT-FILE>` - (two occurrences) with the value of the `tls_cert`, the certificate string that you downloaded from the root CA in Part One, step 4.
- `<INTERMEDIATE-TLS-CA-ADMIN>` - with the enroll ID that you specified in Part One, step 3 when you registered the intermediate TLS CA admin identity.
- `<INTERMEDIATE-TLS-CA-ADMIN-PW>` - with the enroll secret that you specified in Part One, step 3 when you registered the intermediate TLS CA admin identity.
- `<PARENT-TLS-CA-SERVER-URL>` - with the value of the parent server URL that you built in Part One, step 4; however you need to replace the enroll ID and secret with the values that you specified when you registered the admin for the intermediate TLS CA, in Part One, step 3. Because both the intermediate CA and intermediate TLS CA are deployed to the same endpoint address, the rest of the URL remains the same as the `<PARENT-CA-SERVER-URL>`.

Your completed JSON looks similar to:

```json
{
 	"ca": {
 		"debug": true,
 		"registry": {
 			"maxenrollments": -1,
 			"identities": [{
 				"name": "icaadmin",
 				"pass": "icaadminpw",
 				"type": "client",
 				"attrs": {
 					"hf.Registrar.Roles": "*",
 					"hf.Registrar.DelegateRoles": "*",
 					"hf.Revoker": true,
 					"hf.IntermediateCA": true,
 					"hf.GenCRL": true,
 					"hf.Registrar.Attributes": "*",
 					"hf.AffiliationMgr": true
 				}
 			}]
 		},
 		"intermediate": {
 			"parentserver": {
 				"url": "https://icaadmin:icaadminpw@nb535a5-rootca.org1Cluster.us-south.containers.appdomain.cloud:7054",
 				"caname": "ca"
 			},
 			"tls": {
 				"enabled": true,
 				"certfiles": ["LS0tLS1CRUdJTiBDRVJUSUZJQ0FURS0tLS0tCk1JSUNvRENDQWtXZ0F3SUJBZ0lRWXZYRUgzUktMUVNiUnZTdnRLWUJDVEFLQmdncWhrak9QUVFEQWpDQnB6RUwKTUFrR0ExVUVCaE1DVlZNeEZ6QVZCZ05WQkFnVERrNXZjblJvSUVOaGNtOXNhVzVoTVE4d0RRWURWUVFIRXdaRQpkWEpvWVcweEREQUtCZ05WQkFvVEEwbENUVEVUTUJFR0ExVUJVaHAKVkM1VE5OVzNqRzJkVVZwUUlkSmEvMWtDSVFEMGE0QWRzazJ0RlhDRFp1RFB0VG5lTkNZWnBDSzMxL2MrUTFLTQozLzVGdEE9PQotLS0tLUVORCBDRVJUSUZJQ0FURS0tLS0tCga1"]
 			}
 		}
 	},
 	"tlsca": {
 		"debug": true,
 		"registry": {
 			"maxenrollments": -1,
 			"identities": [{
 				"name": "itlscaadmin",
 				"pass": "itlscaadminpw",
 				"type": "client",
 				"attrs": {
 					"hf.Registrar.Roles": "*",
 					"hf.Registrar.DelegateRoles": "*",
 					"hf.Revoker": true,
 					"hf.IntermediateCA": true,
 					"hf.GenCRL": true,
 					"hf.Registrar.Attributes": "*",
 					"hf.AffiliationMgr": true
 				}
 			}]
 		},
 		"intermediate": {
 			"parentserver": {
 				"url": "https://itlscaadmin:itlscaadminpw@nb535a5-rootca.org1Cluster.us-south.containers.appdomain.cloud:7054",
 				"caname": "tlsca"
 			},
 			"tls": {
 				"enabled": true,
 				"certfiles": ["LS0tLS1CRUdJTiBDRVJUSUZJQ0FURS0tLS0tCk1JSUNvRENDQWtXZ0F3SUJBZ0lRWXZYRUgzUktMUVNiUnZTdnRLWUJDVEFLQmdncWhrak9QUVFEQWpDQnB6RUwKTUFrR0ExVUVCaE1DVlZNeEZ6QVZCZ05WQkFnVERrNXZjblJvSUVOaGNtOXNhVzVoTVE4d0RRWURWUVFIRXdaRQpkWEpvWVcweEREQUtCZ05WQkFvVEEwbENUVEVUTUJFR0ExVUJVaHAKVkM1VE5OVzNqRzJkVVZwUUlkSmEvMWtDSVFEMGE0QWRzazJ0RlhDRFp1RFB0VG5lTkNZWnBDSzMxL2MrUTFLTQozLzVGdEE9PQotLS0tLUVORCBDRVJUSUZJQ0FURS0tLS0tCga1"]
 			}
 		}
 	}
 }
```

## Part Three: Actions you perform on the intermediate CA
{: #ibp-ica-part-three}

You've registered the intermediate CA and intermediate TLS CA identities with the root CA and have built the JSON for the CA override. You are now ready to deploy the intermediate CA.

1. From the **Nodes** tab, click **Add Certificate Authority** followed by **Create a Certificate Authority**.
2. Give your intermediate CA a meaningful name and then provide the intermediate CA admin enroll ID and secret that you registered with the root CA. For example `icaadmin` and `icaadminpw`. You can leave the **Advanced deployment options** unchecked, none are required for an intermediate CA.
3. On the **Summary** panel, click **Edit configuration JSON (Advanced)**.
4. Delete the existing content from the **Configuration JSON** box and paste the JSON that you built in [Part Two](#ibp-ica-part-two), then click **Add Certificate Authority**.

It will take a few minutes for the CA to deploy. You'll know it is ready when a green status indicator box is visible on the tile. Depending on your cluster activity, you might need to refresh your browser for the status to turn green.
{: tip}

## Next steps
{: #ibp-ica-next-steps}

### Register and enroll identities against the intermediate CA
{: #ibp-ica-next-steps-reg-enroll}

Your intermediate CA is now operational and can be used to register and enroll identities just like you do with the root CA. See the topic on [Creating and managing identities](/docs/blockchain?topic=blockchain-ibp-console-identities) to learn more.

### Create organization MSPs using the intermediate CA
{: #ibp-ica-next-steps-msp}

When you build organization MSP definitions for your peer or ordering nodes, you can now reference the intermediate CA as the "root CA" instead of your root CA.
![Int CA MSP](../images/int-ca-msp.png "MSP using intermediate CA"){: caption="Figure 3. MSP using intermediate CA" caption-side="bottom"}
If you want to learn more about creating MSPs, see [Managing organizations](https://cloud.ibm.com/docs/blockchain?topic=blockchain-ibp-console-organizations).

### Scale down the root CA
{: #ibp-ica-next-steps-scale}

Because the intermediate CA can process all requests that the root CA handles, you can effectively turn off your root CA by scaling down the root CA CPU to 0.001 CPU. Taking this action renders the root CA non-functional while in this state. As long as you don't need the root CA to issue identities or enroll other intermediate CAs, it can be safely scaled down. See the topic on [Reallocating resources](/docs/blockchain?topic=blockchain-ibp-console-govern-components) for the steps to scale down the CPU. If the root CA is needed later, for example to enroll another intermediate CA, you can always repeat the steps to scale back up the CPU. The minimum CPU required for a CA to be operational is `0.1 CPU`.

By default, certificates generated by {{site.data.keyword.blockchainfull_notm}} Platform CAs expire after one year and TLS certificates expire after 10 years. Exactly thirty days before **peer** and **ordering node** certificates are due to expire, the platform automatically attempts to contact the CA and renew the certificates. If the CA is not online, the automatic renewal fails and enrollment to generate a new certificate needs to be performed manually. If automatic renewal of these certificates is important for your business, careful consideration should be given to scaling down a root CA or intermediate CA. See the topic on [Certificate expiration and renewal](/docs/blockchain-sw-25?topic=blockchain-sw-25-ibp-console-identities#ibp-console-identities-expiration) to learn more about the process.

