---
title: Azure HDInsight Python SDK (プレビュー)
description: Azure HDInsight Python SDK のリファレンス。 HDInsight Python SDK には、HDInsight クラスターの管理に使用できるクラスとメソッドが用意されています。
ms.service: hdinsight
author: tylerfox
ms.author: tyfox
ms.date: 09/18/2018
ms.topic: reference
ms.devlang: python
ms.openlocfilehash: 9447d50fd734bd9221accbf470a456210bb57a7f
ms.sourcegitcommit: e2e4b1ecfac9804a72973477634128061c1ec990
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/17/2018
ms.locfileid: "53455109"
---
# <a name="hdinsight-python-management-sdk-preview"></a><span data-ttu-id="1053d-104">HDInsight Python Management SDK (プレビュー)</span><span class="sxs-lookup"><span data-stu-id="1053d-104">HDInsight Python Management SDK Preview</span></span>

## <a name="overview"></a><span data-ttu-id="1053d-105">概要</span><span class="sxs-lookup"><span data-stu-id="1053d-105">Overview</span></span>

<span data-ttu-id="1053d-106">HDInsight Python SDK には、HDInsight クラスターの管理に使用できるクラスとメソッドが用意されています。</span><span class="sxs-lookup"><span data-stu-id="1053d-106">The HDInsight Python SDK provides classes and methods that allow you to manage your HDInsight clusters.</span></span> <span data-ttu-id="1053d-107">これには、スクリプト アクションを作成、削除、更新、一覧表示、サイズ変更、実行したり、HDInsight クラスターのプロパティを監視、取得したりする操作が含まれます。</span><span class="sxs-lookup"><span data-stu-id="1053d-107">It includes operations to create, delete, update, list, resize, execute script actions, monitor, get properties of HDInsight clusters, and more.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="1053d-108">前提条件</span><span class="sxs-lookup"><span data-stu-id="1053d-108">Prerequisites</span></span>

