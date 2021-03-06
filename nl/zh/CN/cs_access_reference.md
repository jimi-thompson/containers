---

copyright:
  years: 2014, 2019
lastupdated: "2019-03-21"

keywords: kubernetes, iks

subcollection: containers

---

{:new_window: target="blank"}
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


# 用户访问许可权
{: #access_reference}

[分配集群许可权](/docs/containers?topic=containers-users)时，很难判断需要将哪个角色分配给用户。请使用以下各部分中的表来确定在 {{site.data.keyword.containerlong}} 中执行常见任务所需的最低许可权级别。
{: shortdesc}

自 2019 年 1 月 30 日开始，{{site.data.keyword.containerlong_notm}} 采用新的方式通过 {{site.data.keyword.Bluemix_notm}} IAM：[服务访问角色](#service)来对用户授权。这些服务角色用于授予对集群中资源（例如，Kubernetes 名称空间）的访问权。有关更多信息，请查看博客 [Introducing service roles and namespaces in IAM for more granular control of cluster access ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://www.ibm.com/blogs/bluemix/2019/02/introducing-service-roles-and-namespaces-in-iam-for-more-granular-control-of-cluster-access/)。
{: note}

## {{site.data.keyword.Bluemix_notm}} IAM 平台角色
{: #iam_platform}

{{site.data.keyword.containerlong_notm}} 已配置为使用 {{site.data.keyword.Bluemix_notm}} Identity and Access Management (IAM) 角色。{{site.data.keyword.Bluemix_notm}} IAM 平台角色确定用户可对集群、工作程序节点和 Ingress 应用程序负载均衡器等 {{site.data.keyword.Bluemix_notm}} 资源执行的操作。此外，{{site.data.keyword.Bluemix_notm}} IAM 平台角色还会自动为用户设置基本基础架构许可权。要设置平台角色，请参阅[分配 {{site.data.keyword.Bluemix_notm}} IAM 平台许可权](/docs/containers?topic=containers-users#platform)。
{: shortdesc}

以下每个部分中的各表显示了每个 {{site.data.keyword.Bluemix_notm}} IAM 平台角色授予的集群管理、日志记录和 Ingress 许可权。这些表按 CLI 命令名称以字母顺序进行组织。

* [无需许可权的操作](#none-actions)
* [查看者操作](#view-actions)
* [编辑者操作](#editor-actions)
* [操作员操作](#operator-actions)
* [管理员操作](#admin-actions)

### 无需许可权的操作
{: #none-actions}

您帐户中运行 CLI 命令或对下表中的操作发出 API 调用的任何用户都会看到相应结果，即使没有为该用户分配任何许可权也是如此。
{: shortdesc}

<table>
<caption>在 {{site.data.keyword.containerlong_notm}} 中无需许可权的 CLI 命令和 API 调用概述</caption>
<thead>
<th id="none-actions-action">集群管理操作</th>
<th id="none-actions-cli">CLI 命令</th>
<th id="none-actions-api">API 调用</th>
</thead>
<tbody>
<tr>
<td>查看 {{site.data.keyword.containerlong_notm}} 的 API 端点或将其设定为目标。</td>
<td><code>[ibmcloud ks api](/docs/containers?topic=containers-cs_cli_reference#cs_api)</code></td>
<td>-</td>
</tr>
<tr>
<td>查看支持的命令和参数的列表。</td>
<td><code>[ibmcloud ks help](/docs/containers?topic=containers-cs_cli_reference#cs_help)</code></td>
<td>-</td>
</tr>
<tr>
<td>初始化 {{site.data.keyword.containerlong_notm}} 插件或指定要在其中创建或访问 Kubernetes 集群的区域。</td>
<td><code>[ibmcloud ks init](/docs/containers?topic=containers-cs_cli_reference#cs_init)</code></td>
<td>-</td>
</tr>
<tr>
<td>查看 {{site.data.keyword.containerlong_notm}} 中支持的 Kubernetes 版本列表。</td><td><code>[ibmcloud ks kube-versions](/docs/containers?topic=containers-cs_cli_reference#cs_kube_versions)</code></td>
<td><code>[GET /v1/kube-versions](https://containers.cloud.ibm.com/swagger-api/#!/util/GetKubeVersions)</code></td>
</tr>
<tr>
<td>查看可用于工作程序节点的机器类型的列表。</td>
<td><code>[ibmcloud ks machine-types](/docs/containers?topic=containers-cs_cli_reference#cs_machine_types)</code></td>
<td><code>[GET /v1/datacenters/{datacenter}/machine-types](https://containers.cloud.ibm.com/swagger-api/#!/util/GetDatacenterMachineTypes)</code></td>
</tr>
<tr>
<td>查看 IBM 标识用户的当前消息。</td>
<td><code>[ibmcloud ks messages](/docs/containers?topic=containers-cs_cli_reference#cs_messages)</code></td>
<td><code>[GET /v1/messages](https://containers.cloud.ibm.com/swagger-api/#!/util/GetMessages)</code></td>
</tr>
<tr>
<td>查找当前所在的 {{site.data.keyword.containerlong_notm}} 区域。</td>
<td><code>[ibmcloud ks region](/docs/containers?topic=containers-cs_cli_reference#cs_region)</code></td>
<td>-</td>
</tr>
<tr>
<td>设置 {{site.data.keyword.containerlong_notm}} 的区域。</td>
<td><code>[ibmcloud ks region-set](/docs/containers?topic=containers-cs_cli_reference#cs_region-set)</code></td>
<td>-</td>
</tr>
<tr>
<td>列出可用的区域。</td>
<td><code>[ibmcloud ks regions](/docs/containers?topic=containers-cs_cli_reference#cs_regions)</code></td>
<td><code>[GET /v1/regions](https://containers.cloud.ibm.com/swagger-api/#!/util/GetRegions)</code></td>
</tr>
<tr>
<td>查看可用于在其中创建集群的专区的列表。</td>
<td><code>[ibmcloud ks zones](/docs/containers?topic=containers-cs_cli_reference#cs_datacenters)</code></td>
<td><code>[GET /v1/zones](https://containers.cloud.ibm.com/swagger-api/#!/util/GetZones)</code></td>
</tr>
</tbody>
</table>

### 查看者操作
{: #view-actions}

**查看者**平台角色包含[无需许可权的操作](#none-actions)以及下表中显示的许可权。
{: shortdesc}

<table>
<caption>在 {{site.data.keyword.containerlong_notm}} 中需要查看者平台角色的集群管理 CLI 命令和 API 调用概述</caption>
<thead>
<th id="view-actions-mngt">集群管理操作</th>
<th id="view-actions-cli">CLI 命令</th>
<th id="view-actions-api">API 调用</th>
</thead>
<tbody>
<tr>
<td>查看资源组和区域的 {{site.data.keyword.Bluemix_notm}} IAM API 密钥所有者的姓名和电子邮件地址。</td>
<td><code>[ibmcloud ks api-key-info](/docs/containers?topic=containers-cs_cli_reference#cs_api_key_info)</code></td>
<td><code>[GET /v1/logging/{idOrName}/clusterkeyowner](https://containers.cloud.ibm.com/swagger-logging/#!/logging/GetClusterKeyOwner)</code></td>
</tr>
<tr>
<td>下载 Kubernetes 配置数据和证书，以连接到集群并运行 `kubectl` 命令。</td>
<td><code>[ibmcloud ks cluster-config](/docs/containers?topic=containers-cs_cli_reference#cs_cluster_config)</code></td>
<td><code>[GET /v1/clusters/{idOrName}/config](https://containers.cloud.ibm.com/swagger-api/#!/clusters/GetClusterConfig)</code></td>
</tr>
<tr>
<td>查看集群的信息。</td>
<td><code>[ibmcloud ks cluster-get](/docs/containers?topic=containers-cs_cli_reference#cs_cluster_get)</code></td>
<td><code>[GET /v1/clusters/{idOrName}](https://containers.cloud.ibm.com/swagger-api/#!/clusters/GetCluster)</code></td>
</tr>
<tr>
<td>列出绑定到某个集群的所有名称空间中的所有服务。</td>
<td><code>[ibmcloud ks cluster-services](/docs/containers?topic=containers-cs_cli_reference#cs_cluster_services)</code></td>
<td><code>[GET /v1/clusters/{idOrName}/services](https://containers.cloud.ibm.com/swagger-api/#!/clusters/ListServicesForAllNamespaces)</code></td>
</tr>
<tr>
<td>列出所有集群。</td>
<td><code>[ibmcloud ks clusters](/docs/containers?topic=containers-cs_cli_reference#cs_clusters)</code></td>
<td><code>[GET /v1/clusters](https://containers.cloud.ibm.com/swagger-api/#!/clusters/GetClusters)</code></td>
</tr>
<tr>
<td>获取为 {{site.data.keyword.Bluemix_notm}} 帐户设置用于访问其他 IBM Cloud Infrastructure (SoftLayer) 产品服务组合的基础架构凭证。</td>
<td><code>[        ibmcloud ks credential-get
        ](/docs/containers?topic=containers-cs_cli_reference#cs_credential_get)</code></td><td><code>[GET /v1/credentials](https://containers.cloud.ibm.com/swagger-api/#!/accounts/GetUserCredentials)</code></td>
</tr>
<tr>
<td>列出绑定到特定名称空间的所有服务。</td>
<td>-</td>
<td><code>[GET /v1/clusters/{idOrName}/services/{namespace}](https://containers.cloud.ibm.com/swagger-api/#!/clusters/ListServicesInNamespace)</code></td>
</tr>
<tr>
<td>列出绑定到某个集群的所有由用户管理的子网。</td>
<td>-</td>
<td><code>[GET /v1/clusters/{idOrName}/usersubnets](https://containers.cloud.ibm.com/swagger-api/#!/clusters/GetClusterUserSubnet)</code></td>
</tr>
<tr>
<td>列出基础架构帐户中的可用子网。</td>
<td><code>[ibmcloud ks subnets](/docs/containers?topic=containers-cs_cli_reference#cs_subnets)</code></td>
<td><code>[GET /v1/subnets](https://containers.cloud.ibm.com/swagger-api/#!/properties/ListSubnets)</code></td>
</tr>
<tr>
<td>查看基础架构帐户的 VLAN 生成状态。</td>
<td><code>[ibmcloud ks vlan-spanning-get](/docs/containers?topic=containers-cs_cli_reference#cs_vlan_spanning_get)</code></td>
<td><code>[GET /v1/subnets/vlan-spanning](https://containers.cloud.ibm.com/swagger-api/#!/accounts/GetVlanSpanning)</code></td>
</tr>
<tr>
<td>为一个集群设置时：列出该集群在一个专区中连接到的 VLAN。</br>为帐户中的所有集群设置时：列出一个专区中的所有可用 VLAN。</td>
<td><code>[ibmcloud ks vlans](/docs/containers?topic=containers-cs_cli_reference#cs_vlans)</code></td>
<td><code>[GET /v1/datacenters/{datacenter}/vlans](https://containers.cloud.ibm.com/swagger-api/#!/properties/GetDatacenterVLANs)</code></td>
</tr>
<tr>
<td>列出一个集群的所有 Webhook。</td>
<td>-</td>
<td><code>[GET /v1/clusters/{idOrName}/webhooks](https://containers.cloud.ibm.com/swagger-api/#!/clusters/GetClusterWebhooks)</code></td>
</tr>
<tr>
<td>查看工作程序节点的信息。</td>
<td><code>[ibmcloud ks worker-get](/docs/containers?topic=containers-cs_cli_reference#cs_worker_get)</code></td>
<td><code>[GET /v1/clusters/{idOrName}/workers/{workerId}](https://containers.cloud.ibm.com/swagger-api/#!/clusters/GetWorkers)</code></td>
</tr>
<tr>
<td>查看工作程序池的信息。</td>
<td><code>[ibmcloud ks worker-pool-get](/docs/containers?topic=containers-cs_cli_reference#cs_worker_pool_get)</code></td>
<td><code>[GET /v1/clusters/{idOrName}/workerpools/{poolidOrName}](https://containers.cloud.ibm.com/swagger-api/#!/clusters/GetWorkerPool)</code></td>
</tr>
<tr>
<td>列出一个集群中的所有工作程序池。</td>
<td><code>[ibmcloud ks worker-pools](/docs/containers?topic=containers-cs_cli_reference#cs_worker_pools)</code></td>
<td><code>[GET /v1/clusters/{idOrName}/workerpools](https://containers.cloud.ibm.com/swagger-api/#!/clusters/GetWorkerPools)</code></td>
</tr>
<tr>
<td>列出一个集群中的所有工作程序节点。</td>
<td><code>[ibmcloud ks workers](/docs/containers?topic=containers-cs_cli_reference#cs_workers)</code></td>
<td><code>[GET /v1/clusters/{idOrName}/workers](https://containers.cloud.ibm.com/swagger-api/#!/clusters/GetClusterWorkers)</code></td>
</tr>
</tbody>
</table>

<table>
<caption>在 {{site.data.keyword.containerlong_notm}} 中需要查看者平台角色的 Ingress CLI 命令和 API 调用概述</caption>
<thead>
<th id="view-actions-ingress">Ingress 操作</th>
<th id="view-actions-cli2">CLI 命令</th>
<th id="view-actions-api2">API 调用</th>
</thead>
<tbody>
<tr>
<td>查看 Ingress ALB 的信息。</td>
<td><code>[ibmcloud ks alb-get](/docs/containers?topic=containers-cs_cli_reference#cs_alb_get)</code></td>
<td><code>[GET /albs/{albId}](https://containers.cloud.ibm.com/swagger-alb-api/#!/alb/GetClusterALB)</code></td>
</tr>
<tr>
<td>查看区域中支持的 ALB 类型。</td>
<td><code>[ibmcloud ks alb-types](/docs/containers?topic=containers-cs_cli_reference#cs_alb_types)</code></td>
<td><code>[GET /albtypes](https://containers.cloud.ibm.com/swagger-alb-api/#!/util/GetAvailableALBTypes)</code></td>
</tr>
<tr>
<td>列出一个集群中的所有 Ingress ALB。</td>
<td><code>[ibmcloud ks albs](/docs/containers?topic=containers-cs_cli_reference#cs_albs)</code></td>
<td><code>[GET /clusters/{idOrName}](https://containers.cloud.ibm.com/swagger-alb-api/#!/alb/GetClusterALBs)</code></td>
</tr>
</tbody>
</table>


<table>
<caption>在 {{site.data.keyword.containerlong_notm}} 中需要查看者平台角色的日志记录 CLI 命令和 API 调用概述</caption>
<thead>
<th id="view-actions-log">日志记录操作</th>
<th id="view-actions-cli3">CLI 命令</th>
<th id="view-actions-api3">API 调用</th>
</thead>
<tbody>
<tr>
<td>查看 Fluentd 附加组件自动更新的状态。</td>
<td><code>[ibmcloud ks logging-autoupdate-get](/docs/containers?topic=containers-cs_cli_reference#cs_log_autoupdate_get)</code></td>
<td><code>[GET /v1/logging/{idOrName}/updatepolicy](https://containers.cloud.ibm.com/swagger-logging/#!/logging/GetUpdatePolicy)</code></td>
</tr>
<tr>
<td>查看目标区域的缺省日志记录端点。</td>
<td>-</td>
<td><code>[GET /v1/logging/{idOrName}/default](https://containers.cloud.ibm.com/swagger-logging/#!/logging/GetDefaultLoggingEndpoint)</code></td>
</tr>
<tr>
<td>列出集群中或集群中特定日志源的所有日志转发配置。</td>
<td><code>[ibmcloud ks logging-config-get](/docs/containers?topic=containers-cs_cli_reference#cs_logging_get)</code></td>
<td><code>[GET /v1/logging/{idOrName}/loggingconfig](https://containers.cloud.ibm.com/swagger-logging/#!/logging/FetchLoggingConfigs) 和 [GET /v1/logging/{idOrName}/loggingconfig/{logSource}](https://containers.cloud.ibm.com/swagger-logging/#!/logging/FetchLoggingConfigsForSource)</code></td>
</tr>
<tr>
<td>查看日志过滤配置的信息。</td>
<td><code>[ibmcloud ks logging-filter-get](/docs/containers?topic=containers-cs_cli_reference#cs_log_filter_view)</code></td>
<td><code>[GET /v1/logging/{idOrName}/filterconfigs/{id}](https://containers.cloud.ibm.com/swagger-logging/#!/filter/FetchFilterConfig)</code></td>
</tr>
<tr>
<td>列出集群中的所有日志记录过滤器配置。</td>
<td><code>[ibmcloud ks logging-filter-get](/docs/containers?topic=containers-cs_cli_reference#cs_log_filter_view)</code></td>
<td><code>[GET /v1/logging/{idOrName}/filterconfigs](https://containers.cloud.ibm.com/swagger-logging/#!/filter/FetchFilterConfigs)</code></td>
</tr>
</tbody>
</table>

### 编辑者操作
{: #editor-actions}

**编辑者**平台角色包含**查看者**授予的许可权以及以下许可权。**提示**：将此角色用于应用程序开发者，并分配 <a href="#cloud-foundry">Cloud Foundry</a> **开发者**角色。
{: shortdesc}

<table>
<caption>在 {{site.data.keyword.containerlong_notm}} 中需要编辑者平台角色的集群管理 CLI 命令和 API 调用概述</caption>
<thead>
<th id="editor-actions-mngt">集群管理操作</th>
<th id="editor-actions-cli">CLI 命令</th>
<th id="editor-actions-api">API 调用</th>
</thead>
<tbody>
<tr>
<td>将服务绑定到集群。 **注**：还需要服务所在的空间中的 Cloud Foundry 开发者角色。</td>
<td><code>[ibmcloud ks cluster-service-bind](/docs/containers?topic=containers-cs_cli_reference#cs_cluster_service_bind)</code></td>
<td><code>[POST /v1/clusters/{idOrName}/services](https://containers.cloud.ibm.com/swagger-api/#!/clusters/BindServiceToNamespace)</code></td>
</tr>
<tr>
<td>从集群取消绑定服务。**注**：还需要服务所在的空间中的 Cloud Foundry 开发者角色。</td>
<td><code>[ibmcloud ks cluster-service-unbind](/docs/containers?topic=containers-cs_cli_reference#cs_cluster_service_unbind)</code></td>
<td><code>[DELETE /v1/clusters/{idOrName}/services/{namespace}/{serviceInstanceId}](https://containers.cloud.ibm.com/swagger-api/#!/clusters/UnbindServiceFromNamespace)</code></td>
</tr>
<tr>
<td>在集群中创建 Webhook。</td>
<td><code>[ibmcloud ks webhook-create](/docs/containers?topic=containers-cs_cli_reference#cs_webhook_create)</code></td>
<td><code>[POST /v1/clusters/{idOrName}/webhooks](https://containers.cloud.ibm.com/swagger-api/#!/clusters/AddClusterWebhooks)</code></td>
</tr>
</tbody>
</table>

<table>
<caption>在 {{site.data.keyword.containerlong_notm}} 中需要编辑者平台角色的 Ingress CLI 命令和 API 调用概述</caption>
<thead>
<th id="editor-actions-ingress">Ingress 操作</th>
<th id="editor-actions-cli2">CLI 命令</th>
<th id="editor-actions-api2">API 调用</th>
</thead>
<tbody>
<tr>
<td>禁用 Ingress ALB 附加组件的自动更新。</td>
<td><code>[ibmcloud ks alb-autoupdate-disable](/docs/containers?topic=containers-cs_cli_reference#cs_alb_autoupdate_disable)</code></td>
<td><code>[PUT /clusters/{idOrName}/updatepolicy](https://containers.cloud.ibm.com/swagger-alb-api/#!/alb/ChangeUpdatePolicy)</code></td>
</tr>
<tr>
<td>启用 Ingress ALB 附加组件的自动更新。</td>
<td><code>[ibmcloud ks alb-autoupdate-enable](/docs/containers?topic=containers-cs_cli_reference#cs_alb_autoupdate_enable)</code></td>
<td><code>[PUT /clusters/{idOrName}/updatepolicy](https://containers.cloud.ibm.com/swagger-alb-api/#!/alb/ChangeUpdatePolicy)</code></td>
</tr>
<tr>
<td>检查是否启用了 Ingress ALB 附加组件的自动更新。</td>
<td><code>[ibmcloud ks alb-autoupdate-get](/docs/containers?topic=containers-cs_cli_reference#cs_alb_autoupdate_get)</code></td>
<td><code>[GET /clusters/{idOrName}/updatepolicy](https://containers.cloud.ibm.com/swagger-alb-api/#!/alb/GetUpdatePolicy)</code></td>
</tr>
<tr>
<td>启用或禁用 Ingress ALB。</td>
<td><code>[ibmcloud ks alb-configure](/docs/containers?topic=containers-cs_cli_reference#cs_alb_configure)</code></td>
<td><code>[POST /albs](https://containers.cloud.ibm.com/swagger-alb-api/#!/alb/EnableALB) 和 [DELETE /albs/{albId}](https://containers.cloud.ibm.com/swagger-alb-api/#/)</code></td>
</tr>
<tr>
<td>将 Ingress ALB 附加组件更新回滚到 ALB pod 先前在运行的构建。</td>
<td><code>[ibmcloud ks alb-rollback](/docs/containers?topic=containers-cs_cli_reference#cs_alb_rollback)</code></td>
<td><code>[PUT /clusters/{idOrName}/updaterollback](https://containers.cloud.ibm.com/swagger-alb-api/#!/alb/RollbackUpdate)</code></td>
</tr>
<tr>
<td>通过手动更新 Ingress ALB 附加组件，强制一次性更新 ALB pod。</td>
<td><code>[ibmcloud ks alb-update](/docs/containers?topic=containers-cs_cli_reference#cs_alb_update)</code></td>
<td><code>[PUT /clusters/{idOrName}/update](https://containers.cloud.ibm.com/swagger-alb-api/#!/alb/UpdateALBs)</code></td>
</tr>
</tbody>
</table>



<table>
<caption>在 {{site.data.keyword.containerlong_notm}} 中需要编辑者平台角色的日志记录 CLI 命令和 API 调用概述</caption>
<thead>
<th id="editor-log">日志记录操作</th>
<th id="editor-cli3">CLI 命令</th>
<th id="editor-api3">API 调用</th>
</thead>
<tbody>
<tr>
<td>创建 API 服务器审计 Webhook。</td>
<td><code>[ibmcloud ks apiserver-config-set](/docs/containers?topic=containers-cs_cli_reference#cs_apiserver_config_set)</code></td>
<td><code>[PUT /v1/clusters/{idOrName}/apiserverconfigs/auditwebhook](https://containers.cloud.ibm.com/swagger-api/#!/clusters/apiserverconfigs/UpdateAuditWebhook)</code></td>
</tr>
<tr>
<td>删除 API 服务器审计 Webhook。</td>
<td><code>[ibmcloud ks apiserver-config-unset](/docs/containers?topic=containers-cs_cli_reference#cs_apiserver_config_unset)</code></td>
<td><code>[DELETE /v1/clusters/{idOrName}/apiserverconfigs/auditwebhook](https://containers.cloud.ibm.com/swagger-api/#!/apiserverconfigs/DeleteAuditWebhook)</code></td>
</tr>
<tr>
<td>为除 <code>kube-audit</code> 之外的其他所有日志源创建日志转发配置。</td>
<td><code>[ibmcloud ks logging-config-create](/docs/containers?topic=containers-cs_cli_reference#cs_logging_create)</code></td>
<td><code>[POST /v1/logging/{idOrName}/loggingconfig/{logSource}](https://containers.cloud.ibm.com/swagger-logging/#!/logging/CreateLoggingConfig)</code></td>
</tr>
<tr>
<td>刷新日志转发配置。</td>
<td><code>[ibmcloud ks logging-config-refresh](/docs/containers?topic=containers-cs_cli_reference#cs_logging_refresh)</code></td>
<td><code>[PUT /v1/logging/{idOrName}/refresh](https://containers.cloud.ibm.com/swagger-logging/#!/logging/RefreshLoggingConfig)</code></td>
</tr>
<tr>
<td>针对除 <code>kube-audit</code> 之外的其他所有日志源，删除日志转发配置。</td>
<td><code>[ibmcloud ks logging-config-rm](/docs/containers?topic=containers-cs_cli_reference#cs_logging_rm)</code></td>
<td><code>[DELETE /v1/logging/{idOrName}/loggingconfig/{logSource}/{id}](https://containers.cloud.ibm.com/swagger-logging/#!/logging/DeleteLoggingConfig)</code></td>
</tr>
<tr>
<td>删除集群的所有日志转发配置。</td>
<td>-</td>
<td><code>[DELETE /v1/logging/{idOrName}/loggingconfig](https://containers.cloud.ibm.com/swagger-logging/#!/logging/DeleteLoggingConfigs)</code></td>
</tr>
<tr>
<td>更新日志转发配置。</td>
<td><code>[ibmcloud ks logging-config-update](/docs/containers?topic=containers-cs_cli_reference#cs_logging_update)</code></td>
<td><code>[PUT /v1/logging/{idOrName}/loggingconfig/{logSource}/{id}](https://containers.cloud.ibm.com/swagger-logging/#!/logging/UpdateLoggingConfig)</code></td>
</tr>
<tr>
<td>创建日志过滤配置。</td>
<td><code>[ibmcloud ks logging-filter-create](/docs/containers?topic=containers-cs_cli_reference#cs_log_filter_create)</code></td>
<td><code>[POST /v1/logging/{idOrName}/filterconfigs](https://containers.cloud.ibm.com/swagger-logging/#!/filter/CreateFilterConfig)</code></td>
</tr>
<tr>
<td>删除日志过滤配置。</td>
<td><code>[ibmcloud ks logging-filter-rm](/docs/containers?topic=containers-cs_cli_reference#cs_log_filter_delete)</code></td>
<td><code>[DELETE /v1/logging/{idOrName}/filterconfigs/{id}](https://containers.cloud.ibm.com/swagger-logging/#!/filter/DeleteFilterConfig)</code></td>
</tr>
<tr>
<td>删除 Kubernetes 集群的所有日志记录过滤器配置。</td>
<td>-</td>
<td><code>[DELETE /v1/logging/{idOrName}/filterconfigs](https://containers.cloud.ibm.com/swagger-logging/#!/filter/DeleteFilterConfigs)</code></td>
</tr>
<tr>
<td>更新日志过滤配置。</td>
<td><code>[ibmcloud ks logging-filter-update](/docs/containers?topic=containers-cs_cli_reference#cs_log_filter_update)</code></td>
<td><code>[PUT /v1/logging/{idOrName}/filterconfigs/{id}](https://containers.cloud.ibm.com/swagger-logging/#!/filter/UpdateFilterConfig)</code></td>
</tr>
</tbody>
</table>

### 操作员操作
{: #operator-actions}

**操作员**平台角色包含**查看者**授予的许可权以及下表中显示的许可权。
{: shortdesc}

<table>
<caption>在 {{site.data.keyword.containerlong_notm}} 中需要操作员平台角色的集群管理 CLI 命令和 API 调用概述</caption>
<thead>
<th id="operator-mgmt">集群管理操作</th>
<th id="operator-cli">CLI 命令</th>
<th id="operator-api">API 调用</th>
</thead>
<tbody>
<tr>
<td>刷新 Kubernetes 主节点。</td>
<td><code>[ibmcloud ks apiserver-refresh](/docs/containers?topic=containers-cs_cli_reference#cs_apiserver_refresh)</code></td>
<td><code>[PUT /v1/clusters/{idOrName}/masters](https://containers.cloud.ibm.com/swagger-api/#!/clusters/HandleMasterAPIServer)</code></td>
</tr>
<tr>
<td>为集群创建 {{site.data.keyword.Bluemix_notm}} IAM 服务标识，为该服务标识创建策略，以用于在 {{site.data.keyword.registrylong_notm}} 中分配**读者**服务访问角色，然后为该服务标识创建 API 密钥。</td>
<td><code>[ibmcloud ks cluster-pull-secret-apply](/docs/containers?topic=containers-cs_cli_reference#cs_cluster_pull_secret_apply)</code></td>
<td>-</td>
</tr>
<tr>
<td>重新启动集群主节点以应用新的 Kubernetes API 配置更改。</td>
<td><code>[ibmcloud ks cluster-refresh](/docs/containers?topic=containers-cs_cli_reference#cs_cluster_refresh)</code></td>
<td><code>[PUT /v1/clusters/{idOrName}/masters](https://containers.cloud.ibm.com/swagger-api/#!/clusters/HandleMasterAPIServer)</code></td>
</tr>
<tr>
<td>向集群添加子网。</td>
<td><code>[ibmcloud ks cluster-subnet-add](/docs/containers?topic=containers-cs_cli_reference#cs_cluster_subnet_add)</code></td>
<td><code>[PUT /v1/clusters/{idOrName}/subnets/{subnetId}](https://containers.cloud.ibm.com/swagger-api/#!/clusters/AddClusterSubnet)</code></td>
</tr>
<tr>
<td>创建子网。</td>
<td><code>[ibmcloud ks cluster-subnet-create](/docs/containers?topic=containers-cs_cli_reference#cs_cluster_subnet_create)</code></td>
<td><code>[POST /v1/clusters/{idOrName}/vlans/{vlanId}](https://containers.cloud.ibm.com/swagger-api/#!/clusters/CreateClusterSubnet)</code></td>
</tr>
<tr>
<td>更新集群。</td>
<td><code>[ibmcloud ks cluster-update](/docs/containers?topic=containers-cs_cli_reference#cs_cluster_update)</code></td>
<td><code>[PUT /v1/clusters/{idOrName}](https://containers.cloud.ibm.com/swagger-api/#!/clusters/UpdateCluster)</code></td>
</tr>
<tr>
<td>向集群添加用户管理的子网。</td>
<td><code>[ibmcloud ks cluster-user-subnet-add](/docs/containers?topic=containers-cs_cli_reference#cs_cluster_user_subnet_add)</code></td>
<td><code>[POST /v1/clusters/{idOrName}/usersubnets](https://containers.cloud.ibm.com/swagger-api/#!/clusters/AddClusterUserSubnet)</code></td>
</tr>
<tr>
<td>从集群中除去用户管理的子网。</td>
<td><code>[ibmcloud ks cluster-user-subnet-rm](/docs/containers?topic=containers-cs_cli_reference#cs_cluster_user_subnet_rm)</code></td>
<td><code>[DELETE /v1/clusters/{idOrName}/usersubnets/{subnetId}/vlans/{vlanId}](https://containers.cloud.ibm.com/swagger-api/#!/clusters/RemoveClusterUserSubnet)</code></td>
</tr>
<tr>
<td>添加工作程序节点。</td>
<td><code>[ibmcloud ks worker-add（不推荐）](/docs/containers?topic=containers-cs_cli_reference#cs_worker_add)</code></td>
<td><code>[POST /v1/clusters/{idOrName}/workers](https://containers.cloud.ibm.com/swagger-api/#!/clusters/AddClusterWorkers)</code></td>
</tr>
<tr>
<td>创建工作程序池。</td>
<td><code>[ibmcloud ks worker-pool-create](/docs/containers?topic=containers-cs_cli_reference#cs_worker_pool_create)</code></td>
<td><code>[POST /v1/clusters/{idOrName}/workerpools](https://containers.cloud.ibm.com/swagger-api/#!/clusters/CreateWorkerPool)</code></td>
</tr>
<tr>
<td>重新均衡工作程序池。</td>
<td><code>[ibmcloud ks worker-pool-rebalance](/docs/containers?topic=containers-cs_cli_reference#cs_rebalance)</code></td>
<td><code>[PATCH /v1/clusters/{idOrName}/workerpools/{poolidOrName}](https://containers.cloud.ibm.com/swagger-api/#!/clusters/PatchWorkerPool)</code></td>
</tr>
<tr>
<td>调整工作程序池的大小。</td>
<td><code>[ibmcloud ks worker-pool-resize](/docs/containers?topic=containers-cs_cli_reference#cs_worker_pool_resize)</code></td>
<td><code>[PATCH /v1/clusters/{idOrName}/workerpools/{poolidOrName}](https://containers.cloud.ibm.com/swagger-api/#!/clusters/PatchWorkerPool)</code></td>
</tr>
<tr>
<td>删除工作程序池。</td>
<td><code>[ibmcloud ks worker-pool-rm](/docs/containers?topic=containers-cs_cli_reference#cs_worker_pool_rm)</code></td>
<td><code>[DELETE /v1/clusters/{idOrName}/workerpools/{poolidOrName}](https://containers.cloud.ibm.com/swagger-api/#!/clusters/RemoveWorkerPool)</code></td>
</tr>
<tr>
<td>重新引导工作程序节点。</td>
<td><code>[ibmcloud ks worker-reboot](/docs/containers?topic=containers-cs_cli_reference#cs_worker_reboot)</code></td>
<td><code>[PUT /v1/clusters/{idOrName}/workers/{workerId}](https://containers.cloud.ibm.com/swagger-api/#!/clusters/UpdateClusterWorker)</code></td>
</tr>
<tr>
<td>重新装入工作程序节点。</td>
<td><code>[ibmcloud ks worker-reload](/docs/containers?topic=containers-cs_cli_reference#cs_worker_reload)</code></td>
<td><code>[PUT /v1/clusters/{idOrName}/workers/{workerId}](https://containers.cloud.ibm.com/swagger-api/#!/clusters/UpdateClusterWorker)</code></td>
</tr>
<tr>
<td>除去工作程序节点。</td>
<td><code>[ibmcloud ks worker-rm](/docs/containers?topic=containers-cs_cli_reference#cs_worker_rm)</code></td>
<td><code>[DELETE /v1/clusters/{idOrName}/workers/{workerId}](https://containers.cloud.ibm.com/swagger-api/#!/clusters/RemoveClusterWorker)</code></td>
</tr>
<tr>
<td>更新工作程序节点。</td>
<td><code>[ibmcloud ks worker-update](/docs/containers?topic=containers-cs_cli_reference#cs_worker_update)</code></td>
<td><code>[PUT /v1/clusters/{idOrName}/workers/{workerId}](https://containers.cloud.ibm.com/swagger-api/#!/clusters/UpdateClusterWorker)</code></td>
</tr>
<tr>
<td>向工作程序池添加专区。</td>
<td><code>[ibmcloud ks zone-add](/docs/containers?topic=containers-cs_cli_reference#cs_zone_add)</code></td>
<td><code>[POST /v1/clusters/{idOrName}/workerpools/{poolidOrName}/zones](https://containers.cloud.ibm.com/swagger-api/#!/clusters/AddWorkerPoolZone)</code></td>
</tr>
<tr>
<td>更新工作程序池中给定专区的网络配置。</td>
<td><code>[ibmcloud ks zone-network-set](/docs/containers?topic=containers-cs_cli_reference#cs_zone_network_set)</code></td>
<td><code>[PATCH /v1/clusters/{idOrName}/workerpools/{poolidOrName}/zones/{zoneid}](https://containers.cloud.ibm.com/swagger-api/#!/clusters/AddWorkerPoolZoneNetwork)</code></td>
</tr>
<tr>
<td>从工作程序池中除去专区。</td>
<td><code>[ibmcloud ks zone-rm](/docs/containers?topic=containers-cs_cli_reference#cs_zone_rm)</code></td>
<td><code>[DELETE /v1/clusters/{idOrName}/workerpools/{poolidOrName}/zones/{zoneid}](https://containers.cloud.ibm.com/swagger-api/#!/clusters/RemoveWorkerPoolZone)</code></td>
</tr>
</tbody>
</table>

### 管理员操作
{: #admin-actions}

**管理员**平台角色包含由**查看者**、**编辑者**和**操作员**角色授予的所有许可权以及以下许可权。要创建资源（如机器、VLAN 和子网），管理员用户需要**超级用户**<a href="#infra">基础架构角色</a>。
{: shortdesc}

<table>
<caption>在 {{site.data.keyword.containerlong_notm}} 中需要管理员平台角色的集群管理 CLI 命令和 API 调用概述</caption>
<thead>
<th id="admin-mgmt">集群管理操作</th>
<th id="admin-cli">CLI 命令</th>
<th id="admin-api">API 调用</th>
</thead>
<tbody>
<tr>
<td>为 {{site.data.keyword.Bluemix_notm}} 帐户设置 API 密钥以访问链接的 IBM Cloud Infrastructure (SoftLayer) 产品服务组合。</td>
<td><code>[ibmcloud ks api-key-reset](/docs/containers?topic=containers-cs_cli_reference#cs_api_key_reset)</code></td>
<td><code>[POST /v1/keys](https://containers.cloud.ibm.com/swagger-api/#!/accounts/ResetUserAPIKey)</code></td>
</tr>
<tr>
<td>在集群中禁用受管附加组件（例如，Istio 或 Knative）。</td>
<td><code>[ibmcloud ks cluster-addon-disable](/docs/containers?topic=containers-cs_cli_reference#cs_cluster_addon_disable)</code></td>
<td><code>[PATCH /v1/clusters/{idOrName}/addons](https://containers.cloud.ibm.com/swagger-api/#!/clusters/ManageClusterAddons)</code></td>
</tr>
<tr>
<td>在集群中启用受管附加组件（例如，Istio 或 Knative）。</td>
<td><code>[ibmcloud ks cluster-addon-enable](/docs/containers?topic=containers-cs_cli_reference#cs_cluster_addon_enable)</code></td>
<td><code>[PATCH /v1/clusters/{idOrName}/addons](https://containers.cloud.ibm.com/swagger-api/#!/clusters/ManageClusterAddons)</code></td>
</tr>
<tr>
<td>列出在集群中启用的受管附加组件（例如，Istio 或 Knative）。</td>
<td><code>[ibmcloud ks cluster-addons](/docs/containers?topic=containers-cs_cli_reference#cs_cluster_addons)</code></td>
<td><code>[GET /v1/clusters/{idOrName}/addons](https://containers.cloud.ibm.com/swagger-api/#!/clusters/GetClusterAddons)</code></td>
</tr>
<tr>
<td>创建免费或标准集群。**注**：还需要对 {{site.data.keyword.registrylong_notm}} 的管理员平台角色以及超级用户基础架构角色。</td>
<td><code>[ibmcloud ks cluster-create](/docs/containers?topic=containers-cs_cli_reference#cs_cluster_create)</code></td>
<td><code>[POST /v1/clusters](https://containers.cloud.ibm.com/swagger-api/#!/clusters/CreateCluster)</code></td>
</tr>
<tr>
<td>禁用集群的指定功能，例如集群主节点的公共服务端点。</td>
<td><code>[ibmcloud ks cluster-feature-disable](/docs/containers?topic=containers-cs_cli_reference#cs_cluster_feature_disable)</code></td>
<td>-</td>
</tr>
<tr>
<td>启用集群的指定功能，例如集群主节点的专用服务端点。</td>
<td><code>[ibmcloud ks cluster-feature-enable](/docs/containers?topic=containers-cs_cli_reference#cs_cluster_feature_enable)</code></td>
<td>-</td>
</tr>
<tr>
<td>列出集群中启用的功能。</td>
<td><code>[ibmcloud ks cluster-features](/docs/containers?topic=containers-cs_cli_reference#cs_cluster_feature_ls)</code></td>
<td>-</td>
</tr>
<tr>
<td>删除集群。</td>
<td><code>[ibmcloud ks cluster-rm](/docs/containers?topic=containers-cs_cli_reference#cs_cluster_rm)</code></td>
<td><code>[DELETE /v1/clusters/{idOrName}](https://containers.cloud.ibm.com/swagger-api/#!/clusters/RemoveCluster)</code></td>
</tr>
<tr>
<td>为 {{site.data.keyword.Bluemix_notm}} 帐户设置用于访问其他 IBM Cloud Infrastructure (SoftLayer) 产品服务组合的基础架构凭证。</td>
<td><code>[ibmcloud ks credential-set](/docs/containers?topic=containers-cs_cli_reference#cs_credentials_set)</code></td>
<td><code>[POST /v1/credentials](https://containers.cloud.ibm.com/swagger-api/#!/clusters/accounts/StoreUserCredentials)</code></td>
</tr>
<tr>
<td>除去 {{site.data.keyword.Bluemix_notm}} 帐户用于访问其他 IBM Cloud Infrastructure (SoftLayer) 产品服务组合的基础架构凭证。</td>
<td><code>[ibmcloud ks credential-unset](/docs/containers?topic=containers-cs_cli_reference#cs_credentials_unset)</code></td>
<td><code>[DELETE /v1/credentials](https://containers.cloud.ibm.com/swagger-api/#!/clusters/accounts/RemoveUserCredentials)</code></td>
</tr>
<tr>
<td>使用 {{site.data.keyword.keymanagementservicefull}} 加密 Kubernetes 私钥。</td>
<td><code>[ibmcloud ks key-protect-enable](/docs/containers?topic=containers-cs_cli_reference#cs_messages)</code></td>
<td><code>[POST /v1/clusters/{idOrName}/kms](https://containers.cloud.ibm.com/swagger-api/#!/clusters/CreateKMSConfig)</code></td>
</tr>
</tbody>
</table>

<table>
<caption>在 {{site.data.keyword.containerlong_notm}} 中需要管理员平台角色的 Ingress CLI 命令和 API 调用概述</caption>
<thead>
<th id="admin-ingress">Ingress 操作</th>
<th id="admin-cli2">CLI 命令</th>
<th id="admin-api2">API 调用</th>
</thead>
<tbody>
<tr>
<td>将证书从 {{site.data.keyword.cloudcerts_long_notm}} 实例部署到 ALB 或更新该证书。</td>
<td><code>[ibmcloud ks alb-cert-deploy](/docs/containers?topic=containers-cs_cli_reference#cs_alb_cert_deploy)</code></td>
<td><code>[POST /albsecrets](https://containers.cloud.ibm.com/swagger-alb-api/#!/alb/CreateALBSecret) 或 [PUT /albsecrets](https://containers.cloud.ibm.com/swagger-alb-api/#!/alb/UpdateALBSecret)</code></td>
</tr>
<tr>
<td>查看有关集群中 ALB 私钥的详细信息。</td>
<td><code>[ibmcloud ks alb-cert-get](/docs/containers?topic=containers-cs_cli_reference#cs_alb_cert_get)</code></td>
<td><code>[GET /clusters/{idOrName}/albsecrets](https://containers.cloud.ibm.com/swagger-alb-api/#!/alb/ViewClusterALBSecrets)</code></td>
</tr>
<tr>
<td>从集群中除去 ALB 私钥。</td>
<td><code>[ibmcloud ks alb-cert-rm](/docs/containers?topic=containers-cs_cli_reference#cs_alb_cert_rm)</code></td>
<td><code>[DELETE /clusters/{idOrName}/albsecrets](https://containers.cloud.ibm.com/swagger-alb-api/#!/alb/DeleteClusterALBSecrets)</code></td>
</tr>
<tr>
<td>列出一个集群中的所有 ALB 私钥。</td>
<td><code>[ibmcloud ks alb-certs](/docs/containers?topic=containers-cs_cli_reference#cs_alb_certs)</code></td>
<td>-</td>
</tr>
</tbody>
</table>

<table>
<caption>在 {{site.data.keyword.containerlong_notm}} 中需要管理员平台角色的日志记录 CLI 命令和 API 调用概述</caption>
<thead>
<th id="admin-log">日志记录操作</th>
<th id="admin-cli3">CLI 命令</th>
<th id="admin-api3">API 调用</th>
</thead>
<tbody>
<tr>
<td>禁用 Fluentd 集群附加组件的自动更新。</td>
<td><code>[ibmcloud ks logging-autoupdate-disable](/docs/containers?topic=containers-cs_cli_reference#cs_log_autoupdate_disable)</code></td>
<td><code>[PUT /v1/logging/{idOrName}/updatepolicy](https://containers.cloud.ibm.com/swagger-logging/#!/logging/ChangeUpdatePolicy)</code></td>
</tr>
<tr>
<td>启用 Fluentd 集群附加组件的自动更新。</td>
<td><code>[ibmcloud ks logging-autoupdate-enable](/docs/containers?topic=containers-cs_cli_reference#cs_log_autoupdate_enable)</code></td>
<td><code>[PUT /v1/logging/{idOrName}/updatepolicy](https://containers.cloud.ibm.com/swagger-logging/#!/logging/ChangeUpdatePolicy)</code></td>
</tr>
<tr>
<td>在 {{site.data.keyword.cos_full_notm}} 存储区中收集 API 服务器日志的快照。</td>
<td><code>[ibmcloud ks logging-collect](/docs/containers?topic=containers-cs_cli_reference#cs_log_collect)</code></td>
<td>-</td>
</tr>
<tr>
<td>查看 API 服务器日志快照请求的状态。</td>
<td><code>[ibmcloud ks logging-collect-status](/docs/containers?topic=containers-cs_cli_reference#cs_log_collect_status)</code></td>
<td>-</td>
</tr>
<tr>
<td>为 <code>kube-audit</code> 日志源创建日志转发配置。</td>
<td><code>[ibmcloud ks logging-config-create](/docs/containers?topic=containers-cs_cli_reference#cs_logging_create)</code></td>
<td><code>[POST /v1/logging/{idOrName}/loggingconfig/{logSource}](https://containers.cloud.ibm.com/swagger-logging/#!/logging/CreateLoggingConfig)</code></td>
</tr>
<tr>
<td>删除 <code>kube-audit</code> 日志源的日志转发配置。</td>
<td><code>[ibmcloud ks logging-config-rm](/docs/containers?topic=containers-cs_cli_reference#cs_logging_rm)</code></td>
<td><code>[DELETE /v1/logging/{idOrName}/loggingconfig/{logSource}/{id}](https://containers.cloud.ibm.com/swagger-logging/#!/logging/DeleteLoggingConfig)</code></td>
</tr>
</tbody>
</table>

<br />


## {{site.data.keyword.Bluemix_notm}} IAM 服务角色
{: #service}

分配有 {{site.data.keyword.Bluemix_notm}} IAM 服务访问角色的每个用户还会在特定名称空间中自动分配有相应的 Kubernetes 基于角色的访问控制 (RBAC) 角色。要了解有关服务访问角色的更多信息，请参阅 [{{site.data.keyword.Bluemix_notm}} IAM 服务角色](/docs/containers?topic=containers-users#platform)。要了解有关 RBAC 角色的更多信息，请参阅[分配 RBAC 许可权](/docs/containers?topic=containers-users#role-binding)。
{: shortdesc}

需要了解每个服务角色通过 RBAC 授权的 Kubernetes 操作？请参阅[每个 RBAC 角色的 Kubernetes 资源许可权](#rbac)。
{: tip}

下表显示了每个服务角色及其相应的 RBAC 角色授予的 Kubernetes 资源许可权。

<table>
<caption>服务角色和相应的 RBAC 角色授予的 Kubernetes 资源许可权</caption>
<thead>
    <th id="service-role">服务角色</th>
    <th id="rbac-role">相应的 RBAC 角色、绑定和作用域</th>
    <th id="kube-perm">Kubernetes 资源许可权</th>
</thead>
<tbody>
  <tr>
    <td id="service-role-reader" headers="service-role">读取者角色</td>
    <td headers="service-role-reader rbac-role">作用域限定为一个名称空间时：由该名称空间中的 <strong><code>ibm-view</code></strong> 角色绑定所应用的 <strong><code>view</code></strong> 集群角色</br><br>作用域限定为所有名称空间时：由集群的每个名称空间中的 <strong><code>ibm-view</code></strong> 角色绑定所应用的 <strong><code>view</code></strong> 集群角色</td>
    <td headers="service-role-reader kube-perm"><ul>
      <li>对名称空间中资源的读访问权</li>
      <li>没有对角色和角色绑定的读访问权，也没有对 Kubernetes 私钥的读访问权</li>
      <li>访问 Kubernetes 仪表板可查看名称空间中的资源</li></ul>
    </td>
  </tr>
  <tr>
    <td id="service-role-writer" headers="service-role">写入者角色</td>
    <td headers="service-role-writer rbac-role">作用域限定为一个名称空间时：由该名称空间中的 <strong><code>ibm-edit</code></strong> 角色绑定所应用的 <strong><code>edit</code></strong> 集群角色</br><br>作用域限定为所有名称空间时：由集群中每个名称空间中的 <strong><code>ibm-edit</code></strong> 角色绑定所应用的 <strong><code>edit</code></strong> 集群角色</td>
    <td headers="service-role-writer kube-perm"><ul><li>对名称空间中资源的读/写访问权</li>
    <li>没有对角色和角色绑定的读/写访问权。</li>
    <li>访问 Kubernetes 仪表板可查看名称空间中的资源</li></ul>
    </td>
  </tr>
  <tr>
    <td id="service-role-manager" headers="service-role">管理者角色</td>
    <td headers="service-role-manager rbac-role">作用域限定为一个名称空间时：由该名称空间中的 <strong><code>ibm-operate</code></strong> 角色绑定所应用的 <strong><code>admin</code></strong> 集群角色</br><br>作用域限定为所有名称空间时：由应用于所有名称空间的 <strong><code>ibm-admin</code></strong> 集群角色绑定所应用的 <strong><code>cluster-admin</code></strong> 集群角色</td>
    <td headers="service-role-manager kube-perm">作用域限定为一个名称空间时：
      <ul><li>对名称空间中所有资源的读/写访问权，但没有对资源配额或名称空间本身的读/写访问权</li>
      <li>在名称空间中创建 RBAC 角色和角色绑定</li>
      <li>访问 Kubernetes 仪表板可查看名称空间中的所有资源</li></ul>
    </br>作用域限定为所有名称空间时：
      <ul><li>对每个名称空间中所有资源的读/写访问权</li>
        <li>在名称空间中创建 RBAC 角色和角色绑定，或在所有名称空间中创建集群角色和集群角色绑定</li>
        <li>访问 Kubernetes 仪表板</li>
        <li>创建使应用程序公共可用的 Ingress 资源</li>
        <li>查看集群度量值，例如使用 <code>kubectl top pods</code>、<code>kubectl top nodes</code> 或 <code>kubectl get nodes</code> 命令</li></ul>
    </td>
  </tr>
</tbody>
</table>

<br />


## 每个 RBAC 角色的 Kubernetes 资源许可权
{: #rbac}

分配有 {{site.data.keyword.Bluemix_notm}} IAM 服务访问角色的每个用户还会自动分配有相应的预定义 Kubernetes 基于角色的访问控制 (RBAC) 角色。如果计划管理您自己的定制 Kubernetes RBAC 角色，请参阅[为用户、组或服务帐户创建定制 RBAC 许可权](/docs/containers?topic=containers-users#rbac)。
{: shortdesc}

想了解您是否具有对名称空间中的某个资源运行特定 `kubectl` 命令的正确许可权？请尝试使用 [`kubectl auth can-i` 命令 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#-em-can-i-em-)。
{: tip}

下表显示了每个 RBAC 角色授予的对单个 Kubernetes 资源的许可权。许可权显示为具有该角色的用户可以对资源完成的操作动词，例如“get”、“list”、“describe”、“create”或“delete”。

<table>
 <caption>每个预定义 RBAC 角色授予的 Kubernetes 资源许可权</caption>
 <thead>
  <th>Kubernetes 资源</th>
  <th><code>view</code></th>
  <th><code>edit</code></th>
  <th><code>admin</code> 和 <code>cluster-admin</code></th>
 </thead>
<tbody>
<tr>
  <td>bindings</td>
  <td><code>get</code>、<code>list</code> 和 <code>watch</code></td>
  <td><code>get</code>、<code>list</code> 和 <code>watch</code></td>
  <td><code>get</code>、<code>list</code> 和 <code>watch</code></td>
</tr><tr>
  <td>configmaps</td>
  <td><code>get</code>、<code>list</code> 和 <code>watch</code></td>
  <td><code>create</code>、<code>delete</code>、<code>deletecollection</code>、<code>get</code>、<code>list</code>、<code>patch</code>、<code>update</code> 和 <code>watch</code></td>
  <td><code>create</code>、<code>delete</code>、<code>deletecollection</code>、<code>get</code>、<code>list</code>、<code>patch</code>、<code>update</code> 和 <code>watch</code></td>
</tr><tr>
  <td>cronjobs.batch</td>
  <td><code>get</code>、<code>list</code> 和 <code>watch</code></td>
  <td><code>create</code>、<code>delete</code>、<code>deletecollection</code>、<code>get</code>、<code>list</code>、<code>patch</code>、<code>update</code> 和 <code>watch</code></td>
  <td><code>create</code>、<code>delete</code>、<code>deletecollection</code>、<code>get</code>、<code>list</code>、<code>patch</code>、<code>update</code> 和 <code>watch</code></td>
</tr><tr>
  <td>daemonsets.apps</td>
  <td><code>get</code>、<code>list</code> 和 <code>watch</code></td>
  <td><code>create</code>、<code>delete</code>、<code>deletecollection</code>、<code>get</code>、<code>list</code>、<code>patch</code>、<code>update</code> 和 <code>watch</code></td>
  <td><code>create</code>、<code>delete</code>、<code>deletecollection</code>、<code>get</code>、<code>list</code>、<code>patch</code>、<code>update</code> 和 <code>watch</code></td>
</tr><tr>
  <td>daemonsets.extensions</td>
  <td><code>get</code>、<code>list</code> 和 <code>watch</code></td>
  <td><code>create</code>、<code>delete</code>、<code>deletecollection</code>、<code>get</code>、<code>list</code>、<code>patch</code>、<code>update</code> 和 <code>watch</code></td>
  <td><code>create</code>、<code>delete</code>、<code>deletecollection</code>、<code>get</code>、<code>list</code>、<code>patch</code>、<code>update</code> 和 <code>watch</code></td>
</tr><tr>
  <td>deployments.apps</td>
  <td><code>get</code>、<code>list</code> 和 <code>watch</code></td>
  <td><code>create</code>、<code>delete</code>、<code>deletecollection</code>、<code>get</code>、<code>list</code>、<code>patch</code>、<code>update</code> 和 <code>watch</code></td>
  <td><code>create</code>、<code>delete</code>、<code>deletecollection</code>、<code>get</code>、<code>list</code>、<code>patch</code>、<code>update</code> 和 <code>watch</code></td>
</tr><tr>
  <td>deployments.apps/rollback</td>
  <td>-</td>
  <td><code>create</code>、<code>delete</code>、<code>deletecollection</code>、<code>get</code>、<code>list</code>、<code>patch</code>、<code>update</code> 和 <code>watch</code></td>
  <td><code>create</code>、<code>delete</code>、<code>deletecollection</code>、<code>get</code>、<code>list</code>、<code>patch</code>、<code>update</code> 和 <code>watch</code></td>
</tr><tr>
  <td>deployments.apps/scale</td>
  <td><code>get</code>、<code>list</code> 和 <code>watch</code></td>
  <td><code>create</code>、<code>delete</code>、<code>deletecollection</code>、<code>get</code>、<code>list</code>、<code>patch</code>、<code>update</code> 和 <code>watch</code></td>
  <td><code>create</code>、<code>delete</code>、<code>deletecollection</code>、<code>get</code>、<code>list</code>、<code>patch</code>、<code>update</code> 和 <code>watch</code></td>
</tr><tr>
  <td>deployments.extensions</td>
  <td><code>get</code>、<code>list</code> 和 <code>watch</code></td>
  <td><code>create</code>、<code>delete</code>、<code>deletecollection</code>、<code>get</code>、<code>list</code>、<code>patch</code>、<code>update</code> 和 <code>watch</code></td>
  <td><code>create</code>、<code>delete</code>、<code>deletecollection</code>、<code>get</code>、<code>list</code>、<code>patch</code>、<code>update</code> 和 <code>watch</code></td>
</tr><tr>
  <td>deployments.extensions/rollback</td>
  <td>-</td>
  <td><code>create</code>、<code>delete</code>、<code>deletecollection</code>、<code>get</code>、<code>list</code>、<code>patch</code>、<code>update</code> 和 <code>watch</code></td>
  <td><code>create</code>、<code>delete</code>、<code>deletecollection</code>、<code>get</code>、<code>list</code>、<code>patch</code>、<code>update</code> 和 <code>watch</code></td>
</tr><tr>
  <td>deployments.extensions/scale</td>
  <td><code>get</code>、<code>list</code> 和 <code>watch</code></td>
  <td><code>create</code>、<code>delete</code>、<code>deletecollection</code>、<code>get</code>、<code>list</code>、<code>patch</code>、<code>update</code> 和 <code>watch</code></td>
  <td><code>create</code>、<code>delete</code>、<code>deletecollection</code>、<code>get</code>、<code>list</code>、<code>patch</code>、<code>update</code> 和 <code>watch</code></td>
</tr><tr>
  <td>endpoints</td>
  <td><code>get</code>、<code>list</code> 和 <code>watch</code></td>
  <td><code>create</code>、<code>delete</code>、<code>deletecollection</code>、<code>get</code>、<code>list</code>、<code>patch</code>、<code>update</code> 和 <code>watch</code></td>
  <td><code>create</code>、<code>delete</code>、<code>deletecollection</code>、<code>get</code>、<code>list</code>、<code>patch</code>、<code>update</code> 和 <code>watch</code></td>
</tr><tr>
  <td>events</td>
  <td><code>get</code>、<code>list</code> 和 <code>watch</code></td>
  <td><code>get</code>、<code>list</code> 和 <code>watch</code></td>
  <td><code>get</code>、<code>list</code> 和 <code>watch</code></td>
</tr><tr>
  <td>horizontalpodautoscalers.autoscaling</td>
  <td><code>get</code>、<code>list</code> 和 <code>watch</code></td>
  <td><code>create</code>、<code>delete</code>、<code>deletecollection</code>、<code>get</code>、<code>list</code>、<code>patch</code>、<code>update</code> 和 <code>watch</code></td>
  <td><code>create</code>、<code>delete</code>、<code>deletecollection</code>、<code>get</code>、<code>list</code>、<code>patch</code>、<code>update</code> 和 <code>watch</code></td>
</tr><tr>
  <td>ingresses.extensions</td>
  <td><code>get</code>、<code>list</code> 和 <code>watch</code></td>
  <td><code>create</code>、<code>delete</code>、<code>deletecollection</code>、<code>get</code>、<code>list</code>、<code>patch</code>、<code>update</code> 和 <code>watch</code></td>
  <td><code>create</code>、<code>delete</code>、<code>deletecollection</code>、<code>get</code>、<code>list</code>、<code>patch</code>、<code>update</code> 和 <code>watch</code></td>
</tr><tr>
  <td>jobs.batch</td>
  <td><code>get</code>、<code>list</code> 和 <code>watch</code></td>
  <td><code>create</code>、<code>delete</code>、<code>deletecollection</code>、<code>get</code>、<code>list</code>、<code>patch</code>、<code>update</code> 和 <code>watch</code></td>
  <td><code>create</code>、<code>delete</code>、<code>deletecollection</code>、<code>get</code>、<code>list</code>、<code>patch</code>、<code>update</code> 和 <code>watch</code></td>
</tr><tr>
  <td>limitranges</td>
  <td><code>get</code>、<code>list</code> 和 <code>watch</code></td>
  <td><code>get</code>、<code>list</code> 和 <code>watch</code></td>
  <td><code>get</code>、<code>list</code> 和 <code>watch</code></td>
</tr><tr>
  <td>localsubjectaccessreviews</td>
  <td>-</td>
  <td>-</td>
  <td><code>create</code></td>
</tr><tr>
  <td>namespaces</td>
  <td><code>get</code>、<code>list</code> 和 <code>watch</code></td>
  <td><code>get</code>、<code>list</code> 和 <code>watch</code></td>
  <td><code>get</code>、<code>list</code> 和 <code>watch</code></br>**仅限 cluster-admin：**<code>create</code> 和 <code>delete</code></td>
</tr><tr>
  <td>namespaces/status</td>
  <td><code>get</code>、<code>list</code> 和 <code>watch</code></td>
  <td><code>get</code>、<code>list</code> 和 <code>watch</code></td>
  <td><code>get</code>、<code>list</code> 和 <code>watch</code></td>
</tr><tr>
  <td>networkpolicies</td>
  <td><code>get</code>、<code>list</code> 和 <code>watch</code></td>
  <td><code>create</code>、<code>delete</code>、<code>deletecollection</code>、<code>get</code>、<code>list</code>、<code>patch</code>、<code>update</code> 和 <code>watch</code></td>
  <td><code>create</code>、<code>delete</code>、<code>deletecollection</code>、<code>get</code>、<code>list</code>、<code>patch</code>、<code>update</code> 和 <code>watch</code></td>
</tr><tr>
  <td>networkpolicies.extensions</td>
  <td><code>get</code>、<code>list</code> 和 <code>watch</code></td>
  <td><code>create</code>、<code>delete</code>、<code>deletecollection</code>、<code>get</code>、<code>list</code>、<code>patch</code>、<code>update</code> 和 <code>watch</code></td>
  <td><code>create</code>、<code>delete</code>、<code>deletecollection</code>、<code>get</code>、<code>list</code>、<code>patch</code>、<code>update</code> 和 <code>watch</code></td>
</tr><tr>
  <td>persistentvolumeclaims</td>
  <td><code>get</code>、<code>list</code> 和 <code>watch</code></td>
  <td><code>create</code>、<code>delete</code>、<code>deletecollection</code>、<code>get</code>、<code>list</code>、<code>patch</code>、<code>update</code> 和 <code>watch</code></td>
  <td><code>create</code>、<code>delete</code>、<code>deletecollection</code>、<code>get</code>、<code>list</code>、<code>patch</code>、<code>update</code> 和 <code>watch</code></td>
</tr><tr>
  <td>poddisruptionbudgets.policy</td>
  <td><code>get</code>、<code>list</code> 和 <code>watch</code></td>
  <td><code>create</code>、<code>delete</code>、<code>deletecollection</code>、<code>get</code>、<code>list</code>、<code>patch</code>、<code>update</code> 和 <code>watch</code></td>
  <td><code>create</code>、<code>delete</code>、<code>deletecollection</code>、<code>get</code>、<code>list</code>、<code>patch</code>、<code>update</code> 和 <code>watch</code></td>
</tr><tr>
  <td>pods</td>
  <td><code>get</code>、<code>list</code> 和 <code>watch</code></td>
  <td><code>create</code>、<code>delete</code>、<code>deletecollection</code>、<code>get</code>、<code>list</code>、<code>patch</code>、<code>update</code> 和 <code>watch</code></td>
  <td><code>create</code>、<code>delete</code>、<code>deletecollection</code>、<code>get</code>、<code>list</code>、<code>top</code>、<code>patch</code>、<code>update</code> 和 <code>watch</code></td>
</tr><tr>
  <td>pods/attach</td>
  <td>-</td>
  <td><code>create</code>、<code>delete</code>、<code>deletecollection</code>、<code>get</code>、<code>list</code>、<code>patch</code>、<code>update</code> 和 <code>watch</code></td>
  <td><code>create</code>、<code>delete</code>、<code>deletecollection</code>、<code>get</code>、<code>list</code>、<code>patch</code>、<code>update</code> 和 <code>watch</code></td>
</tr><tr>
  <td>pods/exec</td>
  <td>-</td>
  <td><code>create</code>、<code>delete</code>、<code>deletecollection</code>、<code>get</code>、<code>list</code>、<code>patch</code>、<code>update</code> 和 <code>watch</code></td>
  <td><code>create</code>、<code>delete</code>、<code>deletecollection</code>、<code>get</code>、<code>list</code>、<code>patch</code>、<code>update</code> 和 <code>watch</code></td>
</tr><tr>
  <td>pods/log</td>
  <td><code>get</code>、<code>list</code> 和 <code>watch</code></td>
  <td><code>get</code>、<code>list</code> 和 <code>watch</code></td>
  <td><code>get</code>、<code>list</code> 和 <code>watch</code></td>
</tr><tr>
  <td>pods/portforward</td>
  <td>-</td>
  <td><code>create</code>、<code>delete</code>、<code>deletecollection</code>、<code>get</code>、<code>list</code>、<code>patch</code>、<code>update</code> 和 <code>watch</code></td>
  <td><code>create</code>、<code>delete</code>、<code>deletecollection</code>、<code>get</code>、<code>list</code>、<code>patch</code>、<code>update</code> 和 <code>watch</code></td>
</tr><tr>
  <td>pods/proxy</td>
  <td>-</td>
  <td><code>create</code>、<code>delete</code>、<code>deletecollection</code>、<code>get</code>、<code>list</code>、<code>patch</code>、<code>update</code> 和 <code>watch</code></td>
  <td><code>create</code>、<code>delete</code>、<code>deletecollection</code>、<code>get</code>、<code>list</code>、<code>patch</code>、<code>update</code> 和 <code>watch</code></td>
</tr><tr>
  <td>pods/status</td>
  <td><code>get</code>、<code>list</code> 和 <code>watch</code></td>
  <td><code>get</code>、<code>list</code> 和 <code>watch</code></td>
  <td><code>get</code>、<code>list</code> 和 <code>watch</code></td>
</tr><tr>
  <td>replicasets.apps</td>
  <td><code>get</code>、<code>list</code> 和 <code>watch</code></td>
  <td><code>create</code>、<code>delete</code>、<code>deletecollection</code>、<code>get</code>、<code>list</code>、<code>patch</code>、<code>update</code> 和 <code>watch</code></td>
  <td><code>create</code>、<code>delete</code>、<code>deletecollection</code>、<code>get</code>、<code>list</code>、<code>patch</code>、<code>update</code> 和 <code>watch</code></td>
</tr><tr>
  <td>replicasets.apps/scale</td>
  <td><code>get</code>、<code>list</code> 和 <code>watch</code></td>
  <td><code>create</code>、<code>delete</code>、<code>deletecollection</code>、<code>get</code>、<code>list</code>、<code>patch</code>、<code>update</code> 和 <code>watch</code></td>
  <td><code>create</code>、<code>delete</code>、<code>deletecollection</code>、<code>get</code>、<code>list</code>、<code>patch</code>、<code>update</code> 和 <code>watch</code></td>
</tr><tr>
  <td>replicasets.extensions</td>
  <td><code>get</code>、<code>list</code> 和 <code>watch</code></td>
  <td><code>create</code>、<code>delete</code>、<code>deletecollection</code>、<code>get</code>、<code>list</code>、<code>patch</code>、<code>update</code> 和 <code>watch</code></td>
  <td><code>create</code>、<code>delete</code>、<code>deletecollection</code>、<code>get</code>、<code>list</code>、<code>patch</code>、<code>update</code> 和 <code>watch</code></td>
</tr><tr>
  <td>replicasets.extensions/scale</td>
  <td><code>get</code>、<code>list</code> 和 <code>watch</code></td>
  <td><code>create</code>、<code>delete</code>、<code>deletecollection</code>、<code>get</code>、<code>list</code>、<code>patch</code>、<code>update</code> 和 <code>watch</code></td>
  <td><code>create</code>、<code>delete</code>、<code>deletecollection</code>、<code>get</code>、<code>list</code>、<code>patch</code>、<code>update</code> 和 <code>watch</code></td>
</tr><tr>
  <td>replicationcontrollers</td>
  <td><code>get</code>、<code>list</code> 和 <code>watch</code></td>
  <td><code>create</code>、<code>delete</code>、<code>deletecollection</code>、<code>get</code>、<code>list</code>、<code>patch</code>、<code>update</code> 和 <code>watch</code></td>
  <td><code>create</code>、<code>delete</code>、<code>deletecollection</code>、<code>get</code>、<code>list</code>、<code>patch</code>、<code>update</code> 和 <code>watch</code></td>
</tr><tr>
  <td>replicationcontrollers/scale</td>
  <td><code>get</code>、<code>list</code> 和 <code>watch</code></td>
  <td><code>create</code>、<code>delete</code>、<code>deletecollection</code>、<code>get</code>、<code>list</code>、<code>patch</code>、<code>update</code> 和 <code>watch</code></td>
  <td><code>create</code>、<code>delete</code>、<code>deletecollection</code>、<code>get</code>、<code>list</code>、<code>patch</code>、<code>update</code> 和 <code>watch</code></td>
</tr><tr>
  <td>replicationcontrollers/status</td>
  <td><code>get</code>、<code>list</code> 和 <code>watch</code></td>
  <td><code>get</code>、<code>list</code> 和 <code>watch</code></td>
  <td><code>get</code>、<code>list</code> 和 <code>watch</code></td>
</tr><tr>
  <td>replicationcontrollers.extensions/scale</td>
  <td><code>get</code>、<code>list</code> 和 <code>watch</code></td>
  <td><code>create</code>、<code>delete</code>、<code>deletecollection</code>、<code>get</code>、<code>list</code>、<code>patch</code>、<code>update</code> 和 <code>watch</code></td>
  <td><code>create</code>、<code>delete</code>、<code>deletecollection</code>、<code>get</code>、<code>list</code>、<code>patch</code>、<code>update</code> 和 <code>watch</code></td>
</tr><tr>
  <td>resourcequotas</td>
  <td><code>get</code>、<code>list</code> 和 <code>watch</code></td>
  <td><code>get</code>、<code>list</code> 和 <code>watch</code></td>
  <td><code>get</code>、<code>list</code> 和 <code>watch</code></td>
</tr><tr>
  <td>resourcequotas/status</td>
  <td><code>get</code>、<code>list</code> 和 <code>watch</code></td>
  <td><code>get</code>、<code>list</code> 和 <code>watch</code></td>
  <td><code>get</code>、<code>list</code> 和 <code>watch</code></td>
</tr><tr>
  <td>rolebindings</td>
  <td>-</td>
  <td>-</td>
  <td><code>create</code>、<code>delete</code>、<code>deletecollection</code>、<code>get</code>、<code>list</code>、<code>patch</code>、<code>update</code> 和 <code>watch</code></td>
</tr><tr>
  <td>角色
</td>
  <td>-</td>
  <td>-</td>
  <td><code>create</code>、<code>delete</code>、<code>deletecollection</code>、<code>get</code>、<code>list</code>、<code>patch</code>、<code>update</code> 和 <code>watch</code></td>
</tr><tr>
  <td>私钥</td>
  <td>-</td>
  <td><code>create</code>、<code>delete</code>、<code>deletecollection</code>、<code>get</code>、<code>list</code>、<code>patch</code>、<code>update</code> 和 <code>watch</code></td>
  <td><code>create</code>、<code>delete</code>、<code>deletecollection</code>、<code>get</code>、<code>list</code>、<code>patch</code>、<code>update</code> 和 <code>watch</code></td>
</tr><tr>
  <td> serviceaccounts                               </td>
  <td><code>get</code>、<code>list</code> 和 <code>watch</code></td>
  <td><code>create</code>、<code>delete</code>、<code>deletecollection</code>、<code>get</code>、<code>list</code>、<code>patch</code>、<code>update</code>、<code>watch</code> 和 <code>impersonate</code></td>
  <td><code>create</code>、<code>delete</code>、<code>deletecollection</code>、<code>get</code>、<code>list</code>、<code>patch</code>、<code>update</code>、<code>watch</code> 和 <code>impersonate</code></td>
</tr><tr>
  <td>服务</td>
  <td><code>get</code>、<code>list</code> 和 <code>watch</code></td>
  <td><code>create</code>、<code>delete</code>、<code>deletecollection</code>、<code>get</code>、<code>list</code>、<code>patch</code>、<code>update</code> 和 <code>watch</code></td>
  <td><code>create</code>、<code>delete</code>、<code>deletecollection</code>、<code>get</code>、<code>list</code>、<code>patch</code>、<code>update</code> 和 <code>watch</code></td>
</tr><tr>
  <td>services/proxy</td>
  <td>-</td>
  <td><code>create</code>、<code>delete</code>、<code>deletecollection</code>、<code>get</code>、<code>list</code>、<code>patch</code>、<code>update</code> 和 <code>watch</code></td>
  <td><code>create</code>、<code>delete</code>、<code>deletecollection</code>、<code>get</code>、<code>list</code>、<code>patch</code>、<code>update</code> 和 <code>watch</code></td>
</tr><tr>
  <td>statefulsets.apps</td>
  <td><code>get</code>、<code>list</code> 和 <code>watch</code></td>
  <td><code>create</code>、<code>delete</code>、<code>deletecollection</code>、<code>get</code>、<code>list</code>、<code>patch</code>、<code>update</code> 和 <code>watch</code></td>
  <td><code>create</code>、<code>delete</code>、<code>deletecollection</code>、<code>get</code>、<code>list</code>、<code>patch</code>、<code>update</code> 和 <code>watch</code></td>
</tr><tr>
  <td>statefulsets.apps/scale</td>
  <td><code>get</code>、<code>list</code> 和 <code>watch</code></td>
  <td><code>create</code>、<code>delete</code>、<code>deletecollection</code>、<code>get</code>、<code>list</code>、<code>patch</code>、<code>update</code> 和 <code>watch</code></td>
  <td><code>create</code>、<code>delete</code>、<code>deletecollection</code>、<code>get</code>、<code>list</code>、<code>patch</code>、<code>update</code> 和 <code>watch</code></td>
</tr>
</tbody>
</table>

<br />


## Cloud Foundry 角色
{: #cloud-foundry}

Cloud Foundry 角色会授予对帐户内组织和空间的访问权。要在 {{site.data.keyword.Bluemix_notm}} 中查看基于 Cloud Foundry 的服务的列表，请运行 `ibmcloud service list`。要了解更多信息，请参阅 {{site.data.keyword.Bluemix_notm}} IAM 文档中的所有可用[组织和空间角色](/docs/iam?topic=iam-cfaccess)或[管理 Cloud Foundry 访问权](/docs/iam?topic=iam-mngcf)的步骤。
{: shortdesc}

下表显示了集群操作许可权所需的 Cloud Foundry 角色。

<table>
  <caption>集群管理许可权（按 Cloud Foundry 角色）</caption>
  <thead>
    <th>Cloud Foundry 角色</th>
    <th>集群管理许可权</th>
  </thead>
  <tbody>
  <tr>
    <td>空间角色：管理员</td>
    <td>管理用户对 {{site.data.keyword.Bluemix_notm}} 空间的访问权</td>
  </tr>
  <tr>
    <td>空间角色：开发者</td>
    <td>
      <ul><li>创建 {{site.data.keyword.Bluemix_notm}} 服务实例</li>
      <li>将 {{site.data.keyword.Bluemix_notm}} 服务实例绑定到集群</li>
      <li>通过集群日志转发配置在空间级别查看日志</li></ul>
    </td>
  </tr>
  </tbody>
</table>

## 基础架构角色
{: #infra}

具有**超级用户**基础架构访问角色的用户[为区域和资源组设置 API 密钥](/docs/containers?topic=containers-users#api_key)时，帐户中其他用户的基础架构许可权由 {{site.data.keyword.Bluemix_notm}} IAM 平台角色进行设置。您无需编辑其他用户的 IBM Cloud Infrastructure (SoftLayer) 许可权。仅当无法将**超级用户**分配给设置 API 密钥的用户时，才可以使用下表来定制用户的 IBM Cloud Infrastructure (SoftLayer) 许可权。有关更多信息，请参阅[定制基础架构许可权](/docs/containers?topic=containers-users#infra_access)。
{: shortdesc}

下表显示了完成常见任务组所需的基础架构许可权。

<table>
 <caption>{{site.data.keyword.containerlong_notm}} 通常必需的基础架构许可权</caption>
 <thead>
  <th>{{site.data.keyword.containerlong_notm}} 中的常见任务</th>
  <th>必需的基础架构许可权（按类别）</th>
 </thead>
 <tbody>
   <tr>
     <td><strong>最低许可权</strong>：<ul><li>创建集群。</li></ul></td>
     <td><strong>设备</strong>：<ul><li>查看虚拟服务器详细信息</li><li>重新引导服务器并查看 IPMI 系统信息</li><li>发出操作系统重装并启动急救内核</li></ul><strong>帐户</strong>：<ul><li>添加服务器</li></ul></td>
   </tr>
   <tr>
     <td><strong>集群管理</strong>：<ul><li>创建、更新和删除集群。</li><li>添加、重新装入和重新引导工作程序节点。</li><li>查看 VLAN。</li><li>创建子网。</li><li>部署 pod 和 LoadBalancer 服务。</li></ul></td>
     <td><strong>支持</strong>：<ul><li>查看凭单</li><li>添加凭单</li><li>编辑凭单</li></ul>
     <strong>设备</strong>：<ul><li>查看硬件详细信息</li><li>查看虚拟服务器详细信息</li><li>重新引导服务器并查看 IPMI 系统信息</li><li>发出操作系统重装并启动急救内核</li></ul>
     <strong>网络</strong>：<ul><li>使用公用网络端口添加计算</li></ul>
     <strong>帐户</strong>：<ul><li>取消服务器</li><li>添加服务器</li></ul></td>
   </tr>
   <tr>
     <td><strong>存储器</strong>：<ul><li>创建持久卷声明以供应持久卷。</li><li>创建和管理存储器基础架构资源。</li></ul></td>
     <td><strong>服务</strong>：<ul><li>管理存储器</li></ul><strong>帐户</strong>：<ul><li>添加存储器</li></ul></td>
   </tr>
   <tr>
     <td><strong>专用联网</strong>：<ul><li>管理用于集群内联网的专用 VLAN。</li><li>设置与专用网络的 VPN 连接。</li></ul></td>
     <td><strong>网络</strong>：<ul><li>管理网络子网路径</li></ul></td>
   </tr>
   <tr>
     <td><strong>公用网络</strong>：<ul><li>设置公共负载均衡器或 Ingress 联网以公开应用程序。</li></ul></td>
     <td><strong>设备</strong>：<ul><li>编辑主机名/域</li><li>管理端口控制</li></ul>
     <strong>网络</strong>：<ul><li>使用公用网络端口添加计算</li><li>管理网络子网路径</li><li>添加 IP 地址</li></ul>
     <strong>服务</strong>：<ul><li>管理 DNS、逆向 DNS 和 WHOIS</li><li>查看证书 (SSL)</li><li>管理证书 (SSL)</li></ul></td>
   </tr>
 </tbody>
</table>
