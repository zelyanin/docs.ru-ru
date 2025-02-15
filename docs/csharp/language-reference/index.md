---
title: Справочник по C#
ms.date: 02/14/2017
helpviewer_keywords:
- Visual C#, language reference
- language reference [C#]
- Programmer's Reference for C#
- C# language, reference
- reference, C# language
ms.assetid: 06de3167-c16c-4e1a-b3c5-c27841d4569a
ms.openlocfilehash: 4fed33272dbed50100a37aa9fcd30befc46435f9
ms.sourcegitcommit: 559259da2738a7b33a46c0130e51d336091c2097
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/22/2019
ms.locfileid: "72771840"
---
# <a name="c-reference"></a>Справочник по C#

Этот раздел содержит подробные справочные сведения о ключевых словах, операторах, специальных символах, директивах препроцессора, параметрах компилятора и ошибках и предупреждениях компилятора в среде C#.  
  
## <a name="in-this-section"></a>Содержание раздела

 [Ключевые слова в C#](./keywords/index.md)  
 Ссылки на сведения о ключевых словах и синтаксисе языка C#.  
  
 [Операторы в C#](./operators/index.md)  
 Ссылки на сведения об операторах и синтаксисе языка C#.  

 [Специальные символы в C#](./tokens/index.md)  
 Предоставляет ссылки на сведения о специальных контекстные символов в C# и их использовании.  

 [Директивы препроцессора C#](./preprocessor-directives/index.md)  
 Ссылки на сведения о командах компилятора для внедрения в исходном коде C#.  
  
 [Параметры компилятора C# ](./compiler-options/index.md)  
 Сведения о параметрах компилятора и их использовании.  
  
 [Ошибки компилятора C#](./compiler-messages/index.md)  
 Фрагменты кода, демонстрирующие причины и способы исправления ошибок и предупреждений компилятора C#.  
  
 [Спецификация языка C#](../../../_csharplang/spec/introduction.md)  
 Спецификация языка C# версии 6.0 Это черновой вариант для языка C# версии 6.0. Этот документ будет пересмотрен в рамках работы с комитетом по стандартам C# ECMA. Версия 5.0 была выпущена в декабре 2017 г. как [стандартный 5-й выпуск ECMA-334](https://www.ecma-international.org/publications/files/ECMA-ST/ECMA-334.pdf).

Возможности, которые были реализованы в языке C# версий более поздних чем 6.0, представлены в предложениях по спецификации языка. В этих документах описываются изменения спецификации языка, связанные с добавлением новых функций. Это черновой вариант для формы. Эти спецификации будут улучшены и отправлены в комитет по стандартам ECMA для официального анализа и внедрения в будущую версию стандарта C#.

 [Предложения по спецификации C# 7.0](../../../_csharplang/proposals/csharp-7.0/pattern-matching.md)  
 В версии C# 7.0 реализован ряд новых возможностей, включая сопоставления шаблонов, локальные функции, объявления выходной переменной, выражения throw, двоичные литералы и разделители между цифрами. Эта папка содержит спецификации для каждой из этих функций.
  
 [Предложения по спецификации C# 7.1](../../../_csharplang/proposals/csharp-7.1/async-main.md)  
 В версию C# 7.1 добавлено несколько новых возможностей. Можно написать метод `Main`, возвращающий `Task` или `Task<int>`. Это позволяет добавлять модификатор `async` в метод `Main`. Выражение `default` можно использовать без типа в тех расположениях, где возможен вывод типа. Кроме того, появилось еще одно дополнительное усовершенствование: вывод имен элементов кортежа. И, наконец, сопоставление шаблонов можно использовать с универсальными шаблонами.

 [Предложения по спецификации C# 7.2](../../../_csharplang/proposals/csharp-7.2/readonly-ref.md)  
 В версию C#7.2 добавлен ряд простых функций. С помощью ключевого слова `in` можно передавать аргументы по ссылке только для чтения. Внесен ряд незначительных изменений для поддержки безопасности во время компиляции для `Span` и связанных типов. В некоторых ситуациях можно использовать именованные аргументы, если следующие за ними аргументы являются позиционными. Модификатор доступа `private protected` позволяет указывать, что вызывающие объекты ограничены производными типами, реализованными в той же сборке. Оператор `?:` можно использовать для разрешения в ссылку на переменную. С помощью разделителя начальных цифр можно форматировать шестнадцатеричные и двоичные числа.

 [Предложения по спецификации C# 7.3](../../../_csharplang/proposals/csharp-7.3/blittable.md)  
 Версия C# 7.3 является очередным промежуточным выпуском, содержащим несколько небольших обновлений. К параметрам универсальных типов можно применять новые ограничения. Другие изменения упрощают работу с полями `fixed`, включая использование выделений [`stackalloc`](./operators/stackalloc.md). Локальные переменные, объявленные с ключевым словом `ref`, можно переназначать для указания на новое хранилище. Можно применять атрибуты к автоматически реализуемым свойствам, предназначенным для созданного компилятором резервного поля. Переменные выражений можно использовать в инициализаторах. Кортежи можно проверять на равенство (или неравенство). Кроме того, были внесены некоторые улучшения в разрешение перегрузки.
  
 [Предложения по спецификации C# 8.0](../../../_csharplang/proposals/csharp-8.0/nullable-reference-types.md)  
 Версия C# 8.0 доступна для .NET Core 3.0. В число возможностей входят использование ссылочных типов, допускающих значения NULL, рекурсивное сопоставление шаблонов, методы интерфейса по умолчанию, асинхронные потоки, диапазоны и индексы, использование шаблонов и объявлений using, назначение объединения со значением NULL и члены экземпляров с доступом только на чтение.
  
## <a name="related-sections"></a>Связанные разделы  

 [Руководство по языку C#](../index.md)  
 Портал для документации по Visual C#.  
  
 [Использование среды разработки Visual Studio для C#](/visualstudio/get-started/csharp)  
 Ссылки на концептуальные разделы и разделы задач, описывающие интегрированную среду разработки и редактор.  
  
 [Руководство по программированию на C#](../programming-guide/index.md)  
 Сведения об использовании языка программирования C#.
