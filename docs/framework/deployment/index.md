---
title: "Развертывание .NET Framework и приложений | Документы Майкрософт"
ms.custom: 
ms.date: 03/30/2017
ms.prod: .net-framework
ms.reviewer: 
ms.suite: 
ms.technology:
- dotnet-clr
ms.tgt_pltfrm: 
ms.topic: article
dev_langs:
- VB
- CSharp
- C++
- jsharp
helpviewer_keywords:
- deploying applications [.NET Framework], packaging
- deploying applications [.NET Framework]
- deploying applications [.NET Framework], features
- deploying applications [.NET Framework], distribution
- .NET Framework, deploying
- .NET Framework application deployment
ms.assetid: 238d8284-6042-4a38-a7f6-1ee8efd719da
caps.latest.revision: 56
author: mairaw
ms.author: mairaw
manager: wpickett
translationtype: Human Translation
ms.sourcegitcommit: 3dbadeec14ce9c023af39ae4ff95d0183826e7c1
ms.openlocfilehash: eb0f0636abb39eabb387388fcdd39df8aca88c34
ms.lasthandoff: 04/13/2017

---
# <a name="deploying-the-net-framework-and-applications"></a>Развертывание .NET Framework и приложений
Эта статья поможет приступить к развертыванию платформы .NET Framework с приложением. Большая часть информации предназначена для разработчиков, изготовителей оборудования и администраторов предприятия. Пользователям, которые хотят установить .NET Framework на своих компьютерах, следует прочитать статью [Установка .NET Framework](~/docs/framework/install/index.md).  
  
## <a name="key-deployment-resources"></a>Основные ресурсы, посвященные развертыванию  
 Для получения дополнительных сведений о развертывании и обслуживании .NET Framework воспользуйтесь приведенными ниже ссылками на другие разделы MSDN.  
  
 **Установка и развертывание**  
  
