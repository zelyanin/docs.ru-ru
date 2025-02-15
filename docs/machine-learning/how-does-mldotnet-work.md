---
title: Что такое ML.NET и принципы работы этой системы
description: ML.NET позволяет добавлять в приложения .NET возможности машинного обучения в автономном и подключенном режимах. Используя эту функцию, вы можете получать автоматические прогнозы на основе данных, доступных вашему приложению, без необходимости подключаться к сети для работы с ML.NET. В этой статье рассматриваются основы машинного обучения в ML.NET.
ms.date: 09/27/2019
ms.topic: overview
ms.custom: mvc
ms.author: nakersha
author: natke
ms.openlocfilehash: 1ae6b82ada841ad172cbe6a59b667aaaf619e714
ms.sourcegitcommit: 35da8fb45b4cca4e59cc99a5c56262c356977159
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/28/2019
ms.locfileid: "71592053"
---
# <a name="what-is-mlnet-and-how-does-it-work"></a>Что такое ML.NET и принципы работы этой системы

ML.NET позволяет добавлять в приложения .NET возможности машинного обучения в автономном и подключенном режимах. Используя эту функцию, вы можете получать автоматические прогнозы на основе данных, доступных вашему приложению, без необходимости подключаться к сети. В этой статье рассматриваются основы машинного обучения в ML.NET.

ML.NET работает в Windows, Linux и macOS с .NET Core или в Windows с .NET Framework. 64-разрядная версия поддерживается на всех платформах. 32-разрядная версия поддерживается в Windows, за исключением функций, связанных с TensorFlow, LightGBM и ONNX.

С помощью ML.NET можно получать прогнозы следующих типов:

|||
|-|-|
|Классификация/категоризация|Автоматическое разделение отзывов клиентов на положительные и отрицательные|
|Регрессия/прогноз непрерывных значений|Прогноз цены на дома, исходя из их размера и местонахождения|
|Обнаружение аномалий|Обнаружение мошеннических банковских операций |
|Рекомендации|Предложение продуктов, которые онлайн-покупатели могут захотеть купить, исходя из их предыдущих покупок|

## <a name="hello-mlnet-world"></a>Hello ML.NET World

В следующем фрагменте кода показано простейшее приложение ML.NET. Код в этом примере создает модель линейной регрессии для прогнозирования цен на дома, исходя из их размера и стоимости. В реальных приложения данные и модель будут намного более сложными.

 ```csharp
    using System;
    using Microsoft.ML;
    using Microsoft.ML.Data;
    
    class Program
    {
        public class HouseData
        {
            public float Size { get; set; }
            public float Price { get; set; }
        }
    
        public class Prediction
        {
            [ColumnName("Score")]
            public float Price { get; set; }
        }
    
        static void Main(string[] args)
        {
            MLContext mlContext = new MLContext();
    
            // 1. Import or create training data
            HouseData[] houseData = {
                new HouseData() { Size = 1.1F, Price = 1.2F },
                new HouseData() { Size = 1.9F, Price = 2.3F },
                new HouseData() { Size = 2.8F, Price = 3.0F },
                new HouseData() { Size = 3.4F, Price = 3.7F } };
            IDataView trainingData = mlContext.Data.LoadFromEnumerable(houseData);

            // 2. Specify data preparation and model training pipeline
            var pipeline = mlContext.Transforms.Concatenate("Features", new[] { "Size" })
                .Append(mlContext.Regression.Trainers.Sdca(labelColumnName: "Price", maximumNumberOfIterations: 100));
    
            // 3. Train model
            var model = pipeline.Fit(trainingData);
    
            // 4. Make a prediction
            var size = new HouseData() { Size = 2.5F };
            var price = mlContext.Model.CreatePredictionEngine<HouseData, Prediction>(model).Predict(size);

            Console.WriteLine($"Predicted price for size: {size.Size*1000} sq ft= {price.Price*100:C}k");

            // Predicted price for size: 2500 sq ft= $261.98k
        }
    } 
```

## <a name="code-workflow"></a>Порядок работы с кодом

На представленной ниже схеме показана структура кода приложения, а также итеративный процесс разработки модели.

