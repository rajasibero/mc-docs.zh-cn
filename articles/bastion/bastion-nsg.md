---
title: 在 Azure Bastion 中使用 VM 和 NSG
description: 可以将网络安全组与 Azure Bastion 配合使用。 了解此配置所需的子网。
services: bastion
ms.service: bastion
ms.topic: conceptual
origin.date: 07/07/2020
author: rockboyfor
ms.date: 09/21/2020
ms.testscope: no
ms.testdate: ''
ms.author: v-yeche
ms.openlocfilehash: 06242589ad90ac372f667a123c7cee09d7bcad6e
ms.sourcegitcommit: f3fee8e6a52e3d8a5bd3cf240410ddc8c09abac9
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91146461"
---
<!--Verified succesfully for only charactors only-->
# <a name="working-with-nsg-access-and-azure-bastion"></a>使用 NSG 访问和 Azure Bastion

使用 Azure Bastion 时，可以使用网络安全组 (NSG)。 有关详细信息，请参阅[安全组](../virtual-network/security-overview.md)。

:::image type="content" source="./media/bastion-nsg/figure-1.png" alt-text="NSG":::

在此图中：

* Bastion 主机部署到虚拟网络中。
* 用户使用任何 HTML5 浏览器连接到 Azure 门户。
* 用户导航到虚拟机，以便进行 RDP/SSH 操作。
* 连接集成 - 在浏览器中单击“RDP/SSH 会话”
* Azure VM 无需公共 IP。

<a name="nsg"></a>
## <a name="network-security-groups"></a>网络安全组

本部分介绍用户与 Azure Bastion 之间的网络流量，以及到虚拟网络中的目标 VM 的网络流量：

<a name="apply"></a>
### <a name="azurebastionsubnet"></a>AzureBastionSubnet

Azure Bastion 将专门部署到 ***AzureBastionSubnet***。

* **入口流量：**

    * **来自公共 Internet 的入口流量：** Azure Bastion 将创建一个公共 IP，需要在该公共 IP 上启用端口 443，用于入口流量。 不需要在 AzureBastionSubnet 上打开端口 3389/22。
    * **来自 Azure Bastion 控制平面的入口流量：** 对于控制平面连接，请启用从 GatewayManager 服务标记进行的端口 443 入站。 这使控制平面（即网关管理器）能够与 Azure Bastion 通信。

    :::image type="content" source="./media/bastion-nsg/inbound.png" alt-text="入站":::

* **出口流量：**

    * **流向目标 VM 的出口流量：** Azure Bastion 将通过专用 IP 到达目标 VM。 NSG 需要允许端口 3389 和 22 的出口流量流向其他目标 VM 子网。
    * **流向 Azure 中其他公共终结点的出口流量：** Azure Bastion 需要能够连接到 Azure 中的各种公共终结点，以便执行相应操作（例如，存储诊断日志和计量日志）。 因此，Azure Bastion 需要出站到 443，再到 AzureCloud 服务标记。

    :::image type="content" source="./media/bastion-nsg/outbound.png" alt-text="Outbound":::

### <a name="target-vm-subnet"></a>目标 VM 子网
此子网包含要通过 RDP/SSH 连接到的目标虚拟机。

* **来自 Azure Bastion 的入口流量：** Azure Bastion 将通过专用 IP 到达目标 VM。 需要在目标 VM 端通过专用 IP 打开 RDP/SSH 端口（即端口 3389/22）。 最佳做法是，可以在此规则中添加 Azure Bastion 子网 IP 地址范围，仅允许 Bastion 在目标 VM 子网中的目标 VM 上打开这些端口。

## <a name="next-steps"></a>后续步骤

有关 Azure Bastion 的详细信息，请参阅[常见问题解答](bastion-faq.md)。

<!-- Update_Description: update meta properties, wording update, update link -->