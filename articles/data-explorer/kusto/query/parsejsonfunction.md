---
title: parse_json() - Azure 数据资源管理器
description: 本文介绍 Azure 数据资源管理器中的 parse_json()。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
origin.date: 02/13/2020
ms.date: 08/06/2020
ms.openlocfilehash: 1261c168181aee20dbe6c26a99ed7cf8f4fd9efc
ms.sourcegitcommit: 7ceeca89c0f0057610d998b64c000a2bb0a57285
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/06/2020
ms.locfileid: "87841349"
---
# <a name="parse_json"></a>parse_json()

将 `string` 解释为 JSON 值并以 `dynamic` 形式返回值。

当需要提取 JSON 复合对象的多个元素时，使用此函数比使用 [extractjson() function](./extractjsonfunction.md) 函数更好。

**语法**

`parse_json(`*json*`)`

别名：
- [todynamic()](./todynamicfunction.md)
- [toobject()](./todynamicfunction.md)

**参数**

* json：`string` 类型的表达式。 它表示 [JSON 格式的值](https://json.org/)或 [dynamic](./scalar-data-types/dynamic.md) 类型的表达式（表示实际的 `dynamic` 值）。

**返回**

`dynamic` 类型的对象，该对象由 json 的值确定：
* 如果 json 的类型为 `dynamic`，则其值将按原样使用。
* 如果 json 的类型为 `string`，并且是[格式正确的 JSON 字符串](https://json.org/)，则系统会分析字符串并返回生成的值。
* 如果 json 的类型为 `string`，但不是[格式正确的 JSON 字符串](https://json.org/)，则返回的值是包含原始 `string` 值的类型为 `dynamic` 的对象。

**示例**

在以下示例中，如果 `context_custom_metrics` 是类似如下的 `string`：

```json
{"duration":{"value":118.0,"count":5.0,"min":100.0,"max":150.0,"stdDev":0.0,"sampledValue":118.0,"sum":118.0}}
```

则以下 CSL 片段会检索对象中 `duration` 槽的值，并从中检索两个槽：`duration.value` 和 `duration.min`（分别为 `118.0` 和 `110.0`）。

```kusto
T
| extend d=parse_json(context_custom_metrics) 
| extend duration_value=d.duration.value, duration_min=d["duration"]["min"]
```

**备注**

通常用一个 JSON 字符串来描述属性包，其中的一个“槽”是另一个 JSON 字符串。 

例如：

```kusto
let d='{"a":123, "b":"{\\"c\\":456}"}';
print d
```

在这种情况下，不仅需要调用 `parse_json` 两次，而且还需要确保在第二次调用中使用 `tostring`。 否则，对 `parse_json` 的第二次调用会只按原样将输入传递到输出，因为它的已声明类型为 `dynamic`。

```kusto
let d='{"a":123, "b":"{\\"c\\":456}"}';
print d_b_c=parse_json(tostring(parse_json(d).b)).c
```
