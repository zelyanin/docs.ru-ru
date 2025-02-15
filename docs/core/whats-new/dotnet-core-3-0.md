---
title: Новые возможности .NET Core 3.0
description: Дополнительные сведения о новых возможностях .NET Core 3.0.
dev_langs:
- csharp
- vb
author: thraka
ms.author: adegeo
ms.date: 09/22/2019
ms.openlocfilehash: 758c8fd30fbd2e5ce8ace997005b91d6872a06d1
ms.sourcegitcommit: 559259da2738a7b33a46c0130e51d336091c2097
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/22/2019
ms.locfileid: "72773895"
---
# <a name="whats-new-in-net-core-30"></a>Новые возможности .NET Core 3.0

В этой статье описываются новые возможности в .NET Core 3.0. Одно из основных усовершенствований — это поддержка классических приложений Windows (только Windows). С помощью пакета SDK для .NET Core 3.0 под названием Windows Desktop можно переносить приложения Windows Forms и Windows Presentation Foundation (WPF). Следует уточнить, что компонент Windows Desktop поддерживается и включен только в Windows. Дополнительные сведения см. далее в этой статье, в разделе [Рабочий стол Windows](#windows-desktop).

В .NET Core 3.0 добавлена поддержка C# 8.0. Настоятельно рекомендуется использовать [Visual Studio 2019 версии 16.3](https://visualstudio.microsoft.com/vs/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2019) или более поздней, [Visual Studio для Mac 8.3](/visualstudio/mac/install-preview) или более поздней или [Visual Studio Code](https://code.visualstudio.com/) с последним **расширением C#** .

[Скачайте .NET Core 3.0 и начните работу](https://aka.ms/netcore3download) прямо сейчас в Windows, macOS или Linux.

Дополнительные сведения см. в [объявлении о выпуске .NET Core 3.0](https://devblogs.microsoft.com/dotnet/announcing-net-core-3-0/).

Релиз-кандидат 1 .NET Core считался корпорацией Майкрософт готовым к эксплуатации и полностью поддерживался. Если вы используете предварительную версию, необходимо перейти на версию RTM, чтобы продолжать получать поддержку.

## <a name="net-core-sdk-windows-installer"></a>Установщик пакета SDK Windows для .NET Core

Начиная с .NET Core 3.0, установщик MSI для Windows был изменен. Установщики пакетов SDK теперь обновляют дополнительные пакеты функций SDK на месте. Пакеты функций определяют *сотни* в обозначении *исправления* в номере версии. Например, в версиях **3.0._101_** и **3.0._201_** пакеты функций различаются, а в версиях **3.0._101_** и **3.0._199_**  — одинаковы. При установке пакета SDK для .NET Core **3.0._101_** пакет SDK для .NET Core **3.0._100_** , если он есть на компьютере, удаляется. Когда на тот же компьютер устанавливается пакет SDK для .NET Core **3.0._200_** , пакет SDK для .NET Core **3.0._101_** удаляться не будет.

Дополнительные сведения об управлении версиями см. в разделе [Общие сведения об управлении версиями в .NET Core](../versions/index.md).

## <a name="c-80"></a>C# 8.0

В этот выпуск также входит версия языка C# 8.0, в которой появились ссылочные типы, допускающие значение NULL, асинхронные потоки и другие возможности. Дополнительные сведения о функциях C# 8.0 см. в разделе [Новые возможности C# 8.0](../../csharp/whats-new/csharp-8.md).

## <a name="net-standard-21"></a>.NET Standard 2.1

Несмотря на то что .NET Core 3.0 поддерживает **.NET Standard 2.1**, шаблон `dotnet new classlib` по умолчанию по-прежнему создает проект, предназначенный для **.NET Standard 2.0**. Чтобы создать проект для **.NET Standard 2.1**, откройте файл проекта и измените значение свойства `TargetFramework` на `netstandard2.1`:

```xml
<Project Sdk="Microsoft.NET.Sdk">

  <PropertyGroup>
    <TargetFramework>netstandard2.1</TargetFramework>
  </PropertyGroup>

</Project>
```

Если вы используете Visual Studio, у вас должна быть версия [Visual Studio 2019](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2019), так как Visual Studio 2017 не поддерживает **.NET Standard 2.1** или **.NET Core 3.0**.

## <a name="improved-net-core-version-apis"></a>Улучшенные API версий .NET Core

Начиная с .NET Core 3.0, API версий, предоставляемые с .NET Core, возвращают те данные, которые должны. Например:

```csharp
System.Console.WriteLine($"Environment.Version: {System.Environment.Version}");

// Old result
//   Environment.Version: 4.0.30319.42000
//
// New result
//   Environment.Version: 3.0.0
```

```csharp
System.Console.WriteLine($"RuntimeInformation.FrameworkDescription: {System.Runtime.InteropServices.RuntimeInformation.FrameworkDescription}");

// Old result
//   RuntimeInformation.FrameworkDescription: .NET Core 4.6.27415.71
//
// New result
//   RuntimeInformation.FrameworkDescription: .NET Core 3.0.0-preview4-27615-11
```

> [!WARNING]
> Критическое изменение. С технической точки зрения это изменение является критическим, поскольку изменилась схема управления версиями.

## <a name="net-platform-dependent-intrinsics"></a>Встроенные объекты, зависимые от платформы .NET

Были добавлены API-интерфейсы, которые разрешают доступ к определенным инструкциям ЦП, ориентированным на производительность, например **SIMD** или наборы **инструкций побитовой обработки**. Эти инструкции помогут значительно улучшить производительность в некоторых сценариях, таких как эффективная параллельная обработка данных.

Там, где это возможно, библиотеки .NET начали использовать эти инструкции для повышения производительности.

Дополнительные сведения см. в разделе [Встроенные объекты, зависимые от платформы .NET](https://github.com/dotnet/designs/blob/master/accepted/platform-intrinsics.md).

## <a name="default-executables"></a>Исполняемые файлы по умолчанию

.NET Core теперь по умолчанию собирает [исполняемые файлы, зависимые от платформы](../deploying/index.md#framework-dependent-executables-fde). Такое поведение ново для приложений, которые используют глобально установленную версию .NET Core. Раньше исполняемые файлы создавались только в [автономных развертываниях](../deploying/index.md#self-contained-deployments-scd).

Во время выполнения команды `dotnet build` или `dotnet publish` создается исполняемый файл, который соответствует среде и платформе используемого пакета SDK. Предполагается, что с этими исполняемыми файлами можно выполнять те же действия, что и с другими исполняемыми файлами в машинном коде, например:

- исполняемый файл можно дважды щелкнуть;
- приложение можно запустить из командной строки напрямую, например `myapp.exe` в Windows и `./myapp` в Linux и macOS.

## <a name="single-file-executables"></a>Однофайловые исполняемые файлы

Команда `dotnet publish` поддерживает упаковку приложения в однофайловый исполняемый файл для конкретной платформы. Исполняемый файл является самоизвлекаемым и содержит все зависимости (включая машинные), необходимые для запуска приложения. При первом запуске приложение извлекается в каталог, который зависит от имени и идентификатора сборки приложения. Впоследствии запуск происходит быстрее. Если версия не изменилась, приложению не нужно извлекать себя заново.

Чтобы опубликовать однофайловый исполняемый файл, задайте `PublishSingleFile` в своем проекте или в командной строке с помощью команды `dotnet publish`:

```xml
<PropertyGroup>
  <RuntimeIdentifier>win10-x64</RuntimeIdentifier>
  <PublishSingleFile>true</PublishSingleFile>
</PropertyGroup>
```

-или-

```dotnetcli
dotnet publish -r win10-x64 -p:PublishSingleFile=true
```

Дополнительные сведения о публикации однофайловых исполняемых файлов см. в [документе о разработке однофайловых пакетных установщиков](https://github.com/dotnet/designs/blob/master/accepted/single-file/design.md).

## <a name="assembly-linking"></a>Связывание сборок

Пакет SDK для .NET Core 3.0 содержит инструмент, который позволяет уменьшить размер приложения, анализируя промежуточный язык и устраняя неиспользуемые сборки.

Автономные приложения включают все необходимое для выполнения кода. Это позволяет не устанавливать .NET на соответствующем компьютере. Но во многих случаях для работы приложения достаточно небольшого набора функций платформы, а все неиспользуемые библиотеки можно удалить.

Теперь .NET Core предоставляет параметр, который позволяет использовать [компоновщик IL](https://github.com/mono/linker) для сканирования кода приложения на промежуточном языке. Это средство отслеживает необходимый код, а затем удаляет все неиспользуемые библиотеки. Это позволяет значительно снизить размер развертывания для некоторых приложений.

Чтобы включить этот инструмент, укажите в проекте параметр `<PublishTrimmed>` и опубликуйте автономное приложение:

```xml
<PropertyGroup>
  <PublishTrimmed>true</PublishTrimmed>
</PropertyGroup>
```

```dotnetcli
dotnet publish -r <rid> -c Release
```

Например, простейший шаблон консольного приложения hello world, который входит в стандартную поставку, при публикации достигает размера около 70 МБ. С помощью `<PublishTrimmed>` этот размер можно снизить до 30 МБ.

Важно учитывать, что приложения и платформы (в том числе ASP.NET Core и WPF), которые используют отражение или связанные динамические функции, могут стать неработоспособными после обрезки. Такие нарушения возникают потому, что компоновщик ничего не знает о динамическом поведении и не может определить, какие типы платформы потребуются для отражения. Но вы можете настроить компоновщик IL так, чтобы он учитывал этот сценарий.

В любом случае обязательно протестируйте приложение после обрезки.

Дополнительные сведения о компоновщике IL вы найдете в [этой документации](https://aka.ms/dotnet-illink) или на страницах репозитория [mono/linker]( https://github.com/mono/linker).

## <a name="tiered-compilation"></a>Многоуровневая компиляция

[Многоуровневая компиляция](https://devblogs.microsoft.com/dotnet/tiered-compilation-preview-in-net-core-2-1/) (МК) по умолчанию входит только в .NET Core 3.0. Эта функция позволяет среде выполнения более адаптивно использовать компилятор JIT для повышения производительности.

Основное преимущество МК состоит в том, что с ее помощью можно использовать методы JIT и выбирать более простой и при этом более медленный код или более сложный код, который лучше работает. Это позволяет повысить производительность приложения на разных этапах его выполнения, с первого запуска и до достижения стабильной работы. Если же МК не используется, каждый метод компилируется одним и тем же способом (как уровень высокого качества), который опирается на стабильную работу после запуска.

Чтобы включить быстрый JIT (код JIT уровня 0), добавьте в файл проекта следующий параметр:

```xml
<PropertyGroup>
  <TieredCompilationQuickJit>true</TieredCompilationQuickJit>
</PropertyGroup>
```

Чтобы полностью отключить МК, добавьте в файл проекта следующий параметр:

```xml
<TieredCompilation>false</TieredCompilation>
```

## <a name="readytorun-images"></a>Образы ReadyToRun

Вы можете снизить время запуска приложения .NET Core, скомпилировав все сборки приложения в формат ReadyToRun (R2R). R2R является разновидностью компиляции AOT.

Бинарные файлы R2R повышают производительность при запуске, снижая объем работы, выполняемой на этом этапе компилятором JIT. Бинарные файлы содержат такой же машинный код, который создается компилятором JIT. Но бинарные файлы R2R имеют больший размер, так как содержат не только код на промежуточном языке (IL), который по-прежнему необходим для некоторых сценариев, но и версию того же кода на машинном языке. Функция R2R доступна только при публикации автономного приложения, предназначенного для конкретной среды выполнения (RID), например для Windows x64 или Linux x64.

Чтобы скомпилировать проект как ReadyToRun, выполните следующие действия:

01. Добавьте в проект параметр `<PublishReadyToRun>`

    ```xml
    <PropertyGroup>
      <PublishReadyToRun>true</PublishReadyToRun>
    </PropertyGroup>
    ```

01. Опубликуйте автономное приложение. Например, такая команда создает автономное приложение для 64-разрядной версии Windows:

    ```dotnetcli
    dotnet publish -c Release -r win-x64 --self-contained true
    ```

### <a name="cross-platformarchitecture-restrictions"></a>Ограничения при работе с несколькими платформами и архитектурами

Компилятор ReadyToRun пока не поддерживает перекрестное нацеливание. Компиляцию необходимо выполнять в конкретной целевой среде. Например, если вам нужен образ R2R для 64-разрядной ОС Windows, команду публикации следует выполнять именно в этой среде.

Исключения для кроссплатформенного таргетирования:

- можно использовать Windows x64 для компиляции образов Windows ARM32, ARM64 и x86;
- можно использовать Windows x86 для компиляции образов Windows ARM32;
- можно использовать Linux x64 для компиляции образов Linux ARM32 и ARM64.

## <a name="build-copies-dependencies"></a>Сборка копирует зависимости

Команда `dotnet build` копирует зависимости NuGet для вашего приложения из кэша NuGet в выходную папку сборки. Ранее зависимости копировались только в рамках выполнения команды `dotnet publish`.

Для некоторых операций, таких как связывание и публикация страницы Razor, по-прежнему будет требоваться публикация.

## <a name="local-tools"></a>Локальные средства

В .NET Core 3.0 появились локальные средства. Локальные средства похожи на [глобальные средства](../tools/global-tools.md), но связаны с определенным расположением на диске. Локальные средства недоступны глобально и распространяются в виде пакетов NuGet.

> [!WARNING]
> Если вы пробовали использовать локальные средства в предварительной версии 1 .NET Core 3.0, например запуск `dotnet tool restore` или `dotnet tool install`, удалите папку кэша локальных средств. В противном случае локальные средства не будут работать ни в одной более новой версии. Эта папка находится по адресу:
>
> В macOS, Linux: `rm -r $HOME/.dotnet/toolResolverCache`
>
> В Windows: `rmdir /s %USERPROFILE%\.dotnet\toolResolverCache`

Локальные средства используют имя файла манифеста `dotnet-tools.json` в текущем каталоге. Этот файл манифеста определяет, какие средства доступны в этой папке и далее. Файл манифеста можно распространять вместе с кодом, чтобы те же средства мог восстановить и использовать любой, кто работает с вашим кодом.

Для глобальных и локальных средств требуется совместимая версия среды выполнения. Сейчас на сайте NuGet.org многие средства предназначены для среды выполнения .NET Core 2.1. Чтобы установить эти средства глобально или локально, нужно, как и раньше, установить [среду выполнения NET Core 2.1](https://dotnet.microsoft.com/download/dotnet-core/2.1).

## <a name="major-version-roll-forward"></a>Накат основной версии

В .NET Core 3.0 появилась функция согласия, которая позволяет приложению выполнять накат до последней основной версии .NET Core. Кроме того, добавлен новый параметр для управления тем, как накат применяется к приложению. Его можно настроить одним из следующих способов:

- Свойство файла проекта: `RollForward`
- Свойство файла конфигурации среды выполнения: `rollForward`
- Переменная среды: `DOTNET_ROLL_FORWARD`
- Аргумент командной строки: `--roll-forward`

Необходимо указать одно из следующих значений. Если параметр не указан, ему по умолчанию присваивается значение **Minor**.

- **LatestPatch**\
Накат до версии с наибольшим номером исправления. Отключает накат дополнительных версий.
- **Minor**\
Накат до дополнительной версии со следующим по порядку возрастания номером, если запрошенная дополнительная версия отсутствует. Если запрошенная дополнительная версия присутствует, используется политика **LatestPatch**.
- **Major**\
Накат до основной версии со следующим по порядку возрастания или дополнительной версии с наименьшим номером, если запрошенная дополнительная версия отсутствует. Если запрошенная основная версия присутствует, используется политика **Minor**.
- **LatestMinor**\
Накат до дополнительной версии с наибольшим номером, даже если запрошенная дополнительная версия присутствует. Предназначен для сценариев размещения компонентов.
- **LatestMajor**\
Накат до основной версии с наибольшим номером и дополнительной версии с наибольшим номером, даже если запрошенная основная версия присутствует. Предназначен для сценариев размещения компонентов.
- **Disable**\
Накат не выполняется. Привязка только к указанной версии. Эта политика не рекомендуется для общего использования, поскольку отключает возможность наката до последних исправлений. Это значение рекомендуется использовать только для тестирования.

Все параметры, кроме параметра **Disable**, будут использовать версию с последним доступным исправлением.

## <a name="windows-desktop"></a>Классические приложения Windows

.NET Core 3.0 поддерживает классические приложения Windows с помощью Windows Presentation Foundation (WPF) и Windows Forms. Эти платформы также поддерживают использование современных элементов управления и стилей Fluent из библиотеки XAML пользовательского интерфейса Windows (WinUI) через [острова XAML](/windows/uwp/xaml-platform/xaml-host-controls).

Компонент Windows Desktop является частью пакета SDK .NET Core 3.0 для Windows.

Вы можете создать приложение WPF или Windows Forms с помощью следующих команд `dotnet`:

```dotnetcli
dotnet new wpf
dotnet new winforms
```

Visual Studio 2019 добавляет шаблоны **Новый проект** для .NET Core 3.0 Windows Forms и WPF.

Дополнительные сведения о переносе существующего приложения .NET Framework см. в разделах [Перенос проектов WPF](../porting/wpf.md) и [Перенос проектов Windows Forms](../porting/winforms.md).

## <a name="com-callable-components---windows-desktop"></a>Компоненты, вызываемые COM — Windows Desktop

Теперь в Windows можно создавать управляемые компоненты, вызываемые COM. Эта возможность необходима для использования .NET Core с моделями надстроек COM и обеспечивает соответствие с платформой .NET Framework.

В отличие от .NET Framework, где в качестве сервера использовалась библиотека *mscoree.dll*, .NET Core при сборке вашего COM-компонента добавляет в каталог *bin* dll собственного средства запуска.

Пример того, как создать и использовать компонент COM, см. в разделе [Демонстрация COM](https://github.com/dotnet/samples/tree/master/core/extensions/COMServerDemo).

## <a name="msix-deployment---windows-desktop"></a>Развертывание MSIX — Windows Desktop

[MSIX](https://docs.microsoft.com/windows/msix/) — это новый формат пакета приложения Windows. Его можно использовать для развертывания классических приложений .NET Core 3.0 для Windows 10.

[Проект упаковки приложений Windows](https://docs.microsoft.com/windows/uwp/porting/desktop-to-uwp-packaging-dot-net), доступный в Visual Studio 2019, позволяет создавать пакеты MSIX с [автономными](../deploying/index.md#self-contained-deployments-scd) приложениями .NET Core.

Файл проекта .NET Core должен указывать поддерживаемые среды выполнения в свойстве `<RuntimeIdentifiers>`:

```xml
<RuntimeIdentifiers>win-x86;win-x64</RuntimeIdentifiers>
```

## <a name="winforms-high-dpi"></a>Высокое разрешение для WinForms

Приложения .NET Core Windows Forms могут устанавливать режим высокого разрешения экрана с помощью <xref:System.Windows.Forms.Application.SetHighDpiMode(System.Windows.Forms.HighDpiMode)?displayProperty=nameWithType>. Метод `SetHighDpiMode` задает соответствующий режим высокого разрешения, если параметр не задан другими способами, например с помощью `App.Manifest` или P/Invoke перед `Application.Run`.

Возможны следующие значения `highDpiMode`, выраженные перечислением <xref:System.Windows.Forms.HighDpiMode?displayProperty=nameWithType>:

- `DpiUnaware`
- `SystemAware`
- `PerMonitor`
- `PerMonitorV2`
- `DpiUnawareGdiScaled`

См. о [разработке классических приложений с поддержкой высокого разрешения экрана в Windows](/windows/desktop/hidpi/high-dpi-desktop-application-development-on-windows).

## <a name="ranges-and-indices"></a>Диапазоны и индексы

Новый тип <xref:System.Index?displayProperty=nameWithType> можно использовать для индексирования. Вы можете создать с помощью `int` индекс, который отсчитывается с начала, а с помощью оператора `^` префикса (C#) индекс, который отсчитывается с конца:

```csharp
Index i1 = 3;  // number 3 from beginning
Index i2 = ^4; // number 4 from end
int[] a = { 0, 1, 2, 3, 4, 5, 6, 7, 8, 9 };
Console.WriteLine($"{a[i1]}, {a[i2]}"); // "3, 6"
```

Кроме того, есть тип <xref:System.Range?displayProperty=nameWithType>, который состоит из двух значений `Index` (одно для начала и одно для конца) и который можно написать с помощью выражения диапазона `x..y` (C#). После этого можно выполнить индексацию с помощью команды `Range`, которая создает срез:

```csharp
var slice = a[i1..i2]; // { 3, 4, 5 }
```

Дополнительные сведения см. в [руководстве по диапазонам и индексам](../../csharp/tutorials/ranges-indexes.md).

## <a name="async-streams"></a>Асинхронные потоки

Тип <xref:System.Collections.Generic.IAsyncEnumerable%601> — это новая асинхронная версия <xref:System.Collections.Generic.IEnumerable%601>. Язык позволяет выполнить действие `await foreach` с этими объектами `IAsyncEnumerable<T>`, чтобы использовать их элементы, и действие `yield return` по отношению к ним, чтобы создать элементы.

В приведенном ниже примере демонстрируется создание и применение асинхронных потоков. Инструкция `foreach` является асинхронной и использует `yield return`, чтобы создать асинхронные потоки для вызывающих объектов. Этот шаблон (с использованием `yield return`) является рекомендуемой моделью для создания асинхронных потоков.

```csharp
async IAsyncEnumerable<int> GetBigResultsAsync()
{
    await foreach (var result in GetResultsAsync())
    {
        if (result > 20) yield return result;
    }
}
```

Помимо возможности `await foreach` вы также можете создать асинхронные итераторы, например, итератор, который возвращает `IAsyncEnumerable/IAsyncEnumerator`, для которого можно применить `await` и `yield`. Для объектов, которые необходимо удалить, можно использовать `IAsyncDisposable`, который реализовывают различные типы BCL, такие как `Stream` и `Timer`.

Дополнительные сведения см. в [руководстве по асинхронным потокам](../../csharp/tutorials/generate-consume-asynchronous-stream.md).

## <a name="ieee-floating-point-improvements"></a>Усовершенствования чисел с плавающей запятой по IEEE

API плавающей запятой сейчас обновляются, чтобы соответствовать [редакции IEEE 754-2008](https://en.wikipedia.org/wiki/IEEE_754-2008_revision). Цель этих изменений — предоставлять все **обязательные** операции и гарантировать, что их поведение будет соответствовать спецификации IEEE. Дополнительные сведения об улучшениях, связанных с плавающей запятой, см. в записи блога [Улучшения в синтаксическом анализе и форматировании плавающей запятой в .NET Core 3.0](https://devblogs.microsoft.com/dotnet/floating-point-parsing-and-formatting-improvements-in-net-core-3-0/).

Исправления синтаксического анализа и форматирования включают:

- Правильный анализ и округление входных данных любой длины.
- Правильный анализ и форматирование отрицательного нуля.
- Правильный анализ `Infinity` и `NaN` с помощью проверки без учета регистра и допущения необязательного `+` в начале, где это применимо.

Новый API <xref:System.Math?displayProperty=nameWithType> включают:

- <xref:System.Math.BitIncrement(System.Double)> и <xref:System.Math.BitDecrement(System.Double)>\
Соответствует операциям IEEE `nextUp` и `nextDown`. Они возвращают наименьшее число с плавающей запятой, которое может быть больше или меньше входных данных (соответственно). Например, `Math.BitIncrement(0.0)` вернет `double.Epsilon`.

- <xref:System.Math.MaxMagnitude(System.Double,System.Double)> и <xref:System.Math.MinMagnitude(System.Double,System.Double)>\
Соответствуют операциям IEEE `maxNumMag` и `minNumMag`. Они возвращают значение, которое будет больше или меньше из двух величин (соответственно). Например, `Math.MaxMagnitude(2.0, -3.0)` вернет `-3.0`.

- <xref:System.Math.ILogB(System.Double)>\
Соответствует операции IEEE `logB`, которая возвращает целочисленное значение. Она возвращает целочисленный логарифм по основанию 2 входного параметра. Этот метод действует практически так же, как `floor(log2(x))`, но с минимальными погрешностями округления.

- <xref:System.Math.ScaleB(System.Double,System.Int32)>\
Соответствует операции IEEE `scaleB`, которая принимает целочисленное значение. Возвращает `x * pow(2, n)`, но выполняется с минимальными погрешностями округления.

- <xref:System.Math.Log2(System.Double)>\
Соответствует операции IEEE `log2`. Возвращает логарифм по основанию 2. Сводит к минимуму погрешность округления.

- <xref:System.Math.FusedMultiplyAdd(System.Double,System.Double,System.Double)>\
Соответствует операции IEEE `fma`. Выполняет умножение и сложение. То есть он выполняет `(x * y) + z` как одну операцию, тем самым сводя к минимуму погрешность округления. Пример — `FusedMultiplyAdd(1e308, 2.0, -1e308)`, возвращающий `1e308`. Стандартный `(1e308 * 2.0) - 1e308` возвращает `double.PositiveInfinity`.

- <xref:System.Math.CopySign(System.Double,System.Double)>\
Соответствует операции IEEE `copySign`. Возвращает значение `x`, но со знаком `y`.

## <a name="fast-built-in-json-support"></a>Быстрая встроенная поддержка JSON

Пользователи .NET в основном полагались на [**Json.NET**](https://www.newtonsoft.com/json) и другие популярные библиотеки JSON, которые по-прежнему остаются хорошими вариантами. **Json.NET** использует в качестве базового типа данных строки .NET, которые обладают внутренней структурой UTF-16.

Новая встроенная поддержка JSON отличается высокой производительностью и малым распределением и основана на `Span<byte>`. Были добавлены три новых основных типа, связанных с JSON, в пространство имен <xref:System.Text.Json?displayProperty=nameWithType> в .NET Core 3.0. Эти типы *пока* не поддерживают сериализацию и десериализацию POCO CLR.

### <a name="utf8jsonreader"></a>Utf8JsonReader

<xref:System.Text.Json.Utf8JsonReader?displayProperty=nameWithType> — это однопроходный модуль чтения текста JSON в кодировке UTF-8 из `ReadOnlySpan<byte>` с высокой производительностью и низким уровнем распределения. `Utf8JsonReader` — это основной тип низкого уровня, с помощью которого можно создавать пользовательские средства синтаксического анализа и десериализаторы. Чтение полезных данных JSON с помощью нового `Utf8JsonReader` осуществляется в два раза быстрее, чем при использовании модуля чтения от **JSON.NET**. Распределение не осуществляется, пока не понадобится актуализировать токены JSON в виде строк (UTF-16).

Ниже приведен пример чтения с помощью файла [ **launch.json**](https://github.com/dotnet/samples/blob/master/snippets/core/whats-new/whats-new-in-30/cs/launch.json), созданного Visual Studio Code.

[!CODE-csharp[Utf8JsonReader](~/samples/snippets/core/whats-new/whats-new-in-30/cs/program.cs#PrintJson)]

[!CODE-csharp[Utf8JsonReader](~/samples/snippets/core/whats-new/whats-new-in-30/cs/program.cs#PrintJsonCall)]

### <a name="utf8jsonwriter"></a>Utf8JsonWriter

<xref:System.Text.Json.Utf8JsonWriter?displayProperty=nameWithType> предоставляет высокопроизводительный, не использующий кэширование и однонаправленный способ записи текста JSON в кодировке UTF-8 из распространенных типов .NET, например `String`, `Int32` и `DateTime`. Как и средство чтения, средство записи — это основной тип низкого уровня, с помощью которого можно создавать пользовательские сериализаторы. Запись полезных данных JSON с помощью нового `Utf8JsonWriter` выполняется на 30–80 % быстрее, чем с помощью средства записи из **Json.NET**, и не требует выделения.

### <a name="jsondocument"></a>JsonDocument

<xref:System.Text.Json.JsonDocument?displayProperty=nameWithType> создается на основе `Utf8JsonReader`. `JsonDocument` предоставляет возможность анализировать данные JSON и создавать модель DOM только для чтения, которая может запрашиваться для поддержки случайного доступа и перечисления. Доступ к элементам JSON, составляющим данные, может осуществляться через тип <xref:System.Text.Json.JsonElement>, который `JsonDocument` предоставляет как свойство с именем `RootElement`. `JsonElement` содержит перечислители массива и объекта JSON вместе с API-интерфейсами для преобразования текста JSON в стандартные типы .NET. Синтаксический анализ типичных полезных данных JSON и доступ ко всем членам с помощью `JsonDocument` выполняется в 2–3 раза быстрее, чем **Json.NET**, с небольшим количеством распределений данных приемлемого размера (т. е. < 1 МБ).

Ниже приведен пример использования `JsonDocument` и `JsonElement`, который может использоваться в качестве отправной точки:

[!CODE-csharp[JsonDocument](~/samples/snippets/core/whats-new/whats-new-in-30/cs/program.cs#ReadJson)]

Ниже приведен пример чтения в C# 8.0 с помощью файла [ **launch.json**](https://github.com/dotnet/samples/blob/master/snippets/core/whats-new/whats-new-in-30/cs/launch.json), созданного Visual Studio Code:

[!CODE-csharp[JsonDocument](~/samples/snippets/core/whats-new/whats-new-in-30/cs/program.cs#ReadJsonCall)]

### <a name="jsonserializer"></a>JsonSerializer

<xref:System.Text.Json.JsonSerializer?displayProperty=nameWithType> создается на основе <xref:System.Text.Json.Utf8JsonReader> и <xref:System.Text.Json.Utf8JsonWriter>, а также позволяет при работе с JSON-документами и фрагментами выполнять быструю сериализацию с минимальной нагрузкой на ресурсы памяти.

Пример сериализации объекта в JSON:

[!CODE-csharp[JsonSerializer](~/samples/snippets/core/whats-new/whats-new-in-30/cs/JSON.cs#JsonSerialize)]

Ниже приведен пример десериализации строки JSON в объект. Можно использовать строку JSON, созданную в предыдущем примере:

[!CODE-csharp[JsonDeserializer](~/samples/snippets/core/whats-new/whats-new-in-30/cs/JSON.cs#JsonDeserialize)]

## <a name="interop-improvements"></a>Улучшения в совместимости

В .NET Core 3.0 улучшена совместимость собственного API.

### <a name="type-nativelibrary"></a>Тип: NativeLibrary

<xref:System.Runtime.InteropServices.NativeLibrary?displayProperty=nameWithType> обеспечивает инкапсуляцию для загрузки собственной библиотеки (используя ту же логику загрузки, что и .NET Core P/Invoke) и предоставления соответствующих вспомогательных функций, таких как `getSymbol`. Пример кода см. в разделе [Демонстрация DLLMap](https://github.com/dotnet/samples/tree/master/core/extensions/DllMapDemo).

### <a name="windows-native-interop"></a>Собственное взаимодействие Windows

Windows предоставляет собственный API с широкими возможностями в виде API C, COM и WinRT. При том что .NET Core поддерживает **P/Invoke**, в .NET Core 3.0 добавлена возможность **воссоздавать API COM** и **активировать API WinRT**. Пример кода см. в разделе [Демонстрация Excel](https://github.com/dotnet/samples/tree/master/core/extensions/ExcelDemo).

## <a name="http2-support"></a>Поддержка HTTP/2

Тип <xref:System.Net.Http.HttpClient?displayProperty=nameWithType> поддерживает протокол HTTP/2. Если протокол HTTP/2 включен, версия протокола HTTP согласуется через TLS/ALPN, а HTTP/2 используется, только если его выбрал сервер.

Протоколом по умолчанию остается HTTP/1.1, но у вас есть два способа включить HTTP/2. Во-первых, можно указать использование HTTP/2 в заголовке запроса:

[!CODE-csharp[Http2Request](~/samples/snippets/core/whats-new/whats-new-in-30/cs/http.cs#Request)]

Во-вторых, можно изменить <xref:System.Net.Http.HttpClient> для использования HTTP/2 по умолчанию:

[!CODE-csharp[Http2Client](~/samples/snippets/core/whats-new/whats-new-in-30/cs/http.cs#Client)]

При разработке приложений часто требуется подключение без шифрования. Если вы знаете, что целевая конечная точка использует протокол HTTP/2, вы можете включить незашифрованные подключения для HTTP/2. Для этого задайте переменной среды `DOTNET_SYSTEM_NET_HTTP_SOCKETSHTTPHANDLER_HTTP2UNENCRYPTEDSUPPORT` значение `1` или включите этот параметр в контексте приложения:

[!CODE-csharp[Http2Context](~/samples/snippets/core/whats-new/whats-new-in-30/cs/http.cs#AppContext)]

## <a name="tls-13--openssl-111-on-linux"></a>TLS 1.3 и OpenSSL 1.1.1 в Linux

Теперь .NET Core использует преимущества [поддержки TLS 1.3 в OpenSSL 1.1.1](https://www.openssl.org/blog/blog/2018/09/11/release111/), когда она доступна в данной среде. В TLS 1.3:

- За счет уменьшения количества круговых путей между клиентом и сервером уменьшено время соединения.
- За счет удаления различных устаревших и небезопасных алгоритмов шифрования повышена безопасность.

В системе Linux .NET Core 3.0 по возможности использует **OpenSSL 1.1.1**, **OpenSSL 1.1.0** или **OpenSSL 1.0.2**. Если есть доступ к **OpenSSL 1.1.1**, оба типа, <xref:System.Net.Security.SslStream?displayProperty=nameWithType> и <xref:System.Net.Http.HttpClient?displayProperty=nameWithType>, используют **TLS 1.3** (при условии что клиент и сервер поддерживают **TLS 1.3**).

>[!IMPORTANT]
>Windows и macOS еще не поддерживают **TLS 1.3**. .NET Core 3.0 будет поддерживать **TLS 1.3** в этих операционных системах, когда поддержка станет доступной.

В следующем примере C# 8.0 показано, как .NET Core 3.0 в Ubuntu 18.10 подключается к <https://www.cloudflare.com>:

[!CODE-csharp[TLSExample](~/samples/snippets/core/whats-new/whats-new-in-30/cs/TLS.cs#TLS)]

## <a name="cryptography-ciphers"></a>Шифры криптографии

В .NET 3.0 добавлена поддержка шифров **AES-GCM** и **AES-CCM**, реализованных с помощью <xref:System.Security.Cryptography.AesGcm?displayProperty=nameWithType> и <xref:System.Security.Cryptography.AesCcm?displayProperty=nameWithType> соответственно. Оба этих алгоритма представляют собой [алгоритмы AEAD (аутентифицированного шифрования с присоединенными данными)](https://en.wikipedia.org/wiki/Authenticated_encryption).

В следующем коде показано использование шифра `AesGcm` для шифрования и расшифровки случайных данных.

[!CODE-csharp[AesGcm](~/samples/snippets/core/whats-new/whats-new-in-30/cs/Cipher.cs#AesGcm)]

## <a name="cryptographic-key-importexport"></a>Импорт и экспорт криптографических ключей

.NET Core 3.0 поддерживает импорт и экспорт асимметричных открытых и закрытых ключей из стандартных форматов. Сертификат X.509 использовать не нужно.

Все типы ключей, включая *RSA*, *DSA*, *ECDsa* и *ECDiffieHellman*, поддерживают следующие форматы:

- **Открытый ключ**
  - X.509 SubjectPublicKeyInfo

- **Закрытый ключ**
  - PKCS#8 PrivateKeyInfo
  - PKCS#8 EncryptedPrivateKeyInfo

Ключи RSA также поддерживают:

- **Открытый ключ**
  - PKCS#1 RSAPublicKey

- **Закрытый ключ**
  - PKCS#1 RSAPrivateKey

Методы экспорта создают двоичные данные в кодировке DER, а методы импорта выполняют то же самое. Если ключ хранится в формате PEM в виде текста, вызывающему объекту потребуется выполнить декодирование содержимого в формате base64, прежде чем вызывать метод импорта.

[!CODE-csharp[RSA](~/samples/snippets/core/whats-new/whats-new-in-30/cs/RSA.cs#Rsa)]

Файлы **PKCS 8** можно проверять с помощью <xref:System.Security.Cryptography.Pkcs.Pkcs8PrivateKeyInfo?displayProperty=nameWithType>, а файлы **PFX/PKCS #12** — с помощью <xref:System.Security.Cryptography.Pkcs.Pkcs12Info?displayProperty=nameWithType>. Для манипуляций с файлами **PFX/PKCS #12** можно использовать <xref:System.Security.Cryptography.Pkcs.Pkcs12Builder?displayProperty=nameWithType>.

## <a name="serialport-for-linux"></a>SerialPort для Linux

.NET Core 3.0 обеспечивает базовую поддержку <xref:System.IO.Ports.SerialPort?displayProperty=nameWithType> в Linux.

Раньше среда .NET Core поддерживала `SerialPort` только в Windows.

Дополнительные сведения об ограниченной поддержке последовательного порта в Linux см. в описании [проблемы № 33146 на сайте GitHub](https://github.com/dotnet/corefx/issues/33146).

## <a name="docker-and-cgroup-memory-limits"></a>Ограничения памяти в Docker и cgroup

Запуск .NET Core 3.0 на платформе Linux с помощью Docker лучше работает с ограничениями памяти в cgroup. Запуск контейнера Docker с ограничениями памяти, например с `docker run -m`, изменяет поведение .NET Core.

- Размер кучи сборщика мусора (GC) по умолчанию составляет не более 20 МБ или 75 % от ограничения памяти для контейнера.
- Конкретный размер можно указать абсолютным числом или в виде процента от ограничения cgroup.
- Минимальный зарезервированный размер сегмента в куче GC составляет 16 МБ. При использовании такого размера количество создаваемых на компьютерах куч будет меньше.

## <a name="smaller-garbage-collection-heap-sizes"></a>Уменьшенные размеры куч сборки мусора

Размер кучи сборщика мусора по умолчанию был уменьшен, так что теперь .NET Core использует меньше памяти. Это изменение лучше соответствует выделению бюджета нулевого поколения и размерам кэша современных процессоров.

## <a name="garbage-collection-large-page-support"></a>Поддержка больших страниц сборки мусора

Большие страницы (также известные как огромные страницы в Linux) — это функция, благодаря которой операционная система может задавать области памяти больше, чем размер нативной страницы (часто 4K), чтобы повысить производительность приложения, запрашивающего такие страницы.

Сборщик мусора теперь можно настраивать с помощью параметра **GCLargePages** как дополнительной функции, позволяющей выделять большие страницы в Windows.

## <a name="gpio-support-for-raspberry-pi"></a>Поддержка GPIO для Raspberry Pi

В NuGet выпущено два новых пакета, которые можно использовать для программирования GPIO:

- [System.Device.Gpio](https://www.nuget.org/packages/System.Device.Gpio)
- [Iot.Device.Bindings](https://www.nuget.org/packages/Iot.Device.Bindings)

Пакеты GPIO содержат API для устройств *GPIO*, *SPI*, *I2C* и *PWM*. Пакет привязок Интернета вещей содержит привязки устройств. Дополнительные сведения см. в [репозитории GitHub устройств](https://github.com/dotnet/iot/blob/master/src/devices/).

## <a name="arm64-linux-support"></a>Поддержка ARM64 Linux

В .NET Core 3.0 добавлена поддержка ARM64 для Linux. Основной вариант использования для ARM64 в данный момент — это сценарии Интернета вещей. Дополнительные сведения см. в статье [Состояние .NET Core в ARM64](https://github.com/dotnet/announcements/issues/82).

[Образы Docker для .NET Core в ARM64](https://hub.docker.com/r/microsoft/dotnet/) доступны для Alpine, Debian и Ubuntu.

> [!NOTE]
> Поддержка **ARM64** в Windows еще недоступна.
