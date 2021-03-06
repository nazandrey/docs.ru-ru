---
title: "Конечные точки: адреса, привязки и контракты | Microsoft Docs"
ms.custom: ""
ms.date: "03/30/2017"
ms.prod: ".net-framework-4.6"
ms.reviewer: ""
ms.suite: ""
ms.technology: 
  - "dotnet-clr"
ms.tgt_pltfrm: ""
ms.topic: "article"
helpviewer_keywords: 
  - "конечные точки [WCF]"
  - "WCF [WCF], конечные точки"
  - "Windows Communication Foundation [WCF], конечные точки"
ms.assetid: 9ddc46ee-1883-4291-9926-28848c57e858
caps.latest.revision: 14
author: "Erikre"
ms.author: "erikre"
manager: "erikre"
caps.handback.revision: 14
---
# Конечные точки: адреса, привязки и контракты
Вся связь со службой [!INCLUDE[indigo1](../../../../includes/indigo1-md.md)] осуществляется через *конечные точки* службы.Конечные точки обеспечивают доступ клиентов к функциональным возможностям службы [!INCLUDE[indigo2](../../../../includes/indigo2-md.md)].  
  
 Каждая конечная точка состоит из четырех свойств:  
  
-   адрес, показывающий, где можно найти конечную точку;  
  
-   привязка, показывающая, как клиент может связаться с конечной точкой;  
  
-   контракт, определяющий доступные операции;  
  
-   набор поведений, задающих сведения о локальной реализации конечной точки.  
  
 В настоящем разделе описана структура конечной точки и объясняется, каким образом она представлена в объектной модели [!INCLUDE[indigo2](../../../../includes/indigo2-md.md)].  
  
## Структура конечной точки  
 Каждая конечная точка состоит из следующего.  
  
-   Адрес: адрес однозначно определяет конечную точку и указывает потенциальным потребителям на место расположения службы.В объектной модели [!INCLUDE[indigo2](../../../../includes/indigo2-md.md)] адрес представлен классом <xref:System.ServiceModel.EndpointAddress>.Класс <xref:System.ServiceModel.EndpointAddress> содержит следующее.  
  
    -   Свойство <xref:System.ServiceModel.EndpointAddress.Uri%2A>, представляющее адрес службы.  
  
    -   Свойство <xref:System.ServiceModel.EndpointAddress.Identity%2A>, представляющее удостоверение безопасности службы и коллекцию необязательных заголовков сообщений.Необязательные заголовки сообщений используются для вывода дополнительной и более подробной информации, необходимой для идентификации конечной точки или взаимодействия с ней.  
  
     [!INCLUDE[crdefault](../../../../includes/crdefault-md.md)] [Задание адреса конечной точки](../../../../docs/framework/wcf/specifying-an-endpoint-address.md).  
  
-   Привязка. Привязка задает способ связи клиента с конечной точкой.В том числе следующее:  
  
    -   используемый транспортный протокол \(например, TCP или HTTP\);  
  
    -   используемую в сообщениях кодировку \(например, текст или двоичное кодирование\);  
  
    -   необходимые требования безопасности \(например, безопасность сообщений SSL или SOAP\).  
  
     [!INCLUDE[crdefault](../../../../includes/crdefault-md.md)] [Общие сведения о привязках WCF](../../../../docs/framework/wcf/bindings-overview.md).Привязка в объектной модели [!INCLUDE[indigo2](../../../../includes/indigo2-md.md)] представлена абстрактным базовым классом <xref:System.ServiceModel.Channels.Binding>.В большинстве сценариев пользователи могут использовать только одну из предусмотренных системой привязок.Дополнительные сведения см. в разделе [Привязки, предоставляемые системой](../../../../docs/framework/wcf/system-provided-bindings.md).  
  
-   Контракты. Контракты показывают, какие функциональные возможности дает клиенту конечная точка.В контракте задается следующее:  
  
    -   операции, которые могут быть вызваны клиентом;  
  
    -   форма сообщения;  
  
    -   тип входных параметров или данных, требуемых для вызова операции;  
  
    -   тип обработки или ответного сообщения, который может ожидать клиент.  
  
     Дополнительные сведения об определении контракта см. в [Создание контрактов служб](../../../../docs/framework/wcf/designing-service-contracts.md).  
  
-   Поведения. Поведения конечной точки можно использовать для настройки локального поведения конечной точки службы.Поведения конечной точки выполняют это путем участия в процессе создания среды выполнения [!INCLUDE[indigo2](../../../../includes/indigo2-md.md)].Примером поведения является свойство <xref:System.ServiceModel.Description.ServiceEndpoint.ListenUri%2A>, позволяющее указывать отличный от адреса SOAP или WSDL адрес прослушивания.[!INCLUDE[crdefault](../../../../includes/crdefault-md.md)][ClientViaBehavior](../../../../docs/framework/wcf/diagnostics/wmi/clientviabehavior.md).  
  
## Определение конечных точек  
 Конечную точку для службы можно указать либо императивным методом \(с помощью кода\), либо декларативным \(через настройки\).[!INCLUDE[crdefault](../../../../includes/crdefault-md.md)][Практическое руководство. Создание конечной точки службы в конфигурации](../../../../docs/framework/wcf/feature-details/how-to-create-a-service-endpoint-in-configuration.md) и [Как создать конечную точку службы в коде](../../../../docs/framework/wcf/feature-details/how-to-create-a-service-endpoint-in-code.md).  
  
## В этом разделе  
 В данном разделе объясняется назначение привязок, конечных точек и адресов; показано, как конфигурировать привязку и конечную точку, и демонстрируется, как использовать поведение `ClientVia` и свойство `ListenUri`.  
  
 [Адреса](../../../../docs/framework/wcf/feature-details/endpoint-addresses.md)  
 Описывается, как выполняется обращение к конечным точкам в [!INCLUDE[indigo2](../../../../includes/indigo2-md.md)].  
  
 [Привязки](../../../../docs/framework/wcf/feature-details/bindings.md)  
 Описывается, как привязки используются для указания транспорта, кодировки и данных протокола, требуемых для связи клиентов и служб.  
  
 [Контракты](../../../../docs/framework/wcf/feature-details/contracts.md)  
 Описывается, как контакты определяют методы службы.  
  
 [Практическое руководство. Создание конечной точки службы в конфигурации](../../../../docs/framework/wcf/feature-details/how-to-create-a-service-endpoint-in-configuration.md)  
 Описывается, как создать конечную точку службы в конфигурации.  
  
 [Как создать конечную точку службы в коде](../../../../docs/framework/wcf/feature-details/how-to-create-a-service-endpoint-in-code.md)  
 Описывается, как создать конечную точку службы в коде.  
  
 [Практическое руководство. Использование программы Svcutil.exe для проверки скомпилированного кода службы](../../../../docs/framework/wcf/feature-details/how-to-use-svcutil-exe-to-validate-compiled-service-code.md)  
 Описывается, как находить ошибки в реализациях службы и конфигурациях, не размещая службу при помощи [Служебное средство ServiceModel Metadata Utility Tool \(Svcutil.exe\)](../../../../docs/framework/wcf/servicemodel-metadata-utility-tool-svcutil-exe.md).  
  
## См. также  
 [Настройка служб](../../../../docs/framework/wcf/configuring-services.md)   
 [Расширение привязок](../../../../docs/framework/wcf/extending/extending-bindings.md)