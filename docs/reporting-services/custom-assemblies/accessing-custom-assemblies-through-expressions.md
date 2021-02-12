---
title: Accès aux assemblages personnalisés par le biais d’expressions | Microsoft Docs
description: Une fois que vous avez créé un assembly personnalisé, découvrez comment accéder aux classes de votre assembly personnalisé à l’aide d’expressions de rapport.
ms.date: 03/04/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: custom-assemblies
ms.topic: reference
helpviewer_keywords:
- expressions [Reporting Services], custom assemblies
- static member calls
- instance member calls [Reporting Services]
- calling class members
- custom assemblies [Reporting Services], expressions
ms.assetid: 917c4d47-1a95-4f54-98b1-e8cb2165d90f
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: df929de930289f9683608ec42240f03a93983ccb
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100064634"
---
# <a name="accessing-custom-assemblies-through-expressions"></a>Accès aux assemblys personnalisés par le biais d'expressions
  Une fois que vous avez créé un assembly personnalisé, qu'il est disponible auprès du Concepteur de rapports ou du serveur de rapports, que vous avez ajouté la stratégie de sécurité appropriée et une référence à votre assembly personnalisé dans votre définition de rapport, vous pouvez accéder aux membres des classes dans votre assembly à l'aide d'expressions de rapport. Pour faire référence à du code personnalisé dans une expression, vous devez appeler le membre d'une classe au sein de l'assembly. La procédure pour ce faire dépend du type de méthode, à savoir statique ou basée sur une instance.  
  
## <a name="calling-static-members-from-a-report-definition-file"></a>Appel de membres statiques d'un fichier de définition de rapport  
 Les membres statiques appartiennent à la classe ou au type, pas à un objet instancié. Ces membres sont accessibles en les appelant directement depuis la classe. Vous devez utiliser des membres statiques pour appeler des fonctions personnalisées dans un rapport chaque fois que possible, parce que les membres statiques offrent de meilleures performances. Pour appeler un membre statique, vous devez le référencer comme une expression qui prend la forme =*Namespace.Class.Method*.  
  
#### <a name="to-call-static-members"></a>Pour appeler des membres statiques  
  
-   Pour appeler un membre statique, définissez votre expression pour qu'elle soit égale au nom complet du membre, à savoir l'espace de noms, le nom de classe et le nom de membre. L’exemple suivant appelle la méthode **ToGBP** qui convertit la valeur de champ **StandardCost** de dollars en livres sterling et l’affiche dans un rapport :  
  
    ```  
    =CurrencyConversion.DollarCurrencyConversion.ToGBP(Fields!StandardCost.Value)  
    ```  
  
### <a name="important-information-regarding-static-fields-and-properties"></a>Informations importantes concernant les champs et les propriétés statiques  
 Actuellement, tous les rapports sont exécutés dans le même domaine d'application. Cela signifie que les rapports contenant des données statiques spécifiques à l'utilisateur exposent ces données à d'autres instances du même rapport. Cette condition peut permettre aux données statiques d'un utilisateur d'être disponibles pour tous les utilisateurs qui exécutent actuellement un rapport particulier. Pour cette raison, il est fortement recommandé de ne pas faire appel aux champs ou propriétés statiques dans les assemblys personnalisés ou dans l’élément **Code** ; à la place, utilisez des champs ou des propriétés d’instance dans vos rapports. Les méthodes statiques peuvent encore être utilisées car elles ne stockent pas d'état ou de données.  
  
## <a name="calling-instance-members-from-a-report-definition-file"></a>Appel de membres d'instance à partir d'un fichier de définition de rapport  
 Si votre assembly personnalisé contient des membres d'instance auxquels vous devez accéder dans une définition de rapport, vous devez ajouter un nom d'instance pour votre classe au rapport. Vous pouvez ajouter un nom d’instance pour une classe à l’aide de l’onglet **Code** de la boîte de dialogue **Propriétés de rapport**. Pour plus d’informations sur l’ajout d’instances de classes à un rapport, consultez [Code personnalisé et références d’assembly dans les expressions du Concepteur de rapports &#40;SSRS&#41;](../../reporting-services/report-design/custom-code-and-assembly-references-in-expressions-in-report-designer-ssrs.md).  
  
 Pour appeler un membre statique, vous devez le référencer comme une expression qui prend la forme =Code *.InstanceName.Method*.  
  
#### <a name="to-call-instance-members"></a>Pour appeler des membres d'instance  
  
-   Pour appeler un membre d’instance d’un assembly personnalisé, vous devez référencer le mot clé **Code** suivi par le nom de l’instance et la méthode. L’exemple suivant appelle une méthode d’instance **ToEUR** qui convertit la valeur de champ **StandardCost** de dollars en euros et l’affiche dans un rapport :  
  
    ```  
    =Code.m_myDollarCoversion.ToEUR(Fields!StandardCost.Value)  
    ```  
  
## <a name="see-also"></a>Voir aussi  
 [Utilisation d'assemblages personnalisés avec des rapports](../../reporting-services/custom-assemblies/using-custom-assemblies-with-reports.md)  
  
  
