---
title: "Использование Windows Management Instrumentation для диагностики | Microsoft Docs"
ms.custom: ""
ms.date: "03/30/2017"
ms.prod: ".net-framework-4.6"
ms.reviewer: ""
ms.suite: ""
ms.technology: 
  - "dotnet-clr"
ms.tgt_pltfrm: ""
ms.topic: "article"
ms.assetid: fe48738d-e31b-454d-b5ec-24c85c6bf79a
caps.latest.revision: 24
author: "Erikre"
ms.author: "erikre"
manager: "erikre"
caps.handback.revision: 24
---
# Использование Windows Management Instrumentation для диагностики
[!INCLUDE[indigo1](../../../../../includes/indigo1-md.md)] предоставляет данные проверки службы в среде выполнения с помощью поставщика инструментария управления Windows \(WMI\) [!INCLUDE[indigo2](../../../../../includes/indigo2-md.md)].  
  
## Реализация WMI  
 Инструментарий WMI \- это реализованный корпорацией Майкрософт стандарт управления предприятием через Интернет \(WBEM\).  [!INCLUDE[crabout](../../../../../includes/crabout-md.md)] пакете средств разработки инструментария WMI SDK см. в библиотеке MSDN.  \(http:\/\/msdn.microsoft.com\/library\/default.asp?url\=\/library\/wmisdk\/wmi\/wmi\_start\_page.asp\).  WBEM является отраслевым стандартом предоставления приложениями инструментария управления для внешних средств управления.  
  
 Поставщик инструментария WMI \- это компонент, предоставляющий инструментарий в среде выполнения с помощью совместимого с WBEM интерфейса.  Он состоит из набора объектов инструментария WMI, имеющих пары атрибут\/значение.  Пары могут быть нескольких простых типов.  Средства управления могут подключаться к службам через интерфейс во время выполнения.  [!INCLUDE[indigo2](../../../../../includes/indigo2-md.md)] предоставляет атрибуты служб, такие как адреса, привязки, поведения и прослушиватели.  
  
 Встроенный поставщик инструментария WMI может быть активирован в файле конфигурации приложения.  Это делается с помощью атрибута `wmiProviderEnabled` [\<диагностика\>](../../../../../docs/framework/configure-apps/file-schema/wcf/diagnostics.md) в разделе [\<system.serviceModel\>](../../../../../docs/framework/configure-apps/file-schema/wcf/system-servicemodel.md), как показано в следующем примере конфигурации.  
  
```  
<system.serviceModel>  
    …  
    <diagnostics wmiProviderEnabled="true" />  
    …  
</system.serviceModel>  
  
```  
  
 Эта запись конфигурации предоставляет интерфейс инструментария WMI.  Теперь приложения управления могут подключаться через этот интерфейс и обращаться к инструментарию управления приложения.  
  
