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



# IBM Cloud HSM PKCS #11 proxy
{: #ibp-hsm-build-pkcs11-proxy-ic}

Learn how to configure a PKCS #11 proxy to work with the {{site.data.keyword.cloud_notm}} HSM.
{: shortdesc}

Use of this process has been deprecated in favor of using an [HSM client image](/docs/blockchain?topic=blockchain-ibp-console-adv-deployment#ibp-console-adv-deployment-hsm-build-docker).
{: deprecated}

## Build PKCS #11 Docker image
{: #ibp-hsm-build-pkcs11-proxy-ic-build}

This process involves deploying an **HSM** and configuring a partition as well as deploying an **HSM client** that you will use to configure the device. Throughout these instructions you will run some commands from the HSM server and others from the HSM client. For clarity, we prefix these steps with either <img src="../images/icon-hsm-2.png" alt="HSM server" width="30" style="width:30px; border-style: none"/> or <img src="../images/icon-hsm-client.png" alt="HSM client" width="30" style="width:30px; border-style: none"/>. These steps assume you have already deployed an HSM either from  [{{site.data.keyword.cloud_notm}}](https://cloud.ibm.com/catalog/infrastructure/hardware-security-module){: external} or your own PKCS #11 compliant HSM. The value of the HSM `partition` and `PIN` is required.

Build a PKCS #11 Docker image that contains the HSM client that will run on your Kubernetes cluster. But before you can build the image, two files are required on the client: the `docker-entrypoint.sh` and the Docker image file.

1. <img src="../images/icon-hsm-client.png" alt="HSM client" width="30" style="width:30px; border-style: none"/> First, copy and save the following text to a file named `docker-entrypoint.sh`. You do not need to make any changes to this file.

  ```bash
  #!/bin/bash -ex

  # CLIENT_ADDRESS - address where the client is running
  # HSM_ADDRESS - address where the HSM server is running

  # add the server
  vtl addServer -n ${HSM_ADDRESS} -c /configs/server.pem

  # create fake certs for client for lunaclient to register the addresses
  # in the config
  vtl createcert -n ${CLIENT_ADDRESS}

  # copy the certs mounted to the location where the client looks
  # for them
  cp /configs/cert.pem /usr/safenet/lunaclient/cert/client/${CLIENT_ADDRESS}.pem
  cp /configs/key.pem /usr/safenet/lunaclient/cert/client/${CLIENT_ADDRESS}Key.pem

  # finally verify that the connection worked
  vtl verify

  # start the pkcs11 proxy
  pkcs11-daemon $LIBRARY_LOCATION -
  ```
  {: codeblock}

2. <img src="../images/icon-hsm-client.png" alt="HSM client" width="30" style="width:30px; border-style: none"/> Next, save the following text as a Docker file on your client. (The command in the subsequent step refers to this Docker file with the name of `test`.)

  - You should be aware that this Docker file automatically accepts the Gemalto client license.
  - Note that the `64` folder inside the Docker file is required for installing the HSM client.

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

  ########## FINAL image - Build luna client ##########

  FROM registry.access.redhat.com/ubi8/ubi-minimal

  ## This directory contains the installation files for gemalto/luna client
  COPY 64 64

  RUN microdnf install -y \
  	gcc \
  	gcc-c++ \
  	openssh-clients \
  	bind-utils \
  	iputils \
  	&& cd 64 && \
  	# NOTE we are accepting the license for installing gemalto client here
  	# please take a look at the license before moving forward
  	echo "y" | ./install.sh -p sa

  COPY --from=builder /usr/local/bin/pkcs11-daemon /usr/local/bin/pkcs11-daemon
  COPY --from=builder /usr/local/lib/libpkcs11-proxy.so.0.1 /usr/local/lib/libpkcs11-proxy.so.0.1
  COPY --from=builder /usr/local/lib/libpkcs11-proxy.so.0 /usr/local/lib/libpkcs11-proxy.so.0
  COPY --from=builder /usr/local/lib/libpkcs11-proxy.so /usr/local/lib/libpkcs11-proxy.so

  WORKDIR /

  COPY docker-entrypoint.sh docker-entrypoint.sh
  RUN chmod +x docker-entrypoint.sh

  ENV PKCS11_DAEMON_SOCKET="tcp://0.0.0.0:2345"
  ENV PATH="$PATH:/usr/safenet/lunaclient/bin"
  ENV LIBRARY_LOCATION=/usr/safenet/lunaclient/lib/libCryptoki2_64.so

  EXPOSE 2345

  ENTRYPOINT [ "sh", "-c", "./docker-entrypoint.sh" ]
  ```
  {: codeblock}

3. <img src="../images/icon-hsm-client.png" alt="HSM client" width="30" style="width:30px; border-style: none"/> Now, run the following command to build the Docker image:

   ```
   docker build -t test -f Dockerfile .
   ```
   {: codeblock}

4. <img src="../images/icon-hsm-client.png" alt="HSM client" width="30" style="width:30px; border-style: none"/> Before you deploy the Docker image to your Kubernetes cluster, it is recommended that you first try to run the image locally. Run the following command to verify that the image was built successfully:

   ```
   docker run -it -e HSM_ADDRESS=<HSM_ADDRESS> -e CLIENT_ADDRESS=<CLIENT_ADDRESS> -v configs/:/configs test
   ```
   {: codeblock}

   Replace
   - `<HSM_ADDRESS>` with the IP address of the HSM.
   - `<CLIENT_ADDRESS>` with either the IP address or fully qualified host name of the client.

  The output of this command looks similar to:
  ```
  $ docker run -it -e HSM_ADDRESS=<HSM_ADDRESS> -e CLIENT_ADDRESS=<CLIENT_ADDRESS> -v ${PWD}/configs/:/configs test
  + vtl addServer -n 10.208.66.177 -c /configs/server.pem

  New server 10.208.66.177 successfully added to server list.

  + vtl createcert -n 10.220.203.73
  Private Key created and written to: /usr/safenet/lunaclient/cert/client/10.220.203.73Key.pem
  Certificate created and written to: /usr/safenet/lunaclient/cert/client/10.220.203.73.pem
  + cp /configs/cert.pem /usr/safenet/lunaclient/cert/client/10.220.203.73.pem
  + cp /configs/key.pem /usr/safenet/lunaclient/cert/client/10.220.203.73Key.pem
  + vtl verify

  The following Luna SA Slots/Partitions were found:

  Slot	Serial #        	Label
  ====	================	=====
     0	       500752010 	partition1

  + pkcs11-daemon /usr/safenet/lunaclient/lib/libCryptoki2_64.so -
  pkcs11-proxy[12]: Listening on: tcp://0.0.0.0:2345
  ```

## Deploy the Docker image onto your Kubernetes cluster
{: #ibp-hsm-build-pkcs11-proxy-ic-deploy-k8s}  

After the local test in the previous step is successful, you are ready to deploy the Docker image to your Kubernetes cluster. In order to deploy the image, you need to create the `hsm` namespace, a Kubernetes secret, as well as the `service.yaml` and `deployment.yaml` files.
1. <img src="../images/icon-hsm-client.png" alt="HSM client" width="30" style="width:30px; border-style: none"/> Create a namespace for HSM on your Kubernetes cluster:

  ```
  kubectl create ns hsm
  ```
  {: codeblock}

2. <img src="../images/icon-hsm-client.png" alt="HSM client" width="30" style="width:30px; border-style: none"/> Create a Kubernetes secret in the `hsm` namespace:  
  Create an image pull secret named `docker-pull-secret` for pulling the image from DockerHub which allows you to deploy containers to Kubernetes namespaces other than `default`. You will use the name of this secret in the `deployment.yaml` file in a subsequent step.

  ```
  kubectl create secret docker-registry docker-pull-secret --docker-username=<DOCKER_HUB_ID> --docker-password=<DOCKER_HUB_PWD> --docker-email=<EMAIL> --docker-server=<<DOCKER_HUB_ID>:pkcs11-proxy:v1 -n hsm
  ```
  {: codeblock}

  - Replace `<DOCKER_HUB_ID>` with the Docker user name.
  - Replace `<DOCKER_HUB_PWD>` with the Docker password.
  - Replace `<EMAIL>` with your DockerHub email address.

  For example:
  ```
  kubectl create secret docker-registry docker-pull-secret --docker-username=dockeruser --docker-password=dockerpwd --docker-email=dockeruser@example.com --docker-server=dockeruser/pkcs11-proxy:v1 -n hsm
  ```
  {: codeblock}
    These instructions are obviously for the Docker registry. If you are using the {{site.data.keyword.IBM_notm}} Container Registry, then you need to set up your own image pull secret in your cluster (doing so will allow you to deploy containers to Kubernetes namespaces other than default). This also implies that you'll need to define a corresponding image pull secret entry in the deployment YAML file. See the following links for further details:

      - [Using an image pull secret to access images in other IBM Cloud accounts or external private registries from non-default Kubernetes namespaces](/docs/containers?topic=containers-registry#other)
      - [Copying an existing image pull secret](/docs/containers?topic=containers-registry#copy_imagePullSecret)
      - [Referring to the image pull secret in your pod deployment](/docs/containers?topic=containers-images#pod_imagePullSecret)
3. <img src="../images/icon-hsm-client.png" alt="HSM client" width="30" style="width:30px; border-style: none"/>  Copy and paste the following text to a file named `service.yaml`:

  ```yaml
  apiVersion: v1
  kind: Service
  metadata:
    name: pkcs11-proxy
    namespace: hsm
    labels:
      app: <LABEL>
  spec:
    ports:
    - name: http
      port: 2345
      protocol: TCP
      targetPort: 2345
    selector:
      app: <LABEL>
    type: ClusterIP
  ```
  {: codeblock}

  Replace
  `<LABEL>` with any value you want to use to represent this proxy. You need to use the same value in the `service.yaml` and the `deployment.yaml` in the next step.

  If you are setting up multiple partitions and proxies, the value of the `<LABEL>` and `metadata.name` parameters need to be unique across proxies.
  {: note}

4. <img src="../images/icon-hsm-client.png" alt="HSM client" width="30" style="width:30px; border-style: none"/> Copy and paste the following text to a file named `deployment.yaml`:

  ```yaml
  apiVersion: apps/v1
  kind: Deployment
  metadata:
    name: pkcs11-proxy
    namespace: hsm
    labels:
      app: <LABEL>
  spec:
    replicas: 1
    selector:
      matchLabels:
        app: <LABEL>
    template:
      metadata:
        labels:
          app: <LABEL>
      spec:
        imagePullSecrets:
        - name: <DOCKER-PULL-SECRET>
        containers:
        - name: proxy
          image: <DOCKER-IMAGE>
          imagePullPolicy: Always
          resources:
            requests:
              cpu: "0.2"
              memory: "400Mi"
            limits:
              cpu: "0.2"
              memory: "400Mi"
          env:
            - name: LICENSE
              value: accept
            - name: HSM_ADDRESS
              value: <HSM_ADDRESS>
            - name: CLIENT_ADDRESS
              value: <CLIENT_ADDRESS>
          volumeMounts:
          - name: config-volume
            mountPath: /configs
        volumes:
        - name: config-volume
          configMap:
            name: gemalto-config
          # readinessProbe:
          #   tcpSocket:
          #     port: 2345
          #   initialDelaySeconds: 5
          #   periodSeconds: 10
          # livenessProbe:
          #   tcpSocket:
          #     port: 2345
          #   initialDelaySeconds: 15
          #   periodSeconds: 20
  ```
  {: codeblock}

  Replace
  - `<LABEL>` with same value you specified in the `service.yaml`.
  - `<DOCKER-IMAGE>` with Docker image that you created in [Part Four of the IBM Cloud Hardware Security Module (HSM) instructions](/docs/blockchain?topic=blockchain-ibp-hsm-gemalto#ibp-hsm-gemalto-part-four). For example: `mydockerhub/ibp-pkcs11proxy:latest`.
  - `<DOCKER-PULL-SECRET>` the name of the Kubernetes secret you created in the previous step.
  - `<HSM_ADDRESS>` with the IP address of the HSM.
  - `<CLIENT_ADDRESS>` with either the IP address or fully qualified host name of the client.

  If you are setting up multiple partitions and proxies, the value of <LABEL> and `metadata.name` parameters need to be unique across proxies.
  {: tip}

  When you create this deployment on your Kubernetes infrastructure, Kubernetes will attempt to download your Docker image from the specified image registry. You could replace `<DOCKER-IMAGE>` with something similar to `us.icr.io/ns/hsm-proxy:latest`. This tells the Kubernetes environment that the hsm-proxy:latest image should be downloaded from a server whose hostname is `us.icr.io`.

  If you are deploying to a Kubernetes cluster on {{site.data.keyword.cloud_notm}}, then more than likely you will be using the IBM Container Registry as your private image repository. For details on how to leverage this service for hosting your images, see [Setting up an image registry](/docs/containers?topic=containers-registry){: external}.

5. <img src="../images/icon-hsm-client.png" alt="HSM client" width="30" style="width:30px; border-style: none"/> Now, run the following commands using the Kubernetes CLI from your HSM client:

  ```
  # Create configmap on cluster
  kubectl create cm -n hsm gemalto-config --from-file=configs/server.pem --from-file=configs/cert.pem --from-file=configs/key.pem

  # Create service on cluster
  kubectl create -f service.yaml

  # Create deployment on cluster
  kubectl create -f deployment.yaml
  ```
  {: codeblock}

6. <img src="../images/icon-hsm-client.png" alt="HSM client" width="30" style="width:30px; border-style: none"/> In order to use the HSM, the {{site.data.keyword.blockchainfull_notm}} Platform needs the address of the PCKS #11 proxy. The combination of the **cluster-ip address** of the PCKS #11 proxy and the associated port form the PCKS #11 proxy address that is required by console when you configure a node to use the HSM.  Again, run the following command using the Kubernetes CLI from your HSM client:

  ```
  $ kubectl get service -n hsm
  ```
  {: codeblock}

  The output of this command looks similar to:

  ```
  NAME            TYPE        CLUSTER-IP       EXTERNAL-IP   PORT(S)    AGE
  pkcs11-proxy1   ClusterIP   172.21.191.187   <none>        2345/TCP   5d3h
  ```

  Combine the value of the **CLUSTER-IP** address above `172.21.191.187` and **PORT** `2345` to build the PCKS #11 proxy address that is required by console in the form `tcp://172.21.191.187:2345`.

  Save the value of the PCKS #11 proxy address for later when you configure a blockchain node to use HSM as it represents the **HSM proxy endpoint**.
  {: important}

## Next steps
{: #ibp-hsm-build-pkcs11-proxy-ic-next-steps}  

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
