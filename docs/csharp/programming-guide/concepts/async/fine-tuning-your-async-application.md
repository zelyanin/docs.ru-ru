---
title: Настройка асинхронного приложения (C#)
ms.date: 07/20/2015
ms.assetid: 97696eb9-81fc-4940-9655-84daa8eb4d5c
ms.openlocfilehash: a7c730992a9bbb4853b6451323e1c49bd19bdf42
ms.sourcegitcommit: 68653db98c5ea7744fd438710248935f70020dfb
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/22/2019
ms.locfileid: "69924437"
---
# <a name="fine-tuning-your-async-application-c"></a>Настройка асинхронного приложения (C#)
Методы и свойства, доступные при использовании типа <xref:System.Threading.Tasks.Task>, позволяют сделать приложение более точным и гибким. В подразделах этого раздела приводятся примеры, в которых используются <xref:System.Threading.CancellationToken> и важные методы `Task`, такие как <xref:System.Threading.Tasks.Task.WhenAll%2A?displayProperty=nameWithType> и <xref:System.Threading.Tasks.Task.WhenAny%2A?displayProperty=nameWithType>.  
  
 С помощью `WhenAny` и `WhenAll` можно легко запускать несколько задач и ожидать их завершения путем наблюдения за одной из них.  
  
- `WhenAny` возвращает задачу, которая завершается после завершения любой задачи в коллекции.  
  
     Примеры использования `WhenAny` см. в разделах [Отмена оставшихся асинхронных задач после завершения одной из них (C#)](./cancel-remaining-async-tasks-after-one-is-complete.md) и [Запуск нескольких асинхронных задач и их обработка по мере завершения (C#)](./start-multiple-async-tasks-and-process-them-as-they-complete.md).  
  
- `WhenAll` возвращает задачу, которая завершается после завершения всех задач в коллекции.  
  
     Дополнительные сведения и пример кода, использующий `WhenAll`, см. в разделе [Практический пример. Оптимизация производительности асинхронных процедур с использованием метода Task.WhenAll в C#](./how-to-extend-the-async-walkthrough-by-using-task-whenall.md).  
  
 Этот раздел содержит следующие примеры.  
  
- [Отмена асинхронной задачи или списка задач (C#)](./cancel-an-async-task-or-a-list-of-tasks.md)  
  
- [Отмена асинхронных задач после определенного периода времени (C#)](./cancel-async-tasks-after-a-period-of-time.md)  
  
- [Отмена оставшихся асинхронных задач после завершения одной из них (C#)](./cancel-remaining-async-tasks-after-one-is-complete.md)  
  
- [Запуск нескольких асинхронных задач и их обработка по мере завершения (C#)](./start-multiple-async-tasks-and-process-them-as-they-complete.md)  
  
> [!NOTE]
> Для выполнения примеров необходимо, чтобы на компьютере были установлены Visual Studio 2012 или более поздняя версия и .NET Framework 4.5 или более поздняя версия.  
  
 Проекты создают пользовательский интерфейс, содержащий кнопку, которая запускает процесс, и кнопку, которая его отменяет, как показано на следующем рисунке. Кнопки называются `startButton` и `cancelButton`.  
  
 ![Окно WPF с кнопкой "Отмена"](./media/fine-tuning-your-async-application/cancellation-and-start-button.png "Диалоговое окно с кнопками запуска и остановки")  
  
 Скачать полный проект Windows Presentation Foundation (WPF) можно со страницы [Пример асинхронности. Тонкая настройка приложения](https://code.msdn.microsoft.com/Async-Fine-Tuning-Your-a676abea).  
  
## <a name="see-also"></a>См. также

- [Асинхронное программирование с использованием ключевых слов async и await (C#)](./index.md)
