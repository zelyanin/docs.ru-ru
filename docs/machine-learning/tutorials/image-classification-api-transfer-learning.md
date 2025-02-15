---
title: Учебник. Автоматизированная визуальная проверка с использованием передачи обучения
description: В этом учебнике показано, как применить передачу обучения для обучения модели глубокого обучения TensorFlow в ML.NET, используя API обнаружения изображений для классификации изображений бетонных поверхностей как растрескавшихся или нерастрескавшихся.
author: luisquintanilla
ms.author: luquinta
ms.date: 10/25/2019
ms.topic: tutorial
ms.custom: mvc
ms.openlocfilehash: b8aec80134188811eb80ad1394e5a64d65a3c6f0
ms.sourcegitcommit: 9b2ef64c4fc10a4a10f28a223d60d17d7d249ee8
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/26/2019
ms.locfileid: "72961956"
---
# <a name="tutorial-automated-visual-inspection-using-transfer-learning-with-the-mlnet-image-classification-api"></a>Учебник. Автоматизированная визуальная проверка с использованием передачи обучения и API классификации изображений ML.NET

Сведения о том, как обучить пользовательскую модель глубокого обучения с использованием передачи обучения, предварительно обученной модели TensorFlow и API классификации изображений ML.NET для классификации изображений бетонных поверхностей как растрескавшихся или нерастрескавшихся.

> [!NOTE]
> В этом учебнике используется предварительная версия API классификации изображений ML.NET.

В этом руководстве вы узнаете, как:
> [!div class="checklist"]
>
> - Определение проблемы
> - Сведения об API классификации изображений ML.NET
> - Описание предварительно обученной модели
> - Использование передачи обучения для обучения пользовательской модели классификации изображений TensorFlow
> - Классификация изображений с помощью пользовательской модели

## <a name="prerequisites"></a>Предварительные требования

- [Visual Studio 2017 15.6 или более поздней версии](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2017) с установленной рабочей нагрузкой "Кроссплатформенная разработка .NET Core".

## <a name="image-classification-transfer-learning-sample-overview"></a>Обзор примера с передачей обучения для классификации изображений

Этот пример представляет собой консольное приложение C# .NET Core, которое классифицирует изображения с помощью предварительно обученной модели TensorFlow для глубокого обучения. Код для этого примера можно найти в репозитории [dotnet/machinelearning-samples](https://github.com/dotnet/machinelearning-samples/tree/master/samples/csharp/getting-started/DeepLearning_ImageClassification_Binary) на сайте GitHub.

## <a name="understand-the-problem"></a>Определение проблемы

Классификация изображений — это задача из области компьютерного зрения. Классификация изображений принимает изображение в качестве входных данных и классифицирует его, относя к предписанному классу. Ниже описаны сценарии, в которых удобно использовать классификацию изображений.

- Распознавание лиц
- Распознавание эмоций
- Постановка медицинских диагнозов
- Распознавание ориентиров

В этом учебнике обучается пользовательская модель классификации изображений для проведения автоматизированной визуальной проверки эстакад моста для выявления конструкций с трещинами.

## <a name="mlnet-image-classification-api"></a>API классификации изображений ML.NET

