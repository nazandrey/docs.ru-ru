---
title: "Участники сохраняемости | Microsoft Docs"
ms.custom: ""
ms.date: "03/30/2017"
ms.prod: ".net-framework"
ms.reviewer: ""
ms.suite: ""
ms.tgt_pltfrm: ""
ms.topic: "article"
ms.assetid: f84d2d5d-1c1b-4f19-be45-65b552d3e9e3
caps.latest.revision: 14
author: "Erikre"
ms.author: "erikre"
manager: "erikre"
caps.handback.revision: 14
---
# Участники сохраняемости
Участник сохраняемости может участвовать в операции сохраняемости \(«Сохранение» или «Загрузка»\), запущенной узлом приложения.[!INCLUDE[netfx_current_long](../../../includes/netfx-current-long-md.md)] поставляется с двумя абстрактными классами: **PersistenceParticipant** и **PersistenceIOParticipant**, которые можно использовать для создания участника сохраняемости.Участник сохраняемости создается как производное от одного из этих классов, реализует методы, представляющие интерес, а затем добавляет экземпляр класса в коллекцию <xref:System.ServiceModel.Activities.WorkflowServiceHost.WorkflowExtensions%2A> узла <xref:System.ServiceModel.Activities.WorkflowServiceHost>.Узел приложения может искать такие расширения рабочих процессов при сохранении экземпляра рабочего процесса и в нужное время вызывать соответствующие методы для участников сохраняемости.  
  
 В следующем списке описываются задания, выполняемые подсистемой сохраняемости на разных стадиях операции сохраняемости \(Сохранение\).Участники сохраняемости используются на третей и четвертой стадиях.Если участник сохраняемости также является участником ввода\/вывода \(т.е. участвует в операциях ввода\/вывода\), он также используется на шестой стадии.  
  
1.  Собирает встроенные значения, включая состояние рабочего процесса, закладки, сопоставленные переменные и отметки времени.  
  
2.  Собирает всех участников сохраняемости, которые были добавлены в коллекцию модулей, связанных с экземпляром рабочего процесса.  
  
3.  Вызывает метод <xref:System.Activities.Persistence.PersistenceParticipant.CollectValues%2A>, реализуемый всеми участниками сохранения.  
  
4.  Вызывает метод <xref:System.Activities.Persistence.PersistenceParticipant.MapValues%2A>, реализуемый всеми участниками сохранения.  
  
5.  Сохраняет рабочий процесс в хранилище сохраняемости.  
  
6.  Вызывает метод <xref:System.Activities.Persistence.PersistenceIOParticipant.BeginOnSave%2A> для всех участников сохраняемости, являющихся также участниками ввода\/вывода.Если участник не участвует в вводе\/выводе данных, это задание пропускается.Если эпизод сохраняемости транзакционный, транзакция предоставляется в свойстве Transaction.Current.  
  
7.  Ждет завершения работы всех участников сохраняемости.Фиксирует транзакцию, если все участники успешно сохраняют данные экземпляра.  
  
 Участник сохраняемости является производным от класса **PersistenceParticipant** и может использовать методы **CollectValues** и **MapValues**.Участник сохраняемости, участвующий во вводе\-выводе, является производным от класса **PersistenceIOParticipant** и может использовать метод **BeginOnSave** помимо методов **CollectValues** и **MapValues**.  
  
 Каждая следующая стадия начинается только после завершения предыдущей.Например, на первой стадии собираются данные из **всех** участников сохраняемости.На второй стадии все значения, собранные на первой стадии, предоставляются всем участникам сохраняемости для сопоставления.Затем все значения, собранные и сопоставленные на первой и второй стадиях, предоставляются поставщику сохраняемости на третьей стадии и так далее.  
  
 В следующем списке описываются задания, выполняемые подсистемой сохраняемости на разных стадиях операции загрузки.Участники сохраняемости используются на четвертой стадии.Участники сохраняемости, участвующие в операциях ввода\/вывода, используются также на третьей стадии.  
  
1.  Собирает всех участников сохраняемости, которые были добавлены в коллекцию модулей, связанных с экземпляром рабочего процесса.  
  
2.  Загружает рабочий процесс из хранилища сохраняемости.  
  
3.  Вызывает <xref:System.Activities.Persistence.PersistenceIOParticipant.BeginOnLoad%2A> для всех участников сохраняемости, участвующих также во вводе\/выводе, и ждет завершения работы всех участников сохраняемости.Если эпизод сохраняемости транзакционный, транзакция предоставляется в свойстве Transaction.Current.  
  
4.  Загружает экземпляр рабочего процесса в память на основе данных, извлеченных из хранилища сохраняемости.  
  
5.  Вызывает <xref:System.Activities.Persistence.PersistenceParticipant.PublishValues%2A> на каждом участнике сохраняемости.  
  
 Участник сохраняемости происходит от класса **PersistenceParticipant** и может использовать метод **PublishValues**.Участник сохраняемости, участвующий также во вводе\/выводе, происходит от класса **PersistenceIOParticipant** и может использовать метод **BeginOnLoad** помимо метода **PublishValues**.  
  
 При загрузке экземпляра рабочего процесса поставщик сохраняемости создает блокировку для этого экземпляра.Это не дает загружать экземпляр более чем одному узлу в конфигурации с несколькими узлами.При попытке загрузить экземпляр рабочего процесса, который был заблокирован, появляется исключение наподобие следующего: Исключение: "System.ServiceModel.Persistence.InstanceLockException: Не удалось выполнить запрошенную операцию, поскольку не удалось получить блокировку для экземпляра "00000000\-0000\-0000\-0000\-000000000000".Эта ошибка возникает в одной из следующих ситуаций.  
  
-   В конфигурации с несколькими узлами экземпляр загружен другим узлом.Существует несколько способов решить конфликты этих типов: переадресовать обработку в узел \(владелец блокировки\) и повторить попытку либо выполнить принудительную загрузку, из\-за которой другому узлу не удастся сохранить свою работу.  
  
-   В сценарии с одним узлом, если произошел сбой узла.При повторном запуске \(в результате перезапуска процесса или создания новой фабрики поставщика сохраняемости\) узел пытается загрузить экземпляр, все еще заблокированный старым узлом, поскольку срок блокировки еще не истек.  
  
-   В сценарии с одним узлом работа рассматриваемого экземпляра была в какой\-то момент прервана и был создан новый экземпляр поставщика сохраняемости с другим идентификатором узла.  
  
 Значение времени ожидания блокировки по умолчанию равно 5 минут; вы можете указать другое значение времени ожидания при вызове метода <xref:System.ServiceModel.Persistence.PersistenceProvider.Load%2A>.  
  
## В этом подразделе  
  
-   [Как создать настраиваемого участника сохраняемости](../../../docs/framework/windows-workflow-foundation//how-to-create-a-custom-persistence-participant.md)  
  
## См. также  
 [Расширяемость хранилища](../../../docs/framework/windows-workflow-foundation//store-extensibility.md)