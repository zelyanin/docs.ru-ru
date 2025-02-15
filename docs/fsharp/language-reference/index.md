---
title: Справочник по языку F#
description: Найдите F# сведения о компонентах языка на основе этой ссылки на маркеры языка, основные понятия, типы, выражения и разделы, поддерживаемые компилятором.
ms.date: 05/16/2016
ms.openlocfilehash: ac7e268b28d6bb654e4443d04695cb15fe756e9f
ms.sourcegitcommit: 14ad34f7c4564ee0f009acb8bfc0ea7af3bc9541
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/01/2019
ms.locfileid: "73424993"
---
# <a name="f-language-reference"></a>Справочник по языку F#

Этот раздел представляет собой ссылку на F# язык, язык программирования с несколькими парадигмами, предназначенный для .NET. Язык F# поддерживает функциональные, объектно-ориентированные и императивные модели программирования.

## <a name="f-tokens"></a>Токены F#

Следующая таблица содержит статьи, где приведены таблицы ключевых слов, символов и литералов, используемых в F# в качестве токенов.

|Заголовок|Описание|
|-----|-----------|
|[Справочник ключевых слов](keyword-reference.md)|Содержит ссылки на сведения обо всех ключевых словах языка F#.|
|[Справочник символов и операторов](./symbol-and-operator-reference/index.md)|Содержит таблицу символов и операторов, которые используются в языке F#.|
|[Литералы](literals.md)|Описывает синтаксис для литеральных значений в F# и способы указания сведений о типах для литералов F#.|

## <a name="f-language-concepts"></a>Основные понятия языка F#

Следующая таблица содержит статьи, где описаны основные понятия языка.

