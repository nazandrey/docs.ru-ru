---
title: "Строки стандартных числовых форматов | Microsoft Docs"
ms.custom: ""
ms.date: "03/30/2017"
ms.prod: ".net"
ms.reviewer: ""
ms.suite: ""
ms.technology: 
  - "dotnet-standard"
ms.tgt_pltfrm: ""
ms.topic: "article"
helpviewer_keywords: 
  - "описатели формата, числовой"
  - "описатели формата, строки стандартных числовых форматов"
  - "строки формата"
  - "форматирование [платформа .NET Framework], числа"
  - "форматирование чисел [платформа .NET Framework]"
  - "числа [платформа .NET Framework], форматирование"
  - "строки числовых форматов [платформа .NET Framework]"
  - "строки стандартного формата, числовой"
  - "строки стандартных числовых форматов"
ms.assetid: 580e57eb-ac47-4ffd-bccd-3a1637c2f467
caps.latest.revision: 99
author: "rpetrusha"
ms.author: "ronpet"
manager: "wpickett"
caps.handback.revision: 95
---
# Строки стандартных числовых форматов
Строки стандартных числовых форматов служат для форматирования стандартных числовых типов. Строка стандартных числовых форматов использует формат `Axx`, где:  
  
-   `A` — это один буквенный символ, который называют *описателем формата*. Любая строка числового формата, содержащая более одной буквы, включая пробелы, интерпретируется как строка настраиваемого числового формата. Для получения дополнительной информации см. [Строки настраиваемых числовых форматов](../../../docs/standard/base-types/custom-numeric-format-strings.md).  
  
-   `xx` — это необязательное целое число, которое называют *описателем точности*. Спецификатор точности находится в диапазоне от 0 до 99 и влияет на число цифр в результате. Описатель точности управляет количеством цифр в строковом представлении числа. Он не округляет само число. Для выполнения операции округления используйте метод <xref:System.Math.Ceiling%2A?displayProperty=fullName>, <xref:System.Math.Floor%2A?displayProperty=fullName> или <xref:System.Math.Round%2A?displayProperty=fullName>.  
  
     Если *описатель точности* определяет число цифр дробной части в итоговой строке, в итоговых строках содержатся числа, округляемые в большую сторону \(то есть с использованием <xref:System.MidpointRounding?displayProperty=fullName>\).  
  
 Строки стандартного числового формата поддерживаются некоторыми перегрузками метода `ToString` всех числовых типов. Например, можно задать строку числового формата для методов <xref:System.Int32.ToString%28System.String%29> и <xref:System.Int32.ToString%28System.String%2CSystem.IFormatProvider%29> типа <xref:System.Int32>. Строки стандартного числового формата также поддерживаются [функцией составного форматирования](../../../docs/standard/base-types/composite-formatting.md) .NET Framework, используемой некоторыми методами `Write` и `WriteLine` классов <xref:System.Console> и <xref:System.IO.StreamWriter>, методом <xref:System.String.Format%2A?displayProperty=fullName>, а также методом <xref:System.Text.StringBuilder.AppendFormat%2A?displayProperty=fullName>. Композитный формат позволяет включить строковое представление нескольких элементов данных в одну строку, чтобы задать ширину поля и выровнять числа в поле. Для получения дополнительной информации см. [Составное форматирование](../../../docs/standard/base-types/composite-formatting.md).  
  
