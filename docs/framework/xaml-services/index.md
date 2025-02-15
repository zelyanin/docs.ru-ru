---
title: Службы XAML
ms.date: 03/30/2017
helpviewer_keywords:
- XAML [XAML Services], System.Xaml concepts
- XAML Services in WPF [XAML Services]
- System.Xaml [XAML Services], conceptual documentation
ms.assetid: 0e11f386-808c-4eae-9ba6-029ad7ba2211
ms.openlocfilehash: a99b9f3cb8c008f72eaac7ee1b8790d63c547a8d
ms.sourcegitcommit: 944ddc52b7f2632f30c668815f92b378efd38eea
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/03/2019
ms.locfileid: "73453967"
---
# <a name="xaml-services"></a>Службы XAML
В этом разделе описываются возможности набора технологий, известного как службы .NET Framework XAML. Большинство описанных служб и API-интерфейсов находятся в сборке System. XAML, которая представляет собой сборку, появившуюся с набором .NET Framework 4 сборок .NET Core. Службы включают модули чтения и записи, классы схем и поддержку схем, фабрики, атрибуты классов, встроенную поддержку языка XAML и другие возможности языка XAML.  
  
## <a name="about-this-documentation"></a>Сведения об этой документации  
 В концептуальной документации по .NET Framework службам XAML предполагается, что у вас есть опыт работы с языком XAML и как он может применяться к конкретной платформе, например [!INCLUDE[TLA#tla_winclient](../../../includes/tlasharptla-winclient-md.md)] или Windows Workflow Foundation или определенной функциональной области. Например, функции настройки сборки в <xref:Microsoft.Build.Framework.XamlTypes>. Эта документация не попытается объяснить основы XAML как языка разметки, терминологии синтаксиса XAML или других вводных материалов. Вместо этого в этой документации особое внимание уделяется использованию .NET Framework служб XAML, включенных в библиотеке сборки System. XAML. Большинство этих API предназначены для сценариев интеграции языка XAML и расширяемости. Это может быть любой из следующих элементов:  
  
- Расширение возможностей базовых средств чтения и записи XAML (обработка потока узлов XAML напрямую; наследование собственного средства чтения XAML или средства записи XAML).  
  
- Определение настраиваемых типов, пригодных для использования XAML, которые не имеют конкретных зависимостей платформы, и присвоение атрибутов типам для передачи своих характеристик системы типов XAML в .NET Framework служб XAML.  
  
- Размещение средств чтения или записи XAML в качестве компонента приложения, такого как визуальный конструктор или интерактивный редактор для источников разметки XAML.  
  
- Запись преобразователей значений XAML (расширения разметки; преобразователи типов для пользовательских типов).  
  
- Определение пользовательского контекста схемы XAML (использование альтернативных методов загрузки сборок для резервных источников данных; использование методов поиска известных типов вместо постоянного отражения сборок; использование загруженных концепций сборки, не использующих `AppDomain` CLR и связанная модель безопасности).  
  
- Расширение базовой системы типов XAML.  
  
- Использование `Lookup` или `Invoker` методик, влияющих на систему типов XAML и способ оценки резервов типов.  
  
 Если вы ищете вводный материал о XAML в качестве языка, вы можете попробовать ознакомиться с [обзором XAML (WPF)](../../desktop-wpf/fundamentals/xaml.md). В этом разделе описывается XAML для аудитории, которая является новой для [!INCLUDE[TLA#tla_winclient](../../../includes/tlasharptla-winclient-md.md)], а также для использования разметки XAML и возможностей языка XAML. Еще один полезный документ — вводный материал в [спецификации языка XAML](https://go.microsoft.com/fwlink/?LinkId=114525).  
  
## <a name="net-framework-xaml-services-and-systemxaml-in-the-net-architecture"></a>.NET Framework служб XAML и System. XAML в архитектуре .NET  
 В предыдущих версиях Microsoft .NET Framework поддержка функций языка XAML была реализована платформами, созданными на основе Microsoft .NET Framework ([!INCLUDE[TLA#tla_winclient](../../../includes/tlasharptla-winclient-md.md)], Windows Workflow Foundation и Windows Communication Foundation (WCF)), и поэтому изменяются его поведение и API, используемые в зависимости от конкретной платформы. Это включало средство синтаксического анализа XAML и его механизм создания графа объектов, встроенные функции языка XAML, поддержку сериализации и т. д.  
  
 В .NET Framework 4 .NET Framework службы XAML и сборка System. xaml определяют большую часть возможностей, необходимых для поддержки функций языка XAML. Сюда входят базовые классы для средств чтения и записи XAML. Самая важная функция, добавленная к .NET Framework службам XAML, которые отсутствовали в каких-либо специфических реализациях XAML, — это представление системы типов для XAML. Представление системы типов представляет XAML в объектно-ориентированном подходе, который предоставляет возможности XAML без зависимости от конкретных возможностей платформ.  
  
 Система типов XAML не ограничивается формой разметки или характерной для времени выполнения в происхождении XAML. и не ограничивается какой-либо конкретной системой резервных типов. Система типов XAML включает представления объектов для типов, членов, контекстов схемы XAML, концепций на уровне XML, а также других концепций языка XAML или встроенных функций XAML. Использование или расширение системы типов XAML позволяет создавать производные от классов, таких как средства чтения и записи XAML, а также расширять функциональные возможности представлений XAML в определенные функции, предоставляемые платформой, технологией или приложением, которое использует или выдает XAML. Понятие контекста схемы XAML позволяет реализовать практическую операцию записи графа объектов из сочетания реализации средства записи объектов XAML, системы типизированных типов технологии, которая передается через сведения о сборке в контексте, и узел XAML. источника. Дополнительные сведения о концепции схемы XAML. см. раздел [контекст схемы XAML по умолчанию и контекст схемы XAML WPF](default-xaml-schema-context-and-wpf-xaml-schema-context.md).  
  
## <a name="xaml-node-streams-xaml-readers-and-xaml-writers"></a>Потоки узлов XAML, средства чтения XAML и модули записи XAML  
 Чтобы понять, какая роль .NET Framework служб XAML играет в связи между языком XAML и конкретными технологиями, которые используют XAML в качестве языка, полезно понимать концепцию потока узлов XAML и то, как это понятие формирует API и Терминология. Поток узлов XAML — это концептуальный промежуточный объект между представлением языка XAML и графом объекта, который представляет или определяет XAML.  
  
- Средство чтения XAML — это сущность, которая обрабатывает XAML в некоторой форме и создает поток узлов XAML. В API средство чтения XAML представлено базовым классом <xref:System.Xaml.XamlReader>.  
  
- Модуль записи XAML — это сущность, которая обрабатывает поток узлов XAML и создает что-то другое. В API средство записи XAML представлено базовым классом <xref:System.Xaml.XamlWriter>.  
  
 Две наиболее распространенные сценарии, включающие XAML, — это загрузка XAML для создания экземпляра графа объектов и сохранение графа объектов из приложения или средства и создание представления XAML (обычно в форме разметки, сохраненной как текстовый файл). Загрузка XAML и создание графа объектов часто называются в этой документации в качестве пути загрузки. Сохранение или сериализация существующего графа объектов в XAML часто упоминается в этой документации в качестве пути сохранения.  
  
 Наиболее распространенный тип пути загрузки можно описать следующим образом.  
  
- Начните с представления XAML в формате XML в кодировке UTF и сохранении в виде текстового файла.  
  
- Загрузка XAML в <xref:System.Xaml.XamlXmlReader>. <xref:System.Xaml.XamlXmlReader> является подклассом <xref:System.Xaml.XamlReader>.  
  
- Результатом является поток узлов XAML. Доступ к отдельным узлам потока узлов XAML можно получить с помощью <xref:System.Xaml.XamlXmlReader> / <xref:System.Xaml.XamlReader> API. Наиболее типичная операция заключается в том, чтобы продвинуть поток узлов XAML, обрабатывая каждый узел, используя метафору текущей записи.  
  
- Передайте полученные узлы из потока узлов XAML в <xref:System.Xaml.XamlObjectWriter> API. <xref:System.Xaml.XamlObjectWriter> является подклассом <xref:System.Xaml.XamlWriter>.  
  
- <xref:System.Xaml.XamlObjectWriter> записывает граф объектов, по одному объекту за раз, в соответствии с ходом выполнения через исходный поток узлов XAML. Это делается с помощью контекста схемы XAML и реализации, которая может получить доступ к сборкам и типам системы резервного типа и платформы.  
  
- Вызовите <xref:System.Xaml.XamlObjectWriter.Result%2A> в конце потока узлов XAML, чтобы получить корневой объект графа объекта.  
  
 Наиболее распространенный тип пути сохранения можно описать следующим образом.  
  
- Начните с графа объектов всего времени выполнения приложения, содержимого пользовательского интерфейса и состояния времени выполнения или меньшего сегмента общего представления объекта приложения во время выполнения.  
  
- Из логического начального объекта, такого как корневой каталог приложения или корневой каталог документа, загрузите объекты в <xref:System.Xaml.XamlObjectReader>. <xref:System.Xaml.XamlObjectReader> является подклассом <xref:System.Xaml.XamlReader>.  
  
- Результатом является поток узлов XAML. Доступ к отдельным узлам потока узлов XAML можно получить с помощью <xref:System.Xaml.XamlObjectReader> и <xref:System.Xaml.XamlReader> API. Наиболее типичная операция заключается в том, чтобы продвинуть поток узлов XAML, обрабатывая каждый узел, используя метафору текущей записи.  
  
- Передайте полученные узлы из потока узлов XAML в <xref:System.Xaml.XamlXmlWriter> API. <xref:System.Xaml.XamlXmlWriter> является подклассом <xref:System.Xaml.XamlWriter>.  
  
- <xref:System.Xaml.XamlXmlWriter> записывает XAML в кодировке XML UTF. Его можно сохранить как текстовый файл, как поток или в других формах.  
  
- Вызовите <xref:System.Xaml.XamlXmlWriter.Flush%2A>, чтобы получить окончательный результат.  
  
 Дополнительные сведения о концепциях потока узлов XAML см. в разделе Основные сведения о [структурах и концепциях потоков узлов XAML](understanding-xaml-node-stream-structures-and-concepts.md).  
  
### <a name="the-xamlservices-class"></a>Класс Ксамлсервицес  
 Не всегда требуется работать с потоком узлов XAML. Если требуется базовый путь загрузки или базовый путь сохранения, можно использовать API в классе <xref:System.Xaml.XamlServices>.  
  
- Различные сигнатуры <xref:System.Xaml.XamlServices.Load%2A> реализуют путь загрузки. Можно загрузить файл или поток или загрузить <xref:System.Xml.XmlReader>, <xref:System.IO.TextReader> или <xref:System.Xaml.XamlReader>, которые заключают входные данные XAML, загрузив их с помощью интерфейсов API этого средства чтения.  
  
- Различные сигнатуры <xref:System.Xaml.XamlServices.Save%2A> сохраняют граф объектов и выводят выходные данные в виде потока, файла или <xref:System.Xml.XmlWriter>/экземпляра <xref:System.IO.TextWriter>.  
  
- <xref:System.Xaml.XamlServices.Transform%2A> преобразует XAML, связывая путь загрузки и путь сохранения как единую операцию. Для <xref:System.Xaml.XamlReader> и <xref:System.Xaml.XamlWriter>можно использовать другой контекст схемы или другую систему резервных типов, что влияет на преобразование полученного XAML-кода.  
  
 Дополнительные сведения об использовании <xref:System.Xaml.XamlServices>см. в разделе [класс ксамлсервицес и основные операции чтения и записи XAML](xamlservices-class-and-basic-xaml-reading-or-writing.md).  
  
## <a name="xaml-type-system"></a>Система типов XAML  
 Система типов XAML предоставляет API-интерфейсы, необходимые для работы с определенным отдельным узлом потока узлов XAML.  
  
 <xref:System.Xaml.XamlType> — это представление объекта, которое обрабатывается между начальным и конечным узлом объекта.  
  
 <xref:System.Xaml.XamlMember> — это представление для элемента объекта — то, что обрабатывается между начальным и конечным элементами узла.  
  
 Интерфейсы API, такие как <xref:System.Xaml.XamlType.GetAllMembers%2A> и <xref:System.Xaml.XamlType.GetMember%2A>, и <xref:System.Xaml.XamlMember.DeclaringType%2A> сообщают о связях между <xref:System.Xaml.XamlType> и <xref:System.Xaml.XamlMember>.  
  
 Поведение системы типов XAML по умолчанию, реализованное .NET Framework службах XAML, основано на среде CLR и статического анализа типов CLR в сборках с помощью отражения. Поэтому для конкретного типа CLR реализация системы типов XAML по умолчанию может предоставить схему XAML этого типа и его членов и сообщить о ней в терминах системы типов XAML. В системе типов XAML по умолчанию понятие о назначении типов сопоставляется с наследованием CLR, а понятия экземпляров, типов значений и т. д. также сопоставляются с поддержкой поведения и функций среды CLR.  
  
## <a name="reference-for-xaml-language-features"></a>Справочник по функциям языка XAML  
 Для поддержки XAML .NET Framework службы XAML предоставляют определенную реализацию концепций языка XAML, как определено для пространства имен XAML языка XAML. Они описаны в виде конкретных страниц ссылок. Функции языка задокументированы с точки зрения поведения этих функций языка, когда они обрабатываются средством чтения XAML или средством записи XAML, определенным в .NET Framework службах XAML. Для получения дополнительной информации см. [XAML Namespace (x:) Language Features](xaml-namespace-x-language-features.md).
