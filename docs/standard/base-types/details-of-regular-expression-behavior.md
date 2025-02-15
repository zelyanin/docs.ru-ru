---
title: Подробные сведения о поведении регулярных выражений
ms.date: 03/30/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
helpviewer_keywords:
- regular expressions, behavior
- .NET Framework regular expressions, behavior
ms.assetid: 0ee1a6b8-caac-41d2-917f-d35570021b10
author: rpetrusha
ms.author: ronpet
ms.openlocfilehash: f4d7cbd00dbf94900185643490b952ced7887965
ms.sourcegitcommit: 5ae5a1a9520b8b8b6164ad728d396717f30edafc
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/11/2019
ms.locfileid: "70895218"
---
# <a name="details-of-regular-expression-behavior"></a>Подробные сведения о поведении регулярных выражений
Обработчик регулярных выражений .NET Framework выполняет поиск с возвратом для регулярных выражений и является реализацией традиционного механизма NFA (недетерминированного конечного автомата), аналогично тем, которые используются в Perl, Python, Emacs и Tcl. Это отличает его от более быстрых, но и более ограниченных DFA-машин (детерминированный конечный автомат), предназначенных только для регулярных выражений и используемых в awk, egrep или lex. Это также отличает его от типовых, но более медленных POSIX NFA-машин. В следующем разделе представлено описание трех типов обработчиков регулярных выражений и объясняется причина реализации регулярных выражений на платформе .NET с помощью классического механизма NFA.  
  
## <a name="benefits-of-the-nfa-engine"></a>Преимущества NFA-машины  
 Когда DFA-машины выполняют сопоставление шаблонов, порядок обработки определяется входной строкой. Машина начинает обработку с начала входной строки и продолжает последовательную обработку для определения соответствия следующего символа шаблону регулярного выражения. Их можно обнаружить как соответствия максимальной длины. Так как DFA-машины не проверяют один и тот же знак дважды, они не поддерживают поиск с возвратом. Но поскольку DFA-машина поддерживает только ограниченный режим работы, она не способна выполнять поиск соответствий по шаблону с обратными ссылками. Кроме того, она не создает явные выражения и не способна выделять части выражения.  
  
 В отличие от DFA-машин обычные NFA-машины выполняют поиск соответствий по шаблонам, и порядок обработки определяется шаблоном регулярных выражений. По мере обработки определенного элемента языка машина использует жадное сопоставление: она выполняет сопоставление как можно большей части входной строки. И кроме того, она сохраняет свое состояние после успешного поиска соответствия части выражения. Если поиск соответствия в конечном счете завершился с ошибкой, машина может вернуться в сохраненное состояние, поэтому она может попытаться найти дополнительные соответствия. Такой процесс, при котором успешно найденное соответствие части выражения "откладывается" для поиска соответствий с последующими языковыми элементами в регулярном выражении, называется *поиском с возвратом*. NFA-машины используют поиск с возвратом, проверяя все возможные расширения регулярного выражения в определенном порядке и принимая первое соответствие. Поскольку обычная NFA-машина создает определенное расширение регулярного выражения для успешного сопоставления, она способна находить соответствия для частей выражений и обратных ссылок. Но так как обычная NFA-машина выполняет поиск с возвратом, она может анализировать одно и то же состояние несколько раз, если к нему ведут несколько разных путей. В результате в наихудшем случае работа может замедляться по экспоненте. Так как обычная NFA-машина принимает первое найденное соответствие, другие (возможно, более длинные) соответствия могут остаться необнаруженными.  
  
 POSIX NFA-машины похожи на обычные NFA-машины, за исключением того, что они продолжают поиск с возвратом до тех пор, пока не будет найдено наиболее длинное совпадение. В результате POSIX NFA-машина работает медленнее обычной NFA-машины, и при использовании POSIX NFA-машины, изменив порядок поиска с возвратом, невозможно задать предпочтение короткому совпадению вместо более длинного.  
  
 Программисты предпочитают обычные NFA-машины, поскольку они превосходят по возможностям управления строковыми соответствиями обычные DFA-машины или POSIX NFA-машины. Несмотря на то, что в наихудшем случае быстродействие NFA-машин снижается, ими можно управлять так, что поиск соответствий будет проходить по линейному или полиномиальному времени. Добиться этого можно с помощью шаблонов, уменьшающих неоднозначности и ограничивающих количество возвратов. Другими словами, несмотря на то, что NFA-машины жертвуют производительностью в обмен на мощность и гибкость, в большинстве случаев они обеспечивают хорошую (или хотя бы приемлемую) производительность, если регулярное выражение грамотно написано и позволяет избежать ситуаций, при которых производительность поиска с возвратом снижается экспоненциально.  
  
