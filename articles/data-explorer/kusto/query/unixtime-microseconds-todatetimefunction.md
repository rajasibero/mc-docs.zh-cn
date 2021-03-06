---
title: unixtime_microseconds_todatetime() - Azure 数据资源管理器
description: 本文介绍 Azure 数据资源管理器中的 unixtime_microseconds_todatetime()。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
origin.date: 11/27/2019
ms.date: 08/06/2020
ms.openlocfilehash: 69d794fcfb84ebaa851ed12d69606877f0d637fc
ms.sourcegitcommit: 7ceeca89c0f0057610d998b64c000a2bb0a57285
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/06/2020
ms.locfileid: "87841473"
---
# <a name="unixtime_microseconds_todatetime"></a>unixtime_microseconds_todatetime()

将 unix-epoch 微秒转换为 UTC 日期/时间。

**语法**

`unixtime_microseconds_todatetime(*microseconds*)`

**参数**

* microseconds：一个实数，表示以微秒为单位的纪元时间戳。 在纪元时间 (1970-01-01 00:00:00) 之前发生的 `Datetime` 时间戳值为负。

**返回**

如果转换成功，则结果将为[日期/时间](./scalar-data-types/datetime.md)值。 如果转换不成功，则结果将为 null。

**另请参阅**

* 使用 [unixtime_seconds_todatetime()](unixtime-seconds-todatetimefunction.md) 将 unix-epoch 秒转换为 UTC 日期/时间。
* 使用 [unixtime_milliseconds_todatetime()](unixtime-milliseconds-todatetimefunction.md) 将 unix-epoch 毫秒转换为 UTC 日期/时间。
* 使用 [unixtime_nanoseconds_todatetime()](unixtime-nanoseconds-todatetimefunction.md) 将 unix-epoch 纳秒转换为 UTC 日期/时间。

**示例**

<!-- csl: https://help.kusto.chinacloudapi.cn/Samples  -->
```kusto
print date_time = unixtime_microseconds_todatetime(1546300800000000)
```

|date_time|
|---|
|2019-01-01 00:00:00.0000000|
