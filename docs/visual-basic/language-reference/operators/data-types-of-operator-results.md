---
title: Типы данных результатов оператора (Visual Basic)
ms.date: 07/20/2015
helpviewer_keywords:
- data types [Visual Basic], operator result data types
- result data types [Visual Basic]
- operator result data types [Visual Basic]
- operators [Visual Basic], data types
- data types [Visual Basic], ranges
- operators [Visual Basic], result data types
ms.assetid: 9d524533-e1a1-4aa8-b1b8-622068173d06
ms.openlocfilehash: bc7f29ae0e29a4c2fbfdf2e40d2226e174a06d3a
ms.sourcegitcommit: 205b9a204742e9c77256d43ac9d94c3f82909808
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/10/2019
ms.locfileid: "70856048"
---
# <a name="data-types-of-operator-results-visual-basic"></a>Типы данных результатов оператора (Visual Basic)
Visual Basic определяет тип данных результата операции на основе типов данных операндов. В некоторых случаях это может быть тип данных с большим диапазоном, чем любой из операндов.  
  
## <a name="data-type-ranges"></a>Диапазоны типов данных  
 Диапазоны соответствующих типов данных в порядке от меньшего к большему, имеют следующий вид:  
  
- [Логическое значение](../../../visual-basic/language-reference/data-types/boolean-data-type.md) — два возможных значения  
  
- [SByte](../../../visual-basic/language-reference/data-types/sbyte-data-type.md), [Byte](../../../visual-basic/language-reference/data-types/byte-data-type.md) — 256 возможных целочисленных значений  
  
- [Short](../../../visual-basic/language-reference/data-types/short-data-type.md), [UShort](../../../visual-basic/language-reference/data-types/ushort-data-type.md) — 65 536 (6.5... E + 4) возможные целочисленные значения  
  
- [Integer](../../../visual-basic/language-reference/data-types/integer-data-type.md), [UInteger](../../../visual-basic/language-reference/data-types/uinteger-data-type.md) — 4 294 967 296 (4.2... E + 9) возможных целочисленных значений  
  
- [Long](../../../visual-basic/language-reference/data-types/long-data-type.md), [ulong](../../../visual-basic/language-reference/data-types/ulong-data-type.md) — 18446744073709551615 (1.8... E + 19) возможных целочисленных значений  
  
- [Десятичное число](../../../visual-basic/language-reference/data-types/decimal-data-type.md) — 1.5... E + 29 возможных целочисленных значений, максимальный диапазон 7,9... E + 28 (абсолютное значение)  
  
- [Single](../../../visual-basic/language-reference/data-types/single-data-type.md) — максимальный диапазон 3.4... E + 38 (абсолютное значение)  
  
- [Double](../../../visual-basic/language-reference/data-types/double-data-type.md) — максимальный диапазон 1.7... E + 308 (абсолютное значение)  
  
 Дополнительные сведения о типах данных Visual Basic см. в разделе [типы данных](../../../visual-basic/language-reference/data-types/index.md).  
  
 Если операнду присвоено значение [Nothing](../../../visual-basic/language-reference/nothing.md), то арифметические операторы Visual Basic обрабатывают его как ноль.  
  
## <a name="decimal-arithmetic"></a>Десятичная арифметика  
 Обратите внимание, что тип данных [Decimal](../../../visual-basic/language-reference/data-types/decimal-data-type.md) не является ни типом с плавающей запятой, ни целым числом.  
  
 Если один из `+`операндов операции `–` `*`,, `/`, или `Mod` является `Decimal` , а другой — нет `Single` или `Double`, Visual Basic расширяет другой операнд до `Decimal`. Он выполняет операцию в `Decimal`, а тип данных result —. `Decimal`  
  
## <a name="floating-point-arithmetic"></a>Арифметика с плавающей запятой  
 Visual Basic выполняет большинство арифметических операций с плавающей запятой в [Double](../../../visual-basic/language-reference/data-types/double-data-type.md), что является наиболее эффективным типом данных для таких операций. Однако если один операнд является [одиночным](../../../visual-basic/language-reference/data-types/single-data-type.md) , а другой — нет `Double`, Visual Basic выполняет операцию в. `Single` Он расширяет каждый операнд по мере необходимости до соответствующего типа данных перед операцией, а результат имеет этот тип данных.  
  
