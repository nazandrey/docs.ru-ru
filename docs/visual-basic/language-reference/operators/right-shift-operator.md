---
title: "Оператор &#39;&#39;&gt;&gt;&#39;&#39; (Visual Basic) | Microsoft Docs"
ms.date: "2015-07-20"
ms.prod: ".net"
ms.reviewer: ""
ms.suite: ""
ms.technology: 
  - "devlang-visual-basic"
ms.topic: "article"
f1_keywords: 
  - "vb.>>"
dev_langs: 
  - "VB"
helpviewer_keywords: 
  - ">> - оператор [Visual Basic]"
  - "операторы сдвига разряда"
  - ">> - оператор"
  - ">> - оператор"
  - "операторы сдвига вправо"
ms.assetid: 054dc6a6-47d9-47ef-82da-cfa2b59fbf8f
caps.latest.revision: 14
author: "stevehoag"
ms.author: "shoag"
caps.handback.revision: 14
---
# Оператор &#39;&#39;&gt;&gt;&#39;&#39; (Visual Basic)
[!INCLUDE[vs2017banner](../../../visual-basic/includes/vs2017banner.md)]

Выполняется арифметический сдвиг битового шаблона вправо.  
  
## Синтаксис  
  
```  
  
result = pattern >> amount  
```  
  
## Части  
 `result`  
 Обязательный.  Целое числовое значение.  Результат сдвига битового шаблона.  Тип данных такой же, как для `pattern`.  
  
 `pattern`  
 Обязательный.  Целое числовое выражение.  Битовый шаблон, подлежащий сдвигу.  Данные должны быть целого типа \(`SByte`, `Byte`, `Short`, `UShort`, `Integer`, `UInteger`, `Long` или `ULong`\).  
  
 `amount`  
 Обязательный.  Числовое выражение.  Количество разрядов, на которое следует сдвинуть битовый шаблон.  Данные должны относиться к типу `Integer` или допускать расширение до типа `Integer`.  
  
## Заметки  
 Арифметические сдвиги не являются циклическими. Это означает, что биты, сдвинутые в один конец результата, не вводятся повторно в другой конец.  В арифметическом сдвиге вправо биты, сдвинутые за пределы крайней правой позиции, отбрасываются, а крайний левый \(знаковый\) бит передается в освободившиеся слева позиции битов.  Это значит, что если `pattern` имеет отрицательное значение, освободившиеся позиции задаются как единица; иначе они задаются как ноль.  
  
 Обратите внимание, что типы данных `Byte`, `UShort`, `UInteger` и `ULong` — это типы данных без знака, поэтому знаковый бит для передачи отсутствует.  Если `pattern` относится к какому\-либо типу без знака, освободившиеся позиции всегда обнуляются.  
  
 Чтобы предотвратить сдвиг на число битов, превышающее допустимое число битов результата, Visual Basic маскирует значение элемента `amount` с помощью маски размера, соответствующей типу данных элемента `pattern`.  Двоичное AND этих значений используется для задания величины сдвига.  Используются следующие маски размера:  
  
|Тип данных `pattern`|Маска размера \(десятичная\)|Маска размера \(шестнадцатеричная\)|  
|--------------------------|----------------------------------|-----------------------------------------|  
|`SByte`, `Byte`|7|&H00000007|  
|`Short`, `UShort`|15|&H0000000F|  
|`Integer`, `UInteger`|31|&H0000001F|  
|`Long`, `ULong`|63|&H0000003F|  
  
 Если `amount` равно нулю, значение из `result` совпадает со значением `pattern`.  Если `amount` имеет отрицательное значение, берется значение без знака и маскируется с помощью соответствующей маски размера.  
  
 В результате арифметического сдвига никогда не создаются исключения переполнения.  
  
## Перегрузка  
 Оператор `>>` может быть *перегружен*; это означает, что класс или структура может переопределить его поведение, если операнд имеет тип соответствующего класса или структуры.  Если в коде используется этот оператор для такого класса или структуры, убедитесь, что его переопределенное поведение вам понятно.  Дополнительные сведения см. в разделе [Процедуры операторов](../../../visual-basic/programming-guide/language-features/procedures/operator-procedures.md).  
  
## Пример  
 В приведенном ниже примере оператор `>>` используется для выполнения арифметических сдвигов целых значений вправо.  Результат всегда имеет тот же тип данных, что и выражение, для которого выполнен сдвиг.  
  
 [!code-vb[VbVbalrOperators#14](../../../visual-basic/language-reference/operators/codesnippet/VisualBasic/right-shift-operator_1.vb)]  
  
 В предыдущем примере получаются следующие результаты:  
  
-   `result1` равен 2560 \(0000 1010 0000 0000\).  
  
-   `result2` равен 160 \(0000 0000 1010 0000\).  
  
-   `result3` равен 2 \(0000 0000 0000 0010\).  
  
-   `result4` равен 640 \(0000 0010 1000 0000\).  
  
-   `result5` равен 0 \(сдвинут на 15 позиций вправо\).  
  
 Для `result4` сдвиг вычисляется как 18 AND 15, что равно 2.  
  
 В приведенном ниже примере показаны арифметические сдвиги отрицательных значений.  
  
 [!code-vb[VbVbalrOperators#55](../../../visual-basic/language-reference/operators/codesnippet/VisualBasic/right-shift-operator_2.vb)]  
  
 В предыдущем примере получаются следующие результаты:  
  
-   `negresult1` равен \-512 \(1111 1110 0000 0000\).  
  
-   `negresult2` равен \-1 \(знаковый бит передан\).  
  
## См. также  
 [Операторы поразрядного сдвига](../../../visual-basic/language-reference/operators/bit-shift-operators.md)   
 [Операторы присваивания](../../../visual-basic/language-reference/operators/assignment-operators.md)   
 [Оператор \>\>\=](../../../visual-basic/language-reference/operators/right-shift-assignment-operator.md)   
 [Порядок применения операторов в Visual Basic](../../../visual-basic/language-reference/operators/operator-precedence.md)   
 [Список операторов, сгруппированных по функциональному назначению](../../../visual-basic/language-reference/operators/operators-listed-by-functionality.md)   
 [Арифметические операторы в Visual Basic](../../../visual-basic/programming-guide/language-features/operators-and-expressions/arithmetic-operators.md)