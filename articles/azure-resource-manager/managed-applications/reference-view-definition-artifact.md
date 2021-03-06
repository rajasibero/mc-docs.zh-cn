---
title: 视图定义项目参考
description: 提供 Azure 托管应用程序的视图定义项目示例。 文件名为 viewDefinition.json。
ms.topic: conceptual
author: rockboyfor
origin.date: 07/11/2019
ms.date: 01/20/2020
ms.author: v-yeche
ms.openlocfilehash: 0c97953f67fe718866c289b938c4f1bc5a9f03e3
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "78209153"
---
# <a name="reference-view-definition-artifact"></a>参考：视图定义项目

本文是 Azure 托管应用程序中 viewDefinition.json  项目的参考。 有关创作视图配置的详细信息，请参阅[视图定义项目](concepts-view-definition.md)。

## <a name="view-definition"></a>查看定义

以下 JSON 演示了 Azure 托管应用程序的 viewDefinition.json  文件的示例：

```json
{
  "views": [{
    "kind": "Overview",
    "properties": {
      "header": "Welcome to your Demo Azure Managed Application",
      "description": "This Managed application with Custom Provider is for demo purposes only.",
      "commands": [{
          "displayName": "Ping Action",
          "path": "/customping",
          "icon": "LaunchCurrent"
      }]
    }
  },
  {
    "kind": "CustomResources",
    "properties": {
      "displayName": "Users",
      "version": "1.0.0.0",
      "resourceType": "users",
      "createUIDefinition": {
        "parameters": {
          "steps": [{
            "name": "add",
            "label": "Add user",
            "elements": [{
              "name": "name",
              "label": "User's Full Name",
              "type": "Microsoft.Common.TextBox",
              "defaultValue": "",
              "toolTip": "Provide a full user name.",
              "constraints": { "required": true }
            },
            {
              "name": "location",
              "label": "User's Location",
              "type": "Microsoft.Common.TextBox",
              "defaultValue": "",
              "toolTip": "Provide a Location.",
              "constraints": { "required": true }
            }]
          }],
          "outputs": {
            "name": "[steps('add').name]",
            "properties": {
              "FullName": "[steps('add').name]",
              "Location": "[steps('add').location]"
            }
          }
        }
      },
      "commands": [{
        "displayName": "Custom Context Action",
        "path": "users/contextAction",
        "icon": "Start"
      }],
      "columns": [
        { "key": "properties.FullName", "displayName": "Full Name" },
        { "key": "properties.Location", "displayName": "Location", "optional": true }
      ]
    }
  }]
}
```

## <a name="next-steps"></a>后续步骤

<!--Not Available on - [Tutorial: Create managed application with custom actions and resources](tutorial-create-managed-app-with-custom-provider.md)-->

- [参考：用户界面元素项目](reference-createuidefinition-artifact.md)
- [参考：部署模板项目](reference-main-template-artifact.md)

<!-- Update_Description: new article about reference view definition artifact -->
<!--NEW.date: 01/20/2020-->