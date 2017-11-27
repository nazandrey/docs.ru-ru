---
title: "Task Parallelism (Task Parallel Library) | Microsoft Docs"
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
  - "parallelism, task"
ms.assetid: 458b5e69-5210-45e5-bc44-3888f86abd6f
caps.latest.revision: 51
author: "rpetrusha"
ms.author: "ronpet"
manager: "wpickett"
caps.handback.revision: 47
---
# Task Parallelism (Task Parallel Library)
Библиотека параллельных задач \(TPL\) основана на концепции *задач*, представляющих асинхронные операции.  В некотором смысле задача похожа на поток или рабочий элемент <xref:System.Threading.ThreadPool>, но на более высоком уровне абстракции.  Термин *параллелизм задач* означает одновременное выполнение одной или нескольких разных задач.  Задачи предоставляют два основных преимущества.  
  
-   Более эффективное и масштабируемое использование системных ресурсов.  
  
     В фоновом режиме задачи помещаются в очередь <xref:System.Threading.ThreadPool>, усовершенствованную с помощью алгоритмов, которые определяют и настраивают количество потоков и обеспечивают балансировку нагрузки для повышения производительности.  Это делает задачи относительно простыми и позволяет создавать множество задач для использования точного параллелизма.  
  
-   Больший программный контроль по сравнению с потоком или рабочим элементом.  
  
     Задачи и построение платформы на их основе предоставляют богатый набор интерфейсов API, которые поддерживают ожидание, отмену, продолжения, надежную обработку исключений, подробные состояния, пользовательское планирование и многое другое.  
  
 По этим причинам TPL в .NET Framework — предпочтительный API для написания многопоточного асинхронного параллельного кода.  
  
## Неявное создание и запуск задач  
 Метод <xref:System.Threading.Tasks.Parallel.Invoke%2A?displayProperty=fullName> предоставляет удобный способ одновременного запуска любого числа произвольных операторов.  Достаточно передать в делегат <xref:System.Action> для каждого рабочего элемента.  Самым простым способом создания этих делегатов является использование лямбда\-выражений.  Лямбда\-выражение может вызвать именованный метод или предоставить встроенный код.  В следующем примере показан вызов базового метода <xref:System.Threading.Tasks.Parallel.Invoke%2A>, который создает и запускается две задачи, выполняемые параллельно.  Первая задача представляется лямбда\-выражением, вызывающим метод `DoSomeWork`, а вторая — лямбда\-выражением, вызывающим метод `DoSomeOtherWork`.  
  
