---
title: Разработка и развертывание служб WCF Data Services
ms.date: 03/30/2017
helpviewer_keywords:
- WCF Data Services, developing
- WCF Data Services, deploying
- deploying [WCF Data Services
- developing applications [WCF Data Services]
ms.assetid: 6557c0e3-5aea-4f6e-bc14-77ad317a168b
ms.openlocfilehash: d7ddae58874c69468eb6ff1762db9083897b1acd
ms.sourcegitcommit: 205b9a204742e9c77256d43ac9d94c3f82909808
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/10/2019
ms.locfileid: "70854066"
---
# <a name="develop-and-deploy-wcf-data-services"></a>Разработка и развертывание WCF Data Services

В этом разделе содержатся сведения о разработке и развертывании WCF Data Services. Дополнительные основные сведения о WCF Data Services см. в разделе [Начало работы](getting-started-with-wcf-data-services.md) и [Обзор](wcf-data-services-overview.md).

## <a name="develop-wcf-data-services"></a>Разработка WCF Data Services

При использовании WCF Data Services для создания службы данных, которая поддерживает [!INCLUDE[ssODataFull](../../../../includes/ssodatafull-md.md)], необходимо выполнить следующие основные задачи во время разработки:

1. **Определение модели данных**

     WCF Data Services поддерживает различные поставщики служб данных, позволяющие определить модель данных на основе данных из различных источников данных, от реляционных баз данных до типов данных с поздним связыванием. Дополнительные сведения см. в разделе [поставщики служб данных](data-services-providers-wcf-data-services.md).

2. **Создание службы данных**

     Самая базовая служба данных предоставляет класс, производный от класса <xref:System.Data.Services.DataService%601> , с типом `T` , представляющим имя контейнера сущностей, квалифицированное пространством имен. Дополнительные сведения см. в разделе [Defining WCF Data Services](defining-wcf-data-services.md).

3. **Настройка службы данных**

     По умолчанию WCF Data Services отключает доступ к ресурсам, предоставляемым контейнером сущностей. <xref:System.Data.Services.DataServiceConfiguration> Интерфейс позволяет настроить доступ к ресурсам и операциям службы, указать поддерживаемую версию OData и определить другие поведения на уровне службы, например поведение пакетной обработки или максимальное число возвращаемых сущностей. в одном канале ответа. Дополнительные сведения см. [в разделе Настройка службы данных](configuring-the-data-service-wcf-data-services.md).

В этой статье рассматривается основная часть разработки и развертывания служб данных с помощью Visual Studio. Сведения о гибкости, обеспечиваемой WCF Data Services для предоставления данных в виде веб-каналов OData, см. в разделе [определение WCF Data Services](defining-wcf-data-services.md).

### <a name="choose-a-development-web-server"></a>Выбор веб-сервера разработки

При разработке службы данных WCF в качестве ASP.NET приложения или веб-сайта ASP.NET с помощью Visual Studio 2015 можно выбрать веб-серверы, на которых будет выполняться служба данных во время разработки. Следующие веб-серверы интегрируются с Visual Studio, чтобы облегчить тестирование и отладку служб данных на локальном компьютере.

1. **Локальный сервер служб IIS**

     При создании службы данных, которая является ASP.NET приложением или веб-сайтом ASP.NET, запущенным на службы IIS (IIS), рекомендуется разрабатывать и тестировать службу данных с помощью служб IIS на локальном компьютере. Запуск службы данных в IIS упрощает трассировку HTTP-запросов во время отладки. Это также позволяет заранее определить необходимые права, которые требуются службам IIS для доступа к файлам, базам данных и другим ресурсам для службы данных. Чтобы запустить службу данных в службах IIS, необходимо убедиться, что службы IIS и Windows Communication Foundation (WCF) установлены и настроены правильно и предоставляют доступ к учетным записям IIS в файловой системе и базах данных. Дополнительные сведения см. в разделе [Практическое руководство. Разработка службы данных WCF, работающей на](how-to-develop-a-wcf-data-service-running-on-iis.md)IIS.

    > [!NOTE]
    > Чтобы разрешить среде разработки настраивать локальный сервер IIS, необходимо запустить Visual Studio с правами администратора.

2. **Сервер разработки Visual Studio**

     Visual Studio включает встроенный веб-сервер, Visual Studio Development Server, который является веб-сервером по умолчанию для проектов ASP.NET. Этот веб-сервер предназначен для запуска проектов ASP.NET на локальном компьютере во время разработки. В [WCF Data Services кратком руководстве](quickstart-wcf-data-services.md) показано, как создать службу данных, которая выполняется в Visual Studio Development Server.

     При использовании Visual Studio Development Server для разработки службы данных следует учитывать следующие ограничения.

    - Доступ к этому серверу возможен только с локального компьютера.

    - Этот сервер прослушивает `localhost` и указанный порт, отличный от порта 80, который по умолчанию настроен для HTTP-сообщений. Дополнительные сведения см. в разделе [Веб-серверы в Visual Studio для веб-проектов ASP.NET](https://docs.microsoft.com/previous-versions/aspnet/58wxa9w5(v=vs.120)).

    - На этом сервере служба данных работает в контексте текущей учетной записи пользователя. Например, если вы работаете как пользователь уровня администратора, то служба данных, работающая в Visual Studio Development Server, будет иметь права уровня администратора. Это может обеспечить службе данных доступ к ресурсам, для которых у нее не будет прав после развертывания на сервере служб IIS.

    - На этом сервере отсутствуют дополнительные возможности служб IIS, такие как проверка подлинности.

    - Этот сервер не может обрабатывать фрагментированные потоки HTTP, которые по умолчанию отправляются клиентом WCF Data Services при доступе к большим двоичным данным из службы данных. Дополнительные сведения см. в разделе [Streaming Provider](streaming-provider-wcf-data-services.md).

    - На этом сервере возникли проблемы с обработкой символа period (`.`) в URL-адресе, хотя этот символ поддерживается WCF Data Services в значениях ключа.

    > [!TIP]
    > Несмотря на то, что можно использовать Visual Studio Development Server для тестирования служб данных во время разработки, их следует протестировать снова после развертывания на веб-сервере, на котором работают службы IIS.

3. **Среда разработки Microsoft Azure**

     Инструменты Windows Azure для Visual Studio включают в себя интегрированный набор средств для разработки служб Windows Azure в Visual Studio. С помощью этих средств можно разрабатывать службы данных, которые могут развертываться на Microsoft Azure, а также тестировать службу данных на локальном компьютере перед развертыванием. Используйте эти средства при разработке службы данных, работающей на платформе Windows Azure, с помощью Visual Studio. Вы можете скачать инструменты Windows Azure для Visual Studio из [центра загрузки Майкрософт](https://go.microsoft.com/fwlink/?LinkID=201848). Дополнительные сведения о разработке службы данных, работающей в Windows Azure, см. в разделе Публикация [службы OData в Windows Azure](https://go.microsoft.com/fwlink/?LinkId=201847).

### <a name="development-tips"></a>Советы по разработке

При разработке службы данных необходимо учитывать следующее:

- Определите требования по безопасности к службе данных, если планируется проверять подлинность пользователей или ограничивать доступ определенным пользователям. Дополнительные сведения см. в разделе [Securing WCF Data Services](securing-wcf-data-services.md).

- Программа проверки HTTP может быть очень полезной при развертывании службы данных, позволяя проверять содержимое сообщений запросов и ответов. Любой планировщик сетевых пакетов, способный отображать необработанные пакеты, можно использовать для проверки HTTP-запросов к службе данных и ответов от нее.

- При отладке службы данных может потребоваться получить дополнительные сведения об ошибке из службы данных, чем во время обычной работы. Дополнительные сведения об ошибках можно получить из службы данных, установив свойство <xref:System.Data.Services.DataServiceConfiguration.UseVerboseErrors%2A> в <xref:System.Data.Services.DataServiceConfiguration> значение `true` , а свойство <xref:System.ServiceModel.Description.ServiceDebugBehavior.IncludeExceptionDetailInFaults%2A> атрибута <xref:System.ServiceModel.Description.ServiceDebugBehavior> класса службы данных — в значение `true`. Дополнительные сведения см. в разделе Post [Debugging WCF Data Services](https://go.microsoft.com/fwlink/?LinkId=201868). Можно также включить трассировку в WCF для просмотра исключений, возникающих на уровне HTTP-сообщений. Для получения дополнительной информации см. [Configuring Tracing](../../wcf/diagnostics/tracing/configuring-tracing.md).

- Служба данных обычно разрабатывается как проект приложения ASP.NET, но вы также можете создать службу данных в качестве проекта веб-сайта ASP.NET в Visual Studio. Сведения о различиях между двумя типами проектов см. [в разделе проекты веб-приложений и проекты веб-сайтов в Visual Studio](https://docs.microsoft.com/previous-versions/aspnet/dd547590(v=vs.110)).

- При создании службы данных с помощью диалогового окна **Добавление нового элемента** в Visual Studio Служба данных размещается в ASP.NET в IIS. Хотя ASP.NET и IIS является узлом по умолчанию для службы данных, поддерживаются другие варианты размещения. Дополнительные сведения см. [в разделе Размещение службы данных](hosting-the-data-service-wcf-data-services.md).

## <a name="deploy-wcf-data-services"></a>Развертывание WCF Data Services

Службы данных WCF обеспечивают гибкость в выборе процесса, в котором будет размещаться служба данных. Visual Studio можно использовать для развертывания службы данных на следующих платформах:

- **Веб-сервер, размещенный в службах IIS**

    Когда служба данных разрабатывается как проект ASP.NET, ее можно развернуть на веб-сервере IIS с помощью стандартных процессов развертывания ASP.NET.  Visual Studio предоставляет следующие технологии развертывания для ASP.NET в зависимости от типа проекта ASP.NET, в котором размещается развертываемая служба данных.

  - **Технологии развертывания для веб-приложений ASP.NET**

    - [Практическое руководство. Создание пакета веб-развертывания в Visual Studio](https://docs.microsoft.com/previous-versions/aspnet/dd465323(v=vs.110))

    - [Практическое руководство. Развертывание веб-проекта с помощью публикации одним щелчком в Visual Studio](https://docs.microsoft.com/previous-versions/aspnet/dd465337(v=vs.110))

  - **Технологии развертывания для веб-узлов ASP.NET**

    - [Практическое руководство. Копирование файлов веб-сайта с помощью средства копирования веб-сайта](https://docs.microsoft.com/previous-versions/aspnet/c95809c0(v=vs.100))

    - [Практическое руководство. Публикация веб-сайтов](https://docs.microsoft.com/previous-versions/aspnet/20yh9f1b(v=vs.100))

    - [Пошаговое руководство: Развертывание веб-приложения ASP.NET с помощью XCOPY](https://docs.microsoft.com/previous-versions/aspnet/f735abw9(v=vs.100))

     Дополнительные сведения о вариантах развертывания для приложения ASP.NET см. в разделе [Обзор веб-развертывания для Visual Studio и ASP.NET](https://docs.microsoft.com/previous-versions/aspnet/dd394698(v=vs.110)).

    > [!TIP]
    > Прежде чем пытаться выполнить развертывание службы данных в IIS, обязательно протестируйте развертывание на веб-сервере, где работают службы IIS. Дополнительные сведения см. в разделе [Практическое руководство. Разработка службы данных WCF, работающей на](how-to-develop-a-wcf-data-service-running-on-iis.md)IIS.

- **Microsoft Azure**

     Вы можете развернуть службу данных в Windows Azure с помощью инструментов Windows Azure для Visual Studio. Вы можете скачать инструменты Windows Azure для Visual Studio из [центра загрузки Майкрософт](https://go.microsoft.com/fwlink/?LinkID=201848). Дополнительные сведения о развертывании службы данных в Windows Azure см. в разделе Публикация [службы OData в Windows Azure](https://go.microsoft.com/fwlink/?LinkId=201847).

### <a name="deployment-considerations"></a>Требования к развертыванию

При разработке службы данных необходимо учитывать следующее:

- При развертывании службы данных, которая использует поставщик Entity Framework для доступа к базе данных SQL Server, может также потребоваться распространить структуры данных, данные или и то, и другое вместе с развертыванием службы данных. Visual Studio может автоматически создавать скрипты (SQL-файлы), чтобы сделать это в целевой базе данных, и эти скрипты можно добавить в пакет веб-развертывания приложения ASP.NET. Дополнительные сведения см. в разделе [Практическое руководство. Развертывание базы данных с помощью проекта](https://docs.microsoft.com/previous-versions/dd465343(v=vs.100))веб-приложения. Для веб-сайта ASP.NET это можно сделать с помощью **мастера публикации баз данных** в Visual Studio. Дополнительные сведения см. [в разделе Публикация базы данных SQL](https://docs.microsoft.com/previous-versions/aspnet/bb907585(v=vs.100)).

- Поскольку WCF Data Services включает базовую реализацию WCF, можно использовать Windows Server AppFabric для мониторинга службы данных, развернутой в службах IIS, работающих на Windows Server. Дополнительные сведения об использовании Windows Server AppFabric для мониторинга службы данных см. в подразделе " [Отслеживание после WCF Data Services с помощью Windows Server AppFabric](https://go.microsoft.com/fwlink/?LinkID=202005)".

## <a name="see-also"></a>См. также

- [Размещение служб данных](hosting-the-data-service-wcf-data-services.md)
- [Защита служб WCF Data Services](securing-wcf-data-services.md)
- [Определение служб данных WCF](defining-wcf-data-services.md)
