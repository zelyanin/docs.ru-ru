---
title: Практическое руководство. Запрос к базе данных с помощью LINQ (Visual Basic)
ms.date: 07/20/2015
helpviewer_keywords:
- query samples [LINQ]
- database querying [LINQ]
- queries [LINQ in Visual Basic], database querying
- querying databases [LINQ]
- queries [LINQ in Visual Basic], how-to topics
- query samples [Visual Basic]
ms.assetid: bcf5e9c3-a236-4399-9a7f-3991eca7cf56
ms.openlocfilehash: 3fa4434ed43e41959d6ebd3521bb1eecd041c9ab
ms.sourcegitcommit: 0be8a279af6d8a43e03141e349d3efd5d35f8767
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/18/2019
ms.locfileid: "59326116"
---
# <a name="how-to-query-a-database-by-using-linq-visual-basic"></a>Практическое руководство. Запрос к базе данных с помощью LINQ (Visual Basic)
Language-Integrated Query (LINQ) позволяет легко получить доступ к информации базы данных и выполнения запросов.  
  
 В следующем примере показано, как создать новое приложение, которое выполняет запросы к базе данных SQL Server.  
  
 В примерах в этом разделе используется образец базы данных "Борей". Если у вас нет этой базы данных на компьютере разработчика, его можно загрузить из центра загрузки Майкрософт. Инструкции см. в разделе [Загрузка примеров баз данных](../../../../framework/data/adonet/sql/linq/downloading-sample-databases.md).  
  
[!INCLUDE[note_settings_general](~/includes/note-settings-general-md.md)]  
  
## <a name="to-create-a-connection-to-a-database"></a>Чтобы создать подключение к базе данных  
  
1. В Visual Studio откройте **обозревателя серверов**/**обозреватель баз данных** , щелкнув **обозревателя серверов**/**базы данных Обозреватель** на **представление** меню.  
  
2. Щелкните правой кнопкой мыши **подключения к данным** в **обозревателя серверов**/**обозреватель баз данных** и нажмите кнопку **добавить подключение**.  
  
3. Укажите допустимое соединение с образцом базы данных "Борей".  
  
## <a name="to-add-a-project-that-contains-a-linq-to-sql-file"></a>Чтобы добавить в проект, содержащий файл классов LINQ to SQL  
  
1. В меню **Файл** окна Visual Studio наведите указатель мыши на пункт **Создать** и щелкните **Проект**. Выберите Visual Basic **приложение Windows Forms** в качестве типа проекта.  
  
2. В меню **Проект** выберите пункт **Добавить новый элемент**. Выберите **LINQ to SQL Classes** шаблона элемента.  
  
3. Назовите файл `northwind.dbml`. Нажмите кнопку **Добавить**. Для файла northwind.dbml открывается реляционный конструктор объектов (O/R Designer).  
  
## <a name="to-add-tables-to-query-to-the-or-designer"></a>Добавление таблицы к запросу в конструкторе O/R  
  
1. В **обозревателя серверов**/**обозреватель баз данных**, разверните подключение к базе данных "Борей". Разверните **таблиц** папки.  
  
     Если вы уже закрыли конструктор O/R, его можно открыть, дважды щелкнув файл northwind.dbml, который был добавлен ранее.  
  
2. Щелкните таблицу Customers и перетащите его в левую область конструктора. Щелкните в таблице Orders и перетащите его в левую область конструктора.  
  
     Конструктор создает новый `Customer` и `Order` объектов для проекта. Обратите внимание на то, что конструктор автоматически обнаруживает связи между таблицами и создает дочерние свойства для связанных объектов. Например, IntelliSense покажет, что `Customer` объект имеет `Orders` свойство для всех заказов, связанных с клиентом.  
  
3. Сохраните изменения и закройте конструктор.  
  
4. Сохраните проект.  
  
## <a name="to-add-code-to-query-the-database-and-display-the-results"></a>Добавление кода для запроса к базе данных и отображения результатов  
  
1. Из **элементов**, перетащите <xref:System.Windows.Forms.DataGridView> элемента управления на форме Windows по умолчанию для проекта, Form1.  
  
2. Дважды щелкните файл Form1, чтобы добавить код для `Load` событий формы.  
  
3. При добавлении таблиц в конструктор O/R designer добавлены <xref:System.Data.Linq.DataContext> объект для проекта. Этот объект содержит код, который должен иметь доступ к этим таблицам, в дополнение к отдельным объектам и коллекциям для каждой таблицы. <xref:System.Data.Linq.DataContext> Объектов для проекта имя на основе имени файла .dbml. Для этого проекта <xref:System.Data.Linq.DataContext> объект называется `northwindDataContext`.  
  
     Можно создать экземпляр <xref:System.Data.Linq.DataContext> в коде и запрос таблицы, указанные в конструкторе O/R.  
  
     Добавьте следующий код, чтобы `Load` событий для запроса к таблицам, которые представляются как свойства контекста данных.  
  
     [!code-vb[VbLINQToSQLHowTos#3](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbLINQtoSQLHowTos/VB/Form2.vb#3)]  
  
4. Нажмите клавишу F5, чтобы запустить проект и просмотреть результаты.  
  
5. Ниже приведены некоторые дополнительные запросы, которые могут помочь.  
  
     [!code-vb[VbLINQToSQLHowTos#4](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbLINQtoSQLHowTos/VB/Form2.vb#4)]  
    [!code-vb[VbLINQToSQLHowTos#5](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbLINQtoSQLHowTos/VB/Form2.vb#5)]  
  
## <a name="see-also"></a>См. также

- [LINQ](../../../../visual-basic/programming-guide/language-features/linq/index.md)
- [Запросы](../../../../visual-basic/language-reference/queries/index.md)
- [LINQ to SQL](../../../../framework/data/adonet/sql/linq/index.md)
- [Методы DataContext (реляционный конструктор объектов)](/visualstudio/data-tools/datacontext-methods-o-r-designer)
