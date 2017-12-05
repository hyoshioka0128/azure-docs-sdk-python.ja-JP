---
title: "インストール"
description: "Azure Python SDK のインストール方法"
keywords: Azure, Python, SDK, API
author: lisawong19
ms.author: liwong
manager: douge
ms.date: 06/05/2017
ms.topic: install
ms.prod: azure
ms.technology: azure
ms.devlang: python
ms.service: multiple
ms.assetid: 
ms.openlocfilehash: 1ee0c110673f908832c1c9e8fd6dac4c90c16e8e
ms.sourcegitcommit: ce2921d9a6f21d58fdf77cbc843c9b4af0ea796d
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/31/2017
---
# <a name="installation"></a><span data-ttu-id="bcb23-104">インストール</span><span class="sxs-lookup"><span data-stu-id="bcb23-104">Installation</span></span>

## <a name="installation-with-pip"></a><span data-ttu-id="bcb23-105">pip によるインストール</span><span class="sxs-lookup"><span data-stu-id="bcb23-105">Installation with pip</span></span>

<span data-ttu-id="bcb23-106">各 Azure サービスのライブラリは、個別にインストールできます。</span><span class="sxs-lookup"><span data-stu-id="bcb23-106">You can install each Azure service's library individually:</span></span>

```bash
pip install azure-batch          # Install the latest Batch runtime library
pip install azure-mgmt-scheduler # Install the latest Storage management library
```

<span data-ttu-id="bcb23-107">プレビュー パッケージをインストールするには、`--pre` フラグを使用します。</span><span class="sxs-lookup"><span data-stu-id="bcb23-107">Preview packages can be installed using the `--pre` flag:</span></span>

```bash
pip install --pre azure-mgmt-compute # will install only the latest Compute Management library
```

<span data-ttu-id="bcb23-108">`azure` メタ パッケージを使用して、一連の Azure ライブラリを 1 行でインストールすることもできます。</span><span class="sxs-lookup"><span data-stu-id="bcb23-108">You can also install a set of Azure libraries in a single line using the `azure` meta-package.</span></span>

```bash
pip install azure
```

<span data-ttu-id="bcb23-109">このパッケージのプレビュー バージョンを公開します。このバージョンには、次のように、--pre フラグを使用してアクセスすることができます。</span><span class="sxs-lookup"><span data-stu-id="bcb23-109">We publish a preview version of this package, which you can access using the --pre flag:</span></span>

```bash
pip install --pre azure
```

## <a name="install-from-github"></a><span data-ttu-id="bcb23-110">GitHub からのインストール</span><span class="sxs-lookup"><span data-stu-id="bcb23-110">Install from GitHub</span></span>

<span data-ttu-id="bcb23-111">`azure` をソースからインストールする場合は、次のようにします。</span><span class="sxs-lookup"><span data-stu-id="bcb23-111">If you want to install `azure` from source:</span></span>

    git clone git://github.com/Azure/azure-sdk-for-python.git
    cd azure-sdk-for-python
    python setup.py install