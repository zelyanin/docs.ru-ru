---
title: Шаблоны элементов управления модели автоматизации пользовательского интерфейса для клиентов
ms.date: 03/30/2017
helpviewer_keywords:
- UI Automation, control patterns for clients
- control patterns, UI Automation clients
ms.assetid: 571561d8-5f49-43a9-a054-87735194e013
ms.openlocfilehash: c7d9eeceaba2ed8b624d3001dae86868ef626c08
ms.sourcegitcommit: 944ddc52b7f2632f30c668815f92b378efd38eea
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/03/2019
ms.locfileid: "73458114"
---
# <a name="ui-automation-control-patterns-for-clients"></a>Шаблоны элементов управления модели автоматизации пользовательского интерфейса для клиентов
> [!NOTE]
> Эта документация предназначена для разработчиков .NET Framework, желающих использовать управляемые классы [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] , заданные в пространстве имен <xref:System.Windows.Automation> . Последние сведения о [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)]см. в разделе [API автоматизации Windows. Автоматизация пользовательского интерфейса](https://go.microsoft.com/fwlink/?LinkID=156746).  
  
 В этом обзоре представлены шаблоны элементов управления для клиентов автоматизации пользовательского интерфейса. Он содержит сведения о том, как клиент автоматизации пользовательского интерфейса может использовать шаблоны элементов управления для доступа к сведениям о [!INCLUDE[TLA#tla_ui](../../../includes/tlasharptla-ui-md.md)].  
  
 Шаблоны элементов управления позволяют классифицировать и предоставлять функции элемента управления независимо от типа или внешнего вида элемента управления. Клиенты автоматизации пользовательского интерфейса могут проверять <xref:System.Windows.Automation.AutomationElement> , чтобы определить, какие шаблоны элементов управления поддерживаются и какое поведение элемента управления будет применяться.  
  
 Полный список шаблонов элементов управления см. в разделе [UI Automation Control Patterns Overview](ui-automation-control-patterns-overview.md).  
  
<a name="uiautomation_getting_control_patterns"></a>   
## <a name="getting-control-patterns"></a>Получение шаблонов элементов управления  
 Клиенты получают шаблон элемента управления из <xref:System.Windows.Automation.AutomationElement> , вызывая <xref:System.Windows.Automation.AutomationElement.GetCachedPattern%2A?displayProperty=nameWithType> или <xref:System.Windows.Automation.AutomationElement.GetCurrentPattern%2A?displayProperty=nameWithType>.  
  
 Клиенты могут использовать метод <xref:System.Windows.Automation.AutomationElement.GetSupportedPatterns%2A> или отдельное свойство `IsPatternAvailable` (например, <xref:System.Windows.Automation.AutomationElement.IsTextPatternAvailableProperty>), чтобы определить, поддерживается ли шаблон или группа шаблонов в <xref:System.Windows.Automation.AutomationElement>. Однако более эффективно будет попытаться получить шаблон элемента управления и проверить его на наличие ссылки `null` , чем проверить поддерживаемые свойства и получить шаблон элемента управления, поскольку это приводит к меньшим числом вызовов между процессами.  
  
 В следующем примере показано, как получить шаблон элемента управления <xref:System.Windows.Automation.TextPattern> из объекта <xref:System.Windows.Automation.AutomationElement>.  
  
 [!code-csharp[UIATextPattern_snip#1037](../../../samples/snippets/csharp/VS_Snippets_Wpf/UIATextPattern_snip/CSharp/SearchWindow.cs#1037)]  
  
<a name="uiautomation_properties_on_control_patterns"></a>   
## <a name="retrieving-properties-on-control-patterns"></a>Получение свойств в шаблонах элементов управления  
 Клиенты могут получать значения свойств в шаблонах элементов управления, вызывая <xref:System.Windows.Automation.AutomationElement.GetCachedPropertyValue%2A?displayProperty=nameWithType> или <xref:System.Windows.Automation.AutomationElement.GetCurrentPropertyValue%2A?displayProperty=nameWithType> и преобразуя возвращаемый объекта в соответствующий тип. Дополнительные сведения о [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] свойствах см. в разделе [Свойства модели автоматизации пользовательского интерфейса для клиентов](ui-automation-properties-for-clients.md).  
  
 Кроме `GetPropertyValue`, значения свойств можно получить с помощью методов доступа среды CLR для доступа к свойствам [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] в шаблоне.  
  
<a name="uiautomation_with_variable_patterns"></a>   
## <a name="controls-with-variable-patterns"></a>Элементы управления с переменными шаблонами  
 Некоторые типы элементов управления поддерживают различные шаблоны в зависимости от их состояния или способа использования элемента управления. К примерам элементов управления, которые могут иметь шаблоны переменных, относятся представления списков (эскизы, плитки, значки, список, сведения), диаграммы Microsoft Excel (круговые, линейные, линейчатые, значения ячеек с формулой), область документа Microsoft Word (обычная, веб-макет, структура, макет печати, печать Предварительная версия) и обложки проигрывателя Microsoft Windows Media.  
  
 Элементы управления, реализующие настраиваемые типы элементов управления, могут содержать любой набор шаблонов элементов управления, необходимых для представления их возможностей.  
  
## <a name="see-also"></a>См. также

- [Шаблоны модели автоматизации пользовательского интерфейса](ui-automation-control-patterns.md)
- [Шаблон текста модели автоматизации пользовательского интерфейса](ui-automation-text-pattern.md)
- [Вызов элемента управления с помощью модели автоматизации пользовательского интерфейса](invoke-a-control-using-ui-automation.md)
- [Получение состояния флажка с использованием модели автоматизации пользовательского интерфейса](get-the-toggle-state-of-a-check-box-using-ui-automation.md)
- [Сопоставление шаблона элемента управления для клиентов автоматизации пользовательского интерфейса](control-pattern-mapping-for-ui-automation-clients.md)
- [Пример вставки текста TextPattern](https://github.com/Microsoft/WPF-Samples/tree/master/Accessibility/InsertText)
- [Пример поиска и выбора TextPattern](https://github.com/Microsoft/WPF-Samples/tree/master/Accessibility/FindText)
- [Пример InvokePattern, ExpandCollapsePattern и Тогглепаттерн](https://github.com/Microsoft/WPF-Samples/tree/master/Accessibility/InvokePattern)
