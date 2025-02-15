---
title: Resgen.exe (генератор файлов ресурсов)
ms.date: 03/30/2017
helpviewer_keywords:
- resource files, .resources files
- resource files, .resx files
- resx files (resource files)
- .resources files
- files, converting
- Resource File Generator
- .resx files
- Resgen.exe
- resource files, converting
- converting files, Resource File Generator
- binary resources files
- embedding files in runtime binary executable
ms.assetid: 8ef159de-b660-4bec-9213-c3fbc4d1c6f4
ms.openlocfilehash: 4605c7361705ba37091eb2e34d8425810854973e
ms.sourcegitcommit: 559fcfbe4871636494870a8b716bf7325df34ac5
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/30/2019
ms.locfileid: "73104895"
---
# <a name="resgenexe-resource-file-generator"></a>Resgen.exe (генератор файлов ресурсов)
Генератор файлов ресурсов (Resgen.exe) преобразует текстовые файлы (TXT или RESTEXT) и файлы ресурсов на основе XML (RESX) в двоичные файлы среды CLR (RESOURCES), которые можно внедрить в двоичный исполняемый файл среды выполнения или вспомогательную сборку. (См. раздел [Создание файлов ресурсов](../resources/creating-resource-files-for-desktop-apps.md).)  
  
 Программа Resgen.exe представляет собой универсальную служебную программу для преобразования ресурсов, которая выполняет следующие задачи.  
  
- Преобразование TXT-файлов и RESTEXT-файлов в файлы RESOURCES или RESX. (Формат RESTEXT-файлов идентичен формату TXT-файлов. Однако с помощью расширения RESTEXT легче идентифицировать текстовые файлы, содержащие определения ресурсов.)  
  
- Преобразование RESOURCES-файлов в RESX-файлы или текстовые файлы.  
  
- Преобразование RESX-файлов в RESOURCES-файлы или текстовые файлы.  
  
- Извлекает строковые ресурсы из сборки в RESW-файл, который пригоден для использования в приложении [!INCLUDE[win8_appname_long](../../../includes/win8-appname-long-md.md)].  
  
- Создает класс со строгой типизацией, который предоставляет доступ к ресурсам с отдельными именами и экземпляру <xref:System.Resources.ResourceManager>.  
  
 В случае любого сбоя программа Resgen.exe возвращается значение "–1".  
  
 Чтобы получить справочные сведения по синтаксису команд и параметрам программы Resgen.exe, можно ввести следующую команду без параметров.  
  
```console  
resgen  
```  
  
 Также можно использовать ключ `/?`.  
  
```console  
resgen /?  
```  
  
 При использовании программы Resgen.exe для создания двоичных RESOURCES-файлов можно воспользоваться языковым компилятором, чтобы внедрить двоичные файлы в исполняемые сборки, или [Компоновщиком сборок (Al.exe)](al-exe-assembly-linker.md), чтобы скомпилировать их во вспомогательные сборки.  
  
 Эта программа автоматически устанавливается вместе с Visual Studio. Чтобы применить этот инструмент, воспользуйтесь командной строкой разработчика для Visual Studio (или командной строкой Visual Studio в Windows 7). Дополнительные сведения см. в разделе [Командные строки](developer-command-prompt-for-vs.md).  
  
 В командной строке введите следующее.  
  
## <a name="syntax"></a>Синтаксис  
  
```console  
resgen  [-define:symbol1[,symbol2,...]] [/useSourcePath] filename.extension  | /compile filename.extension... [outputFilename.extension] [/r:assembly] [/str:lang[,namespace[,class[,file]]] [/publicclass]]   
```  
  
```console  
resgen filename.extension [outputDirectory]  
```  
  
## <a name="parameters"></a>Параметры  
  
