---
title: endofmonth() - Azure 数据资源管理器 | Microsoft Docs
description: 本文介绍 Azure 数据资源管理器中的 endofmonth()。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
origin.date: 02/13/2020
ms.date: 08/06/2020
ms.openlocfilehash: 903147339b4281b5068dbdddd8ad1ff5ec30cdcd
ms.sourcegitcommit: 7ceeca89c0f0057610d998b64c000a2bb0a57285
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/06/2020
ms.locfileid: "87841276"
---
# <a name="endofmonth"></a>endofmonth()

返回包含日期的一月的终点，根据偏移量移动（如提供）。

**语法**

`endofmonth(`*date* [`,`*offset*]`)`

**参数**

* `date`：输入日期。
* `offset`：输入日期中的可选偏移月数（整数，默认值为 0）。

**返回**

一个日期/时间，表示给定 date 值的那一月的结束（如果指定了偏移量，则还包含该信息）。

**示例**

```kusto
  range offset from -1 to 1 step 1
 | project monthEnd = endofmonth(datetime(2017-01-01 10:10:17), offset) 
```

|monthEnd|
|---|
|2016-12-31 23:59:59.9999999|
|2017-01-31 23:59:59.9999999|
|2017-02-28 23:59:59.9999999|