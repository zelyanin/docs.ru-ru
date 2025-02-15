---
title: Элемент <network> (параметры сети)
ms.date: 03/30/2017
f1_keywords:
- http://schemas.microsoft.com/.NetConfiguration/v2.0#network
- http://schemas.microsoft.com/.NetConfiguration/v2.0#configuration/system.net/mailSettings/smtp/network
helpviewer_keywords:
- <network> element
- network element
ms.assetid: 2c2c6ad4-ed11-48ab-b28e-2bc0ba9b42c7
ms.openlocfilehash: bac288bb28c4176e52366d0e0b7d8bc7d313cf96
ms.sourcegitcommit: 3094dcd17141b32a570a82ae3f62a331616e2c9c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/01/2019
ms.locfileid: "71698006"
---
# <a name="network-element-network-settings"></a>Элемент > @no__t 0network (параметры сети)
Настраивает параметры сети для внешнего SMTP-сервера.  
  
[ **\<configuration>** ](../configuration-element.md)  
&nbsp; @ no__t-1[ **@no__t -4system. NET >** ](system-net-element-network-settings.md)  
&nbsp; @ no__t-1 @ no__t-2 @ no__t-3[ **\<mailSettings >** ](mailsettings-element-network-settings.md)  
&nbsp; @ no__t-1 @ no__t-2 @ no__t-3 @ no__t-4 @ no__t-5[ **\<smtp >** ](smtp-element-network-settings.md)  
&nbsp; @ no__t-1 @ no__t-2 @ no__t-3 @ no__t-4 @ no__t-5 @ no__t-6 @ no__t-7 **\<network >**  
  
## <a name="syntax"></a>Синтаксис  
  
```xml  
<network  
  clientDomain="string"   
  defaultCredentials="true|false"  
  enableSsl="true|false"  
  host="string"   
  password="string"  
  port="integer"   
  targetName="string"  
  userName="string"  
/>  
```  
  
## <a name="attributes-and-elements"></a>Атрибуты и элементы  
 В следующих разделах описаны атрибуты, дочерние и родительские элементы.  
  
### <a name="attributes"></a>Атрибуты  
  
|Атрибут|Описание|  
|---------------|-----------------|  
|`clientDomain`|Указывает доменное имя клиента для использования в первоначальном запросе протокола SMTP для подключения к почтовому SMTP-серверу. Значение по умолчанию — имя localhost локального компьютера, отправляющего запрос.|  
|`defaultCredentials`|Указывает, следует ли использовать учетные данные пользователя по умолчанию для доступа к почтовому серверу SMTP для транзакций SMTP. Значение по умолчанию — `false`.|  
|`enableSsl`|Указывает, используется ли протокол SSL для доступа к почтовому серверу SMTP. Значение по умолчанию — `false`.|  
|`host`|Указывает имя узла почтового SMTP-сервера, используемого для транзакций SMTP. У этого атрибута нет значения по умолчанию.|  
|`password`|Указывает пароль, используемый для проверки подлинности на почтовом сервере SMTP. У этого атрибута нет значения по умолчанию.|  
|`port`|Указывает номер порта, используемый для подключения к почтовому SMTP-серверу. Значение по умолчанию — 25.|  
|`targetName`|Указывает имя поставщика услуг (SPN), используемое для проверки подлинности при использовании расширенной защиты для транзакций SMTP. У этого атрибута нет значения по умолчанию.|  
|`userName`|Указывает имя пользователя, используемое для проверки подлинности на почтовом сервере SMTP. У этого атрибута нет значения по умолчанию.|  
  
### <a name="child-elements"></a>Дочерние элементы  
 Нет.  
  
### <a name="parent-elements"></a>Родительские элементы  
  
|Элемент|Описание|  
|-------------|-----------------|  
|[Элемент > @no__t 1smtp (параметры сети)](smtp-element-network-settings.md)|Настраивает параметры отправки почты по протоколу SMTP.|  
  
## <a name="remarks"></a>Примечания  
 Некоторые SMTP-серверы должны пройти проверку подлинности на сервере перед использованием. Если вы хотите пройти проверку подлинности самостоятельно, используя сетевые учетные данные по умолчанию на узле, задайте для атрибута `defaultCredentials` значение `true`. Свойство <xref:System.Net.Configuration.SmtpNetworkElement.DefaultCredentials%2A?displayProperty=nameWithType> можно использовать для получения текущего значения атрибута `defaultCredentials` из применимых файлов конфигурации.  
  
 Для проверки подлинности на SMTP-сервере также можно использовать обычную проверку подлинности (имя пользователя и пароль). Чтобы использовать этот параметр, необходимо указать допустимое имя пользователя и пароль для указанного SMTP-сервера.  
  
