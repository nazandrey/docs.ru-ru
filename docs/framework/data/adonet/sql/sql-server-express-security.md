---
title: "Безопасность SQL Server Express | Microsoft Docs"
ms.custom: ""
ms.date: "03/30/2017"
ms.prod: ".net-framework-4.6"
ms.reviewer: ""
ms.suite: ""
ms.technology: 
  - "dotnet-ado"
ms.tgt_pltfrm: ""
ms.topic: "article"
ms.assetid: cf9cf6d9-4b05-43e9-ac7b-6cefbfcd6d4e
caps.latest.revision: 6
author: "JennieHubbard"
ms.author: "jhubbard"
manager: "jhubbard"
caps.handback.revision: 6
---
# Безопасность SQL Server Express
Выпуск Microsoft SQL Server Express \(SQL Server Express\) основан на версии Microsoft SQL Server и поддерживает большинство средств ядра базы данных этой версии.  Этот выпуск спроектирован так, что несущественные функции и средства обеспечения сетевого взаимодействия отменены по умолчанию.  Благодаря этому уменьшается контактная зона, доступная для атаки со стороны злонамеренного пользователя.  
  
 Программное обеспечение SQL Server Express обычно устанавливается в виде именованного экземпляра.  Стандартное имя экземпляра \- `SQLExpress`.  Именованный экземпляр определяется по сетевому имени компьютера, а также по имени экземпляра, которое указывается во время установки.  
  
## Сетевой доступ  
 По соображениям безопасности в выпуске SQL Server Express применение сетевых протоколов запрещено по умолчанию.  Это исключает возможность атаки со стороны внешних пользователей, которые могли бы поставить под угрозу компьютер, применяемый для эксплуатации экземпляра SQL Server Express.  Необходимо явно разрешить использование средств сетевого взаимодействия и запустить службу браузера SQL Server для подключения к экземпляру SQL Server Express с другого компьютера.  
  
 После ввода в действие средств сетевого взаимодействия необходимо соблюдать по отношению к экземпляру SQL Server Express такие же требования к безопасности, как и при эксплуатации других выпусков SQL Server.  
  
## Пользовательские экземпляры  
 Пользовательский экземпляр \- это отдельный экземпляр ядра базы данных SQL Server Express, который создается родительским экземпляром SQL Server Express.  Основное назначение пользовательского экземпляра состоит в том, чтобы дать возможность пользователям, эксплуатирующим операционную систему Windows под управлением учетной записи пользователя с минимальными разрешениями, получить права системного администратора \(`sysadmin`\) по отношению к экземпляру SQL Server Express на своем локальном компьютере.  Пользовательские экземпляры не предназначены для пользователей, которые имеют права системных администраторов на своих собственных компьютерах.  
  
 Пользовательский экземпляр создается на основе исходного экземпляра SQL Server или SQL Server Express от имени пользователя.  Он функционирует как пользовательский процесс в контексте безопасности Windows самого пользователя, а не службы.  Применение имен входа SQL Server не разрешено; поддерживаются только имена входа Windows.  Благодаря этому исключается возможность вносить с помощью программного обеспечения, применяемого в рамках пользовательского экземпляра, такие общесистемные изменения, разрешения на выполнение которых пользователь не имеет.  Пользовательский экземпляр именуется также дочерним или клиентским экземпляром, а иногда обозначается с помощью сокращения RANU \(run as normal user \- эксплуатируемый с правами обычного пользователя\).  
  
 Каждый пользовательский экземпляр изолирован от своего родительского экземпляра и от других пользовательских экземпляров, работающих на том же компьютере.  Базы данных, установленные для пользовательских экземпляров, открываются только в однопользовательском режиме; возможность подключения к ним нескольких пользователей отсутствует.  Репликация, распределенные запросы и удаленные соединения для пользовательских экземпляров не разрешены.  Пользователи, подключенные к пользовательскому экземпляру, не имеют каких\-либо особых прав по отношению к родительскому экземпляру SQL Server Express.  
  
## Внешние ресурсы  
 Дополнительные сведения о SQL Server Express см. в указанных ниже ресурсах.  
  
|||  
|-|-|  
|[Электронная документация по SQL Server](http://msdn.microsoft.com/library/bb543165.aspx)|Содержит документацию по SQL Server Express.|  
|[Подключение к SQL Server Express](http://msdn.microsoft.com/library/ms165679.aspx) в электронной документации по SQL Server.|Описывает, как использовать выпуск SQL Server Express в сети.|  
|[Электронная документация по Microsoft SQL Server 2005 Express Edition](http://msdn.microsoft.com/library/ms165706.aspx)|Полная документация по выпуску SQL Server 2005 Express.|  
|[Пользовательские экземпляры для тех, кто не обладает правами администратора](http://msdn.microsoft.com/library/ms143684.aspx) в электронной документации по SQL Server|Описывает, как создать и развернуть пользовательские экземпляры.|  
|[Пользовательские экземпляры SQL Server Express](../../../../../docs/framework/data/adonet/sql/sql-server-express-user-instances.md)|Описывает возможности пользовательского экземпляра в приложении ADO.NET.  Предоставляет сведения о том, как разрешить пользовательский экземпляр, подключиться к пользовательскому экземпляру с помощью <xref:System.Data.SqlClient.SqlConnection>, а также о времени существования пользовательского экземпляра и сценариях пользовательского экземпляра.|  
  
## См. также  
 [Безопасность SQL Server](../../../../../docs/framework/data/adonet/sql/sql-server-security.md)   
 [Пользовательские экземпляры SQL Server Express](../../../../../docs/framework/data/adonet/sql/sql-server-express-user-instances.md)   
 [Центр разработчиков, поставщики ADO.NET Managed Provider и набор данных](http://go.microsoft.com/fwlink/?LinkId=217917)