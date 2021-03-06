---
title: "Основные сведения о проблемах и исключениях WebRequest"
ms.custom: 
ms.date: 03/30/2017
ms.prod: .net-framework
ms.reviewer: 
ms.suite: 
ms.tgt_pltfrm: 
ms.topic: article
dev_langs:
- VB
- CSharp
- C++
- jsharp
ms.assetid: 74a361a5-e912-42d3-8f2e-8e9a96880a2b
caps.latest.revision: 6
author: mcleblanc
ms.author: markl
manager: markl
ms.translationtype: HT
ms.sourcegitcommit: 306c608dc7f97594ef6f72ae0f5aaba596c936e1
ms.openlocfilehash: 918528e99396bd71f8c44dadcef7f6dfa6a7a47e
ms.contentlocale: ru-ru
ms.lasthandoff: 08/21/2017

---
# <a name="understanding-webrequest-problems-and-exceptions"></a>Основные сведения о проблемах и исключениях WebRequest
Класс <xref:System.Net.WebRequest> и его производные классы (<xref:System.Net.HttpWebRequest>, <xref:System.Net.FtpWebRequest> и <xref:System.Net.FileWebRequest>) сообщают об аномальных состояниях, создавая исключения. Иногда способ решения проблемы не очевиден.  
  
## <a name="solutions"></a>Решения  
 Чтобы определить причину проблемы, изучите свойство <xref:System.Net.WebException.Status%2A> объекта <xref:System.Net.WebException>. В приведенной ниже таблице представлены некоторые значения состояния и возможные решения.  
  
