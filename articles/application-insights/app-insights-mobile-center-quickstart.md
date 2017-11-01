---
title: "Azure Application Insights 快速入门 | Microsoft docs"
description: "提供有关快速安装移动应用以使用 Application Insights 和 Mobile Center 进行监视的说明"
services: application-insights
keywords: 
author: numberbycolors
ms.author: daviste
ms.date: 10/05/2017
ms.service: application-insights
ms.topic: quickstart
manager: carmonm
ms.openlocfilehash: c10af49ddc1642967d76314b6b7ac36fe806e603
ms.sourcegitcommit: a7c01dbb03870adcb04ca34745ef256414dfc0b3
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/17/2017
---
# <a name="start-analyzing-your-mobile-app-with-mobile-center-and-application-insights"></a>开始使用 Mobile Center 和 Application Insights 分析移动应用

本快速入门将指导你完成将应用的 Mobile Center 实例连接到 Application Insights 的整个过程。 通过 Application Insights，可以使用比 Mobile Center 的分析[](https://docs.microsoft.com/mobile-center/analytics/)服务提供的工具更为强大的工具来查询、分段、筛选和分析遥测。

## <a name="prerequisites"></a>先决条件

若要完成本快速入门，你需要：

- Azure 订阅。
- iOS、Android、Xamarin、通用 Windows 或 React 本机应用。
 
如果你还没有 Azure 订阅，可以在开始前创建一个[免费](https://azure.microsoft.com/free/)帐户。

## <a name="onboard-to-mobile-center"></a>载入到 Mobile Center

在将 Application Insights 用于移动应用之前，需要将应用载入 [Mobile Center](https://docs.microsoft.com/mobile-center/)。 Application Insights 不直接从移动应用接收遥测。 相反，应用会将自定义事件遥测发送到 Mobile Center。 然后，在接收事件时，Mobile Center 会将这些自定义事件的副本连续导出到 Application Insights。

若要载入应用，请按照应用支持的每个平台的 Mobile Center 快速入门进行操作。 为每个平台创建单独的 Mobile Center 实例：

* [iOS](https://docs.microsoft.com/mobile-center/sdk/getting-started/ios)。
* [Android](https://docs.microsoft.com/mobile-center/sdk/getting-started/android)。
* [Xamarin](https://docs.microsoft.com/mobile-center/sdk/getting-started/xamarin)。
* [通用 Windows](https://docs.microsoft.com/mobile-center/sdk/getting-started/uwp)。
* [React 本机](https://docs.microsoft.com/mobile-center/sdk/getting-started/react-native)。

## <a name="track-events-in-your-app"></a>跟踪应用中的事件

在将应用载入到 Mobile Center 后，需要对其进行修改，以使用 Mobile Center SDK 发送自定义事件遥测。 自定义事件是导出到 Application Insights 的唯一 Mobile Center 遥测类型。

若要从 iOS 应用发送自定义事件，请在 Mobile Center SDK 中使用 `trackEvent` 或 `trackEvent:withProperties` 方法。 [了解有关从 iOS 应用跟踪事件的详细信息。](https://docs.microsoft.com/mobile-center/sdk/analytics/ios)

```Swift
MSAnalytics.trackEvent("Video clicked")
```

若要从 Android 应用发送自定义事件，请在 Mobile Center SDK 中使用 `trackEvent` 方法。 [了解有关从 Android 应用跟踪事件的详细信息。](https://docs.microsoft.com/mobile-center/sdk/analytics/android)

```Java
Analytics.trackEvent("Video clicked")
```

若要从其他应用平台发送自定义事件，请在其 Mobile Center SDK 中使用 `trackEvent` 方法。

若要确保接收自定义事件，请转到 Mobile Center “分析”部分下的“事件”选项卡。 从应用发送事件后，可能需要等待几分钟才会显示事件。

## <a name="create-an-application-insights-resource"></a>创建 Application Insights 资源

在应用发送自定义事件，并且 Mobile Center 收到这些事件后，需要在 Azure 门户中创建 Mobile Center 类型的 Application Insights 资源：

1. 登录到 [Azure 门户](https://portal.azure.com/)。
2. 选择“新建” > “监视 + 管理” > “Application Insights”。

    ![添加 Application Insights 资源](./media/app-insights-mobile-center-quickstart/add.png)

    随后会显示一个配置对话框。 请使用下表填写输入字段。

    | 设置        |  值           | 说明  |
   | ------------- |:-------------|:-----|
   | **Name**      | 某些全局唯一值，如“myApp-iOS” | 标识所监视的应用的名称 |
   | **应用程序类型** | Mobile Center 应用程序 | 所监视的应用的类型 |
   | **资源组**     | 一个新资源组或菜单中的一个现有资源组 | 在其中创建新 Application Insights 资源的资源组 |
   | **位置** | 菜单中的某个位置 | 选择离你近的位置或离托管应用的位置近的位置 |

3. 单击“创建” 。

如果应用支持多个平台（iOS、Android 等），则最好创建单独的 Application Insights 资源，每个平台使用一个资源。

## <a name="export-to-application-insights"></a>导出到 Application Insights

在顶部“概要”部分中“概述”页面上的新 Application Insights 资源中，复制此资源的检测密钥。

在应用的 Mobile Center 实例中，执行以下操作：

1. 在“设置”页上，单击“导出”。
2. 选择“新建导出”，选择“Application Insights”，然后单击“自定义”。
3. 将 Application Insights 检测密钥粘贴到此框中。
4. 同意增加包含 Application Insights 资源的 Azure 订阅的使用量。 每个 Application Insights 资源对每月收到的前 1GB 数据是免费的。 [了解有关 Application Insights 定价的详细信息。](https://azure.microsoft.com/pricing/details/application-insights/)

请务必为应用支持的每个平台重复此过程。

在设置导出[](https://docs.microsoft.com/mobile-center/analytics/export)后，会将 Mobile Center 收到的每个自定义事件复制到 Application Insights 中。 可能需要几分钟的时间才能将事件复制到 Application Insights 中，因此，如果事件未立即显示，请等待一段时间，然后再做进一步的诊断。

为了在首次连接时提供更多的数据，会将 Mobile Center 中最近 48 小时发生的自定义事件自动导出到 Application Insights。

## <a name="start-monitoring-your-app"></a>开始监视应用

Application Insights 可以查询、分段、筛选和分析应用中的自定义事件遥测，比 Mobile Center 提供的分析工具功能更强大。

1. **查询自定义事件遥测。** 从 Application Insights“概述”页面上，选择“分析”。 

   ![Application Insights 中的“分析”按钮](./media/app-insights-mobile-center-quickstart/analytics.png)

   将打开与 Application Insights 资源关联的 Application Insights 分析门户。 通过分析门户，可以直接使用 Log Analytics 查询语言来查询数据，因此，可以询问有关应用及其用户的任意复杂的问题。
   
   在分析门户中打开一个新选项卡，然后粘贴以下查询。 它将返回在过去24小时内从应用发送每个自定义事件的用户数，这些数量按照非重复计数排序。

   ```AIQL
   customEvents
   | where timestamp >= ago(24h)
   | summarize dcount(user_Id) by name 
   | order by dcount_user_Id desc 
   ```

   ![分析门户](./media/app-insights-mobile-center-quickstart/analytics-portal.png)

   1. 通过单击文本编辑器中查询的任意位置，选择此查询。
   2. 然后，单击“运行”运行查询。 

   详细了解有关 [Application Insights 分析](app-insights-analytics.md)和 [Log Analytics 查询语言](https://docs.loganalytics.io/docs/Language-Reference)的信息。


2. **分段和筛选自定义事件遥测。** 从 Application Insights“概述”页面上，选择目录中的“用户”。

   ![用户工具图标](./media/app-insights-mobile-center-quickstart/users-icon.png)

   用户工具可显示应用中有多少用户单击了某些按钮、访问了某些屏幕或使用 Mobile Center SDK 执行了作为事件跟踪的任何其他操作。 如果你一直在寻找对 Mobile Center 事件进行分段和筛选的方法，那么用户工具是一个不错的选择。

   ![用户工具](./media/app-insights-mobile-center-quickstart/users.png) 

   例如，通过选择“拆分依据”下拉菜单中的“国家或地区”，来根据地域对使用量进行分段。

3. **分析应用中的转换、保留和导航模式。** 从 Application Insights“概述”页面上，选择目录中的“用户流”。

   ![用户流工具](./media/app-insights-mobile-center-quickstart/user-flows.png)

   用户流工具直观显示用户在某些起始事件之后发送的事件。 它可用于获取用户浏览应用的整体情况。 它可以显示许多用户改动应用的地方，或反复执行相同操作的地方。

   除了用户流，Application Insights 还提供几种其他使用情况分析工具来回答特定的问题：

   * 漏斗图，用于分析和监视转换率。
   * 保留，用于分析随着时间的推移应用保留用户的情况。
   * 工作簿，用于将可视化效果和文本组合到可共享的报表中。
   * 队列，用于命名和保存特定用户或事件组，以便可以轻松地通过其他分析工具引用它们。

## <a name="clean-up-resources"></a>清理资源

如果不希望继续将 Application Insights 和 Mobile Center 一起使用，请关闭 Mobile Center 中的导出并删除 Application Insights 资源。 这将防止 Application Insights 进一步收取此资源的费用。

要关闭 Mobile Center 中的导出，请执行以下操作：

1. 在 Mobile Center 中，转到“设置”，然后选择“导出”。
2. 单击想要删除的 Application Insights 导出，然后单击底部的“删除导出”并确认。

要删除 Application Insights 资源，请执行以下操作：

1. 在 Azure 门户的左侧菜单中，单击“资源组”，然后选择在其中创建 Application Insights 资源的资源组。
2. 打开要删除的 Application Insights 资源。 然后在该资源的顶部菜单中单击“删除”并确认。 这将永久删除导出到 Application Insights 的数据的副本。

## <a name="next-steps"></a>后续步骤

> [!div class="nextstepaction"]
> [了解客户如何使用应用](app-insights-usage-overview.md)