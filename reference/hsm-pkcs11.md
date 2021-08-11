---

copyright:
  years: 2021
lastupdated: "2021-08-11"

keywords: HSM, PKCS11 proxy

subcollection: blockchain

---

{:DomainName: data-hd-keyref="APPDomain"}
{:DomainName: data-hd-keyref="DomainName"}
{:android: data-hd-operatingsystem="android"}
{:api: .ph data-hd-interface='api'}
{:apikey: data-credential-placeholder='apikey'}
{:app_key: data-hd-keyref="app_key"}
{:app_name: data-hd-keyref="app_name"}
{:app_secret: data-hd-keyref="app_secret"}
{:app_url: data-hd-keyref="app_url"}
{:audio: .audio}
{:authenticated-content: .authenticated-content}
{:beta: .beta}
{:c#: .ph data-hd-programlang='c#'}
{:c#: data-hd-programlang="c#"}
{:cli: .ph data-hd-interface='cli'}
{:codeblock: .codeblock}
{:curl: #curl .ph data-hd-programlang='curl'}
{:curl: .ph data-hd-programlang='curl'}
{:deprecated: .deprecated}
{:dotnet-standard: .ph data-hd-programlang='dotnet-standard'}
{:download: .download}
{:external: .external target="_blank"}
{:external: target="_blank" .external}
{:faq: data-hd-content-type='faq'}
{:fuzzybunny: .ph data-hd-programlang='fuzzybunny'}
{:generic: data-hd-operatingsystem="generic"}
{:generic: data-hd-programlang="generic"}
{:gif: data-image-type='gif'}
{:go: .ph data-hd-programlang='go'}
{:help: data-hd-content-type='help'}
{:hide-dashboard: .hide-dashboard}
{:hide-in-docs: .hide-in-docs}
{:important: .important}
{:ios: data-hd-operatingsystem="ios"}
{:java: #java .ph data-hd-programlang='java'}
{:java: .ph data-hd-programlang='java'}
{:java: data-hd-programlang="java"}
{:javascript: .ph data-hd-programlang='javascript'}
{:javascript: data-hd-programlang="javascript"}
{:middle: .ph data-hd-position='middle'}
{:navgroup: .navgroup}
{:new_window: target="_blank"}
{:node: .ph data-hd-programlang='node'}
{:note: .note}
{:objectc: .ph data-hd-programlang='Objective C'}
{:objectc: data-hd-programlang="objectc"}
{:org_name: data-hd-keyref="org_name"}
{:php: .ph data-hd-programlang='PHP'}
{:php: data-hd-programlang="php"}
{:pre: .pre}
{:preview: .preview}
{:python: .ph data-hd-programlang='python'}
{:python: data-hd-programlang="python"}
{:right: .ph data-hd-position='right'}
{:route: data-hd-keyref="route"}
{:row-headers: .row-headers}
{:ruby: .ph data-hd-programlang='ruby'}
{:ruby: data-hd-programlang="ruby"}
{:runtime: architecture="runtime"}
{:runtimeIcon: .runtimeIcon}
{:runtimeIconList: .runtimeIconList}
{:runtimeLink: .runtimeLink}
{:runtimeTitle: .runtimeTitle}
{:screen: .screen}
{:script: data-hd-video='script'}
{:service: architecture="service"}
{:service_instance_name: data-hd-keyref="service_instance_name"}
{:service_name: data-hd-keyref="service_name"}
{:shortdesc: .shortdesc}
{:space_name: data-hd-keyref="space_name"}
{:step: data-tutorial-type='step'}
{:step: data-tutorial-type='step'} 
{:subsection: outputclass="subsection"}
{:support: data-reuse='support'}
{:swift: #swift .ph data-hd-programlang='swift'}
{:swift: .ph data-hd-programlang='swift'}
{:swift: data-hd-programlang="swift"}
{:table: .aria-labeledby="caption"}
{:term: .term}
{:terraform: .ph data-hd-interface='terraform'}
{:tip: .tip}
{:tooling-url: data-tooling-url-placeholder='tooling-url'}
{:topicgroup: .topicgroup}
{:troubleshoot: data-hd-content-type='troubleshoot'}
{:tsCauses: .tsCauses}
{:tsResolve: .tsResolve}
{:tsSymptoms: .tsSymptoms}
{:tutorial: data-hd-content-type='tutorial'}
{:ui: .ph data-hd-interface='ui'}
{:unity: .ph data-hd-programlang='unity'}
{:url: data-credential-placeholder='url'}
{:user_ID: data-hd-keyref="user_ID"}
{:vbnet: .ph data-hd-programlang='vb.net'}
{:video: .video}



# HSM PKCS #11 proxy
{: #ibp-hsm-build-pkcs11-proxy}

Learn how to configure a PKCS #11 proxy to work with any HSM that supports the PKCS #11 standard.
{: shortdesc}

Use of this process has been deprecated in favor of using an [HSM client image](/docs/blockchain?topic=blockchain-ibp-console-adv-deployment#ibp-console-adv-deployment-hsm-build-docker).
{: deprecated}

## Setting up a PKCS #11 proxy for your HSM
{: #ibp-hsm-build-pkcs11-proxy}  

An HSM is a hardware appliance that provides cryptographic processing services. In the context of a blockchain network, an HSM is used to generate and store private keys inside a tamper-resistent device. After you deploy the HSM, you need to also deploy a PKCS #11 proxy that allows the blockchain node to communicate with the HSM.  These instructions describe how to build the proxy image, how to deploy it to your cluster, and how to configure it.

The HSM and the proxy can reside anywhere, but, assuming your cluster has access to the same private network where your HSM is found, a best practice would be for the proxy to be running on the same cluster as your blockchain network.
{: tip}

Before attempting these instructions you should already have a [DockerHub login](https://hub.docker.com/signup/){: external}.
{: important}

## Why is a proxy required?
{: #ibp-hsm-build-pkcs11-proxy-why}  

The {{site.data.keyword.blockchainfull_notm}} Platform HSM implementation is based on Hyperledger Fabric which supports devices that use the [PKCS #11 standard](http://docs.oasis-open.org/pkcs11/pkcs11-base/v2.40/os/pkcs11-base-v2.40-os.html){: external}. Therefore, after deploying an HSM, you need to configure a PKCS #11 proxy to set up the communications between the appliance and the nodes on the network. One PKCS #11 proxy can communicate with multiple partitions on an HSM. When a user is enrolled, their private key is generated by the HSM and stored in the partition. A partition can store multiple keys in it, although the exact number varies by HSM provider. Organizations can share a slot or partition, but it is more likely that each organization would want their own partition or potentially even their own HSM.

## Building the proxy image
{: #ibp-hsm-build-pkcs11-proxy-build-img}

The first step is to build a Docker image for the PKCS #11 proxy and add the HSM-specific library to the image.

- The following example shows the process for adding the `SoftHSM` drivers to the image. (`SoftHSM` is a software version of HSM that can be used for HSM simulation and testing, but if you are using your own HSM, you need to replace it with the library from your HSM provider.)  If you are using {{site.data.keyword.cloud_notm}} HSM, the proxy instructions are included in the [tutorial](/docs/blockchain-sw-252?topic=blockchain-sw-252-ibp-hsm-gemalto).

If you are running the platform behind a firewall, you need to pull the proxy image to a machine that has internet access and then push the image to a Docker registry that you can access from behind your firewall.
{: note}

Starting under `### YOUR HSM LIBRARY BUILD GOES HERE ###`, edit the code for your HSM, and save it to a file named `Dockerfile`. If you are not using `SoftHSM` to try out the process, you should remove the section that starts with the label `### EXAMPLE CONFIGURATION FOR SOFTHSM ###` and ends with `### EXAMPLE CONFIGURATION FOR SOFTHSM ENDS HERE###`.

```

########## Build the pkcs11 proxy ##########
FROM registry.access.redhat.com/ubi8/ubi-minimal as builder
ARG ARCH=amd64

ARG VERSION=2032875

RUN microdnf install -y \
	git \
	make \
	cmake \
	openssl-devel \
	gcc;

RUN if [ "${ARCH}" == "amd64" ]; then ARCH="x86_64"; fi && \
	rpm -ivh https://kojipkgs.fedoraproject.org/packages/libseccomp/2.4.2/2.fc30/${ARCH}/libseccomp-2.4.2-2.fc30.${ARCH}.rpm && \
	rpm -ivh https://kojipkgs.fedoraproject.org/packages/libseccomp/2.4.2/2.fc30/${ARCH}/libseccomp-devel-2.4.2-2.fc30.${ARCH}.rpm


RUN git clone https://github.com/SUNET/pkcs11-proxy && \
	cd pkcs11-proxy && \
	git checkout ${VERSION} && \
	cmake . && \
	make && \
	make install

########## FINAL image - Build softhsm client ##########

FROM registry.access.redhat.com/ubi8/ubi-minimal
ARG ARCH=amd64

RUN microdnf install -y \
	shadow-utils \
	&& microdnf clean all \
	&& groupadd -g 7051 ibp-user \
	&& useradd -u 7051 -g ibp-user -s /bin/bash ibp-user \
	&& mkdir /licenses \
	&& if [ "${ARCH}" == "amd64" ]; then ARCH="x86_64"; fi \
	&& rpm -ivh https://kojipkgs.fedoraproject.org/packages/libseccomp/2.4.2/2.fc30/${ARCH}/libseccomp-2.4.2-2.fc30.${ARCH}.rpm \
	&& microdnf remove shadow-utils \
	&& microdnf clean all;

COPY --from=builder /usr/local/bin/pkcs11-daemon /usr/local/bin/pkcs11-daemon
COPY --from=builder /usr/local/lib/libpkcs11-proxy.so.0.1 /usr/local/lib/libpkcs11-proxy.so.0.1
COPY --from=builder /usr/local/lib/libpkcs11-proxy.so.0 /usr/local/lib/libpkcs11-proxy.so.0
COPY --from=builder /usr/local/lib/libpkcs11-proxy.so /usr/local/lib/libpkcs11-proxy.so

ENV PKCS11_DAEMON_SOCKET="tcp://0.0.0.0:2345"
# ENV PKCS11_PROXY_TLS_PSK_FILE="/tls.psk"
EXPOSE 2345

### YOUR HSM LIBRARY BUILD GOES HERE ###

ENV LIBRARY_LOCATION="/usr/lib64/softhsm/libsofthsm.so"

### YOUR HSM LIBRARY BUILD ENDS HERE ###

### EXAMPLE CONFIGURATION FOR SOFTHSM ###

ENV SLOT 0
ENV LABEL test
ENV PIN 5678
ENV SO_PIN 5678
ENV ARCH x86_64
ENV DEBUG_OUTPUT 1

RUN microdnf update && \
    microdnf install -y shadow-utils && \
    rpm -ivh https://kojipkgs.fedoraproject.org/packages/softhsm/2.5.0/4.fc30/${ARCH}/softhsm-2.5.0-4.fc30.${ARCH}.rpm
RUN mkdir -p /var/lib/softhsm/tokens/
RUN softhsm2-util --slot ${SLOT} --init-token --label ${LABEL} --pin ${PIN} --so-pin ${SO_PIN}

### EXAMPLE CONFIGURATION FOR SOFTHSM ENDS HERE###

### ###

CMD ["sh", "-c", "pkcs11-proxy $LIBRARY_LOCATION -"]
```
{: codeblock}

Run the following command to build the image:

```
docker build -t pkcs11-proxy:v1 -f Dockerfile .
```
{: codeblock}
After the image is built, the next step is to push the image to your Docker registry (for example, Docker Hub). This command will look similar to:

```
docker login -u <DOCKER_HUB_ID> -p <DOCKER_HUB_PWD>
docker tag pkcs11-proxy:v1 <DOCKER_HUB_ID>/pkcs11-proxy:v1
docker push <DOCKER_HUB_ID>/pkcs11-proxy:v1
```
{: codeblock}

- Replace `<DOCKER_HUB_ID>` with your Docker Hub id.
- Replace `<DOCKER_HUB_PWD>` with your Docker Hub password.

## Deploying the proxy to your cluster
{: #ibp-hsm-build-pkcs11-proxy-deploy}

In order to use HSM with any blockchain node, you need to configure a PKCS #11 proxy that enables communications between the HSM and your blockchain nodes. The following steps guide you through the process of building an image for the proxy, deploying it, and configuring the communications with your blockchain node.

### **Step one:**  Create a new namespace
{: #ibp-hsm-build-pkcs11-proxy-deploy-s1}


On your Kubernetes cluster you need to create a namespace for HSM.
```
kubectl create ns hsm
```
{: codeblock}


If your cluster is on OpenShift, you need to run the command:
```
oc new-project hsm
```
{: codeblock}


### **Step two:** Create a Kubernetes secret
{: #ibp-hsm-build-pkcs11-proxy-deploy-s2}

Create an image pull secret for pulling the image from my.repo.docker.hub. The command might look something like:

```
kubectl create secret docker-registry docker-pull-secret --docker-username=<DOCKER_HUB_ID> --docker-password=<DOCKER_HUB_PWD> --docker-email=<EMAIL> --docker-server=<<DOCKER_HUB_ID>:pkcs11-proxy:v1
```
{: codeblock}

- Replace `<DOCKER_HUB_ID>` with the Docker user name.
- Replace `<DOCKER_HUB_PWD>` with the Docker password.
- Replace `<EMAIL>` with your Docker Hub email address.

For example:
```
kubectl create secret docker-registry docker-pull-secret --docker-username=dockeruser --docker-password=dockerpwd --docker-email=dockeruser@example.com --docker-server=dockeruser/pkcs11-proxy:v1 -n hsm
```
{: codeblock}

### **Step three:** Deploy the proxy pod
{: #ibp-hsm-build-pkcs11-proxy-deploy-s3}

After the secret is created with the name `docker-pull-secret`, you can use the following file to deploy the proxy pod. Copy and edit the `yaml` below and save it to a file named `pod.yaml`.

- Replace `<DOCKER_HUB_ID>` with your DockerHub login.

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: pkcs11-proxy
  namespace: hsm
  labels:
    app: pkcs11
spec:
  imagePullSecrets:
  - name: docker-pull-secret
  containers:
  - name: proxy
    command:
      - sh
      - -c
      - pkcs11-daemon $LIBRARY_LOCATION
    image: <DOCKER_HUB_ID>/pkcs11-proxy:v1
    imagePullPolicy: Always
    readinessProbe:
      tcpSocket:
        port: 2345
      initialDelaySeconds: 5
      periodSeconds: 10
    livenessProbe:
      tcpSocket:
        port: 2345
      initialDelaySeconds: 15
      periodSeconds: 20
```
{: codeblock}

Deploy the file on the cluster in the `hsm` namespace by running the following command:

```
kubectl create -f pod.yaml -n hsm
```
{: codeblock}

Verify that the pod is running and passes the liveliness and readiness probes by running the command:
```
kubectl describe pod pkcs11-proxy -n hsm
```
{: codeblock}

If the pod starts successfully you will see something similar to:

```
Type    Reason     Age   From                  Message
  ----    ------     ----  ----                  -------
  Normal  Scheduled  19s   default-scheduler     Successfully assigned hsm/pkcs11-proxy to 10.95.42.51
  Normal  Pulling    18s   kubelet, 10.95.42.51  pulling image "pamaibm/pkcs11-proxy:v1"
  Normal  Pulled     7s    kubelet, 10.95.42.51  Successfully pulled image "pamaibm/pkcs11-proxy:v1"
  Normal  Created    7s    kubelet, 10.95.42.51  Created container
  Normal  Started    7s    kubelet, 10.95.42.51  Started container
```
{: codeblock}


### **Step four:** Configure communication between the proxy and the blockchain components
{: #ibp-hsm-build-pkcs11-proxy-deploy-s4}

After verifying that the pod is running, we need to configure communications between the proxy and the blockchain components by using the Kubernetes service. The `ClusterIP` service is used to ensure that the proxy is not exposed to outside the cluster. If required, you can add networking rules for all members who can access the proxy. Copy the `yaml` below and save it to a file named `service.yaml`.

```yaml
apiVersion: v1
kind: Service
metadata:
  name: pkcs11-proxy
  namespace: hsm
  labels:
    app: pkcs11
spec:
  ports:
  - name: http
    port: 2345
    protocol: TCP
    targetPort: 2345
  selector:
    app: pkcs11
  type: ClusterIP
```
{: codeblock}

Create the `ClusterIP` service by running the command:

```
kubectl create -f service.yaml -n hsm
```
{: codeblock}

When this command is successful you should see something similar to:
```
service/pkcs11-proxy created
```


After creating the service you need to get the ClusterIP address by running the command:
```
kubectl get service -n hsm pkcs11-proxy -o wide
```
{: codeblock}

You will see results similar to:
```
NAME           TYPE        CLUSTER-IP       EXTERNAL-IP   PORT(S)    AGE   SELECTOR
pkcs11-proxy   ClusterIP   172.21.106.217   <none>        2345/TCP   16s   app=pkcs11
```

Save the value of the  `CLUSTER-IP` address and `PORT` because they will be used to form the `HSM proxy endpoint` in the next step.

## Next steps
{: #ibp-hsm-build-pkcs11-proxy-next-steps}  

You are now ready to deploy a new CA, peer, or ordering node that uses the HSM. When you deploy the new node from the console, ensure that you select the advanced deployment option **Hardware security module (HSM)**.

![Configuring a node to use HSM](../images/hsm-cfg.png "Configuring a node to use HSM"){: caption="Figure 1. Configuring a node to use HSM" caption-side="bottom"}

On the HSM configuration panel, enter the following values:
- **HSM proxy endpoint** -Enter the URL for the PKCS #11 proxy that begins with `tcp://` and includes the `CLUSTER-IP` address and `PORT`. For example, `tcp://172.21.106.217:2345`.
- **HSM label** - Enter the name of the HSM partition to be used for this node.
- **HSM PIN** - Enter the PIN for the HSM slot.  

Lastly, on the CA **Summary** panel, you can override the default HSM configuration, for example if you want to customize which crypto library implementation to use. Click **Edit configuration JSON (Advanced)** on the **Summary** panel to view the `JSON`. Scroll down to the `BCCSP (Blockchain Crypto Service Provider) section` where you can modify the crypto library settings. For example, if you are using AWS HSM, you also need to include additional parameters in the BCCSP section of the configuration for your node. See [Fabric documentation](https://hyperledger-fabric.readthedocs.io/en/release-2.2/hsm.html#example) for details.

Because the HSM implementation currently only supports HSMs that implement the PKCS #11 standard, you cannot modify the `bccsp.default` that is set to `PKCS11`.
{: note}

When the node is deployed, a private key for the specified node enroll ID and secret is generated by the HSM and stored securely in the appliance.