> [!TIP]
>  Вы можете загрузить [служебную программу форматирования](http://code.msdn.microsoft.com/NET-Framework-4-Formatting-9c4dae8d) — приложение, позволяющее применять строки формата к значениям даты и времени и числовым значениям и отображающее результирующую строку.  
  
<a name="table"></a> В следующей таблице описаны спецификаторы стандартных числовых форматов и отображены примеры выходных данных, производимых каждым спецификатором формата. Дополнительные сведения о использовании строк стандартных числовых форматов см. в разделе [Примечания](#NotesStandardFormatting); обширную демонстрацию их использования см. в разделе [Пример](#example).  
  
|Описатель формата|Имя|Описание|Примеры|  
|-----------------------|---------|--------------|-------------|  
|"C" или "c"|Валюта|Результат: значение валюты.<br /><br /> Поддерживается: всеми числовыми типами данных.<br /><br /> Описатель точности: количество цифр в дробной части.<br /><br /> Описатель точности по умолчанию: определяется <xref:System.Globalization.NumberFormatInfo.CurrencyDecimalDigits%2A?displayProperty=fullName>.<br /><br /> Дополнительные сведения: [Описатель формата валюты \("C"\)](#CFormatString).|123.456 \("C", en\-US\) \-\> $123.46<br /><br /> 123.456 \("C", fr\-FR\) \-\> 123,46 €<br /><br /> 123.456 \("C", ja\-JP\) \-\> ¥123<br /><br /> \-123.456 \("C3", en\-US\) \-\> \($123.456\)<br /><br /> \-123.456 \("C3", fr\-FR\) \-\> \-123,456 €<br /><br /> \-123.456 \("C3", ja\-JP\) \-\> \-¥123.456|  
|"D" или "d"|Десятичное число|Результат: целочисленные цифры с необязательным отрицательным знаком.<br /><br /> Поддерживается: только целочисленными типами данных.<br /><br /> Описатель точности: минимальное число цифр.<br /><br /> Описатель точности по умолчанию: минимальное необходимое число цифр.<br /><br /> Дополнительные сведения: [Описатель десятичного формата \("D"\)](#DFormatString).|1234 \("D"\) \-\> 1234<br /><br /> \-1234 \("D6"\) \-\> \-001234|  
|"E" или "e"|Экспоненциальный \(научный\)|Результат: экспоненциальная нотация.<br /><br /> Поддерживается: всеми числовыми типами данных.<br /><br /> Описатель точности: количество цифр дробной части.<br /><br /> Описатель точности по умолчанию: 6.<br /><br /> Дополнительные сведения: [Описатель экспоненциального формата \("E"\)](#EFormatString).|1052.0329112756 \("E", en\-US\) \-\> 1.052033E\+003<br /><br /> 1052.0329112756 \("e", fr\-FR\) \-\> 1,052033e\+003<br /><br /> \-1052.0329112756 \("e2", en\-US\) \-\> \-1.05e\+003<br /><br /> \-1052.0329112756 \("E2", fr\_FR\) \-\> \-1,05E\+003|  
|"F" или "f"|С фиксированной запятой|Результат: цифры целой и дробной частей с необязательным отрицательным знаком.<br /><br /> Поддерживается: всеми числовыми типами данных.<br /><br /> Описатель точности: количество цифр в дробной части.<br /><br /> Описатель точности по умолчанию: определяется <xref:System.Globalization.NumberFormatInfo.NumberDecimalDigits%2A?displayProperty=fullName>.<br /><br /> Дополнительные сведения: [Описатель формата с фиксированной запятой \("F"\)](#FFormatString).|1234.567 \("F", en\-US\) \-\> 1234.57<br /><br /> 1234.567 \("F", de\-DE\) \-\> 1234,57<br /><br /> 1234 \("F1", en\-US\) \-\> 1234.0<br /><br /> 1234 \("F1", de\-DE\) \-\> 1234,0<br /><br /> \-1234.56 \("F4", en\-US\) \-\> \-1234.5600<br /><br /> \-1234.56 \("F4", de\-DE\) \-\> \-1234,5600|  
|"G" или "g"|Общие правила|Результат: наиболее компактная запись из двух вариантов: экспоненциального и с фиксированной запятой.<br /><br /> Поддерживается: всеми числовыми типами данных.<br /><br /> Описатель точности: количество значащих цифр.<br /><br /> Описатель точности по умолчанию: определяется численным типом.<br /><br /> Дополнительные сведения: [Описатель общего формата \("G"\)](#GFormatString).|\-123.456 \("G", en\-US\) \-\> \-123.456<br /><br /> \-123.456 \("G", sv\-SE\) \-\> \-123,456<br /><br /> 123.4546 \("G4", en\-US\) \-\> 123.5<br /><br /> 123.4546 \("G4", sv\-SE\) \-\> 123,5<br /><br /> \-1.234567890e\-25 \("G", en\-US\) \-\> \-1.23456789E\-25<br /><br /> \-1.234567890e\-25 \("G", sv\-SE\) \-\> \-1,23456789E\-25|  
|"N" или "n"|Числовой|Результат: цифры целой и дробной частей, разделители групп и разделитель целой и дробной частей с необязательным отрицательным знаком.<br /><br /> Поддерживается: всеми числовыми типами данных.<br /><br /> Описатель точности: желаемое число знаков дробной части.<br /><br /> Описатель точности по умолчанию: определяется <xref:System.Globalization.NumberFormatInfo.NumberDecimalDigits%2A?displayProperty=fullName>.<br /><br /> Дополнительные сведения: [Описатель числового формата \("N"\)](#NFormatString).|1234.567 \("N", en\-US\) \-\> 1,234.57<br /><br /> 1234.567 \("N", ru\-RU\) \-\> 1 234,57<br /><br /> 1234 \("N1", en\-US\) \-\> 1,234.0<br /><br /> 1234 \("N1", ru\-RU\) \-\> 1 234,0<br /><br /> \-1234.56 \("N3", en\-US\) \-\> \-1,234.560<br /><br /> \-1234.56 \("N3", ru\-RU\) \-\> \-1 234,560|  
|"P" или "p"|Процент|Результат: число, умноженное на 100 и отображаемое с символом процента.<br /><br /> Поддерживается: всеми числовыми типами данных.<br /><br /> Описатель точности: желаемое число знаков дробной части.<br /><br /> Описатель точности по умолчанию: определяется <xref:System.Globalization.NumberFormatInfo.PercentDecimalDigits%2A?displayProperty=fullName>.<br /><br /> Дополнительные сведения: [Описатель формата процента \("P"\)](#PFormatString).|1 \("P", en\-US\) \-\> 100.00 %<br /><br /> 1 \("P", fr\-FR\) \-\> 100,00 %<br /><br /> \-0.39678 \("P1", en\-US\) \-\> \-39.7 %<br /><br /> \-0.39678 \("P1", fr\-FR\) \-\> \-39,7 %|  
|"R" или "r"|Приемо\-передача|Результат: строка, дающая при обратном преобразовании идентичное число.<br /><br /> Поддерживается: <xref:System.Single>, <xref:System.Double> и <xref:System.Numerics.BigInteger>.<br /><br /> Описатель точности: игнорируется.<br /><br /> Дополнительные сведения: [Описатель формата обратного преобразования \("R"\)](#RFormatString).|123456789.12345678 \("R"\) \-\> 123456789.12345678<br /><br /> \-1234567890.12345678 \("R"\) \-\> \-1234567890.1234567|  
|"X" или "x"|Шестнадцатеричный|Результат: шестнадцатеричная строка.<br /><br /> Поддерживается: только целочисленными типами данных.<br /><br /> Описатель точности: число цифр в результирующей строке.<br /><br /> Дополнительные сведения: [Описатель шестнадцатеричного формата \("X"\)](#XFormatString).|255 \("X"\) \-\> FF<br /><br /> \-1 \("x"\) \-\> ff<br /><br /> 255 \("x4"\) \-\> 00ff<br /><br /> \-1 \("X4"\) \-\> 00FF|  
|Любой другой символ|Неизвестный описатель|Результат: порождение исключения <xref:System.FormatException> во время выполнения.||  
  
<a name="Using"></a>   
## Использование строк стандартных числовых форматов  
 Строку стандартного числового формата можно использовать для определения форматирования числового значения одним из двух следующих способов:  
  
-   Ее можно передать перегруженному методу `ToString`, у которого есть параметр `format`. В следующем примере осуществляется форматирование числового значения в качестве строки со значением валюты с использованием текущего языка и региональных параметров \(в примере это "en\-US"\).  
  
     [!code-cpp[Formatting.Numeric.Standard#10](../../../samples/snippets/cpp/VS_Snippets_CLR/Formatting.Numeric.Standard/cpp/standardusage1.cpp#10)]
     [!code-csharp[Formatting.Numeric.Standard#10](../../../samples/snippets/csharp/VS_Snippets_CLR/Formatting.Numeric.Standard/cs/standardusage1.cs#10)]
     [!code-vb[Formatting.Numeric.Standard#10](../../../samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.Numeric.Standard/vb/standardusage1.vb#10)]  
  
-   Эту строку можно передать в качестве аргумента `formatString` в элемент форматирования, используемый с такими методами, как <xref:System.String.Format%2A?displayProperty=fullName>, <xref:System.Console.WriteLine%2A?displayProperty=fullName> и <xref:System.Text.StringBuilder.AppendFormat%2A?displayProperty=fullName>. Дополнительные сведения см. в разделе [Составное форматирование](../../../docs/standard/base-types/composite-formatting.md). В следующем примере элемент форматирования используется для вставки значения валюты в строку.  
  
     [!code-cpp[Formatting.Numeric.Standard#11](../../../samples/snippets/cpp/VS_Snippets_CLR/Formatting.Numeric.Standard/cpp/standardusage1.cpp#11)]
     [!code-csharp[Formatting.Numeric.Standard#11](../../../samples/snippets/csharp/VS_Snippets_CLR/Formatting.Numeric.Standard/cs/standardusage1.cs#11)]
     [!code-vb[Formatting.Numeric.Standard#11](../../../samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.Numeric.Standard/vb/standardusage1.vb#11)]  
  
     При необходимости можно предоставить аргумент `alignment`, чтобы указать ширину числового поля и его выравнивание по правому или левому краю. В следующем примере денежное значение в поле длиной 28 символов выравнивается по левому краю, а денежное значение в поле длиной 14 символов выравнивается по правому краю.  
  
     [!code-cpp[Formatting.Numeric.Standard#12](../../../samples/snippets/cpp/VS_Snippets_CLR/Formatting.Numeric.Standard/cpp/standardusage1.cpp#12)]
     [!code-csharp[Formatting.Numeric.Standard#12](../../../samples/snippets/csharp/VS_Snippets_CLR/Formatting.Numeric.Standard/cs/standardusage1.cs#12)]
     [!code-vb[Formatting.Numeric.Standard#12](../../../samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.Numeric.Standard/vb/standardusage1.vb#12)]  
  
 В приведенных ниже разделах содержится подробная информация о всех строках стандартных числовых форматов.  
  
<a name="CFormatString"></a>   
## Описатель формата валюты \("C"\)  
 При использовании описателя формата валюты \("C"\) число преобразуется в строку, представляющую сумму в некоторой валюте. Желаемое количество знаков дробной части в результирующей строке задается описателем точности. Если описатель точности не задан, точность по умолчанию определяется свойством <xref:System.Globalization.NumberFormatInfo.CurrencyDecimalDigits%2A?displayProperty=fullName>.  
  
 Если форматируемое значение содержит больше десятичных знаков, чем задано или возможно по умолчанию, в результирующей строке дробное значение округляется. Если значение справа от заданного числа десятичных знаков больше или равно 5, последний знак в результирующей строке округляется в сторону от нуля.  
  
 Форматирование результирующей строки определяется сведениями о форматировании в текущем объекте <xref:System.Globalization.NumberFormatInfo>. В следующей таблице представлены свойства <xref:System.Globalization.NumberFormatInfo>, обеспечивающие управление форматированием возвращаемой строки.  
  
|Свойство NumberFormatInfo|Описание|  
|-------------------------------|--------------|  
|<xref:System.Globalization.NumberFormatInfo.CurrencyPositivePattern%2A>|Определяет положение символа валюты в положительных значениях.|  
|<xref:System.Globalization.NumberFormatInfo.CurrencyNegativePattern%2A>|Определяет положение символа валюты в отрицательных значениях и указывает, как именно представляется отрицательный знак: круглыми скобками или свойством <xref:System.Globalization.NumberFormatInfo.NegativeSign%2A>.|  
|<xref:System.Globalization.NumberFormatInfo.NegativeSign%2A>|Задает отрицательный знак, используемый в случае, если свойство <xref:System.Globalization.NumberFormatInfo.CurrencyNegativePattern%2A> указывает на то, что скобки для отрицания не используются.|  
|<xref:System.Globalization.NumberFormatInfo.CurrencySymbol%2A>|Определяет символ валюты.|  
|<xref:System.Globalization.NumberFormatInfo.CurrencyDecimalDigits%2A>|Определяет количество цифр дробной части в значении валюты по умолчанию. Это значение можно переопределить с помощью описателя точности.|  
|<xref:System.Globalization.NumberFormatInfo.CurrencyDecimalSeparator%2A>|Определяет строку, разделяющую целую и дробную части числа.|  
|<xref:System.Globalization.NumberFormatInfo.CurrencyGroupSeparator%2A>|Определяет строку, разделяющую группы цифр целой части.|  
|<xref:System.Globalization.NumberFormatInfo.CurrencyGroupSizes%2A>|Определяет число целочисленных цифр, входящих в группу.|  
  
 В следующем примере значение <xref:System.Double> форматируется с помощью спецификатора денежного формата.  
  
 [!code-cpp[Formatting.Numeric.Standard#1](../../../samples/snippets/cpp/VS_Snippets_CLR/Formatting.Numeric.Standard/cpp/Standard.cpp#1)]
 [!code-csharp[Formatting.Numeric.Standard#1](../../../samples/snippets/csharp/VS_Snippets_CLR/Formatting.Numeric.Standard/cs/Standard.cs#1)]
 [!code-vb[Formatting.Numeric.Standard#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.Numeric.Standard/vb/Standard.vb#1)]  
  
 [К таблице](#table)  
  
<a name="DFormatString"></a>   
## Описатель десятичного формата \("D"\)  
 При использовании описателя десятичного формата \("D"\) число преобразуется в строку, состоящую из десятичных цифр \(0–9\); если число отрицательное, перед ним ставится отрицательный знак. Этот формат доступен только для целых типов.  
  
 Минимальное количество знаков в выходной строке задается спецификатором точности. Недостающие знаки в строке заменяются нулями. Если описатель точности не задан, по умолчанию используется минимальное значение, позволяющее представить целое число без нулей в начале.  
  
 Форматирование результирующей строки определяется сведениями о форматировании в текущем объекте <xref:System.Globalization.NumberFormatInfo>. Как показано в следующей таблице, управление форматированием результирующей строки осуществляется с помощью одного свойства.  
  
|Свойство NumberFormatInfo|Описание|  
|-------------------------------|--------------|  
|<xref:System.Globalization.NumberFormatInfo.NegativeSign%2A>|Определяет строку, указывающую, что число является отрицательным.|  
  
 В следующем примере значение <xref:System.Int32> форматируется с помощью описателя десятичного формата.  
  
 [!code-cpp[Formatting.Numeric.Standard#2](../../../samples/snippets/cpp/VS_Snippets_CLR/Formatting.Numeric.Standard/cpp/Standard.cpp#2)]
 [!code-csharp[Formatting.Numeric.Standard#2](../../../samples/snippets/csharp/VS_Snippets_CLR/Formatting.Numeric.Standard/cs/Standard.cs#2)]
 [!code-vb[Formatting.Numeric.Standard#2](../../../samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.Numeric.Standard/vb/Standard.vb#2)]  
  
 [К таблице](#table)  
  
<a name="EFormatString"></a>   
## Описатель экспоненциального формата \("E"\)  
 При использовании описателя экспоненциального формата \("E"\) число преобразуется в строку вида "\-d.ddd…E\+ddd" или "\-d.ddd…e\+ddd", где каждый символ "d" обозначает цифру \(0–9\). Если число отрицательное, в начале строки ставится отрицательный знак. Перед разделителем целой и дробной части всегда стоит ровно одна цифра.  
  
 Требуемое число знаков дробной части задается спецификатором точности. Если спецификатор точности отсутствует, по умолчанию число знаков дробной части равно шести.  
  
 Регистр описателя формата задает регистр буквы, стоящей перед экспонентой \("E" или "e"\). Экспонента состоит из знака "плюс" или "минус" и трех цифр. Недостающие до минимума цифры заменяются нулями, если это необходимо.  
  
 Форматирование результирующей строки определяется сведениями о форматировании в текущем объекте <xref:System.Globalization.NumberFormatInfo>. В следующей таблице представлены свойства <xref:System.Globalization.NumberFormatInfo>, обеспечивающие управление форматированием возвращаемой строки.  
  
|Свойство NumberFormatInfo|Описание|  
|-------------------------------|--------------|  
|<xref:System.Globalization.NumberFormatInfo.NegativeSign%2A>|Определяет строку, указывающую на то, что число является отрицательным \(как мантисса, так и экспонента\).|  
|<xref:System.Globalization.NumberFormatInfo.NumberDecimalSeparator%2A>|Определяет строку, разделяющую целую и дробную части мантиссы.|  
|<xref:System.Globalization.NumberFormatInfo.PositiveSign%2A>|Определяет строку, указывающую, что экспонента является положительной.|  
  
 В следующем примере значение <xref:System.Double> форматируется с помощью описателя экспоненциального формата.  
  
 [!code-cpp[Formatting.Numeric.Standard#3](../../../samples/snippets/cpp/VS_Snippets_CLR/Formatting.Numeric.Standard/cpp/Standard.cpp#3)]
 [!code-csharp[Formatting.Numeric.Standard#3](../../../samples/snippets/csharp/VS_Snippets_CLR/Formatting.Numeric.Standard/cs/Standard.cs#3)]
 [!code-vb[Formatting.Numeric.Standard#3](../../../samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.Numeric.Standard/vb/Standard.vb#3)]  
  
 [К таблице](#table)  
  
<a name="FFormatString"></a>   
## Описатель формата с фиксированной запятой \("F"\)  
 При использовании спецификатора формата с фиксированной точкой \("F"\) число преобразуется в строку вида "\-ddd.ddd…", где каждое "d" обозначает цифру \(0–9\). Если число отрицательное, в начале строки ставится отрицательный знак.  
  
 Требуемое число знаков дробной части задается спецификатором точности. Если описатель точности отсутствует, то используется численная точность, определяемая текущим значением свойства <xref:System.Globalization.NumberFormatInfo.NumberDecimalDigits%2A?displayProperty=fullName>.  
  
 Форматирование результирующей строки определяется сведениями о форматировании в текущем объекте <xref:System.Globalization.NumberFormatInfo>. В следующей таблице представлены свойства объекта <xref:System.Globalization.NumberFormatInfo>, обеспечивающие управление форматированием результирующей строки.  
  
|Свойство NumberFormatInfo|Описание|  
|-------------------------------|--------------|  
|<xref:System.Globalization.NumberFormatInfo.NegativeSign%2A>|Определяет строку, указывающую, что число является отрицательным.|  
|<xref:System.Globalization.NumberFormatInfo.NumberDecimalSeparator%2A>|Определяет строку, разделяющую целую и дробную части числа.|  
|<xref:System.Globalization.NumberFormatInfo.NumberDecimalDigits%2A>|Определяет количество цифр дробной части по умолчанию. Это значение можно переопределить с помощью описателя точности.|  
  
 В следующем примере значение <xref:System.Double> и значение <xref:System.Int32> форматируются с помощью спецификатора формата с фиксированной точкой.  
  
 [!code-cpp[Formatting.Numeric.Standard#4](../../../samples/snippets/cpp/VS_Snippets_CLR/Formatting.Numeric.Standard/cpp/Standard.cpp#4)]
 [!code-csharp[Formatting.Numeric.Standard#4](../../../samples/snippets/csharp/VS_Snippets_CLR/Formatting.Numeric.Standard/cs/Standard.cs#4)]
 [!code-vb[Formatting.Numeric.Standard#4](../../../samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.Numeric.Standard/vb/Standard.vb#4)]  
  
 [К таблице](#table)  
  
<a name="GFormatString"></a>   
## Описатель общего формата \("G"\)  
 При использовании описателя общего формата \("G"\) число преобразуется в наиболее короткий из двух вариантов: запись с фиксированной запятой или экспоненциальная запись. При этом учитывается тип числа и наличие описателя точности. Описатель точности определяет максимальное количество значащих цифр, которые могут быть использованы в результирующей строке. Если описатель точности не задан или равен нулю, точность определяется типом числа, как показано в следующей таблице.  
  
|Числовой тип|Точность по умолчанию|  
|------------------|---------------------------|  
|<xref:System.Byte> или <xref:System.SByte>|3 знака|  
|<xref:System.Int16> или <xref:System.UInt16>|5 знака|  
|<xref:System.Int32> или <xref:System.UInt32>|10 знака|  
|<xref:System.Int64>|19 знака|  
|<xref:System.UInt64>|20 знака|  
|<xref:System.Numerics.BigInteger>|50 знака|  
|<xref:System.Single>|7 знака|  
|<xref:System.Double>|15 знака|  
|<xref:System.Decimal>|29 знака|  
  
 Нотация с фиксированной запятой используется, если экспонента результата в экспоненциальной нотации длиннее пяти знаков, но меньше спецификатора точности, в противном случае используется научная нотация. При необходимости результат содержит разделитель целой и дробной частей; нули в конце дробной части после разделителя отбрасываются. Если описатель точности задан и число значащих цифр результата превосходит указанное значение точности, лишние цифры отбрасываются округлением.  
  
 Тем не менее, если число относится к типу <xref:System.Decimal> и описатель точности не задан, всегда используется нотация с фиксированной запятой, а нули в конце не отбрасываются.  
  
 Если используется экспоненциальная нотация, регистр буквы, стоящей перед экспонентой, определяется регистром описателя формата \(буква "E" соответствует "G", "e" соответствует "g"\). Экспонента содержит не менее двух цифр. Это отличает данный формат от экспоненциальной записи, создаваемой при использовании описателя экспоненциального формата, поскольку в последнем случае экспонента содержит не менее трех цифр.  
  
 Форматирование результирующей строки определяется сведениями о форматировании в текущем объекте <xref:System.Globalization.NumberFormatInfo>. В следующей таблице представлены свойства <xref:System.Globalization.NumberFormatInfo>, обеспечивающие управление форматированием результирующей строки.  
  
|Свойство NumberFormatInfo|Описание|  
|-------------------------------|--------------|  
|<xref:System.Globalization.NumberFormatInfo.NegativeSign%2A>|Определяет строку, указывающую, что число является отрицательным.|  
|<xref:System.Globalization.NumberFormatInfo.NumberDecimalSeparator%2A>|Определяет строку, разделяющую целую и дробную части числа.|  
|<xref:System.Globalization.NumberFormatInfo.PositiveSign%2A>|Определяет строку, указывающую, что экспонента является положительной.|  
  
 В следующем примере различные значения с плавающей запятой форматируются с помощью спецификатора общего формата.  
  
 [!code-cpp[Formatting.Numeric.Standard#5](../../../samples/snippets/cpp/VS_Snippets_CLR/Formatting.Numeric.Standard/cpp/Standard.cpp#5)]
 [!code-csharp[Formatting.Numeric.Standard#5](../../../samples/snippets/csharp/VS_Snippets_CLR/Formatting.Numeric.Standard/cs/Standard.cs#5)]
 [!code-vb[Formatting.Numeric.Standard#5](../../../samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.Numeric.Standard/vb/Standard.vb#5)]  
  
 [К таблице](#table)  
  
<a name="NFormatString"></a>   
## Описатель числового формата \("N"\)  
 Спецификатор числового формата \("N"\) преобразует число в стоку вида "\-d,ddd,ddd.ddd… ", где знак "\-" при необходимости представляет знак отрицательного числа, знак "d" означает цифру \(0\-9\), знак "," — разделитель групп, а знак "." —\- разделитель целой и дробной части. Требуемое число знаков дробной части задается спецификатором точности. Если описатель точности не задан, число десятичных знаков определяется текущим свойством <xref:System.Globalization.NumberFormatInfo.NumberDecimalDigits%2A?displayProperty=fullName>.  
  
 Форматирование результирующей строки определяется сведениями о форматировании в текущем объекте <xref:System.Globalization.NumberFormatInfo>. В следующей таблице представлены свойства <xref:System.Globalization.NumberFormatInfo>, обеспечивающие управление форматированием результирующей строки.  
  
|Свойство NumberFormatInfo|Описание|  
|-------------------------------|--------------|  
|<xref:System.Globalization.NumberFormatInfo.NegativeSign%2A>|Определяет строку, указывающую, что число является отрицательным.|  
|<xref:System.Globalization.NumberFormatInfo.NumberNegativePattern%2A>|Определяет формат отрицательных значений и указывает, как именно представляется отрицательный знак: круглыми скобками или свойством <xref:System.Globalization.NumberFormatInfo.NegativeSign%2A>.|  
|<xref:System.Globalization.NumberFormatInfo.NumberGroupSizes%2A>|Определяет число цифр целой части, стоящих между разделителями групп.|  
|<xref:System.Globalization.NumberFormatInfo.NumberGroupSeparator%2A>|Определяет строку, разделяющую группы цифр целой части.|  
|<xref:System.Globalization.NumberFormatInfo.NumberDecimalSeparator%2A>|Определяет строку, разделяющую целую и дробную части числа.|  
|<xref:System.Globalization.NumberFormatInfo.NumberDecimalDigits%2A>|Определяет количество цифр дробной части по умолчанию. Это значение можно переопределить с помощью описателя точности.|  
  
 В следующем примере различные значения с плавающей запятой форматируются с помощью спецификатора числового формата.  
  
 [!code-cpp[Formatting.Numeric.Standard#6](../../../samples/snippets/cpp/VS_Snippets_CLR/Formatting.Numeric.Standard/cpp/Standard.cpp#6)]
 [!code-csharp[Formatting.Numeric.Standard#6](../../../samples/snippets/csharp/VS_Snippets_CLR/Formatting.Numeric.Standard/cs/Standard.cs#6)]
 [!code-vb[Formatting.Numeric.Standard#6](../../../samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.Numeric.Standard/vb/Standard.vb#6)]  
  
 [К таблице](#table)  
  
<a name="PFormatString"></a>   
## Описатель формата процента \("P"\)  
 При использовании описателя формата процента \("P"\) число умножается на 100 и преобразуется в строку, представляющую процентную долю. Требуемое число знаков дробной части задается спецификатором точности. Если описатель точности отсутствует, то используется значение точности числа по умолчанию, заданное свойством <xref:System.Globalization.NumberFormatInfo.PercentDecimalDigits%2A>.  
  
 В следующей таблице представлены свойства <xref:System.Globalization.NumberFormatInfo>, обеспечивающие управление форматированием возвращаемой строки.  
  
|Свойство NumberFormatInfo|Описание|  
|-------------------------------|--------------|  
|<xref:System.Globalization.NumberFormatInfo.PercentPositivePattern%2A>|Определяет положение символа процента в положительных значениях.|  
|<xref:System.Globalization.NumberFormatInfo.PercentNegativePattern%2A>|Определяет положение символа процента и отрицательного знака в отрицательных значениях.|  
|<xref:System.Globalization.NumberFormatInfo.NegativeSign%2A>|Определяет строку, указывающую, что число является отрицательным.|  
|<xref:System.Globalization.NumberFormatInfo.PercentSymbol%2A>|Определяет символ процента.|  
|<xref:System.Globalization.NumberFormatInfo.PercentDecimalDigits%2A>|Определяет количество цифр дробной части в значении процента по умолчанию. Это значение можно переопределить с помощью описателя точности.|  
|<xref:System.Globalization.NumberFormatInfo.PercentDecimalSeparator%2A>|Определяет строку, разделяющую целую и дробную части числа.|  
|<xref:System.Globalization.NumberFormatInfo.PercentGroupSeparator%2A>|Определяет строку, разделяющую группы цифр целой части.|  
|<xref:System.Globalization.NumberFormatInfo.PercentGroupSizes%2A>|Определяет число целочисленных цифр, входящих в группу.|  
  
 В следующем примере значения с плавающей запятой форматируются с помощью спецификатора процентного формата.  
  
 [!code-cpp[Formatting.Numeric.Standard#7](../../../samples/snippets/cpp/VS_Snippets_CLR/Formatting.Numeric.Standard/cpp/Standard.cpp#7)]
 [!code-csharp[Formatting.Numeric.Standard#7](../../../samples/snippets/csharp/VS_Snippets_CLR/Formatting.Numeric.Standard/cs/Standard.cs#7)]
 [!code-vb[Formatting.Numeric.Standard#7](../../../samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.Numeric.Standard/vb/Standard.vb#7)]  
  
 [К таблице](#table)  
  
<a name="RFormatString"></a>   
## Описатель формата обратного преобразования \("R"\)  
 Описатель формата обратного преобразования \("R"\) гарантирует, что строка, полученная преобразованием из числового значения, при обратном преобразовании даст то же самое числовое значение. Этот формат поддерживается только для типов <xref:System.Single>, <xref:System.Double> и <xref:System.Numerics.BigInteger>.  
  
 Если с помощью этого описателя форматируется значение типа <xref:System.Numerics.BigInteger>, то его строковое представление будет содержать все значащие цифры <xref:System.Numerics.BigInteger>. Если с помощью этого описателя форматируется значение типа <xref:System.Single> или <xref:System.Double>, то сначала для него проверяется общий формат, при этом для <xref:System.Double> используется 15\-знаковая точность, а для <xref:System.Single> — 7\-знаковая точность. Если строка может быть разобрана так, чтобы переменная приняла то же значение, форматирование производится с помощью общего указателя формата. Если при обратном преобразовании значение все же меняется, то количество значащих цифр увеличивается до 17 для типа <xref:System.Double> и 9 — для типа <xref:System.Single>.  
  
 Хотя описатель точности можно указать, он будет проигнорирован. Приведенные указатели приема\-передачи в данном случае имеют преимущество перед указателем точности.  
  
 Форматирование результирующей строки определяется сведениями о форматировании в текущем объекте <xref:System.Globalization.NumberFormatInfo>. В следующей таблице представлены свойства <xref:System.Globalization.NumberFormatInfo>, обеспечивающие управление форматированием результирующей строки.  
  
|Свойство NumberFormatInfo|Описание|  
|-------------------------------|--------------|  
|<xref:System.Globalization.NumberFormatInfo.NegativeSign%2A>|Определяет строку, указывающую, что число является отрицательным.|  
|<xref:System.Globalization.NumberFormatInfo.NumberDecimalSeparator%2A>|Определяет строку, разделяющую целую и дробную части числа.|  
|<xref:System.Globalization.NumberFormatInfo.PositiveSign%2A>|Определяет строку, указывающую, что экспонента является положительной.|  
  
 В следующем примере значения <xref:System.Double> форматируются с помощью спецификатора формата приема\-передачи.  
  
 [!code-cpp[Formatting.Numeric.Standard#8](../../../samples/snippets/cpp/VS_Snippets_CLR/Formatting.Numeric.Standard/cpp/Standard.cpp#8)]
 [!code-csharp[Formatting.Numeric.Standard#8](../../../samples/snippets/csharp/VS_Snippets_CLR/Formatting.Numeric.Standard/cs/Standard.cs#8)]
 [!code-vb[Formatting.Numeric.Standard#8](../../../samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.Numeric.Standard/vb/Standard.vb#8)]  
  
> [!IMPORTANT]
>  В некоторых случаях для значений <xref:System.Double>, отформатированных с использованием строки стандартного числового формата "R", не удается успешно выполнить обратное преобразование при компиляции с использованием параметра `/platform:x64` или `/platform:anycpu` и запуска в 64\-разрядных системах. Дополнительные сведения см. в следующем абзаце.  
  
 Чтобы избежать проблемы со значениями <xref:System.Double>, отформатированными с использованием строки стандартного числового формата R, для которых не удалось выполнить обратное преобразование при компиляции с использованием параметра `/platform:x64` или `/platform:anycpu` в 64\-разрядных системах, можно отформатировать значения <xref:System.Double> с помощью строки стандартного числового формата G17. В примере ниже используется строка формата "R" со значением <xref:System.Double>, для которого не удается выполнить обратное преобразование, а также строка формата "G17" для успешного обратного преобразования исходного значения.  
  
 [!code-csharp[System.Double.ToString#5](../../../samples/snippets/csharp/VS_Snippets_CLR_System/system.Double.ToString/cs/roundtripex1.cs#5)]
 [!code-vb[System.Double.ToString#5](../../../samples/snippets/visualbasic/VS_Snippets_CLR_System/system.Double.ToString/vb/roundtripex1.vb#5)]  
  
 [К таблице](#table)  
  
<a name="XFormatString"></a>   
## Описатель шестнадцатеричного формата \("X"\)  
 При использовании описателя шестнадцатеричного формата \("X"\) число преобразуется в строку шестнадцатеричных цифр. Регистр шестнадцатеричных цифр больше 9 совпадает с регистром описателя формата. Например, чтобы получить запись "ABCDEF", задайте описатель X" или, наоборот, задайте описатель "x", чтобы получить "abcdef". Этот формат доступен только для целых типов.  
  
 Минимальное количество знаков в выходной строке задается спецификатором точности. Недостающие знаки в строке заменяются нулями.  
  
 Форматирование результирующей строки не зависит от сведений о форматировании в текущем объекте <xref:System.Globalization.NumberFormatInfo>.  
  
 В следующем примере значения <xref:System.Int32> форматируются с помощью спецификатора шестнадцатеричного формата.  
  
 [!code-cpp[Formatting.Numeric.Standard#9](../../../samples/snippets/cpp/VS_Snippets_CLR/Formatting.Numeric.Standard/cpp/Standard.cpp#9)]
 [!code-csharp[Formatting.Numeric.Standard#9](../../../samples/snippets/csharp/VS_Snippets_CLR/Formatting.Numeric.Standard/cs/Standard.cs#9)]
 [!code-vb[Formatting.Numeric.Standard#9](../../../samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.Numeric.Standard/vb/Standard.vb#9)]  
  
 [К таблице](#table)  
  
<a name="NotesStandardFormatting"></a>   
## Примечания  
  
### Настройки панели управления  
 Параметры элемента панели управления **Язык и региональные стандарты** влияют на выходную строку, получаемую в результате операции форматирования. Эти параметры используются для инициализации объекта <xref:System.Globalization.NumberFormatInfo>, связанного с языком и региональными параметрами текущего потока, откуда берутся значения, используемые для управления форматированием. Результирующие строки будут различаться на компьютерах с разными параметрами.  
  
 Кроме того, если конструктор <xref:System.Globalization.CultureInfo.%23ctor%28System.String%29?displayProperty=fullName> используется для создания нового объекта <xref:System.Globalization.CultureInfo>, представляющего язык и региональные параметры, аналогичные текущему языку и региональным параметрам системы, то все настройки, заданные в разделе **Язык и региональные стандарты** панели управления, будут применяться к новому объекту <xref:System.Globalization.CultureInfo>. Можно воспользоваться конструктором <xref:System.Globalization.CultureInfo.%23ctor%28System.String%2CSystem.Boolean%29?displayProperty=fullName> для создания объекта <xref:System.Globalization.CultureInfo>, который не отражает настройки системы.  
  
### Свойства NumberFormatInfo  
 На форматирование влияют свойства текущего объекта <xref:System.Globalization.NumberFormatInfo>, который неявно определяется на основе языка и региональных параметров текущего потока или явно задается параметром <xref:System.IFormatProvider> метода, который вызывает форматирование. Укажите объект <xref:System.Globalization.NumberFormatInfo> или объект <xref:System.Globalization.CultureInfo> для этого параметра.  
  
> [!NOTE]
>  Дополнительные сведения о настройке шаблонов или строк, используемых в форматировании числовых значений см. статью о классе <xref:System.Globalization.NumberFormatInfo>.  
  
### Целочисленные типы и типы с плавающей запятой  
 Некоторые описания спецификаторов стандартных числовых форматов относятся к целочисленным типам и типам с плавающей запятой. Целочисленные типы — это <xref:System.Byte>, <xref:System.SByte>, <xref:System.Int16>, <xref:System.Int32>, <xref:System.Int64>, <xref:System.UInt16>, <xref:System.UInt32>, <xref:System.UInt64> и <xref:System.Numerics.BigInteger>. Числовыми типами с плавающей запятой являются <xref:System.Decimal>, <xref:System.Single> и <xref:System.Double>.  
  
### Бесконечности действительных чисел с плавающей запятой и NaN  
 Если, вне зависимости от строки формата, значение типа с плавающей запятой <xref:System.Single> или <xref:System.Double> является положительной бесконечностью, отрицательной бесконечностью или не является числом \(NaN\), отформатированная строка будет содержать значение соответствующего свойства \(<xref:System.Globalization.NumberFormatInfo.PositiveInfinitySymbol%2A>, <xref:System.Globalization.NumberFormatInfo.NegativeInfinitySymbol%2A> или <xref:System.Globalization.NumberFormatInfo.NaNSymbol%2A>\) применимого в настоящий момент объекта <xref:System.Globalization.NumberFormatInfo>.  
  
<a name="example"></a>   
## Пример  
 В следующем примере с помощью языка и региональных параметров "en\-US" и всех описателей стандартных числовых форматов форматируется целочисленное значение и числовое значение с плавающей запятой. В этом примере используются два числовых типа \(<xref:System.Double> и <xref:System.Int32>\), но аналогичные результаты были бы получены для любых других числовых типов \(<xref:System.Byte>, <xref:System.SByte>, <xref:System.Int16>, <xref:System.Int32>, <xref:System.Int64>, <xref:System.UInt16>, <xref:System.UInt32>, <xref:System.UInt64>, <xref:System.Numerics.BigInteger>, <xref:System.Decimal> и <xref:System.Single>\).  
  
 [!code-csharp[system.x.tostring-and-culture#1](../../../samples/snippets/csharp/VS_Snippets_CLR_System/system.X.ToString-and-Culture/cs/xts.cs#1)]
 [!code-vb[system.x.tostring-and-culture#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR_System/system.X.ToString-and-Culture/vb/xts.vb#1)]  
  
## См. также  
 <xref:System.Globalization.NumberFormatInfo>   
 [Строки настраиваемых числовых форматов](../../../docs/standard/base-types/custom-numeric-format-strings.md)   
 [Типы форматирования](../../../docs/standard/base-types/formatting-types.md)   
 [Практическое руководство. Добавление к числу начальных нулей.](../../../docs/standard/base-types/how-to-pad-a-number-with-leading-zeros.md)   
 [Пример. Служебная программа форматирования .NET Framework 4](http://code.msdn.microsoft.com/NET-Framework-4-Formatting-9c4dae8d)   
 [Составное форматирование](../../../docs/standard/base-types/composite-formatting.md)