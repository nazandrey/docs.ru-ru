---
title: "Извлечение XML-данных с помощью XPathNavigator | Microsoft Docs"
ms.custom: ""
ms.date: "03/30/2017"
ms.prod: ".net"
ms.reviewer: ""
ms.suite: ""
ms.technology: 
  - "dotnet-standard"
ms.tgt_pltfrm: ""
ms.topic: "article"
dev_langs: 
  - "VB"
  - "CSharp"
  - "C++"
  - "jsharp"
ms.assetid: 095b0987-ee4b-4595-a160-da1c956ad576
caps.latest.revision: 2
author: "mairaw"
ms.author: "mairaw"
manager: "wpickett"
caps.handback.revision: 2
---
# Извлечение XML-данных с помощью XPathNavigator
В платформе Microsoft .NET Framework есть несколько способов представления XML\-документа.  К ним относится использование класса <xref:System.String>, <xref:System.Xml.XmlReader>, <xref:System.Xml.XmlWriter>, <xref:System.Xml.XmlDocument> или <xref:System.Xml.XPath.XPathDocument>.  Чтобы ускорить перемещение между различными представлениями XML\-документа, в классе <xref:System.Xml.XPath.XPathNavigator> предусмотрено несколько методов и свойств для извлечения XML как объекта <xref:System.String>, <xref:System.Xml.XmlReader> или <xref:System.Xml.XmlWriter>.  
  
## Преобразование XPathNavigator в строку  
 Свойство <xref:System.Xml.XPath.XPathNavigator.OuterXml%2A> класса <xref:System.Xml.XPath.XPathNavigator> используется для получения разметки всего XML\-документа или просто разметки одного узла и его дочерних узлов.  
  
> [!NOTE]
>  Свойство <xref:System.Xml.XPath.XPathNavigator.InnerXml%2A> получает разметку или просто дочерние узлы данного узла.  
  
 В следующем примере кода показано, как сохранить целый XML\-документ, содержащийся в объекте <xref:System.Xml.XPath.XPathNavigator> как <xref:System.String>, а также один узел и его дочерние узлы.  
  
```vb  
Dim document As XPathDocument = New XPathDocument("input.xml")  
Dim navigator As XPathNavigator = document.CreateNavigator()  
  
' Save the entire input.xml document to a string.  
Dim xml As String = navigator.OuterXml  
  
' Now save the Root element and its child nodes to a string.  
navigator.MoveToChild(XPathNodeType.Element)  
Dim root As String = navigator.OuterXml  
```  
  
```csharp  
XPathDocument document = new XPathDocument("input.xml");  
XPathNavigator navigator = document.CreateNavigator();  
  
// Save the entire input.xml document to a string.  
string xml = navigator.OuterXml;  
  
// Now save the Root element and its child nodes to a string.  
navigator.MoveToChild(XPathNodeType.Element);  
string root = navigator.OuterXml;  
```  
  
## Преобразование XPathNavigator в XmlReader  
 Метод <xref:System.Xml.XPath.XPathNavigator.ReadSubtree%2A> используется для потоковой записи всего содержимого XML\-документа или отдельного его узла с дочерними узлами в объект <xref:System.Xml.XmlReader>.  
  
 Когда объект <xref:System.Xml.XmlReader> создан с текущим узлом и дочерними узлами, свойство <xref:System.Xml.XmlReader.ReadState%2A> объекта <xref:System.Xml.XmlReader> имеет значение <xref:System.Xml.ReadState>.  Когда метод <xref:System.Xml.XmlReader.Read%2A> объекта <xref:System.Xml.XmlReader> вызывается в первый раз, <xref:System.Xml.XmlReader> перемещается в текущий узел <xref:System.Xml.XPath.XPathNavigator>.  Новый объект <xref:System.Xml.XmlReader> продолжает считывание до тех пор, пока не будет достигнут конец XML\-дерева.  На этом этапе метод <xref:System.Xml.XmlReader.Read%2A> возвращает `false`, а свойство <xref:System.Xml.XmlReader.ReadState%2A> объекта <xref:System.Xml.XmlReader> имеет значение <xref:System.Xml.ReadState>.  
  
 Позиция объекта <xref:System.Xml.XPath.XPathNavigator> не изменяется при создании или перемещении объекта <xref:System.Xml.XmlReader>.  Метод <xref:System.Xml.XPath.XPathNavigator.ReadSubtree%2A> допустим только при размещении на элементе или в корневом узле.  
  
 Следующий пример показывает, как получить объект <xref:System.Xml.XmlReader>, содержащий весь XML\-документ в объекте <xref:System.Xml.XPath.XPathDocument>, так же как и для отдельного узла с дочерними узлами.  
  