- Сбор и загрузка обучающих данных в объект **IDataView**
- Указание конвейера операций для извлечения функций и применение алгоритма машинного обучения
- Обучение модели путем вызова функции **Fit()** для конвейера
- Оценка модели и итерации для ее улучшения
- Сохранение модели в двоичном формате для использования в приложении
- Загрузка модели обратно в объект **ITransformer**
- Прогнозирование с помощью функции **CreatePredictionEngine.Predict()**

![Процесс разработки приложения ML.NET, включая компоненты для создания данных, разработку конвейера, а также обучение, оценку и использование модели](./media/mldotnet-annotated-workflow.png) 

Рассмотрим эти этапы более подробно.

## <a name="machine-learning-model"></a>Модель машинного обучения

Модель ML.NET — это объект, содержащий преобразования для получения прогнозов на основе предоставленных вами данных.

### <a name="basic"></a>Basic

Самая базовая модель — это двухмерная линейная регрессия, где одно непрерывное количество пропорционально другому, как в приведенном выше примере с ценами на дома. 

![Модель линейной регрессии с параметрами смещения и веса](./media/linear-regression-model.svg)

Модель проста: $Цена = b + Размер * w$. Параметры $b$ и $w$ рассчитываются с помощью линии, проведенной через набор пар данных (размер, цена). Данные, используемые для поиска параметров модели, называется **обучающими**. Входные данные модели машинного обучения называются **компонентами**. В этом примере $Размер$ — единственный компонент. Эталонные значения, которые используются для обучения модели машинного обучения, называются **метками**. В этом примере метками служат значения параметра $Цена$ в обучающем наборе данных.

### <a name="more-complex"></a>Более сложная модель

Более сложная модель классифицирует финансовые транзакции по категориям, используя текстовые описания транзакций.

Описание транзакции разбивается на набор компонентов — для этого удаляются избыточные слова и символы и подсчитывается количество слов и комбинаций символов. Набор компонентов используется для обучения линейной модели на основе набора категорий в обучающих данных. Чем больше новые описания похожи на те, что уже присутствуют в наборе обучающих данных, тем выше вероятность того, что соответствующим транзакциям будет присвоена та же категория. 

![Модель классификации текста](./media/text-classification-model.svg)

И модель прогнозирования цен на дома, и модель классификации текста являются **линейными**. В зависимости от характера ваших данных и решаемой задачи можно также использовать модели **дерева принятия решений**, **обобщенные аддитивные** модели и т. д. Дополнительные сведения о моделях см. в статье [Задачи](./resources/tasks.md).

## <a name="data-preparation"></a>Подготовка данных

В большинстве случаев доступные данные не подходят для обучения модели машинного обучения в их исходном виде. Необработанные данные необходимо подготовить (подвергнуть предварительной обработке), прежде чем их можно будет использовать для поиска параметров вашей модели. Возможно, данные придется преобразовать из строковых значений в числовые, освободить от избыточной информации, уменьшить или увеличить их размер, масштабировать или нормализовать.

В [учебниках ML.NET](./tutorials/index.md) рассказывается о различных конвейерах обработки данных для текстов, изображений, числовых данных и временных рядов, которые используются для выполнения задач машинного обучения.

В статье [Подготовка данных](./how-to-guides/prepare-data-ml-net.md) вы найдете более общую информацию о подготовке данных.

Информацию обо всех [доступных преобразованиях](./resources/transforms.md) вы найдете в разделе ресурсов.

## <a name="model-evaluation"></a>Оценка модели

Как узнать, насколько точные прогнозы будет давать обученная вами модель? ML.NET позволяет оценить модель по новым тестовым данным. 

У каждого типа задач машинного обучения имеются метрики, по которым можно оценить точность и аккуратность модели, применив ее к тестовому набору данных.

В примере с ценами на дома мы использовали задачу **Регрессия**. Чтобы оценить модель, добавьте к исходному образцу следующий код:

```csharp
        HouseData[] testHouseData =
        {
            new HouseData() { Size = 1.1F, Price = 0.98F },
            new HouseData() { Size = 1.9F, Price = 2.1F },
            new HouseData() { Size = 2.8F, Price = 2.9F },
            new HouseData() { Size = 3.4F, Price = 3.6F }
        };

        var testHouseDataView = mlContext.Data.LoadFromEnumerable(testHouseData);
        var testPriceDataView = model.Transform(testHouseDataView);
                
        var metrics = mlContext.Regression.Evaluate(testPriceDataView, labelColumnName: "Price");

        Console.WriteLine($"R^2: {metrics.RSquared:0.##}");
        Console.WriteLine($"RMS error: {metrics.RootMeanSquaredError:0.##}");

        // R^2: 0.96
        // RMS error: 0.19
```