### <a name="-and--operators"></a>Операторы/и ^  
 Оператор `/` определяется только для типов данных [Decimal](../../../visual-basic/language-reference/data-types/decimal-data-type.md), [Single](../../../visual-basic/language-reference/data-types/single-data-type.md), и [Double](../../../visual-basic/language-reference/data-types/double-data-type.md). Visual Basic расширяет каждый операнд по мере необходимости до соответствующего типа данных перед операцией, а результат будет иметь этот тип данных.  
  
 В следующей таблице показаны типы данных результата для `/` оператора. Обратите внимание, что эта таблица является симметричной. для данного сочетания типов данных операндов тип данных результата одинаковый, независимо от порядка операндов.  
  
||||||  
|---|---|---|---|---|  
||`Decimal`|`Single`|`Double`|Любой целочисленный тип|  
|`Decimal`|Decimal|Single|Double|Decimal|  
|`Single`|Single|Single|Double|Single|  
|`Double`|Double|Double|Double|Double|  
|Любой целочисленный тип|Decimal|Single|Double|Double|  
  
 Оператор определяется только `Double` для типа данных. `^` Visual Basic расширяет каждый операнд по мере необходимости `Double` перед операцией, а тип данных Result всегда. `Double`  
  
## <a name="integer-arithmetic"></a>Целочисленная арифметика  
 Тип данных результата целочисленной операции зависит от типов данных операндов. Как правило, Visual Basic использует следующие политики для определения типа данных result:  
  
- Если оба операнда бинарного оператора имеют один и тот же тип данных, результат будет иметь этот тип данных. Исключением является `Boolean`, что `Short`принудительно.  
  
- Если неподписанный операнд участвует в подписанном операнде, то результат имеет тип со знаком, содержащий по меньшей мере один из операндов с диапазоном.  
  
- В противном случае результат обычно имеет больший из двух типов данных операндов.  
  
 Обратите внимание, что тип данных результата может отличаться от типа данных операнда.  
  
> [!NOTE]
> Тип данных результата не всегда достаточен для хранения всех возможных значений, полученных в результате операции. Исключение <xref:System.OverflowException> может возникнуть, если значение слишком велико для типа данных результата.  
  
### <a name="unary--and--operators"></a>Унарные операторы + и –  
 В следующей таблице показаны типы данных результата для двух унарных операторов `+` и. `–`  
  
|||||||||||  
|---|---|---|---|---|---|---|---|---|---|  
||`Boolean`|`SByte`|`Byte`|`Short`|`UShort`|`Integer`|`UInteger`|`Long`|`ULong`|  
|Унар`+`|Short|SByte|Byte|Short|UShort|Целое число|UInteger|Long|ULong|  
|Унар`–`|Short|SByte|Short|Short|Целое число|Целое число|Long|Long|Decimal|  
  
### <a name="-and--operators"></a><\<Операторы и > >  
 В следующей таблице показаны типы данных результата для двух операторов `<<` поразрядного сдвига и. `>>` Visual Basic обрабатывает каждый оператор сдвига в битах как унарный оператор в его левом операнде (битовый шаблон для сдвига).  
  
|||||||||||  
|---|---|---|---|---|---|---|---|---|---|  
||`Boolean`|`SByte`|`Byte`|`Short`|`UShort`|`Integer`|`UInteger`|`Long`|`ULong`|  
|`<<`, `>>`|Short|SByte|Byte|Short|UShort|Целое число|UInteger|Long|ULong|  
  
 Если левый операнд имеет значение `Decimal` `Double`, `Single`, `String` или,`Long`Visual Basic пытается преобразовать его передоперацией,атипданныхresult—.`Long` Правый операнд (число битовых позиций для сдвига) должен быть `Integer` или типом, который расширяется до. `Integer`  
  
### <a name="binary----and-mod-operators"></a>Бинарные операторы +, \*–, и mod  
 В следующей таблице показаны типы данных `+` результата для бинарных операторов `–` `*` `Mod` и операторов и. Обратите внимание, что эта таблица является симметричной. для данного сочетания типов данных операндов тип данных результата одинаковый, независимо от порядка операндов.  
  
