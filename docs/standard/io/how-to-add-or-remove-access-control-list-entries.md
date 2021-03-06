---
title: "Практическое руководство. Добавление или удаление записей списка управления доступом | Microsoft Docs"
ms.custom: ""
ms.date: "03/30/2017"
ms.prod: ".net"
ms.reviewer: ""
ms.suite: ""
ms.technology: 
  - "dotnet-standard"
ms.tgt_pltfrm: ""
ms.topic: "article"
helpviewer_keywords: 
  - "записи управления доступом [платформа .NET Framework]"
  - "списки управления доступом [платформа .NET Framework]"
  - "ACE - записи [платформа .NET Framework]"
  - "ACL - списки [платформа .NET Framework]"
  - "ввод-вывод [платформа .NET Framework], записи списка управления доступом"
ms.assetid: 53758b39-bd9b-4640-bb04-cad5ed8d0abf
caps.latest.revision: 10
author: "mairaw"
ms.author: "mairaw"
manager: "wpickett"
caps.handback.revision: 10
---
# Практическое руководство. Добавление или удаление записей списка управления доступом
Для добавления записей списка управления доступом \(ACL\) в файл или удаления из него необходимо получить объект <xref:System.Security.AccessControl.FileSecurity> или <xref:System.Security.AccessControl.DirectorySecurity> из файла или каталога, изменить его и затем применить обратно к файлу или каталогу.  
  
### Добавление или удаление элемента списка ACL из файла  
  
1.  Вызовите метод <xref:System.IO.File.GetAccessControl%2A> для получения объекта <xref:System.Security.AccessControl.FileSecurity>, содержащего текущие элементы списка ACL файла.  
  
2.  Добавьте и удалите элементы списка ACL из объекта <xref:System.Security.AccessControl.FileSecurity>, возвращенного в шаге 1.  
  
3.  Передайте объект <xref:System.Security.AccessControl.FileSecurity> в метод <xref:System.IO.File.SetAccessControl%2A>, чтобы применить изменения.  
  
### Добавление или удаление элемента списка ACL из каталога  
  
1.  Вызовите метод <xref:System.IO.Directory.GetAccessControl%2A> для получения объекта <xref:System.Security.AccessControl.DirectorySecurity>, содержащего текущие элементы списка ACL каталога.  
  
2.  Добавьте и удалите элементы списка ACL из объекта <xref:System.Security.AccessControl.DirectorySecurity>, возвращенного в шаге 1.  
  
3.  Передайте объект <xref:System.Security.AccessControl.DirectorySecurity> в метод <xref:System.IO.Directory.SetAccessControl%2A>, чтобы применить изменения.  
  
## Пример  
 [!code-cpp[IO.File.GetAccessControl-SetAccessControl#1](../../../samples/snippets/cpp/VS_Snippets_CLR/IO.File.GetAccessControl-SetAccessControl/cpp/sample.cpp#1)]
 [!code-csharp[IO.File.GetAccessControl-SetAccessControl#1](../../../samples/snippets/csharp/VS_Snippets_CLR/IO.File.GetAccessControl-SetAccessControl/CS/sample.cs#1)]
 [!code-vb[IO.File.GetAccessControl-SetAccessControl#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR/IO.File.GetAccessControl-SetAccessControl/VB/sample.vb#1)]  
  
## Компиляция кода  
 Для выполнения этого примера необходимо указать допустимую учетную запись пользователя или группы.  В этом примере используется объект <xref:System.IO.File>; однако та же самая процедура используется для классов <xref:System.IO.FileInfo>, <xref:System.IO.Directory> и <xref:System.IO.DirectoryInfo>.