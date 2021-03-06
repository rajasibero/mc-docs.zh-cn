---
title: 使用 Azure Policy 实现合规性
description: 分配 Azure Policy 中的内置策略来审核 Azure 容器注册表的合规性
ms.topic: article
origin.date: 06/11/2020
author: rockboyfor
ms.date: 10/05/2020
ms.testscope: no
ms.testdate: 06/08/2020
ms.author: v-yeche
ms.openlocfilehash: eb193beb9b1c0fb37218d5767392383db9159c32
ms.sourcegitcommit: 29a49e95f72f97790431104e837b114912c318b4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/30/2020
ms.locfileid: "91564405"
---
<!--Verified successfully-->
# <a name="audit-compliance-of-azure-container-registries-using-azure-policy"></a>使用 Azure Policy 审核 Azure 容器注册表的合规性

[Azure Policy](../governance/policy/overview.md) 是 Azure 中的一项服务，用于创建、分配和管理策略。 这些策略将在整个资源中强制实施不同的规则和效果，使这些资源符合公司标准和服务级别协议。

本文介绍适用于 Azure 容器注册表的内置策略。 可以使用这些策略来审核新的和现有的注册表的合规性。

可以免费使用 Azure Policy。

## <a name="built-in-policy-definitions"></a>内置策略定义

以下内置策略定义特定于 Azure 容器注册表：

[!INCLUDE [azure-policy-reference-policies-container-registry](../../includes/policy/reference/bycat/policies-container-registry.md)]

另请参阅内置网络策略定义：[容器注册表应使用虚拟网络服务终结点](https://portal.azure.cn/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2Fc4857be7-912a-4c75-87e6-e30292bcdf78)。

## <a name="assign-policies"></a>分配策略

* 使用 [Azure 门户](../governance/policy/assign-policy-portal.md)、[Azure CLI](../governance/policy/assign-policy-azurecli.md)、[资源管理器模板](../governance/policy/assign-policy-template.md)或 Azure Policy SDK 来分配策略。
* 将策略分配的作用域限定为资源组、订阅或 [Azure 管理组](../governance/management-groups/overview.md)。 容器注册表策略分配适用于该作用域内现有的和新的容器注册表。
* 可以随时启用或禁用[策略实施](../governance/policy/concepts/assignment-structure.md#enforcement-mode)功能。

> [!NOTE]
> 在分配或更新某个策略后，需要花费一些时间才会将分配应用到所定义作用域中的资源。 请查看有关[策略评估触发器](../governance/policy/how-to/get-compliance-data.md#evaluation-triggers)的信息。

## <a name="review-policy-compliance"></a>查看策略合规性

可以使用 Azure 门户、Azure 命令行工具或 Azure Policy SDK 来访问策略分配生成的合规性信息。 有关详细信息，请参阅[获取 Azure 资源的合规性数据](../governance/policy/how-to/get-compliance-data.md)。

有多种可能的原因会导致资源不合规。 若要确定原因或查找导致问题的变更，请参阅[确定不合规情况](../governance/policy/how-to/determine-non-compliance.md)。

### <a name="policy-compliance-in-the-portal"></a>门户中的策略合规性：

1. 选择“所有服务”  ，然后搜索“策略”。
1. 选择“合规性”。
1. 将筛选器用于限制合规性状态，或用于搜索策略。

    :::image type="content" source="./media/container-registry-azure-policy/azure-policy-compliance.png" alt-text="门户中的策略合规性":::

1. 选择一个策略来查看聚合合规性详细信息和事件。 然后，根据需要选择一个适用于资源合规性的特定注册表。

### <a name="policy-compliance-in-the-azure-cli"></a>Azure CLI 中的策略合规性

也可使用 Azure CLI 来获取合规性数据。 例如，在 CLI 中使用 [az policy assignment list](https://docs.azure.cn/cli/policy/assignment#az-policy-assignment-list) 命令获取已应用的 Azure 容器注册表策略的策略 ID：

```azurecli
az policy assignment list --query "[?contains(displayName,'Container Registries')].{name:displayName, ID:id}" --output table
```

示例输出：

```
Name                                                                                   ID
------------------------------------------------------------------------------------- --------------------------------------------------------------------------------------------------------------------------------
Container Registries should not allow unrestricted network access           /subscriptions/<subscriptionID>/providers/Microsoft.Authorization/policyAssignments/b4faf132dc344b84ba68a441
Container Registries should be encrypted with a Customer-Managed Key (CMK)  /subscriptions/<subscriptionID>/providers/Microsoft.Authorization/policyAssignments/cce1ed4f38a147ad994ab60a
```

然后运行 [az policy state list](https://docs.azure.cn/cli/policy/state#az-policy-state-list)，以便返回特定策略 ID 下所有资源的 JSON 格式合规性状态：

```azurecli
az policy state list \
  --resource <policyID>
```

也可运行 [az policy state list](https://docs.azure.cn/cli/policy/state#az-policy-state-list)，以便返回特定注册表资源（例如 myregistry）的 JSON 格式合规性状态：

```azurecli
az policy state list \
 --resource myregistry \
 --namespace Microsoft.ContainerRegistry \
 --resource-type registries \
 --resource-group myresourcegroup
```

## <a name="next-steps"></a>后续步骤

* 详细了解 Azure Policy [定义](../governance/policy/concepts/definition-structure.md)和[效果](../governance/policy/concepts/effects.md)。

* 创建[自定义策略定义](../governance/policy/tutorials/create-custom-policy-definition.md)。

* 详细了解 Azure 中的[治理功能](../governance/index.yml)。

<!-- Update_Description: update meta properties, wording update, update link -->