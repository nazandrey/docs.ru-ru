---
title: "Загрузка из XAML | Microsoft Docs"
ms.custom: ""
ms.date: "03/30/2017"
ms.prod: ".net-framework-4.6"
ms.reviewer: ""
ms.suite: ""
ms.tgt_pltfrm: ""
ms.topic: "article"
ms.assetid: 1f103ef6-7bed-4f16-ae52-9e665c5a43d7
caps.latest.revision: 10
author: "Erikre"
ms.author: "erikre"
manager: "erikre"
caps.handback.revision: 10
---
# Загрузка из XAML
В этом образце показан способ динамической загрузки рабочего процесса XAML без применения средства XamlBuildTask.Вместо этого в образце вызывается метод <xref:System.Activities.XamlIntegration.ActivityXamlServices.Load%2A>.Образец представляет клиентское приложение [!INCLUDE[avalon1](../../../../includes/avalon1-md.md)], которое загружает рабочие процессы XAML, используя класс <xref:System.Activities.XamlIntegration.ActivityXamlServices>, и выполняет их.После загрузки этих процессов с использованием класса <xref:System.Activities.XamlIntegration.ActivityXamlServices> возвращается действие <xref:System.Activities.DynamicActivity%601>, которое может быть выполнено.  
  
#### Использование этого образца  
  
1.  Используя [!INCLUDE[vs2010](../../../../includes/vs2010-md.md)], откройте файл решения LoadFromXAML.sln.  
  
2.  Для построения решения нажмите CTRL\+SHIFT\+B.  
  
3.  Чтобы запустить решение, нажмите клавиши CTRL\+F5.  
  
> [!IMPORTANT]
>  Образцы уже могут быть установлены на компьютере.Перед продолжением проверьте следующий каталог \(по умолчанию\).  
>   
>  `<диск_установки>:\WF_WCF_Samples`  
>   
>  Если этот каталог не существует, перейдите на страницу [Образцы Windows Communication Foundation \(WCF\) и Windows Workflow Foundation \(WF\) для .NET Framework 4](http://go.microsoft.com/fwlink/?LinkId=150780), чтобы загрузить все образцы [!INCLUDE[indigo1](../../../../includes/indigo1-md.md)] и [!INCLUDE[wf1](../../../../includes/wf1-md.md)].Этот образец расположен в следующем каталоге.  
>   
>  `<диск_установки>:\WF_WCF_Samples\WF\Basic\Built-InActivities\DynamicActivity\LoadFromXAML`  
  
## См. также