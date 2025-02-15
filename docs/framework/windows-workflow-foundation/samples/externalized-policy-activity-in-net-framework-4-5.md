---
title: Реализованное действие политики в .NET Framework 4.5
ms.date: 03/30/2017
ms.assetid: 92fd6f92-23a1-4adf-b96a-2754ea93ad3e
ms.openlocfilehash: 7d3c9b2bd9da7e3793479c002094504a4a556aa0
ms.sourcegitcommit: 005980b14629dfc193ff6cdc040800bc75e0a5a5
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/14/2019
ms.locfileid: "70989572"
---
# <a name="externalized-policy-activity-in-net-framework-45"></a>Реализованное действие политики в .NET Framework 4.5

В этом примере показано, как действие ExternalizedPolicy4 позволяет выполнять [!INCLUDE[netfx35_long](../../../../includes/netfx35-long-md.md)] существующие объекты Windows Workflow Foundation (WF <xref:System.Workflow.Activities.Rules.RuleSet> 3,5) [!INCLUDE[netfx_current_long](../../../../includes/netfx-current-long-md.md)] в Windows Workflow Foundation (WF 4,5) непосредственно с помощью обработчика правил. , поставляемый в WF 3,5. Используя это действие, можно создавать и выполнять любой существующий набор правил <xref:System.Workflow.Activities.Rules.RuleSet> WF 3.5. Дополнительные сведения о обработчике правил WF 3,5, включенном в состав Windows Workflow Foundation, см. в статье [Введение в обработчик правил Windows Workflow Foundation](https://go.microsoft.com/fwlink/?LinkId=166079). Дополнительные сведения о переносе правил [!INCLUDE[wf1](../../../../includes/wf1-md.md)] [!INCLUDE[netfx_current_short](../../../../includes/netfx-current-short-md.md)]в см. в [руководстве по миграции](../migration-guidance.md).

## <a name="projects-in-this-sample"></a>Проекты в этом образце

|Имя проекта|Описание|Основные файлы|
|-|-|-|
|ExternalizedPolicy4|Содержит действие PolicyExternalizedPolicy4 и его конструктор WF 4.5.|**ExternalizedPolicy4.CS**: определение действия.<br /><br /> **ExternalizedPolicy4Designer. XAML**: Пользовательский конструктор для действия ExternalizedPolicy4. Он использует редактор правил (<xref:System.Workflow.Activities.Rules.Design.RuleSetDialog>), определенный в конструкторе правил WF 3.5.|
|ImperativeCodeClientSample|Образец клиентского приложения, осуществляющего конфигурирование и запуск рабочего процесса с использованием приложения PolicyExternalizedPolicy4, использующего императивный код C# (конструктор не используется).|**ApplyDiscount. rules**: Файл с [!INCLUDE[wf1](../../../../includes/wf1-md.md)] определениями правил.<br /><br /> **Order.CS**: Тип, представляющий заказ клиента. Правила применяются к объектам этого типа.<br /><br /> **Program.cs**: Настраивает и запускает рабочий процесс с действием Policy4 для применения правил, определенных в ApplyDiscount. rules, к экземплярам объектов Order.<br /><br /> Файл App. config: Файл конфигурации с путем к файлу правил.|
|DesignerClientSample|Образец клиентского приложения, осуществляющего конфигурирование и запуск рабочего процесса с использованием приложения ExternalPolicy4 в конструкторе [!INCLUDE[wf1](../../../../includes/wf1-md.md)].|**Sequence1. XAML**: Последовательный рабочий процесс, использующий действие Policy4 для выполнения оценки правил.<br /><br /> **Program.cs**: Выполняет экземпляр рабочего процесса, определенного в Sequence1.xaml.|

## <a name="the-externalizedpolicy4-activity"></a>Действие ExternalizedPolicy4

Действие ExternalizedPolicy4 представляет собой действие <xref:System.Activities.NativeActivity>, позволяющее выполнять объекты WF 3.5 <xref:System.Workflow.Activities.Rules.RuleSet> в рамках рабочих процессов WF 4.5. Следующий пример кода представляет собой упрощенное определение публичной объектной модели действия.

```csharp
public class ExternalizedPolicy4Activity<TResult>: CodeActivity
{
    public string RulesFilePath

    public string RuleSetName

    [RequiredArgument]
    public InArgument<T> TargetObject

    [RequiredArgument]
    public OutArgument<T> ResultObject

    public OutArgument<ValidationErrorCollection> ValidationErrors
}
```

|Свойство.|Описание|
|-|-|
|RuleSetFilePath|Путь к файлу <xref:System.Workflow.Activities.Rules.RuleSet> в .NET Framework 3.5, который определяется при выполнении действия.|
|RuleSetName|Имя <xref:System.Workflow.Activities.Rules.RuleSet>, которое будет использоваться в файлах правил с расширением RULES.|
|TargetObject|Объект, для которого рассчитываются объекты <xref:System.Workflow.Activities.Rules.Rule> в <xref:System.Workflow.Activities.Rules.RuleSet>.|
|ResultObject|Результирующий объект, полученный после применения правил (например, правил, примененных к входному аргументу), с сохранением результата в результирующем аргументе.|
|ValidationError|Список ошибок проверки, возвращаемый обработчиком правил WF 3.5 при проверке класса <xref:System.Workflow.Activities.Rules.RuleSet> для целевого объекта до его выполнения.|

## <a name="externalizedpolicy4-activity-designer"></a>Конструктор действия ExternalizedPolicy4

Конструктор действия ExternalizedPolicy4 позволяет конфигурировать активность таким образом, чтобы использовать существующий набор правил без необходимости написания кода. Достаточно просто задать путь к файлу с расширением RULES и указать желаемое имя <xref:System.Workflow.Activities.Rules.RuleSet>. Конструктор также позволяет вносить изменения в <xref:System.Workflow.Activities.Rules.RuleSet>. После построения решения действие будет занесено в область элементов в раздел Microsoft.Samples.Activities.Rules. Конструктор позволяет выбирать файл с расширением RULES и <xref:System.Workflow.Activities.Rules.RuleSet>. При нажатии кнопки **изменить набор правил** отображается WF 3,5 <xref:System.Workflow.Activities.Rules.Design.RuleSetDialog> . Это диалоговое окно представляет собой повторно размещенный редактор правил WF 3.5, используемый для просмотра и изменения правил, выполняемых действием ExternalizedPolicy4.

## <a name="policy4-and-externalpolicy4"></a>Policy4 и ExternalPolicy4

Действие политики позволяет создавать и выполнять .NET Framework 3,5 RuleSet в рабочем процессе WF 4,5. Набор правил <xref:System.Workflow.Activities.Rules.RuleSet> сериализуется в рамках определения действия Policy4 в языке XAML. Образец действия ExternalizedPolicy4 показывает, как использовать существующий внешний набор правил <xref:System.Workflow.Activities.Rules.RuleSet> (содержащийся в файле с расширением RULES).

## <a name="use-this-sample"></a>Используйте этот пример

Для запуска этого образца специальные настройки не требуются. Откройте решение в Visual Studio и нажмите клавишу **F5** , чтобы запустить приложение.

Этот пример содержит два клиентских приложения: Императивекодеклиентсампле и Десигнерклиентсампле. Клиентское приложение ImperativeCodeClientSample показывает, как конфигурировать и запускать действие ExternalizedPolicy4 с использованием императивного кода C#. Клиентское приложение DesignerClientSample показывает, как конфигурировать и запускать действие ExternalizedPolicy4 с использованием конструктора.

### <a name="run-the-imperativecodeclientsample-application"></a>Запуск приложения Императивекодеклиентсампле

1. С помощью Visual Studio откройте файл решения *Policy4sample. sln* .

2. В **Обозреватель решений**щелкните правой кнопкой мыши проект **Императивекодеклиентсампле** и выберите **Назначить запускаемым проектом**.

3. Чтобы запустить проект, нажмите клавиши **CTRL**+**F5**.

### <a name="run-the-designerclientsample-application"></a>Запуск приложения Десигнерклиентсампле

1. С помощью Visual Studio откройте файл решения *Policy4sample. sln* .

2. В **Обозреватель решений**щелкните правой кнопкой мыши проект **Десигнерклиентсампле** и выберите **Назначить запускаемым проектом**.

3. Нажмите клавиши **CTRL**+**SHIFT**+**B** , чтобы скомпилировать проект.

4. Нажмите клавиши **CTRL**+**F5** , чтобы запустить проект.

> [!IMPORTANT]
> Образцы уже могут быть установлены на компьютере. Перед продолжением проверьте следующий каталог (по умолчанию).
>
> `<InstallDrive>:\WF_WCF_Samples`
>
> Если этот каталог не существует, перейдите к [примерам Windows Communication Foundation (WCF) и Windows Workflow Foundation (WF) для .NET Framework 4](https://go.microsoft.com/fwlink/?LinkId=150780) , чтобы скачать все Windows Communication Foundation (WCF) [!INCLUDE[wf1](../../../../includes/wf1-md.md)] и примеры.
>
> Этот образец находится в следующем каталоге:
>
> `<InstallDrive>:\WF_WCF_Samples\WF\Scenario\ActivityLibrary\Rules-ExternalizedPolicy4`
