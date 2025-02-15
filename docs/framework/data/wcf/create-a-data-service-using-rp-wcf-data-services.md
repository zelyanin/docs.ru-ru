---
title: Практическое руководство. Создание службы данных с помощью поставщика отражения (WCF Data Services)
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- WCF Data Services, providers
ms.assetid: 7315c6d8-f452-4fb2-a0c1-76ab0593c146
ms.openlocfilehash: feee679c37cd3dafb80021752ee25444c54f0809
ms.sourcegitcommit: d2e1dfa7ef2d4e9ffae3d431cf6a4ffd9c8d378f
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/07/2019
ms.locfileid: "70791006"
---
# <a name="how-to-create-a-data-service-using-the-reflection-provider-wcf-data-services"></a>Практическое руководство. Создание службы данных с помощью поставщика отражения (WCF Data Services)
[!INCLUDE[ssAstoria](../../../../includes/ssastoria-md.md)] позволяет определить модель данных на основе произвольных классов, если они предоставляются как объекты, реализующие интерфейс <xref:System.Linq.IQueryable%601>. Дополнительные сведения см. в разделе [поставщики служб данных](data-services-providers-wcf-data-services.md).  
  
## <a name="example"></a>Пример  
 Следующий пример определяет модель данных, включающую сущности `Orders` и `Items`. Класс контейнера сущностей `OrderItemData` имеет два публичных метода, которые возвращают интерфейсы <xref:System.Linq.IQueryable%601>. Эти интерфейсы являются наборами сущностей типа `Orders` и типами сущностей `Items`. Сущность `Order` может включать несколько сущностей `Items`, поэтому тип сущности `Orders` имеет свойство навигации `Items`, возвращающее коллекцию объектов `Items`. Класс контейнера сущностей `OrderItemData` является универсальным типом класса <xref:System.Data.Services.DataService%601>, производным которого является класс `OrderItems` службы данных.  
  
> [!NOTE]
> Поскольку пример демонстрирует поставщика данных в памяти и изменения не сохраняются за пределами текущих экземпляров объектов, реализация интерфейса <xref:System.Data.Services.IUpdatable> не приносит никаких выгод. Пример, в <xref:System.Data.Services.IUpdatable> котором реализуется интерфейс, см [. в разделе как Создание службы данных с помощью LINQ to SQL источника](create-a-data-service-using-linq-to-sql-wcf.md)данных.  
  
 [!code-csharp[Astoria Reflection Provider#CustomIQueryable](../../../../samples/snippets/csharp/VS_Snippets_Misc/astoria_reflection_provider/cs/orderitems.svc.cs#customiqueryable)]
 [!code-vb[Astoria Reflection Provider#CustomIQueryable](../../../../samples/snippets/visualbasic/VS_Snippets_Misc/astoria_reflection_provider/vb/orderitems.svc.vb#customiqueryable)]  
  
## <a name="see-also"></a>См. также

- [Практическое руководство. Создание службы данных с помощью LINQ to SQL источника данных](create-a-data-service-using-linq-to-sql-wcf.md)
- [Поставщики служб данных](data-services-providers-wcf-data-services.md)
- [Практическое руководство. Создание службы данных с помощью источника данных ADO.NET Entity Framework](create-a-data-service-using-an-adonet-ef-data-wcf.md)
