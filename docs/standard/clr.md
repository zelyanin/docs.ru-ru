---
title: Обзор среды CLR — .NET Framework
ms.date: 04/02/2019
ms.technology: dotnet-standard
helpviewer_keywords:
- compiling source code, runtime functionality
- code, execution
- managed data
- runtime
- common language runtime
- metadata, runtime functionality
- .NET Framework, common language runtime
- language compilers
- managed code
- source code execution
- code, runtime functionality
ms.assetid: 059a624e-f7db-4134-ba9f-08b676050482
ms.custom: updateeachrelease
ms.openlocfilehash: c866e3d1a4de31361843f5c071510fd18247cb39
ms.sourcegitcommit: 559fcfbe4871636494870a8b716bf7325df34ac5
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/30/2019
ms.locfileid: "73132814"
---
# <a name="common-language-runtime-clr-overview"></a>Обзор среды CLR

.NET Framework предоставляет среду выполнения (среду CLR), которая выполняет код и предлагает службы, облегчающие процесс разработки.

Компиляторы и иные средства позволяют использовать функции среды CLR и дают разработчикам возможность писать код, использующий преимущества этой среды управляемого выполнения. Код, разработанный с языковым компилятором, который обращается к среде выполнения, называют управляемым кодом. В нем используются преимущества таких средств, как объединение языков программирования, объединенная обработка исключений, усиленная безопасность, поддержка отслеживания версий и развертывания, упрощенная модель взаимодействия компонентов, а также службы отладки и профилирования.

> [!NOTE]
> Компиляторы и инструменты могут создавать выходные данные для общеязыковой среды выполнения (CLR), поскольку система типов, формат метаданных и среда выполнения (виртуальная система выполнения) определяются открытым стандартом — спецификацией общеязыковой инфраструктуры ECMA. Дополнительные сведения см. в разделе [Спецификации ECMA для C# и инфраструктуры Common Language Infrastructure (CLI)](https://visualstudio.microsoft.com/license-terms/ecma-c-common-language-infrastructure-standards/).

Чтобы включить в среде выполнения предоставление служб управляемому коду, языковые компиляторы должны предоставлять метаданные с описанием типов, членов и ссылок в коде. Метаданные хранятся вместе с кодом. Они содержатся в каждом загружаемом переносимом исполняемом (PE) файле среды CLR. Метаданные в среде выполнения используются для поиска и загрузки классов, размещения экземпляров в памяти, разрешения имен при вызове методов, создания машинного кода, обеспечения безопасности и установки границ контекста времени выполнения.

Среда выполнения обеспечивает автоматическое размещение объектов и управление ссылками на них, а также освобождение объектов, когда они больше не используются. Объекты, время жизни которых управляется подобным образом, называются управляемыми данными. Сборка мусора исключает утечку памяти и некоторые другие часто возникающие ошибки программирования. Если код является управляемым, в приложении .NET Framework можно использовать управляемые данные, неуправляемые данные или управляемые и неуправляемые данные одновременно. Поскольку языковые компиляторы поставляют собственные типы, например, простые типы, пользователь может не знать (и не иметь такой необходимости), являются ли данные управляемыми.

Среда CLR упрощает разработку компонентов и приложений, объекты которых могут работать в разных языках. Объекты, написанные на разных языках, могут взаимодействовать друг с другом, а их поведение может быть тесно интегрировано. Например, разработчик может определить класс, а затем на другом языке создать производный от него класс или вызвать метод из исходного класса. Можно также передать экземпляр класса в метод класса, написанного на другом языке. Такая интеграция языков программирования возможна в силу того, что языковые компиляторы и программы, которые обращаются к среде выполнения, используют систему общих типов, определенную средой выполнения, и следуют правилам среды выполнения при определении новых типов, а также при создании, использовании, сохранении и привязки к типам.

В составе своих метаданных все управляемые компоненты содержат сведения о компонентах и ресурсах, на базе которых они построены. Среда выполнения использует эти сведения, чтобы обеспечить наличие всех необходимых ресурсов для компонента или приложения. Это снижает вероятность сбоев кода из-за каких-либо неудовлетворенных зависимостей. Сведения о регистрации и данные о состоянии больше не сохраняются в реестре, где их трудно задавать и поддерживать. Вместо этого сведения об определяемых разработчиком типах (и их зависимостях) сохраняются вместе с кодом в виде метаданных, что существенно упрощает репликацию и удаление компонентов.

Языковые компиляторы и программы предоставляют функции среды выполнения так, чтобы они были полезны и интуитивно понятны для разработчиков. Это означает, что некоторые средства среды выполнения могут быть заметными в одной среде больше, чем в другой. Характеристики среды выполнения зависят от используемых языковых компиляторов и программ. Например, разработчик Visual Basic при работе со средой CLR может заметить, что язык Visual Basic имеет больше средств объектно-ориентированного программирования, чем раньше. Среда выполнения предоставляет следующие преимущества:

- повышение производительности;

- возможность легко использовать компоненты, разработанные на других языках;

- расширяемые типы, предоставляемые библиотекой классов;

- языковые возможности (например, наследование, интерфейсы и перегрузку) для объектно-ориентированного программирования;

- поддержку явной свободной потоковой обработки, позволяющую создавать масштабируемые многопотоковые приложения;

- поддержку структурированной обработки исключений;

- поддержку настраиваемых атрибутов;

- сборка мусора;

- использование делегатов вместо указателей на функции для повышения типобезопасности и уровня защиты. Подробнее о делегатах см. в разделе [Система общих типов CTS](../../docs/standard/base-types/common-type-system.md).

## <a name="clr-versions"></a>Версии CLR

Номер версии платформы .NET Framework не всегда соответствует номеру версии среды CLR, которую он содержит. В следующей таблице показано, как они соотносятся:

|Версия платформы .NET Framework|Содержит версию среды CLR|
|----------------------------|--------------------------|
|1.0|1.0|
|1.1|1.1|
|2.0|2.0|
|3.0|2.0|
|3.5|2.0|
|4|4|
|4.5 (включая 4.5.1 и 4.5.2)|4|
|4.6 (включая 4.6.1 и 4.6.2)|4|
|4.7 (включая 4.7.1 и 4.7.2)|4|
|4.8|4|

## <a name="related-topics"></a>См. также

|Заголовок|ОПИСАНИЕ|
|-----------|-----------------|
|[Процесс управляемого выполнения](managed-execution-process.md)|Описание шагов, необходимых для использования преимуществ общеязыковой среды выполнения.|
|[Автоматическое управление памятью](automatic-memory-management.md)|Описание выделения и освобождения памяти сборщиком мусора.|
|[Общие сведения о платформе .NET Framework](../framework/get-started/overview.md)|Описание ключевых понятий платформы .NET Framework: системы общих типов CTS, межъязыкового взаимодействия, управляемого выполнения, доменов приложений и сборок.|
|[Система общих типов CTS](./base-types/common-type-system.md)|Описание того, как типы объявляются, используются и контролируются в среде выполнения в рамках поддержки межъязыковой интеграции.|

## <a name="see-also"></a>См. также

- [Версии и зависимости](../framework/migration-guide/versions-and-dependencies.md)