ML.NET предоставляет различные способы для классификации изображений. В этом учебнике применяется передача обучения с помощью API классификации изображений. API классификации изображений использует [TensorFlow.NET](https://github.com/SciSharp/TensorFlow.NET), низкоуровневую библиотеку, которая предоставляет привязки C# для API TensorFlow C++.

## <a name="what-is-transfer-learning"></a>Что собой представляет передача обучения?

Передача обучения позволяет применить знания, полученные при решении одной проблемы, для другой связанной проблемы.

Обучение модели глубокого обучения с нуля предусматривает настройку нескольких параметров, а также использование больших объемов помеченных данных обучения и вычислительных ресурсов (сотни часов работы GPU). Использование предварительно обученной модели вместе с передачей обучения позволяет упростить процесс обучения. 

## <a name="training-process"></a>Процесс обучения

API классификации изображений начинает процесс обучения с загрузки предварительно обученной модели TensorFlow. Процесс обучения состоит из двух этапов:

1. Этап узкого места
2. Этап обучения

![Этапы обучения](./media/image-classification-api-transfer-learning/training.png)

### <a name="bottleneck-phase"></a>Этап узкого места

На этапе узкого места загружается набор обучающих изображений, а значения пикселей используются в качестве входных данных или признаков для зафиксированных слоев предварительно обученной модели. Зафиксированные слои включают все слои нейронной сети вплоть до предпоследнего, который неофициально называют слоем узкого места. Эти слои называются зафиксированными, так как обучение на них производиться не будет, а операции просто передаются через них. Именно в этих зафиксированных слоях вычисляются шаблоны более низкого уровня, которые помогают модели различать различные классы. Чем больше количество слоев, тем больше вычислительных ресурсов используется на этом шаге. К счастью, так как это разовое вычисление, результаты можно кэшировать и использовать в последующих запусках при экспериментах с различными параметрами.

### <a name="training-phase"></a>Этап обучения

После вычисления выходных значений на этапе узкого места они используются в качестве входных данных для переобучения последнего слоя модели. Этот процесс является итеративным и выполняется то количество раз, которое указано в параметрах модели. При каждом запуске вычисляются потери и точность. Затем вносятся соответствующие корректировки для улучшения модели с целью минимизации потерь и максимизации точности. После завершения обучения выводятся два формата модели. Один из них — это версия модели `.pb`, а другой — сериализованная ML.NET версия модели `.zip`. При работе в средах, поддерживаемых ML.NET, рекомендуется использовать версию модели `.zip`. Однако в средах, где ML.NET не поддерживается, можно использовать версию `.pb`.

## <a name="understand-the-pretrained-model"></a>Описание предварительно обученной модели

Предварительно обученная модель, используемая в этом учебнике, представляет собой 101-слойный вариант модели остаточной сети (ResNet) версии 2. Исходная модель обучается для классификации изображений по тысячам категорий. Она принимает в качестве входных данных изображение размером 224 на 224 и выводит вероятности для каждого из классов, по которым она обучена. Часть этой модели используется для обучения новой модели с помощью пользовательских изображений, чтобы делать прогнозы между двумя классами. 

## <a name="create-console-application"></a>Создание консольного приложения

Теперь, когда у вас есть общее представление о передаче обучения и API классификации изображений, пришло время создать приложение.

1. Создайте **консольное приложение C# .NET Core** с именем DeepLearning_ImageClassification_Binary.
1. Установите пакет NuGet **Microsoft.ML 1.4.0-preview2**:
    1. В обозревателе решений щелкните проект правой кнопкой мыши и выберите **Управление пакетами NuGet**.
    1. Выберите nuget.org в качестве источника пакета.
    1. Выберите вкладку **Обзор**.
    1. Установите флажок **Включить предварительные версии**.
    1. Выполните поиск **Microsoft.ML**.
    1. Нажмите кнопку **Установить**.
    1. Нажмите кнопку **ОК** в диалоговом окне **Предварительный просмотр изменений**, а затем нажмите кнопку **Принимаю** в диалоговом окне **Принятие условий лицензионного соглашения**, если вы согласны с указанными условиями лицензионного соглашения для выбранных пакетов.
    1. Повторите эти шаги для пакетов **Microsoft.ML.Dnn 0.16.0-preview2** и **Microsoft.ML.ImageAnalytics 1.4.0-preview2**.

### <a name="prepare-and-understand-the-data"></a>Подготовка и анализ данных

> [!NOTE]
> Наборы данных для этого учебника взяты из следующего источника: Maguire, Marc; Dorafshan, Sattar и Thomas, Robert J., "SDNET2018: A concrete crack image dataset for machine learning applications" (2018). Просмотрите все наборы данных. Документ 48. https://digitalcommons.usu.edu/all_datasets/48

SDNET2018 — это набор данных изображений, который содержит аннотации для растрескавшихся и нерастрескавшихся бетонных конструкций (эстакады моста, стены и покрытие). 

![Примеры для эстакад моста из набора данных SDNET2018](./media/image-classification-api-transfer-learning/sdnet2018decksamples.png)

Эти данные упорядочены по трем подкаталогам:

- D содержит изображения эстакад моста.
- P содержит изображения покрытия.
- W содержит изображения стен.

Каждый из этих подкаталогов содержит еще два вложенных каталога с префиксами:

- Префикс C используется для растрескавшихся поверхностей.
- Префикс U используется для нерастрескавшихся поверхностей.

В этом учебнике используются только изображения эстакад моста.

1. Скачайте [набор данных](https://github.com/dotnet/machinelearning-samples/raw/master/samples/csharp/getting-started/DeepLearning_ImageClassification_Binary/DeepLearning_ImageClassification_Binary/assets.zip) и распакуйте его.
1. Создайте каталог с именем "assets" в проекте, чтобы сохранять файлы набора данных.
1. Скопируйте подкаталоги *CD* и *UD* из недавно распакованного каталога в каталог *assets*.

### <a name="create-input-and-output-classes"></a>Создание классов входных и выходных данных

1. Откройте файл *Program.cs* и замените операторы `using` в его начале следующими:

    [!code-csharp [ProgramUsings](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ImageClassification_Binary/DeepLearning_ImageClassification_Binary/Program.cs#L1-L7)]

1. Под классом `Program` в *Program.cs* создайте класс `ImageData`. Этот класс служит для представления начальных загруженных данных. 

    [!code-csharp [ImageDataClass](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ImageClassification_Binary/DeepLearning_ImageClassification_Binary/Program.cs#L135-L140)]

    `ImageData` содержит следующие свойства:

    - `ImagePath` — полный путь, по которому хранится изображение.
    - `Label` — категория, к которой принадлежит это изображение. Это прогнозируемое значение.

1. Создайте классы для входных и выходных данных.

    1. Под классом `ImageData` определите схему входных данных в новом классе `ModelInput`.

        [!code-csharp [ModelInputClass](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ImageClassification_Binary/DeepLearning_ImageClassification_Binary/Program.cs#L142-L151)]

        `ModelInput` содержит следующие свойства:

        - `ImagePath` — полный путь, по которому хранится изображение. 
        - `Label` — категория, к которой принадлежит это изображение. Это прогнозируемое значение.
        - `Image` является представлением изображения `byte[]`. Модель ожидает, что для обучения используются данные изображений этого типа.
        - `LabelAsKey` является численным представлением `Label`. 

        Только `Image` и `LabelAsKey` используются для обучения модели и составления прогнозов. Свойства `ImagePath` и `Label` хранятся на случай обращения к имени и категории исходного файла изображения.

    1. Затем под классом `ModelInput` определите схему выходных данных в новом классе `ModelOutput`. 

        [!code-csharp [ModelOutputClass](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ImageClassification_Binary/DeepLearning_ImageClassification_Binary/Program.cs#L153-L160)]

        `ModelOutput` содержит следующие свойства:

        - `ImagePath` — полный путь, по которому хранится изображение.
        - `Label` — исходная категория, к которой принадлежит это изображение. Это прогнозируемое значение. 
        - `PredictedLabel` — значение, спрогнозированное моделью.

        По аналогии с `ModelInput`, для создания прогнозов требуется только `PredictedLabel`, так как оно содержит прогноз, выполненный моделью. Свойства `ImagePath` и `Label` хранятся на случай обращения к имени и категории исходного файла изображения.

### <a name="define-paths-and-initialize-variables"></a>Определение путей и инициализация переменных

1. Внутри метода `Main` определите расположение ресурсов.

    [!code-csharp [DefinePaths](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ImageClassification_Binary/DeepLearning_ImageClassification_Binary/Program.cs#L15-L16)]

1. Затем инициализируйте переменную `mlContext` с помощью нового экземпляра [MLContext](xref:Microsoft.ML.MLContext).

    [!code-csharp [MLContext](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ImageClassification_Binary/DeepLearning_ImageClassification_Binary/Program.cs#L18)]

Класс [MLContext](xref:Microsoft.ML.MLContext) является отправной точкой для любых операций ML.NET. В результате инициализации mlContext создается среда ML.NET, которая может использоваться всеми объектами в рамках процесса создания модели. По существу он аналогичен классу `DBContext` в Entity Framework.

## <a name="load-the-data"></a>Загрузка данных

### <a name="create-data-loading-utility-method"></a>Создание служебного метода для загрузки данных

Изображения хранятся в двух подкаталогах. Перед загрузкой данных их необходимо отформатировать в виде списка объектов `ImageData`. Для этого создайте метод `LoadImagesFromDirectory` под методом `Main`.

```csharp
public static IEnumerable<ImageData> LoadImagesFromDirectory(string folder, bool useFolderNameAsLabel = true)
{

}
```

1. Внутри `LoadImagesDirectory` добавьте следующий код, чтобы получить все пути к файлам из подкаталогов:

    [!code-csharp [GetFiles](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ImageClassification_Binary/DeepLearning_ImageClassification_Binary/Program.cs#L102-L103)]

1. Затем выполните итерацию по каждому из файлов, используя оператор `foreach`.

    ```csharp
    foreach (var file in files)
    {

    }
    ```

1. Внутри оператора `foreach` убедитесь, что поддерживаются расширения файлов. API классификации изображений поддерживает форматы JPEG и PNG.

    [!code-csharp [CheckExtension](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ImageClassification_Binary/DeepLearning_ImageClassification_Binary/Program.cs#L107-L108)]

1. Затем получите метку для файла. Если параметр `useFolderNameAsLabel` имеет значение `true`, то в качестве метки используется родительский каталог, где сохранен файл. В противном случае ожидается, что метка будет именем файла или его префиксом.

    [!code-csharp [GetLabel](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ImageClassification_Binary/DeepLearning_ImageClassification_Binary/Program.cs#L110-L124)]

1. Наконец, создайте экземпляр класса `ModelInput`.

    [!code-csharp [CreateImageData](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ImageClassification_Binary/DeepLearning_ImageClassification_Binary/Program.cs#L126-L130)]

### <a name="prepare-the-data"></a>Подготовка данных

1. Вернувшись в метод `Main`, используйте служебный метод `LoadFromDirectory`, чтобы получить список изображений, используемых для обучения.

    [!code-csharp [LoadImages](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ImageClassification_Binary/DeepLearning_ImageClassification_Binary/Program.cs#L20)]

1. Затем загрузите эти изображения в [`IDataView`](xref:Microsoft.ML.IDataView) с помощью метода [`LoadFromEnumerable`](xref:Microsoft.ML.DataOperationsCatalog.LoadFromEnumerable*).

    [!code-csharp [CreateIDataView](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ImageClassification_Binary/DeepLearning_ImageClassification_Binary/Program.cs#L22)]    

1. Данные загружаются в том порядке, в котором они были считаны из каталогов. Чтобы сбалансировать данные, перемешайте их в случайном порядке с помощью метода [`ShuffleRows`](xref:Microsoft.ML.DataOperationsCatalog.ShuffleRows*).

    [!code-csharp [ShuffleRows](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ImageClassification_Binary/DeepLearning_ImageClassification_Binary/Program.cs#L24)]

1. Модели машинного обучения ожидают входные данные в числовом формате. Поэтому перед обучением необходимо выполнить некоторую предварительную обработку данных. Создайте [`EstimatorChain`](xref:Microsoft.ML.Data.EstimatorChain%601), состоящий из преобразований [`MapValueToKey`](xref:Microsoft.ML.ConversionsExtensionsCatalog.MapValueToKey*) и [`LoadImages`](xref:Microsoft.ML.ImageEstimatorsCatalog.LoadImages*). Преобразование `MapValueToKey` принимает значение категории в столбце `Label`, преобразует его в числовое значение `KeyType` и сохраняет в новом столбце `LabelAsKey`. `LoadImages` принимает значения из столбца `ImagePath` вместе с параметром `imageFolder` для загрузки изображений для обучения. При установке значения `false` для `useImageType` изображения преобразуются в `byte[]`. 

    [!code-csharp [DefinePreprocessingPipeline](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ImageClassification_Binary/DeepLearning_ImageClassification_Binary/Program.cs#L26-L33)]

1. Используйте метод [`Fit`](xref:Microsoft.ML.Data.EstimatorChain%601.Fit*), чтобы применить данные к `preprocessingPipeline` [`EstimatorChain`](xref:Microsoft.ML.Data.EstimatorChain%601), а затем метод [`Transform`](xref:Microsoft.ML.Data.TransformerChain`1.Transform*), который возвращает [`IDataView`](xref:Microsoft.ML.IDataView), содержащий предварительно обработанные данные.

    [!code-csharp [PreprocessData](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ImageClassification_Binary/DeepLearning_ImageClassification_Binary/Program.cs#L35-L37)]

1. Для обучения модели важно иметь набор данных для обучения и проверочный набор данных. Модель обучается на наборе для обучения. То, насколько точно она делает прогнозы по неизвестным данным, оценивается с точки зрения эффективности по сравнению с проверочным набором. В зависимости от достигнутой эффективности модель вносит корректировки в то, что она изучила, для улучшения результата. Проверочный набор можно получить, разделив исходный набор данных, либо из другого источника, который уже был подобран специально для этого. В данном случае предварительно обработанный набор данных разделяется на наборы для обучения, проверки и тестирования.

    [!code-csharp [CreateDataSplits](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ImageClassification_Binary/DeepLearning_ImageClassification_Binary/Program.cs#L39-L40)]

    В приведенном выше примере кода выполняется два разделения. Сначала предварительно обработанные данные разделяются в следующей пропорции: 70 % для обучения и оставшиеся 30 % для проверки. Затем 30-процентный проверочный набор снова разделяется на наборы для проверки и тестирования в следующей пропорции: 90 % для проверки и 10 % для тестирования. 

    Аналогом такого разделения данных может быть сдача экзамена. При подготовке к экзамену вы изучаете заметки, книги или другие ресурсы, чтобы получить представление об основных понятиях, о которых спрашивают на экзамене. Именно для этого и предназначен набор обучения. Затем вы можете пройти пробный экзамен, чтобы проверить свои знания. Именно здесь может пригодиться набор проверки. Вы хотите убедиться, что правильно уловили смысл основных понятий, прежде сдавать настоящий экзамен. Основываясь на этих результатах, вы определяете, что именно вы недопоняли или поняли неправильно, и вносите соответствующие изменения в процесс подготовки к настоящему экзамену. Наконец, вы сдаете сам экзамен. Именно для этого и предназначен набор для тестирования. Вы никогда не сталкивались с вопросами, которые спрашивают на экзамене, и теперь применяете знания, полученные в ходе обучения и проверки, для решения рассматриваемой задачи. 

1. Назначьте сегментам соответствующие значения для данных обучения, проверки и тестирования.

    [!code-csharp [CreateDatasets](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ImageClassification_Binary/DeepLearning_ImageClassification_Binary/Program.cs#L42-L44)]

## <a name="define-the-training-pipeline"></a>Определение конвейера обучения

Обучение модели состоит из нескольких шагов. Сначала для обучения модели используется API классификации изображений. Затем закодированные метки в столбце `PredictedLabel` преобразуются обратно в исходное значение категории с помощью преобразования `MapKeyToValue`. 

1. Определите конвейер обучения [`EstimatorChain`](xref:Microsoft.ML.Data.EstimatorChain%601), состоящий из обоих преобразований `mapLabelEstimator` и `ImageClassification`.

    [!code-csharp [DefineTrainingPipeline](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ImageClassification_Binary/DeepLearning_ImageClassification_Binary/Program.cs#L46-L58)]    

    Средство оценки `ImageClassification` принимает несколько параметров:

    - `featuresColumnName` — столбец, используемый в качестве входных данных для модели.
    - `labelColumnName` — столбец для прогнозируемого значения.
    - `arch` определяет, какую из архитектур предварительно обученной модели нужно использовать. В этом учебнике используется 101-слойный вариант модели ResNetv2.
    - `epoch` указывает максимальное число итераций по всему набору данных в рамках процесса обучения. Чем выше это число, тем дольше модель обучается и, возможно, лучше становится.
    - `batchSize` — число выборок для обучения, используемых за один раз. В течение одной эпохи для обучения и обновления модели используется несколько пакетов, равных batchSize. Чем меньше это число, тем меньше памяти требуется при обработке каждого пакета.
    - `testOnTrainSet` указывает модели измерять эффективность относительно обучающего набора, если проверочный набор отсутствует.
    - `metricsCallback` привязывает функцию для отслеживания хода выполнения во время обучения.
    - `validationSet` является [`IDataView`](xref:Microsoft.ML.IDataView), содержащим проверочные данные.
    - `reuseTrainSetBottleneckCachedValues` сообщает модели, следует ли использовать кэшированные значения из этапа узкого места при последующих запусках. Этап узкого места представляет собой однократное сквозное вычисление, которое потребляет много ресурсов при первом выполнении. Если данные для обучения не изменяются и вы хотите поэкспериментировать с разным числом эпох или разными размерами пакета, использование кэшированных значений значительно сокращает время, необходимое для обучения модели.
    - `reuseValidationSetBottleneckCachedValues` аналогичен `reuseTrainSetBottleneckCachedValues`, только в данном случае он предназначен для проверочного набора данных.
    - `disableEarlyStopping` сообщает средству оценки ImageClassification, следует ли применять стратегию ранней остановки. По мере того как модель ищет оптимальные значения, которые помогут добиться точных прогнозов во время обучения, эффективность может увеличиваться или уменьшаться. В конечном итоге, если модель достигает последней эпохи, результаты ее обучения могут оказаться неоптимальными. Функция ранней остановки отслеживает процесс обучения на предмет таких снижений и останавливает его для сохранения оптимальной версии модели.

1. Используйте метод [`Fit`](xref:Microsoft.ML.Data.EstimatorChain%601.Fit*) для обучения модели.

    [!code-csharp [TrainModel](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ImageClassification_Binary/DeepLearning_ImageClassification_Binary/Program.cs#L60)]

## <a name="use-the-model"></a>Использование модели

Теперь, когда вы обучили модель, пора использовать ее для классификации изображений.

Под методом `Main` создайте служебный метод `OutputPrediction`, чтобы отобразить сведения о прогнозе в консоли.

[!code-csharp [OuputPredictionMethod](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ImageClassification_Binary/DeepLearning_ImageClassification_Binary/Program.cs#L94-L98)]

### <a name="classify-a-single-image"></a>Классификация одного изображения

1. Добавьте новый метод `ClassifySingleImage` под методом `Main`, чтобы создать и вывести прогноз для одного изображения.

    ```csharp
    public static void ClassifySingleImage(MLContext mlContext, IDataView data, ITransformer trainedModel)
    {

    }
    ```

1. Создайте [`PredictionEngine`](xref:Microsoft.ML.PredictionEngine%602) в методе `ClassifySingleImage`. [`PredictionEngine`](xref:Microsoft.ML.PredictionEngine%602) представляет собой удобный API, позволяющий передать один экземпляр данных и осуществить прогнозирование на его основе.

    [!code-csharp [CreatePredictionEngine](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ImageClassification_Binary/DeepLearning_ImageClassification_Binary/Program.cs#L71)]    

1. Чтобы обратиться к одному экземпляру `ModelInput`, преобразуйте `data` [`IDataView`](xref:Microsoft.ML.IDataView) в [`IEnumerable`](xref:System.Collections.Generic.IEnumerable%601) с помощью метода [`CreateEnumerable`](xref:Microsoft.ML.DataOperationsCatalog.CreateEnumerable*), а затем получите первое наблюдение. 

    [!code-csharp [GetTestInputData](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ImageClassification_Binary/DeepLearning_ImageClassification_Binary/Program.cs#L73)]

1. Используйте метод [`Predict`](xref:Microsoft.ML.PredictionEngine%602.Predict*) для классификации изображения.

    [!code-csharp [MakeSinglePrediction](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ImageClassification_Binary/DeepLearning_ImageClassification_Binary/Program.cs#L75)]

1. Выведите прогноз на консоль с помощью метода `OutputPrediction`.

    [!code-csharp [OuputSinglePrediction](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ImageClassification_Binary/DeepLearning_ImageClassification_Binary/Program.cs#L77-L78)]

1. В методе `Main` вызовите `ClassifySingleImage`, используя тестовый набор изображений.

    [!code-csharp [ClassifySingleImage](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ImageClassification_Binary/DeepLearning_ImageClassification_Binary/Program.cs#L62)]

### <a name="classify-multiple-images"></a>Классификация нескольких изображений

1. Добавьте новый метод `ClassifyImages` под методом `ClassifySingleImage`, чтобы создать и вывести прогноз для нескольких изображений.

    ```csharp
    public static void ClassifyImages(MLContext mlContext, IDataView data, ITransformer trainedModel)
    {

    }
    ```

1. Создайте [`IDataView`](xref:Microsoft.ML.IDataView), содержащий прогнозы, с помощью метода [`Transform`](xref:Microsoft.ML.ITransformer.Transform*). Добавьте следующий код в метод `ClassifyImages`.

    [!code-csharp [MakeMultiplePredictions](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ImageClassification_Binary/DeepLearning_ImageClassification_Binary/Program.cs#L83)]

1. Чтобы выполнить итерацию по прогнозам, преобразуйте `predictionData` [`IDataView`](xref:Microsoft.ML.IDataView) в [`IEnumerable`](xref:System.Collections.Generic.IEnumerable%601) с помощью метода [`CreateEnumerable`](xref:Microsoft.ML.DataOperationsCatalog.CreateEnumerable*), а затем получите первые 10 наблюдений.

    [!code-csharp [IEnumerablePredictions](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ImageClassification_Binary/DeepLearning_ImageClassification_Binary/Program.cs#L85)]

1. Выполните итерацию и выведите исходные и прогнозируемые метки для прогнозов.

    [!code-csharp [OutputMultiplePredictions](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ImageClassification_Binary/DeepLearning_ImageClassification_Binary/Program.cs#L87-L91)]

1. Наконец, в методе `Main` вызовите `ClassifyImages`, используя тестовый набор изображений.

    [!code-csharp [ClassifyImages](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ImageClassification_Binary/DeepLearning_ImageClassification_Binary/Program.cs#L64)]

## <a name="run-the-application"></a>Запуск приложения

Запустите консольное приложение. Должен быть получен результат, аналогичный приведенному ниже. Кроме того, могут выводиться предупреждения или сообщения об обработке, но для удобства здесь мы убрали их. Для краткости выходные данные были сжаты.

**Этап узкого места**

Значение для имени изображения не выводится, так как изображения загружаются в виде `byte[]`, поэтому выводимое имя изображения отсутствует.

```test
Phase: Bottleneck Computation, Dataset used:      Train, Image Index: 279, Image Name:
Phase: Bottleneck Computation, Dataset used:      Train, Image Index: 280, Image Name:
Phase: Bottleneck Computation, Dataset used: Validation, Image Index:   1, Image Name:
Phase: Bottleneck Computation, Dataset used: Validation, Image Index:   2, Image Name:
```

**Этап обучения**

```text
Phase: Training, Dataset used: Validation, Batch Processed Count:   6, Epoch:  21, Accuracy:  0.6797619
Phase: Training, Dataset used: Validation, Batch Processed Count:   6, Epoch:  22, Accuracy:  0.7642857
Phase: Training, Dataset used: Validation, Batch Processed Count:   6, Epoch:  23, Accuracy:  0.7916667
```

**Выходные данные классификации изображений**

```text
Classifying single image
Image: 7001-220.jpg | Actual Value: UD | Predicted Value: UD

Classifying multiple images
Image: 7001-220.jpg | Actual Value: UD | Predicted Value: UD
Image: 7001-163.jpg | Actual Value: UD | Predicted Value: UD
Image: 7001-210.jpg | Actual Value: UD | Predicted Value: UD
```

После проверки изображения *7001-220.jpg* можно увидеть, что на самом деле на нем отсутствуют трещины. 

![Изображение набора данных SDNET2018, используемое для прогнозирования](./media/image-classification-api-transfer-learning/predictedimage.jpg)

Поздравляем! Вы успешно создали модель глубокого обучения для классификации изображений.

### <a name="improve-the-model"></a>Улучшение модели

Если вы не удовлетворены результатами работы модели, можно попытаться улучшить ее эффективность с помощью следующих подходов:

- **Увеличение объема данных**: чем больше примеров, на которых обучается модель, тем лучше она работает. Скачайте полный [набор данных SDNET2018](https://digitalcommons.usu.edu/cgi/viewcontent.cgi?filename=2&article=1047&context=all_datasets&type=additional) и используйте его для обучения. 
- **Дополнение данных**: часто, чтобы сделать данные более разнообразными, их дополняют, преобразуя изображение различным образом (поворот, отражение, сдвиг, обрезка). Это позволяет модели обучаться на более разнообразных примерах. 
- **Более длительное обучение**: чем дольше длится обучение, тем лучше будет настроена модель. Увеличение числа эпох может повысить эффективность модели.
- **Эксперименты с гиперпараметрами**: в дополнение к параметрам, используемым в этом учебнике, можно настроить и другие параметры, способные повысить эффективность. Изменение скорости обучения, которая определяет величину изменений, вносимых в модель после каждой эпохи, может повысить эффективность.
- **Использование другой архитектуры модели**: в зависимости от характера ваших данных модель, способная лучше изучить их признаки, может отличаться. Если вы не удовлетворены эффективностью модели, попробуйте изменить архитектуру.

### <a name="additional-resources"></a>Дополнительные ресурсы

- [Сравнение глубокого и машинного обучения](/azure/machine-learning/service/concept-deep-learning-vs-machine-learning).

## <a name="next-steps"></a>Следующие шаги

В этом учебнике вы узнали, как создать пользовательскую модель глубокого обучения с использованием передачи обучения, предварительно обученной модели классификации изображений TensorFlow и API классификации изображений ML.NET для классификации изображений бетонных поверхностей как растрескавшихся или нерастрескавшихся.

Переходите к следующему руководству.

> [!div class="nextstepaction"]
> [Обнаружение объектов](object-detection-onnx.md)