|||||||||||  
|---|---|---|---|---|---|---|---|---|---|  
||`Boolean`|`SByte`|`Byte`|`Short`|`UShort`|`Integer`|`UInteger`|`Long`|`ULong`|  
|`Boolean`|Short|SByte|Short|Short|Целое число|Целое число|Long|Long|Decimal|  
|`SByte`|SByte|SByte|Short|Short|Целое число|Целое число|Long|Long|Decimal|  
|`Byte`|Short|Short|Byte|Short|UShort|Целое число|UInteger|Long|ULong|  
|`Short`|Short|Short|Short|Short|Целое число|Целое число|Long|Long|Decimal|  
|`UShort`|Целое число|Целое число|UShort|Целое число|UShort|Целое число|UInteger|Long|ULong|  
|`Integer`|Целое число|Целое число|Целое число|Целое число|Целое число|Целое число|Long|Long|Decimal|  
|`UInteger`|Long|Long|UInteger|Long|UInteger|Long|UInteger|Long|ULong|  
|`Long`|Long|Long|Long|Long|Long|Long|Long|Long|Decimal|  
|`ULong`|Decimal|Decimal|ULong|Decimal|ULong|Decimal|ULong|Decimal|ULong|  
  
### <a name="-operator"></a>Оператор \\  
 В следующей таблице показаны типы данных результата для `\` оператора. Обратите внимание, что эта таблица является симметричной. для данного сочетания типов данных операндов тип данных результата одинаковый, независимо от порядка операндов.  
  
|||||||||||  
|---|---|---|---|---|---|---|---|---|---|  
||`Boolean`|`SByte`|`Byte`|`Short`|`UShort`|`Integer`|`UInteger`|`Long`|`ULong`|  
|`Boolean`|Short|SByte|Short|Short|Целое число|Целое число|Long|Long|Long|  
|`SByte`|SByte|SByte|Short|Short|Целое число|Целое число|Long|Long|Long|  
|`Byte`|Short|Short|Byte|Short|UShort|Целое число|UInteger|Long|ULong|  
|`Short`|Short|Short|Short|Short|Целое число|Целое число|Long|Long|Long|  
|`UShort`|Целое число|Целое число|UShort|Целое число|UShort|Целое число|UInteger|Long|ULong|  
|`Integer`|Целое число|Целое число|Целое число|Целое число|Целое число|Целое число|Long|Long|Long|  
|`UInteger`|Long|Long|UInteger|Long|UInteger|Long|UInteger|Long|ULong|  
|`Long`|Long|Long|Long|Long|Long|Long|Long|Long|Long|  
|`ULong`|Long|Long|ULong|Long|ULong|Long|ULong|Long|ULong|  
  
 Если любой `\` из операндов оператора является [десятичным](../../../visual-basic/language-reference/data-types/decimal-data-type.md), [одинарным](../../../visual-basic/language-reference/data-types/single-data-type.md)или [двойным](../../../visual-basic/language-reference/data-types/double-data-type.md), Visual Basic пытается преобразовать его в [Long](../../../visual-basic/language-reference/data-types/long-data-type.md) перед операцией, а тип данных Result `Long`имеет значение.  
  
## <a name="relational-and-bitwise-comparisons"></a>Реляционные и побитовые сравнения  
 `=`Тип данных результата реляционной операции (, `<>`, `<`, `>` `<=`,, `>=`) всегда `Boolean`имеет [логический тип данных](../../../visual-basic/language-reference/data-types/boolean-data-type.md). Это справедливо и`And`для логических операций ( `Not`, `Or` `Xor` `OrElse` `AndAlso`,,,,) на `Boolean` операндах.  
  
 Тип данных результата побитовой логической операции зависит от типов данных операндов. Обратите `AndAlso` внимание `OrElse` , что и задаются только для `Boolean`, а Visual Basic преобразует каждый `Boolean` операнд по мере необходимости в перед выполнением операции.  
  
### <a name="-----and--operators"></a>=, < >, \<, >, \<= и > = операторы  
 Если оба операнда имеют `Boolean`значение, Visual Basic `True` считается меньше `False`. Если числовой тип сравнивается с `String`, Visual Basic пытается `String` преобразовать `Double` в перед операцией. Операнд `Char` или`Date` может сравниваться только с другим операндом того же типа данных. Тип данных Result всегда `Boolean`имеет значение.  
  
### <a name="bitwise-not-operator"></a>Оператор побитового не  
 В следующей таблице показаны типы данных результата для побитового `Not` оператора.  
  
|||||||||||  
|---|---|---|---|---|---|---|---|---|---|  
||`Boolean`|`SByte`|`Byte`|`Short`|`UShort`|`Integer`|`UInteger`|`Long`|`ULong`|  
|`Not`|логический|SByte|Byte|Short|UShort|Целое число|UInteger|Long|ULong|  
  
 Если операнд имеет значение `Decimal` `Single` `String`,, или, Visual Basic пытается преобразовать его `Long` передоперацией,атипданныхresult—.`Long` `Double`  
  
### <a name="bitwise-and-or-and-xor-operators"></a>Побитовые операторы and, OR и XOR  
 В следующей таблице показаны типы данных результата для побитовых `And`операторов `Or`, и `Xor` . Обратите внимание, что эта таблица является симметричной. для данного сочетания типов данных операндов тип данных результата одинаковый, независимо от порядка операндов.  
  
|||||||||||  
|---|---|---|---|---|---|---|---|---|---|  
||`Boolean`|`SByte`|`Byte`|`Short`|`UShort`|`Integer`|`UInteger`|`Long`|`ULong`|  
|`Boolean`|логический|SByte|Short|Short|Целое число|Целое число|Long|Long|Long|  
|`SByte`|SByte|SByte|Short|Short|Целое число|Целое число|Long|Long|Long|  
|`Byte`|Short|Short|Byte|Short|UShort|Целое число|UInteger|Long|ULong|  
|`Short`|Short|Short|Short|Short|Целое число|Целое число|Long|Long|Long|  
|`UShort`|Целое число|Целое число|UShort|Целое число|UShort|Целое число|UInteger|Long|ULong|  
|`Integer`|Целое число|Целое число|Целое число|Целое число|Целое число|Целое число|Long|Long|Long|  
|`UInteger`|Long|Long|UInteger|Long|UInteger|Long|UInteger|Long|ULong|  
|`Long`|Long|Long|Long|Long|Long|Long|Long|Long|Long|  
|`ULong`|Long|Long|ULong|Long|ULong|Long|ULong|Long|ULong|  
  
 Если `Decimal`операнд имеет значение `Long` `Long`, `Single`, или `String`, Visual Basic пытается преобразовать его в перед операцией, а тип данных результата будет таким же, как если бы этот операнд уже был. `Double`  
  
## <a name="miscellaneous-operators"></a>Прочие операторы  
 Оператор определен только для объединения `String` операндов. `&` Visual Basic преобразует каждый операнд по мере необходимости `String` в перед операцией, а тип данных результата всегда `String`равен. В целях `&` оператора все преобразования в `String` считаются расширяющими, даже если `Option Strict` имеет значение `On`.  
  
 Для операторов `IsNot` и оба операнда должны иметь ссылочный тип. `Is` `TypeOf`... `Is` для выражения требуется, чтобы первый операнд был ссылочного типа, а второй операнд был именем типа данных. Во всех этих случаях тип данных result имеет `Boolean`значение.  
  
 Оператор определяется только для шаблонов, `String` соответствующих операндам. `Like` Visual Basic пытается преобразовать каждый операнд по мере необходимости `String` в перед операцией. Тип данных Result всегда `Boolean`имеет значение.  
  
## <a name="see-also"></a>См. также

- [Типы данных](../../../visual-basic/language-reference/data-types/index.md)
- [Операторы и выражения](../../../visual-basic/programming-guide/language-features/operators-and-expressions/index.md)
- [Арифметические операторы в Visual Basic](../../../visual-basic/programming-guide/language-features/operators-and-expressions/arithmetic-operators.md)
- [Операторы сравнения в Visual Basic](../../../visual-basic/programming-guide/language-features/operators-and-expressions/comparison-operators.md)
- [Инструкции](../../../visual-basic/language-reference/operators/index.md)
- [Порядок применения операторов в Visual Basic](../../../visual-basic/language-reference/operators/operator-precedence.md)
- [Список операторов, сгруппированных по функциональному назначению](../../../visual-basic/language-reference/operators/operators-listed-by-functionality.md)
- [Арифметические операторы](../../../visual-basic/language-reference/operators/arithmetic-operators.md)
- [Операторы сравнения](../../../visual-basic/language-reference/operators/comparison-operators.md)
- [Оператор Option Strict](../../../visual-basic/language-reference/statements/option-strict-statement.md)
