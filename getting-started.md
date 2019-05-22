---

copyright:
  years: 2014, 2019
lastupdated: "2019-04-08"

keywords: kubernetes, iks

subcollection: containers

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:codeblock: .codeblock}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:deprecated: .deprecated}
{:download: .download}


# Getting started tutorial
{: #getting-started}

Hit the ground running with {{site.data.keyword.containerlong}} by deploying highly available apps in Docker containers that run in Kubernetes clusters.
{:shortdesc}

Containers are a standard way to package apps and all their dependencies so that you can seamlessly move the apps between environments. Unlike virtual machines, containers do not bundle the operating system. Only the app code, run time, system tools, libraries, and settings are packaged inside containers. Containers are more lightweight, portable, and efficient than virtual machines.

## Getting started with clusters
{: #clusters_gs}

So you want to deploy an app in a container? Hold on! Start by creating a Kubernetes cluster first. Kubernetes is an orchestration tool for containers. With Kubernetes, developers can deploy highly available apps in a flash by using the power and flexibility of clusters.
{:shortdesc}

And what is a cluster? A cluster is a set of resources, worker nodes, networks, and storage devices that keep apps highly available. After you have your cluster, then you can deploy your apps in containers.

Note: If you want to use a private-only cluster, you will need to enable VRF on your account.  You can do this by opening a ticket and request that it be enabled.  This will need to be done prior to ordering the private only cluster.  

Before you begin, get the [{{site.data.keyword.Bluemix_notm}} account](https://cloud.ibm.com/registration) type that is right for you:
* **Billable (Pay-As-You-Go or Subscription)**: You can create a free cluster. You can also provision IBM Cloud infrastructure (SoftLayer) resources to create and use in standard clusters.
* **Lite**: You cannot create a free or standard cluster. [Upgrade your account](/docs/account?topic=account-accountfaqs#changeacct) to a billable account.

To create a free cluster:

1.  In the [{{site.data.keyword.Bluemix_notm}} **Catalog** ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cloud.ibm.com/catalog?category=containers), select **{{site.data.keyword.containershort_notm}}** and click **Create**. A cluster configuration page opens. By default, **Free cluster** is selected.

2.  Give your cluster a unique name.

3.  Click **Create Cluster**. A worker pool is created that contains 1 worker node. The worker node can take a few minutes to provision, but you can see the progress in the **Worker nodes** tab. When the status reaches `Ready` you can start working with your cluster!

<br>

Good work! You created your first Kubernetes cluster. Here are some details about your free cluster:

*   **Machine type**: The free cluster has one virtual worker node that is grouped into a worker pool, with 2 CPU, 4 GB memory, and a single 100 GB SAN disk available for your apps to use. When you create a standard cluster, you can choose between physical (bare metal) or virtual machines, along with various machine sizes.
*   **Managed master**: The worker node is centrally monitored and managed by a dedicated and highly available {{site.data.keyword.IBM_notm}}-owned Kubernetes master that controls and monitors all of the Kubernetes resources in the cluster. You can focus on your worker node and the apps that are deployed in the worker node without worrying about managing this master too.
*   **Infrastructure resources**: The resources that are required to run the cluster, such as VLANs and IP addresses, are managed in an {{site.data.keyword.IBM_notm}}-owned IBM Cloud infrastructure (SoftLayer) account. When you create a standard cluster, you manage these resources in your own IBM Cloud infrastructure (SoftLayer) account. You can learn more about these resources and the [permissions needed](/docs/containers?topic=containers-users#infra_access) when you create a standard cluster.
*   **Other options**: Free clusters are deployed within the region that you select, but you cannot choose which zone. For control over zone, networking, and persistent storage, create a standard cluster. [Learn more about the benefits of free and standard clusters](/docs/containers?topic=containers-cs_ov#cluster_types).

<br>

**What's next?**</br>
Try out some things with your free cluster before it expires.

* Go through the [first {{site.data.keyword.containerlong_notm}} tutorial](/docs/containers?topic=containers-cs_cluster_tutorial#cs_cluster_tutorial) for creating a Kubernetes cluster, installing the CLI or using the Kubernetes Terminal, creating a private registry, setting up your cluster environment, and adding a service to your cluster.
* Keep up your momentum with the [second {{site.data.keyword.containerlong_notm}} tutorial](/docs/containers?topic=containers-cs_apps_tutorial#cs_apps_tutorial) about deploying apps to the cluster.
* [Create a standard cluster](/docs/containers?topic=containers-clusters#clusters_ui) with multiple nodes for higher availability.