Метрики оценки покажут, что ошибки незначительны, а корреляция между прогнозируемыми результатами и выходными данными теста высока. Это было просто. На практике для получения хороших метрик модели требуется гораздо более тщательная настройка.

## <a name="mlnet-architecture"></a>Архитектура ML.NET

В этом разделе мы рассмотрим архитектурные шаблоны ML.NET. Если вы опытный разработчик .NET, какие-то из этих шаблонов будут вам знакомы больше, а какие-то меньше. Держитесь крепче, начинаем погружение!

Приложение ML.NET начинается с объекта <xref:Microsoft.ML.MLContext>. Этот одноэлементный объект содержит **каталоги**. Каталог — это фабрика загрузки и сохранения данных, преобразований, инструкторов и компонентов операций модели. Каждый объект каталога имеет методы для создания различных типов компонентов:

|||||
|-|-|-|-|
|Загрузка и сохранение данных||<xref:Microsoft.ML.DataOperationsCatalog>||
|Подготовка данных||<xref:Microsoft.ML.TransformsCatalog>||
|Алгоритмы обучения|Двоичная классификация|<xref:Microsoft.ML.BinaryClassificationCatalog>||
||Многоклассовая классификация|<xref:Microsoft.ML.MulticlassClassificationCatalog>||
||Обнаружение аномалий|<xref:Microsoft.ML.AnomalyDetectionCatalog>||
||Кластеризация|<xref:Microsoft.ML.ClusteringCatalog>||
||Прогнозирование|<xref:Microsoft.ML.ForecastingCatalog>||
||Ранжирование|<xref:Microsoft.ML.RankingCatalog>||
||Регрессия|<xref:Microsoft.ML.RegressionCatalog>||
||Рекомендация|<xref:Microsoft.ML.RecommendationCatalog>|Добавление пакета NuGet `Microsoft.ML.Recommender`|
||TimeSeries|<xref:Microsoft.ML.TimeSeriesCatalog>|Добавление пакета NuGet `Microsoft.ML.TimeSeries`|
|Использование модели ||<xref:Microsoft.ML.ModelOperationsCatalog>||

Через каждую из указанных выше категорий можно перейти к соответствующим методам создания. В Visual Studio каталоги отображаются с помощью IntelliSense.

   ![IntelliSense для инструкторов регрессии](./media/catalog-intellisense.png)

### <a name="build-the-pipeline"></a>Сборка конвейера

В каждом каталоге есть набор методов расширения. Посмотрим, как методы расширения используются при создании конвейера обучения.

```csharp
    var pipeline = mlContext.Transforms.Concatenate("Features", new[] { "Size" })
        .Append(mlContext.Regression.Trainers.Sdca(labelColumnName: "Price", maximumNumberOfIterations: 100));
```

В представленном ниже фрагменте кода `Concatenate` и `Sdca` — это методы из каталога. Каждый из них создает объект [IEstimator](xref:Microsoft.ML.IEstimator%601), который добавляется в конвейер.

Этот этап включает только создание объектов. Выполнение не происходит.

### <a name="train-the-model"></a>Обучение модели

После создания объектов в конвейере данные можно использовать для обучения модели.

```csharp
    var model = pipeline.Fit(trainingData);
```

Вызов функции `Fit()` задействует входные обучающие данные для оценки параметров модели. Это называется обучение модели. Напомним, что представленная выше модель линейной регрессии включала два параметра модели: **смещение** и **вес**. После вызова функции `Fit()` значения этих параметров становятся известны. В большинстве моделей параметров будет намного больше.

Дополнительные сведения об обучении моделей см. в статье [Обучение модели](./how-to-guides/train-machine-learning-model-ml-net.md).

Полученный объект модели реализует интерфейс <xref:Microsoft.ML.ITransformer>. Это значит, что модель преобразует входные данные в прогнозы.

