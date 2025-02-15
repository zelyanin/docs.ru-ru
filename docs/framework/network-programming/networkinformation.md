---
title: NetworkInformation
ms.date: 03/30/2017
helpviewer_keywords:
- Network
ms.assetid: 31b44dd3-b903-4a48-8419-40419a3e4038
ms.openlocfilehash: 65b15e61acaa39c9bfc4e0bd81b26f5a211bd1f1
ms.sourcegitcommit: 289e06e904b72f34ac717dbcc5074239b977e707
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/17/2019
ms.locfileid: "71047551"
---
# <a name="networkinformation"></a>NetworkInformation
Пространство имен <xref:System.Net.NetworkInformation> содержит классы для программного сбора информации о событиях, изменениях, статистике и свойствах сети. Можно также определить доступность удаленного узла с помощью класса <xref:System.Net.NetworkInformation.Ping?displayProperty=nameWithType>.  
  
## <a name="network-availability-and-events"></a>Доступность сети и события  
 Класс <xref:System.Net.NetworkInformation.NetworkChange?displayProperty=nameWithType> позволяет определить, изменились ли адрес или состояние доступности сети. Чтобы использовать этот класс, создайте обработчик событий для обработки этого изменения и свяжите его с <xref:System.Net.NetworkInformation.NetworkAddressChangedEventHandler> или <xref:System.Net.NetworkInformation.NetworkAvailabilityChangedEventHandler>. Дополнительные сведения см. в разделе [Практическое руководство. Определение доступности сети и изменений адреса](how-to-detect-network-availability-and-address-changes.md).  
  
## <a name="network-statistics-and-properties"></a>Статистика сети и свойства  
 Вы можете собирать статистику сетевой активности и сведения о свойствах на основе интерфейса или протокола. Классы <xref:System.Net.NetworkInformation.NetworkInterface>, <xref:System.Net.NetworkInformation.NetworkInterfaceType> и <xref:System.Net.NetworkInformation.PhysicalAddress> предоставляют информацию о конкретном сетевом интерфейсе, а классы <xref:System.Net.NetworkInformation.IPInterfaceProperties>, <xref:System.Net.NetworkInformation.IPGlobalProperties>, <xref:System.Net.NetworkInformation.IPGlobalStatistics>, <xref:System.Net.NetworkInformation.TcpStatistics> и <xref:System.Net.NetworkInformation.UdpStatistics> — информацию о пакетах уровня 3 и 4. Дополнительные сведения см. в разделе [Практическое руководство. Получение информации об интерфейсах и протоколах](how-to-get-interface-and-protocol-information.md).  
  
## <a name="determine-if-a-remote-host-is-reachable"></a>Определение доступности удаленного узла  
 С помощью класса <xref:System.Net.NetworkInformation.Ping> можно определить, работает ли удаленный узел, находится ли он в сети и возможен ли доступ к нему. Дополнительные сведения см. в разделе [Практическое руководство. Проверка связи с узлом](how-to-ping-a-host.md).  
  
## <a name="see-also"></a>См. также

- [Примеры сетевого программирования](network-programming-samples.md)
- [Пример сетевых информационных технологий](https://go.microsoft.com/fwlink/?LinkID=179564)
- [Пример технологии средства NetStat](https://go.microsoft.com/fwlink/?LinkID=179562)
- [Пример клиентской технологии проверки связи](https://go.microsoft.com/fwlink/?LinkID=179565)
