---

copyright:
  years: 2020
lastupdated: "2020-06-16"

keywords: ansible playbooks, docker image, blockchain network, APIs, ansible galaxy

subcollection: blockchain

---

{:external: target="_blank" .external}
{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:javascript: data-hd-programlang="javascript"}
{:java: data-hd-programlang="java"}
{:screen: .screen}
{:note: .note}
{:important: .important}
{:tip: .tip}
{:help: data-hd-content-type='help'}
{:support: data-reuse='support'}
{:pre: .pre}

# Getting started with Ansible playbooks on the IBM Blockchain Platform
{: #ansible}

Customers who are interested in automating their {{site.data.keyword.blockchainfull}} Platform network deployments can use Ansible playbooks to  build out their networks.
{:shortdesc}

**Target audience:** This topic is designed for network creators and system administrators who are responsible for planning and configuring {{site.data.keyword.blockchainfull_notm}} networks on {{site.data.keyword.cloud_notm}} or on their own Kubernetes cluster and are new to Ansible playbooks.

## What is Ansible
{: #ansible-whatis}

Ansible is a powerful open source automation language and in the context of the {{site.data.keyword.blockchainfull_notm}} Platform can be used to script deployments. It is a configuration management tool that automates the network deployment across multiple hosts by the use of Ansible **playbooks**.
Playbooks declare configurations and orchestrate the steps of any manual ordered process, synchronously or asynchronously.

Each playbook is composed of one or more **plays** in a list. The goal of a playbook is to map a group of hosts to some well-defined roles, represented by actions that Ansible calls **tasks**. Bascially, a task is simply a call to an Ansible module.

Ansible Content Collections, or simply **collections**, are the mechanism for distributing, maintaining, and consuming the automation. Combining multiple types of Ansible content (playbooks, roles, modules, and plug-ins) into a collection, adds flexibility and scalability for automating your blockchain network deployment. The open source collections are published in [Ansible Galaxy](https://galaxy.ansible.com/){: external} where they can be freely downloaded. Check out their [documentation](https://galaxy.ansible.com/docs/) for more education.

{{site.data.keyword.blockchainfull_notm}} Platform offers an Ansible collection that includes the pertinent roles, modules, and documentation for rapidly deploying, replicating, and tearing down blockchain networks in a reproducible fashion.


The Ansible collection includes a set of playbooks that automate the tasks that you would otherwise perform from the console UI or by using the APIs, for example deploy a Certificate Authority (CA). But the playbooks are more robust than just that. The playbooks automate a logical series of tasks that you would repeatedly perform in the console, for example create a CA, register identities, create an organization MSP definition, and create a peer. And all of these steps can be performed by running a single playbook..

This tutorial series is designed for users who are unfamiliar with how to use Ansible playbooks. Experienced Ansible users can go directly to the collection [documentation](https://ibm-blockchain.github.io/ansible-collection/){: external} to get started.
{: tip}

## How does the {{site.data.keyword.blockchainfull_notm}} Platform Ansible collection work?
{: #ansible-how}

The {{site.data.keyword.blockchainfull_notm}} Platform Ansible collection uses the [IBM Blockchain Platform REST APIs](https://cloud.ibm.com/apidocs/blockchain). It includes an entire set of playbooks that can be used to deploy your peer, Certificate Authority (CA), and ordering service nodes, create organizations, identities, channels, and install and instantiate smart contracts. And when you need to start over, there are scripts for tearing it all back down. These playbooks will run on your blockchain network that is running on {{site.data.keyword.cloud_notm}} or on your own Kubernetes cluster where the {{site.data.keyword.blockchainfull_notm}} Platform is installed.

## Why would I want to use an Ansible playbook?
{: #ansible-why}

The Ansible playbooks include a set of modules that you can use to automate the deployment of your nodes, registering of identities, channel creation, and smart contracts installation and instantiation.

After you deploy the {{site.data.keyword.blockchainfull_notm}} Platform to a Kubernetes cluster on {{site.data.keyword.cloud_notm}}, or on your supported Kubernetes distribution, there are three ways to build your network. To understand the benefit of using the Ansible playbooks, we compare the options:

  1. **Deploy your nodes and configure your network manually using the console user interface.** This option is most useful for getting started and learning about the platform and how the console works. It requires stepping through multiple panels to configure all the settings for your nodes, organizations, channels, and smart contracts.

  2. **Use the {{site.data.keyword.blockchainfull_notm}} Platform REST APIs to build your network.** This option is for advanced users who want to leverage the available REST APIs, often in conjunction with the Hyperledger Fabric APIs, to set up and administer their blockchain network. Configuration is done through editing API parameters. Because the {{site.data.keyword.blockchainfull_notm}} Platform APIs are mainly for create, retrieve, update, and delete operations on a node, you may need to also learn and use the **Hyperledger Fabric APIs** for some tasks.

  3. **Use the {{site.data.keyword.blockchainfull_notm}} Platform Ansible Collection**. After you are familiar with the process to set up the various nodes, organizations, channels, and smart contracts, you might want to automate the process. The Ansible playbooks provide a simple, straightforward, way to build and deploy a reproducible network. For getting started, a set of scripts are provided that stitches the relevant playbooks together into a single executable shell script of deployments to perform on your network. Configuration is done through editing `.yml` files.  The Ansible playbooks also automate some of the tasks performed by the Fabric APIs, such as creating a channel, joining a peer to a channel, and installing and instantiating smart contracts. Even more, there are playbooks available for tearing down the configuration so you can reset your network as needed. This option is a great option for setting up networks for POCs, demos, development, and test networks.

  The Ansible playbooks provide an easy-to-use, automated process to replicate your blockchain networks on-demand. All components that are deployed with the playbooks are also visible in the console where you can interact with them as normal and govern your network.

## Considerations and Limitations
{: #ansible-limits}

**Console wallet identities**  

When you use the Ansible playbooks to deploy blockchain nodes, the identities that are required to operate the nodes in the network are generated for you. Don't worry, you have the opportunity to configure the node enroll ID and secrets. But the identities aren't added to the console wallet in the browser. That is a simple manual import step that you need to perform after the playbooks finish and is described in the  [tutorial](/docs/blockchain?topic=blockchain-ansible-build#ansible-build-next-steps) on using the playbooks to build your network.

## Getting started
{: #ansible-getting-started}


There are multiple ways to install the Ansible collection. One option is to install all of the prerequisite software locally, which can be cumbersome for first time Ansible users. Another simpler option is to run the playbooks from a Docker image. Because using a Docker image completely bypasses the need for the prerequisite setup (the prereqs are part of the Docker image), this approach is by far the simplest option and is the process that we use throughout these tutorials. This option does require that you install Docker before proceeding. To check to see if you already have Docker installed, run the command `docker --version`. If it is installed, you'll see something similar to:

  ```
  Docker version 18.09.2, build 6247962
  ```
If you do not have Docker installed, see [install Docker](https://docs.docker.com/get-docker/){: external} to download and install it. You can also check out the [Docker site](https://www.docker.com/){: external} for more education and documentation.

Advanced users can review the [Ansible collection documentation](https://ibm-blockchain.github.io/ansible-collection/installation.html){: external} for other installation options.



### Build the Docker image
{: #ansible-docker-build}



When you build a Docker image, you are creating an executable container that includes all of the prerequisites to run the Ansible playbooks.

1. Copy the sample Dockerfile from [GitHub](https://github.com/IBM-Blockchain/ansible-collection/blob/master/docker/Dockerfile){: external} and save it to your local system as `Dockerfile`.

2. Next, use the Dockerfile to build the Docker image. The following command builds the image and tags it with the name `mydockerorg/ansible`. We reference that tag later when we run the Docker image.
  ```
  docker build -t mydockerorg/ansible -f Dockerfile .

  ```
  {: codeblock}

  The command can take several minutes to complete while it downloads and installs all of the prerequisites to the image.

## Next steps
{: #ansible-next-steps}

After the image is successfully built, it can be used to run a playbook. To get started with the playbooks see [Building an {{site.data.keyword.blockchainfull_notm}} Platform network using Ansible playbooks](/docs/blockchain?topic=blockchain-ansible-build) if you already have an existing instance of the {{site.data.keyword.blockchainfull_notm}} Platform deployed and want to use the Ansible playbooks to deploy your network components.

## Where to find support
{: #ansible-support}

Ansible is an open source technology, therefore the Ansible playbooks are not officially supported by {{site.data.keyword.IBM_notm}}. For support related to the usage of the {{site.data.keyword.blockchainfull_notm}} Platform and Ansible playbooks open an issue in the [GitHub repository](https://github.com/IBM-Blockchain/ansible-role-blockchain-platform-manager/issues){: external}