-   Общие сведения об установщиках и развертывании:  
  
    -   Тип установщика:  
  
        -   [веб-установщик](~/docs/framework/install/guide-for-developers.md#to-install-or-download-the-net-framework-redistributable);  
  
        -   [автономный установщик](~/docs/framework/install/guide-for-developers.md#to-install-or-download-the-net-framework-redistributable).  
  
    -   Режимы установки:  
  
        -   [автоматическая установка](../../../docs/framework/deployment/deployment-guide-for-developers.md#chaining_custom);  
  
        -   [отображение пользовательского интерфейса](../../../docs/framework/deployment/deployment-guide-for-developers.md#chaining_default).  
  
    -   [Уменьшение числа перезагрузок при установке платформы .NET Framework 4.5](../../../docs/framework/deployment/reducing-system-restarts.md)  
  
    -   [Устранение неполадок заблокированных установок и удалений .NET Framework](~/docs/framework/install/troubleshoot-blocked-installations-and-uninstallations.md)  
  
-   Развертывание .NET Framework с клиентским приложением (для разработчиков):  
  
    -   [Использование InstallShield](../../../docs/framework/deployment/deployment-guide-for-developers.md#installshield) в проекте установки и развертывания.  
  
    -   [Использование приложения Visual Studio ClickOnce](../../../docs/framework/deployment/deployment-guide-for-developers.md#clickonce).  
  
    -   [Создание пакета установки WiX](../../../docs/framework/deployment/deployment-guide-for-developers.md#wix).  
  
    -   [Использование настраиваемого установщика](../../../docs/framework/deployment/deployment-guide-for-developers.md#chaining).  
  
    -   [Дополнительные сведения](../../../docs/framework/deployment/deployment-guide-for-developers.md) для разработчиков.  
  
-   Развертывание .NET Framework (для изготовителей оборудования и администраторов):  
  
    -   [Комплект средств для развертывания и оценки Windows (ADK)](http://go.microsoft.com/fwlink/p/?LinkId=254976).  
  
    -   [Руководство администратора](../../../docs/framework/deployment/guide-for-administrators.md).  
  
 **Обслуживание**.  
  
-   Общие сведения см. в [блоге по .NET Framework](http://go.microsoft.com/fwlink/p/?LinkId=254977).  
  
-   [Обнаружение версий](../../../docs/framework/migration-guide/how-to-determine-which-versions-are-installed.md).  
  
-   [Обнаружение обновлений и пакетов обновления](../../../docs/framework/migration-guide/how-to-determine-which-net-framework-updates-are-installed.md).  
  
## <a name="features-that-simplify-deployment"></a>Возможности, упрощающие развертывание  
 Платформа .NET Framework включает ряд функций, которые упрощают развертывание приложений.  
  
-   Изолированные приложения.  
  
     Эта функция обеспечивает изоляцию приложений и исключает конфликты библиотек DLL. По умолчанию компоненты не влияют на другие приложения.  
  
-   Частные компоненты по умолчанию.  
  
     По умолчанию компоненты развертываются в каталоге приложения и доступны только содержащему их приложению.  
  
-   Контролируемое совместное использование кода.  
  
     Для совместного использования кода необходимо явным образом предоставить к нему общий доступ — по умолчанию он не предоставляется.  
  
-   Управление параллельными версиями.  
  
     Одновременно могут сосуществовать несколько версий компонента или приложения. Вы можете выбрать используемые версии, а среда CLR применит политику управления версиями.  
  
-   Развертывание и репликация XCOPY.  
  
     Самоописываемые и автономные компоненты и приложения можно развертывать без записей в реестре и зависимостей.  
  
-   Оперативные обновления.  
  
     Администраторы могут использовать узлы, например ASP.NET, для обновления библиотек DLL программы даже на удаленных компьютерах.  
  
-   Интеграция с установщиком Windows.  
  
     При развертывании приложения доступны объявление, публикация, восстановление и установка по требованию.  
  
-   Корпоративное развертывание.  
  
     Эта функция упрощает распространение программного обеспечения, в том числе с помощью Active Directory.  
  
-   Скачивание и кэширование.  
  
     Добавочное скачивание позволяет сократить объем скачиваемых файлов. Кроме того, можно изолировать компоненты для использования только приложением, что позволяет снизить влияние развертывания на среду.  
  
-   Частично доверенный код.  
  
     Идентификация производится на основе кода, а не на основе пользователя. Не выводятся диалоговые окна сертификата.  
  
## <a name="packaging-and-distributing-net-framework-applications"></a>Упаковка и распространение приложений .NET Framework  
 Некоторые сведения о создании пакетов и развертывании для платформы .NET Framework приводятся в других разделах документации. В этих разделах содержатся сведения о самоописываемых блоках, называемых [сборками](../../../docs/framework/app-domains/assemblies-in-the-common-language-runtime.md), для которых не требуются записи в реестре, [сборках со строгими именами](../../../docs/framework/app-domains/strong-named-assemblies.md), которые гарантируют уникальность имен и предотвращают их подмену, и об [управлении версиями сборок](../../../docs/framework/app-domains/assembly-versioning.md), которое решает многие проблемы, связанные с конфликтами библиотек DLL. В разделах ниже содержатся сведения об упаковке и распространении приложений .NET Framework.  
  
### <a name="packaging"></a>Упаковка  
 Платформа .NET Framework предоставляет следующие способы упаковки приложений:  
  
-   В виде отдельной сборки или коллекции сборок.  
  
     В этом варианте DLL- или EXE-файлы используются в том виде, в котором они были созданы.  
  
-   В виде CAB-файлов.  
  
     В этом варианте файлы сжимаются в CAB-файлы, чтобы распространение или скачивание занимало меньше времени.  
  
-   В виде пакета установщика Windows или в других форматах установщика.  
  
     В этом случае вы создаете MSI-файлы для использования с установщиком Windows или пакет приложения для использования с другими установщиками.  
  
### <a name="distribution"></a>Распределение  
 Платформа .NET Framework предоставляет следующие варианты распространения приложений.  
  
-   С помощью XCOPY или FTP.  
  
     Так как приложения среды CLR являются самоописываемыми и не требуют записей в реестре, вы можете использовать XCOPY или FTP, чтобы просто скопировать приложение в соответствующий каталог. Затем приложение можно будет запустить из этого каталога.  
  
-   С помощью скачивания кода.  
  
     Если приложение распространяется через Интернет или корпоративную интрасеть, можно просто скачать код на компьютер и запустить его.  
  
-   С помощью программы установки, например установщика Windows версии 2.0.  
  
     Установщик Windows версии 2.0 позволяет устанавливать, восстанавливать и удалять сборки .NET Framework в глобальном кэше сборок и в личных каталогах.  
  
### <a name="installation-location"></a>Расположение установки  
 Сведения о том, как выбрать место для развертывания сборок приложения таким образом, чтобы их могла найти среда выполнения, см. в статье [Обнаружение сборок в среде выполнения](../../../docs/framework/deployment/how-the-runtime-locates-assemblies.md).  
  
 На выбор способа развертывания приложения могут также влиять соображения безопасности. Разрешения безопасности предоставляются управляемому коду в соответствии с его расположением. Развертывание приложения или компонента в расположении, где они получают низкий уровень доверия, например в Интернете, ограничивает возможности приложения или компонента. Дополнительные сведения о развертывании и аспектах безопасности см. в статье [Основы управления доступом для кода](../../../docs/framework/misc/code-access-security-basics.md).  
  
## <a name="related-topics"></a>Связанные разделы  
  
|Заголовок|Описание|  
|-----------|-----------------|  
|[Обнаружение сборок в среде выполнения](../../../docs/framework/deployment/how-the-runtime-locates-assemblies.md)|Описывается то, как среда CLR определяет, какую сборку следует использовать для выполнения запроса привязки.|  
|[Рекомендации для загрузки сборок](../../../docs/framework/deployment/best-practices-for-assembly-loading.md)|Описываются способы избежания проблем с идентификацией типов, которые могут привести к таким ошибкам, как <xref:System.InvalidCastException>, <xref:System.MissingMethodException> и т. д.|  
|[Уменьшение числа перезагрузок при установке платформы .NET Framework 4.5](../../../docs/framework/deployment/reducing-system-restarts.md)|Описывается диспетчер перезапуска, который по возможности предотвращает перезагрузки, и его преимущества для приложений, устанавливающих платформу .NET Framework.|  
|[Руководство по развертыванию для администраторов](../../../docs/framework/deployment/guide-for-administrators.md)|Описывается развертывание платформы .NET Framework и ее системных зависимостей в сети с помощью System Center Configuration Manager (SCCM).|  
|[Руководство по развертыванию для разработчиков](../../../docs/framework/deployment/deployment-guide-for-developers.md)|Описываются способы установки .NET Framework на компьютеры пользователей вместе с приложениями.|  
|[Развертывание приложений, служб и компонентов](https://docs.microsoft.com/visualstudio/deployment/deploying-applications-services-and-components)|Рассматриваются варианты развертывания в Visual Studio, включая инструкции по публикации приложения с помощью технологии ClickOnce и установщика Windows.| 
|[Публикация приложений ClickOnce](http://msdn.microsoft.com/library/eb6dfe79-f54c-4331-8e36-073688e70973)|Описывается, как упаковать приложение Windows Forms и развернуть его на клиентских компьютерах в сети с помощью технологии ClickOnce.|  
|[Упаковка и развертывание ресурсов](../../../docs/framework/resources/packaging-and-deploying-resources-in-desktop-apps.md)|Описывается модель "звезда", которую платформа .NET Framework использует для упаковки и развертывания ресурсов; рассматриваются соглашения об именовании ресурсов, процесс перехода на резервные ресурсы и альтернативные способы упаковки.|  
|[Развертывание приложения взаимодействия](../../../docs/framework/interop/deploying-an-interop-application.md)|Описывается поставка и установка приложений взаимодействия, которые обычно включают клиентскую сборку .NET Framework, одну или несколько сборок взаимодействия (представляющих различные библиотеки типов COM) и один или несколько зарегистрированных COM-компонентов.|  
|[Практическое руководство. Получение хода выполнения установщика .NET Framework 4.5](../../../docs/framework/deployment/how-to-get-progress-from-the-dotnet-installer.md)|Описывается автоматический запуск и отслеживание процесса установки .NET Framework с выводом собственного представления хода выполнения установки.|  
  
## <a name="see-also"></a>См. также  
 [Руководство по разработке](../../../docs/framework/development-guide.md)
