---
title: "Лямбда-выражение не будет удалено из этого обработчика событий | Документы Microsoft"
ms.date: 2015-07-20
ms.prod: .net
ms.reviewer: 
ms.suite: 
ms.technology:
- devlang-visual-basic
ms.topic: article
f1_keywords:
- bc42326
- vbc42326
dev_langs:
- VB
helpviewer_keywords:
- BC42326
ms.assetid: 63214dc6-0112-4245-8ebf-7c9e8f5a5782
caps.latest.revision: 8
author: dotnet-bot
ms.author: dotnetcontent
translation.priority.ht:
- cs-cz
- de-de
- es-es
- fr-fr
- it-it
- ja-jp
- ko-kr
- pl-pl
- pt-br
- ru-ru
- tr-tr
- zh-cn
- zh-tw
translationtype: Machine Translation
ms.sourcegitcommit: a06bd2a17f1d6c7308fa6337c866c1ca2e7281c0
ms.openlocfilehash: bdf7ad8f8a116c818e72d67150d72d0c96a4dc3b
ms.lasthandoff: 03/13/2017

---
# <a name="lambda-expression-will-not-be-removed-from-this-event-handler"></a>Лямбда-выражение не будет удалено из этого обработчика событий
Лямбда-выражение не будет удалено из этого обработчика событий. Назначьте лямбда-выражение переменной и использовать переменную для добавления и удаления события.  
  
 При использовании лямбда-выражения с обработчиками событий, ожидаемое поведение может отсутствовать. Компилятор создает новый метод для каждого определения лямбда-выражения, даже если они идентичны. Таким образом, следующий код отображает `False`.  
  
```vb  
Module Module1  
  
    Sub Main()  
        Dim fun1 As ChangeInteger = Function(p As Integer) p + 1  
        Dim fun2 As ChangeInteger = Function(p As Integer) p + 1  
        Console.WriteLine(fun1 = fun2)  
    End Sub  
  
    Delegate Function ChangeInteger(ByVal x As Integer) As Integer  
  
End Module  
```  
  
 При использовании лямбда-выражения с обработчиками событий, это может привести к непредвиденным результатам. В следующем примере лямбда-выражение добавленные `AddHandler` не удаляется с `RemoveHandler` инструкции.  
  
```vb  
Module Module1  
  
    Event ProcessInteger(ByVal x As Integer)  
  
    Sub Main()  
  
        ' The following line adds one listener to the event.  
        AddHandler ProcessInteger, Function(m As Integer) m  
  
        ' The following statement searches the current listeners   
        ' for a match to remove. However, this lambda is not the same  
        ' as the previous one, so nothing is removed.  
        RemoveHandler ProcessInteger, Function(m As Integer) m  
  
    End Sub  
End Module  
```  
  
 По умолчанию данное сообщение является предупреждением. Дополнительные сведения о том, как скрыть предупреждения или как рассматривать предупреждения как ошибки в разделе [в Visual Basic](https://docs.microsoft.com/visualstudio/ide/configuring-warnings-in-visual-basic).  
  
 **Идентификатор ошибки:** BC42326  
  
## <a name="to-correct-this-error"></a>Исправление ошибки  
  
-   Чтобы избежать предупреждения и удалить лямбда-выражение, назначьте лямбда-выражение переменной и использовать переменную в обоих `AddHandler` и `RemoveHandler` инструкции, как показано в следующем примере.  
  
```vb  
Module Module1  
  
    Event ProcessInteger(ByVal x As Integer)  
  
    Dim PrintHandler As ProcessIntegerEventHandler  
  
    Sub Main()  
  
        ' Assign the lambda expression to a variable.  
        PrintHandler = Function(m As Integer) m  
  
        ' Use the variable to add the listener.  
        AddHandler ProcessInteger, PrintHandler  
  
        ' Use the variable again when you want to remove the listener.  
        RemoveHandler ProcessInteger, PrintHandler  
  
    End Sub  
End Module  
```  
  
## <a name="see-also"></a>См. также  
 [Лямбда-выражения](../../../visual-basic/programming-guide/language-features/procedures/lambda-expressions.md)   
 [Неявное преобразование делегата](../../../visual-basic/programming-guide/language-features/delegates/relaxed-delegate-conversion.md)   
 [События](../../../visual-basic/programming-guide/language-features/events/index.md)