```csharp
   IDataView predictions = model.Transform(inputData);
```

### <a name="use-the-model"></a>Использование модели

Входные данные можно преобразовывать в прогнозы все сразу или по очереди. В примере с ценами на дома мы использовали оба варианта: все сразу для оценки модели и поочередно для создания нового прогноза. Рассмотрим получение одиночных прогнозов.

```csharp
    var size = new HouseData() { Size = 2.5F };
    var predEngine = mlContext.CreatePredictionEngine<HouseData, Prediction>(model);
    var price = predEngine.Predict(size);
```
 
Метод `CreatePredictionEngine()` принимает входной и выходной класс данных. Имена полей и/или атрибуты кода определяют имена столбцов данных, которые используются при обучении модели и прогнозировании. Прочтите, [как создать одиночный прогноз](./how-to-guides/single-predict-model-ml-net.md), в разделе инструкций.

### <a name="data-models-and-schema"></a>Модели и схема данных

В основе конвейера машинного обучения ML.NET лежат объекты [DataView](xref:Microsoft.ML.IDataView).

Каждое преобразование в конвейере имеет входную схему (имена, типы и размеры данных, которые должны присутствовать во входных данных) и схему вывода (имена, типы и размеры данных, которые создаются после преобразования). В следующем документе содержится подробное описание [интерфейса IDataView и его системы типов](https://xadupre.github.io/machinelearningext/mlnetdocs/idataviewtypesystem.html).

Если схема вывода из одного преобразования в конвейере не соответствует входной схеме, ML.NET выдает исключение.

Объект представления данных содержит столбцы и строки. Каждый столбец содержит имя, тип и длину. Например, столбцы входных данных в примере с ценами на дома — это **размер** и **цена**. Оба имеют тип и являются скорее скалярными, а не векторными величинами.

   ![Пример представления данных ML.NET с данными прогнозов по ценам на дома](./media/ml-net-dataview.png)

Все алгоритмы ML.NET ищут векторный столбец входных данных. По умолчанию этот столбец вектора называется **Компоненты**. Именно поэтому в примере с ценами на дома мы включили столбец **Размер** в новый столбец **Компоненты**.

 ```csharp
    var pipeline = mlContext.Transforms.Concatenate("Features", new[] { "Size" })
 ```

Кроме того, после выполнения прогноза все алгоритмы создают новые столбцы. Фиксированные имена этих новых столбцов зависят от типа алгоритма машинного обучения. В задаче регрессии один из новых столбцов называется **Оценка**. Вот почему мы назвали точно так же наши данные цен.

```csharp
    public class Prediction
    {
        [ColumnName("Score")]
        public float Price { get; set; }
    }
```    

Дополнительные сведения о выходных столбцах различных задач машинного обучения см в руководстве [Задачи машинного обучения](resources/tasks.md).

Важным свойством объектов DataView является то, что они вычисляются **неактивно**. Представления данных загружаются и используются только во время обучения и оценки модели и при прогнозировании данных. В процессе написания и тестирования приложения ML.NET можно использовать отладчик Visual Studio, чтобы посмотреть на любой объект представления данных, вызвав метод [предварительного просмотра](xref:Microsoft.ML.DebuggerExtensions.Preview*).

```csharp
    var debug = testPriceDataView.Preview();
```

Вы можете проверить переменную `debug` в отладчике и изучить ее содержимое. Не используйте метод предварительного просмотра в рабочем коде, поскольку он сильно снижает производительность.

### <a name="model-deployment"></a>Развертывание модели

В реальных приложениях обучение и оценка модели будут отделены от прогнозирования. На практике эти действия часто выполняют разные группы разработчиков. Ваша команда разработчиков модели может сохранить модель для использования в приложении прогнозирования.

```csharp   
   mlContext.Model.Save(model, trainingData.Schema,"model.zip");
```

## <a name="where-to-now"></a>Что дальше

Узнайте, как создавать приложения, используя другие задачи машинного обучения и более реалистичные наборы данных, из [руководств](./tutorials/index.md).

Или изучите подробнее отдельные темы в [практических руководствах](./how-to-guides/index.md).

А если вы полны энтузиазма, то [справочная документация по API](https://docs.microsoft.com/dotnet/api/?view=ml-dotnet) всегда к вашим услугам!