|Заголовок|Описание|
|-----|-----------|
|[Функции](./functions/index.md)|Функции являются основным элементом выполнения программы на любом языке программирования. Как и в других языках, функция F# имеет имя, может иметь параметры и принимать аргументы, а также имеет тело. F# также поддерживает конструкции функционального программирования, например, обработку функций как значений, использование неименованных функций в выражениях, объединение функций для образования новых функций, каррированные функции и неявное определение функций посредством частичного применения аргументов функции.|
|[Типы языка F#](fsharp-types.md)|Описывает типы, которые используются в F#, а также способы их именования и описания.|
|[Вывод типа](type-inference.md)|Описывает, как компилятор F# определяет типы значений, переменных, параметров и возвращаемых значений.|
|[Автоматическое обобщение](./generics/automatic-generalization.md)|Описывает универсальные конструкции в F#.|
|[Наследование](inheritance.md)|Описывает наследование, которое применяется для моделирования отношения "is-a" — подтипирования — в объектно-ориентированном программировании.|
|[Члены](./members/index.md)|Описывает элементы типов объектов F#.|
|[Параметры и аргументы](Parameters-and-Arguments.md)|Описывает языковую поддержку для определения параметров и передачи аргументов в функции, методы и свойства. Содержит сведения о передаче по ссылке.|
|[Перегрузка операторов](operator-overloading.md)|Описывает перегрузку арифметических операторов в классе или типе записи, а также на глобальном уровне.|
|[Приведение и преобразование](casting-and-conversions.md)|Описывает поддержку для преобразований типов в F#.|
|[Управление доступом](access-control.md)|Описывает управление доступом в F#. Управление доступом означает объявление того, какие именно клиенты могут использовать определенные элементы программы, например типы, методы, функции и т. д.|
|[Соответствие шаблону](pattern-matching.md)|Описывает шаблоны, представляющие собой правила преобразования входных данных, которые применяются в языке F# для сравнения данных с шаблоном, разложения данных на составные части или извлечения информации из данных различными способами.|
|[Активные шаблоны](active-patterns.md)|Описывает активные шаблоны. Активные шаблоны позволяют определять именованные разделы, на которые подразделяются входные данные. Активные шаблоны можно использовать для разложения данных в настраиваемом порядке для каждого раздела.|
|[Утверждения](assertions.md)|Описывает выражение `assert`, являющееся функцией отладки, которую можно использовать для тестирования выражения. При сбое в режиме отладки утверждение создает диалоговое окно системной ошибки.|
|[Обработка исключений](./exception-handling/index.md)|Содержит сведения о поддержке обработки исключений в языке F#.|
|[Атрибуты](attributes.md)|Описывает атрибуты, позволяющие применять метаданные к программным конструкциям.|
|[Управление ресурсами: ключевое слово `use`](resource-management-the-use-keyword.md)|Описывает ключевые слова `use` и `using`, позволяющие управлять инициализацией и освобождением ресурсов.|
|[Пространства имен](namespaces.md)|Описывает поддержку пространств имен в F#. С помощью пространства имен вы можете упорядочить код по областям соответствующей функциональности, подключив имя к группированию элементов программы.|
|[Модули](modules.md)|Описывает модули. Модуль F# — это группирование кода F#, например значений, типов и значений функций, в программе F#. Код группирования в модулях объединяет связанный код и помогает избежать конфликтов имен в программе.|
|[Объявления импорта: ключевое слово `open`](import-declarations-the-open-keyword.md)|Описывает работу `open`. Объявление импорта указывает модуль или пространство имен, на элементы которого можно ссылаться без использования полного имени.|
|[Сигнатуры](signature-files.md)|Описывает сигнатуры и их файлы. В файле сигнатур содержатся сведения об открытых сигнатурах набора элементов программы на языке F#, таких как типы, пространства имен и модули. Файл сигнатур можно использовать для указания доступности этих элементов программы.|
|[Документация XML](xml-documentation.md)|Описывает создание файлов документации для комментариев к XML-документам. Вы можете создать документацию из комментариев к коду в F#, а также в других языках .NET.|
|[Подробный синтаксис](verbose-syntax.md)|Описывает синтаксис для конструкций F#, когда упрощенный синтаксис отключен. Подробный синтаксис указывается директивой `#light "off"` в верхней части файла кода.|

## <a name="f-types"></a>Типы языка F#

Следующая таблица содержит статьи, где описаны типы, поддерживаемые языком F#.

|Заголовок|Описание|
|-----|-----------|
|[Значения](./values/index.md)|Описывает значения, которые являются величинами, имеющими конкретный тип. Значения могут быть целыми числами или числами с плавающей запятой, символами или текстом, списками, последовательностями, массивами, кортежами, размеченными объединениями, записями, типами классов или значениями функции.|
|[Базовые типы](basic-types.md)|Описывает основные основные типы, используемые в F# языке. Кроме того, указывает соответствующие типы .NET и минимальное и максимальное значения для каждого из них.|
|[Тип единиц](unit-type.md)|Описывает тип `unit`, который указывает на отсутствие конкретного значения. Тип `unit` имеет только одно значение, которое выступает в качестве заполнителя, если другое значение не существует или не требуется.|
|[Строки](strings.md)|Описывает строки в F#. Тип `string` представляет неизменяемый текст в виде последовательности символов Юникода. `string` — это псевдоним для `System.String` в .NET Framework.|
|[Кортежи](tuples.md)|Описывает кортежи, представляющие собой группирования неименованных, но упорядоченных значений, которые могут иметь разные типы.|
|[Типы коллекций F#](fsharp-collection-types.md)|Обзор типов функциональных коллекций F#, включая типы для массивов, списков, последовательностей (seq), карт и наборов.|
|[Списки](lists.md)|Описывает списки. В языке F# список — это упорядоченная, неизменная серия элементов одного типа.|
|[Параметры](options.md)|Описывает тип параметра. Параметр в F# используется, когда значение может как существовать, так и не существовать. Параметр имеет базовый тип и может содержать значение этого типа либо не содержать никакого значения.|
|[Последовательности](sequences.md)|Описывает последовательности. Последовательность — это логический ряд элементов одного типа. Отдельные элементы последовательности вычисляются только при необходимости, поэтому представление может быть меньше указанного количества элементов литерала.|
|[Массивы](arrays.md)|Описывает массивы. Массивы — это изменяемые последовательности элементов данных одного типа, имеющие фиксированный размер и индексируемые с нуля.|
|[Записи](records.md)|Описывает записи. Записи представляют собой простые агрегаты именованных значений, которые могут иметь элементы.|
|[Размеченные объединения](discriminated-unions.md)|Описывает размеченные объединения, обеспечивающие поддержку значений, которые могут представлять один из множества именованных вариантов, каждый из которых может иметь разные значения и типы.|
|[Перечисления](enumerations.md)|Описывает перечисления, которые являются типами, имеющими определенный набор именованных значений. Их можно использовать вместо литералов, чтобы сделать код более понятным и простым в обслуживании.|
|[Ссылочные ячейки](reference-cells.md)|Описывает ссылочные ячейки, представляющие собой места хранения, которые позволяют создавать изменяющиеся переменные с семантикой ссылок.|
|[Сокращенные формы типов](type-abbreviations.md)|Описывает сокращенные формы типов, которые являются альтернативными именами типов.|
|[Классы](classes.md)|Описывает классы, которые являются типами, представляющими объекты, которые могут иметь свойства, методы и события.|
|[Структуры](structures.md)|Описывает структуры, представляющие собой компактный тип объекта, который может быть более эффективным, чем класс для типов, имеющих небольшое количество данных и простое поведение.|
|[Интерфейсы](interfaces.md)|Описывает интерфейсы, которые определяют наборы связанных элементов, реализуемых другими классами.|
|[Абстрактные классы](abstract-classes.md)|Описывает абстрактные классы, которые оставляют некоторые или все элементы нереализованными, чтобы реализации могли предоставляться производными классами.|
|[Расширения типов](type-extensions.md)|Описывает расширения типов, которые позволяют добавлять новые элементы в ранее определенный тип объекта.|
|[Гибкие типы](flexible-types.md)|Описывает гибкие типы. Заметка с гибким типом указывает на то, что параметр, переменная или значение имеют тип, который совместим с указанным типом, где совместимость определяется положением в объектно ориентированной иерархии классов или интерфейсов.|
|[Делегаты](delegates.md)|Описывает делегаты, которые представляют вызов функции в качестве объекта.|
|[Единицы измерения](units-of-measure.md)|Описывает единицы измерения. Значения с плавающей запятой в языке F# могут иметь связанные единицы измерения, которые обычно используются для указания длины, объема, массы и т. д.|
|[Поставщики типов](../tutorials/type-providers/index.md)|Описывает поставщики типов и предоставляет ссылки на пошаговые инструкции по использованию встроенных поставщиков типов для доступа к базам данных и веб-службам.|

## <a name="f-expressions"></a>Выражения F#

Следующая таблица содержит список статей, описывающих выражения F#.

|Заголовок|Описание|
|-----|-----------|
|[Условные выражения: `if...then...else`](conditional-expressions-if-then-else.md)|Описывает выражение `if...then...else`, которое выполняет различные ветви кода, а также дает разные значения при вычислении в зависимости от заданного логического выражения.|
|[Выражения match](match-expressions.md)|Описывает выражение `match`, которое позволяет управлять ветвлением за счет сравнения выражения с набором шаблонов.|
|[Циклы: выражение `for...to`](loops-for-to-expression.md)|Описывает выражение `for...to`, которое используется для циклической итерации по диапазону значений переменной цикла.|
|[Циклы: выражение `for...in`](loops-for-in-expression.md)|Описывает выражение `for...in` — циклическую конструкцию, которая используется для итерации по совпадениям с шаблоном в перечислимой коллекции, такой как выражение диапазона, последовательность, список, массив или другая конструкция, поддерживающая перечисление.|
|[Циклы: выражение `while...do`](loops-while-do-expression.md)|Описывает выражение `while...do`, используемое для итеративного выполнения (выполнения в цикле), пока заданное условие теста истинно.|
|[Выражения объекта](object-expressions.md)|Описывает выражения объектов, формирующие новые экземпляры динамически создаваемого анонимного типа объекта, который основан на существующем базовом типе, интерфейсе или наборе интерфейсов.|
|[Отложенные выражения](lazy-expressions.md)|Описывает отложенные выражения, которые являются вычислениями, которые не оцениваются немедленно, а оцениваются, когда фактически требуется результат.|
|[Выражения вычисления](computation-expressions.md)|Описывает выражения вычисления в F#, обеспечивающие удобный синтаксис для записи вычислений, которые можно упорядочивать и комбинировать с помощью привязок и конструкций потока управления. Они позволяют предоставить удобный синтаксис для компонентов функционального программирования *monad*, которые можно использовать для управления данными и побочными эффектами в функциональных программах. Один из типов выражений вычисления — асинхронный рабочий процесс — обеспечивает поддержку асинхронных и параллельных вычислений. Дополнительные сведения см. в статье [Асинхронные рабочие потоки](asynchronous-workflows.md).|
|[Асинхронные рабочие потоки](asynchronous-workflows.md)|Описывает асинхронные рабочие процессы — функцию языка, позволяющую писать асинхронный код практически аналогично синхронному.|
|[Цитирование кода](code-quotations.md)|Описывает цитирование кода — функцию языка, позволяющую программно создавать выражения кода F# и работать с ними.|
|[Выражения запросов](query-expressions.md)|Описывает выражения запросов — функцию языка, которая реализует LINQ для F# и позволяет составлять запросы к источнику данных или перечислимой коллекции.|

## <a name="compiler-supported-constructs"></a>Конструкции, поддерживаемые компилятором

Следующая таблица содержит список статей, где описаны специальные конструкции, поддерживаемые компилятором.

|Раздел|Описание|
|-----|-----------|
|[Параметры компилятора](compiler-options.md)|Описывает параметры командной строки для компилятора F#.|
|[Директивы компилятора](compiler-directives.md)|Описывает директивы процессора и компилятора.|
|[Идентификаторы Source Line, File и Path](source-line-file-path-identifiers.md)|Описывает идентификаторы `__LINE__`, `__SOURCE_DIRECTORY__` и `__SOURCE_FILE__`, представляющие собой встроенные значения, которые позволяют получить доступ к номеру исходной строки, каталогу и имени файла в коде.|

## <a name="see-also"></a>См. также

- [Visual F#](../index.md)