|Параметр или ключ|ОПИСАНИЕ|  
|-------------------------|-----------------|  
|`/define:` *symbol1*[, *symbol2*,...]|Начиная с .NET Framework 4.5 поддерживается условная компиляция в файлы ресурсов на основе текста (TXT или RESTEXT). Если *symbol* соответствует символу, включенному во входной текстовый файл в конструкции `#ifdef`, соответствующий строковый ресурс включается в RESOURCES-файл. Если входной текстовый файл содержит оператор `#if !` с символом, который не определен ключом `/define`, соответствующий строковый ресурс включается в файл ресурсов.<br /><br /> Параметр `/define` игнорируется, если он применяется для нетекстовых файлов. В символах учитывается регистр.<br /><br /> Дополнительные сведения об этом параметре см. в разделе [Условная компиляция ресурсов](#Conditional) ниже.|  
|`useSourcePath`|Задает использование текущего каталога входного файла для разрешения относительных путей к файлам.|  
|`/compile`|Позволяет указать несколько RESX-файлов или текстовых файлов, которые следует преобразовать в несколько RESOURCES-файлов в ходе одной групповой операции. Если этот параметр не указывается, можно задать только один аргумент входного файла. Имена выходных файлов имеют формат *имя_файла*.resources.<br /><br /> Данный параметр невозможно использовать одновременно с параметром `/str:`.<br /><br /> Дополнительные сведения об этом параметре см. в разделе [Компиляция или преобразование нескольких файлов](#Multiple) ниже.|  
|`/r:` `assembly`|Ссылается на метаданные из указанной сборки. Он используется при преобразовании RESX-файлов и позволяет программе Resgen.exe выполнять сериализацию или десериализацию ресурсов объектов. Он похож на параметр `/reference:` или `/r:` для компиляторов C# и Visual Basic.|  
|`filename.extension`|Задает имя входного файла, который требуется преобразовать. Если используется первый, более длинный синтаксис командной строки, представленный перед этой таблицей, параметр `extension` должен быть одним из следующих.<br /><br /> TXT или RESTEXT<br /> Текстовый файл, который преобразуется в файл с расширением RESOURCES или RESX. Текстовые файлы могут содержать только строковые ресурсы. Дополнительные сведения о формате файлов см. в разделе "Ресурсы в текстовых файлах" в статье [Создание файлов ресурсов](../resources/creating-resource-files-for-desktop-apps.md).<br /><br /> RESX<br /> Файл ресурсов на основе XML, который преобразуется в RESOURCES-файл или текстовый файл (с расширением TXT или RESTEXT).<br /><br /> RESOURCES<br /> Двоичный файл ресурсов, который преобразуется в RESX-файл или текстовый файл (с расширением TXT или RESTEXT).<br /><br /> Если используется второй, более короткий синтаксис командной строки, представленный перед этой таблицей, параметр `extension` должен быть одним из следующих.<br /><br /> EXE или DLL<br /> Сборка .NET Framework (исполняемый файл или библиотека), строковые ресурсы которой должны извлекаться в RESW-файл для последующего использования при разработке приложений [!INCLUDE[win8_appname_long](../../../includes/win8-appname-long-md.md)].|  
|`outputFilename.extension`|Задает имя и тип создаваемого файла ресурсов.<br /><br /> При преобразовании из файлов с расширением TXT, RESTEXT или RESX в RESOURCES-файл этот аргумент необязателен. Если аргумент `outputFilename` не указан, программа Resgen.exe добавляет расширение RESOURCES к входному файлу `filename` и записывает этот файл в каталог, который содержит `filename,extension`.<br /><br /> Аргумент `outputFilename.extension` является обязательным при преобразовании из RESOURCES-файла. Для преобразования RESOURCES-файла в файл ресурсов на основе XML следует указать имя файла. Для преобразования RESOURCES-файла в текстовый файл следует указать имя файла с расширением TXT или RESTEXT. RESOURCES-файл можно преобразовать в TXT-файл только в том случае, если RESOURCES-файл содержит только строковые значения.|  
|`outputDirectory`|Для приложений [!INCLUDE[win8_appname_long](../../../includes/win8-appname-long-md.md)] задает каталог, в котором будет сохранен RESW-файл, содержащий строковые ресурсы в `filename.extension`. Каталог `outputDirectory` должен существовать.|  
|`/str:` `language[,namespace[,classname[,filename]]]`|Создает файл класса ресурса со строгой типизацией на языке программирования, указанном в параметре `language`. `language` может состоять из одного из следующих литералов.<br /><br /> Для C#: `c#`, `cs` или `csharp`.<br />Для Visual Basic: `vb` или `visualbasic`.<br />Для VBScript: `vbs` или `vbscript`.<br />Для C++: `c++`, `mc` или `cpp`.<br />Для JavaScript: `js`, `jscript` или `javascript`.<br /><br /> Параметр `namespace` задает для проекта пространство имен по умолчанию, параметр `classname` задает имя создаваемого класса, а параметр `filename` — имя файла класса.<br /><br /> С помощью параметра `/str:` можно указать только один входной файл, поэтому этот параметр нельзя использовать с параметром `/compile`.<br /><br /> Если указан параметр `namespace`, а параметр `classname` — нет, имя класса задается на основе имени выходного файла (например, точки заменяются символами подчеркивания). В результате ресурсы со строгой типизацией могут работать неправильно. Чтобы этого избежать, укажите и имя класса, и имя выходного файла.<br /><br /> Дополнительные сведения об этом параметре см. в разделе [Создание класса ресурсов со строгой типизацией](#Strong) ниже.|  
|`/publicClass`|Создает класс ресурса со строгой типизацией как открытый класс. По умолчанию классом ресурса в C# является `internal`, а в Visual Basic — `Friend`.<br /><br /> Этот параметр игнорируется, если не указан параметр `/str:`.|  
  
## <a name="resgenexe-and-resource-file-types"></a>Программа Resgen.exe и типы файлов ресурсов  
 Чтобы программа Resgen.exe могла успешно выполнять преобразование, файлы ресурсов, текстовые файлы и RESX-файлы должны иметь правильный формат.  
  
### <a name="text-txt-and-restext-files"></a>Текстовые файлы (TXT и RESTEXT)  
 Текстовые файлы (TXT или RESTEXT) могут содержать только строковые ресурсы. Строковые ресурсы используются при создании приложений, содержащих строки, которые будут переводиться на разные языки. Например, строки меню можно легко локализовать с помощью соответствующего строкового ресурса. Программа Resgen.exe считывает текстовые файлы, содержащие пары имя/значение, где имя представляет собой строку, описывающую ресурс, а значение является собственно строкой ресурса.  
  
> [!NOTE]
> Дополнительные сведения о формате TXT- и RESTEXT-файлов см. в разделе "Ресурсы в текстовых файлах" в статье [Создание файлов ресурсов](../resources/creating-resource-files-for-desktop-apps.md).  
  
 Текстовый файл, содержащий ресурсы, должен быть сохранен в кодировке UTF-8 или Юникод (UTF-16), кроме тех случаев, когда он содержит только символы основной латиницы (до U+007F). Программа Resgen.exe удаляет знаки расширенного набора ANSI при обработке текстового файла, который сохранен в кодировке ANSI.  
  
 Программа Resgen.exe проверяет текстовый файл на наличие повторяющихся имен ресурсов. Если в текстовом файле содержатся совпадающие имена ресурсов, программа Resgen.exe выведет предупреждение и проигнорирует второе значение.  
  
### <a name="resx-files"></a>RESX-файлы  
 Файл ресурсов RESX состоит из записей XML. В этих записях XML можно указывать строковые ресурсы как в текстовых файлах. Основное преимущество RESX-файлов по сравнению с текстовыми файлами состоит в том, что в них можно также указывать или внедрять объекты. При просмотре RESX-файла внедренный объект (например, рисунок) отображается в двоичном виде, если эти двоичные данные являются частью манифеста ресурса. Как и текстовые файлы, RESX-файл можно открыть в текстовом редакторе (например, в Notepad или Microsoft Word), чтобы записывать, анализировать или иным образом обрабатывать его содержимое. Следует отметить, что для этого необходимо хорошо знать теги XML и структуру RESX-файла. Дополнительные сведения о формате RESX-файла см. в разделе "Ресурсы в RESX-файлах" в статье [Создание файлов ресурсов](../resources/creating-resource-files-for-desktop-apps.md).  
  
 Чтобы создать RESOURCES-файл, содержащий внедренные нестроковые объекты, можно преобразовать с помощью программы Resgen.exe RESX-файл, содержащий объекты, или добавить ресурсы объектов из кода непосредственно в файл, используя методы класса <xref:System.Resources.ResourceWriter>.  
  
 При использовании программы Resgen.exe для преобразования RESX-файла или RESOURCES-файла, содержащего объекты, в текстовый файл все строковые ресурсы будут преобразованы правильно, но при этом типы данных нестроковых объектов также будут записаны в файл в виде строк. При таком преобразовании внедренные объекты будут потеряны, а программа Resgen.exe выдаст сообщение об ошибке извлечения ресурсов.  
  
### <a name="converting-between-resources-file-types"></a>Преобразование типов файлов ресурсов  
 При преобразовании разных типов файлов ресурсов программа Resgen.exe может быть не в состоянии выполнять преобразование или может терять сведения о конкретных ресурсах в зависимости от типа исходного и конечного файлов. В следующей таблице указаны типы успешных преобразований из файла ресурсов одного типа в другой.  
  
|Тип, из которого выполняется преобразование|В текстовый файл|В RESX-файл|В RESW-файл|В RESOURCES-файл|  
|------------------|------------------|-------------------|-------------------|------------------------|  
|Текстовый файл (TXT или RESTEXT)|--|Без проблем|Не поддерживается|Без проблем|  
|RESX-файл|Преобразование завершается неудачей, если файл содержит нестроковые ресурсы (включая файловые связи)|--|Не поддерживается|Без проблем|  
|RESOURCES-файл|Преобразование завершается неудачей, если файл содержит нестроковые ресурсы (включая файловые связи)|Без проблем|Не поддерживается|--|  
|Сборка EXE или DLL|Не поддерживается|Не поддерживается|Только строковые ресурсы (включая пути) считаются ресурсами|Не поддерживается|  
  
## <a name="performing-specific-resgenexe-tasks"></a>Выполнение конкретных задач Resgen.exe  
 Программу Resgen.exe можно использовать разными способами: компилировать файл ресурсов на основе текста или XML в двоичный файл, выполнять преобразование файлов в разные форматы, создавать класс, который образует оболочку для функциональных возможностей <xref:System.Resources.ResourceManager> и предоставляет доступ к ресурсам. В данном разделе представлены подробные сведения по каждой задаче.  
  
- [Компиляция ресурсов в двоичный файл](resgen-exe-resource-file-generator.md#Compiling)  
  
- [Преобразование типов файлов ресурсов](resgen-exe-resource-file-generator.md#Convert)  
  
- [Компиляция или преобразование нескольких файлов](resgen-exe-resource-file-generator.md#Multiple)  
  
- [Экспорт ресурсов в RESW-файл](resgen-exe-resource-file-generator.md#Exporting)  
  
- [Условная компиляция ресурсов](resgen-exe-resource-file-generator.md#Conditional)  
  
- [Создание класса ресурсов со строгой типизацией](resgen-exe-resource-file-generator.md#Strong)  
  
<a name="Compiling"></a>   
### <a name="compiling-resources-into-a-binary-file"></a>Компиляция ресурсов в двоичный файл  
 Чаще всего программа Resgen.exe используется для компиляции файла ресурсов на основе текста (TXT или RESTEXT) или XML (RESX) в двоичный RESOURCES-файл. После компиляции выходной файл можно встроить в основную сборку с помощью языкового компилятора или во вспомогательную сборку с помощью [Компоновщика сборок (AL.exe)](al-exe-assembly-linker.md).  
  
 Синтаксис компиляции файла ресурсов выглядит следующим образом.  
  
```console  
resgen inputFilename [outputFilename]   
```  
  
 Используются следующие параметры.  
  
 `inputFilename`  
 Имя компилируемого файла ресурсов, включая расширение. Программа Resgen.exe компилирует только файлы с расширениями TXT, RESTEXT или RESX.  
  
 `outputFilename`  
 Имя выходного файла. Если не указывать `outputFilename`, программа Resgen.exe создаст RESOURCES-файл с корневым именем файла `inputFilename` в том же каталоге, что и `inputFilename`. Если `outputFilename` содержит путь к каталогу, этот каталог должен существовать.  
  
 Полное имя пространства имен для RESOURCES-файла указывается в имени файла и отделяется от корневого имени файла точкой. Например, если параметр `outputFilename` имеет значение `MyCompany.Libraries.Strings.resources`, пространство имен имеет вид "MyCompany.Libraries".  
  
 Следующая команда считывает пары имя/значение из файла "Resources.txt" и создает двоичный RESOURCES-файл с именем "Resources.resources". Поскольку имя выходного файла не указано явным образом, его имя по умолчанию аналогично имени входного файла.  
  
```console  
resgen Resources.txt   
```  
  
 Следующая команда считывает пары имя/значение из файла "Resources.restext" и создает двоичный файл ресурсов с именем "StringResources.resources".  
  
```console  
resgen Resources.restext StringResources.resources  
```  
  
 Следующая команда считывает входной файл на основе XML "Resources.resx" и создает двоичный RESOURCES-файл с именем "Resources.resources".  
  
```console  
resgen Resources.resx Resources.resources  
```  
  
<a name="Convert"></a>   
### <a name="converting-between-resource-file-types"></a>Преобразование типов файлов ресурсов  
 Помимо компиляции файлов ресурсов на основе текста или XML в двоичные файлы ресурсов программа Resgen.exe может преобразовать любой поддерживаемый тип файлов в другой. Это означает, что программа может выполнить следующие преобразования.  
  
- TXT-файлы и RESTEXT-файлы в RESX-файлы.  
  
- RESX-файлы в TXT-файлы и RESTEXT-файлы.  
  
- RESOURCES-файлы в TXT-файлы и RESTEXT-файлы.  
  
- RESOURCES-файлы в RESX-файлы.  
  
 Синтаксис аналогичен синтаксису, который показан в предыдущем разделе.  
  
 Кроме того, с помощью программы Resgen.exe можно выполнять преобразование внедренных ресурсов в сборке .NET Framework в RESW-файл для приложений [!INCLUDE[win8_appname_long](../../../includes/win8-appname-long-md.md)].  
  
 Следующая команда считывает двоичный файл ресурсов "Resources.resources" и создает выходной файл на основе XML с именем "Resources.resx".  
  
```console  
resgen Resources.resources Resources.resx  
```  
  
 Следующая команда считывает файл ресурсов на основе текста "StringResources.txt" и создает файл ресурсов на основе XML с именем "LibraryResources.resx". Помимо хранения строковых ресурсов RESX-файл можно также использовать для хранения нестроковых ресурсов.  
  
```console  
resgen StringResources.txt LibraryResources.resx  
```  
  
 Следующие две команды считывают файл ресурсов на основе XML "Resources.resx" и создают текстовые файлы с именами "Resources.txt" и "Resources.restext". Следует заметить, что если RESX-файл содержит внедренные объекты, они будут неправильно преобразованы в текстовые файлы.  
  
```console  
resgen Resources.resx Resources.txt  
resgen Resources.resx Resources.restext  
```  
  
<a name="Multiple"></a>   
### <a name="compiling-or-converting-multiple-files"></a>Компиляция или преобразование нескольких файлов  
 С помощью ключа `/compile` можно преобразовать список файлов ресурсов из одного формата в другой за одну операцию. Синтаксис выглядит следующим образом.  
  
```console  
resgen /compile filename.extension [filename.extension...]  
```  
  
 Следующая команда компилирует три файла — "StringResources.txt", "TableResources.resw" и "ImageResources.resw" — в отдельные RESOURCES-файлы с именами "StringResources.resources", "TableResources.resources" и "ImageResources.resources".  
  
```console  
resgen /compile StringResources.txt TableResources.resx ImageResources.resx  
```  
  
<a name="Exporting"></a>   
### <a name="exporting-resources-to-a-resw-file"></a>Экспорт ресурсов в RESW-файл  
 При разработке приложения [!INCLUDE[win8_appname_long](../../../includes/win8-appname-long-md.md)] может понадобиться использовать ресурсы из существующего классического приложения. Однако два типа приложений поддерживают разные форматы файлов. В классических приложениях ресурсы в текстовых файлах (TXT или RESTEXT) или RESX-файлах компилируются в двоичные RESOURCES-файлы. В приложениях [!INCLUDE[win8_appname_long](../../../includes/win8-appname-long-md.md)] RESW-файлы компилируются в двоичные PRI-файлы. Чтобы решить эту проблему, с помощью программы Resgen.exe можно извлекать ресурсы из исполняемого файла или вспомогательной сборки и записывать их в один или несколько RESW-файлов, которые можно использовать при разработке приложения [!INCLUDE[win8_appname_long](../../../includes/win8-appname-long-md.md)].  
  
> [!IMPORTANT]
> Visual Studio автоматически обрабатывает все преобразования, необходимые для включения ресурсов в переносимой библиотеке в приложение [!INCLUDE[win8_appname_long](../../../includes/win8-appname-long-md.md)]. Использование программы Resgen.exe для преобразования ресурсов в сборке в файл формата RESW напрямую представляет интерес только для разработчиков, которые разрабатывают приложения [!INCLUDE[win8_appname_long](../../../includes/win8-appname-long-md.md)] без Visual Studio.  
  
 Синтаксис для создания RESW-файлов из сборки выглядит следующим образом.  
  
```console  
resgen filename.extension  [outputDirectory]  
```  
  
 Используются следующие параметры.  
  
 `filename.extension`  
 Имя сборки .NET Framework (исполняемый файл или библиотека DLL). Если файл не содержит ресурсы, программа Resgen.exe не создает файлы.  
  
 `outputDirectory`  
 Существующий каталог, в котором сохраняются RESW-файлы. Если параметр `outputDirectory` не задан, RESW-файлы записываются в текущий каталог. Программа Resgen.exe создает один RESW-файл для каждого RESOURCES-файла в сборке. Корневое имя RESW-файла аналогично корневому имени RESOURCES-файла.  
  
 Следующая команда создает RESW-файл в каталоге "Win8Resources" для каждого RESOURCES-файла, внедренного в приложение MyApp.exe.  
  
```console  
resgen MyApp.exe Win8Resources  
```  
  
<a name="Conditional"></a>   
### <a name="conditionally-compiling-resources"></a>Условная компиляция ресурсов  
 Начиная с .NET Framework 4.5 программа Resgen.exe поддерживает условную компиляцию строковых ресурсов в текстовых файлах (TXT и RESTEXT). Благодаря такому подходу можно использовать один файл ресурсов на основе текста для нескольких конфигураций построения.  
  
 В TXT- или RESTEXT-файле используется конструкция `#ifdef`...`#endif`, чтобы включить ресурс в двоичный RESOURCES-файл, если символ задан, а конструкция `#if !`... `#endif` используется, чтобы включить ресурс, если символ не задан. Во время компиляции символы задаются с помощью параметра `/define:`, за которым следует список разделенных запятыми символов. В сравнении учитывается регистр символов, который задается параметром `/define` и должен соответствовать регистру символов в компилируемых текстовых файлах.  
  
 Например, следующий файл с именем "UIResources.rext" содержит строковый ресурс с именем `AppTitle`, который может принимать одно из трех значений в зависимости от заданного символа: `PRODUCTION`, `CONSULT` или `RETAIL`.  
  
```text
#ifdef PRODUCTION  
AppTitle=My Software Company Project Manager   
#endif  
#ifdef CONSULT  
AppTitle=My Consulting Company Project Manager  
#endif  
#ifdef RETAIL  
AppTitle=My Retail Store Project Manager  
#endif  
FileMenuName=File  
```  
  
 После этого файл можно скомпилировать в двоичный RESOURCES-файл с помощью следующей команды.  
  
```console  
resgen /define:CONSULT UIResources.restext  
```  
  
 При этом создается RESOURCES-файл, содержащий два строковых ресурса. Значение ресурса `AppTitle` — "My Consulting Company Project Manager".  
  
<a name="Strong"></a>   
### <a name="generating-a-strongly-typed-resource-class"></a>Создание класса ресурсов со строгой типизацией  
 Программа Resgen.exe поддерживает ресурсы со строгой типизацией, при этом доступ к ресурсам инкапсулируется путем создания классов, содержащих набор статических свойств только для чтения. Это альтернативный способ прямого вызова методов класса <xref:System.Resources.ResourceManager> для извлечения ресурсов. Можно включить поддержку ресурсов со строгой типизацией с помощью параметра `/str` в программе Resgen.exe, при которой реализуются функциональные возможности класса <xref:System.Resources.Tools.StronglyTypedResourceBuilder>. Если задан параметр `/str`, результатом работы программы Resgen.exe будет класс, содержащий свойства со строгой типизацией, которые соответствуют ресурсам, указанным входным параметром. Этот класс обеспечивает доступ только для чтения со строгой типизацией к ресурсам в отрабатываемом файле.  
  
 Синтаксис для создания ресурса со строгой типизацией выглядит следующим образом.  
  
```console  
resgen inputFilename [outputFilename] /str:language[,namespace,[classname[,filename]]] [/publicClass]  
```  
  
 Параметры и ключи.  
  
 `inputFilename`  
 Имя файла ресурсов, включая расширение, для которого создается класс ресурса со строгой типизацией. Файл может быть двоичным RESOURCES-файлом или RESOURCES-файлом на основе текста или XML с расширением RESW, TXT, RESTEXT или RESOURCES.  
  
 `outputFilename`  
 Имя выходного файла. Если `outputFilename` содержит путь к каталогу, этот каталог должен существовать. Если не указывать `outputFilename`, программа Resgen.exe создаст RESOURCES-файл с корневым именем файла `inputFilename` в том же каталоге, что и `inputFilename`.  
  
 Файл `outputFilename` может быть двоичным RESOURCES-файлом или файлом на основе текста или XML. Если расширение файла `outputFilename` отличается от расширения файла `inputFilename`, программа Resgen.exe выполняет преобразование файла.  
  
 Если файл `inputFilename` является RESOURCES-файлом, программа Resgen.exe копирует RESOURCES-файл, если файл `outputFilename` также является RESOURCES-файлом. Если `outputFilename` не задано, программа Resgen.exe перезаписывает файл `inputFilename` аналогичным RESOURCES-файлом.  
  
 *language*  
 Язык, применяемый для создания исходного кода для класса ресурсов со строгой типизацией. Возможные значения: `cs`, `C#`, и `csharp` для кода на C# `vb` и `visualbasic` для кода на Visual Basic; `vbs` и `vbscript` для кода на VBScript; и `c++`, `mc` и `cpp` для кода на C++.  
  
 *namespace*  
 Пространство имен, содержащее класс ресурсов со строгой типизацией. RESOURCES-файл и класс ресурсов должны иметь одно и то же пространство имен. Дополнительные сведения о задании пространства имен в параметре `outputFilename` см. в разделе [Компиляция ресурсов в двоичный файл](resgen-exe-resource-file-generator.md#Compiling). Если параметр *namespace* не задан, класс ресурсов отсутствует в пространстве имен.  
  
 *classname*  
 Имя класса ресурсов со строгой типизацией. Это имя должно совпадать с корневым именем RESOURCES-файла. Например, если программа Resgen.exe создает в RESOURCES-файл с именем "MyCompany.Libraries.Strings.resources", то именем класса ресурсов со строгой типизацией будет "String". Если параметр *classname* не задан, созданный класс является производным от корневого имени файла `outputFilename`. Если параметр `outputFilename` не задан, созданный класс является производным от корневого имени файла `inputFilename`.  
  
 Имя *classname* не должно содержать недопустимые символы, например пробелы. Если имя *classname* содержит пробелы или имя *classname* создается по умолчанию на основе *inputFilename* и *inputFilename* содержит пробелы, программа Resgen.exe заменяет недопустимые символы на символ подчеркивания (\_).  
  
 *filename*  
 Имя файла класса.  
  
 `/publicclass`  
 Делает класс ресурса со строгой типизацией открытым, а не `internal` (в C#) или `Friend` (в Visual Basic). С помощью такого решения можно оценивать ресурсы за пределами сборки, в которую они внедрены.  
  
> [!IMPORTANT]
> При создании класса ресурсов со строгой типизацией имя RESOURCES-файла должно совпадать с именем пространства имен и класса для созданного кода. Однако программа Resgen.exe позволяет задавать параметры, которые приводят к созданию RESOURCES-файла с несовместимым именем. Для временного решения этой проблемы переименуйте выходной файл после его создания.  
  
 Класс ресурсов со строгой типизацией включает в себя следующие элементы.  
  
- Конструктор без параметров, который можно использовать создания экземпляра класс ресурсов со строгой типизацией.  
  
- Свойство `static` (C#) или `Shared` (Visual Basic) и свойство `ResourceManager` только для чтения, которое возвращает экземпляр <xref:System.Resources.ResourceManager>, который управляет ресурсом со строгой типизацией.  
  
- Статическое свойство `Culture`, с помощью которого можно задать язык и региональные параметры, используемые для извлечения ресурсов. По умолчанию его значение равно `null`, то есть для пользовательского интерфейса используются текущий язык и региональные параметры.  
  
- Одно свойство `static` (C#) или `Shared` (Visual Basic) и свойство только для чтения для каждого ресурса в RESOURCES-файле. Имя свойства является именем ресурса.  
  
 Например, при выполнении следующей команды выполняется компиляция файла ресурсов с именем "StringResources.txt" в "StringResources.resources" и создается класс с именем `StringResources` в файле исходного кода Visual Basic с именем "StringResources.vb", который можно использовать для доступа к диспетчеру ресурсов.  
  
```console  
resgen StringResources.txt /str:vb,,StringResources   
```  
  
## <a name="see-also"></a>См. также

- [Инструменты](index.md)
- [Ресурсы в приложениях для настольных систем](../resources/index.md)
- [Создание файлов ресурсов](../resources/creating-resource-files-for-desktop-apps.md)
- [Al.exe (компоновщик сборок)](al-exe-assembly-linker.md)
- [Командные строки](developer-command-prompt-for-vs.md)
