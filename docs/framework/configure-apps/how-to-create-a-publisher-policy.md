---
title: Практическое руководство. Создание политики издателя
ms.date: 03/30/2017
helpviewer_keywords:
- publisher policy assembly
- publisher policy files
- GAC (global assembly cache), publisher policy assembly
- global assembly cache, publisher policy assembly
ms.assetid: 8046bc5d-2fa9-4277-8a5e-6dcc96c281d9
ms.openlocfilehash: 346671d4febd5f3999f1f4fbf2fe4b7e475ae5fa
ms.sourcegitcommit: ad800f019ac976cb669e635fb0ea49db740e6890
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/29/2019
ms.locfileid: "73040190"
---
# <a name="how-to-create-a-publisher-policy"></a>Практическое руководство. Создание политики издателя

Поставщики сборок могут указать, что приложения должны использовать более новую версию сборки, включив файл политики издателя с обновленной сборкой. Файл политики издателя определяет параметры перенаправления сборок и базы кода, а также использует тот же формат, что и файл конфигурации приложения. Файл политики издателя компилируется в сборку и помещается в глобальный кэш сборок.

Создание политики издателя состоит из трех этапов.

1. Создайте файл политики издателя.

2. Создание сборки политики издателя.

3. Добавьте сборку политики издателя в глобальный кэш сборок.

Схема для политики издателя описана в разделе [Перенаправление версий сборки](redirect-assembly-versions.md). В следующем примере показан файл политики издателя, который перенаправляет одну версию `myAssembly` в другую.

```xml
<configuration>
   <runtime>
      <assemblyBinding xmlns="urn:schemas-microsoft-com:asm.v1">
       <dependentAssembly>
         <assemblyIdentity name="myAssembly"
                           publicKeyToken="32ab4ba45e0a69a1"
                           culture="en-us" />
         <!-- Redirecting to version 2.0.0.0 of the assembly. -->
         <bindingRedirect oldVersion="1.0.0.0"
                          newVersion="2.0.0.0"/>
       </dependentAssembly>
      </assemblyBinding>
   </runtime>
</configuration>
```

Сведения о том, как указать базу кода, см. в разделе [Указание расположения сборки](specify-assembly-location.md).

## <a name="creating-the-publisher-policy-assembly"></a>Создание сборки политики издателя

Используйте [Компоновщик сборок (Al. exe)](../tools/al-exe-assembly-linker.md) для создания сборки политики издателя.

#### <a name="to-create-a-publisher-policy-assembly"></a>Создание сборки политики издателя

В командной строке введите следующую команду:

```console
al /link:publisherPolicyFile /out:publisherPolicyAssemblyFile /keyfile:keyPairFile /platform:processorArchitecture
```

В этой команде:

- Аргумент `publisherPolicyFile` — это имя файла политики издателя.

- Аргумент `publisherPolicyAssemblyFile` — это имя сборки политики издателя, полученное в результате выполнения этой команды. Имя файла сборки должно соответствовать формату:

  "Policy. Мажорнумбер. Минорнумбер. Маинассемблинаме. dll"

- Аргумент `keyPairFile` — это имя файла, содержащего пару ключей. Сборку политики сборки и издателя необходимо подписать с помощью той же пары ключей.

- Аргумент `processorArchitecture` определяет платформу, на которую ссылается сборка для конкретного процессора.

  > [!NOTE]
  > Возможность ориентироваться на конкретную архитектуру процессора доступна начиная с .NET Framework 2,0.

Возможность ориентироваться на конкретную архитектуру процессора доступна начиная с .NET Framework 2,0. Следующая команда создает сборку политики издателя с именем `policy.1.0.myAssembly` из файла политики издателя с именем `pub.config`, присваивает сборке строгое имя с помощью пары ключей в файле `sgKey.snk` и указывает, что сборка предназначена для процессора x86. AHA.

```console
al /link:pub.config /out:policy.1.0.myAssembly.dll /keyfile:sgKey.snk /platform:x86
```

Сборка политики издателя должна соответствовать архитектуре процессора сборки, к которой он применяется. Таким же, если сборка имеет <xref:System.Reflection.AssemblyName.ProcessorArchitecture%2A> значение <xref:System.Reflection.ProcessorArchitecture.MSIL>, сборка политики издателя для этой сборки должна быть создана с помощью `/platform:anycpu`. Для каждой сборки конкретного процессора необходимо предоставить отдельную сборку политики издателя.

Следствием этого правила является то, что для изменения архитектуры процессора для сборки необходимо изменить основной или дополнительный компонент номера версии, чтобы можно было указать новую сборку политики издателя с правильной архитектурой процессора. Старая сборка политики издателя не может обслуживать сборку, если сборка имеет другую архитектуру процессора.

Другой следствием является то, что компоновщик версии 2,0 нельзя использовать для создания сборки политики издателя для сборки, скомпилированной с помощью более ранних версий .NET Framework, так как она всегда определяет архитектуру процессора.

## <a name="adding-the-publisher-policy-assembly-to-the-global-assembly-cache"></a>Добавление сборки политики издателя в глобальный кэш сборок

Используйте [средство глобального кэша сборок (Gacutil. exe)](../tools/gacutil-exe-gac-tool.md) , чтобы добавить сборку политики издателя в глобальный кэш сборок.

### <a name="to-add-the-publisher-policy-assembly-to-the-global-assembly-cache"></a>Добавление сборки политики издателя в глобальный кэш сборок

В командной строке введите следующую команду:

```console
gacutil /i publisherPolicyAssemblyFile
```

Следующая команда добавляет `policy.1.0.myAssembly.dll` в глобальный кэш сборок.

```console
gacutil /i policy.1.0.myAssembly.dll
```

> [!IMPORTANT]
> Сборка политики издателя не может быть добавлена в глобальный кэш сборок, если файл политики исходного издателя не находится в том же каталоге, что и сборка.

## <a name="see-also"></a>См. также

- [Программирование с использованием сборок](../../standard/assembly/program.md)
- [Обнаружение сборок в среде выполнения](../deployment/how-the-runtime-locates-assemblies.md)
- [Настройка приложений с помощью файлов конфигурации](index.md)
- [Схема параметров среды выполнения](./file-schema/runtime/index.md)
- [Схема файла конфигурации](./file-schema/index.md)
- [Перенаправление версий сборки](redirect-assembly-versions.md)
