---
title: 添加事件中心事件源 - Azure 时序见解 | Microsoft Docs
description: 了解如何将 Azure 事件中心事件源添加到 Azure 时序见解环境。
ms.service: time-series-insights
services: time-series-insights
author: deepakpalled
ms.author: v-junlch
manager: diviso
ms.reviewer: v-mamcge, jasonh, kfile
ms.workload: big-data
ms.topic: conceptual
ms.date: 08/04/2020
ms.custom: seodec18
ms.openlocfilehash: 357025d8a9b2151829c79a0d0435d4301e7685fa
ms.sourcegitcommit: 25d542cf9c8c7bee51ec75a25e5077e867a9eb8b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/09/2020
ms.locfileid: "89593850"
---
# <a name="add-an-event-hub-event-source-to-your-azure-time-series-insights-environment"></a>将事件中心事件源添加到 Azure 时序见解环境

本文介绍如何使用 Azure 门户将从 Azure 事件中心读取数据的事件源添加到 Azure 时序见解环境。

> [!NOTE]
> 本文中介绍的步骤对于 Azure 时序见解第 1 代和 Azure 时序见解第 2 代环境都适用。

## <a name="prerequisites"></a>先决条件

- 按照[创建 Azure 时序见解环境](./tutorials-set-up-tsi-environment.md)中的说明创建一个 Azure 时序见解环境。
- 创建事件中心。 请阅读[使用 Azure 门户创建事件中心命名空间和事件中心](../event-hubs/event-hubs-create.md)。
- 事件中心必须有发送进来的活动消息事件。 了解如何[使用 .NET Framework 将事件发送到 Azure 事件中心](../event-hubs/event-hubs-dotnet-framework-getstarted-send.md)。
- 在事件中心创建专用使用者组，以供 Azure 时序见解环境使用。 每个 Azure 时序见解事件源都必须具有自己的专用使用者组，该组不与其他使用者共享。 如果多个读取器使用同一使用者组中的事件，则所有读取器都可能出现故障。 有一项限制，即每个事件中心只能有 20 个使用者组。 有关详细信息，请阅读[事件中心编程指南](../event-hubs/event-hubs-programming-guide.md)。

### <a name="add-a-consumer-group-to-your-event-hub"></a>将使用者组添加到事件中心

应用程序使用使用者组从 Azure 事件中心提取数据。 若要可靠地从事件中心读取数据，请提供一个仅供此 Azure 时序见解环境使用的专用使用者组。

若要将新使用者组添加到事件中心，请执行以下操作：

