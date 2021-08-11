---

copyright:
  years: 2017, 2021
lastupdated: "2021-08-11"


keywords: IBM Blockchain offerings, Linux Foundation, Hyperledger Fabric, open source, community contribution

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



# Disclaimer
{: #disclaimer}

**ATTENTION:** You must review the following information before you use any {{site.data.keyword.blockchainfull}} plans.

## {{site.data.keyword.IBM_notm}} support statement
{: #disclaimer-support-statement}

{{site.data.keyword.IBM_notm}}'s long history of leadership in innovation continues with the {{site.data.keyword.blockchainfull_notm}} Platform offerings on {{site.data.keyword.cloud_notm}}. Blockchain is a rapidly progressing technology that is projected to disrupt the financial industry, local and global supply chains, and logistical support in any number of business spaces. Through various early adoption programs, {{site.data.keyword.IBM_notm}} customers and business partners are actively driving blockchain as an industrial solution. {{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}} is one such program. **As with any new technology, {{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}} users need to be aware of the potential for rapid and fundamental change**.
{: shortdesc}

The architecture behind {{site.data.keyword.blockchainfull_notm}} is the Linux Foundation's Hyperledger Fabric project. Each open-source community contribution improves Hyperledger Fabric but can present adoption challenges. **{{site.data.keyword.IBM_notm}} cautions against defining or exchanging financial assets directly on any Hyperledger Fabric blockchain network**.

{{site.data.keyword.IBM_notm}} does not support networks that use Hyperledger Composer in production, including the Composer CLI, JavaScript APIs, REST server, and Web Playground.
{: note}

## Open-source statement
{: #disclaimer-open-source-statement}

{{site.data.keyword.blockchainfull_notm}} offerings are built based on the Linux Foundation's Hyperledger Fabric stack. Hyperledger Project members, including {{site.data.keyword.IBM_notm}}, continue to contribute to various subprojects under the Hyperledger umbrella.  All contributions are reviewed, vetted, and tested by the community.

## Chaincode support statement
{: #disclaimer-chaincode-support-statement}

The following coding practices are NOT supported on {{site.data.keyword.blockchainfull_notm}} networks:

1. Using associative arrays with iteration (the order is randomized in Go).
2. Reading a list of items from a KVS table (the order isn't guaranteed).
3. Writing thread-unsafe chaincode (query and invoke might be called in parallel).
4. Substituting global memory or cache storage for ledger state variables in chaincode.
5. Accessing external services, such as databases, directly from chaincode.
6. Using libraries or global variables that could introduce non-determinism (such as using "random" or "time").

Lastly, it is not recommended to write non-deterministic chaincode, which introduces risk to data consistency and integrity. The Hyperledger Fabric architecture is designed to counter against non-deterministic chaincode through a series of endorsement and validation checks. However, you are still urged to code deterministic functions that are not reliant on non-static global variables, such as time.
