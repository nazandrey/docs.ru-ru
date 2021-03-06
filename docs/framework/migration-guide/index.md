---
title: "Руководство по переходу на .NET Framework 4.7, 4.6 и 4.5 "
ms.custom: 
ms.date: 03/30/2017
ms.prod: .net-framework
ms.reviewer: 
ms.suite: 
ms.technology:
- dotnet-clr
ms.tgt_pltfrm: 
ms.topic: article
helpviewer_keywords:
- .NET Framework, migrating applications to
- migration, .NET Framework
ms.assetid: 02d55147-9b3a-4557-a45f-fa936fadae3b
caps.latest.revision: 56
author: rpetrusha
ms.author: ronpet
manager: wpickett
ms.translationtype: HT
ms.sourcegitcommit: 306c608dc7f97594ef6f72ae0f5aaba596c936e1
ms.openlocfilehash: 820d1966172a93c06c6451c51bc7f360496f46b8
ms.contentlocale: ru-ru
ms.lasthandoff: 07/28/2017

---
# <a name="migration-guide-to-the-net-framework-47-46-and-45"></a>Руководство по переходу на .NET Framework 4.7, 4.6 и 4.5 
Если приложение создано с использованием более ранней версии .NET Framework, обычно его можно легко обновить до версии .NET Framework 4.5 и ее доработанных выпусков (4.5.1 и 4.5.2), версии NET Framework 4.6 и ее доработанных выпусков (4.6.1 и 4.6.2) или версии .NET Framework 4.7. Откройте проект в Visual Studio. Если проект был создан в более ранней версии, автоматически откроется диалоговое окно **Совместимость проекта**. Дополнительные сведения об обновлении проекта в Visual Studio см. в разделах [Перенос, миграция и обновление проектов Visual Studio](/visualstudio/porting/port-migrate-and-upgrade-visual-studio-projects) и [Целевая платформа и совместимость для Visual Studio 2017](https://www.visualstudio.com/en-us/productinfo/vs2017-compatibility-vs).  
  
 Однако некоторые изменения в платформе .NET Framework требуют внесения изменений в код. Кроме того, возможно, вы захотите воспользоваться преимуществами новых возможностей в версии .NET Framework 4.5, ее доработанных выпусках, в версии .NET Framework 4.6 и ее доработанных выпусках или в версии .NET Framework 4.7. Внесение подобных изменений в приложение для новой версии платформы .NET Framework обычно называется *миграцией*. Если нет необходимости в миграции приложения, его можно выполнить в среде .NET Framework 4.5 и более поздних версий без компиляции.  
  
## <a name="migration-resources"></a>Ресурсы миграции  
 Перед миграцией приложения из более ранних версий .NET Framework в версию 4.5, 4.5.1, 4.5.2, 4.6, 4.6.1, 4.6.2 или 4.7 просмотрите следующие документы:  
  
-   См. раздел [Версии и зависимости](../../../docs/framework/migration-guide/versions-and-dependencies.md), чтобы определить версию среды CLR, лежащую в основе каждой версии .NET Framework, и ознакомиться с рекомендациями по определению целевой платформы для приложений.  
  
-   Просмотрите раздел [Совместимость приложений](../../../docs/framework/migration-guide/application-compatibility.md), чтобы узнать об изменениях в среде выполнения и изменении целевой платформы, которые могут повлиять на приложение, а также о том, что с ними делать.  
  
-   Просмотрите раздел [Устаревшие классы библиотеки классов](../../../docs/framework/whats-new/whats-obsolete.md), чтобы определить устаревшие типы и члены в коде и рекомендуемые альтернативы.  
  
-   См. раздел [Новые возможности](../../../docs/framework/whats-new/index.md), чтобы узнать о новых возможностях, которые вы, возможно, захотите добавить в приложение.  
  
## <a name="see-also"></a>См. также  
 [Совместимость приложений](../../../docs/framework/migration-guide/application-compatibility.md)   
 [Миграция из .NET Framework 1.1](../../../docs/framework/migration-guide/migrating-from-the-net-framework-1-1.md)   
 [Совместимость версий](../../../docs/framework/migration-guide/version-compatibility.md)   
 [Версии и зависимости](../../../docs/framework/migration-guide/versions-and-dependencies.md)   
 [Практическое руководство. Настройка приложения для поддержки платформы .NET Framework 4 или 4.5](../../../docs/framework/migration-guide/how-to-configure-an-app-to-support-net-framework-4-or-4-5.md)   
 [Новые возможности](../../../docs/framework/whats-new/index.md)   
 [Устаревшие классы библиотеки классов](../../../docs/framework/whats-new/whats-obsolete.md)   
 [Сведения о версиях и сборках платформы .NET Framework](http://go.microsoft.com/fwlink/?LinkId=201701)   
 [Политика жизненного цикла поддержки Microsoft .NET Framework](http://go.microsoft.com/fwlink/?LinkId=196607) [Проблемы при миграции на .NET Framework 4](net-framework-4-migration-issues.md)

