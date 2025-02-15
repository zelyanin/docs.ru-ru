---
title: Пошаговое руководство. Размещение содержимого Direct3D9 в WPF
ms.date: 03/30/2017
helpviewer_keywords:
- Direct3D9 [WPF interoperability], hosting Direct3D9 content
- WPF [WPF], hosting Direct3D9 content
ms.assetid: 60983736-0ab5-42cc-8b16-e9fbde261a43
ms.openlocfilehash: 2c31c044aa50a74255a61da1675037ab3d09f615
ms.sourcegitcommit: 289e06e904b72f34ac717dbcc5074239b977e707
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/17/2019
ms.locfileid: "71053455"
---
# <a name="walkthrough-hosting-direct3d9-content-in-wpf"></a>Пошаговое руководство. Размещение содержимого Direct3D9 в WPF

В этом пошаговом руководстве показано, как разместить содержимое Direct3D9 в приложении Windows Presentation Foundation (WPF).

В руководстве выполняются следующие задачи:

- Создайте проект WPF для размещения содержимого Direct3D9.

- Импортируйте содержимое Direct3D9.

- Отображение содержимого Direct3D9 с помощью <xref:System.Windows.Interop.D3DImage> класса.

 По завершении вы узнаете, как разместить содержимое Direct3D9 в приложении WPF.

## <a name="prerequisites"></a>Предварительные требования

Ниже приведены компоненты, необходимые для выполнения данного пошагового руководства.

- Visual Studio.

- Пакет SDK для DirectX 9 или более поздней версии.

- Библиотека DLL, содержащая содержимое Direct3D9 в формате, совместимом с WPF. Дополнительные сведения см. в разделе [взаимодействие WPF и Direct3D9](wpf-and-direct3d9-interoperation.md) и [пошаговое руководство. Создание содержимого Direct3D9 для размещения в WPF](walkthrough-creating-direct3d9-content-for-hosting-in-wpf.md).

## <a name="creating-the-wpf-project"></a>Создание проекта WPF

Первым шагом является создание проекта для приложения WPF.

### <a name="to-create-the-wpf-project"></a>Создание проекта WPF

Создайте новый проект приложения WPF в Visual C# с именем `D3DHost`. Дополнительные сведения см. в разделе [Пошаговое руководство: Мое первое приложение](../getting-started/walkthrough-my-first-wpf-desktop-application.md)WPF для настольных систем.

Файл MainWindow. XAML откроется в [!INCLUDE[wpfdesigner_current_short](../../../../includes/wpfdesigner-current-short-md.md)].

## <a name="importing-the-direct3d9-content"></a>Импорт содержимого Direct3D9

Содержимое Direct3D9 импортируется из неуправляемой библиотеки DLL с помощью `DllImport` атрибута.

### <a name="to-import-direct3d9-content"></a>Импорт содержимого Direct3D9

1. Откройте MainWindow.xaml.cs в редакторе кода.

2. Замените автоматически созданный код следующим кодом.

    [!code-csharp[System.Windows.Interop.D3DImage#1](~/samples/snippets/csharp/VS_Snippets_Wpf/System.Windows.Interop.D3DImage/CS/window1.xaml.cs#1)]

## <a name="hosting-the-direct3d9-content"></a>Размещение содержимого Direct3D9

Наконец, используйте <xref:System.Windows.Interop.D3DImage> класс для размещения содержимого Direct3D9.

### <a name="to-host-the-direct3d9-content"></a>Размещение содержимого Direct3D9

1. В файле MainWindow. XAML замените автоматически созданный код XAML следующим кодом XAML.

    [!code-xaml[System.Windows.Interop.D3DImage#10](~/samples/snippets/csharp/VS_Snippets_Wpf/System.Windows.Interop.D3DImage/CS/window1.xaml#10)]

2. Выполните построение проекта.

3. Скопируйте библиотеку DLL, содержащую содержимое Direct3D9, в папку bin/Debug.

4. Нажмите клавишу F5, чтобы запустить проект.

    Содержимое Direct3D9 отображается в приложении WPF.

## <a name="see-also"></a>См. также

- <xref:System.Windows.Interop.D3DImage>
- [Вопросы производительности, связанные с взаимодействием Direct3D9 и WPF](performance-considerations-for-direct3d9-and-wpf-interoperability.md)
