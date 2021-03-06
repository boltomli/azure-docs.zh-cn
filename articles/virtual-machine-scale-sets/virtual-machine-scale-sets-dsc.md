---
title: 将 Desired State Configuration 与虚拟机规模集配合使用
description: 将虚拟机规模集与 Azure Desired State Configuration 扩展配合使用来配置虚拟机。
author: zjalexander
tags: azure-service-management,azure-resource-manager
ms.assetid: c8f047b5-0e6c-4ef3-8a47-f1b284d32942
ms.service: virtual-machine-scale-sets
ms.topic: conceptual
ms.tgt_pltfrm: vm-windows
ms.date: 04/05/2017
ms.author: zachal
ms.openlocfilehash: a93a8a9c27be5a1736a50e6c4c4b830980b7d974
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/28/2020
ms.locfileid: "76278076"
---
# <a name="using-virtual-machine-scale-sets-with-the-azure-dsc-extension"></a>将虚拟机规模集与 Azure DSC 扩展配合使用
[虚拟机规模集](virtual-machine-scale-sets-overview.md)可与 [Azure 期望状态配置 (DSC)](../virtual-machines/windows/extensions-dsc-overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) 扩展处理程序配合使用。 虚拟机规模集提供部署和管理大量虚拟机的方法，并且可根据负载情况实现弹性横向缩减和扩展。 VM 联机时，DSC 用于配置 VM，使它们能够运行生产软件。

## <a name="differences-between-deploying-to-virtual-machines-and-virtual-machine-scale-sets"></a>部署到虚拟机和部署到虚拟机规模集之间的区别
虚拟机规模集的基础模板结构与单一 VM 略有不同。 具体而言，单一 VM 是在“virtualMachines”节点下扩展部署。 其中有一个 "type": "extensions" 条目，用于将 DSC 添加到模板中

```
"resources": [
          {
              "name": "Microsoft.Powershell.DSC",
              "type": "extensions",
              "location": "[resourceGroup().location]",
              "apiVersion": "2015-06-15",
              "dependsOn": [
                  "[concat('Microsoft.Compute/virtualMachines/', variables('vmName'))]"
              ],
              "tags": {
                  "displayName": "dscExtension"
              },
              "properties": {
                  "publisher": "Microsoft.Powershell",
                  "type": "DSC",
                  "typeHandlerVersion": "2.20",
                  "autoUpgradeMinorVersion": false,
                  "forceUpdateTag": "[parameters('dscExtensionUpdateTagVersion')]",
                  "settings": {
                      "configuration": {
                          "url": "[concat(parameters('_artifactsLocation'), '/', variables('dscExtensionArchiveFolder'), '/', variables('dscExtensionArchiveFileName'))]",
                          "script": "DscExtension.ps1",
                          "function": "Main"
                      },
                      "configurationArguments": {
                          "nodeName": "[variables('vmName')]"
                      }
                  },
                  "protectedSettings": {
                      "configurationUrlSasToken": "[parameters('_artifactsLocationSasToken')]"
                  }
              }
          }
      ]
```

虚拟机规模集节点具有“properties”节，其中包含“VirtualMachineProfile”和“extensionProfile”属性。 DSC 添加在“extensions”下面

```
"extensionProfile": {
            "extensions": [
                {
                    "name": "Microsoft.Powershell.DSC",
                    "properties": {
                        "publisher": "Microsoft.Powershell",
                        "type": "DSC",
                        "typeHandlerVersion": "2.20",
                        "autoUpgradeMinorVersion": false,
                        "forceUpdateTag": "[parameters('DscExtensionUpdateTagVersion')]",
                        "settings": {
                            "configuration": {
                                "url": "[concat(parameters('_artifactsLocation'), '/', variables('DscExtensionArchiveFolder'), '/', variables('DscExtensionArchiveFileName'))]",
                                "script": "DscExtension.ps1",
                                "function": "Main"
                            },
                            "configurationArguments": {
                                "nodeName": "localhost"
                            }
                        },
                        "protectedSettings": {
                            "configurationUrlSasToken": "[parameters('_artifactsLocationSasToken')]"
                        }
                    }
                }
            ]
```

## <a name="behavior-for-a-virtual-machine-scale-set"></a>虚拟机规模集的行为
虚拟机规模集的行为与单一 VM 的行为相同。 创建新 VM 后，会自动使用 DSC 扩展对其进行预配。 如果扩展需要更新的 WMF 版本，则 VM 会重新启动，并联机。 VM 联机后，将下载 DSC 配置 .zip 文件，并在 VM 上预配该文件。 在 [Azure DSC 扩展概述](../virtual-machines/windows/extensions-dsc-overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)中可以找到详细信息。

## <a name="next-steps"></a>后续步骤
检查[适用于 DSC 扩展的 Azure 资源管理器模板](../virtual-machines/windows/extensions-dsc-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)。

了解[DSC 扩展安全处理凭据](../virtual-machines/windows/extensions-dsc-credentials.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)的方法。 

有关 Azure DSC 扩展处理程序的详细信息，请参阅 [Introduction to the Azure Desired State Configuration extension handler](../virtual-machines/windows/extensions-dsc-overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)（Azure Desired State Configuration 扩展处理程序简介）。 

有关 PowerShell DSC 的详细信息，请[访问 PowerShell 文档中心](/powershell/scripting/dsc/overview/overview)。 

