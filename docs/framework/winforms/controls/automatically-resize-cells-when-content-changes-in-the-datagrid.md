---
title: "Практическое руководство. Автоматическое изменение размера ячеек при изменении содержимого в элементе управления DataGridView в Windows Forms | Microsoft Docs"
ms.custom: ""
ms.date: "03/30/2017"
ms.prod: ".net-framework"
ms.reviewer: ""
ms.suite: ""
ms.technology: 
  - "dotnet-winforms"
ms.tgt_pltfrm: ""
ms.topic: "article"
dev_langs: 
  - "jsharp"
helpviewer_keywords: 
  - "таблицы данных, автоматическое изменение размера ячеек"
  - "ячейки, автоматическое изменение размера"
  - "DataGridView — элемент управления [Windows Forms], изменение размера ячеек"
ms.assetid: 1d68934d-a04c-4b12-9e66-c856c6828131
caps.latest.revision: 19
author: "dotnet-bot"
ms.author: "dotnetcontent"
manager: "wpickett"
caps.handback.revision: 19
---
# Практическое руководство. Автоматическое изменение размера ячеек при изменении содержимого в элементе управления DataGridView в Windows Forms
Для элемента управления <xref:System.Windows.Forms.DataGridView> можно настроить автоматическое изменение размера строк, столбцов и заголовков при изменении содержимого так, чтобы значения ячеек могли отображаться в них без усечения.  
  
 Ограничить ячейки для определения нового размера можно несколькими вариантами. Например, для элемента управления можно настроить автоматическое изменение ширины, исходя исключительно из значений в строках, отображаемых в данный момент. Благодаря этому можно добиться большей эффективности при работе с большим количеством строк. Однако в этом случае может потребоваться использовать методы изменения размера <xref:System.Windows.Forms.DataGridView.AutoResizeColumns%2A> для контроля размера в нужный момент.  
  
 Дополнительные сведения об автоматическом изменении размера см. в разделе [Изменение размеров управления DataGridView в Windows Forms](../../../../docs/framework/winforms/controls/sizing-options-in-the-windows-forms-datagridview-control.md).  
  
 В примере кода ниже показаны варианты автоматического изменения размера.  
  
## Пример  
 [!code-cpp[System.Windows.Forms.DataGridView.AutoSizing#0](../../../../samples/snippets/cpp/VS_Snippets_Winforms/System.Windows.Forms.DataGridView.AutoSizing/CPP/autosizing.cpp#0)]
 [!code-csharp[System.Windows.Forms.DataGridView.AutoSizing#0](../../../../samples/snippets/csharp/VS_Snippets_Winforms/System.Windows.Forms.DataGridView.AutoSizing/CS/autosizing.cs#0)]
 [!code-vb[System.Windows.Forms.DataGridView.AutoSizing#0](../../../../samples/snippets/visualbasic/VS_Snippets_Winforms/System.Windows.Forms.DataGridView.AutoSizing/VB/autosizing.vb#0)]  
  
## Компиляция кода  
 Для этого примера требуются:  
  
-   ссылки на сборки System, System.Drawing и System.Windows.Forms.  
  
 Дополнительные сведения о выполнении сборки этого примера из командной строки для [!INCLUDE[vbprvb](../../../../includes/vbprvb-md.md)] или [!INCLUDE[csprcs](../../../../includes/csprcs-md.md)] см. в разделе [Построение из командной строки](../Topic/Building%20from%20the%20Command%20Line%20\(Visual%20Basic\).md) или [Построение из командной строки с помощью csc.exe](../../../../ocs/csharp/language-reference/compiler-options/command-line-building-with-csc-exe.md). Чтобы выполнить сборку этого примера в [!INCLUDE[vsprvs](../../../../includes/vsprvs-md.md)], можно также вставить код в новый проект.  См. также [Практическое руководство. Компиляция и выполнение откомпилированного примера кода формы Windows Forms с помощью Visual Studio](http://msdn.microsoft.com/library/Bb129228\(v=vs.110\)).  
  
## См. также  
 <xref:System.Windows.Forms.DataGridView>   
 <xref:System.Windows.Forms.DataGridView.ColumnHeadersHeightSizeMode%2A?displayProperty=fullName>   
 <xref:System.Windows.Forms.DataGridView.RowHeadersWidthSizeMode%2A?displayProperty=fullName>   
 <xref:System.Windows.Forms.DataGridView.AutoSizeColumnsMode%2A?displayProperty=fullName>   
 <xref:System.Windows.Forms.DataGridView.AutoSizeRowsMode%2A?displayProperty=fullName>   
 <xref:System.Windows.Forms.DataGridViewColumn.AutoSizeMode%2A?displayProperty=fullName>   
 <xref:System.Windows.Forms.DataGridViewColumn.InheritedAutoSizeMode%2A?displayProperty=fullName>   
 <xref:System.Windows.Forms.DataGridViewAutoSizeRowsMode>   
 <xref:System.Windows.Forms.DataGridViewAutoSizeColumnMode>   
 <xref:System.Windows.Forms.DataGridViewAutoSizeColumnsMode>   
 <xref:System.Windows.Forms.DataGridViewColumnHeadersHeightSizeMode>   
 <xref:System.Windows.Forms.DataGridViewRowHeadersWidthSizeMode>   
 [Изменение размера столбцов и строк элемента управления DataGridView в Windows Forms](../../../../docs/framework/winforms/controls/resizing-columns-and-rows-in-the-windows-forms-datagridview-control.md)   
 [Изменение размеров управления DataGridView в Windows Forms](../../../../docs/framework/winforms/controls/sizing-options-in-the-windows-forms-datagridview-control.md)   
 [Практическое руководство. Программное изменение размера ячеек элемента управления DataGridView в соответствии с размером отображаемых данных в Windows Forms](../../../../docs/framework/winforms/controls/programmatically-resize-cells-to-fit-content-in-the-datagrid.md)