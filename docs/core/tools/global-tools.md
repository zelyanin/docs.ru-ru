---
title: Глобальные средства .NET Core
description: Обзор глобальных средств .NET Core и доступных для них команд .NET Core CLI.
author: KathleenDollard
ms.date: 05/29/2018
ms.custom: seodec18
ms.openlocfilehash: 40a0aabcf523e8dac9a3ad226064bbb3c1b3ce5b
ms.sourcegitcommit: 8b8dd14dde727026fd0b6ead1ec1df2e9d747a48
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/27/2019
ms.locfileid: "71332023"
---
# <a name="net-core-global-tools-overview"></a>Обзор глобальных средств .NET Core

[!INCLUDE [topic-appliesto-net-core-21plus.md](../../../includes/topic-appliesto-net-core-21plus.md)]

Глобальное средство .NET Core — это специальный пакет NuGet, который содержит консольное приложение. Глобальное средство можно установить на компьютере в расположении по умолчанию, которое включено в переменную среды PATH, или в другом расположении.

Процесс работы с глобальным средством .NET Core включает следующие этапы:

* поиск информации о средстве (обычно на веб-сайте или странице GitHub);
* проверку автора и статистики в корневой папке веб-канала (обычно NuGet.org);
* установку средства;
* вызов средства;
* обновление средства;
* удаление средства.

> [!IMPORTANT]
> Глобальные средства .NET Core устанавливаются по выбранному вами пути и выполняются в режиме полного доверия. Не устанавливайте глобальные средства .NET Core, если вы не доверяете автору.

## <a name="find-a-net-core-global-tool"></a>Как найти глобальное средство .NET Core

В интерфейсе командной строки (CLI) .NET Core пока нет функции поиска глобальных средств.

Глобальные средства .NET Core можно найти в [NuGet](https://www.nuget.org). При этом NuGet пока не позволяет выполнять поиск конкретных глобальных средств .NET Core.

Вы можете найти рекомендации средств в записях блога или в репозитории GitHub [natemcmaster/dotnet-tools](https://github.com/natemcmaster/dotnet-tools).

Кроме того, в репозитории GitHub [aspnet/DotNetTools](https://github.com/aspnet/DotNetTools/) вам доступен исходный код для глобальных средств, созданных командой разработчиков для ASP.NET.

## <a name="check-the-author-and-statistics"></a>Проверка автора и статистики

Поскольку глобальные средства .NET Core выполняются в режиме полного доверия и обычно устанавливаются по заданному вами пути, они имеют большие привилегии. Не скачивайте средства авторов, которым вы не доверяете.

Если средство размещается в NuGet, вы можете проверить автора и статистику, выполнив поиск средства.

## <a name="install-a-global-tool"></a>Установка глобального средства

Чтобы установить глобальное средство, используйте команду .NET Core CLI [dotnet tool install](dotnet-tool-install.md). В примере ниже показано, как установить глобальное средство в расположении по умолчанию:

```dotnetcli
dotnet tool install -g dotnetsay
```

Если не удается установить средство, отображаются сообщения об ошибках. Убедитесь, что установлены флажки для нужных веб-каналов.

Если вам необходимо установить предварительную или конкретную версию средства, можно указать номер версии, используя следующий формат:

```dotnetcli
dotnet tool install -g <package-name> --version <version-number>
```

Если установка выполнена успешно, отображается сообщение с командой, используемой для вызова средства, и установленной версией, аналогичное приведенному ниже:

```output
You can invoke the tool using the following command: dotnetsay
Tool 'dotnetsay' (version '2.0.0') was successfully installed.
```

Глобальные средства можно установить в каталоге по умолчанию или в выбранном вами расположении. Каталоги по умолчанию:

| ОС          | Путь                          |
|-------------|-------------------------------|
| Linux/macOS | `$HOME/.dotnet/tools`         |
|  Windows     | `%USERPROFILE%\.dotnet\tools` |

Эти расположения добавляются в путь пользователя при первом запуске пакета SDK, поэтому установленные в них глобальные средства можно вызывать напрямую.

Обратите внимание, что глобальные средства входят в область конкретного пользователя, а не глобальную область компьютера. Таким образом, нельзя установить глобальное средство, которое будет доступно для всех пользователей компьютера. Средство доступно только для тех профилей пользователей, в которых оно установлено.

Глобальные средства также можно установить в конкретном каталоге. При установке в конкретном каталоге необходимо убедиться, что команда доступна, включив этот каталог в путь, вызвав команду с указанным каталогом или вызвав средство из указанного каталога.
В этом случае .NET Core CLI не добавляет это расположение автоматически в переменную среды PATH.

## <a name="use-the-tool"></a>Работа со средством

После установки средства его можно вызывать с помощью команды. Обратите внимание, что команда может отличаться от имени пакета.

Если команда — `dotnetsay`, она вызывается с помощью:

```console
dotnetsay
```

Если автор средства предусмотрел его отображение в контексте запроса `dotnet`, оно может вызываться как `dotnet <command>`, например:

```dotnetcli
dotnet doc
```

Чтобы узнать, какие средства входят в установленный пакет глобального средства, можно перечислить установленные пакеты с помощью команды [dotnet tool list](dotnet-tool-list.md).

Вы также можете найти инструкции по использованию на веб-сайте средства или выполнив одну из следующих команд:

```console
<command> --help
dotnet <command> --help
```

## <a name="other-cli-commands"></a>Другие команды интерфейса командной строки

Пакет SDK для .NET Core содержит другие команды, которые поддерживают глобальные средства .NET Core. Все команды `dotnet tool` можно использовать с одним из следующих параметров:

* `--global` или `-g` указывает, что команда применяется к глобальному средству уровня пользователя;
* `--tool-path` задает пользовательское расположение для глобальных средств.

Чтобы узнать, какие команды доступны для глобальных средств, выполните следующую команду:

```dotnetcli
dotnet tool --help
```

Обновление глобального средства подразумевает его удаление и установку последней стабильной версии. Чтобы обновить глобальное средство, используйте команду [dotnet tool update](dotnet-tool-update.md):

```dotnetcli
dotnet tool update -g <packagename>
```

Удалите глобальное средство с помощью команды [dotnet tool uninstall](dotnet-tool-uninstall.md):

```dotnetcli
dotnet tool uninstall -g <packagename>
```

Чтобы отобразить все глобальные средства, установленные на компьютере (вместе со сведениями о версиях и командах), используйте команду [dotnet tool list](dotnet-tool-list.md):

```dotnetcli
dotnet tool list -g
```

## <a name="see-also"></a>См. также

* [Устранение неполадок при использовании средства .NET Core](troubleshoot-usage-issues.md)
