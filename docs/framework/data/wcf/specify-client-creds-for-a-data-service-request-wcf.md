---
title: "Как указать учетные данные клиента для запроса службы данных (службы WCF Data Services) | Microsoft Docs"
ms.custom: ""
ms.date: "03/30/2017"
ms.prod: ".net-framework-oob"
ms.reviewer: ""
ms.suite: ""
ms.technology: 
  - "dotnet-clr"
ms.tgt_pltfrm: ""
ms.topic: "article"
helpviewer_keywords: 
  - "WCF Data Services, настройка запросов"
ms.assetid: 1632f9af-e45f-4363-9222-03823daa8e28
caps.latest.revision: 2
author: "Erikre"
ms.author: "erikre"
manager: "erikre"
caps.handback.revision: 2
---
# Как указать учетные данные клиента для запроса службы данных (службы WCF Data Services)
По умолчанию клиентская библиотека не поддерживает учетные данные при отправке запроса службе [!INCLUDE[ssODataShort](../../../../includes/ssodatashort-md.md)].  Тем не менее можно задать отправку учетных данных для проверки подлинности запросов службе данных, указав <xref:System.Net.NetworkCredential> для свойства <xref:System.Data.Services.Client.DataServiceContext.Credentials%2A> в контексте <xref:System.Data.Services.Client.DataServiceContext>.  Для получения дополнительной информации см. [Защита служб WCF Data Services](../../../../docs/framework/data/wcf/securing-wcf-data-services.md).  В примере в этом разделе показано, как явно предоставить учетные данные, используемые клиентом [!INCLUDE[ssAstoria](../../../../includes/ssastoria-md.md)] при запросе данных из службы данных.  
  
 Пример в этом разделе использует образец службы данных Northwind и автоматически сформированные клиентские классы службы данных.  Эта служба и клиентские классы данных создаются после выполнения действий, описанных в разделе [Краткое руководство по службам WCF Data Services](../../../../docs/framework/data/wcf/quickstart-wcf-data-services.md).  Можно также использовать [образец службы данных Northwind](http://go.microsoft.com/fwlink/?LinkId=187426), опубликованный на веб\-узле [!INCLUDE[ssODataShort](../../../../includes/ssodatashort-md.md)]. Этот образец службы данных доступен только для чтения, попытка сохранить изменения возвращает ошибку.  Образцы служб данных на веб\-узле [!INCLUDE[ssODataShort](../../../../includes/ssodatashort-md.md)] допускают анонимную проверку подлинности.  
  
## Пример  
 Следующий пример взят из страницы с выделенным кодом для файла на языке XAML, который является основной страницей WPF\-приложения.  В этом примере отображается экземпляр `LoginWindow`, получающий учетные данные у пользователя и использующий их при составлении запроса к службе данных.  
  
  
  
## Пример  
 Ниже приведен код на языке XAML, который определяет основную страницу в WPF\-приложении.  
  
  
  
## Пример  
 Следующий пример взят из страницы с выделенным кодом для окна, которое используется для получения учетных данных проверки подлинности у пользователя, прежде чем отправить запрос службе данных.  
  
  
  
## Пример  
 Ниже приведен код на языке XAML, который определяет имя входа в WPF\-приложении.  
  
  
  
## Безопасность платформы .NET Framework  
 В отношении примера в этом разделе следует учитывать следующие рекомендации по безопасности.  
  
-   Чтобы убедиться, что учетные данные, предоставленные в этом образце, работоспособны, служба данных Northwind должна использовать схему проверки подлинности, отличную от анонимного доступа.  В противном случае веб\-узел, на котором размещена служба данных, не запросит учетные данные.  
  
-   Учетные данные пользователя должны запрашиваться только во время выполнения и не должны кэшироваться.  Учетные данные следует хранить безопасным способом.  
  
-   Данные, отправляемые с помощью базовой и дайджест\-аутентификации, не шифруются, поэтому злоумышленник может их видеть.  Помимо этого, учетные данных обычной проверки подлинности \(имя пользователя и пароль\) отправляются открытым текстом и могут быть перехвачены.  
  
 Для получения дополнительной информации см. [Защита служб WCF Data Services](../../../../docs/framework/data/wcf/securing-wcf-data-services.md).  
  
## См. также  
 [Защита служб WCF Data Services](../../../../docs/framework/data/wcf/securing-wcf-data-services.md)   
 [Клиентская библиотека служб WCF Data Services](../../../../docs/framework/data/wcf/wcf-data-services-client-library.md)