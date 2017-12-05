---
title: "Python 用 Azure 仮想マシン ライブラリ"
description: 
keywords: "Azure, Python, SDK, API, コンピューティング, 仮想マシン"
author: lisawong19
ms.author: liwong
manager: douge
ms.date: 06/09/2017
ms.topic: article
ms.prod: azure
ms.technology: azure
ms.devlang: python
ms.service: compute
ms.openlocfilehash: c4128dae1c1fd47d2ac34b178b7e1031aa14c948
ms.sourcegitcommit: 1229121faaae8536a7d8cc89cddd24abf1e30cb8
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/15/2017
---
# <a name="azure-virtual-machine-libraries"></a><span data-ttu-id="7f944-103">Azure 仮想マシン ライブラリ</span><span class="sxs-lookup"><span data-stu-id="7f944-103">Azure virtual machine libraries</span></span>

## <a name="overview"></a><span data-ttu-id="7f944-104">概要</span><span class="sxs-lookup"><span data-stu-id="7f944-104">Overview</span></span>

<span data-ttu-id="7f944-105">Linux または Windows を実行するオンデマンドのスケーラブルなコンピューティング リソースです。</span><span class="sxs-lookup"><span data-stu-id="7f944-105">On-demand, scalable computing resources running Linux or Windows.</span></span>

<span data-ttu-id="7f944-106">Azure 仮想マシンの概要については、「[Azure Portal で Linux 仮想マシンを作成する](/azure/virtual-machines/linux/quick-create-portal)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="7f944-106">To get started with Azure Virtual Machines, see [Create a Linux virtual machine with the Azure portal](/azure/virtual-machines/linux/quick-create-portal).</span></span>

## <a name="management-api"></a><span data-ttu-id="7f944-107">Management API</span><span class="sxs-lookup"><span data-stu-id="7f944-107">Management API</span></span>

<span data-ttu-id="7f944-108">Azure の Windows 仮想マシンと Linux 仮想マシンは、コードから Management API を使って作成、構成、管理、スケーリングを行えます。</span><span class="sxs-lookup"><span data-stu-id="7f944-108">Create, configure, manage and scale Windows and Linux virtual machines in Azure from your code with the management API.</span></span>

<span data-ttu-id="7f944-109">pip を使ってライブラリをインストールします。</span><span class="sxs-lookup"><span data-stu-id="7f944-109">Install the library via pip.</span></span>

```bash
pip install azure-mgmt-compute 
```   

### <a name="example"></a><span data-ttu-id="7f944-110">例</span><span class="sxs-lookup"><span data-stu-id="7f944-110">Example</span></span>

<span data-ttu-id="7f944-111">管理対象のサービス ID (MSI) 認証を使って既存の Azure リソース グループに新しい Linux 仮想マシンを作成します。</span><span class="sxs-lookup"><span data-stu-id="7f944-111">Create a new Linux virtual machine in an existing Azure resource group with Managed Service Identity(MSI) authentication.</span></span>

```python
VM_PARAMETERS={
        'location': 'LOCATION',
        'os_profile': {
            'computer_name': 'VM_NAME',
            'admin_username': 'USERNAME',
            'admin_password': 'PASSWORD'
        },
        'hardware_profile': {
            'vm_size': 'Standard_DS1_v2'
        },
        'storage_profile': {
            'image_reference': {
                'publisher': 'Canonical',
                'offer': 'UbuntuServer',
                'sku': '16.04.0-LTS',
                'version': 'latest'
            },
        },
        'network_profile': {
            'network_interfaces': [{
                'id': 'NIC_ID',
            }]
        },
    }

def create_vm()
    compute_client.virtual_machines.create_or_update(
        'RESOURCE_GROUP_NAME', 'VM_NAME', VM_PARAMETERS)
```

> [!div class="nextstepaction"]
> [<span data-ttu-id="7f944-112">Management API を探す</span><span class="sxs-lookup"><span data-stu-id="7f944-112">Explore the Management APIs</span></span>](/python/api/overview/azure/virtualmachines/managementlibrary)

## <a name="samples"></a><span data-ttu-id="7f944-113">サンプル</span><span class="sxs-lookup"><span data-stu-id="7f944-113">Samples</span></span>

* <span data-ttu-id="7f944-114">[仮想マシンを管理する][1]</span><span class="sxs-lookup"><span data-stu-id="7f944-114">[Manage virtual machines][1]</span></span>
* <span data-ttu-id="7f944-115">[管理対象のサービス ID で認証を行う][2]</span><span class="sxs-lookup"><span data-stu-id="7f944-115">[Authenticate with Managed Service Identity][2]</span></span>
* <span data-ttu-id="7f944-116">[管理対象のサービス ID 拡張機能で仮想マシンを作成する][3]</span><span class="sxs-lookup"><span data-stu-id="7f944-116">[Create a virtual machine with Managed Service Identity Extension][3]</span></span>
* <span data-ttu-id="7f944-117">[ロード バランサーを管理する][4]</span><span class="sxs-lookup"><span data-stu-id="7f944-117">[Manage a load balancer][4]</span></span>
* <span data-ttu-id="7f944-118">[管理ディスクを作成して構成する][5]</span><span class="sxs-lookup"><span data-stu-id="7f944-118">[Create and configure managed disks][5]</span></span>
* <span data-ttu-id="7f944-119">[イメージを一覧表示する][6]</span><span class="sxs-lookup"><span data-stu-id="7f944-119">[List images][6]</span></span> 
* <span data-ttu-id="7f944-120">[仮想マシンを監視する][7]</span><span class="sxs-lookup"><span data-stu-id="7f944-120">[Monitor virtual machines][7]</span></span>

<span data-ttu-id="7f944-121">仮想マシン サンプルの[完全な一覧](https://azure.microsoft.com/resources/samples/?platform=python&term=virtual-machines)をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="7f944-121">View the [complete list](https://azure.microsoft.com/resources/samples/?platform=python&term=virtual-machines) of virtual machine samples.</span></span>

[1]: https://azure.microsoft.com/resources/samples/virtual-machines-python-manage/
[2]: https://github.com/Azure-Samples/resource-manager-python-manage-resources-with-msi
[3]: https://github.com/Azure-Samples/compute-python-msi-vm
[4]: https://azure.microsoft.com/resources/samples/network-python-manage-loadbalancer
[5]: ../docs-ref-conceptual/python-sdk-azure-samples-managed-disks.md
[6]: ../docs-ref-conceptual/python-sdk-azure-samples-list-images.md
[7]: ../docs-ref-conceptual/python-sdk-azure-samples-monitor-vms.md