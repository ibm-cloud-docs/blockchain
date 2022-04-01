---

copyright:
  years: 2022
lastupdated: "2022-04-01"

keywords:  monitoring, resource consumption, resource allocation, disk space, memory usage, disk usage, Prometheus, metrics  

subcollection: blockchain

---

{:external: target="_blank" .external}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:codeblock: .codeblock}
{:note: .note}
{:tip: .tip}
{:help: data-hd-content-type='help'}
{:support: data-reuse='support'}
{:download: .download}

# Prometheus metrics
{: #ibp-monitoring-prometheus}
{: help}
{: support}

As an alternative to {{site.data.keyword.mon_full}}, you can use the [Prometheus](https://developer.ibm.com/components/prometheus/) open-source systems monitoring and alerting toolkit to monitor your {{site.data.keyword.blockchainfull_notm}} Platform peer, orderer, and certificate authority (CA) nodes, and smart contract containers.
{: shortdesc}

This tutorial helps you get started using Prometheus to monitor resource usage of peer, CA, and ordering service nodes. It describes how to configure Prometheus to monitor your existing {{site.data.keyword.blockchainfull_notm}} Platform service instance.

Monitoring node resource allocation is critical to operating a blockchain network. You are responsible for monitoring resource usage and taking steps to address any resource contention. Prometheus is a useful tool for monitoring resources, but you can use any comparable alternative.
{: important}

## Before you begin
{: #ibp-monitoring-prometheus-before}

Before using this tutorial, you should have an {{site.data.keyword.blockchainfull_notm}} Platform service instance deployed and configured with peer, CA, and ordering service nodes running and ready for monitoring. You will also use the {{site.data.keyword.blockchainfull_notm}} Platform Console.

## Step one: Create a Sysdig identity to access nodes
{: #ibp-monitoring-prometheus-sysdig}

[Sysdig](https://sysdig.com/) uses mutual Transport Layer Security (TLS) to access cluster nodes. Use your organization’s TLS certificate authority (CA) to generate a private and public key pair for your new Sysdig identity.

For the component you are scraping Prometheus metrics from, select the CA that issued its certificates. Then, click `Register User` to create a new user identity that will access your network from Sysdig. Specify the `Enroll ID` and `Secret` for this user, and set the type to `client`.

Next, click `Enroll Identity` and select the `TLS Certificate Authority` from the CA drop-down list. Enter the `Enroll ID` and `Secret` that you specified in the previous step. When you click `Next`, the subsequent panel contains the certificates generated for your new user identity.

Click `Add identity to Wallet` to store the keys in your console wallet. These certificates are stored in the browser and are not saved by {{site.data.keyword.blockchainfull_notm}} Platform. It is your responsibility to manage the certificates and keep them secure&mdash;click `Export identity` to save the identity to your file system for importing back into the wallet, as needed.

If you have multiple components with certificates issued by the same CA, it is not necessary to create a new identity for each component&mdash;the certificates generated for the identity will be valid for all of these components. To collect metrics for components with certificates issued by a different CA, repeat the previous steps (create a new identity) for this other CA.
{: note}

## Step two: Provision an IBM Cloud Monitoring instance
{: #ibp-monitoring-prometheus-provision}

In this step you will provision an IBM Cloud Monitoring instance, which collects and displays metrics for Kubernetes clusters, including IBM Blockchain Platform Prometheus metrics.

Navigate to the `Catalog` from your IBM Cloud dashboard and search for `IBM Cloud Monitoring`. `Platform Metrics` does not have to be enabled for Prometheus metrics to work. Furthermore, you can choose any region to locate the instance&mdash;the region will not affect the setup.

Once your IBM Cloud Monitoring instance is provisioned, navigate to `Get Started > Install the agent > Add Sources`. This will redirect you to the command for installing `sysdig` agents on your Kubernetes cluster. Log in to your cluster using the CLI and run the `Public Endpoint` curl command to install the agents, if necessary.

## Step three: Configure the TLS secret
{: #ibp-monitoring-prometheus-configure}

Configuring Sysdig created the `ibm-observe` namespace in your Kubernetes cluster. You will repeat this configuration for Prometheus by editing the associated `daemonset` and `configmap`. First, store the downloaded TLS certs in a Kubernetes secret by logging into the cluster (via the CLI) and running the following commands.

Create a TLS secret in the `ibm-observe` namespace by running the following command, where `<cert-path>` and `<key-path>` are the paths to the certs you downloaded to your local file system. Be sure to keep a record of the `<secret-name>` that you use:

```bash
kubectl create secret tls <secret-name> --cert=<cert-path> --key=<key-path> -n ibm-observe
```

If you are setting up Prometheus metrics for components from multiple CAs, run the previous command for each set of generated certificates, making sure to give them each a unique `<secret-name>`.
{: note}

Next, get the current `daemonset` by running:

```bash
kubectl get daemonset sysdig-agent -o yaml > dm-sysdig-agent.yaml -n ibm-observe
```

Edit the `daemonset` by locating `spec.template.spec.containers.volumeMounts` and adding the following section:

```yaml
- mountPath: /<directory-path>
  name: <volume-name>
```

For example:

```yaml
- mountPath: /ibp
  name: ibp-volume
```

If you set up multiple secrets, you should add as many `volumeMounts` as there are secrets, making sure to specify a unique `directory-path` and `volume-name` for each one.
{: note}

Next, edit the `daemonset` by locating `spec.template.spec.containers.volumes` and adding the following section:

```yaml
- name: <volume-name>
  secret:
      defaultMode: 420
      secretName: <secret-name>
```

For example:

```yaml
- name: ibp-volume
  secret:
      defaultMode: 420
      secretName: sysdig-tls-cert-key
```

If you set up multiple `volumeMounts`, you should add as many volumes as there are `volumeMounts`, each using the corresponding unique `name` and `secretName`.
{: note}

Finally, apply the modified `daemonset` by running:

```bash
kubectl apply -f dm-sysdig-agent-modified.yaml
```

## Step four: Configure Prometheus settings
{: #ibp-monitoring-prometheus-configure-settings}

Configure the Prometheus scraper by modifying the `configmap` of the `sysdig-agent`.

First, get the current `configmap` by running:

```bash
kubectl get configmap sysdig-agent -o yaml > cm-sysdig-agent.yaml -n ibm-observe
```

Edit the `configmap` by adding the following lines as a sibling of the `dragent.yaml` section, replacing `<directory-path>` with the path set up in the previous step:

```yaml
prometheus.yaml: |
    global:
      scrape_interval: 10s
    scrape_configs:
    - job_name: 'ibm-blockchain'
      scheme: https
      tls_config:
        insecure_skip_verify: true
        cert_file: /<directory-path>/tls.crt
        key_file: /<directory-path>/tls.key
      metrics_path: /metrics
      kubernetes_sd_configs:
      - role: pod
      relabel_configs:
      - action: keep
        source_labels: [__meta_kubernetes_pod_label_creator]
        regex: ibp
```

For example:

```yaml
prometheus.yaml: |
    global:
      scrape_interval: 10s
    scrape_configs:
    - job_name: 'ibm-blockchain'
      scheme: https
      tls_config:
        insecure_skip_verify: true
        cert_file: /ibp/tls.crt
        key_file: /ibp/tls.key
      metrics_path: /metrics
      kubernetes_sd_configs:
      - role: pod
      relabel_configs:
      - action: keep
        source_labels: [__meta_kubernetes_pod_label_creator]
        regex: ibp
```

To scrape metrics for multiple components, add a new job by duplicating the `job_name: 'ibm-blockchain'` section, specifying a unique `job_name`, and modifying the `cert_file` and `key_file` fields to the correct location.
{: note}

Finally, apply the modified `configmap` by running:

```bash
kubectl apply -f cm-sysdig-agent-modified.yaml
```

The `sysdig-agent` pods should automatically restart when the `daemonset` is applied, but you should still manually restart them after applying the `configmap`, using either the CLI or the Kubernetes dashboard.

## Step five: View Prometheus metrics

View Prometheus metrics by clicking the `Explore` tab in your IBM Cloud Monitoring dashboard. From `Hosts & Containers`, click `Entire Infrastructure`. Expand `Overview by Host` and scroll down to view the `Prometheus` metrics.

Dashboards are useful for saving customizations to a chart for later viewing. Simply create a new IBM Cloud Monitoring dashboard and click `+` to add a new panel, where you can select the metric and segmentation type. The process for creating a new dashboard is described in the [IBM Cloud Monitoring documentation](/docs/blockchain?topic=blockchain-ibp-monitoring#ibp-monitoring-configure-db).
