---
title: "Пошаговое руководство. Выполнение типичных задач с помощью смарт-тегов в элементах управления Windows Forms | Microsoft Docs"
ms.custom: ""
ms.date: "03/30/2017"
ms.prod: ".net-framework-4.6"
ms.reviewer: ""
ms.suite: ""
ms.technology: 
  - "dotnet-winforms"
ms.tgt_pltfrm: ""
ms.topic: "article"
dev_langs: 
  - "jsharp"
helpviewer_keywords: 
  - "действия конструктора"
  - "объектная модель действий конструктора"
  - "смарт-теги"
ms.assetid: cac337e6-00f6-4584-80f4-75728f5ea113
caps.latest.revision: 15
author: "dotnet-bot"
ms.author: "dotnetcontent"
manager: "wpickett"
caps.handback.revision: 15
---
# Пошаговое руководство. Выполнение типичных задач с помощью смарт-тегов в элементах управления Windows Forms
При создании форм и элементов управления для приложений Windows Forms многие задания приходится выполнять неоднократно.  В ряд самых распространенных выполняемых заданий входит:  
  
-   Добавление или удаление вкладки на <xref:System.Windows.Forms.TabControl>.  
  
-   Закрепление элемента управления в родительском элементе.  
  
-   Изменение ориентации элемента управления <xref:System.Windows.Forms.SplitContainer>.  
  
 Для ускорения процесса разработки во многих элементах управления содержатся смарт\-теги. Это контекстные меню, позволяющие во время разработки выполнять подобные распространенные задания одним щелчком мыши.  Эти задачи называются *командами смарт\-тегов*.  
  
 Смарт\-теги остаются прикрепленными к экземпляру элемента управления на протяжении его времени существования в конструкторе и всегда доступны.  
  
 В этом пошаговом руководстве демонстрируется выполнение следующих задач.  
  
-   создание проекта типа Windows Forms;  
  
-   Использование смарт\-тегов  
  
-   Подключение и отключение смарт\-тегов  
  
 По завершению процесса ознакомления вы получите представление о роли, которую играют эти важные средства работы с макетами.  
  
> [!NOTE]
>  Отображаемые диалоговые окна и команды меню могут отличаться от описанных в справке в зависимости от текущих настроек или выпуска.  Чтобы изменить параметры, в меню **Сервис** выберите команду **Импорт и экспорт параметров**.  Дополнительные сведения см. в разделе [Customizing Development Settings in Visual Studio](http://msdn.microsoft.com/ru-ru/22c4debb-4e31-47a8-8f19-16f328d7dcd3).  
  
## Создание проекта  
 Для начала следует создать проект и подготовить форму.  
  
#### Создание проекта  
  
1.  Создайте проект приложения на базе Windows и назовите его "SmartTagsExample".  Дополнительные сведения см. в разделе [How to: Create a Windows Application Project](http://msdn.microsoft.com/ru-ru/b2f93fed-c635-4705-8d0e-cf079a264efa).  
  
2.  В конструкторе **Windows Forms** выберите форму.  
  
## Использование смарт\-тегов  
 Смарт\-теги всегда доступны во время разработки, если элемент управления их содержит.  
  
#### Чтобы использовать сматр\-теги  
  
1.  Перетащите <xref:System.Windows.Forms.TabControl> из **панели элементов** в форму.  Обратите внимание на глиф смарт\-тега \(![Глиф смарт&#45;тега](../../../../docs/framework/winforms/controls/media/vs-winformsmttagglyph.png "VS\_WinFormSmtTagGlyph")\), появляющийся на краю <xref:System.Windows.Forms.TabControl>.  
  
2.  Щелкните глиф смарт\-тега.  Из контекстного меню, появившегося рядом с глифом, выберите элемент **Добавить вкладку**.  Убедитесь, что к <xref:System.Windows.Forms.TabControl> была добавлена новая страница вкладки.  
  
3.  Перетащите элемент управления <xref:System.Windows.Forms.TableLayoutPanel> из **панели элементов** в форму.  
  
4.  Щелкните глиф смарт\-тега.  Из контекстного меню, появившегося рядом с глифом, выберите элемент **Добавить столбец**.  Убедитесь, что к элементу управления <xref:System.Windows.Forms.TableLayoutPanel> добавляется новый столбец.  
  
5.  Перетащите элемент управления <xref:System.Windows.Forms.SplitContainer> из **панели элементов** в форму.  
  
6.  Щелкните глиф смарт\-тега.  Из контекстного меню, появившегося рядом с глифом, выберите элемент **Ориентация горизонтального разделителя**.  Убедитесь, что теперь полоса разделения элемента управления <xref:System.Windows.Forms.SplitContainer> ориентирована по горизонтали.  
  
## См. также  
 <xref:System.Windows.Forms.TextBox>   
 <xref:System.Windows.Forms.TabControl>   
 <xref:System.Windows.Forms.SplitContainer>   
 <xref:System.ComponentModel.Design.DesignerActionList>   
 [Walkthrough: Adding Smart Tags to a Windows Forms Component](../Topic/Walkthrough:%20Adding%20Smart%20Tags%20to%20a%20Windows%20Forms%20Component.md)