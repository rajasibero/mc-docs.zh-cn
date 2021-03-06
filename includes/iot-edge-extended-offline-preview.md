---
title: include 文件
description: IoT Edge
author: kgremban
manager: timlt
ms.reviewer: veyalla
ms.service: iot-edge
services: iot-edge
ms.topic: conceptual
ms.date: 09/18/2018
ms.author: kgremban
ms.openlocfilehash: 046a54630a70814ed1e7f6f621b4d73c645720a9
ms.sourcegitcommit: 59db70ef3ed61538666fd1071dcf8d03864f10a9
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/30/2018
ms.locfileid: "52676046"
---
## <a name="enabling-extended-offline-operation-preview"></a>启用扩展的脱机操作（预览）
使用 Edge 运行时的 [v1.0.4 版本](https://github.com/Azure/azure-iotedge/releases/tag/1.0.4)，可配置 Edge 设备和与之连接的下游设备，以处理扩展脱机操作。 

借助此功能，本地模块或下游设备可根据需要向 Edge 设备重新进行身份验证，即使从 IoT 中心断开连接也可使用消息和方法相互进行通信。 有关详细信息和此功能的范围，请参阅此[博客文章](https://aka.ms/iot-edge-offline)和[概念文章](../articles/iot-edge/offline-capabilities.md)。

在网关中启用扩展脱机方案，可在边缘设备和将与之连接的下游设备之间建立父子节点关系。

1. 从 IoT 中心门户的“边缘设备详细信息”选项卡，单击顶部命令栏中的“管理子设备(预览)”按钮。

1. 单击“+ 添加”按钮。

1. 从设备列表中，选择子设备并使用向右键选择要作为子级添加的设备。

1. 单击“确定”以确认。

现在已启用边缘设备及其子设备处理扩展脱机操作。  