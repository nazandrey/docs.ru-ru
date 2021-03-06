---
title: "Миграция приложения для магазина Windows в машинный код .NET"
ms.custom: 
ms.date: 03/30/2017
ms.prod: .net-framework
ms.reviewer: 
ms.suite: 
ms.technology:
- dotnet-clr
ms.tgt_pltfrm: 
ms.topic: article
ms.assetid: 4153aa18-6f56-4a0a-865b-d3da743a1d05
caps.latest.revision: 29
author: rpetrusha
ms.author: ronpet
manager: wpickett
ms.translationtype: HT
ms.sourcegitcommit: 306c608dc7f97594ef6f72ae0f5aaba596c936e1
ms.openlocfilehash: 55414d4479ca3cca8a7b5fbb15e4982b5dd12aad
ms.contentlocale: ru-ru
ms.lasthandoff: 08/21/2017

---
# <a name="migrating-your-windows-store-app-to-net-native"></a>Миграция приложения для магазина Windows в машинный код .NET
[!INCLUDE[net_native](../../../includes/net-native-md.md)] обеспечивает статическую компиляцию приложений в магазине Windows или на компьютере разработчика. В отличие от динамической компиляции, выполняемой JIT-компилятором для приложений магазина Windows или [Генератором машинных образов (Ngen.exe)](../../../docs/framework/tools/ngen-exe-native-image-generator.md) на устройстве. Несмотря на эти различия [!INCLUDE[net_native](../../../includes/net-native-md.md)] пытается поддерживать совместимость с [приложениями .NET для магазина Windows](http://msdn.microsoft.com/library/windows/apps/br230302.aspx). В большинстве случаев все, что работает в приложении .NET для магазина Windows, также работает с [!INCLUDE[net_native](../../../includes/net-native-md.md)].  Тем не менее в некоторых случаях могут произойти изменения поведения. В этом документе рассматриваются различия между стандартными приложениями .NET для магазина Windows и [!INCLUDE[net_native](../../../includes/net-native-md.md)] в следующих областях:  
  
-   [Общие различия среды выполнения](#Runtime)  
  
-   [Различия динамического программирования](#Dynamic)  
  
-   [Другие различия, связанным с отражением](#Reflection)  
  
-   [Неподдерживаемые сценарии и API-интерфейсы](#Unsupported)  
  
-   [Различия в Visual Studio](#VS)  
  
<a name="Runtime"></a>   
## <a name="general-runtime-differences"></a>Общие различия среды выполнения  
  
-   Исключения, например, <xref:System.TypeLoadException>, которые вызываются с помощью JIT-компилятора при запуске приложения в общеязыковой среде выполнения (CLR), обычно приводят к ошибкам времени компиляции, когда обрабатываются средством [!INCLUDE[net_native](../../../includes/net-native-md.md)].  
  
-   Не вызывайте метод <xref:System.GC.WaitForPendingFinalizers%2A?displayProperty=fullName> из потока пользовательского интерфейса приложения. Это может привести к взаимоблокировке в [!INCLUDE[net_native](../../../includes/net-native-md.md)].  
  
-   Не полагайтесь на порядок вызова конструктора статического класса. В [!INCLUDE[net_native](../../../includes/net-native-md.md)], порядок вызова отличается от порядка в стандартной среде выполнения. (Даже со стандартной средой выполнения, не следует рассчитывать на порядок выполнения конструкторов статических классов.)  
  
-   Бесконечный цикл без вызовов (например, `while(true);`) на любом потоке может привести к остановке приложения. Аналогичным образом, большие или бесконечные ожидания могут также привести приложение к остановке.  
  
-   Некоторые циклы универсальной инициализации не создают исключения в [!INCLUDE[net_native](../../../includes/net-native-md.md)]. Например, следующий код создает исключение <xref:System.TypeLoadException> исключений в стандартной среде CLR. В [!INCLUDE[net_native](../../../includes/net-native-md.md)], это не так.  
  
     [!code-csharp[ProjectN#8](../../../samples/snippets/csharp/VS_Snippets_CLR/projectn/cs/compat1.cs#8)]  
  
-   В некоторых случаях [!INCLUDE[net_native](../../../includes/net-native-md.md)] предоставляет различные реализации библиотеки классов платформы .NET Framework. Объект, возвращенный из метода, всегда будет реализовать члены возвращаемого типа. Тем не менее, поскольку его резервная реализация отличается, может оказаться невозможным привести его к тому же набору типов, как это можно было бы сделать на платформах .NET Framework. Например, в некоторых случаях может оказаться невозможным привести объект интерфейса <xref:System.Collections.Generic.IEnumerable%601> , возвращенный такими методами как <xref:System.Reflection.TypeInfo.DeclaredMembers%2A?displayProperty=fullName> или <xref:System.Reflection.TypeInfo.DeclaredProperties%2A?displayProperty=fullName> , к `T[]`.  
  
-   Кэш WinInet не включен по умолчанию на платформе .NET для приложений магазина Windows, но он включен на [!INCLUDE[net_native](../../../includes/net-native-md.md)]. Это повышает производительность, но сказывается на рабочем наборе. Не требуется никаких действий разработчика.  
  
<a name="Dynamic"></a>   
## <a name="dynamic-programming-differences"></a>Различия динамического программирования  
 [!INCLUDE[net_native](../../../includes/net-native-md.md)] статически связывает код из платформы .NET Framework, чтобы сделать код локальным для приложения с целью достижения максимальной производительности. Тем не менее, двоичные размеры должны оставаться небольшими, поэтому не удается подключить всю платформу .NET Framework. Компилятор [!INCLUDE[net_native](../../../includes/net-native-md.md)] разрешает это ограничение с помощью редуктора зависимостей, который удаляет ссылки на неиспользуемый код. Однако, [!INCLUDE[net_native](../../../includes/net-native-md.md)] не может поддерживать или создавать некоторые сведения о типе и код, если эти сведения не могут быть определены статически во время компиляции, а вместо этого извлекаются динамически во время выполнения.  
  
 [!INCLUDE[net_native](../../../includes/net-native-md.md)] включает отражение и динамическое программирование. При этом, не все типы могут быть помечены для отражения, так как это сделает размер созданного кода слишком большим (особенно потому, что поддерживается отражение на общедоступных API в платформе .NET Framework). Компилятор [!INCLUDE[net_native](../../../includes/net-native-md.md)] делает интеллектуальный выбор типов, которые должны поддерживать отражение, сохраняет метаданные и создает код только для этих типов.  
  
 Например, привязка данных требует, чтобы приложение обеспечивало сопоставление имен свойств с функциями. В приложениях .NET для магазина Windows среда CLR автоматически использует отражение для обеспечения этой возможности для управляемых и общедоступных собственных типов. В [!INCLUDE[net_native](../../../includes/net-native-md.md)], компилятор автоматически включает метаданные для типов, к которым осуществляется привязка данных.  
  
 Компилятор [!INCLUDE[net_native](../../../includes/net-native-md.md)] может также обрабатывать часто используемые универсальные типы, такие как <xref:System.Collections.Generic.List%601> и <xref:System.Collections.Generic.Dictionary%602>, которые работают, не требуя подсказок или директив. Ключевое слово [dynamic](~/docs/csharp/language-reference/keywords/dynamic.md) также поддерживается в заданных пределах.  
  
> [!NOTE]
>  Следует тщательно протестировать все пути динамического кода при переносе приложения в [!INCLUDE[net_native](../../../includes/net-native-md.md)].  
  
 Конфигурация по умолчанию для [!INCLUDE[net_native](../../../includes/net-native-md.md)] достаточна для большинства разработчиков, но некоторым разработчикам может потребоваться точная настройка своих конфигураций с помощью файла директив среды выполнения (. rd.xml) файла. Кроме того, в некоторых случаях компилятору [!INCLUDE[net_native](../../../includes/net-native-md.md)] не удается определить, какие метаданные должны быть доступны для отражения, и он опирается на подсказки, в особенности в следующих случаях:  
  
-   Некоторые конструкции, такие как <xref:System.Type.MakeGenericType%2A?displayProperty=fullName> и <xref:System.Reflection.MethodInfo.MakeGenericMethod%2A?displayProperty=fullName> не удается определить статически.  
  
-   Поскольку компилятор не может определить экземпляры, универсальный тип, который требуется отразить на нем должен быть указан директивами среды выполнения. Это необходимо не только потому, что весь код должен быть включен, но так же и вследствие того, что отражение на универсальных типах могут образовывать бесконечный цикл (например, при вызове универсального метода для универсального типа).  
  
> [!NOTE]
>  Директивы среды выполнения определяются в файле директив среды выполнения (. rd.xml). Общие сведения об использовании этого файла см. в разделе [Приступая к работе](../../../docs/framework/net-native/getting-started-with-net-native.md). Сведения о директивах среды выполнения см. в разделе [Runtime Directives (rd.xml) Configuration File Reference](../../../docs/framework/net-native/runtime-directives-rd-xml-configuration-file-reference.md).  
  
 [!INCLUDE[net_native](../../../includes/net-native-md.md)] также включает средства профилирования, помогающие определить, какие типы, не входящие в набор по умолчанию, должны поддерживать отражения.  
  
<a name="Reflection"></a>   
## <a name="other-reflection-related-differences"></a>Другие различия, связанным с отражением  
 Существует ряд других отдельных, связанных с отражением различий в поведении приложения .NET для магазина Windows и [!INCLUDE[net_native](../../../includes/net-native-md.md)].  
  
 В [!INCLUDE[net_native](../../../includes/net-native-md.md)]:  
  
-   Отражение закрытых типов и членов в библиотеке классов платформы .NET Framework не поддерживается. Тем не менее, можно выполнить отражение на собственных закрытых типах и членах, а также типах и членах библиотек сторонних поставщиков.  
  
-   Свойство <xref:System.Reflection.ParameterInfo.HasDefaultValue%2A?displayProperty=fullName> корректно возвращает `false` для объекта <xref:System.Reflection.ParameterInfo> , который представляет возвращаемое значение. В приложениях .NET для магазина Windows, будет возвращено значение `true`. Промежуточный язык (IL) не поддерживает это непосредственно, и интерпретация выполняется языком.  
  
-   Открытые члены на структурах <xref:System.RuntimeFieldHandle> и <xref:System.RuntimeMethodHandle> не поддерживаются. Эти типы поддерживаются только для LINQ, деревьев выражений и инициализации статического массива.  
  
-   <xref:System.Reflection.RuntimeReflectionExtensions.GetRuntimeProperties%2A?displayProperty=fullName> и <xref:System.Reflection.RuntimeReflectionExtensions.GetRuntimeEvents%2A?displayProperty=fullName> включают скрытые члены в базовых классах и поэтому могут переопределяться без явного переопределения. Это также справедливо для других методов [RuntimeReflectionExtensions.GetRuntime*](http://msdn.microsoft.com/library/system.reflection.runtimereflectionextensions_methods.aspx) .  
  
-   <xref:System.Type.MakeArrayType%2A?displayProperty=fullName> и <xref:System.Type.MakeByRefType%2A?displayProperty=fullName> не дают сбой при попытке создать определенные комбинации (например, массив byrefs).  
  
-   Нельзя использовать отражение для вызова членов, которые содержат параметры указателя.  
  
-   Чтобы получить или задать поле указателя, нельзя использовать отражение.  
  
-   Если неверное количество аргументов и тип одного из аргументов неверен, [!INCLUDE[net_native](../../../includes/net-native-md.md)] вызывает <xref:System.ArgumentException> вместо <xref:System.Reflection.TargetParameterCountException>.  
  
-   Обычно двоичная сериализация исключений не поддерживается. В результате несериализуемые объекты могут быть добавлены к словарю <xref:System.Exception.Data%2A?displayProperty=fullName> .  
  
<a name="Unsupported"></a>   
## <a name="unsupported-scenarios-and-apis"></a>Неподдерживаемые сценарии и API-интерфейсы  
 В следующих разделах перечислены неподдерживаемые сценарии и интерфейсы API для общей разработки, взаимодействия и таких технологий, как HTTPClient и Windows Communication Foundation (WCF):  
  
-   [Общая разработка](#General)  
  
-   [HttpClient](#HttpClient)  
  
-   [Interop](#Interop)  
  
-   [Неподдерживаемые API](#APIs)  
  
<a name="General"></a>   
### <a name="general-development-differences"></a>Различия общей разработки  
 **Типы значений**  
  
-   При переопределении методов <xref:System.ValueType.Equals%2A?displayProperty=fullName> и <xref:System.ValueType.GetHashCode%2A?displayProperty=fullName> для типа значения не вызывайте реализации базового класса. В приложениях .NET для магазина Windows эти методы основаны на отражении. Во время компиляции [!INCLUDE[net_native](../../../includes/net-native-md.md)] генерирует реализацию, которая не зависит от отражения среды выполнения. Это означает, что если не переопределить эти два метода, они будут работать, как ожидалось, поскольку [!INCLUDE[net_native](../../../includes/net-native-md.md)] создает реализацию во время компиляции. Однако, переопределение этих методов с помощью вызова реализации базового класса приводит к возникновению исключения.  
  
-   Типы значений больше одного мегабайта не поддерживаются.  
  
-   Типы значений не могут иметь конструктор по умолчанию в [!INCLUDE[net_native](../../../includes/net-native-md.md)]. (C# и Visual Basic запрещают конструкторы по умолчанию для типов значений. Тем не менее их можно создать в IL.)  
  
 **Массивы**  
  
-   Массивы с нижней границей, отличной от нуля, не поддерживаются. Как правило, эти массивы создаются путем вызова перегрузки <xref:System.Array.CreateInstance%28System.Type%2CSystem.Int32%5B%5D%2CSystem.Int32%5B%5D%29?displayProperty=fullName> .  
  
-   Динамическое создание многомерных массивов не поддерживается. Такие массивы обычно создаются путем вызова перегрузки метода <xref:System.Array.CreateInstance%2A?displayProperty=fullName> , который включает в себя параметр `lengths` , или же путем вызова метода <xref:System.Type.MakeArrayType%28System.Int32%29?displayProperty=fullName> .  
  
-   Многомерные массивы, имеющие четыре или более измерений не поддерживаются; т.е. их значение свойства <xref:System.Array.Rank%2A?displayProperty=fullName> равно или больше четырех. Вместо этого используйте [ступенчатые массивы](~/docs/csharp/programming-guide/arrays/jagged-arrays.md) (массива массивов). Например `array[x,y,z]` является недопустимым, но `array[x][y][z]` нет.  
  
-   Вариативность для многомерных массивов не поддерживается и вызывает исключение <xref:System.InvalidCastException> во время выполнения.  
  
 **Универсальные шаблоны**  
  
-   Расширения бесконечного универсального типа приводят к ошибке компилятора. Например, этот код вызывает ошибку при компиляции:  
  
     [!code-csharp[ProjectN#9](../../../samples/snippets/csharp/VS_Snippets_CLR/projectn/cs/compat2.cs#9)]  
  
 **Pointers**  
  
-   Массивы указателей не поддерживается.  
  
-   Чтобы получить или задать поле указателя, нельзя использовать отражение.  
  
 **Сериализация**  
  
 Атрибут <xref:System.Runtime.Serialization.KnownTypeAttribute.%23ctor%28System.String%29> не поддерживается. Вместо этого используйте атрибут <xref:System.Runtime.Serialization.KnownTypeAttribute.%23ctor%28System.Type%29> .  
  
 **Ресурсы**  
  
 Использование локализованных ресурсов с классом <xref:System.Diagnostics.Tracing.EventSource> не поддерживается. Свойство <xref:System.Diagnostics.Tracing.EventSourceAttribute.LocalizationResources%2A?displayProperty=fullName> не определяет локализованные ресурсы.  
  
 **Делегаты**  
  
 `Delegate.BeginInvoke` и `Delegate.EndInvoke` не поддерживаются.  
  
 **Async**  
  
 Потоковая логика в перегрузках задач Task IAsync не поддерживается.  
  
 **Различные интерфейсы API**  
  
-   Свойство <xref:System.Reflection.TypeInfo.GUID%2A?displayProperty=fullName> вызывает исключение <xref:System.PlatformNotSupportedException> , если атрибут <xref:System.Runtime.InteropServices.GuidAttribute> не применяется к типу. Идентификатор GUID используется в основном для поддержки модели COM.  
  
-   Метод <xref:System.DateTime.Parse%2A?displayProperty=fullName> правильно выполняет синтаксический анализ строк, содержащих даты в коротком формате в [!INCLUDE[net_native](../../../includes/net-native-md.md)]. Тем не менее он не поддерживает совместимость с синтаксическим анализом изменений даты и времени, описанным в статьях базы знаний Microsoft: [KB2803771](http://support.microsoft.com/kb/2803771) и [KB2803755](http://support.microsoft.com/kb/2803755).  
  
-   <xref:System.Numerics.BigInteger.ToString%2A?displayProperty=fullName> `("E")` правильно округляется в [!INCLUDE[net_native](../../../includes/net-native-md.md)]. В некоторых версиях среды CLR, результирующая строка усекается вместо округления.  
  
<a name="HttpClient"></a>   
### <a name="httpclient-differences"></a>Различия HttpClient  
 В [!INCLUDE[net_native](../../../includes/net-native-md.md)]класс <xref:System.Net.Http.HttpClientHandler> внутренним образом использует WinINet (через класс [HttpBaseProtocolFilter](http://msdn.microsoft.com/library/windows/apps/windows.web.http.filters.httpbaseprotocolfilter.aspx) вместо классов <xref:System.Net.WebRequest> и <xref:System.Net.WebResponse> , используемых в стандартных приложениях .NET для магазина Windows.  WinINet не поддерживает все параметры конфигурации, которые поддерживает класс <xref:System.Net.Http.HttpClientHandler> .  Это приводит к следующим результатам.  
  
-   Некоторые свойства возможности на <xref:System.Net.Http.HttpClientHandler> возвращают `false` на [!INCLUDE[net_native](../../../includes/net-native-md.md)], в то время как они возвращают `true` в стандартных приложениях .NET для магазина Windows.  
  
-   Некоторые методы доступа к свойству конфигурации `get` всегда возвращают фиксированное значение на [!INCLUDE[net_native](../../../includes/net-native-md.md)] , которое отличается от настраиваемого значения по умолчанию в приложениях .NET для магазина Windows.  
  
 В следующих подразделах описаны некоторые дополнительные различия в поведении.  
  
 **Прокси-сервер**  
  
 Класс [HttpBaseProtocolFilter](http://msdn.microsoft.com/library/windows/apps/windows.web.http.filters.httpbaseprotocolfilter.aspx) не поддерживает настройку или переопределение прокси для каждого запроса.  Это означает, что все запросы на [!INCLUDE[net_native](../../../includes/net-native-md.md)] используют системные настройки прокси-сервера или прокси-сервер, в зависимости от значения свойства <xref:System.Net.Http.HttpClientHandler.UseProxy%2A?displayProperty=fullName> .  В приложениях .NET для магазина Windows, прокси-сервер определяется свойством <xref:System.Net.Http.HttpClientHandler.Proxy%2A?displayProperty=fullName> .  В [!INCLUDE[net_native](../../../includes/net-native-md.md)]настройка <xref:System.Net.Http.HttpClientHandler.Proxy%2A?displayProperty=fullName> на значение, отличное от `null` , вызывает исключение <xref:System.PlatformNotSupportedException> .  Свойство <xref:System.Net.Http.HttpClientHandler.SupportsProxy%2A?displayProperty=fullName> возвращает `false` на [!INCLUDE[net_native](../../../includes/net-native-md.md)], в то время как оно возвращает `true` в стандартных приложениях .NET Framework для магазина Windows.  
  
 **Автоматическое перенаправление**  
  
 Класс [HttpBaseProtocolFilter](http://msdn.microsoft.com/library/windows/apps/windows.web.http.filters.httpbaseprotocolfilter.aspx) не разрешает настройку максимального числа автоматических перенаправлений.  Значение свойства <xref:System.Net.Http.HttpClientHandler.MaxAutomaticRedirections%2A?displayProperty=fullName> равно 50 по умолчанию в стандартных приложениях .NET для магазина Windows и может быть изменено. На [!INCLUDE[net_native](../../../includes/net-native-md.md)], значение этого свойства равно 10 и попытка его изменить вызывает исключение <xref:System.PlatformNotSupportedException> .  Свойство <xref:System.Net.Http.HttpClientHandler.SupportsRedirectConfiguration%2A?displayProperty=fullName> возвращает `false` на [!INCLUDE[net_native](../../../includes/net-native-md.md)], в то время как оно возвращает `true` в приложениях .NET для магазина Windows.  
  
 **Автоматическая распаковка**  
  
 Приложения .NET для магазина Windows позволяют задать свойство <xref:System.Net.Http.HttpClientHandler.AutomaticDecompression%2A?displayProperty=fullName> на <xref:System.Net.DecompressionMethods.Deflate>, <xref:System.Net.DecompressionMethods.GZip>, оба <xref:System.Net.DecompressionMethods.Deflate> и <xref:System.Net.DecompressionMethods.GZip>или <xref:System.Net.DecompressionMethods.None>.  [!INCLUDE[net_native](../../../includes/net-native-md.md)] поддерживает <xref:System.Net.DecompressionMethods.Deflate> только вместе с <xref:System.Net.DecompressionMethods.GZip>или <xref:System.Net.DecompressionMethods.None>.  При попытке задать свойство <xref:System.Net.Http.HttpClientHandler.AutomaticDecompression%2A> только на <xref:System.Net.DecompressionMethods.Deflate> или <xref:System.Net.DecompressionMethods.GZip> происходит его автоматическое задание на оба <xref:System.Net.DecompressionMethods.Deflate> и <xref:System.Net.DecompressionMethods.GZip>.  
  
 **Файлы cookie**  
  
 Обработка файлов cookie выполняется одновременно с <xref:System.Net.Http.HttpClient> и WinINet.  Файлы cookie из <xref:System.Net.CookieContainer> объединяются вместе файла cookie в кэше WinINet cookie.  Удаление файла cookie из <xref:System.Net.CookieContainer> запрещает <xref:System.Net.Http.HttpClient> отправлять файл cookie, но если файл cookie уже был просмотрен WinINet и файлы "cookie" не были удалены пользователем, WinINet отправляет его.  Не существует средств программного удаления файла cookie из WinINet с использованием API <xref:System.Net.Http.HttpClient>, <xref:System.Net.Http.HttpClientHandler>или <xref:System.Net.CookieContainer> .  Задание свойства <xref:System.Net.Http.HttpClientHandler.UseCookies%2A?displayProperty=fullName> на `false` вызывает только <xref:System.Net.Http.HttpClient> , чтобы остановить отправку файлов "cookie"; WinINet может по-прежнему включить свои файлы cookie в запрос.  
  
 **Учетные данные**  
  
 В приложениях .NET для магазина Windows свойства <xref:System.Net.Http.HttpClientHandler.UseDefaultCredentials%2A?displayProperty=fullName> и <xref:System.Net.Http.HttpClientHandler.Credentials%2A?displayProperty=fullName> работают независимо друг от друга.  Кроме того, свойство <xref:System.Net.Http.HttpClientHandler.Credentials%2A> принимает любой объект, реализующий интерфейс <xref:System.Net.ICredentials> .  В [!INCLUDE[net_native](../../../includes/net-native-md.md)]задание свойства <xref:System.Net.Http.HttpClientHandler.UseDefaultCredentials%2A> на `true` устанавливает свойство <xref:System.Net.Http.HttpClientHandler.Credentials%2A> в `null`.  Кроме этого, свойство <xref:System.Net.Http.HttpClientHandler.Credentials%2A> может быть задано только в `null`, <xref:System.Net.CredentialCache.DefaultCredentials%2A>или объект типа <xref:System.Net.NetworkCredential>.  Назначение любого другого объекта <xref:System.Net.ICredentials> , наиболее популярный из которых <xref:System.Net.CredentialCache>, свойству <xref:System.Net.Http.HttpClientHandler.Credentials%2A> вызывает исключение <xref:System.PlatformNotSupportedException>.  
  
 **Другие неподдерживаемые и ненастраиваемые функции**  
  
 В [!INCLUDE[net_native](../../../includes/net-native-md.md)]:  
  
-   Значение свойства <xref:System.Net.Http.HttpClientHandler.ClientCertificateOptions%2A?displayProperty=fullName> всегда <xref:System.Net.Http.ClientCertificateOption.Automatic>.  В приложения .NET для магазина Windows, по умолчанию используется <xref:System.Net.Http.ClientCertificateOption.Manual>.  
  
-   Свойство <xref:System.Net.Http.HttpClientHandler.MaxRequestContentBufferSize%2A?displayProperty=fullName> не настраивается.  
  
-   Свойство <xref:System.Net.Http.HttpClientHandler.PreAuthenticate%2A?displayProperty=fullName> всегда имеет значение `true`.  В приложения .NET для магазина Windows, по умолчанию используется `false`.  
  
-   Заголовок `SetCookie2` в ответах игнорируется как устаревший.  
  
<a name="Interop"></a>   
### <a name="interop-differences"></a>Различия взаимодействия  
 **Устаревшие интерфейсы API**  
  
 Не рекомендуется использовать несколько редко применяемых API-интерфейсов для взаимодействия с управляемым кодом. При использовании с [!INCLUDE[net_native](../../../includes/net-native-md.md)]эти API-интерфейсы могут вызывать исключение <xref:System.NotImplementedException> или <xref:System.PlatformNotSupportedException> или приводят к ошибке компилятора. В приложениях .NET для магазина Windows эти API-интерфейсы отмечены как устаревшие, хотя их вызов создает предупреждение компилятора, а не ошибку компилятора.  
  
 Устаревшие API-интерфейсы для маршалинга `VARIANT` :  
  
||  
|-|  
|<xref:System.Runtime.InteropServices.BStrWrapper?displayProperty=fullName>|  
|<xref:System.Runtime.InteropServices.CurrencyWrapper?displayProperty=fullName>|  
|<xref:System.Runtime.InteropServices.DispatchWrapper?displayProperty=fullName>|  
|<xref:System.Runtime.InteropServices.ErrorWrapper?displayProperty=fullName>|  
|<xref:System.Runtime.InteropServices.UnknownWrapper?displayProperty=fullName>|  
|<xref:System.Runtime.InteropServices.VariantWrapper?displayProperty=fullName>|  
|<xref:System.Runtime.InteropServices.UnmanagedType.IDispatch?displayProperty=fullName>|  
|<xref:System.Runtime.InteropServices.UnmanagedType.SafeArray?displayProperty=fullName>|  
|<xref:System.Runtime.InteropServices.VarEnum?displayProperty=fullName>|  
  
 <xref:System.Runtime.InteropServices.UnmanagedType.Struct?displayProperty=fullName> поддерживается, но создает исключение в некоторых сценариях, например когда используется с вариантами [IDispatch](http://msdn.microsoft.com/library/windows/apps/ms221608.aspx) или byref.  
  
 Устаревшие интерфейсы API для поддержки [IDispatch](http://msdn.microsoft.com/library/windows/apps/ms221608.aspx) :  
  
|Тип|Член|  
|----------|------------|  
|<xref:System.Runtime.InteropServices.ClassInterfaceType?displayProperty=fullName>|<xref:System.Runtime.InteropServices.ClassInterfaceType.AutoDispatch>|  
|<xref:System.Runtime.InteropServices.ClassInterfaceType?displayProperty=fullName>|<xref:System.Runtime.InteropServices.ClassInterfaceType.AutoDual>|  
|<xref:System.Runtime.InteropServices.ComDefaultInterfaceAttribute?displayProperty=fullName>|Атрибут не поддерживается|  
  
 Устаревшие интерфейсы API для классических COM-событий:  
  
||  
|-|  
|<xref:System.Runtime.InteropServices.ComEventsHelper?displayProperty=fullName>|  
|<xref:System.Runtime.InteropServices.ComSourceInterfacesAttribute>|  
  
 Устаревшие интерфейсы API в интерфейсе <xref:System.Runtime.InteropServices.ICustomQueryInterface?displayProperty=fullName> , который не поддерживается в [!INCLUDE[net_native](../../../includes/net-native-md.md)]:  
  
|Тип|Член|  
|----------|------------|  
|<xref:System.Runtime.InteropServices.ICustomQueryInterface?displayProperty=fullName>|Все члены.|  
|<xref:System.Runtime.InteropServices.CustomQueryInterfaceMode?displayProperty=fullName>|Все члены.|  
|<xref:System.Runtime.InteropServices.CustomQueryInterfaceResult?displayProperty=fullName>|Все члены.|  
|<xref:System.Runtime.InteropServices.Marshal?displayProperty=fullName>|<xref:System.Runtime.InteropServices.Marshal.GetComInterfaceForObject%28System.Object%2CSystem.Type%2CSystem.Runtime.InteropServices.CustomQueryInterfaceMode%29?displayProperty=fullName>|  
  
 Другие неподдерживаемые функции взаимодействия:  
  
|Тип|Член|  
|----------|------------|  
|<xref:System.Runtime.InteropServices.ICustomAdapter?displayProperty=fullName>|Все члены.|  
|<xref:System.Runtime.InteropServices.SafeBuffer?displayProperty=fullName>|Все члены.|  
|<xref:System.Runtime.InteropServices.UnmanagedType?displayProperty=fullName>|<xref:System.Runtime.InteropServices.UnmanagedType.Currency>|  
|<xref:System.Runtime.InteropServices.UnmanagedType?displayProperty=fullName>|<xref:System.Runtime.InteropServices.UnmanagedType.VBByRefStr>|  
|<xref:System.Runtime.InteropServices.UnmanagedType?displayProperty=fullName>|<xref:System.Runtime.InteropServices.UnmanagedType.AnsiBStr>|  
|<xref:System.Runtime.InteropServices.UnmanagedType?displayProperty=fullName>|<xref:System.Runtime.InteropServices.UnmanagedType.AsAny>|  
|<xref:System.Runtime.InteropServices.UnmanagedType?displayProperty=fullName>|<xref:System.Runtime.InteropServices.UnmanagedType.CustomMarshaler>|  
  
 Редко используемые интерфейсы API маршалинга:  
  
|Тип|Член|  
|----------|------------|  
|<xref:System.Runtime.InteropServices.Marshal?displayProperty=fullName>|<xref:System.Runtime.InteropServices.Marshal.ReadByte%28System.Object%2CSystem.Int32%29>|  
|<xref:System.Runtime.InteropServices.Marshal?displayProperty=fullName>|<xref:System.Runtime.InteropServices.Marshal.ReadInt16%28System.Object%2CSystem.Int32%29>|  
|<xref:System.Runtime.InteropServices.Marshal?displayProperty=fullName>|<xref:System.Runtime.InteropServices.Marshal.ReadInt32%28System.Object%2CSystem.Int32%29>|  
|<xref:System.Runtime.InteropServices.Marshal?displayProperty=fullName>|<xref:System.Runtime.InteropServices.Marshal.ReadInt64%28System.Object%2CSystem.Int32%29>|  
|<xref:System.Runtime.InteropServices.Marshal?displayProperty=fullName>|<xref:System.Runtime.InteropServices.Marshal.ReadIntPtr%28System.Object%2CSystem.Int32%29>|  
|<xref:System.Runtime.InteropServices.Marshal?displayProperty=fullName>|<xref:System.Runtime.InteropServices.Marshal.WriteByte%28System.Object%2CSystem.Int32%2CSystem.Byte%29>|  
|<xref:System.Runtime.InteropServices.Marshal?displayProperty=fullName>|<xref:System.Runtime.InteropServices.Marshal.WriteInt16%28System.Object%2CSystem.Int32%2CSystem.Int16%29>|  
|<xref:System.Runtime.InteropServices.Marshal?displayProperty=fullName>|<xref:System.Runtime.InteropServices.Marshal.WriteInt32%28System.Object%2CSystem.Int32%2CSystem.Int32%29>|  
|<xref:System.Runtime.InteropServices.Marshal?displayProperty=fullName>|<xref:System.Runtime.InteropServices.Marshal.WriteInt64%28System.Object%2CSystem.Int32%2CSystem.Int64%29>|  
|<xref:System.Runtime.InteropServices.Marshal?displayProperty=fullName>|<xref:System.Runtime.InteropServices.Marshal.WriteIntPtr%28System.Object%2CSystem.Int32%2CSystem.IntPtr%29>|  
  
 **Вызов неуправляемого кода и совместимость взаимодействия COM**  
  
 Большинство вызовов неуправляемого кода и сценариев COM-взаимодействия по-прежнему поддерживается в [!INCLUDE[net_native](../../../includes/net-native-md.md)]. В частности, поддерживаются все взаимодействия с API среды выполнения Windows (WinRT) и весь необходимый маршалинг для среды выполнения Windows. Это включает поддержку маршалинга:  
  
-   Массивы (включая <xref:System.Runtime.InteropServices.UnmanagedType.ByValArray?displayProperty=fullName>)  
  
-   `BStr`  
  
-   Делегаты  
  
-   Строки (Юникод, Ansi и HSTRING)  
  
-   Структуры (`byref` и `byval`)  
  
-   Объединения  
  
-   Дескрипторы Win32  
  
-   Все конструкции WinRT  
  
-   Частичная поддержка маршалинга типов variant. Поддерживаются следующие функции:  
  
    -   <xref:System.Boolean>  
  
    -   <xref:System.Byte>  
  
    -   <xref:System.Decimal>  
  
    -   <xref:System.Double>  
  
    -   <xref:System.Int16>  
  
    -   <xref:System.Int32>  
  
    -   <xref:System.Int64>  
  
    -   <xref:System.SByte>  
  
    -   <xref:System.Single>  
  
    -   <xref:System.UInt16>  
  
    -   <xref:System.UInt32>  
  
    -   <xref:System.UInt64>  
  
    -   `BStr`  
  
    -   [IUnknown](http://msdn.microsoft.com/library/windows/desktop/ms680509.aspx)  
  
 Однако, [!INCLUDE[net_native](../../../includes/net-native-md.md)] не поддерживается следующее:  
  
-   использование классических COM-событий  
  
-   Реализация интерфейса <xref:System.Runtime.InteropServices.ICustomQueryInterface?displayProperty=fullName> на управляемом типе  
  
-   Реализация интерфейса [IDispatch](http://msdn.microsoft.com/library/windows/apps/ms221608.aspx) в управляемом типе через атрибут <xref:System.Runtime.InteropServices.ComDefaultInterfaceAttribute?displayProperty=fullName> . Однако, обратите внимание, что нельзя вызывать COM-объекты через `IDispatch`, а управляемый объект не может реализовать `IDispatch`.  
  
 Использование отражения для вызова метода неуправляемого кода не поддерживается. Это ограничение можно обойти путем заключения вызова метода в другой метод, вместо этого используя вызов оболочки.  
  
<a name="APIs"></a>   
### <a name="other-differences-from-net-apis-for-windows-store-apps"></a>Другие отличия от API-интерфейсов приложений .NET для магазина Windows  
 В этом разделе перечислены остальные интерфейсы API, которые не поддерживаются в [!INCLUDE[net_native](../../../includes/net-native-md.md)]. Самый большой набор неподдерживаемых интерфейсов API — это API-интерфейсы Windows Communication Foundation (WCF).  
  
 **DataAnnotations (System.ComponentModel.DataAnnotations)**  
  
 Типы в пространствах имен <xref:System.ComponentModel.DataAnnotations> и <xref:System.ComponentModel.DataAnnotations.Schema> не поддерживаются в [!INCLUDE[net_native](../../../includes/net-native-md.md)]. К ним относятся следующие типы, которые присутствуют в приложениях .NET для магазина Windows для Windows 8:  
  
||  
|-|  
|<xref:System.ComponentModel.DataAnnotations.AssociationAttribute?displayProperty=fullName>|  
|<xref:System.ComponentModel.DataAnnotations.ConcurrencyCheckAttribute?displayProperty=fullName>|  
|<xref:System.ComponentModel.DataAnnotations.CustomValidationAttribute?displayProperty=fullName>|  
|<xref:System.ComponentModel.DataAnnotations.DataType?displayProperty=fullName>|  
|<xref:System.ComponentModel.DataAnnotations.DataTypeAttribute?displayProperty=fullName>|  
|<xref:System.ComponentModel.DataAnnotations.DisplayAttribute?displayProperty=fullName>|  
|<xref:System.ComponentModel.DataAnnotations.DisplayColumnAttribute?displayProperty=fullName>|  
|<xref:System.ComponentModel.DataAnnotations.DisplayFormatAttribute>|  
|<xref:System.ComponentModel.DataAnnotations.EditableAttribute>|  
|<xref:System.ComponentModel.DataAnnotations.EnumDataTypeAttribute>|  
|<xref:System.ComponentModel.DataAnnotations.FilterUIHintAttribute?displayProperty=fullName>|  
|<xref:System.ComponentModel.DataAnnotations.KeyAttribute?displayProperty=fullName>|  
|<xref:System.ComponentModel.DataAnnotations.RangeAttribute?displayProperty=fullName>|  
|<xref:System.ComponentModel.DataAnnotations.RegularExpressionAttribute?displayProperty=fullName>|  
|<xref:System.ComponentModel.DataAnnotations.RequiredAttribute?displayProperty=fullName>|  
|<xref:System.ComponentModel.DataAnnotations.StringLengthAttribute?displayProperty=fullName>|  
|<xref:System.ComponentModel.DataAnnotations.TimestampAttribute?displayProperty=fullName>|  
|<xref:System.ComponentModel.DataAnnotations.UIHintAttribute?displayProperty=fullName>|  
|<xref:System.ComponentModel.DataAnnotations.ValidationAttribute?displayProperty=fullName>|  
|<xref:System.ComponentModel.DataAnnotations.ValidationContext?displayProperty=fullName>|  
|<xref:System.ComponentModel.DataAnnotations.ValidationException?displayProperty=fullName>|  
|<xref:System.ComponentModel.DataAnnotations.ValidationResult?displayProperty=fullName>|  
|<xref:System.ComponentModel.DataAnnotations.Validator?displayProperty=fullName>|  
|<xref:System.ComponentModel.DataAnnotations.Schema.DatabaseGeneratedAttribute?displayProperty=fullName>|  
|<xref:System.ComponentModel.DataAnnotations.Schema.DatabaseGeneratedOption?displayProperty=fullName>|  
  
 **Visual Basic**  
  
 Visual Basic не поддерживается в настоящее время в [!INCLUDE[net_native](../../../includes/net-native-md.md)]. Следующие типы в пространствах имен <xref:Microsoft.VisualBasic> и <xref:Microsoft.VisualBasic.CompilerServices> недоступны в [!INCLUDE[net_native](../../../includes/net-native-md.md)]:  
  
||  
|-|  
|<xref:Microsoft.VisualBasic.CallType?displayProperty=fullName>|  
|<xref:Microsoft.VisualBasic.Constants?displayProperty=fullName>|  
|<xref:Microsoft.VisualBasic.HideModuleNameAttribute?displayProperty=fullName>|  
|<xref:Microsoft.VisualBasic.Strings?displayProperty=fullName>|  
|<xref:Microsoft.VisualBasic.CompilerServices.Conversions?displayProperty=fullName>|  
|<xref:Microsoft.VisualBasic.CompilerServices.DesignerGeneratedAttribute?displayProperty=fullName>|  
|<xref:Microsoft.VisualBasic.CompilerServices.IncompleteInitialization?displayProperty=fullName>|  
|<xref:Microsoft.VisualBasic.CompilerServices.NewLateBinding?displayProperty=fullName>|  
|<xref:Microsoft.VisualBasic.CompilerServices.ObjectFlowControl?displayProperty=fullName>|  
|<xref:Microsoft.VisualBasic.CompilerServices.ObjectFlowControl.ForLoopControl?displayProperty=fullName>|  
|<xref:Microsoft.VisualBasic.CompilerServices.Operators?displayProperty=fullName>|  
|<xref:Microsoft.VisualBasic.CompilerServices.OptionCompareAttribute?displayProperty=fullName>|  
|<xref:Microsoft.VisualBasic.CompilerServices.OptionTextAttribute?displayProperty=fullName>|  
|<xref:Microsoft.VisualBasic.CompilerServices.ProjectData?displayProperty=fullName>|  
|<xref:Microsoft.VisualBasic.CompilerServices.StandardModuleAttribute?displayProperty=fullName>|  
|<xref:Microsoft.VisualBasic.CompilerServices.StaticLocalInitFlag?displayProperty=fullName>|  
|<xref:Microsoft.VisualBasic.CompilerServices.Utils?displayProperty=fullName>|  
  
 **Контекст отражения (пространство имен System.Reflection.Context)**  
  
 Класс <xref:System.Reflection.Context.CustomReflectionContext?displayProperty=fullName> не поддерживается в [!INCLUDE[net_native](../../../includes/net-native-md.md)].  
  
 **Часы реального времени (System.Net.Http.Rtc)**  
  
 Класс <xref:System.Net.Http.RtcRequestFactory?displayProperty=fullName> не поддерживается в [!INCLUDE[net_native](../../../includes/net-native-md.md)].  
  
 **Windows Communication Foundation (WCF) (System.ServiceModel.\*)**  
  
 Типы в [пространствах имен System.ServiceModel.*](http://msdn.microsoft.com/library/gg145010.aspx) не поддерживаются в [!INCLUDE[net_native](../../../includes/net-native-md.md)]. В число этих типов входят следующие:  
  
||  
|-|  
|<xref:System.ServiceModel.ActionNotSupportedException?displayProperty=fullName>|  
|<xref:System.ServiceModel.BasicHttpBinding?displayProperty=fullName>|  
|<xref:System.ServiceModel.BasicHttpMessageCredentialType?displayProperty=fullName>|  
|<xref:System.ServiceModel.BasicHttpSecurity?displayProperty=fullName>|  
|<xref:System.ServiceModel.BasicHttpSecurityMode?displayProperty=fullName>|  
|<xref:System.ServiceModel.CallbackBehaviorAttribute?displayProperty=fullName>|  
|<xref:System.ServiceModel.ChannelFactory?displayProperty=fullName>|  
|<xref:System.ServiceModel.ChannelFactory%601?displayProperty=fullName>|  
|<xref:System.ServiceModel.ClientBase%601?displayProperty=fullName>|  
|<xref:System.ServiceModel.ClientBase%601.BeginOperationDelegate?displayProperty=fullName>|  
|<xref:System.ServiceModel.ClientBase%601.ChannelBase%601?displayProperty=fullName>|  
|<xref:System.ServiceModel.ClientBase%601.EndOperationDelegate?displayProperty=fullName>|  
|<xref:System.ServiceModel.ClientBase%601.InvokeAsyncCompletedEventArgs?displayProperty=fullName>|  
|<xref:System.ServiceModel.CommunicationException?displayProperty=fullName>|  
|<xref:System.ServiceModel.CommunicationObjectAbortedException?displayProperty=fullName>|  
|<xref:System.ServiceModel.CommunicationObjectFaultedException?displayProperty=fullName>|  
|<xref:System.ServiceModel.CommunicationState?displayProperty=fullName>|  
|<xref:System.ServiceModel.DataContractFormatAttribute?displayProperty=fullName>|  
|<xref:System.ServiceModel.DnsEndpointIdentity?displayProperty=fullName>|  
|<xref:System.ServiceModel.DuplexChannelFactory%601?displayProperty=fullName>|  
|<xref:System.ServiceModel.DuplexClientBase%601?displayProperty=fullName>|  
|<xref:System.ServiceModel.EndpointAddress?displayProperty=fullName>|  
|<xref:System.ServiceModel.EndpointAddressBuilder?displayProperty=fullName>|  
|<xref:System.ServiceModel.EndpointIdentity?displayProperty=fullName>|  
|<xref:System.ServiceModel.EndpointNotFoundException?displayProperty=fullName>|  
|<xref:System.ServiceModel.EnvelopeVersion?displayProperty=fullName>|  
|<xref:System.ServiceModel.ExceptionDetail?displayProperty=fullName>|  
|<xref:System.ServiceModel.FaultCode?displayProperty=fullName>|  
|<xref:System.ServiceModel.FaultContractAttribute?displayProperty=fullName>|  
|<xref:System.ServiceModel.FaultException?displayProperty=fullName>|  
|<xref:System.ServiceModel.FaultException%601?displayProperty=fullName>|  
|<xref:System.ServiceModel.FaultReason?displayProperty=fullName>|  
|<xref:System.ServiceModel.FaultReasonText?displayProperty=fullName>|  
|<xref:System.ServiceModel.HttpBindingBase?displayProperty=fullName>|  
|<xref:System.ServiceModel.HttpClientCredentialType?displayProperty=fullName>|  
|<xref:System.ServiceModel.HttpTransportSecurity?displayProperty=fullName>|  
|<xref:System.ServiceModel.IClientChannel?displayProperty=fullName>|  
|<xref:System.ServiceModel.ICommunicationObject?displayProperty=fullName>|  
|<xref:System.ServiceModel.IContextChannel?displayProperty=fullName>|  
|<xref:System.ServiceModel.IDefaultCommunicationTimeouts?displayProperty=fullName>|  
|<xref:System.ServiceModel.IExtensibleObject%601?displayProperty=fullName>|  
|<xref:System.ServiceModel.IExtension%601?displayProperty=fullName>|  
|<xref:System.ServiceModel.IExtensionCollection%601?displayProperty=fullName>|  
|<xref:System.ServiceModel.InstanceContext?displayProperty=fullName>|  
|<xref:System.ServiceModel.InvalidMessageContractException?displayProperty=fullName>|  
|<xref:System.ServiceModel.MessageBodyMemberAttribute?displayProperty=fullName>|  
|<xref:System.ServiceModel.MessageContractAttribute?displayProperty=fullName>|  
|<xref:System.ServiceModel.MessageContractMemberAttribute?displayProperty=fullName>|  
|<xref:System.ServiceModel.MessageCredentialType?displayProperty=fullName>|  
|<xref:System.ServiceModel.MessageHeader%601?displayProperty=fullName>|  
|<xref:System.ServiceModel.MessageHeaderException?displayProperty=fullName>|  
|<xref:System.ServiceModel.MessageParameterAttribute?displayProperty=fullName>|  
|<xref:System.ServiceModel.MessageSecurityOverTcp?displayProperty=fullName>|  
|<xref:System.ServiceModel.MessageSecurityVersion?displayProperty=fullName>|  
|<xref:System.ServiceModel.NetHttpBinding?displayProperty=fullName>|  
|<xref:System.ServiceModel.NetHttpMessageEncoding?displayProperty=fullName>|  
|<xref:System.ServiceModel.NetTcpBinding?displayProperty=fullName>|  
|<xref:System.ServiceModel.NetTcpSecurity?displayProperty=fullName>|  
|<xref:System.ServiceModel.OperationContext?displayProperty=fullName>|  
|<xref:System.ServiceModel.OperationContextScope?displayProperty=fullName>|  
|<xref:System.ServiceModel.OperationContractAttribute?displayProperty=fullName>|  
|<xref:System.ServiceModel.OperationFormatStyle?displayProperty=fullName>|  
|<xref:System.ServiceModel.ProtocolException?displayProperty=fullName>|  
|<xref:System.ServiceModel.QuotaExceededException?displayProperty=fullName>|  
|<xref:System.ServiceModel.SecurityMode?displayProperty=fullName>|  
|<xref:System.ServiceModel.ServerTooBusyException?displayProperty=fullName>|  
|<xref:System.ServiceModel.ServiceActivationException?displayProperty=fullName>|  
|<xref:System.ServiceModel.ServiceContractAttribute?displayProperty=fullName>|  
|<xref:System.ServiceModel.ServiceKnownTypeAttribute?displayProperty=fullName>|  
|<xref:System.ServiceModel.SpnEndpointIdentity?displayProperty=fullName>|  
|<xref:System.ServiceModel.TcpClientCredentialType?displayProperty=fullName>|  
|<xref:System.ServiceModel.TcpTransportSecurity?displayProperty=fullName>|  
|<xref:System.ServiceModel.TransferMode?displayProperty=fullName>|  
|<xref:System.ServiceModel.UnknownMessageReceivedEventArgs?displayProperty=fullName>|  
|<xref:System.ServiceModel.UpnEndpointIdentity?displayProperty=fullName>|  
|<xref:System.ServiceModel.XmlSerializerFormatAttribute?displayProperty=fullName>|  
|<xref:System.ServiceModel.Channels.AddressHeader?displayProperty=fullName>|  
|<xref:System.ServiceModel.Channels.AddressHeaderCollection?displayProperty=fullName>|  
|<xref:System.ServiceModel.Channels.AddressingVersion?displayProperty=fullName>|  
|<xref:System.ServiceModel.Channels.BinaryMessageEncodingBindingElement?displayProperty=fullName>|  
|<xref:System.ServiceModel.Channels.ChannelManagerBase?displayProperty=fullName>|  
|<xref:System.ServiceModel.Channels.ChannelParameterCollection?displayProperty=fullName>|  
|<xref:System.ServiceModel.Channels.CommunicationObject?displayProperty=fullName>|  
|<xref:System.ServiceModel.Channels.CompressionFormat?displayProperty=fullName>|  
|<xref:System.ServiceModel.Channels.ConnectionOrientedTransportBindingElement?displayProperty=fullName>|  
|<xref:System.ServiceModel.Channels.CustomBinding?displayProperty=fullName>|  
|<xref:System.ServiceModel.Channels.FaultConverter?displayProperty=fullName>|  
|<xref:System.ServiceModel.Channels.HttpRequestMessageProperty?displayProperty=fullName>|  
|<xref:System.ServiceModel.Channels.HttpResponseMessageProperty?displayProperty=fullName>|  
|<xref:System.ServiceModel.Channels.HttpsTransportBindingElement?displayProperty=fullName>|  
|<xref:System.ServiceModel.Channels.HttpTransportBindingElement?displayProperty=fullName>|  
|<xref:System.ServiceModel.Channels.IChannel?displayProperty=fullName>|  
|<xref:System.ServiceModel.Channels.IChannelFactory?displayProperty=fullName>|  
|<xref:System.ServiceModel.Channels.IChannelFactory%601?displayProperty=fullName>|  
|<xref:System.ServiceModel.Channels.IDuplexChannel?displayProperty=fullName>|  
|<xref:System.ServiceModel.Channels.IDuplexSession?displayProperty=fullName>|  
|<xref:System.ServiceModel.Channels.IDuplexSessionChannel?displayProperty=fullName>|  
|<xref:System.ServiceModel.Channels.IHttpCookieContainerManager?displayProperty=fullName>|  
|<xref:System.ServiceModel.Channels.IInputChannel?displayProperty=fullName>|  
|<xref:System.ServiceModel.Channels.IInputSession?displayProperty=fullName>|  
|<xref:System.ServiceModel.Channels.IInputSessionChannel?displayProperty=fullName>|  
|<xref:System.ServiceModel.Channels.IMessageProperty?displayProperty=fullName>|  
|<xref:System.ServiceModel.Channels.IOutputChannel?displayProperty=fullName>|  
|<xref:System.ServiceModel.Channels.IOutputSession?displayProperty=fullName>|  
|<xref:System.ServiceModel.Channels.IOutputSessionChannel?displayProperty=fullName>|  
|<xref:System.ServiceModel.Channels.IRequestChannel?displayProperty=fullName>|  
|<xref:System.ServiceModel.Channels.IRequestSessionChannel?displayProperty=fullName>|  
|<xref:System.ServiceModel.Channels.ISession?displayProperty=fullName>|  
|<xref:System.ServiceModel.Channels.ISessionChannel%601?displayProperty=fullName>|  
|<xref:System.ServiceModel.Channels.LocalClientSecuritySettings?displayProperty=fullName>|  
|<xref:System.ServiceModel.Channels.Message?displayProperty=fullName>|  
|<xref:System.ServiceModel.Channels.MessageBuffer?displayProperty=fullName>|  
|<xref:System.ServiceModel.Channels.MessageEncoder?displayProperty=fullName>|  
|<xref:System.ServiceModel.Channels.MessageEncoderFactory?displayProperty=fullName>|  
|<xref:System.ServiceModel.Channels.MessageEncodingBindingElement?displayProperty=fullName>|  
|<xref:System.ServiceModel.Channels.MessageFault?displayProperty=fullName>|  
|<xref:System.ServiceModel.Channels.MessageHeader?displayProperty=fullName>|  
|<xref:System.ServiceModel.Channels.MessageHeaderInfo?displayProperty=fullName>|  
|<xref:System.ServiceModel.Channels.MessageHeaders?displayProperty=fullName>|  
|<xref:System.ServiceModel.Channels.MessageProperties?displayProperty=fullName>|  
|<xref:System.ServiceModel.Channels.MessageState?displayProperty=fullName>|  
|<xref:System.ServiceModel.Channels.MessageVersion?displayProperty=fullName>|  
|<xref:System.ServiceModel.Channels.RequestContext?displayProperty=fullName>|  
|<xref:System.ServiceModel.Channels.SecurityBindingElement?displayProperty=fullName>|  
|<xref:System.ServiceModel.Channels.SecurityHeaderLayout?displayProperty=fullName>|  
|<xref:System.ServiceModel.Channels.SslStreamSecurityBindingElement?displayProperty=fullName>|  
|<xref:System.ServiceModel.Channels.TcpConnectionPoolSettings?displayProperty=fullName>|  
|<xref:System.ServiceModel.Channels.TcpTransportBindingElement?displayProperty=fullName>|  
|<xref:System.ServiceModel.Channels.TextMessageEncodingBindingElement?displayProperty=fullName>|  
|<xref:System.ServiceModel.Channels.TransportBindingElement?displayProperty=fullName>|  
|<xref:System.ServiceModel.Channels.TransportSecurityBindingElement?displayProperty=fullName>|  
|<xref:System.ServiceModel.Channels.WebSocketTransportSettings?displayProperty=fullName>|  
|<xref:System.ServiceModel.Channels.WebSocketTransportUsage?displayProperty=fullName>|  
|<xref:System.ServiceModel.Channels.WindowsStreamSecurityBindingElement?displayProperty=fullName>|  
|<xref:System.ServiceModel.Description.ClientCredentials?displayProperty=fullName>|  
|<xref:System.ServiceModel.Description.ContractDescription?displayProperty=fullName>|  
|<xref:System.ServiceModel.Description.DataContractSerializerOperationBehavior?displayProperty=fullName>|  
|<xref:System.ServiceModel.Description.FaultDescription?displayProperty=fullName>|  
|<xref:System.ServiceModel.Description.FaultDescriptionCollection?displayProperty=fullName>|  
|<xref:System.ServiceModel.Description.IContractBehavior?displayProperty=fullName>|  
|<xref:System.ServiceModel.Description.IEndpointBehavior?displayProperty=fullName>|  
|<xref:System.ServiceModel.Description.IOperationBehavior?displayProperty=fullName>|  
|<xref:System.ServiceModel.Description.MessageBodyDescription?displayProperty=fullName>|  
|<xref:System.ServiceModel.Description.MessageDescription?displayProperty=fullName>|  
|<xref:System.ServiceModel.Description.MessageDescriptionCollection?displayProperty=fullName>|  
|<xref:System.ServiceModel.Description.MessageDirection?displayProperty=fullName>|  
|<xref:System.ServiceModel.Description.MessageHeaderDescription?displayProperty=fullName>|  
|<xref:System.ServiceModel.Description.MessageHeaderDescriptionCollection?displayProperty=fullName>|  
|<xref:System.ServiceModel.Description.MessagePartDescription?displayProperty=fullName>|  
|<xref:System.ServiceModel.Description.MessagePartDescriptionCollection?displayProperty=fullName>|  
|<xref:System.ServiceModel.Description.MessagePropertyDescription?displayProperty=fullName>|  
|<xref:System.ServiceModel.Description.MessagePropertyDescriptionCollection?displayProperty=fullName>|  
|<xref:System.ServiceModel.Description.OperationDescription?displayProperty=fullName>|  
|<xref:System.ServiceModel.Description.OperationDescriptionCollection?displayProperty=fullName>|  
|<xref:System.ServiceModel.Description.ServiceEndpoint?displayProperty=fullName>|  
|<xref:System.ServiceModel.Dispatcher.ClientOperation?displayProperty=fullName>|  
|<xref:System.ServiceModel.Dispatcher.ClientRuntime?displayProperty=fullName>|  
|<xref:System.ServiceModel.Dispatcher.DispatchOperation?displayProperty=fullName>|  
|<xref:System.ServiceModel.Dispatcher.DispatchRuntime?displayProperty=fullName>|  
|<xref:System.ServiceModel.Dispatcher.EndpointDispatcher?displayProperty=fullName>|  
|<xref:System.ServiceModel.Dispatcher.IClientMessageFormatter?displayProperty=fullName>|  
|<xref:System.ServiceModel.Dispatcher.IClientMessageInspector?displayProperty=fullName>|  
|<xref:System.ServiceModel.Dispatcher.IClientOperationSelector?displayProperty=fullName>|  
|<xref:System.ServiceModel.Dispatcher.IParameterInspector?displayProperty=fullName>|  
|<xref:System.ServiceModel.Security.BasicSecurityProfileVersion?displayProperty=fullName>|  
|<xref:System.ServiceModel.Security.HttpDigestClientCredential?displayProperty=fullName>|  
|<xref:System.ServiceModel.Security.MessageSecurityException?displayProperty=fullName>|  
|<xref:System.ServiceModel.Security.SecureConversationVersion?displayProperty=fullName>|  
|<xref:System.ServiceModel.Security.SecurityAccessDeniedException?displayProperty=fullName>|  
|<xref:System.ServiceModel.Security.SecurityPolicyVersion?displayProperty=fullName>|  
|<xref:System.ServiceModel.Security.SecurityVersion?displayProperty=fullName>|  
|<xref:System.ServiceModel.Security.TrustVersion?displayProperty=fullName>|  
|<xref:System.ServiceModel.Security.UserNamePasswordClientCredential?displayProperty=fullName>|  
|<xref:System.ServiceModel.Security.WindowsClientCredential?displayProperty=fullName>|  
|<xref:System.ServiceModel.Security.Tokens.SecureConversationSecurityTokenParameters?displayProperty=fullName>|  
|<xref:System.ServiceModel.Security.Tokens.SecurityTokenParameters?displayProperty=fullName>|  
|<xref:System.ServiceModel.Security.Tokens.SupportingTokenParameters?displayProperty=fullName>|  
|<xref:System.ServiceModel.Security.Tokens.UserNameSecurityTokenParameters?displayProperty=fullName>|  
  
### <a name="differences-in-serializers"></a>Различия в сериализаторах  
 Существуют следующие различия, касающиеся сериализации и десериализации с классами <xref:System.Runtime.Serialization.DataContractSerializer>, <xref:System.Runtime.Serialization.Json.DataContractJsonSerializer>и <xref:System.Xml.Serialization.XmlSerializer> :  
  
-   В [!INCLUDE[net_native](../../../includes/net-native-md.md)], <xref:System.Runtime.Serialization.DataContractSerializer> и <xref:System.Runtime.Serialization.Json.DataContractJsonSerializer> не удается сериализовать или десериализовать производный класс, имеющий член базового класса, тип которого не является корневым тип сериализации. Например, в следующем коде при сериализации или десериализации `Y` возникает ошибка:  
  
     [!code-csharp[ProjectN#10](../../../samples/snippets/csharp/VS_Snippets_CLR/projectn/cs/compat3.cs#10)]  
  
     Тип `InnerType` не известен сериализатору, так как члены базового класса не передаются во время сериализации.  
  
-   <xref:System.Runtime.Serialization.DataContractSerializer> и <xref:System.Runtime.Serialization.Json.DataContractJsonSerializer> не удается сериализовать класс или структуру, которая реализует интерфейс <xref:System.Collections.Generic.IEnumerable%601> . Например, не удается сериализовать или десериализовать следующие типы:  
  
  
  
-   <xref:System.Xml.Serialization.XmlSerializer> не удается сериализовать следующие значения объекта, так как он не знает точный тип объекта для сериализации:  
  
  
  
-   <xref:System.Xml.Serialization.XmlSerializer> не удается сериализация или десериализация, если тип сериализованного объекта <xref:System.Xml.XmlQualifiedName>.  
  
-   Все сериализаторам (<xref:System.Runtime.Serialization.DataContractSerializer>, <xref:System.Runtime.Serialization.Json.DataContractJsonSerializer>и <xref:System.Xml.Serialization.XmlSerializer>) не удается создать код сериализации для типа <xref:System.Xml.Linq.XElement?displayProperty=fullName> или типа, содержащего <xref:System.Xml.Linq.XElement>. Вместо этого они отображают ошибки во время построения.  
  
-   Следующие конструкторы типов сериализации не обязательно работают, как должны:  
  
    -   <xref:System.Runtime.Serialization.DataContractSerializer.%23ctor%28System.Type%2CSystem.Collections.Generic.IEnumerable%7BSystem.Type%7D%29?displayProperty=fullName>  
  
    -   <xref:System.Runtime.Serialization.DataContractSerializer.%23ctor%28System.Type%2CSystem.Runtime.Serialization.DataContractSerializerSettings%29?displayProperty=fullName>  
  
    -   <xref:System.Runtime.Serialization.DataContractSerializer.%23ctor%28System.Type%2CSystem.String%2CSystem.String%2CSystem.Collections.Generic.IEnumerable%7BSystem.Type%7D%29?displayProperty=fullName>  
  
    -   <xref:System.Runtime.Serialization.DataContractSerializer.%23ctor%28System.Type%2CSystem.Xml.XmlDictionaryString%2CSystem.Xml.XmlDictionaryString%2CSystem.Collections.Generic.IEnumerable%7BSystem.Type%7D%29?displayProperty=fullName>  
  
    -   <xref:System.Runtime.Serialization.Json.DataContractJsonSerializer.%23ctor%28System.Type%2CSystem.Runtime.Serialization.Json.DataContractJsonSerializerSettings%29?displayProperty=fullName>  
  
    -   <xref:System.Runtime.Serialization.Json.DataContractJsonSerializer.%23ctor%28System.Type%2CSystem.Collections.Generic.IEnumerable%7BSystem.Type%7D%29?displayProperty=fullName>  
  
    -   <xref:System.Xml.Serialization.XmlSerializer.%23ctor%28System.Type%2CSystem.String%29?displayProperty=fullName>  
  
    -   <xref:System.Xml.Serialization.XmlSerializer.%23ctor%28System.Type%2CSystem.Type%5B%5D%29?displayProperty=fullName>  
  
    -   <xref:System.Xml.Serialization.XmlSerializer.%23ctor%28System.Type%2CSystem.Xml.Serialization.XmlAttributeOverrides%29?displayProperty=fullName>  
  
    -   <xref:System.Xml.Serialization.XmlSerializer.%23ctor%28System.Type%2CSystem.Xml.Serialization.XmlRootAttribute%29?displayProperty=fullName>  
  
    -   <xref:System.Xml.Serialization.XmlSerializer.%23ctor%28System.Type%2CSystem.Xml.Serialization.XmlAttributeOverrides%2CSystem.Type%5B%5D%2CSystem.Xml.Serialization.XmlRootAttribute%2CSystem.String%29?displayProperty=fullName>  
  
-   <xref:System.Xml.Serialization.XmlSerializer> не удается создать код для типа, который содержит методы, определенные любым из следующих атрибутов:  
  
    -   <xref:System.Runtime.Serialization.OnSerializingAttribute>  
  
    -   <xref:System.Runtime.Serialization.OnSerializedAttribute>  
  
    -   <xref:System.Runtime.Serialization.OnDeserializingAttribute>  
  
    -   <xref:System.Runtime.Serialization.OnDeserializedAttribute>  
  
-   <xref:System.Xml.Serialization.XmlSerializer> не поддерживает настраиваемый интерфейс сериализации <xref:System.Xml.Serialization.IXmlSerializable> . Если имеется класс, реализующий этот интерфейс, <xref:System.Xml.Serialization.XmlSerializer> рассматривает тип, как тип объекта (POCO) простой старой среды CLR и сериализует только его открытые свойства.  
  
-   Сериализация простого объекта <xref:System.Exception> , как указано ниже, плохо работает с <xref:System.Runtime.Serialization.DataContractSerializer> и <xref:System.Runtime.Serialization.Json.DataContractJsonSerializer>:  
  
  
  
<a name="VS"></a>   
## <a name="visual-studio-differences"></a>Различия в Visual Studio  
 **Исключения и отладка**  
  
 При запуске приложений, скомпилированных с помощью [!INCLUDE[net_native](../../../includes/net-native-md.md)] в отладчике исключения первого шанса включены для следующих типов исключений:  
  
-   <xref:System.MemberAccessException>  
  
-   <xref:System.TypeAccessException>  
  
 **Создание приложений**  
  
 Используйте средства построения x 86, которые используются по умолчанию в Visual Studio. Не рекомендуется использование средств AMD64 MSBuild, которые находятся в C:\Program Files (x86)\MSBuild\12.0\bin\amd64; Это может создать проблемы построения.  
  
 **Профилировщики**  
  
-   Профилировщик ЦП в Visual Studio и профилировщик памяти XAML неправильно отображают «только мой код».  
  
-   Профилировщик памяти XAML неточно отображает данные управляемой кучи.  
  
-   Профилировщик ЦП неправильно идентифицирует модули и отображает имена функций с префиксами.  
  
 **Проекты модульных тестов библиотеки**  
  
 Включение [!INCLUDE[net_native](../../../includes/net-native-md.md)] для библиотеки модульного тестирования проекта приложений для магазина Windows не поддерживается и приводит к сбою построения проекта.  
  
## <a name="see-also"></a>См. также  
 [Начало работы](../../../docs/framework/net-native/getting-started-with-net-native.md)   
 [Справочник по конфигурационному файлу директив среды выполнения (rd.xml)](../../../docs/framework/net-native/runtime-directives-rd-xml-configuration-file-reference.md)   
 [Общие сведения о платформе .NET для приложений Магазина Windows](http://msdn.microsoft.com/library/windows/apps/br230302.aspx)   
 [Поддержка платформы .NET Framework для приложений магазина Windows и среды выполнения Windows](../../../docs/standard/cross-platform/support-for-windows-store-apps-and-windows-runtime.md)

