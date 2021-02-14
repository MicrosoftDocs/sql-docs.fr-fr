---
title: Fonction Number (XQuery) | Microsoft Docs
description: En savoir plus sur le numéro de fonction XQuery () qui retourne la valeur numérique d’un argument spécifié.
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
- number function
- fn:number function
ms.assetid: dff6d19b-765c-4df9-afff-9a0e7be9b91b
author: rothja
ms.author: jroth
ms.openlocfilehash: 6aa035be000fbbda12a7205c33925daf656c3da0
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100345020"
---
# <a name="functions-on-nodes---number"></a>Fonctions sur les nœuds : number
[!INCLUDE [SQL Server Azure SQL Database ](../includes/applies-to-version/sqlserver.md)]

  Retourne la valeur numérique du nœud indiqué par *$arg*.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
fn:number() as xs:double?   
fn:number($arg as node()?) as xs:double?  
```  
  
## <a name="arguments"></a>Arguments  
 *$arg*  
 Nœud dont vous voulez renvoyer la valeur sous forme numérique.  
  
## <a name="remarks"></a>Notes  
 Si *$arg* n’est pas spécifié, la valeur numérique du nœud de contexte, convertie en valeur double, est retournée. Dans SQL Server, **FN : Number ()** sans argument ne peut être utilisé que dans le contexte d’un prédicat dépendant du contexte. Autrement dit, elle ne peut être utilisée qu'à l'intérieur de crochets ([ ]). Par exemple, l’expression suivante retourne l' `ROOT` élément <>.  
  
```  
declare @x xml  
set @x='<ROOT>111</ROOT>'  
select @x.query('/ROOT[number()=111]')  
```  
  
 Si la valeur du nœud n’est pas une représentation lexicale valide d’un type simple numérique, comme défini dans le **schéma XML, partie 2 : types de données, recommandation W3C**, la fonction retourne une séquence vide. Les valeurs NaN ne sont pas prises en charge.  
  
## <a name="examples"></a>Exemples  
 Cette rubrique fournit des exemples de XQuery relatifs à des instances XML stockées dans différentes colonnes de type **XML** dans la base de données AdventureWorks.  
  
### <a name="a-using-the-number-xquery-function-to-retrieve-the-numeric-value-of-an-attribute"></a>R. Utilisation de la fonction XQuery number() pour récupérer la valeur numérique d'un attribut  
 La requête suivante récupère la valeur numérique de l'attribut taille de lot à partir du premier poste de travail du processus de fabrication du modèle de produit 7.  
  
```  
SELECT ProductModelID, Instructions.query('  
declare namespace AWMI="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelManuInstructions" ;  
     for $i in (//AWMI:root//AWMI:Location)[1]  
     return   
       <Location LocationID="{ ($i/@LocationID) }"   
                   LotSizeA="{  $i/@LotSize }"  
                   LotSizeB="{  number($i/@LotSize) }"  
                   LotSizeC="{ number($i/@LotSize) + 1 }" >  
  
       </Location>  
') as Result  
FROM Production.ProductModel  
WHERE ProductModelID=7  
```  
  
 Notez les points suivants dans la requête précédente :  
  
-   La fonction **Number ()** n’est pas obligatoire, comme le montre la requête de l’attribut **LotSizeA** . Il s'agit d'une fonction XPath 1.0 qui est incluse pour garantir la compatibilité ascendante.  
  
-   Le XQuery pour **LotSizeB** spécifie la fonction Number et est redondant.  
  
-   La requête pour un **lot** en nombre montre l’utilisation d’une valeur numérique dans une opération arithmétique.  
  
 Voici le résultat obtenu :  
  
```  
ProductModelID   Result  
----------------------------------------------  
7              <Location LocationID="10"   
                         LotSizeA="100"   
                         LotSizeB="100"   
                         LotSizeC="101" />  
```  
  
### <a name="implementation-limitations"></a>Limites de mise en œuvre  
 Les limitations suivantes s'appliquent :  
  
-   La fonction **Number ()** accepte uniquement les nœuds. Elle n'accepte pas de valeurs atomiques.  
  
-   Lorsque les valeurs ne peuvent pas être retournées sous forme de nombre, la fonction **Number ()** retourne la séquence vide au lieu de Nan.  
  
## <a name="see-also"></a>Voir aussi  
 [Fonctions XQuery impliquant le type de données xml](../xquery/xquery-functions-against-the-xml-data-type.md)  
  
  
