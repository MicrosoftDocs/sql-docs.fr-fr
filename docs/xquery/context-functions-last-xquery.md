---
title: Fonction last (XQuery) | Microsoft Docs
description: En savoir plus sur la fonction XQuery Last () qui retourne l’index entier du dernier élément d’une séquence.
ms.custom: ''
ms.date: 03/09/2017
ms.prod: sql
ms.prod_service: sql
ms.reviewer: ''
ms.technology: xml
ms.topic: language-reference
dev_langs:
- XML
helpviewer_keywords:
- last function [XQuery]
- fn:last function
ms.assetid: dc92086e-3b01-4b0b-9f54-3bbf306cf7ae
author: rothja
ms.author: jroth
ms.openlocfilehash: 3fdc8ce4bf35bb630d0ed0b40892312f89a5285c
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100340521"
---
# <a name="context-functions---last-xquery"></a>Fonctions relatives au contexte : last (XQuery)
[!INCLUDE [SQL Server Azure SQL Database ](../includes/applies-to-version/sqlserver.md)]

  Renvoie le nombre d'éléments de la séquence en cours de traitement. Renvoie plus précisément l'index entier du dernier élément de la séquence. Le premier élément de la séquence a une valeur d'index de 1.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
fn:last() as xs:integer  
```  
  
## <a name="remarks"></a>Notes  
 Dans SQL Server, **FN : Last ()** ne peut être utilisé que dans le contexte d’un prédicat dépendant du contexte. Autrement dit, elle ne peut être utilisée qu'à l'intérieur de crochets (`[ ]`).  
  
## <a name="examples"></a>Exemples  
 Cette rubrique fournit des exemples de XQuery relatifs à des instances XML stockées dans différentes colonnes de type **XML** dans la base de données AdventureWorks.  
  
### <a name="a-using-the-last-xquery-function-to-retrieve-the-last-two-manufacturing-steps"></a>R. Utilisation de la fonction XQuery last() pour récupérer les deux dernières étapes de fabrication.  
 La requête suivante récupère les deux dernières étapes de fabrication d'un modèle de produit spécifique. La valeur, le nombre d’étapes de fabrication, retournée par la fonction **Last ()** , est utilisée dans cette requête pour récupérer les deux dernières étapes de fabrication.  
  
```  
SELECT ProductModelID, Instructions.query('   
declare namespace AWMI="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelManuInstructions";  
  <LastTwoManuSteps>  
   <Last-1Step>   
     { (/AWMI:root/AWMI:Location)[1]/AWMI:step[(last()-1)]/text() }  
   </Last-1Step>  
   <LastStep>   
     { (/AWMI:root/AWMI:Location)[1]/AWMI:step[last()]/text() }  
   </LastStep>  
  </LastTwoManuSteps>  
') as Result  
FROM Production.ProductModel  
WHERE ProductModelID=7  
```  
  
 Dans la requête précédente, la fonction **Last ()** dans/ `/AWMI:root//AWMI:Location)[1]/AWMI:step[last()]` retourne le nombre d’étapes de fabrication. Cette valeur permet de récupérer la dernière étape de fabrication sur ce poste de travail.  
  
 Voici le résultat obtenu :  
  
```  
ProductModelID Result    
-------------- -------------------------------------  
7      <LastTwoManuSteps>  
         <Last-1Step>  
            When finished, inspect the forms for defects per   
            Inspection Specification .  
         </Last-1Step>  
         <LastStep>Remove the frames from the tool and place them   
            in the Completed or Rejected bin as appropriate.  
         </LastStep>  
       </LastTwoManuSteps>  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Fonctions XQuery impliquant le type de données xml](../xquery/xquery-functions-against-the-xml-data-type.md)  
  
  
