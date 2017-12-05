---
title: "ロジック アプリ ワークフローを作成する"
description: "ロジック アプリ ワークフローを作成する"
author: lisawong19
manager: douge
ms.assetid: 
ms.devlang: python
ms.topic: article
ms.service: Azure
ms.technology: Azure
ms.date: 6/15/2017
ms.author: liwong
ms.openlocfilehash: 732791b2ac5689f5421d2449410450aedb7591bc
ms.sourcegitcommit: 3617d0db0111bbc00072ff8161de2d76606ce0ea
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/18/2017
---
# <a name="create-a-logic-app-workflow"></a><span data-ttu-id="dfb07-103">ロジック アプリ ワークフローを作成する</span><span class="sxs-lookup"><span data-stu-id="dfb07-103">Create a Logic App Workflow</span></span>

<span data-ttu-id="dfb07-104">次のコードでは、ロジック アプリ ワークフローを作成します。</span><span class="sxs-lookup"><span data-stu-id="dfb07-104">The following code creates a logic app workflow.</span></span>

```python
from azure.mgmt.logic.models import Workflow

group_name = 'myresourcegroup'
workflow_name = '12HourHeartBeat'
logic_client.workflows.create_or_update(
    group_name,
    workflow_name,
    Workflow(
        location = 'East US',
        definition={
            "$schema": "https://schema.management.azure.com/providers/Microsoft.Logic/schemas/2016-06-01/workflowdefinition.json#",
            "contentVersion": "1.0.0.0",
            "parameters": {},
            "triggers": {},
            "actions": {},
            "outputs": {}
        }
    )
)
```
