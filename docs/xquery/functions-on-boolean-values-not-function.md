---
title: Fonction not (XQuery) | Microsoft Docs
description: Découvrez comment la fonction XQuery not () est utilisée avec des valeurs booléennes.
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
- effective Boolean value [XQuery]
- fn:not function
- not function [XQuery]
- EBV
ms.assetid: 93dfc377-45f1-4384-9392-560d9331a915
author: rothja
ms.author: jroth
ms.openlocfilehash: f857a8f107b26500bb324584d7d7853e09bbb735
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100353298"
---
# <a name="functions-on-boolean-values---not-function"></a>Fonctions sur des valeurs booléennes : fonction not 
[!INCLUDE [SQL Server Azure SQL Database ](../includes/applies-to-version/sqlserver.md)]

  Retourne TRUE si la valeur booléenne effective de *$arg* est false, et retourne false si la valeur booléenne effective de *$arg* a la valeur true.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
fn:not($arg as item()*) as xs:boolean  
```  
  
## <a name="arguments"></a>Arguments  
 *$arg*  
 Séquence d'éléments pour lesquels existe une valeur booléenne effective.  
  
## <a name="examples"></a>Exemples  
 Cette rubrique fournit des exemples de XQuery relatifs à des instances XML stockées dans différentes colonnes de type **XML** dans la base de données AdventureWorks.  
  
### <a name="a-using-the-not-xquery-function-to-find-product-models-whose-catalog-descriptions-do-not-include-the-specifications-element"></a>R. Utilisation de la fonction XQuery not () pour rechercher des modèles de produit dont les descriptions de catalogue n’incluent pas l' \<Specifications> élément.  
 La requête suivante construit du code XML qui contient des ID de modèle de produit pour les modèles de produit dont les descriptions de catalogue n’incluent pas l' `Specifications` élément <>.  
  
```  
WITH XMLNAMESPACES ('https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription' AS pd)  
SELECT ProductModelID, CatalogDescription.query('  
       <Product   
           ProductModelID="{ sql:column("ProductModelID") }"  
        />  
') as Result  
FROM Production.ProductModel  
WHERE CatalogDescription.exist('  
     /pd:ProductDescription[not(pd:Specifications/*)]  '  
     ) = 0  
```  
  
 Notez les points suivants dans la requête précédente :  
  
-   Étant donné que le document utilise des espaces de noms, l'exemple recourt à l'instruction WITH NAMESPACES. Une autre option consiste à utiliser le mot clé **Declare namespace** dans le [prologue XQuery](../xquery/modules-and-prologs-xquery-prolog.md) pour définir le préfixe.  
  
-   La requête construit ensuite le XML qui comprend l’élément <`Product`> et son attribut **ProductModelID** .  
  
-   La clause WHERE utilise la [méthode exist () (type de données XML)](../t-sql/xml/exist-method-xml-data-type.md) pour filtrer les lignes. La méthode **exist ()** retourne true s’il existe des \<ProductDescription> éléments qui n’ont pas d' \<Specification> éléments enfants. Notez l’utilisation de la fonction **not ()** .  
  
 Ce jeu de résultats est vide, car chaque description du catalogue du modèle de produit comprend l' \<Specifications> élément.  
  
### <a name="b-using-the-not-xquery-function-to-retrieve-work-center-locations-that-do-not-have-a-machinehours-attribute"></a>B. Utilisation de la fonction XQuery not() pour extraire les sites de production dépourvus de l'attribut MachineHours  
 La requête suivante porte sur la colonne Instructions. Cette colonne stocke les instructions de fabrication des modèles de produit.  
  
 Pour un modèle de produit particulier, la requête extrait les sites de production qui ne spécifient pas l'attribut MachineHours. Autrement dit, l’attribut **MachineHours** n’est pas spécifié pour l' \<Location> élément.  
  
```  
SELECT ProductModelID, Instructions.query('  
declare namespace AWMI="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelManuInstructions" ;  
     for $i in /AWMI:root/AWMI:Location[not(@MachineHours)]  
     return  
       <Location LocationID="{ $i/@LocationID }"   
                   LaborHrs="{ $i/@LaborHours }" >  
        </Location>  
') as Result  
FROM Production.ProductModel  
WHERE ProductModelID=7   
```  
  
 Dans la requête précédente, notez les points suivants :  
  
-   Le **declarenamespace** dans le [prologue XQuery](../xquery/modules-and-prologs-xquery-prolog.md) définit le préfixe d’espace de noms des instructions de fabrication d’Adventure Works. Il représente le même espace de noms que celui utilisé dans le document des instructions de fabrication.  
  
-   Dans la requête, le prédicat **not ( @MachineHours )** retourne la valeur true s’il n’existe aucun attribut **MachineHours** .  
  
 Voici le résultat obtenu :  
  
```  
ProductModelID Result   
-------------- --------------------------------------------  
7              <Location LocationID="30" LaborHrs="1"/>  
               <Location LocationID="50" LaborHrs="3"/>  
               <Location LocationID="60" LaborHrs="4"/>  
```  
  
### <a name="implementation-limitations"></a>Limites de mise en œuvre  
 Les limitations suivantes s'appliquent :  
  
-   La fonction **not ()** prend uniquement en charge les arguments de type xs : Boolean ou node () *, ou la séquence vide.  
  
## <a name="see-also"></a>Voir aussi  
 [Fonctions XQuery impliquant le type de données xml](../xquery/xquery-functions-against-the-xml-data-type.md)  
  
  
