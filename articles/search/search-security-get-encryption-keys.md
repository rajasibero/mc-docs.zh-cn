---
title: 获取加密密钥信息
titleSuffix: Azure Cognitive Search
description: 检索索引或同义词映射中使用的加密密钥名称和版本，以便可以在 Azure Key Vault 中管理密钥。
manager: nitinme
author: HeidiSteen
ms.author: v-tawe
ms.service: cognitive-search
ms.topic: conceptual
origin.date: 08/01/2020
ms.date: 09/10/2020
ms.openlocfilehash: b44c457148ab9330e3286162b28b67bc1a24d21c
ms.sourcegitcommit: 78c71698daffee3a6b316e794f5bdcf6d160f326
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/11/2020
ms.locfileid: "90021699"
---
# <a name="get-customer-managed-key-information-from-indexes-and-synonym-maps"></a>从索引和同义词映射获取客户管理的密钥信息

在 Azure 认知搜索中，需在 Azure Key Vault 中创建、存储和管理客户管理的加密密钥。 如果需要确定对象是否加密，或者要确定使用的密钥名称或版本，请使用 REST API 或 SDK 从索引或同义词映射定义中检索 encryptionKey 属性。 

建议在 Key Vault 上[启用日志记录](../key-vault/general/logging.md)，以便监视密钥使用情况。

## <a name="get-the-admin-api-key"></a>获取管理 API 密钥

若要从搜索服务获取对象定义，你需要使用管理员权限进行身份验证。 获取管理 API 密钥的最简单的方法是通过门户获取。

1. 登录到 [Azure 门户](https://portal.azure.cn/)，然后打开搜索服务概览页面。

1. 在左侧，单击“密钥”并复制管理 API。 索引和同义词映射检索需要使用管理密钥。

为完成剩余步骤，请切换到 PowerShell 和 REST API。 门户不显示同义词映射，也不显示索引的加密密钥属性。

## <a name="use-powershell-and-rest"></a>使用 PowerShell 和 REST

运行以下命令以设置变量和获取对象定义。

```powershell
<# Connect to Azure #>
$Connect-AzAccount

<# Provide the admin API key used for search service authentication  #>
$headers = @{
'api-key' = '<YOUR-ADMIN-API-KEY>'
'Content-Type' = 'application/json'
'Accept' = 'application/json' }

<# List all existing synonym maps #>
$uri= 'https://<YOUR-SEARCH-SERVICE>.search.azure.cn/synonyms?api-version=2020-06-30&$select=name'
Invoke-RestMethod -Uri $uri -Headers $headers | ConvertTo-Json

<# List all existing indexes #>
$uri= 'https://<YOUR-SEARCH-SERVICE>.search.azure.cn/indexes?api-version=2020-06-30&$select=name'
Invoke-RestMethod -Uri $uri -Headers $headers | ConvertTo-Json

<# Return a specific synonym map definition. The encryptionKey property is at the end #>
$uri= 'https://<YOUR-SEARCH-SERVICE>.search.azure.cn/synonyms/<YOUR-SYNONYM-MAP-NAME>?api-version=2020-06-30'
Invoke-RestMethod -Uri $uri -Headers $headers | ConvertTo-Json

<# Return a specific index definition. The encryptionKey property is at the end #>
$uri= 'https://<YOUR-SEARCH-SERVICE>.search.azure.cn/indexes/<YOUR-INDEX-NAME>?api-version=2020-06-30'
Invoke-RestMethod -Uri $uri -Headers $headers | ConvertTo-Json
```

## <a name="next-steps"></a>后续步骤

现在你了解了所使用的加密密钥和版本，可以在 Azure Key Vault 中管理密钥，或查看其他配置设置。

+ [快速入门：使用 PowerShell 在 Azure Key Vault 中设置和检索机密](../key-vault/secrets/quick-create-powershell.md)

+ [在 Azure 认知服务中配置客户管理的密钥以进行数据加密](search-security-manage-encryption-keys.md)