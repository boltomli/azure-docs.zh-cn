---
title: 映射数据流中的聚合转换
description: 了解如何在 Azure 数据工厂中大规模聚合数据以及映射数据流聚合转换。
author: kromerm
ms.author: makromer
ms.reviewer: daperlov
ms.service: data-factory
ms.topic: conceptual
ms.custom: seo-lt-2019
ms.date: 03/24/2020
ms.openlocfilehash: 871f2b49e2dce9d762ef8a54923da04b0f24e4be
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/28/2020
ms.locfileid: "81606536"
---
# <a name="aggregate-transformation-in-mapping-data-flow"></a>映射数据流中的聚合转换

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

聚合转换定义数据流中列的聚合。 使用表达式生成器可以定义不同类型的聚合，如求和、MIN、MAX 和 COUNT，并按现有或计算列进行分组。

## <a name="group-by"></a>Group by

选择现有列或创建新的计算列，以用作聚合的 group by 子句。 若要使用现有列，请从下拉列表中选择它。 若要创建新的计算列，请将鼠标悬停在子句上，并单击 "**计算列**"。 这将打开 "数据流[表达式生成器](concepts-data-flow-expression-builder.md)"。 创建计算列后，在 "**名称**" 字段中输入输出列的名称。 如果要添加其他 group by 子句，请将鼠标悬停在现有子句上，并单击加号图标。

![聚合转换组（按设置）](media/data-flow/agg.png "聚合转换组（按设置）")

在聚合转换中，group by 子句是可选的。

## <a name="aggregate-column"></a>聚合列 

请参阅 "**聚合**" 选项卡以生成聚合表达式。 您可以使用聚合覆盖现有列，或使用新名称创建新的字段。 在列名称选择器旁的右侧框中输入聚合表达式。 若要编辑该表达式，请单击文本框打开 "表达式生成器"。 若要添加其他聚合，请将鼠标悬停在现有表达式上，并单击加号图标创建新的聚合列或[列模式](concepts-data-flow-column-pattern.md)。

每个聚合表达式必须包含至少一个聚合函数。

![聚合转换聚合设置](media/data-flow/agg2.png "聚合转换聚合设置")


> [!NOTE]
> 在调试模式下，表达式生成器不能用聚合函数生成数据预览。 若要查看聚合转换的数据预览，请关闭 "表达式生成器" 并通过 "数据预览" 选项卡来查看数据。

## <a name="reconnect-rows-and-columns"></a>重新连接行和列

聚合转换类似于 SQL 聚合 select 查询。 未包含在 group by 子句或聚合函数中的列不会流向聚合转换的输出。 如果希望将其他列包含在聚合输出中，请执行以下方法之一：

* 使用聚合函数（如或`last()` `first()` ）来包含该其他列。
* 使用[自联接模式](https://mssqldude.wordpress.com/2018/12/20/adf-data-flows-self-join/)将这些列重新加入到输出流中。

## <a name="removing-duplicate-rows"></a>删除重复行

聚合转换的一个常见用途是删除或标识源数据中的重复项。 此过程称为删除重复。 根据一组按键分组，使用所选的试探法来确定要保留的重复行。 常见试探法`first()`包括`last()`、 `max()`、和`min()`。 使用[列模式](concepts-data-flow-column-pattern.md)将规则应用到除 group by 列之外的每一列。

![重复数据删除](media/data-flow/agg-dedupe.png "重复数据删除")

在上面的示例中， `ProductID`列`Name`和用于分组。 如果两个行的值与这两个列的值相同，则它们被视为重复项。 在此聚合转换中，将保留第一个匹配的行的值，并且将删除所有其他行的值。 使用列模式语法，名称不`ProductID`为和`Name`的所有列都映射到其现有列名，并给定第一个匹配行的值。 输出架构与输入架构相同。

对于数据验证方案， `count()`函数可用于计算有多少重复项。

## <a name="data-flow-script"></a>数据流脚本

### <a name="syntax"></a>语法

```
<incomingStream>
    aggregate(
           groupBy(
                <groupByColumnName> = <groupByExpression1>,
                <groupByExpression2>
               ),
           <aggregateColumn1> = <aggregateExpression1>,
           <aggregateColumn2> = <aggregateExpression2>,
           each(
                match(matchExpression),
                <metadataColumn1> = <metadataExpression1>,
                <metadataColumn2> = <metadataExpression2>
               )
          ) ~> <aggregateTransformationName>
```

### <a name="example"></a>示例

下面的示例使用传入流`MoviesYear`并按列`year`对行进行分组。 转换将创建计算结果为`avgrating`列`Rating`平均值的聚合列。 此聚合转换的名称`AvgComedyRatingsByYear`为。

在数据工厂 UX 中，此转换如下图所示：

![按示例分组](media/data-flow/agg-script1.png "按示例分组")

![聚合示例](media/data-flow/agg-script2.png "聚合示例")

下面的代码片段中提供了此转换的数据流脚本。

```
MoviesYear aggregate(
                groupBy(year),
                avgrating = avg(toInteger(Rating))
            ) ~> AvgComedyRatingByYear
```

![聚合数据流脚本](media/data-flow/aggdfs1.png "聚合数据流脚本")

```MoviesYear```：用于定义年份和标题列```AvgComedyRatingByYear```的派生列：按年```avgrating```分组的 comedies 平均分级的聚合转换：要创建的、用于保存聚合值的新列的名称

```
MoviesYear aggregate(groupBy(year),
    avgrating = avg(toInteger(Rating))) ~> AvgComedyRatingByYear
```

## <a name="next-steps"></a>后续步骤

* 使用[窗口转换](data-flow-window.md)定义基于窗口的聚合