> [!NOTE]
> Сведения о том, как чрезмерное использование поиска с возвратом влияет на производительность и как избегать его в регулярных выражениях, см. в статье [Поиск с возвратом в регулярных выражениях](../../../docs/standard/base-types/backtracking-in-regular-expressions.md).  
  
## <a name="net-framework-engine-capabilities"></a>Возможности обработчика .NET Framework  
 Чтобы использовать все сильные стороны классического механизма NFA, в обработчик регулярных выражений для платформы .NET включен полный набор конструкций, позволяющий программистам управлять процессом поиска с возвратом. Эти структуры можно использовать для ускорения поиска или для выбора предпочтительных расширений.  
  
 Ниже представлены другие возможности обработчика регулярных выражений .NET Framework.  
  
- Ленивые квантификаторы: `??`, `*?`, `+?`, `{`*n*`,`*m*`}?`. В первую очередь они указывают обработчику на необходимость ведения поиска минимального числа повторений. Обычные "жадные" квантификаторы, наоборот, пытаются сначала найти наибольшее число повторений. В следующем примере кода демонстрируется различие между двумя квантификаторами. Регулярное выражение соответствует предложению, оканчивающемуся на число, и захваченная группа должна извлечь это число. Регулярное выражение `.+(\d+)\.` содержит жадный квантификатор `.+`, под влиянием которого обработчик регулярных выражений захватывает только последнюю цифру числа. И наоборот, регулярное выражение `.+?(\d+)\.` содержит ленивый квантификатор `.+?`, под влиянием которого обработчик регулярных выражений захватывает все число.  
  
     [!code-csharp[Conceptual.RegularExpressions.Design#1](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.regularexpressions.design/cs/lazy1.cs#1)]
     [!code-vb[Conceptual.RegularExpressions.Design#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.regularexpressions.design/vb/lazy1.vb#1)]  
  
     Описания "жадной" и "ленивой" версий этого регулярного выражения представлены в следующей таблице.
  
    |Шаблон|ОПИСАНИЕ|  
    |-------------|-----------------|  
    |`.+` ("жадный" квантификатор)|Соответствие как минимум одному вхождению любого символа. Это дает обработчику регулярных выражений команду на сопоставление всей строки и выполнение поиска с возвратом, который требуется для сопоставления оставшейся части шаблона.|  
    |`.+?` ("ленивый" квантификатор)|Соответствие как минимум одному вхождению любого символа, но как можно меньшему количеству.|  
    |`(\d+)`|Соответствие как минимум одной цифре и назначение ее для первой захваченной группы.|  
    |`\.`|Сопоставляется точка.|  
  
     Дополнительные сведения о ленивых квантификаторах см. в статье о [квантификаторах в регулярных выражениях](../../../docs/standard/base-types/quantifiers-in-regular-expressions.md).  
  
- Положительный поиск вперед: `(?=`*часть_выражения*`)`. Эта функция позволяет обработчику с поиском с возвратом возвращаться в начальное место в тексте после сопоставления части выражения. Эта функция полезна при осуществлении поиска с одной позиции с помощью нескольких шаблонов. Она также позволяет обработчику проверять существование части строки в конце соответствия, не включая при этом часть строки в сопоставленный текст. В следующем примере используется положительный поиск вперед для извлечения слов в предложении, после которых нет знаков препинания.  
  
     [!code-csharp[Conceptual.RegularExpressions.Design#2](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.regularexpressions.design/cs/lookahead1.cs#2)]
     [!code-vb[Conceptual.RegularExpressions.Design#2](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.regularexpressions.design/vb/lookahead1.vb#2)]  
  
     Определение регулярного выражения `\b[A-Z]+\b(?=\P{P})` показано в таблице ниже.  
  
    |Шаблон|ОПИСАНИЕ|  
    |-------------|-----------------|  
    |`\b`|Совпадение должно начинаться на границе слова.|  
    |`[A-Z]+`|Совпадение с любым символом один или более раз. Поскольку метод <xref:System.Text.RegularExpressions.Regex.Matches%2A?displayProperty=nameWithType> вызывается с параметром <xref:System.Text.RegularExpressions.RegexOptions.IgnoreCase?displayProperty=nameWithType>, при сравнении не учитывается регистр символов.|  
    |`\b`|Совпадение должно заканчиваться на границе слова.|  
    |`(?=\P{P})`|Поиск для определения, является ли следующий символ знаком препинания. Если не является, соответствие считается успешным.|  
  
     Дополнительные сведения об утверждениях положительного поиска вперед см. в статье [Конструкции группировки в регулярных выражениях](../../../docs/standard/base-types/grouping-constructs-in-regular-expressions.md).  
  
- Отрицательный поиск вперед: `(?!`*часть_выражения*`)`. Сопоставление выражения выполняется только в том случае, когда не было обнаружено соответствия части выражения. Это существенно упрощает поиск, поскольку часто бывает проще описать выражения, не соответствующие правилу, чем само правило. Например, трудно написать выражение для поиска слов, которые не начинаются с "non". В следующем примере для их исключения используется отрицательный поиск.  
  
     [!code-csharp[Conceptual.RegularExpressions.Design#3](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.regularexpressions.design/cs/lookahead2.cs#3)]
     [!code-vb[Conceptual.RegularExpressions.Design#3](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.regularexpressions.design/vb/lookahead2.vb#3)]  
  
     Шаблон регулярного выражения `\b(?!non)\w+\b` определяется, как показано в следующей таблице.  
  
    |Шаблон|ОПИСАНИЕ|  
    |-------------|-----------------|  
    |`\b`|Совпадение должно начинаться на границе слова.|  
    |`(?!non)`|Поиск для определения, не начинается ли текущая строка с "non". Если начинается, соответствие считается неудачным.|  
    |`(\w+)`|Совпадение с одним или несколькими символами слова.|  
    |`\b`|Совпадение должно заканчиваться на границе слова.|  
  
     Дополнительные сведения об утверждениях отрицательного поиска вперед см. в статье [Конструкции группировки в регулярных выражениях](../../../docs/standard/base-types/grouping-constructs-in-regular-expressions.md).  
  
- Условная оценка: `(?(`*выражение*`)`*да*`|`*нет*`)` and `(?(`*имя*`)`*да*`|`*нет*`)`, где *выражение* — сопоставляемая часть выражения, *имя* — имя группы захвата, *да* — сопоставляемая строка, если *выражение* найдено в тексте или *имя* является допустимой непустой захваченной группой, а *нет* — сопоставляемая строка, если *выражение* не найдено или *имя* не является допустимой непустой захваченной группой. Эта функция позволяет обработчику вести поиск с использованием нескольких альтернативных шаблонов в зависимости от результатов сопоставлений предыдущей части выражения или утверждения нулевой ширины. Это более действенный вид обратной ссылки, позволяющий, например, искать соответствия части выражения в зависимости от соответствия предыдущей части выражения. Регулярное выражение в следующем примере соответствует абзацам, предназначенным для общего и внутреннего использования. Абзацы, предназначенные только для внутреннего использования, начинаются с тега `<PRIVATE>`. Шаблон регулярного выражения `^(?<Pvt>\<PRIVATE\>\s)?(?(Pvt)((\w+\p{P}?\s)+)|((\w+\p{P}?\s)+))\r?$` использует условную оценку для назначения содержимого абзацев, предназначенных для общего и внутреннего использования, для отдельных захваченных групп. Поэтому эти абзацы можно обработать по-разному.  
  
     [!code-csharp[Conceptual.RegularExpressions.Design#4](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.regularexpressions.design/cs/conditional1.cs#4)]
     [!code-vb[Conceptual.RegularExpressions.Design#4](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.regularexpressions.design/vb/conditional1.vb#4)]  
  
     Шаблон регулярного выражения определяется, как показано в следующей таблице.  
  
    |Шаблон|ОПИСАНИЕ|  
    |-------------|-----------------|  
    |`^`|Начало совпадения в начале строки.|  
    |`(?<Pvt>\<PRIVATE\>\s)?`|Соответствует вхождениям в количестве 0 или 1 строки `<PRIVATE>` за которым следует символ пробела. Назначение соответствия для захваченной группы с именем `Pvt`.|  
    |`(?(Pvt)((\w+\p{P}?\s)+)`|Если захваченная группа `Pvt` существует, сопоставление одного или нескольких вхождений одного или нескольких вхождений символов слов, за которыми следует 0 или 1 пунктуационный разделитель с последующим любым пробелом. Назначение части строки первой захваченной группе.|  
    |<code>&#124;((\w+\p{P}?\s)+))</code>|Если захваченная группа `Pvt` не существует, сопоставление одного или нескольких вхождений одного или нескольких вхождений символов слов, за которыми следует 0 или 1 пунктуационный разделитель с последующим любым пробелом. Назначение части строки третьей захваченной группе.|  
    |`\r?$`|Соответствует концу строки.|  
  
     Дополнительные сведения об условной оценке см. в статье [Конструкции изменения в регулярных выражениях](../../../docs/standard/base-types/alternation-constructs-in-regular-expressions.md).  
  
- Сбалансированные определения группы: `(?<`*имя1*`-`*имя2*`>` *часть_выражения*`)`. Эта функция позволяет обработчику регулярных выражений отслеживать вложенные конструкции, такие как скобки или открывающие и закрывающие круглые скобки. Пример см. в статье [Конструкции группировки в регулярных выражениях](../../../docs/standard/base-types/grouping-constructs-in-regular-expressions.md).  
  
- Невозвращающиеся ("жадные") части выражения: `(?>`*часть_выражения*`)`. Эта функция обеспечивает для подвыражения верность только первого соответствия, как будто выражение запускалось независимо от содержащего его выражения. Без этой конструкции поиск в большом выражении с использованием поиска с возвратом может изменить поведение части выражения. Например, регулярное выражение `(a+)\w` соответствует одному или нескольким символам "a" наряду с буквой, после которой идет последовательность символов "a", и назначает последовательность символов "a" для первой захваченной группы. Однако если последний символ входной строки также является символом "a", он соответствует элементу языка `\w` и не входит в захваченную группу.  
  
     [!code-csharp[Conceptual.RegularExpressions.Design#7](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.regularexpressions.design/cs/nonbacktracking2.cs#7)]
     [!code-vb[Conceptual.RegularExpressions.Design#7](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.regularexpressions.design/vb/nonbacktracking2.vb#7)]  
  
     Регулярное выражение `((?>a+))\w` препятствует такому поведению. Поскольку все последовательные символы "a" имеют соответствия без поиска с возвратом, первая захваченная группа содержит все последовательные символы "a". Если после символов "a" не следует ни один символ, отличный от "a", соответствие считается неудачным.  
  
     [!code-csharp[Conceptual.RegularExpressions.Design#8](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.regularexpressions.design/cs/nonbacktracking1.cs#8)]
     [!code-vb[Conceptual.RegularExpressions.Design#8](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.regularexpressions.design/vb/nonbacktracking1.vb#8)]  
  
     Дополнительные сведения о невозвращающихся частях выражений см. в статье [Конструкции группировки в регулярных выражениях](../../../docs/standard/base-types/grouping-constructs-in-regular-expressions.md).  
  
- Поиск совпадений справа налево, для применения которого нужно передать параметр <xref:System.Text.RegularExpressions.RegexOptions.RightToLeft?displayProperty=nameWithType> в конструктор класса <xref:System.Text.RegularExpressions.Regex> или в статический метод сопоставления экземпляров. Эта функция полезна при поиске справа налево вместо обычного поиска слева направо, а также бывает более эффективно начинать поиск с правой части шаблона, а не с левой. Как показано в примере ниже, использование поиска соответствий справа налево может изменить поведение жадных квантификаторов. В примере выполняется два поисковых запроса предложения, оканчивающегося на число. При поиске слева направо с использованием жадного квантификатора `+` имеется соответствие одной из шести цифр в предложении, тогда как при поиске справа налево — всем шести цифрам. Описание шаблона регулярного выражения см. в примере с ленивыми квантификаторами ранее в этом разделе.  
  
     [!code-csharp[Conceptual.RegularExpressions.Design#6](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.regularexpressions.design/cs/rtl1.cs#6)]
     [!code-vb[Conceptual.RegularExpressions.Design#6](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.regularexpressions.design/vb/rtl1.vb#6)]  
  
     Дополнительные сведения о поиске соответствий справа налево см. в разделе [Параметры регулярных выражений](../../../docs/standard/base-types/regular-expression-options.md).  
  
- Положительный и отрицательный поиск назад: `(?<=`*часть_выражения*`)` для положительного поиска назад и `(?<!`*часть_выражения*`)` для отрицательного просмотра назад. Эта функция аналогична поиску вперед, рассмотренному ранее в этом разделе. Поскольку обработчик регулярных выражений позволяет выполнять поиск справа налево, к регулярным выражениям можно применять поиск назад без каких-либо ограничений. С помощью положительного и отрицательного поиска назад также можно избегать вложенных квантификаторов, когда вложенная часть выражения является супермножеством внешнего выражения. Регулярные выражения с такими вложенными квантификаторами часто являются причиной низкой производительности. В следующем примере выполняется проверка, начинается ли и оканчивается ли строка с буквы или цифры, а также является ли любой другой символ в строке символом большего подмножества. В результате формируется часть регулярного выражения для проверки адресов электронной почты. Дополнительные сведения см. в статье [Руководство. Проверка строк на соответствие формату электронной почты](../../../docs/standard/base-types/how-to-verify-that-strings-are-in-valid-email-format.md).  
  
     [!code-csharp[Conceptual.RegularExpressions.Design#5](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.regularexpressions.design/cs/lookbehind1.cs#5)]
     [!code-vb[Conceptual.RegularExpressions.Design#5](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.regularexpressions.design/vb/lookbehind1.vb#5)]  
  
     Определение регулярного выражения ``^[A-Z0-9]([-!#$%&'.*+/=?^`{}|~\w])*(?<=[A-Z0-9])$`` показано в таблице ниже.  
  
    |Шаблон|ОПИСАНИЕ|  
    |-------------|-----------------|  
    |`^`|Начало совпадения в начале строки.|  
    |`[A-Z0-9]`|Соответствие любому алфавитно-цифровому символу. (При сравнении регистр не учитывается.)|  
    |<code>([-!#$%&'.*+/=?^\`{}&#124;~\w])\*</code>|Соответствие нулю или нескольким вхождениям любого символа слова или любого символа из следующего набора:  -, !, #, $, %, &, ', ., \*, +, /, =, ?, ^, \`, {, }, &#124; или ~.|  
    |`(?<=[A-Z0-9])`|Поиск назад предыдущего символа, который должен являться числом или буквенно-цифровым символом. (При сравнении регистр не учитывается.)|  
    |`$`|Совпадение должно заканчиваться в конце строки.|  
  
     Дополнительные сведения о положительном и отрицательном поиске назад см. в статье [Конструкции группировки в регулярных выражениях](../../../docs/standard/base-types/grouping-constructs-in-regular-expressions.md).  
  
## <a name="related-topics"></a>См. также  
  
|Заголовок|ОПИСАНИЕ|  
|-----------|-----------------|  
|[Поиск с возвратом](../../../docs/standard/base-types/backtracking-in-regular-expressions.md)|Сведения об использовании поиска с возвратом для поиска альтернативных соответствий.|  
|[Компиляция и многократное использование](../../../docs/standard/base-types/compilation-and-reuse-in-regular-expressions.md)|Сведения о компиляции и многократном использовании регулярных выражений для повышения производительности.|  
|[Потокобезопасность](../../../docs/standard/base-types/thread-safety-in-regular-expressions.md)|Сведения о потокобезопасности регулярных выражений и времени синхронизации доступа к объектам регулярных выражений.|  
|[Регулярные выражения в .NET Framework](../../../docs/standard/base-types/regular-expressions.md)|Общие сведения о регулярных выражениях в контексте языка программирования.|  
|[Объектная модель регулярных выражений](../../../docs/standard/base-types/the-regular-expression-object-model.md)|Сведения об использовании классов регулярных выражений и примеры кода.|  
|[Примеры регулярных выражений](../../../docs/standard/base-types/regular-expression-examples.md)|Примеры кодов, иллюстрирующих использование регулярных выражений в обычных приложениях.|  
|[Элементы языка регулярных выражений — краткий справочник](../../../docs/standard/base-types/regular-expression-language-quick-reference.md)|Сведения о наборе символов, операторов и конструкций, которые можно использовать для определения регулярных выражений.|  
  
## <a name="reference"></a>Справочник  
 <xref:System.Text.RegularExpressions?displayProperty=nameWithType>