* <span data-ttu-id="1053d-109">Azure アカウント。</span><span class="sxs-lookup"><span data-stu-id="1053d-109">An Azure account.</span></span> <span data-ttu-id="1053d-110">所有していない場合は、[無料試用版を入手](https://azure.microsoft.com/free/)してください。</span><span class="sxs-lookup"><span data-stu-id="1053d-110">If you don't have one, [get a free trial](https://azure.microsoft.com/free/).</span></span>
* [<span data-ttu-id="1053d-111">Python</span><span class="sxs-lookup"><span data-stu-id="1053d-111">Python</span></span>](https://www.python.org/downloads/)
* [<span data-ttu-id="1053d-112">pip</span><span class="sxs-lookup"><span data-stu-id="1053d-112">pip</span></span>](https://pypi.org/project/pip/#description)

## <a name="sdk-installation"></a><span data-ttu-id="1053d-113">SDK のインストール</span><span class="sxs-lookup"><span data-stu-id="1053d-113">SDK Installation</span></span>

<span data-ttu-id="1053d-114">HDInsight Python SDK は、[Python Package Index](https://pypi.org/project/azure-mgmt-hdinsight/) にあり、次のコマンドを実行してインストールできます。</span><span class="sxs-lookup"><span data-stu-id="1053d-114">The HDInsight Python SDK can be found in the [Python Package Index](https://pypi.org/project/azure-mgmt-hdinsight/) and can be installed by running:</span></span> 

`pip install azure-mgmt-hdinsight`

## <a name="authentication"></a><span data-ttu-id="1053d-115">Authentication</span><span class="sxs-lookup"><span data-stu-id="1053d-115">Authentication</span></span>

<span data-ttu-id="1053d-116">SDK は最初に Azure サブスクリプションで認証する必要があります。</span><span class="sxs-lookup"><span data-stu-id="1053d-116">The SDK first needs to be authenticated with your Azure subscription.</span></span>  <span data-ttu-id="1053d-117">以下の例に従って、サービス プリンシパルを作成し、これを使用して認証します。</span><span class="sxs-lookup"><span data-stu-id="1053d-117">Follow the example below to create a service principal and use it to authenticate.</span></span> <span data-ttu-id="1053d-118">その後、`HDInsightManagementClient` のインスタンスが生成されます。これには、管理操作の実行に使用できるメソッドが多数含まれています (以下のセクションで説明します)。</span><span class="sxs-lookup"><span data-stu-id="1053d-118">After this is done, you will have an instance of an `HDInsightManagementClient`, which contains many methods (outlined in below sections) that can be used to perform management operations.</span></span>

> [!NOTE]
> <span data-ttu-id="1053d-119">認証方法は以下の例の他にもあり、そちらの方がご自身のニーズに適している可能性もあります。</span><span class="sxs-lookup"><span data-stu-id="1053d-119">There are other ways to authenticate besides the below example that could potentially be better suited for your needs.</span></span> <span data-ttu-id="1053d-120">すべてのメソッドの概要については、「[Python 用 Azure 管理ライブラリを使用した認証](https://docs.microsoft.com/en-us/python/azure/python-sdk-azure-authenticate?view=azure-python)」をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="1053d-120">All methods are outlined here: [Authenticate with the Azure Management Libraries for Python](https://docs.microsoft.com/en-us/python/azure/python-sdk-azure-authenticate?view=azure-python)</span></span>

### <a name="authentication-example-using-a-service-principal"></a><span data-ttu-id="1053d-121">サービス プリンシパルを使用した認証の例</span><span class="sxs-lookup"><span data-stu-id="1053d-121">Authentication Example Using a Service Principal</span></span>

<span data-ttu-id="1053d-122">まず、[Azure Cloud Shell](https://shell.azure.com/bash) にログインします。</span><span class="sxs-lookup"><span data-stu-id="1053d-122">First, login to [Azure Cloud Shell](https://shell.azure.com/bash).</span></span> <span data-ttu-id="1053d-123">現在、サービス プリンシパル作成対象のサブスクリプションを使用していることを確認します。</span><span class="sxs-lookup"><span data-stu-id="1053d-123">Verify you are currently using the subscription in which you want the service principal created.</span></span> 

```azurecli-interactive
az account show
```

<span data-ttu-id="1053d-124">ご自身のサブスクリプション情報が JSON として表示されます。</span><span class="sxs-lookup"><span data-stu-id="1053d-124">Your subscription information is displayed as JSON.</span></span>

```json
{
  "environmentName": "AzureCloud",
  "id": "XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX",
  "isDefault": true,
  "name": "XXXXXXX",
  "state": "Enabled",
  "tenantId": "XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX",
  "user": {
    "cloudShellID": true,
    "name": "XXX@XXX.XXX",
    "type": "user"
  }
}
```

<span data-ttu-id="1053d-125">正しいサブスクリプションにログインしていない場合は、以下を実行して正しいサブスクリプションを選択します。</span><span class="sxs-lookup"><span data-stu-id="1053d-125">If you're not logged into the correct subscription, select the correct one by running:</span></span> 
```azurecli-interactive
az account set -s <name or ID of subscription>
```

> [!IMPORTANT]
> <span data-ttu-id="1053d-126">他の方法で、たとえば Azure portal で HDInsight クラスターを作成する、といった方法で HDInsight リソース プロバイダーをまだ登録していない場合は、認証の前にこれを一度実行する必要があります。</span><span class="sxs-lookup"><span data-stu-id="1053d-126">If you have not already registered the HDInsight Resource Provider by another method (such as by creating an HDInsight Cluster through the Azure Portal), you need to do this once before you can authenticate.</span></span> <span data-ttu-id="1053d-127">これを [Azure Cloud Shell](https://shell.azure.com/bash) から実行するには、次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="1053d-127">This can be done from the [Azure Cloud Shell](https://shell.azure.com/bash) by running the following command:</span></span>
>```azurecli-interactive
>az provider register --namespace Microsoft.HDInsight
>```

<span data-ttu-id="1053d-128">次に、ご自身のサービス プリンシパルの名前を選択し、次のコマンドを使用して作成します。</span><span class="sxs-lookup"><span data-stu-id="1053d-128">Next, choose a name for your service principal and create it with the following command:</span></span>

```azurecli-interactive
az ad sp create-for-rbac --name <Service Principal Name> --sdk-auth
```

<span data-ttu-id="1053d-129">サービス プリンシパル情報が JSON として表示されます。</span><span class="sxs-lookup"><span data-stu-id="1053d-129">The service principal information is displayed as JSON.</span></span>

```json
{
  "clientId": "XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX",
  "clientSecret": "XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX",
  "subscriptionId": "XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX",
  "tenantId": "XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX",
  "activeDirectoryEndpointUrl": "https://login.microsoftonline.com",
  "resourceManagerEndpointUrl": "https://management.azure.com/",
  "activeDirectoryGraphResourceId": "https://graph.windows.net/",
  "sqlManagementEndpointUrl": "https://management.core.windows.net:8443/",
  "galleryEndpointUrl": "https://gallery.azure.com/",
  "managementEndpointUrl": "https://management.core.windows.net/"
}
```
<span data-ttu-id="1053d-130">次の Python スニペットをコピーし、コマンド実行後に返された JSON の文字列を `TENANT_ID`、`CLIENT_ID`、`CLIENT_SECRET`、`SUBSCRIPTION_ID` に入力して、サービス プリンシパルを作成します。</span><span class="sxs-lookup"><span data-stu-id="1053d-130">Copy the below Python snippet and fill in `TENANT_ID`, `CLIENT_ID`, `CLIENT_SECRET`, and `SUBSCRIPTION_ID` with the strings from the JSON that was returned after running the command to create the service principal.</span></span>

```python
from azure.mgmt.hdinsight import HDInsightManagementClient
from azure.common.credentials import ServicePrincipalCredentials
from azure.mgmt.hdinsight.models import *

# Tenant ID for your Azure Subscription
TENANT_ID = ''
# Your Service Principal App Client ID
CLIENT_ID = ''
# Your Service Principal Client Secret
CLIENT_SECRET = ''
# Your Azure Subscription ID
SUBSCRIPTION_ID = ''

credentials = ServicePrincipalCredentials(
    client_id = CLIENT_ID,
    secret = CLIENT_SECRET,
    tenant = TENANT_ID
)

client = HDInsightManagementClient(credentials, SUBSCRIPTION_ID)
```


## <a name="cluster-management"></a><span data-ttu-id="1053d-131">クラスターの管理</span><span class="sxs-lookup"><span data-stu-id="1053d-131">Cluster Management</span></span>

> [!NOTE]
> <span data-ttu-id="1053d-132">このセクションでは、`HDInsightManagementClient` インスタンスの認証と構成が既に完了していること、および `client` と呼ばれる変数にそのインスタンスが格納されていることを前提としています。</span><span class="sxs-lookup"><span data-stu-id="1053d-132">This section assumes you have already authenticated and constructed an `HDInsightManagementClient` instance and store it in a variable called `client`.</span></span> <span data-ttu-id="1053d-133">`HDInsightManagementClient` を認証および取得する方法については、上記の「認証」セクションを参照してください。</span><span class="sxs-lookup"><span data-stu-id="1053d-133">Instructions for authenticating and obtaining an `HDInsightManagementClient` can be found in the Authentication section above.</span></span>

### <a name="create-a-cluster"></a><span data-ttu-id="1053d-134">クラスターの作成</span><span class="sxs-lookup"><span data-stu-id="1053d-134">Create a Cluster</span></span>

<span data-ttu-id="1053d-135">新しいクラスターを作成するには、`client.clusters.create()` を呼び出します。</span><span class="sxs-lookup"><span data-stu-id="1053d-135">A new cluster can be created by calling `client.clusters.create()`.</span></span> 

#### <a name="example"></a><span data-ttu-id="1053d-136">例</span><span class="sxs-lookup"><span data-stu-id="1053d-136">Example</span></span>

<span data-ttu-id="1053d-137">この例は、2 つのヘッド ノードと 1 つの worker ノードを含む Spark クラスターを作成する方法を示しています。</span><span class="sxs-lookup"><span data-stu-id="1053d-137">This example demonstrates how to create a Spark cluster with 2 head nodes and 1 worker node.</span></span>

> [!NOTE]
> <span data-ttu-id="1053d-138">次に示すように、最初にリソース グループとストレージ アカウントを作成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="1053d-138">You first need to create a Resource Group and Storage Account, as explained below.</span></span> <span data-ttu-id="1053d-139">これらが既に作成済みの場合、この手順はスキップできます。</span><span class="sxs-lookup"><span data-stu-id="1053d-139">If you have already created these, you can skip these steps.</span></span>

##### <a name="creating-a-resource-group"></a><span data-ttu-id="1053d-140">リソース グループの作成</span><span class="sxs-lookup"><span data-stu-id="1053d-140">Creating a Resource Group</span></span>

<span data-ttu-id="1053d-141">[Azure Cloud Shell](https://shell.azure.com/bash) を使用して次を実行することで、リソース グループを作成できます</span><span class="sxs-lookup"><span data-stu-id="1053d-141">You can create a resource group using the [Azure Cloud Shell](https://shell.azure.com/bash) by running</span></span>
```azurecli-interactive
az group create -l <Region Name (i.e. eastus)> --n <Resource Group Name>
```
##### <a name="creating-a-storage-account"></a><span data-ttu-id="1053d-142">ストレージ アカウントの作成</span><span class="sxs-lookup"><span data-stu-id="1053d-142">Creating a Storage Account</span></span>

<span data-ttu-id="1053d-143">[Azure Cloud Shell](https://shell.azure.com/bash) を使用して次を実行することで、ストレージ アカウントを作成できます。</span><span class="sxs-lookup"><span data-stu-id="1053d-143">You can create a storage account using the [Azure Cloud Shell](https://shell.azure.com/bash) by running:</span></span>
```azurecli-interactive
az storage account create -n <Storage Account Name> -g <Existing Resource Group Name> -l <Region Name (i.e. eastus)> --sku <SKU i.e. Standard_LRS>
```
<span data-ttu-id="1053d-144">ここで、次のコマンドを実行して、ストレージ アカウントに対するキーを取得します (これはクラスターを作成するときに必要になります)。</span><span class="sxs-lookup"><span data-stu-id="1053d-144">Now run the following command to get the key for your storage account (you will need this to create a cluster):</span></span>
```azurecli-interactive
az storage account keys list -n <Storage Account Name>
```
---
<span data-ttu-id="1053d-145">次の Python スニペットでは、2 つのヘッド ノードと 1 つの worker ノードを含む Spark クラスターを作成します。</span><span class="sxs-lookup"><span data-stu-id="1053d-145">The below Python snippet creates a Spark cluster with 2 head nodes and 1 worker node.</span></span> <span data-ttu-id="1053d-146">コメントの説明に従って空白の変数を入力します。また、ご自身のニーズに合わせて他のパラメーターを変更します。</span><span class="sxs-lookup"><span data-stu-id="1053d-146">Fill in the blank variables as explained in the comments and feel free to change other parameters to suit your specific needs.</span></span>

```python
# The name for the cluster you are creating
cluster_name = ""
# The name of your existing Resource Group
resource_group_name = ""
# Choose a username
username = ""
# Choose a password
password = ""
# Replace <> with the name of your storage account
storage_account = "<>.blob.core.windows.net"
# Storage account key you obtained above
storage_account_key = ""
# Choose a region
location = ""
container = "default"

params = ClusterCreateProperties(
    cluster_version="3.6",
    os_type=OSType.linux,
    tier=Tier.standard,
    cluster_definition=ClusterDefinition(
        kind="spark",
        configurations={
            "gateway": {
                "restAuthCredential.enabled_credential": "True",
                "restAuthCredential.username": username,
                "restAuthCredential.password": password
            }
        }
    ),
    compute_profile=ComputeProfile(
        roles=[
            Role(
                name="headnode",
                target_instance_count=2,
                hardware_profile=HardwareProfile(vm_size="Large"),
                os_profile=OsProfile(
                    linux_operating_system_profile=LinuxOperatingSystemProfile(
                        username=username,
                        password=password
                    )
                )
            ),
            Role(
                name="workernode",
                target_instance_count=1,
                hardware_profile=HardwareProfile(vm_size="Large"),
                os_profile=OsProfile(
                    linux_operating_system_profile=LinuxOperatingSystemProfile(
                        username=username,
                        password=password
                    )
                )
            )
        ]
    ),
    storage_profile=StorageProfile(
        storageaccounts=[StorageAccount(
            name=storage_account,
            key=storage_account_key,
            container=container,
            is_default=True
        )]
    )
)

client.clusters.create(
    cluster_name=cluster_name,
    resource_group_name=resource_group_name,
    parameters=ClusterCreateParametersExtended(
        location=location,
        tags={},
        properties=params
    ))
```

### <a name="get-cluster-details"></a><span data-ttu-id="1053d-147">クラスターの詳細の取得</span><span class="sxs-lookup"><span data-stu-id="1053d-147">Get Cluster Details</span></span>

<span data-ttu-id="1053d-148">特定のクラスターのプロパティを取得するには:</span><span class="sxs-lookup"><span data-stu-id="1053d-148">To get properties for a given cluster:</span></span>

```python
client.clusters.get("<Resource Group Name>", "<Cluster Name>")
```

#### <a name="example"></a><span data-ttu-id="1053d-149">例</span><span class="sxs-lookup"><span data-stu-id="1053d-149">Example</span></span>

<span data-ttu-id="1053d-150">`get` を使用すると、ご自身のクラスターを適切に作成できたことを確認できます。</span><span class="sxs-lookup"><span data-stu-id="1053d-150">You can use `get` to confirm that you have successfully created your cluster.</span></span>

```python
my_cluster = client.clusters.get("<Resource Group Name>", "<Cluster Name>")
print(my_cluster)
```

<span data-ttu-id="1053d-151">出力は次のようになります。</span><span class="sxs-lookup"><span data-stu-id="1053d-151">The output should look like:</span></span>

```
{'additional_properties': {}, 'id': '/subscriptions/XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX/resourceGroups/<Resource Group Name>/providers/Microsoft.HDInsight/clusters/<Cluster Name>', 'name': '<Cluster Name>', 'type': 'Microsoft.HDInsight/clusters', 'location': '<Location>', 'tags': {}, 'etag': 'XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX', 'properties': <azure.mgmt.hdinsight.models.cluster_get_properties_py3.ClusterGetProperties object at 0x0000013766D68048>}
```

### <a name="list-clusters"></a><span data-ttu-id="1053d-152">クラスターの一覧表示</span><span class="sxs-lookup"><span data-stu-id="1053d-152">List Clusters</span></span>

#### <a name="list-clusters-under-the-subscription"></a><span data-ttu-id="1053d-153">サブスクリプションのクラスターの一覧表示</span><span class="sxs-lookup"><span data-stu-id="1053d-153">List Clusters Under The Subscription</span></span>

```python
client.clusters.list()
```
#### <a name="list-clusters-by-resource-group"></a><span data-ttu-id="1053d-154">リソース グループ別のクラスターの一覧表示</span><span class="sxs-lookup"><span data-stu-id="1053d-154">List Clusters By Resource Group</span></span>

```python
client.clusters.list_by_resource_group("<Resource Group Name>")
```
> [!NOTE]
> <span data-ttu-id="1053d-155">`list()` と `list_by_resource_group()` の両方が `ClusterPaged` オブジェクトを返します。</span><span class="sxs-lookup"><span data-stu-id="1053d-155">Both `list()` and `list_by_resource_group()` return an `ClusterPaged` object.</span></span> <span data-ttu-id="1053d-156">`advance_page()` を呼び出すと、そのページ上にクラスターの一覧が返され、`ClusterPaged` オブジェクトが次のページに進められます。</span><span class="sxs-lookup"><span data-stu-id="1053d-156">Calling `advance_page()` returns the a list of clusters on that page and advances the `ClusterPaged` object to the next page.</span></span> <span data-ttu-id="1053d-157">これは、`StopIteration` 例外が発生し、それ以上ページがないことが示されるまで繰り返されます。</span><span class="sxs-lookup"><span data-stu-id="1053d-157">This can be repeated until a `StopIteration` exception is raised, indicating that there are no more pages.</span></span>

#### <a name="example"></a><span data-ttu-id="1053d-158">例</span><span class="sxs-lookup"><span data-stu-id="1053d-158">Example</span></span>

<span data-ttu-id="1053d-159">次の例では、現在のサブスクリプションのすべてのクラスターのプロパティが出力されます。</span><span class="sxs-lookup"><span data-stu-id="1053d-159">The following example prints the properties of all clusters for the current subscription:</span></span>

```python
clusters_paged = client.clusters.list()
while True:
  try:
    for cluster in clusters_paged.advance_page():
      print(cluster)
  except StopIteration: 
    break
```

### <a name="delete-a-cluster"></a><span data-ttu-id="1053d-160">クラスターの削除</span><span class="sxs-lookup"><span data-stu-id="1053d-160">Delete a Cluster</span></span>

<span data-ttu-id="1053d-161">クラスターを削除するには:</span><span class="sxs-lookup"><span data-stu-id="1053d-161">To delete a cluster:</span></span>

```python
client.clusters.delete("<Resource Group Name>", "<Cluster Name>")
```

### <a name="update-cluster-tags"></a><span data-ttu-id="1053d-162">クラスター タグの更新</span><span class="sxs-lookup"><span data-stu-id="1053d-162">Update Cluster Tags</span></span>

<span data-ttu-id="1053d-163">次のように、特定のクラスターのタグを更新できます。</span><span class="sxs-lookup"><span data-stu-id="1053d-163">You can update the tags of a given cluster like so:</span></span>

```python
client.clusters.update("<Resource Group Name>", "<Cluster Name>", tags={<Dictionary of Tags>})
```

#### <a name="example"></a><span data-ttu-id="1053d-164">例</span><span class="sxs-lookup"><span data-stu-id="1053d-164">Example</span></span>

```python
client.clusters.update("<Resource Group Name>", "<Cluster Name>", tags={"tag1Name" : "tag1Value", "tag2Name" : "tag2Value"})
```

### <a name="resize-cluster"></a><span data-ttu-id="1053d-165">クラスターのサイズ変更</span><span class="sxs-lookup"><span data-stu-id="1053d-165">Resize Cluster</span></span>

<span data-ttu-id="1053d-166">特定のクラスターの worker ノード数をサイズ変更するには、次のように新しいサイズを指定します。</span><span class="sxs-lookup"><span data-stu-id="1053d-166">You can resize a given cluster's number of worker nodes by specifying a new size like so:</span></span>

```python
client.clusters.resize("<Resource Group Name>", "<Cluster Name>", target_instance_count=<Num of Worker Nodes>)
```

## <a name="cluster-monitoring"></a><span data-ttu-id="1053d-167">クラスターの監視</span><span class="sxs-lookup"><span data-stu-id="1053d-167">Cluster Monitoring</span></span>

<span data-ttu-id="1053d-168">HDInsight 管理 SDK を使用して、Operations Management Suite (OMS) でご自身のクラスターの監視を管理することもできます。</span><span class="sxs-lookup"><span data-stu-id="1053d-168">The HDInsight Management SDK can also be used to manage monitoring on your clusters via the Operations Management Suite (OMS).</span></span>

### <a name="enable-oms-monitoring"></a><span data-ttu-id="1053d-169">OMS 監視の有効化</span><span class="sxs-lookup"><span data-stu-id="1053d-169">Enable OMS Monitoring</span></span>

> [!NOTE]
> <span data-ttu-id="1053d-170">OMS の監視を有効にするには、既存の Log Analytics ワークスペースが必要です。</span><span class="sxs-lookup"><span data-stu-id="1053d-170">To enable OMS Monitoring, you must have an existing Log Analytics workspace.</span></span> <span data-ttu-id="1053d-171">まだ作成していない場合、その方法については、「[Azure portal で Log Analytics ワークスペースを作成する](https://docs.microsoft.com/en-us/azure/log-analytics/log-analytics-quick-create-workspace)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="1053d-171">If you have not already created one, you can learn how to do that here: [Create a Log Analytics workspace in the Azure portal](https://docs.microsoft.com/en-us/azure/log-analytics/log-analytics-quick-create-workspace).</span></span>

<span data-ttu-id="1053d-172">ご自身のクラスターで OMS 監視を有効にするには:</span><span class="sxs-lookup"><span data-stu-id="1053d-172">To enable OMS Monitoring on your cluster:</span></span>

```python
client.extension.enable_monitoring("<Resource Group Name>", "<Cluster Name>", workspace_id="<Workspace Id>")
```

### <a name="view-status-of-oms-monitoring"></a><span data-ttu-id="1053d-173">OMS 監視の状態の表示</span><span class="sxs-lookup"><span data-stu-id="1053d-173">View Status Of OMS Monitoring</span></span>

<span data-ttu-id="1053d-174">ご自身のクラスターに対する OMS の状態を取得するには:</span><span class="sxs-lookup"><span data-stu-id="1053d-174">To get the status of OMS on your cluster:</span></span>

```python
client.extension.get_monitoring_status("<Resource Group Name", "Cluster Name")
```

### <a name="disable-oms-monitoring"></a><span data-ttu-id="1053d-175">OMS 監視の無効化</span><span class="sxs-lookup"><span data-stu-id="1053d-175">Disable OMS Monitoring</span></span>

<span data-ttu-id="1053d-176">ご自身のクラスターに対する OMS を無効にするには:</span><span class="sxs-lookup"><span data-stu-id="1053d-176">To disable OMS on your cluster:</span></span>

```python
client.extension.disable_monitoring("<Resource Group Name>", "<Cluster Name>")
```

## <a name="script-actions"></a><span data-ttu-id="1053d-177">[スクリプト操作]</span><span class="sxs-lookup"><span data-stu-id="1053d-177">Script Actions</span></span>

<span data-ttu-id="1053d-178">HDInsight には、クラスターをカスタマイズするためにカスタム スクリプトを呼び出すスクリプト アクションという構成方法があります。</span><span class="sxs-lookup"><span data-stu-id="1053d-178">HDInsight provides a configuration method called script actions that invokes custom scripts to customize the cluster.</span></span>
> [!NOTE]
> <span data-ttu-id="1053d-179">スクリプト アクションの使用方法の詳細については、「[スクリプト アクションを使用して Linux ベースの HDInsight クラスターをカスタマイズする](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-hadoop-customize-cluster-linux)」を参照してください</span><span class="sxs-lookup"><span data-stu-id="1053d-179">More information on how to use script actions can be found here: [Customize Linux-based HDInsight clusters using script actions](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-hadoop-customize-cluster-linux)</span></span>

### <a name="execute-script-actions"></a><span data-ttu-id="1053d-180">スクリプト アクションの実行</span><span class="sxs-lookup"><span data-stu-id="1053d-180">Execute Script Actions</span></span>
<span data-ttu-id="1053d-181">特定のクラスターに対してスクリプト アクションを実行するには:</span><span class="sxs-lookup"><span data-stu-id="1053d-181">To execute script actions on a given cluster:</span></span>

```python
script_action1 = RuntimeScriptAction(name="<Script Name>", uri="<URL To Script>", roles=[<List of Roles>]) #valid roles are "headnode", "workernode", "zookeepernode", and "edgenode"

client.clusters.execute_script_actions("<Resource Group Name>", "<Cluster Name>", <persist_on_success (bool)>, script_actions=[script_action1]) #add more RuntimeScriptActions to the list to execute multiple scripts
```

### <a name="delete-script-action"></a><span data-ttu-id="1053d-182">スクリプト アクションの削除</span><span class="sxs-lookup"><span data-stu-id="1053d-182">Delete Script Action</span></span>

<span data-ttu-id="1053d-183">特定のクラスターに対して指定した保存済みスクリプト アクションを削除するには:</span><span class="sxs-lookup"><span data-stu-id="1053d-183">To delete a specified persisted script action on a given cluster:</span></span>

```python
client.script_actions.delete("<Resource Group Name>", "<Cluster Name", "<Script Name>")
```

### <a name="list-persisted-script-actions"></a><span data-ttu-id="1053d-184">保存済みスクリプト アクションの一覧表示</span><span class="sxs-lookup"><span data-stu-id="1053d-184">List Persisted Script Actions</span></span>

> [!NOTE]
> <span data-ttu-id="1053d-185">`list()` と `list_persisted_scripts()` は `RuntimeScriptActionDetailPaged` オブジェクトを返します。</span><span class="sxs-lookup"><span data-stu-id="1053d-185">`list()` and `list_persisted_scripts()` return a `RuntimeScriptActionDetailPaged` object.</span></span> <span data-ttu-id="1053d-186">`advance_page()` を呼び出すと、そのページ上に `RuntimeScriptActionDetailPaged` の一覧が返され、`RuntimeScriptActionDetail` オブジェクトが次のページに進められます。</span><span class="sxs-lookup"><span data-stu-id="1053d-186">Calling `advance_page()` returns the a list of `RuntimeScriptActionDetail` on that page and advances the `RuntimeScriptActionDetailPaged` object to the next page.</span></span> <span data-ttu-id="1053d-187">これは、`StopIteration` 例外が発生し、それ以上ページがないことが示されるまで繰り返されます。</span><span class="sxs-lookup"><span data-stu-id="1053d-187">This can be repeated until a `StopIteration` exception is raised, indicating that there are no more pages.</span></span> <span data-ttu-id="1053d-188">次の例を見てください。</span><span class="sxs-lookup"><span data-stu-id="1053d-188">See the example below.</span></span>

<span data-ttu-id="1053d-189">指定したクラスターに対する保存済みスクリプト アクションを一覧表示するには:</span><span class="sxs-lookup"><span data-stu-id="1053d-189">To list all persisted script actions for the specified cluster:</span></span>
```python
client.script_actions.list_persisted_scripts("<Resource Group Name>", "<Cluster Name>")
```

#### <a name="example"></a><span data-ttu-id="1053d-190">例</span><span class="sxs-lookup"><span data-stu-id="1053d-190">Example</span></span>

```python
scripts_paged = client.script_actions.list_persisted_scripts(resource_group_name, cluster_name)
while True:
  try:
    for script in scripts_paged.advance_page():
      print(script)
  except StopIteration:
    break
```

### <a name="list-all-scripts-execution-history"></a><span data-ttu-id="1053d-191">スクリプトの全実行履歴の一覧表示</span><span class="sxs-lookup"><span data-stu-id="1053d-191">List All Scripts' Execution History</span></span>

<span data-ttu-id="1053d-192">指定したクラスターに対するスクリプトの実行履歴をすべて一覧表示するには:</span><span class="sxs-lookup"><span data-stu-id="1053d-192">To list all scripts' execution history for the specified cluster:</span></span>

```python
client.script_execution_history.list("<Resource Group Name>", "<Cluster Name>")
```

#### <a name="example"></a><span data-ttu-id="1053d-193">例</span><span class="sxs-lookup"><span data-stu-id="1053d-193">Example</span></span>

<span data-ttu-id="1053d-194">この例では、過去のすべてのスクリプト実行の詳細が出力されます。</span><span class="sxs-lookup"><span data-stu-id="1053d-194">This example prints all the details for all past script executions.</span></span>

```python
script_executions_paged = client.script_execution_history.list("<Resource Group Name>", "<Cluster Name>")
while True:
  try:
    for script in script_executions_paged.advance_page():            
      print(script)
    except StopIteration:       
      break
```