|Состояние|Подробные сведения|Решение|  
|------------|-------------|--------------|  
|<xref:System.Net.WebExceptionStatus.SendFailure><br /><br /> -или-<br /><br /> <xref:System.Net.WebExceptionStatus.ReceiveFailure>|Имеется проблема с базовым сокетом. Возможно, соединение было сброшено.|Подключитесь и отправьте запрос повторно.<br /><br /> Убедитесь в том, что установлен последний пакет обновления.<br /><br /> Увеличьте значение свойства <xref:System.Net.ServicePointManager.MaxServicePointIdleTime%2A?displayProperty=fullName>.<br /><br /> Присвойте свойству <xref:System.Net.HttpWebRequest.KeepAlive%2A?displayProperty=fullName> значение `false`.<br /><br /> Увеличьте максимальное число подключений с помощью свойства <xref:System.Net.ServicePointManager.DefaultConnectionLimit%2A>.<br /><br /> Проверьте конфигурацию прокси-сервера.<br /><br /> При использовании SSL проверьте, имеет ли процесс сервера разрешение на доступ к хранилищу сертификатов.<br /><br /> При отправке большого объема данных присвойте <xref:System.Net.HttpWebRequest.AllowWriteStreamBuffering%2A> значение `false`.|  
|<xref:System.Net.WebExceptionStatus.TrustFailure>|Не удалось проверить сертификат сервера.|Попробуйте открыть универсальный код ресурса (URI) в Internet Explorer. Устраните все проблемы, указанные в уведомлениях системы безопасности, отображаемых в Internet Explorer. Если устранить их не удается, вы можете создать класс политики сертификата, который реализует интерфейс <xref:System.Net.ICertificatePolicy>, возвращающий `true`, и передать его в <xref:System.Net.ServicePointManager.CertificatePolicy%2A>.<br /><br /> См. статью по адресу [http://support.microsoft.com/?id=823177](http://go.microsoft.com/fwlink/?LinkID=179653).<br /><br /> Убедитесь в том, что сертификат центра сертификации, подписавшего сертификат сервера, добавлен в список доверенных центров сертификации в Internet Explorer.<br /><br /> Убедитесь в том, что имя узла в URL-адресе совпадает с общим именем в сертификате сервера.|  
|<xref:System.Net.WebExceptionStatus.SecureChannelFailure>|Произошла ошибка в транзакции SSL или возникла проблема с сертификатом.|Платформа .NET Framework версии 1.1 поддерживает только протокол SSL версии 3.0. Если сервер использует только протокол TLS версии 1.0 или SSL версии 2.0, создается исключение. Обновите платформу .NET Framework до версии 2.0 и задайте <xref:System.Net.ServicePointManager.SecurityProtocol%2A> в соответствии с протоколом сервера.<br /><br /> Сертификат клиента был подписан центром сертификации (ЦС), которому сервер не доверяет. Установите сертификат ЦС на сервере. См. статью по адресу [http://support.microsoft.com/?id=332077](http://go.microsoft.com/fwlink/?LinkID=179654).<br /><br /> Убедитесь в том, что установлен последний пакет обновления.|  
|<xref:System.Net.WebExceptionStatus.ConnectFailure>|Ошибка соединения|Брандмауэр или прокси-сервер блокирует подключение. Измените настройки брандмауэра или прокси-сервера, чтобы разрешить подключение.<br /><br /> Явным образом назначьте <xref:System.Net.WebProxy> в клиентском приложении, вызвав конструктор <xref:System.Net.WebProxy> (WebServiceProxyClass.Proxy = new WebProxy([http://server:80](http://server/), true)).<br /><br /> Запустите программу Filemon или Regmon, чтобы проверить, имеет ли удостоверение рабочего процесса необходимые разрешения на доступ к WSPWSP.dll, HKLM\System\CurrentControlSet\Services\DnsCache или HKLM\System\CurrentControlSet\Services\WinSock2.|  
|<xref:System.Net.WebExceptionStatus.NameResolutionFailure>|Службе доменных имен не удалось разрешить имя узла.|Правильно настройте прокси-сервер. См. статью по адресу [http://support.microsoft.com/?id=318140](http://go.microsoft.com/fwlink/?LinkID=179655).<br /><br /> Убедитесь в том, что установленные антивирусные программы и брандмауэр не блокируют подключение.|  
|<xref:System.Net.WebExceptionStatus.RequestCanceled>|Был вызван метод <xref:System.Net.WebRequest.Abort%2A>, или произошла ошибка.|Эта проблема может быть вызвана большой нагрузкой на клиент или сервер. Уменьшите нагрузку.<br /><br /> Увеличьте значение параметра <xref:System.Net.ServicePointManager.DefaultConnectionLimit%2A>.<br /><br /> Сведения об изменении параметров производительности для веб-службы см. в статье по адресу [http://support.microsoft.com/?id=821268](http://go.microsoft.com/fwlink/?LinkID=179656).|  
|<xref:System.Net.WebExceptionStatus.ConnectionClosed>|Приложение попыталось выполнить запись в сокет, который уже закрыт.|Клиент или сервер перегружен. Уменьшите нагрузку.<br /><br /> Увеличьте значение параметра <xref:System.Net.ServicePointManager.DefaultConnectionLimit%2A>.<br /><br /> Сведения об изменении параметров производительности для веб-службы см. в статье по адресу [http://support.microsoft.com/?id=821268](http://go.microsoft.com/fwlink/?LinkID=179656).|  
|<xref:System.Net.WebExceptionStatus.MessageLengthLimitExceeded>|Превышена установленная максимальная длина сообщения (<xref:System.Net.HttpWebRequest.MaximumResponseHeadersLength%2A>).|Увеличьте значение свойства <xref:System.Net.HttpWebRequest.MaximumResponseHeadersLength%2A>.|  
|<xref:System.Net.WebExceptionStatus.ProxyNameResolutionFailure>|Службе доменных имен не удалось разрешить имя узла прокси-сервера.|Правильно настройте прокси-сервер. См. статью по адресу [http://support.microsoft.com/?id=318140](http://go.microsoft.com/fwlink/?LinkID=179655).<br /><br /> Запретите <xref:System.Net.HttpWebRequest> использовать прокси-сервер, присвоив свойству <xref:System.Net.HttpWebRequest.Proxy%2A> значение `null`.|  
|<xref:System.Net.WebExceptionStatus.ServerProtocolViolation>|Ответ от сервера не является допустимым ответом HTTP. Эта проблема возникает, если платформа .NET Framework определяет, что ответ от сервера не соответствует требованиям документа RFC HTTP 1.1. Причиной может быть то, что ответ содержит неправильные заголовки или разделители заголовков. В документе RFC 2616 определяется протокол HTTP 1.1 и допустимый формат ответа от сервера. Дополнительные сведения см. на сайте [http://www.ietf.org](http://go.microsoft.com/fwlink/?LinkID=147388).|Получите сетевую трассировку транзакции и изучите заголовки в ответе.<br /><br /> Если приложению требуется ответ от сервера без синтаксического разбора (это может создать угрозу безопасности), присвойте `useUnsafeHeaderParsing` значение `true` в файле конфигурации. См. раздел [Элемент \<httpWebRequest> (сетевые параметры)](../../../docs/framework/configure-apps/file-schema/network/httpwebrequest-element-network-settings.md).|  
  
## <a name="see-also"></a>См. также  
 <xref:System.Net.HttpWebRequest>   
 <xref:System.Net.HttpWebResponse>   
 <xref:System.Net.Dns>