```vb  
Dim document As XPathDocument = New XPathDocument("books.xml")  
Dim navigator As XPathNavigator = document.CreateNavigator()  
  
' Stream the entire XML document to the XmlReader.  
Dim xml As XmlReader = navigator.ReadSubtree()  
  
While xml.Read()  
    Console.WriteLine(xml.ReadInnerXml())  
End While  
  
xml.Close()  
  
' Stream the book element and its child nodes to the XmlReader.  
navigator.MoveToChild("bookstore", "")  
navigator.MoveToChild("book", "")  
  
Dim book As XmlReader = navigator.ReadSubtree()  
  
While book.Read()  
    Console.WriteLine(book.ReadInnerXml())  
End While  
  
book.Close()  
```  
  
```csharp  
XPathDocument document = new XPathDocument("books.xml");  
XPathNavigator navigator = document.CreateNavigator();  
  
// Stream the entire XML document to the XmlReader.  
XmlReader xml = navigator.ReadSubtree();  
  
while (xml.Read())  
{  
    Console.WriteLine(xml.ReadInnerXml());  
}  
  
xml.Close();  
  
// Stream the book element and its child nodes to the XmlReader.  
navigator.MoveToChild("bookstore", "");  
navigator.MoveToChild("book", "");  
  
XmlReader book = navigator.ReadSubtree();  
  
while (book.Read())  
{  
    Console.WriteLine(book.ReadInnerXml());  
}  
  
book.Close();  
```  
  
 В примере в качестве входных данных используется файл `books.xml`.  
  
 [!code-xml[XPathXMLExamples#1](../../../../samples/snippets/xml/VS_Snippets_Data/XPathXMLExamples/XML/books.xml#1)]  
  
## Преобразование XPathNavigator в XmlWriter  
 Метод <xref:System.Xml.XPath.XPathNavigator.WriteSubtree%2A> используется для потоковой записи всего содержимого XML\-документа или отдельного его узла с дочерними узлами в объект <xref:System.Xml.XmlWriter>.  
  
 Позиция объекта <xref:System.Xml.XPath.XPathNavigator> не изменяется при создании объекта <xref:System.Xml.XmlWriter>.  
  
 Следующий пример показывает, как получить объект <xref:System.Xml.XmlWriter>, содержащий весь XML\-документ в объекте <xref:System.Xml.XPath.XPathDocument>, так же как и для отдельного узла с дочерними узлами.  
  
```vb  
Dim document As XPathDocument = New XPathDocument("books.xml")  
Dim navigator As XPathNavigator = document.CreateNavigator()  
  
' Stream the entire XML document to the XmlWriter.  
Dim xml As XmlWriter = XmlWriter.Create("newbooks.xml")  
navigator.WriteSubtree(xml)  
xml.Close()  
  
' Stream the book element and its child nodes to the XmlWriter.  
navigator.MoveToChild("bookstore", "")  
navigator.MoveToChild("book", "")  
  
Dim book As XmlWriter = XmlWriter.Create("book.xml")  
navigator.WriteSubtree(book)  
book.Close()  
```  
  
```csharp  
XPathDocument document = new XPathDocument("books.xml");  
XPathNavigator navigator = document.CreateNavigator();  
  
// Stream the entire XML document to the XmlWriter.  
XmlWriter xml = XmlWriter.Create("newbooks.xml");  
navigator.WriteSubtree(xml);  
xml.Close();  
  
// Stream the book element and its child nodes to the XmlWriter.  
navigator.MoveToChild("bookstore", "");  
navigator.MoveToChild("book", "");  
  
XmlWriter book = XmlWriter.Create("book.xml");  
navigator.WriteSubtree(book);  
book.Close();  
```  
  
 В данном примере в качестве входного файла используется указанный выше файл `books.xml`.  
  
## См. также  
 <xref:System.Xml.XmlDocument>   
 <xref:System.Xml.XPath.XPathDocument>   
 <xref:System.Xml.XPath.XPathNavigator>   
 [Обработка XML\-данных с использованием модели данных XPath](../../../../docs/standard/data/xml/process-xml-data-using-the-xpath-data-model.md)   
 [Навигация в наборе узлов с помощью XPathNavigator](../../../../docs/standard/data/xml/node-set-navigation-using-xpathnavigator.md)   
 [Навигация по узлам атрибутов и пространств имен с помощью XPathNavigator](../../../../docs/standard/data/xml/attribute-and-namespace-node-navigation-using-xpathnavigator.md)   
 [Доступ к XML\-данным со строгой типизацией с помощью XPathNavigator](../../../../docs/standard/data/xml/accessing-strongly-typed-xml-data-using-xpathnavigator.md)