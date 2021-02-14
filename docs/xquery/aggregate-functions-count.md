---
description: 'Fonctions d’agrégation : count'
title: Fonction count (XQuery) | Microsoft Docs
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
- fn:count function
- count function [XQuery]
ms.assetid: a9f7131f-23e1-4d4d-a36c-180447543926
author: rothja
ms.author: jroth
ms.openlocfilehash: eb3fa2943ead0cb4b4000dbaa534f9b4b232dd3d
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100341973"
---
# <a name="aggregate-functions---count"></a>Fonctions d’agrégation : count
[!INCLUDE [SQL Server Azure SQL Database ](../includes/applies-to-version/sqlserver.md)]

  Retourne le nombre d’éléments contenus dans la séquence spécifiée par *$arg*.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
fn:count($arg as item()*) as xs:integer  
```  
  
## <a name="arguments"></a>Arguments  
 *$arg*  
 Éléments à compter.  
  
## <a name="remarks"></a>Notes  
 Retourne 0 si *$arg* est une séquence vide.  
  
## <a name="examples"></a>Exemples  
 Cette rubrique fournit des exemples de XQuery relatifs à des instances XML stockées dans différentes colonnes de type **XML** dans la base de données AdventureWorks.  
  
### <a name="a-using-the-count-xquery-function-to-count-the-number-of-work-center-locations-in-the-manufacturing-of-a-product-model"></a>R. Utilisation de la fonction XQuery count() pour déterminer le nombre de sites de production impliqués dans la fabrication d'un modèle de produit  
 La requête suivante détermine le nombre de sites de production impliqués dans la fabrication d'un modèle de produit (ProductModelID=7).  
  
```  
SELECT Production.ProductModel.ProductModelID,   
       Production.ProductModel.Name,   
       Instructions.query('  
declare namespace AWMI="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelManuInstructions";  
       <NoOfWorkStations>  
          { count(/AWMI:root/AWMI:Location) }  
       </NoOfWorkStations>  
') as WorkCtrCount  
FROM Production.ProductModel  
WHERE Production.ProductModel.ProductModelID=7  
```  
  
 Notez les points suivants dans la requête précédente :  
  
-   Le mot clé **namespace** dans le [prologue XQuery](../xquery/modules-and-prologs-xquery-prolog.md) définit un préfixe d’espace de noms. Le préfixe est ensuite utilisé dans le corps XQuery.  
  
-   La requête construit le code XML qui comprend l' `NoOfWorkStations` élément <>.  
  
-   La fonction **Count ()** dans le corps XQuery compte le nombre de <`Location`> éléments.  
  
 Voici le résultat obtenu :  
  
```  
ProductModelID   Name                 WorkCtrCount       
-------------- ---------------------------------------------------  
7             HL Touring Frame  <NoOfWorkStations>6</NoOfWorkStations>     
```  
  
 Vous pouvez également construire le document XML de manière à ce qu'il comprenne l'ID et le nom du modèle de produit, comme le montre la requête suivante :  
  
```  
SELECT Instructions.query('  
declare namespace AWMI="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelManuInstructions";  
       <NoOfWorkStations  
             ProductModelID= "{ sql:column("Production.ProductModel.ProductModelID") }"   
             ProductModelName = "{ sql:column("Production.ProductModel.Name") }" >  
          { count(/AWMI:root/AWMI:Location) }  
       </NoOfWorkStations>  
') as WorkCtrCount  
FROM Production.ProductModel  
WHERE Production.ProductModel.ProductModelID= 7  
```  
  
 Voici le résultat obtenu :  
  
```  
<NoOfWorkStations ProductModelID="7"   
                  ProductModelName="HL Touring Frame">6</NoOfWorkStations>  
```  
  
 Comme le montre la requête ci-après, vous pouvez renvoyer ces valeurs en tant que type non-xml, plutôt que sous la forme de données XML. La requête utilise la [méthode value () (type de données XML)](../t-sql/xml/value-method-xml-data-type.md) pour récupérer le nombre d’emplacements de poste de travail.  
  
```  
SELECT  ProductModelID,   
        Name,   
        Instructions.value('declare namespace AWMI="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelManuInstructions";  
           count(/AWMI:root/AWMI:Location)', 'int' ) as WorkCtrCount  
FROM Production.ProductModel  
WHERE ProductModelID=7  
```  
  
 Voici le résultat obtenu :  
  
```  
ProductModelID    Name            WorkCtrCount  
-------------- ---------------------------------  
7              HL Touring Frame        6     
```  
  
## <a name="see-also"></a>Voir aussi  
 [Fonctions XQuery impliquant le type de données xml](../xquery/xquery-functions-against-the-xml-data-type.md)  
  
  
