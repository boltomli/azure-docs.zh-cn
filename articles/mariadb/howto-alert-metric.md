---
title: 配置指标警报-Azure 门户-Azure Database for MariaDB
description: 本文介绍了如何通过 Azure 门户配置和访问针对 Azure Database for MariaDB 的指标警报。
author: ajlam
ms.author: andrela
ms.service: mariadb
ms.topic: conceptual
ms.date: 3/18/2020
ms.openlocfilehash: 683b89b29265159b0316d427a7ae616bf3766f7c
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/28/2020
ms.locfileid: "79529011"
---
# <a name="use-the-azure-portal-to-set-up-alerts-on-metrics-for-azure-database-for-mariadb"></a>使用 Azure 门户设置针对 Azure Database for MariaDB 指标的警报

本文介绍了如何使用 Azure 门户设置 Azure Database for MariaDB 警报。 可根据监视指标接收 Azure 服务的警报。

当指定的指标值越过了分配的阈值时，就会触发此警报。 首次满足条件时，以及之后不再满足条件时，都会触发此警报。

可配置警报，使警报触发时执行以下操作：
* 向服务管理员和共同管理员发送电子邮件通知
* 将电子邮件发送到指定的其他电子邮件地址。
* 调用 Webhook

可使用以下项配置并获取预警规则相关信息：
* [Azure 门户](../azure-monitor/platform/alerts-metric.md#create-with-azure-portal)
* [Azure CLI](../azure-monitor/platform/alerts-metric.md#with-azure-cli)
* [Azure 监视器 REST API](https://docs.microsoft.com/rest/api/monitor/metricalerts)

## <a name="create-an-alert-rule-on-a-metric"></a>在指标上创建警报规则
1. 在 [Azure 门户](https://portal.azure.com/)中，选择要监视的 Azure Database for MariaDB 服务器。

2. 在边栏的“监视”部分，选择“警报”，如下所示   ：

   ![选择“警报规则”](./media/howto-alert-metric/2-alert-rules.png)

3. 选择“添加指标警报”（+ 图标）  。

4. 随即打开“创建规则”页面，如下所示  。 填写所需信息：

   ![添加指标警报窗体](./media/howto-alert-metric/4-add-rule-form.png)

5. 在“条件”部分中，选择“添加条件”   。

6. 从要发出警报的信号列表中选择一个指标。 在此示例中，选择“存储百分比”。
   
   ![选择指标](./media/howto-alert-metric/6-configure-signal-logic.png)

7. 配置警报逻辑，包括“条件”（例如，  “大于”）、“阈值”（例如，  85%）、“时间聚合”、触发警报前必须满足指标规则的“时间段”。（例如，   "过去30分钟"）和**频率**。
   
   完成后选择“完成”  。

   ![选择指标](./media/howto-alert-metric/7-set-threshold-time.png)

8. 在“操作组”部分中，选择“新建”创建新组以接收有关警报的通知   。

9. 使用名称、短名称、订阅和资源组填写“添加操作组”表单。

10. 配置“电子邮件/短信/推送/语音”操作类型****。
    
    选择“电子邮件 Azure 资源管理器角色”以选择订阅所有者、参与者和读取器来接收通知。
   
    （可选）如果希望在警报触发时调用有效的 URI，请将其放入“Webhook”字段  。

    完成后选择“确定”  。

    ![操作组](./media/howto-alert-metric/10-action-group-type.png)

11. 指定预警规则名称、说明和严重性。

    ![操作组](./media/howto-alert-metric/11-name-description-severity.png) 

12. 选择“创建警报规则”可以创建警报  。

    几分钟后，警报将处于活动状态，并按前面所述进行触发。

## <a name="manage-your-alerts"></a>管理警报
创建警报后，可选择它并执行以下操作：

* 查看图，了解与此警报相关的指标阈值和前一天实际值。
* 编辑或删除预警规则   。
* 如果要暂时停止或恢复接收通知，可禁用或启用警报   。


## <a name="next-steps"></a>后续步骤
* 了解[在警报中配置 Webhook](../monitoring-and-diagnostics/insights-webhooks-alerts.md)的详细信息。
* 获取[指标集合概述](../monitoring-and-diagnostics/insights-how-to-customize-monitoring.md)以确保服务可用且响应迅速。