## Доступ к данным WMI  
 Доступ к данным инструментария WMI может осуществляться несколькими различными способами.  Microsoft предоставляет программные интерфейсы WMI для скриптов, приложений [!INCLUDE[vbprvb](../../../../../includes/vbprvb-md.md)], приложений на языке C\+\+ и [!INCLUDE[dnprdnshort](../../../../../includes/dnprdnshort-md.md)].  Дополнительные сведения см. в разделе [Использование инструментария WMI](http://go.microsoft.com/fwlink/?LinkId=95183).  
  
> [!CAUTION]
>  При использовании предоставляемых .NET Framework методов для программного доступа к данным WMI следует помнить о том, что при установке подключения эти методы могут вызывать исключения.  Подключение устанавливается не во время создания экземпляра <xref:System.Management.ManagementObject>, а при получении первого запроса, при котором происходит реальный обмен данными.  Следовательно, следует использовать блок `try..catch` для перехвата возможных исключений.  
  
 Можно изменить уровень ведения журнала сообщений и трассировок, а также параметры журнала сообщений для источника трассировки `System.ServiceModel` в инструментарии WMI.  Для этого необходимо обратиться к экземпляру [AppDomainInfo](../../../../../docs/framework/wcf/diagnostics/wmi/appdomaininfo.md), который предоставляет доступ к следующим логическим свойствам: `LogMessagesAtServiceLevel`, `LogMessagesAtTransportLevel`, `LogMalformedMessages` и `TraceLevel`.  Поэтому, если в файле конфигурации прослушиватель трассировки настроен на ведение журнала, но эти параметры имеют значение `false`, можно впоследствии изменить их значения на `true`, когда приложение будет выполняться.  В результате ведение журнала будет включено во время выполнения.  Аналогично, если ведение журнала было включено в файле конфигурации, его можно отключить во время выполнения с помощью инструментария WMI.  
  
 Необходимо помнить, что если прослушиватели трассировок журнала сообщений для журнала сообщений или прослушиватели трассировок `System.ServiceModel` для трассировок не заданы в файле конфигурации, ни одно из изменений не будет применено, даже если эти изменения принимаются инструментарием WMI.  Дополнительные сведения о правильной настройке соответствующих прослушивателей см. в разделах [Настройка ведения журнала сообщений](../../../../../docs/framework/wcf/diagnostics/configuring-message-logging.md) и [Настройка трассировки](../../../../../docs/framework/wcf/diagnostics/tracing/configuring-tracing.md).  Уровень трассировки всех остальных источников трассировки, заданных конфигурацией, действителен только при запуске приложения и не может быть изменен.  
  
 [!INCLUDE[indigo2](../../../../../includes/indigo2-md.md)] предоставляет метод `GetOperationCounterInstanceName` скриптов.  Если передать этому методу имя операции, он возвращает имя экземпляра счетчика производительности.  Однако он не проверяет входные данные.  Таким образом, если передать ему неправильное имя операции, возвращается неверное имя счетчика.  
  
 Свойство `OutgoingChannel` экземпляра `Service` не выполняет подсчет каналов, открытых службой для подключения к другой службе, если клиент [!INCLUDE[indigo2](../../../../../includes/indigo2-md.md)] для службы назначения не создается в методе `Service`.  
  
 **Внимание.** Инструментарий WMI поддерживает значение <xref:System.TimeSpan> только до 3 знаков после десятичного разделителя.  Например, если одному из свойств служба присваивает значение <xref:System.TimeSpan.MaxValue>, при просмотре через WMI это значение усекается до 3 знаков после десятичного разделителя.  
  
## Безопасность  
 Так как поставщик инструментария WMI [!INCLUDE[indigo2](../../../../../includes/indigo2-md.md)] разрешает обнаружение служб в среде, необходимо проявлять предельную осторожность при предоставлении к нему доступа.  При изменении настроек по умолчанию, при которых доступ предоставляется только администратору, третьи лица с более низкой степенью доверия могут получить доступ к конфиденциальным данным в среде.  В частности, при расширении разрешений для удаленного доступа к WMI могут возникнуть атаки на переполнение.  При переполнении процесса избыточными запросами WMI производительность процесса может снизится.  
  
 Кроме того, при расширении прав доступа к файлу MOF третьи лица с более низкой степенью доверия могут управлять поведением WMI и изменять объекты, находящиеся в схеме WMI.  Например, поля могут быть удалены таким образом, что критические данные скрыты от администратора; или поля, которые не заполняются или вызывают исключения, добавляются в файл.  
  
 По умолчанию поставщик инструментария управления Windows [!INCLUDE[indigo2](../../../../../includes/indigo2-md.md)] гарантирует право доступа администратора к инструментам "Выполнить метод", "Запись поставщика" и "Включить учетную запись", а также к инструменту "Включить учетную запись" для службы ASP.NET, локальной службы и сетевой службы.  В частности, на платформах, отличных от [!INCLUDE[wv](../../../../../includes/wv-md.md)], учетная запись ASP.NET имеет доступ на чтение в пространстве имен ServiceModel инструментария WMI.  Если требуется не предоставлять эти привилегии определенной группе пользователей, нужно либо деактивировать поставщика инструментария WMI \(по умолчанию он отключен\), либо запретить доступ указанной группы пользователей.  
  
 Возможно, поставщик инструментария WMI не удастся включить через конфигурацию по причине недостаточных привилегий пользователя.  Однако при возникновении этого сбоя никакой записи в журнале событий не делается.  
  
 Для изменения уровней привилегий пользователя выполните следующие шаги.  
  
1.  Нажмите "Пуск", затем "Выполнить" и введите **compmgmt.msc**.  
  
2.  Щелкните правой кнопкой мыши **Службы и приложения\/Управляющий элемент WMI** и выберите **Свойства**.  
  
3.  Откройте вкладку **Безопасность** и перейдите в пространство имен **Root\/ServiceModel**.  Нажмите кнопку **Безопасность**.  
  
4.  Выберите группу или пользователя, доступ которых требуется настроить, и установите флажок **Разрешить** или **Запретить**, чтобы задать разрешение.  
  
## Предоставление дополнительным пользователям разрешений на регистрацию WCF WMI  
 WCF предоставляет доступ к данным управления для WMI.  Для этого размещается внутрипроцессный поставщик WMI, который иногда называется «несвязанным поставщиком».  Для предоставления доступа к данным управления учетная запись, которая регистрирует этот поставщик, должна иметь необходимые разрешения.  В Windows регистрация несвязанных поставщиков по умолчанию доступна только небольшому числу привилегированных учетных записей. Это обстоятельство вызывает затруднения, поскольку пользователям обычно нужно предоставлять доступ к данным WMI из службы WCF, которая работает с учетной записью, не входящей в набор по умолчанию.  
  
 Чтобы предоставить такой доступ, администратор должен предоставить дополнительной учетной записи следующие разрешения в указанном порядке:  
  
1.  разрешение на доступ к пространству имен WCF WMI;  
  
2.  разрешение на регистрацию несвязанного поставщика WMI для WCF.  
  
#### Предоставление разрешения на доступ к пространству имен WMI  
  
1.  Выполните следующий скрипт PowerShell.  
  
    ```powershell  
    write-host “”  
    write-host “Granting Access to root/servicemodel WMI namespace to built in users group”  
    write-host “”  
  
    # Create the binary representation of the permissions to grant in SDDL  
    $newPermissions = "O:BAG:BAD:P(A;CI;CCDCLCSWRPWPRCWD;;;BA)(A;CI;CC;;;NS)(A;CI;CC;;;LS)(A;CI;CC;;;BU)"  
    $converter = new-object system.management.ManagementClass Win32_SecurityDescriptorHelper  
    $binarySD = $converter.SDDLToBinarySD($newPermissions)  
    $convertedPermissions = ,$binarySD.BinarySD  
  
    # Get the object used to set the permissions  
    $security = gwmi -namespace root/servicemodel -class __SystemSecurity  
  
    # Get and output the current settings  
    $binarySD = @($null)  
    $result = $security.PsBase.InvokeMethod("GetSD",$binarySD)  
  
    $outsddl = $converter.BinarySDToSDDL($binarySD[0])  
    write-host "Previous ACL: "$outsddl.SDDL  
  
    # Change the Access Control List (ACL) using SDDL  
    $result = $security.PsBase.InvokeMethod("SetSD",$convertedPermissions)   
  
    # Get and output the current settings  
    $binarySD = @($null)  
    $result = $security.PsBase.InvokeMethod("GetSD",$binarySD)  
  
    $outsddl = $converter.BinarySDToSDDL($binarySD[0])  
    write-host "New ACL:      "$outsddl.SDDL  
    write-host “”  
  
    ```  
  
     В этом скрипте PowerShell с помощью языка SDDL встроенной группе «Пользователи» предоставляется доступ к пространству имен WMI «root\/servicemodel».  В нем указываются следующие списки управления доступом.  
  
    -   Встроенная учетная запись администратора \(BA\) \- уже имеет доступ.  
  
    -   Сетевая служба \(NS\) \- уже имеет доступ.  
  
    -   Локальная система \(LS\) \- уже имеет доступ.  
  
    -   Встроенные пользователи \- группа, которой предоставляется доступ.  
  
#### Предоставление доступа к регистрации поставщика  
  
1.  Выполните следующий скрипт PowerShell.  
  
    ```powershell  
    write-host “”  
    write-host “Granting WCF provider registration access to built in users group”  
    write-host “”  
    # Set security on ServiceModel provider  
    $provider = get-WmiObject -namespace "root\servicemodel" __Win32Provider  
  
    write-host "Previous ACL: "$provider.SecurityDescriptor  
    $result = $provider.SecurityDescriptor = "O:BUG:BUD:(A;;0x1;;;BA)(A;;0x1;;;NS)(A;;0x1;;;LS)(A;;0x1;;;BU)"  
  
    # Commit the changes and display it to the console  
    $result = $provider.Put()  
    write-host "New ACL:      "$provider.SecurityDescriptor  
    write-host “”  
  
    ```  
  
### Предоставление доступа произвольным пользователям или группам  
 В примере из этого раздела всем локальным пользователям предоставляются права доступа к регистрации поставщика WMI.  Если нужно предоставить доступ пользователю или группе, которые не являются встроенными, необходимо получить идентификатор безопасности этого пользователя или группы.  Нет общего метода для получения идентификатора безопасности любого пользователя.  В качестве одного из решений можно выполнить вход от имени нужного пользователя, а затем выполнить следующую команду в командной оболочке.  
  
```  
Whoami /user  
```  
  
 Это позволит получить идентификатор безопасности текущего пользователя, однако такой метод нельзя использовать, чтобы получить идентификатор безопасности любого пользователя.  Другим методом для получения идентификатора безопасности является использование программы [getsid.exe](http://go.microsoft.com/fwlink/?LinkId=186467) из [набора Windows 2000 Resource Kit Tools for administrative tasks \(на английском языке\)](http://go.microsoft.com/fwlink/?LinkId=178660).  Эта программа сравнивает идентификаторы безопасности двух пользователей \(локальных или пользователей домена\) и помимо прочего выводит два идентификатора в командную строку.  [!INCLUDE[crdefault](../../../../../includes/crdefault-md.md)][Известные идентификаторы безопасности](http://go.microsoft.com/fwlink/?LinkId=186468).  
  
## Доступ к экземплярам удаленных объектов WMI  
 Если требуется получить доступ к экземплярам инструментария управления Windows [!INCLUDE[indigo2](../../../../../includes/indigo2-md.md)] с удаленного компьютера, необходимо разрешить конфиденциальность пакета в используемых для доступа средствах.  В следующем разделе описывается, как это можно сделать с помощью WMI CIM Studio, тестера инструментария управления Windows или .NET SDK 2.0.  
  
### WMI CIM Studio  
 Если установлены [Средства управления WMI](http://go.microsoft.com/fwlink/?LinkId=95185), для доступа к экземплярам WMI можно использовать WMI CIM Studio  Средства расположены в следующей папке:  
  
 **%windir%\\Program Files\\WMI Tools\\.**  
  
1.  В окне **Подключиться к пространству имен:** введите «root\\ServiceModel» и нажмите **ОК**.  
  
2.  В окне **Вход в WMI CIM Studio** нажмите кнопку **Параметры \>\>**, чтобы развернуть окно.  Выберите **Защита пакетов** для элемента **Уровень проверки подлинности** и нажмите **OK**.  
  
### Тестер инструментария управления Windows  
 Это средство установлено Windows.  Для запуска в диалоговом окне **Пуск или выполнить** откройте программу командной строки, введя «cmd.exe» и нажав **ОК**.  Затем введите wbemtest.exe в командном окне.  Запустится средство Тестер инструментария управления Windows.  
  
1.  В правом верхнем углу окна нажмите кнопку **Подключить**.  
  
2.  В новом окне в поле **Пространство имен** введите «root\\ServiceModel» и выберите **Защита пакетов** для элемента **Уровень проверки подлинности**.  Нажмите **Подключить**.  
  
### Использование управляемого кода  
 Доступ к удаленным экземплярам WMI также может осуществляться программно с помощью классов, предоставляемых пространством имен <xref:System.Management>.  В следующем образце кода показано, как это сделать.  
  
```  
String wcfNamespace = String.Format(@"\\{0}\Root\ServiceModel",      
   this.serviceMachineName);  
  
ConnectionOptions connection = new ConnectionOptions();  
connection.Authentication = AuthenticationLevel.PacketPrivacy;  
ManagementScope scope = new ManagementScope(this.wcfNamespace, connection);  
```