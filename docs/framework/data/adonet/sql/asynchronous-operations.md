---
title: "Асинхронные операции | Microsoft Docs"
ms.custom: ""
ms.date: "03/30/2017"
ms.prod: ".net-framework-4.6"
ms.reviewer: ""
ms.suite: ""
ms.technology: 
  - "dotnet-ado"
ms.tgt_pltfrm: ""
ms.topic: "article"
ms.assetid: e7d32c3c-bf78-4bfc-a357-c9e82e4a4b3c
caps.latest.revision: 5
author: "JennieHubbard"
ms.author: "jhubbard"
manager: "jhubbard"
caps.handback.revision: 5
---
# Асинхронные операции
Для завершения некоторых операций базы данных, например для выполнения команд, требуется значительное время.  В таком случае однопотоковые приложения должны блокировать другие операции и ждать окончания команды перед тем, как они смогут продолжить свои собственные операции.  В противоположность этому, так как поток переднего плана может назначать долговременные операции фоновому потоку, он остается активным во время всей операции.  Приложение Windows, например, делегирующее долговременную операцию фоновому потоку, позволяет потоку пользовательского интерфейса реагировать при выполнении операции.  
  
 .NET Framework предоставляет несколько асинхронных шаблонов конструирования, которые разработчики могут использовать для извлечения преимуществ из фоновых потоков и освобождает пользовательский интерфейс высокоприоритетных потоков от выполнения других операций.  ADO.NET поддерживает те же шаблоны конструирования в его классе <xref:System.Data.SqlClient.SqlCommand>.  Конкретнее, методы <xref:System.Data.SqlClient.SqlCommand.BeginExecuteNonQuery%2A>, <xref:System.Data.SqlClient.SqlCommand.BeginExecuteReader%2A> и <xref:System.Data.SqlClient.SqlCommand.BeginExecuteXmlReader%2A> в сочетании с методами <xref:System.Data.SqlClient.SqlCommand.EndExecuteNonQuery%2A>, <xref:System.Data.SqlClient.SqlCommand.EndExecuteReader%2A> и <xref:System.Data.SqlClient.SqlCommand.EndExecuteXmlReader%2A> предоставляют асинхронную поддержку.  
  
> [!NOTE]
>  Асинхронное программирование является основной функцией .NET Framework, а ADO.NET пользуется всеми преимуществами стандартных шаблонов конструирования.  Дополнительные сведения о разных видах асинхронной техники, доступных разработчикам, см. в разделе [Calling Synchronous Methods Asynchronously](../../../../../docs/standard/asynchronous-programming-patterns/calling-synchronous-methods-asynchronously.md).  
  
 Хотя использование асинхронной техники при помощи функций ADO.NET не требует какого\-либо дополнительного специального рассмотрения, имеется большая вероятность того, что большинство разработчиков будет использовать асинхронные функции в ADO.NET, а не в других областях .NET Framework.  Важно знать о преимуществах и проблемах создания многопотоковых приложений.  Приводимые далее в этом разделы примеры указывают на несколько важных проблем, которые должны учитывать разработчики при построении приложений, включающих функциональные возможности многопотоковой техники.  
  
## В этом подразделе  
 [Windows\-приложения, использующие ответные вызовы](../../../../../docs/framework/data/adonet/sql/windows-applications-using-callbacks.md)  
 Предоставляет пример, демонстрирующий, как безопасно выполнить асинхронную команду, правильно обрабатывающую взаимодействие с формой и свое содержимое из отдельного потока.  
  
 [Приложения ASP.NET, использующие дескрипторы ожидания](../../../../../docs/framework/data/adonet/sql/aspnet-apps-using-wait-handles.md)  
 Предоставляет пример, демонстрирующий, как выполнить несколько параллельных команд из страницы ASP.NET, используя обработки ожидания для управления операцией по завершении всех команд.  
  
 [Опрос в приложениях командной строки](../../../../../docs/framework/data/adonet/sql/polling-in-console-applications.md)  
 Предоставляет пример, демонстрирующий использование ждущего режима для ожидания завершения выполнения асинхронной команды из приложения командной строки.  Эта техника также действительна в библиотеке классов или в другом приложении без пользовательского интерфейса.  
  
## См. также  
 [SQL Server и ADO.NET](../../../../../docs/framework/data/adonet/sql/index.md)   
 [Calling Synchronous Methods Asynchronously](../../../../../docs/standard/asynchronous-programming-patterns/calling-synchronous-methods-asynchronously.md)   
 [Центр разработчиков, поставщики ADO.NET Managed Provider и набор данных](http://go.microsoft.com/fwlink/?LinkId=217917)