> [!NOTE]
> Обычная проверка подлинности отправляет значения `userName` и `password` на сервер без шифрования. Любой пользователь, отслеживающий сетевой трафик, может просматривать ваши учетные данные и использовать их для подключения к серверу. Рекомендуется использовать более безопасный механизм проверки подлинности, например Kerberos или NT LAN Manager (NTLM). Если `defaultCredentials` равно `true`, то будет использоваться Kerberos или NTLM, если сервер поддерживает эти протоколы.  
  
 Параметры обычной проверки подлинности и сетевых учетных данных по умолчанию являются взаимоисключающими. Если для параметра `defaultCredentials` задано значение `true` и задано имя пользователя и пароль, используются учетные данные сети по умолчанию, а основные данные проверки подлинности игнорируются.  
  
 При обычной проверке подлинности при указании `userName` необходимо также указать `password` для самостоятельной проверки подлинности на почтовом сервере.  
  
 Свойство <xref:System.Net.Configuration.SmtpNetworkElement.UserName%2A?displayProperty=nameWithType> можно использовать для получения текущего значения атрибута `userName` из применимых файлов конфигурации. Свойство <xref:System.Net.Configuration.SmtpNetworkElement.Password%2A?displayProperty=nameWithType> можно использовать для получения текущего значения атрибута `password` из применимых файлов конфигурации. Атрибут `password` обычно не будет указываться в файлах конфигурации по соображениям безопасности.  
  
 Атрибут `clientDomain` изменяет доменное имя клиента, используемое в первом запросе протокола SMTP, на SMTP-сервер. Для атрибута `clientDomain` можно задать полное доменное имя локального компьютера, а не имя localhost, используемое по умолчанию. Это обеспечивает более высокий уровень соответствия стандартам протокола SMTP. Значение по умолчанию — имя localhost локального компьютера, отправляющего запрос. Свойство <xref:System.Net.Configuration.SmtpNetworkElement.ClientDomain%2A?displayProperty=nameWithType> можно использовать для получения текущего значения атрибута `clientDomain` из применимых файлов конфигурации.  
  
 Атрибут `targetName` используется для проверки подлинности при использовании расширенной защиты. Значение по умолчанию — "SMTPSVC/\<host >", где \<host > — имя узла почтового SMTP-сервера. Свойство <xref:System.Net.Configuration.SmtpNetworkElement.TargetName%2A?displayProperty=nameWithType> можно использовать для получения текущего значения атрибута `targetName` из применимых файлов конфигурации.  
  
 Атрибут `enableSsl` указывает, используется ли SSL для доступа к почтовому серверу SMTP. <xref:System.Net.Mail.SmtpClient?displayProperty=nameWithType> Класс поддерживает только расширение службы SMTP для защиты SMTP по протоколу TLS, как определено в RFC 3207. В этом режиме сеанс SMTP начинается на незашифрованном канале, после чего клиент отправляет на сервер команду STARTTLS, чтобы переключиться на безопасное взаимодействие с помощью SSL. Дополнительные сведения см. в RFC 3207, опубликованных с помощью IETF.  
  
 Альтернативный способ подключения заключается в том, где сеанс SSL устанавливается перед отправкой любых команд протокола. Этот метод подключения иногда называют SMTP и по умолчанию использует порт 465. Этот альтернативный метод подключения, использующий SSL, в настоящее время не поддерживается.  
  
 Свойство <xref:System.Net.Configuration.SmtpNetworkElement.EnableSsl%2A?displayProperty=nameWithType> можно использовать для получения текущего значения атрибута `enableSsl` из применимых файлов конфигурации.  
  
## <a name="example"></a>Пример  
 В следующем примере задаются соответствующие параметры SMTP для отправки электронной почты с использованием сетевых учетных данных по умолчанию.  
  
```xml  
<configuration>  
  <system.net>  
    <mailSettings>  
      <smtp deliveryMethod="Network">  
        <network  
          clientDomain="www.contoso.com"  
          defaultCredentials="true"  
          enableSsl="false"  
          host="mail.contoso.com"  
          port="25"  
        />  
      </smtp>  
    </mailSettings>  
  </system.net>  
</configuration>  
```  
  
## <a name="see-also"></a>См. также

- <xref:System.Net.Configuration.SmtpNetworkElement?displayProperty=nameWithType>
- <xref:System.Net.Configuration.SmtpSection?displayProperty=nameWithType>
- <xref:System.Net.Mail.SmtpClient?displayProperty=nameWithType>
- [Схема параметров сети](index.md)