1. 在 [Azure 门户](https://portal.azure.cn)中，从事件中心命名空间的“概述”窗格中找到并打开事件中心实例。 选择“实体”>“事件中心”，或在“名称”下查找实例 。

    [![打开事件中心命名空间](./media/time-series-insights-how-to-add-an-event-source-eventhub/tsi-connect-event-hub-namespace.png)](./media/time-series-insights-how-to-add-an-event-source-eventhub/tsi-connect-event-hub-namespace.png#lightbox)

1. 在事件中心实例中，选择“实体”>“使用者组”。 然后，选择“+ 使用者组”，以添加新的使用者组。 

   [![事件中心 - 添加使用者组](./media/time-series-insights-how-to-add-an-event-source-eventhub/add-event-hub-consumer-group.png)](./media/time-series-insights-how-to-add-an-event-source-eventhub/add-event-hub-consumer-group.png#lightbox)

   否则，请选择现有的使用者组并跳到下一部分。

1. 在“使用者组”页上，输入一个新的唯一值作为**名称******。  创建新的事件源时，请在 Azure 时序见解环境中使用此相同名称。

1. 选择“创建”。

## <a name="add-a-new-event-source"></a>添加新的事件源

1. 登录到 [Azure 门户](https://portal.azure.cn)。

1. 查找现有的 Azure 时序见解环境。 在左侧菜单中选择“所有资源”，然后选择 Azure 时序见解环境。

1. 选择“事件源”，然后选择“添加”按钮 。

   [![在“事件源”下选择“添加”按钮](./media/time-series-insights-how-to-add-an-event-source-eventhub/tsi-add-an-event-source.png)](./media/time-series-insights-how-to-add-an-event-source-eventhub/tsi-add-an-event-source.png#lightbox)

1. 输入一个值作为特定于此 Azure 时序见解环境的“事件源名称”，如 `Contoso-TSI-Gen 1-Event-Hub-ES`。

1. 对于“源”，选择“事件中心”********。

1. 选择适当的值作为“导入”选项****：

   * 如果在其中一个订阅中有现有的事件中心，请选择“从可用订阅使用事件中心”****。 此选项是最简单的方法。

     [![选择“事件源导入”选项](./media/time-series-insights-how-to-add-an-event-source-eventhub/tsi-event-hub-select-import-option.png)](./media/time-series-insights-how-to-add-an-event-source-eventhub/tsi-event-hub-select-import-option.png#lightbox)

    *  下表介绍的属性是“通过可用订阅使用事件中心”**** 选项所需的：

       [![订阅和事件中心详细信息](./media/time-series-insights-how-to-add-an-event-source-eventhub/tsi-configure-create-confirm.png)](./media/time-series-insights-how-to-add-an-event-source-eventhub/tsi-configure-create-confirm.png#lightbox)

       | 属性 | 说明 |
       | --- | --- |
       | 订阅 | 所需的事件中心实例和命名空间所属的订阅。 |
       | 事件中心命名空间 | 所需的事件中心实例所属的事件中心命名空间。 |
       | 事件中心名称 | 所需的事件中心实例的名称。 |
       | 事件中心策略值 | 选择所需的共享访问策略。 可以在事件中心的“配置”选项卡上创建共享访问策略。每个共享访问策略具有名称、所设权限以及访问密钥。 事件源的共享访问策略必须** 具有“读取”**** 权限。 |
       | 事件中心策略密钥 | 从所选的事件中心策略值预填充。 |

    * 如果事件中心在订阅外部，或者你希望选择高级选项，请选择“手动提供事件中心设置”****。

       下表介绍“手动提供事件中心设置”选项**** 所需的属性：
 
       | 属性 | 描述 |
       | --- | --- |
       | 订阅 ID | 所需的事件中心实例和命名空间所属的订阅。 |
       | 资源组 | 所需的事件中心实例和命名空间所属的资源组。 |
       | 事件中心命名空间 | 所需的事件中心实例所属的事件中心命名空间。 |
       | 事件中心名称 | 所需的事件中心实例的名称。 |
       | 事件中心策略值 | 选择所需的共享访问策略。 可以在事件中心的“配置”选项卡上创建共享访问策略。每个共享访问策略具有名称、所设权限以及访问密钥。 事件源的共享访问策略必须** 具有“读取”**** 权限。 |
       | 事件中心策略密钥 | 用于对服务总线命名空间的访问权限进行身份验证的共享访问密钥。 在此处输入主密钥或辅助密钥。 |

    * 这两个选项共享以下配置选项：

       | 属性 | 描述 |
       | --- | --- |
       | 事件中心使用者组 | 从事件中心读取事件的使用者组。 强烈建议为事件源使用专用的使用者组。 |
       | 事件序列化格式 | 目前，JSON 是唯一可用的序列化格式。 事件消息必须采用此格式，否则将无法读取任何数据。 |
       | 时间戳属性名称 | 若要确定此值，需要了解发送到事件中心的消息数据的消息格式。 此值是**** 消息数据中你想要用作事件时间戳的特定事件属性的“名称”。 该值区分大小写。 如果留空，则事件源中的“事件排队时间”**** 将用作事件时间戳。 |

1. 添加已添加到事件中心的专用 Azure 时序见解使用者组名称。

1. 选择“创建”。

   创建事件源以后，Azure 时序见解就会自动开始将数据流式传输到环境中。

## <a name="next-steps"></a>后续步骤

* [定义数据访问策略](time-series-insights-data-access.md)，以保护数据。

* [发送事件](time-series-insights-send-events.md)到事件源。

* 在 [Azure 时序见解资源管理器](https://insights.timeseries.azure.cn)中访问环境。