> [!NOTE]
>  В этой документации для определения делегатов в библиотеке параллельных задач используются лямбда\-выражения.  Если вы не знакомы с лямбда\-выражениями в C\# или Visual Basic, см. раздел [Lambda Expressions in PLINQ and TPL](../../../docs/standard/parallel-programming/lambda-expressions-in-plinq-and-tpl.md).  
  
 [!code-csharp[TPL#21](../../../samples/snippets/csharp/VS_Snippets_Misc/tpl/cs/tpl.cs#21)]
 [!code-vb[TPL#21](../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpl/vb/tpl_vb.vb#21)]  
  
> [!NOTE]
>  Число экземпляров <xref:System.Threading.Tasks.Task>, созданных <xref:System.Threading.Tasks.Parallel.Invoke%2A> в фоновом режиме, не обязательно равно числу предоставленных делегатов.  Библиотека параллельных задач может применять различные оптимизации, особенно с большим количеством делегатов.  
  
 Дополнительные сведения см. в разделе [How to: Use Parallel.Invoke to Execute Parallel Operations](../../../docs/standard/parallel-programming/how-to-use-parallel-invoke-to-execute-parallel-operations.md).  
  
 Для большего контроля над выполнением задач или возврата значения из задачи необходимо более явно работать с объектами <xref:System.Threading.Tasks.Task>.  
  
## Явное создание и запуск задач  
 Задача, не возвращающая значение, представляется классом <xref:System.Threading.Tasks.Task?displayProperty=fullName>.  Задача, возвращающая значение, представляется классом <xref:System.Threading.Tasks.Task%601?displayProperty=fullName>, унаследованным от <xref:System.Threading.Tasks.Task>.  Объект задачи обрабатывает сведения инфраструктуры и предоставляет методы и свойства, доступные из вызывающего потока в течение времени существования задачи.  Например, можно получить доступ к свойству <xref:System.Threading.Tasks.Task.Status%2A> задачи в любое время для определения того, было ли начато ее выполнение, завершилась ли она, была ли отменена или создала исключение.  Состояние представлено перечислением <xref:System.Threading.Tasks.TaskStatus>.  
  
 При создании задачи ей передается пользовательский делегат, инкапсулирующий код, который будет выполнять задача.  Делегат может быть выражен как именованный делегат, анонимный метод или лямбда\-выражение.  Лямбда\-выражения могут содержать вызов именованного метода, как показано в следующем примере.  Обратите внимание, что в пример включен вызов метода <xref:System.Threading.Tasks.Task.Wait%2A?displayProperty=fullName>, чтобы убедиться в окончании выполнения задачи до завершения работы приложения консольного режима.  
  
 [!code-csharp[TPL_TaskIntro#1](../../../samples/snippets/csharp/VS_Snippets_Misc/tpl_taskintro/cs/lambda1.cs#1)]
 [!code-vb[TPL_TaskIntro#1](../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpl_taskintro/vb/lambda1.vb#1)]  
  
 Для создания и запуска задачи в одной операции можно также использовать методы <xref:System.Threading.Tasks.Task.Run%2A?displayProperty=fullName>.  Для управления задачей методы <xref:System.Threading.Tasks.Task.Run%2A> используют планировщик задач по умолчанию независимо от того, какой планировщик связан с текущим потоком.  Методы <xref:System.Threading.Tasks.Task.Run%2A> — предпочтительный способ создания и запуска задач, если не требуется более жесткий контроль над созданием и планированием задачи.  
  
 [!code-csharp[TPL_TaskIntro#2](../../../samples/snippets/csharp/VS_Snippets_Misc/tpl_taskintro/cs/run1.cs#2)]
 [!code-vb[TPL_TaskIntro#2](../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpl_taskintro/vb/run1.vb#2)]  
  
 Для создания и запуска задачи в одной операции можно также использовать метод <xref:System.Threading.Tasks.TaskFactory.StartNew%2A?displayProperty=fullName>.  Используйте этот метод, если нет необходимости разделять создание и планирование и требуются дополнительные параметры создания задач или использование определенного планировщика, а также при необходимости передачи дополнительного состояния задаче через ее свойство <xref:System.Threading.Tasks.Task.AsyncState%2A>, как показано в следующем примере.  
  
 [!code-csharp[TPL_TaskIntro#3](../../../samples/snippets/csharp/VS_Snippets_Misc/tpl_taskintro/cs/startnew1.cs#3)]
 [!code-vb[TPL_TaskIntro#3](../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpl_taskintro/vb/startnew1.vb#3)]  
  
 Задачи <xref:System.Threading.Tasks.Task> и <xref:System.Threading.Tasks.Task%601> предоставляют статическое свойство <xref:System.Threading.Tasks.Task.Factory%2A>, возвращающее экземпляр по умолчанию объекта <xref:System.Threading.Tasks.TaskFactory>, чтобы можно было вызвать метод как `Task.Factory.StartNew()`.  Кроме того, поскольку в следующем примере задачи относятся к типу <xref:System.Threading.Tasks.Task%601?displayProperty=fullName>, каждая из них имеет открытое свойство <xref:System.Threading.Tasks.Task%601.Result%2A?displayProperty=fullName>, содержащее результат вычисления.  Задачи выполняются асинхронно и могут завершиться в любом порядке.  При обращении к свойству <xref:System.Threading.Tasks.Task%601.Result%2A> до завершения вычисления оно блокирует вызывающий поток до тех пор, пока значение не станет доступно.  
  
 [!code-csharp[TPL_TaskIntro#4](../../../samples/snippets/csharp/VS_Snippets_Misc/tpl_taskintro/cs/result1.cs#4)]
 [!code-vb[TPL_TaskIntro#4](../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpl_taskintro/vb/result1.vb#4)]  
  
 Дополнительные сведения см. в разделе [How to: Return a Value from a Task](../../../docs/standard/parallel-programming/how-to-return-a-value-from-a-task.md).  
  
 При использовании лямбда\-выражения для создания делегата имеется доступ ко всем переменным, видимым на этом этапе в исходном коде.  Однако в некоторых случаях, особенно в циклах, лямбда\-выражение не перехватывает переменную, как можно было бы ожидать.  Оно только перехватывает окончательное значение, а не значение, изменяющееся после каждой итерации.  В следующем примере показана эта проблема.  В нем счетчик цикла передается лямбда\-выражению, создающему экземпляр объекта `CustomData`, и используется в качестве идентификатора объекта.  Как видно из выходных данных примера, все объекты `CustomData` имеют одинаковые идентификаторы.  
  
 [!code-csharp[TPL_TaskIntro#22](../../../samples/snippets/csharp/VS_Snippets_Misc/tpl_taskintro/cs/iteration1b.cs#22)]
 [!code-vb[TPL_TaskIntro#22](../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpl_taskintro/vb/iteration1b.vb#22)]  
  
 Доступ к значению для каждой итерации можно получить, предоставив объект состояния задаче через ее конструктор.  В следующем примере предыдущий пример изменяется путем использования счетчика цикла при создании объекта `CustomData`, который, в свою очередь, передается лямбда\-выражению.  Как видно из выходных данных примера, каждый объект `CustomData` теперь имеет уникальный идентификатор, основанный на значении счетчика цикла в момент создания экземпляра объекта.  
  
 [!code-csharp[TPL_TaskIntro#21](../../../samples/snippets/csharp/VS_Snippets_Misc/tpl_taskintro/cs/iteration1a.cs#21)]
 [!code-vb[TPL_TaskIntro#21](../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpl_taskintro/vb/iteration1a.vb#21)]  
  
 Это состояние передается в качестве аргумента делегату задачи, и доступ к нему можно получить из объекта задачи с помощью свойства <xref:System.Threading.Tasks.Task.AsyncState%2A?displayProperty=fullName>.  В следующем примере представлен вариант предыдущего примера.  В нем используется свойство <xref:System.Threading.Tasks.Task.AsyncState%2A> для отображения сведений об объектах `CustomData`, переданных лямбда\-выражению.  
  
 [!code-csharp[TPL_TaskIntro#23](../../../samples/snippets/csharp/VS_Snippets_Misc/tpl_taskintro/cs/asyncstate.cs#23)]
 [!code-vb[TPL_TaskIntro#23](../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpl_taskintro/vb/asyncstate.vb#23)]  
  
## Идентификатор задачи  
 Каждая задача получает целочисленный идентификатор, уникально определяющий ее в домене приложения. Доступ к нему можно получить с помощью свойства <xref:System.Threading.Tasks.Task.Id%2A?displayProperty=fullName>.  Этот идентификатор полезен для просмотра сведений о задаче в окнах **Параллельные стеки** и **Задачи** отладчика Visual Studio.  Он создается только после того, как запрашивается. Поэтому при каждом запуске программы задача может иметь разные идентификаторы.  Дополнительные сведения о том, как просматривать идентификаторы задач в отладчике, см. в разделах [Использование окна задач](../Topic/Using%20the%20Tasks%20Window.md) и [Использование окна "Параллельные стеки"](../Topic/Using%20the%20Parallel%20Stacks%20Window.md).  
  
## Параметры создания задачи  
 Большинство интерфейсов API, в которых создаются задачи, предоставляют перегрузки, принимающие параметр <xref:System.Threading.Tasks.TaskCreationOptions>.  Указывая один из этих параметров, пользователь задает планировщику задач способ планирования задачи в пуле потоков.  В следующей таблице перечислены различные параметры создания задач.  
  
|Значение параметра <xref:System.Threading.Tasks.TaskCreationOptions>|Описание|  
|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------|  
|<xref:System.Threading.Tasks.TaskCreationOptions.None>|Значение по умолчанию, если параметр не задан.  Планировщик использует его эвристику по умолчанию для планирования задачи.|  
|<xref:System.Threading.Tasks.TaskCreationOptions.PreferFairness>|Указывает, что задачу необходимо планировать так, чтобы созданные раньше задачи выполнялись раньше, а более поздние задачи — позже.|  
|<xref:System.Threading.Tasks.TaskCreationOptions.LongRunning>|Указывает, что задача представляет длительную операцию.|  
|<xref:System.Threading.Tasks.TaskCreationOptions.AttachedToParent>|Указывает, что задача должна быть создана как вложенная дочерняя задача текущей задачи, если таковая существует.  Дополнительные сведения см. в разделе [Attached and Detached Child Tasks](../../../docs/standard/parallel-programming/attached-and-detached-child-tasks.md).|  
|<xref:System.Threading.Tasks.TaskCreationOptions.DenyChildAttach>|Указывает, что, если внутренняя задача определяет параметр `AttachedToParent`, эта задача не станет дочерней присоединенной задачей.|  
|<xref:System.Threading.Tasks.TaskCreationOptions.HideScheduler>|Указывает, что планировщик задач для задач, созданных вызывающими методами \(например, <xref:System.Threading.Tasks.TaskFactory.StartNew%2A?displayProperty=fullName> или <xref:System.Threading.Tasks.Task%601.ContinueWith%2A?displayProperty=fullName>\) из определенной задачи, — это планировщик по умолчанию, а не планировщик, в котором выполняется эта задача.|  
  
 Параметры можно объединить с использованием побитовой операции **OR**.  В следующем примере показана задача с параметрами <xref:System.Threading.Tasks.TaskCreationOptions> и <xref:System.Threading.Tasks.TaskContinuationOptions>.  
  
 [!code-csharp[TPL_TaskIntro#03](../../../samples/snippets/csharp/VS_Snippets_Misc/tpl_taskintro/cs/taskintro.cs#03)]
 [!code-vb[TPL_TaskIntro#03](../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpl_taskintro/vb/tpl_intro.vb#03)]  
  
## Задачи, потоки и язык и региональные параметры  
 Каждый поток имеет связанный язык и региональные параметры и язык и региональные параметры пользовательского интерфейса, которые определяются свойствами <xref:System.Threading.Thread.CurrentCulture%2A?displayProperty=fullName> и <xref:System.Threading.Thread.CurrentUICulture%2A?displayProperty=fullName> соответственно.  Язык и региональные параметры потока используются в таких операциях, как форматирование, анализ, сортировка и сравнение строк.  Язык и региональные параметры пользовательского интерфейса потока используются при поиске ресурсов.  Как правило, если не заданы язык и региональные параметры по умолчанию для всех потоков в домене приложения с помощью свойств <xref:System.Globalization.CultureInfo.DefaultThreadCurrentCulture%2A?displayProperty=fullName> и <xref:System.Globalization.CultureInfo.DefaultThreadCurrentUICulture%2A?displayProperty=fullName>, то по умолчанию язык и региональные параметры пользовательского интерфейса потока определяется языком и региональными параметрами системы.  Если вы явно задаете язык и региональные параметры потока и запускаете новый поток, этот новый поток не наследует язык и региональные параметры вызывающего потока; вместо них по умолчанию используется язык и региональные параметры системы.  Такого подхода придерживается модель программирования на основе задач для приложений, предназначенных для версий .NET Framework, предшествующих версии [!INCLUDE[net_v46](../../../includes/net-v46-md.md)].  
  
> [!IMPORTANT]
>  Обратите внимание, что язык и региональные параметры вызывающего потока как часть контекста задачи применяется к приложениям, *предназначенным* для [!INCLUDE[net_v46](../../../includes/net-v46-md.md)], а не к приложениям, *работающим в* [!INCLUDE[net_v46](../../../includes/net-v46-md.md)].  Вы можете ориентироваться на конкретную версию платформы .NET Framework при создании проекта в Visual Studio, выбрав ее в раскрывающемся списке в верхней части диалогового окна **Новый проект**, а вне Visual Studio можете использовать атрибут <xref:System.Runtime.Versioning.TargetFrameworkAttribute>.  В приложениях, предназначенных для версий .NET Framework до [!INCLUDE[net_v46](../../../includes/net-v46-md.md)] или не предназначенных для определенной версии .NET Framework, язык и региональные параметры задачи по\-прежнему определяются языком и региональными параметрами потока, в котором она выполняется.  
  
 Начиная с приложений, предназначенных для [!INCLUDE[net_v46](../../../includes/net-v46-md.md)], язык и региональные параметры вызывающего потока наследуются каждой задачей, даже если задача выполняется асинхронно в потоке пула потоков.  
  
 Следующий пример иллюстрирует это.  Он использует атрибут <xref:System.Runtime.Versioning.TargetFrameworkAttribute> для указания [!INCLUDE[net_v46](../../../includes/net-v46-md.md)] и изменяет текущий язык и региональные параметры приложения на французский \(Франция\), а если французский \(Франция\) уже является текущим языком, то изменяет его на английский \(США\).  Затем он вызывает делегат с именем `formatDelegate`, который возвращает некоторые числа в виде значений валюты в новом языке и региональных параметрах.  Обратите внимание, что независимо от того, действует ли делегат как синхронная или асинхронная задача, он возвращает ожидаемый результат, поскольку язык и региональные параметры вызывающего потока наследуются асинхронной задачей.  
  
 [!code-csharp[System.Globalization.CultureInfo.Class.Async#5](../../../samples/snippets/csharp/VS_Snippets_CLR_System/system.globalization.cultureinfo.class.async/cs/asyncculture1.cs#5)]
 [!code-vb[System.Globalization.CultureInfo.Class.Async#5](../../../samples/snippets/visualbasic/VS_Snippets_CLR_System/system.globalization.cultureinfo.class.async/vb/asyncculture1.vb#5)]  
  
 При использовании Visual Studio можно не указывать атрибут <xref:System.Runtime.Versioning.TargetFrameworkAttribute>, выбрав вместо этого .NET Framework 4.6 в качестве целевой платформы при создании проекта в диалоговом окне **Новый проект**.  
  
 Чтобы получить выходные данные, которые отражают поведение приложений для версий платформы .NET Framework до версии [!INCLUDE[net_v46](../../../includes/net-v46-md.md)], удалите атрибут <xref:System.Runtime.Versioning.TargetFrameworkAttribute> из исходного кода.  Выходные данные будут отражать соглашения о форматировании языка и региональных параметров системы по умолчанию, а не языка и региональных параметров вызывающего потока.  
  
 Дополнительные сведения об асинхронных задачах и языке и региональных параметрах см. в разделе "Язык и региональные параметры и асинхронные операции на основе задач" в описании класса <xref:System.Globalization.CultureInfo>.  
  
## Создание продолжений задач  
 С помощью методов <xref:System.Threading.Tasks.Task.ContinueWith%2A?displayProperty=fullName> и <xref:System.Threading.Tasks.Task%601.ContinueWith%2A?displayProperty=fullName> можно указать, чтобы задача запускалась по завершении *предшествующей задачи*.  Делегат задачи продолжения передается в качестве ссылки на предшествующую задачу, чтобы он мог проверить состояние предшествующей задачи и, получив значение свойства <xref:System.Threading.Tasks.Task%601.Result%2A?displayProperty=fullName>, использовать выходные данные предшествующей задачи в качестве входных данных для продолжения.  
  
 В следующем примере задача `getData` запускается вызовом метода <xref:System.Threading.Tasks.TaskFactory.StartNew%60%601%28System.Func%7B%60%600%7D%29?displayProperty=fullName>.  Задача `processData` запускается автоматически по завершении задачи `getData`, а задача `displayData` запускается по завершении задачи `processData`.  Задача `getData` создает целочисленный массив, доступный задаче `processData` через свойство `getData` задачи <xref:System.Threading.Tasks.Task%601.Result%2A?displayProperty=fullName>.  Задача `processData` обрабатывает этот массив и возвращает результат, тип которого определяется на основе возвращаемого типа лямбда\-выражения, переданного методу <xref:System.Threading.Tasks.Task%601.ContinueWith%60%601%28System.Func%7BSystem.Threading.Tasks.Task%7B%600%7D%2C%60%600%7D%29?displayProperty=fullName>.  Задача `displayData` выполняется автоматически по завершении задачи `processData`, и объект <xref:System.Tuple%603>, возвращенный лямбда\-выражением `processData`, доступен задаче `displayData` через свойство `processData` задачи <xref:System.Threading.Tasks.Task%601.Result%2A?displayProperty=fullName>.  Задача `displayData` принимает результат задачи `processData` и выдает результат, тип которого определяется аналогичным образом. Этот результат становится доступным программе в свойстве <xref:System.Threading.Tasks.Task%601.Result%2A>.  
  
 [!code-csharp[TPL_TaskIntro#5](../../../samples/snippets/csharp/VS_Snippets_Misc/tpl_taskintro/cs/continuations1.cs#5)]
 [!code-vb[TPL_TaskIntro#5](../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpl_taskintro/vb/continuations1.vb#5)]  
  
 Поскольку метод <xref:System.Threading.Tasks.Task.ContinueWith%2A?displayProperty=fullName> является экземплярным, вызовы этого метода можно объединять в цепочку, а не создавать экземпляр объекта <xref:System.Threading.Tasks.Task%601> для каждой предшествующей задачи.  Следующий пример функционально идентичен предыдущему, за исключением того, вызовы метода <xref:System.Threading.Tasks.Task.ContinueWith%2A?displayProperty=fullName> в нем объединяются в цепочку.  Обратите внимание, что объект <xref:System.Threading.Tasks.Task%601>, возвращаемый цепочкой вызовов метода, является последней задачей продолжения.  
  
 [!code-csharp[TPL_TaskIntro#24](../../../samples/snippets/csharp/VS_Snippets_Misc/tpl_taskintro/cs/continuations2.cs#24)]
 [!code-vb[TPL_TaskIntro#24](../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpl_taskintro/vb/continuations2.vb#24)]  
  
 С помощью методов <xref:System.Threading.Tasks.TaskFactory.ContinueWhenAll%2A> и <xref:System.Threading.Tasks.TaskFactory.ContinueWhenAny%2A> можно продолжить выполнение с нескольких задач.  
  
 Дополнительные сведения см. в разделе [Chaining Tasks by Using Continuation Tasks](../../../docs/standard/parallel-programming/chaining-tasks-by-using-continuation-tasks.md).  
  
## Создание отсоединенных дочерних задач  
 Если в пользовательском коде, выполняемом в некоторой задаче, создается новая задача и не задается параметр <xref:System.Threading.Tasks.TaskCreationOptions>, новая задача не синхронизируется с родительской никаким особым способом.  Такой тип несинхронизированной задачи называется *отсоединенной вложенной задачей* или *отсоединенной дочерней задачей*.  В следующем примере показана задача, создающая одну отсоединенную дочернюю задачу.  
  
 [!code-csharp[TPL_TaskIntro#07](../../../samples/snippets/csharp/VS_Snippets_Misc/tpl_taskintro/cs/taskintro.cs#07)]
 [!code-vb[TPL_TaskIntro#07](../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpl_taskintro/vb/tpl_intro.vb#07)]  
  
 Обратите внимание, что родительская задача не ожидает завершения отсоединенной дочерней задачи.  
  
## Создание дочерних задач  
 Если в пользовательском коде, выполняемом в некоторой задаче, создается задача с параметром <xref:System.Threading.Tasks.TaskCreationOptions>, новая задача называется *присоединенной дочерней задачей* родительской задачи.  Параметр <xref:System.Threading.Tasks.TaskCreationOptions> можно использовать для выражения структурированного параллелизма задач, поскольку родительская задача неявно ожидает завершения всех присоединенных дочерних задач.  В следующем примере показана родительская задача, создающая десять присоединенных дочерних задач.  Обратите внимание, что в этом примере вызывается метод <xref:System.Threading.Tasks.Task.Wait%2A?displayProperty=fullName> для ожидания завершения родительской задачи. Явное ожидание завершения присоединенных дочерних задач не требуется.  
  
 [!code-csharp[TPL_TaskIntro#8](../../../samples/snippets/csharp/VS_Snippets_Misc/tpl_taskintro/cs/child1.cs#8)]
 [!code-vb[TPL_TaskIntro#8](../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpl_taskintro/vb/child1.vb#8)]  
  
 В родительской задаче может использоваться параметр <xref:System.Threading.Tasks.TaskCreationOptions?displayProperty=fullName>, не позволяющий другим задачам присоединяться к ней.  Дополнительные сведения см. в разделе [Attached and Detached Child Tasks](../../../docs/standard/parallel-programming/attached-and-detached-child-tasks.md).  
  
## Ожидание завершения задач  
 Типы <xref:System.Threading.Tasks.Task?displayProperty=fullName> и <xref:System.Threading.Tasks.Task%601?displayProperty=fullName> предоставляют несколько перегрузок методов <xref:System.Threading.Tasks.Task.Wait%2A?displayProperty=fullName> и <xref:System.Threading.Tasks.Task%601.Wait%2A?displayProperty=fullName>, которые позволяют ожидать завершения задачи.  Кроме того, перегрузки статических методов <xref:System.Threading.Tasks.Task.WaitAll%2A?displayProperty=fullName> и <xref:System.Threading.Tasks.Task.WaitAny%2A?displayProperty=fullName> позволяют ожидать завершения какого\-либо или всех массивов задач.  
  
 Как правило, ожидание задачи выполняется по одной из следующих причин.  
  
-   Основной поток зависит от конечного результата, вычисленного задачей.  
  
-   Необходимо обрабатывать исключения, которые могут быть созданы из задачи.  
  
-   Приложение может завершиться до окончания выполнения всех задач.  Например, выполнение консольных приложений завершается после выполнения всего синхронного кода в `Main` \(точке входа приложения\).  
  
 В следующем примере показан базовый шаблон, в котором не указана обработка исключений.  
  
 [!code-csharp[TPL_TaskIntro#06](../../../samples/snippets/csharp/VS_Snippets_Misc/tpl_taskintro/cs/taskintro.cs#06)]
 [!code-vb[TPL_TaskIntro#06](../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpl_taskintro/vb/tpl_intro.vb#06)]  
  
 Пример обработки исключений см. в разделе [Практическое руководство. Обработка исключений, создаваемых задачами](http://msdn.microsoft.com/ru-ru/d6c47ec8-9de9-4880-beb3-ff19ae51565d).  
  
 Некоторые перегрузки позволяют задать время ожидания, а другие принимают дополнительный объект <xref:System.Threading.CancellationToken> в качестве входного параметра, чтобы ожидание можно было отменить либо программно, либо в ответ на введенные пользователем данные.  
  
 При ожидании задачи неявно ожидаются все ее дочерние задачи, созданные с помощью параметра <xref:System.Threading.Tasks.TaskCreationOptions?displayProperty=fullName>.  <xref:System.Threading.Tasks.Task.Wait%2A?displayProperty=fullName> выполняет возврат немедленно, если задача уже завершена.  Любые исключения, вызванные задачей, будут созданы методом <xref:System.Threading.Tasks.Task.Wait%2A>, даже если метод <xref:System.Threading.Tasks.Task.Wait%2A> был вызван после завершения задачи.  
  
 Дополнительные сведения см. в разделе [How to: Wait on One or More Tasks to Complete](../Topic/How%20to:%20Wait%20on%20One%20or%20More%20Tasks%20to%20Complete.md).  
  
## Составление задач  
 Классы <xref:System.Threading.Tasks.Task> и <xref:System.Threading.Tasks.Task%601> предоставляют несколько методов, позволяющих создать ряд задач для реализации общих шаблонов и более эффективного использования асинхронных функций языка, предусмотренных в языках C\#, [!INCLUDE[vbprvb](../../../includes/vbprvb-md.md)] и F\#.  В этом подразделе описаны методы <xref:System.Threading.Tasks.Task.WhenAll%2A>, <xref:System.Threading.Tasks.Task.WhenAny%2A>, <xref:System.Threading.Tasks.Task.Delay%2A> и <xref:System.Threading.Tasks.Task.FromResult%2A>.  
  
### Task.WhenAll  
 Метод <xref:System.Threading.Tasks.Task.WhenAll%2A?displayProperty=fullName> асинхронно ожидает завершения выполнения нескольких объектов <xref:System.Threading.Tasks.Task> или <xref:System.Threading.Tasks.Task%601>.  Он предоставляет перегруженные версии, позволяющие ожидать неединобразные наборы задач.  Например, можно ожидать завершения выполнения нескольких объектов <xref:System.Threading.Tasks.Task> и <xref:System.Threading.Tasks.Task%601> от одного вызова метода.  
  
### Task.WhenAny  
 Метод <xref:System.Threading.Tasks.Task.WhenAny%2A?displayProperty=fullName> асинхронно ожидает завершения выполнения одного из нескольких объектов <xref:System.Threading.Tasks.Task> или <xref:System.Threading.Tasks.Task%601>.  Как и метод <xref:System.Threading.Tasks.Task.WhenAll%2A?displayProperty=fullName>, этот метод предоставляет перегруженные версии, позволяющие ожидать неединобразные наборы задач.  Метод <xref:System.Threading.Tasks.Task.WhenAny%2A> особенно полезен в следующих ситуациях.  
  
-   Избыточные операции.  Рассмотрим алгоритм или операцию, которые можно выполнить несколькими способами.  Метод <xref:System.Threading.Tasks.Task.WhenAny%2A> можно использовать для выбора операции, завершающейся первой, и последующей отмены оставшихся операций.  
  
-   Операции с чередованием.  Можно запустить несколько операций, которые все должны завершиться, и использовать метод <xref:System.Threading.Tasks.Task.WhenAny%2A> для обработки результатов при завершении каждой операции.  После завершения одной операции можно запустить одну или несколько дополнительных задач.  
  
-   Регулируемые операции.  Метод <xref:System.Threading.Tasks.Task.WhenAny%2A> можно использовать для расширения предыдущего сценария путем ограничения количества одновременно выполняемых операций.  
  
-   Операции с истекшим сроком действия.  Метод <xref:System.Threading.Tasks.Task.WhenAny%2A> можно использовать, чтобы сделать выбор между одной или несколькими задачами и задачей, завершающейся после определенного времени, например задачей, возвращаемой методом <xref:System.Threading.Tasks.Task.Delay%2A>.  Метод <xref:System.Threading.Tasks.Task.Delay%2A> описан в следующем разделе.  
  
### Task.Delay  
 Метод <xref:System.Threading.Tasks.Task.Delay%2A?displayProperty=fullName> создает объект <xref:System.Threading.Tasks.Task>, завершающийся после определенного времени.  Этот метод можно использовать для создания циклов, которые иногда запрашивают данные, вводят тайм\-ауты, задерживают обработку вводимых пользователем данных на заранее определенное время и т. д.  
  
### Task\(T\).FromResult  
 С помощью метода <xref:System.Threading.Tasks.Task.FromResult%2A?displayProperty=fullName> можно создать объект <xref:System.Threading.Tasks.Task%601>, содержащий предварительно вычисленный результат.  Этот метод полезен тогда, когда выполняется асинхронная операция, возвращающая объект <xref:System.Threading.Tasks.Task%601>, и результат этого объекта <xref:System.Threading.Tasks.Task%601> уже вычислен.  Пример использования метода <xref:System.Threading.Tasks.Task.FromResult%2A> для получения результатов асинхронных операций загрузки, удерживаемых в кэше, см. в разделе [How to: Create Pre\-Computed Tasks](../../../docs/standard/parallel-programming/how-to-create-pre-computed-tasks.md).  
  
## Обработка исключений в задачах  
 Если задача создает одно или несколько исключений, они заключаются в исключение <xref:System.AggregateException>.  Это исключение распространяется обратно в поток, который соединяется с задачей и обычно является потоком, ожидающим завершения задачи или обращающимся к свойству <xref:System.Threading.Tasks.Task%601.Result%2A>.  Такое поведение служит для принудительного выполнения политики .NET Framework, согласно которой все необработанные исключения по умолчанию должны завершать процесс.  Ниже указаны элементы блока `try`\/`catch`, с помощью любого из которых вызывающий код может обрабатывать исключения:  
  
-   метод <xref:System.Threading.Tasks.Task.Wait%2A>;  
  
-   метод <xref:System.Threading.Tasks.Task.WaitAll%2A>;  
  
-   метод <xref:System.Threading.Tasks.Task.WaitAny%2A>;  
  
-   Свойство <xref:System.Threading.Tasks.Task%601.Result%2A>.  
  
 Присоединяемый поток также может обрабатывать исключения, обращаясь к свойству <xref:System.Threading.Tasks.Task.Exception%2A> до того, как задача будет собрана сборщиком мусора.  Обращаясь к этому свойству, вы не позволяете необработанному исключению запустить поведение распространения исключений, которое завершает процесс по окончании работы объекта.  
  
 Дополнительные сведения об исключениях и задачах см. в разделах [Обработка исключений](../../../docs/standard/parallel-programming/exception-handling-task-parallel-library.md) и [Практическое руководство. Обработка исключений, создаваемых задачами](http://msdn.microsoft.com/ru-ru/d6c47ec8-9de9-4880-beb3-ff19ae51565d).  
  
## Отмена задач  
 Класс `Task` поддерживает совместную отмену и полностью интегрирован с классами <xref:System.Threading.CancellationTokenSource?displayProperty=fullName> и <xref:System.Threading.CancellationToken?displayProperty=fullName>, появившимися в .NET Framework 4.  Большинство конструкторов в классе <xref:System.Threading.Tasks.Task?displayProperty=fullName> принимают объект <xref:System.Threading.CancellationToken> в качестве входного параметра.  Многие из перегрузок <xref:System.Threading.Tasks.TaskFactory.StartNew%2A> и <xref:System.Threading.Tasks.Task.Run%2A> также содержат параметр <xref:System.Threading.CancellationToken>.  
  
 Можно создать токен и выдать запрос отмены позднее с помощью класса <xref:System.Threading.CancellationTokenSource>.  Передайте токен <xref:System.Threading.Tasks.Task> в качестве аргумента и ссылайтесь на тот же токен в пользовательском делегате, который не отвечает на запрос отмены.  
  
 Дополнительные сведения см. в разделах [Task Cancellation](../../../docs/standard/parallel-programming/task-cancellation.md) и [How to: Cancel a Task and Its Children](../../../docs/standard/parallel-programming/how-to-cancel-a-task-and-its-children.md).  
  
## Класс TaskFactory  
 Класс <xref:System.Threading.Tasks.TaskFactory> предоставляет статические методы, которые инкапсулируют некоторые распространенные шаблоны для создания и запуска задач и задач продолжения.  
  
-   Наиболее распространенным шаблоном является <xref:System.Threading.Tasks.TaskFactory.StartNew%2A>, который создает и запускает задачу в одном операторе.  
  
-   При создании задач продолжения из нескольких предшествующих задач используйте метод <xref:System.Threading.Tasks.TaskFactory.ContinueWhenAll%2A> или <xref:System.Threading.Tasks.TaskFactory.ContinueWhenAny%2A> либо аналогичные методы в классе <xref:System.Threading.Tasks.Task%601>.  Дополнительные сведения см. в разделе [Chaining Tasks by Using Continuation Tasks](../../../docs/standard/parallel-programming/chaining-tasks-by-using-continuation-tasks.md).  
  
-   Чтобы инкапсулировать методы `BeginX` и `EndX` модели асинхронного программирования в экземпляре <xref:System.Threading.Tasks.Task> или <xref:System.Threading.Tasks.Task%601>, используйте методы <xref:System.Threading.Tasks.TaskFactory.FromAsync%2A>.  Дополнительные сведения см. в разделе [TPL and Traditional .NET Framework Asynchronous Programming](../../../docs/standard/parallel-programming/tpl-and-traditional-async-programming.md).  
  
 Класс <xref:System.Threading.Tasks.TaskFactory> доступен как статическое свойство класса <xref:System.Threading.Tasks.Task> или <xref:System.Threading.Tasks.Task%601>.  Кроме того, класс <xref:System.Threading.Tasks.TaskFactory> можно создать напрямую и указать различные параметры, включающие <xref:System.Threading.CancellationToken>, параметр <xref:System.Threading.Tasks.TaskCreationOptions>, параметр <xref:System.Threading.Tasks.TaskContinuationOptions> или <xref:System.Threading.Tasks.TaskScheduler>.  Любые параметры, задаваемые при создании фабрики задач, будут применяться ко всем созданным задачам, если задача <xref:System.Threading.Tasks.Task> не создана с помощью перечисления <xref:System.Threading.Tasks.TaskCreationOptions>. В этом случае параметры задачи переопределяют параметры фабрики задач.  
  
## Задачи без делегатов  
 В некоторых случаях может потребоваться использовать <xref:System.Threading.Tasks.Task> для инкапсуляции некоторой асинхронной операции, которая выполняется внешним компонентом, а не собственным пользовательским делегатом.  Если операция основана на шаблоне Begin\/End модели асинхронного программирования, можно использовать методы <xref:System.Threading.Tasks.TaskFactory.FromAsync%2A>.  В противном случае можно использовать объект <xref:System.Threading.Tasks.TaskCompletionSource%601> для заключения операции в задачу, чтобы получить некоторые преимущества программирования <xref:System.Threading.Tasks.Task>, например поддержку распространения исключений и продолжений.  Для получения дополнительной информации см. <xref:System.Threading.Tasks.TaskCompletionSource%601>.  
  
## Пользовательские планировщики  
 Большинство разработчиков приложений или библиотек не обращают внимания на то, на каком процессоре запускается задача, как она синхронизирует свою работу с другими задачами или как она планируется в <xref:System.Threading.ThreadPool?displayProperty=fullName>.  Им только требуется, чтобы она выполнялась максимально эффективно на главном компьютере.  Если требуется более точное управление сведениями планирования, библиотека параллельных задач позволяет настроить некоторые параметры в планировщике заданий по умолчанию и даже предоставить пользовательский планировщик.  Для получения дополнительной информации см. <xref:System.Threading.Tasks.TaskScheduler>.  
  
## Структуры связанных данных  
 Библиотека параллельных задач имеет несколько новых открытых типов, которые полезны в параллельных и последовательных сценариях.  Они включают несколько потокобезопасных, быстрых и масштабируемых классов коллекций в пространстве имен <xref:System.Collections.Concurrent?displayProperty=fullName> и несколько новых типов синхронизации, например <xref:System.Threading.Semaphore?displayProperty=fullName> и <xref:System.Threading.ManualResetEventSlim?displayProperty=fullName>, которые более эффективны, чем их предшественники для определенных типов рабочих нагрузок.  Другие новые типы в .NET Framework 4, например <xref:System.Threading.Barrier?displayProperty=fullName> и <xref:System.Threading.SpinLock?displayProperty=fullName>, предоставляют функциональные возможности, которые не были доступны в более ранних выпусках.  Дополнительные сведения см. в разделе [Data Structures for Parallel Programming](../../../docs/standard/parallel-programming/data-structures-for-parallel-programming.md).  
  
## Настраиваемые типы задач  
 Наследование из <xref:System.Threading.Tasks.Task?displayProperty=fullName> или <xref:System.Threading.Tasks.Task%601?displayProperty=fullName> не рекомендуется.  Вместо этого рекомендуется с помощью свойства <xref:System.Threading.Tasks.Task.AsyncState%2A> связать дополнительные данные или состояние с объектом <xref:System.Threading.Tasks.Task> или <xref:System.Threading.Tasks.Task%601>.  Можно также использовать методы расширения для расширения функциональных возможностей классов <xref:System.Threading.Tasks.Task> и <xref:System.Threading.Tasks.Task%601>.  Дополнительные сведения о методах расширения см. в разделах [Методы расширения](../Topic/Extension%20Methods%20\(C%23%20Programming%20Guide\).md) и [Методы расширения](../Topic/Extension%20Methods%20\(Visual%20Basic\).md).  
  
 Если необходимо наследовать от <xref:System.Threading.Tasks.Task> или <xref:System.Threading.Tasks.Task%601>, классы <xref:System.Threading.Tasks.Task.Run%2A>, <xref:System.Threading.Tasks.Task.Run%2A>, <xref:System.Threading.Tasks.TaskFactory?displayProperty=fullName>, <xref:System.Threading.Tasks.TaskFactory%601?displayProperty=fullName> и <xref:System.Threading.Tasks.TaskCompletionSource%601?displayProperty=fullName> нельзя использовать для создания экземпляров настраиваемого типа задач, поскольку эти механизмы создают только объекты <xref:System.Threading.Tasks.Task> и <xref:System.Threading.Tasks.Task%601>.  Кроме того, механизмы продолжения задачи, работу которых обеспечивают <xref:System.Threading.Tasks.Task>, <xref:System.Threading.Tasks.Task%601>, <xref:System.Threading.Tasks.TaskFactory> и <xref:System.Threading.Tasks.TaskFactory%601>, нельзя использовать для создания экземпляров настраиваемого типа задач, поскольку эти механизмы также создают только объекты <xref:System.Threading.Tasks.Task> и <xref:System.Threading.Tasks.Task%601>.  
  
## См. также  
  
|||  
|-|-|  
|Заголовок|Описание|  
|[Chaining Tasks by Using Continuation Tasks](../../../docs/standard/parallel-programming/chaining-tasks-by-using-continuation-tasks.md)|Описание работы продолжений.|  
|[Attached and Detached Child Tasks](../../../docs/standard/parallel-programming/attached-and-detached-child-tasks.md)|Описание различий между присоединенными и отсоединенными дочерними задачами.|  
|[Task Cancellation](../../../docs/standard/parallel-programming/task-cancellation.md)|Описание поддержки отмены, встроенной в объект <xref:System.Threading.Tasks.Task>.|  
|[Обработка исключений](../../../docs/standard/parallel-programming/exception-handling-task-parallel-library.md)|Описание обработки исключений в параллельных потоках.|  
|[How to: Use Parallel.Invoke to Execute Parallel Operations](../../../docs/standard/parallel-programming/how-to-use-parallel-invoke-to-execute-parallel-operations.md)|Описание использования <xref:System.Threading.Tasks.Parallel.Invoke%2A>.|  
|[How to: Return a Value from a Task](../../../docs/standard/parallel-programming/how-to-return-a-value-from-a-task.md)|Описание возврата значений из задач.|  
|[How to: Wait on One or More Tasks to Complete](../Topic/How%20to:%20Wait%20on%20One%20or%20More%20Tasks%20to%20Complete.md)|Описание ожидания завершения задач.|  
|[How to: Cancel a Task and Its Children](../../../docs/standard/parallel-programming/how-to-cancel-a-task-and-its-children.md)|Описание отмены задач.|  
|[Практическое руководство. Обработка исключений, создаваемых задачами](http://msdn.microsoft.com/ru-ru/d6c47ec8-9de9-4880-beb3-ff19ae51565d)|Описание обработки исключений, созданных задачами.|  
|[How to: Create Pre\-Computed Tasks](../../../docs/standard/parallel-programming/how-to-create-pre-computed-tasks.md)|Описание использования метода <xref:System.Threading.Tasks.Task.FromResult%2A?displayProperty=fullName> для получения результатов асинхронных операций загрузки, удерживаемых в кэше.|  
|[How to: Traverse a Binary Tree with Parallel Tasks](../../../docs/standard/parallel-programming/how-to-traverse-a-binary-tree-with-parallel-tasks.md)|Описание использования задач для прохождения двоичного дерева.|  
|[How to: Unwrap a Nested Task](../../../docs/standard/parallel-programming/how-to-unwrap-a-nested-task.md)|Демонстрация использования метода расширения <xref:System.Threading.Tasks.TaskExtensions.Unwrap%2A>.|  
|[Data Parallelism](../../../docs/standard/parallel-programming/data-parallelism-task-parallel-library.md)|Описывает способы использования методов <xref:System.Threading.Tasks.Parallel.For%2A> и <xref:System.Threading.Tasks.Parallel.ForEach%2A> для создания параллельных циклов для данных.|  
|[Parallel Programming](../../../docs/standard/parallel-programming/index.md)|Узел верхнего уровня для параллельного программирования в .NET Framework.|  
  
## См. также  
 [Parallel Programming](../../../docs/standard/parallel-programming/index.md)   
 [Примеры параллельного программирования в .NET Framework](http://code.msdn.microsoft.com/Samples-for-Parallel-b4b76364)
