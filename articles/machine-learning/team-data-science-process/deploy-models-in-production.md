---
title: 在生产中部署模型 - Team Data Science Process
description: 如何将模型部署到生产环境使其在进行业务决策方面能够发挥积极作用。
author: marktab
manager: marktab
editor: marktab
ms.service: machine-learning
ms.subservice: team-data-science-process
ms.topic: article
ms.date: 01/10/2020
ms.author: tdsp
ms.custom: seodec18, previous-author=deguhath, previous-ms.author=deguhath
ms.openlocfilehash: e3e2ef3340ca836f56176c21cf3d221f0bf172b9
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/28/2020
ms.locfileid: "76722231"
---
# <a name="deploy-models-to-production-to-play-an-active-role-in-making-business-decisions"></a>将模型部署到生产中，使这些模型在制定业务决策方面能够发挥积极作用

生产部署可让模型在企业中发挥积极作用。 所部署模型提供的预测可用于业务决策。

## <a name="production-platforms"></a>生产平台

可通过多种方法和平台将模型投入生产。 下面是几个选项：

- [使用 Azure 机器学习部署模型的位置](../how-to-deploy-and-where.md)
- [SQL-Server 中的模型部署](https://docs.microsoft.com/sql/advanced-analytics/tutorials/sqldev-py6-operationalize-the-model)
- [Microsoft Machine Learning Server](https://docs.microsoft.com/sql/advanced-analytics/r/r-server-standalone)

>[!NOTE]
>在部署之前，必须确保模型的延迟评分够低，使模型可在生产环境中使用。
>

>[!NOTE]
>对于使用 Azure 机器学习工作室的部署，请参阅[部署 Azure 机器学习 Web 服务](../studio/deploy-a-machine-learning-web-service.md)。
>

## <a name="ab-testing"></a>A/B 测试

当生产中有多个模型时，可以使用[A/B 测试](https://en.wikipedia.org/wiki/A/B_testing)来比较模型的性能。 
 
## <a name="next-steps"></a>后续步骤

我们还提供了相应的演练，用于演示**具体方案**的操作过程的所有步骤。 [示例演练](walkthroughs.md)一文列出了相关步骤并以缩略图说明的形式提供了链接。 这些演练演示如何将云、本地工具和服务合并到工作流或管道中，以创建智能应用程序。 
