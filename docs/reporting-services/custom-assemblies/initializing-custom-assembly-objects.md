---
title: Initialisation d’objets dans un assembly personnalisé | Microsoft Docs
ms.date: 03/03/2017
description: Découvrez comment initialiser vos classes personnalisées avec des valeurs disponibles à partir des collections d’objets globales du rapport.
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: custom-assemblies
ms.topic: reference
helpviewer_keywords:
- initializing custom assemblies [Reporting Services]
- custom assemblies [Reporting Services], initializing
- OnInit method
ms.assetid: 26fd74dc-d02f-40f7-aeb3-50ce05e9e6b9
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 24009d6b4e7d1fbfe107d16b828f16454dff7bc0
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100064524"
---
# <a name="initializing-custom-assembly-objects"></a>Initialisation d'objets Assembly personnalisés
  Dans certains cas, vous pouvez être amené à initialiser des valeurs de propriété et de champ dans vos classes d'assembly personnalisées lorsque vous les instanciez. Vous aurez vraisemblablement besoin d'initialiser vos classes personnalisées avec des valeurs disponibles à partir des collections d'objets globales du rapport. Pour cela, substituez la méthode **OnInit** de l’objet **Code** d’un rapport. Pour accéder à **OnInit**, utilisez l’élément **Code** de la définition de rapport. Il existe deux techniques pour initialiser les valeurs de propriété ou de champ des classes de l’assembly personnalisé que vous prévoyez d’utiliser dans votre rapport : Vous pouvez déclarer et créer une nouvelle instance de votre classe à l’aide de **OnInit**, ou vous pouvez appeler une méthode disponible publiquement à l’aide de **OnInit**.  
  
## <a name="global-object-collections-and-initialization"></a>Collections d'objets globales et initialisation  
 Plusieurs collections sont disponibles pour initialiser vos variables de classe personnalisées. Vous pouvez utiliser les collections **Globals** et **User**. Les collections **Parameters**, **Fields** et **ReportItems** ne sont pas disponibles au moment où, dans le cycle de vie du rapport, la méthode **OnInit** est appelée. Pour utiliser les collections partagées, **Globals** ou **User**, vous devez inclure la référence d’objet **Report**. Par exemple, pour initialiser votre classe personnalisée selon la langue actuelle de l’utilisateur qui accède au rapport, votre élément **Code** peut se présenter comme suit :  
  
```  
<Code>  
   Dim m_myClass As MyClass  
  
   Protected Overrides Sub OnInit()  
      m_myClass = new MyClass(Report.User!Language, _  
         Report.Globals!ExecutionTime)  
   End Sub  
</Code>  
```  
  
 L'une des méthodes permettant d'initialiser les valeurs de propriété et de champ d'une classe comme montré précédemment consiste à déclarer votre classe et à créer une nouvelle instance de celle-ci en appelant un constructeur substitué.  
  
 Une autre méthode permettant d’initialiser les valeurs de propriété et de champ des classes dans vos assemblys personnalisés consiste à appeler une méthode publiquement disponible que vous définissez à partir de la méthode **OnInit**. Vous devez d'abord ajouter un nom d'instance pour votre classe dans le fichier de définition de rapport. Une fois que vous avez ajouté la référence d'assembly et le nom d'instance appropriés, vous pouvez appeler votre méthode d'initialisation pour initialiser les valeurs de propriété et de champ pour votre classe. Votre méthode **OnInit** peut se présenter comme suit :  
  
```  
<Code>  
   Protected Overrides Sub OnInit()  
      m_myClass.MyInitializationMethod(Report.User!Language, _  
         Report.Globals!ExecutionTime)  
   End Sub  
</Code>  
```  
  
 Pour plus d’informations sur l’ajout d’une référence d’assembly et d’un nom de l’instance pour votre classe personnalisée, consultez [Ajouter une référence d’assembly à un rapport &#40;SSRS&#41;](../../reporting-services/report-design/add-an-assembly-reference-to-a-report-ssrs.md).  
  
 Pour plus d’informations sur les collections d’objets globales, consultez [Collections intégrées dans les expressions &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/built-in-collections-in-expressions-report-builder.md).  
  
## <a name="see-also"></a>Voir aussi  
 [Utilisation d'assemblages personnalisés avec des rapports](../../reporting-services/custom-assemblies/using-custom-assemblies-with-reports.md)  
  
  
