---
title: "Система типов (Entity SQL) | Microsoft Docs"
ms.custom: ""
ms.date: "03/30/2017"
ms.prod: ".net-framework-4.6"
ms.reviewer: ""
ms.suite: ""
ms.technology: 
  - "dotnet-ado"
ms.tgt_pltfrm: ""
ms.topic: "article"
dev_langs: 
  - "VB"
  - "CSharp"
  - "C++"
  - "ESQL"
ms.assetid: 818a505b-a196-41dd-aaac-2ccd5f7a2f1a
caps.latest.revision: 3
author: "JennieHubbard"
ms.author: "jhubbard"
manager: "jhubbard"
caps.handback.revision: 3
---
# Система типов (Entity SQL)
Язык [!INCLUDE[esql](../../../../../../includes/esql-md.md)] поддерживает следующие типы.  
  
-   Типы\-примитивы \(простые типы\), такие как `Int32` и `String.`  
  
-   Номинальные типы, которые определяются в схеме, например <xref:System.Data.Metadata.Edm.EntityType>, <xref:System.Data.Metadata.Edm.ComplexType> и <xref:System.Data.Metadata.Edm.RelationshipType>.  
  
-   Анонимные типы, которые явно не определяются в схеме: <xref:System.Data.Metadata.Edm.CollectionType>, <xref:System.Data.Metadata.Edm.RowType> и <xref:System.Data.Metadata.Edm.RefType>.  
  
 В этом разделе рассматриваются анонимные типы, которые не определяются в схеме явно, но поддерживаются языком [!INCLUDE[esql](../../../../../../includes/esql-md.md)].  Сведения о типах\-примитивах и номинальных типах см. в разделе [Conceptual Model Types \(CSDL\)](http://msdn.microsoft.com/ru-ru/987b995f-e429-4569-9559-b4146744def4).  
  
## Строки  
 Структура строки зависит от последовательности типизированных и именованных элементов, из которых состоит строка.  Тип строки не имеет идентификатора и не может наследоваться.  Экземпляры строк одинакового типа считаются равными, если равны соответствующие элементы этих строк.  Со строками не связаны операции, отличные от проверки равнозначности их структуры, к тому же они не имеют эквивалентов в среде CLR.  Запросы могут возвращать структуры, содержащие строки или коллекции строк.  API определения привязки между запросами [!INCLUDE[esql](../../../../../../includes/esql-md.md)] и базовым языком определяет способ реализации строк в запросе, вырабатывающем этот результат.  Сведения о создании экземпляра строки см. в разделе [Сборка типов](../../../../../../docs/framework/data/adonet/ef/language-reference/constructing-types-entity-sql.md).  
  
## Коллекции  
 Типы коллекций представляют ноль и более экземпляров других объектов.  Сведения о конструировании коллекций см. в разделе [Сборка типов](../../../../../../docs/framework/data/adonet/ef/language-reference/constructing-types-entity-sql.md).  
  
## Ссылки  
 Ссылка \- это логический указатель на отдельную сущность в определенном наборе сущностей.  
  
 Язык [!INCLUDE[esql](../../../../../../includes/esql-md.md)] поддерживает следующие операторы для конструирования, деконструирования и перехода по ссылкам.  
  
-   [REF](../../../../../../docs/framework/data/adonet/ef/language-reference/ref-entity-sql.md)  
  
-   [CREATEREF](../../../../../../docs/framework/data/adonet/ef/language-reference/createref-entity-sql.md)  
  
-   [KEY](../../../../../../docs/framework/data/adonet/ef/language-reference/key-entity-sql.md)  
  
-   [DEREF](../../../../../../docs/framework/data/adonet/ef/language-reference/deref-entity-sql.md)  
  
 По ссылке можно переходить с помощью оператора доступа к элементам «точка» \(`.`\).  В следующем фрагменте извлекается свойство Id \(объекта Order\) путем перехода по свойству r \(сокращение от reference\).  
  
```  
select o2.r.Id   
from (select ref(o) as r from LOB.Orders as o) as o2   
```  
  
 Если ссылка имеет значение NULL или цель ссылки не существует, результатом становится NULL.  
  
## См. также  
 [Общие сведения о языке Entity SQL](../../../../../../docs/framework/data/adonet/ef/language-reference/entity-sql-overview.md)   
 [Справочник по Entity SQL](../../../../../../docs/framework/data/adonet/ef/language-reference/entity-sql-reference.md)   
 [CAST](../../../../../../docs/framework/data/adonet/ef/language-reference/cast-entity-sql.md)   
 [Спецификации языка CSDL, SSDL и MSL](../../../../../../docs/framework/data/adonet/ef/language-reference/csdl-ssdl-and-msl-specifications.md)