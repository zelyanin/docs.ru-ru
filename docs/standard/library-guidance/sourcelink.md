---
title: Source Link и библиотеки .NET
description: Практические рекомендации по использованию Source Link для повышения эффективности отладки для библиотек .NET.
author: jamesnk
ms.author: mairaw
ms.date: 01/15/2019
ms.openlocfilehash: 7530c984ce4bbe9e40362bd550bec57ac585a550
ms.sourcegitcommit: 33c8d6f7342a4bb2c577842b7f075b0e20a2fa40
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/12/2019
ms.locfileid: "70928987"
---
# <a name="source-link"></a>Source Link

Source Link — это технология, которая дает возможность разработчикам выполнять отладку исходного кода сборок .NET из NuGet. Source Link выполняется при создании пакета NuGet и внедряет метаданные системы управления версиями внутри сборки и пакета. Разработчики, загрузившие пакет и включившие Source Link в Visual Studio, могут выполнить исходный код по шагам. Source Link предоставляет метаданные системы управления версиями для создания эффективной среды отладки.

## <a name="source-link-demo"></a>Демонстрация Source Link

> [!VIDEO https://www.youtube.com/embed/gyRGhCQPkB4?start=61]

## <a name="using-source-link"></a>Использование Source Link

Инструкции по использованию Source Link можно найти в репозитории GitHub [dotnet/sourceLink](https://github.com/dotnet/sourcelink/blob/master/README.md).

Вы можете использовать [обозреватель пакетов NuGet](https://github.com/NuGetPackageExplorer/NuGetPackageExplorer), чтобы убедиться, что метаданные Source Link успешно внедрены в пакет. Проверьте наличие метаданных `Repository` с идентификатором комментария и убедитесь, что для каждой DLL-библиотеки целевого пакета есть PDB-файлы.

![Source Link в обозревателе пакетов NuGet](./media/sourcelink/nuget-package-explorer-sourcelink.png "Source Link в обозревателе пакетов NuGet")

**✔️ РЕКОМЕНДУЕТСЯ** использовать Source Link для добавления метаданных системы управления версиями в сборки и пакеты NuGet.

> [!TIP]
> Вы можете дополнительно повысить эффективность отладки путем добавления атрибутов отладчика в типы.
>
> * <xref:System.Diagnostics.DebuggerDisplayAttribute> может определять, как класс или поле будет отображаться в окнах переменных отладчика.
> * <xref:System.Diagnostics.DebuggerStepThroughAttribute> дает отладчику указание о сквозной обработке кода (вместо выполнения по шагам).
> * <xref:System.Diagnostics.DebuggerBrowsableAttribute> определяет, будет ли член отображаться в окнах переменных отладчика.

**✔️ ДОПУСТИМО.** Публикация файлов символов (`*.pdb`).

> Для повышения качества отладки библиотеки следует публиковать файлы символов, а также использовать ссылки на источник. См. дополнительные сведения о [файлах и пакетах символов](./nuget.md#symbol-packages).

>[!div class="step-by-step"]
>[Назад](dependencies.md)
>[Вперед](publish-nuget-package.md)
