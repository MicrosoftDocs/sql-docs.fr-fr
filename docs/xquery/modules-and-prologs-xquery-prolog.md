---
title: Prologue XQuery | Microsoft Docs
description: Découvrez le prologue XQuery qui contient une série de déclarations et de définitions qui créent l’environnement requis pour le traitement des requêtes.
ms.custom: ''
ms.date: 08/09/2016
ms.prod: sql
ms.prod_service: sql
ms.reviewer: ''
ms.technology: xml
ms.topic: language-reference
dev_langs:
- XML
helpviewer_keywords:
- XQuery, prolog
- prolog
- namespaces [XQuery]
- default namespace declarations
ms.assetid: 03924684-c5fd-44dc-8d73-c6ab90f5e069
author: rothja
ms.author: jroth
ms.openlocfilehash: 2b460f6da448e45d688ebcc08f418a9acbae85b8
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100341840"
---
# <a name="modules-and-prologs---xquery-prolog"></a>Modules et prologues : prologue XQuery
[!INCLUDE[sqlserver](../includes/applies-to-version/sqlserver.md)]

  Une requête XQuery se compose d'un prologue et d'un corps. Le prologue XQuery est une série de déclarations et de définitions qui créent ensemble l'environnement requis pour le traitement des requêtes. Dans SQL Server, le prologue XQuery peut inclure des déclarations d'espace de noms. Le corps XQuery se compose d'une séquence d'expressions qui spécifient le résultat de requête voulu.  
  
 Par exemple, la requête XQuery suivante est spécifiée par rapport à la colonne Instructions de type **XML** qui stocke les instructions de fabrication au format XML. La requête récupère les instructions de fabrication pour l'emplacement de l'atelier `10`. La `query()` méthode du type de données **XML** est utilisée pour spécifier la requête XQuery.  
  
```  
SELECT Instructions.query('declare namespace AWMI="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelManuInstructions";           
    /AWMI:root/AWMI:Location[@LocationID=10]  
') AS Result   
FROM  Production.ProductModel  
WHERE ProductModelID=7  
```  
  
 Notez les points suivants dans la requête précédente :  
  
-   Le prologue XQuery comprend une déclaration de préfixe d’espace de noms (AWMI), `(namespace AWMI="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelManuInstructions";` .  
  
-   Le mot clé `declare namespace` définit un préfixe d'espace de noms utilisé ultérieurement dans le corps de la requête.  
  
-   `/AWMI:root/AWMI:Location[@LocationID="10"]` représente le corps de la requête.  
  
## <a name="namespace-declarations"></a>Déclarations d'espace de noms  
 Une déclaration d'espace de noms définit un préfixe et l'associe à un URI d'espace de noms, comme illustré dans la requête ci-dessous. Dans la requête, `CatalogDescription` est une colonne de type **XML** .  
  
 Lorsque vous spécifiez une requête XQuery sur cette colonne, le prologue de la requête spécifie la déclaration `declare namespace` pour associer le préfixe `PD` (description du produit) à l'URI d'espace de noms. Ce préfixe est alors utilisé dans le corps de la requête à la place de l'URI d'espace de noms. Les nœuds XML résultants sont dans l'espace de noms associé à l'URI d'espace de noms.  
  
```  
SELECT CatalogDescription.query('  
declare namespace PD="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription";  
         /PD:ProductDescription/PD:Summary   
    ') as Result  
FROM Production.ProductModel  
where ProductModelID=19  
```  
  
 Pour améliorer la lisibilité de la requête, vous pouvez déclarer les espaces de noms à l'aide de WITH XMLNAMESPACES au lieu de déclarer une liaison entre des préfixes et des espaces de noms dans le prologue de la requête à l'aide de `declare namespace`.  
  
```  
WITH XMLNAMESPACES ('https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription' AS PD)  
  
SELECT CatalogDescription.query('  
         /PD:ProductDescription/PD:Summary   
    ') as Result  
FROM Production.ProductModel  
where ProductModelID=19  
```  
  
 Pour plus d’informations, consultez [Ajouter des espaces de noms aux requêtes avec WITH XMLNAMESPACES](../relational-databases/xml/add-namespaces-to-queries-with-with-xmlnamespaces.md).  
  
### <a name="default-namespace-declaration"></a>Déclaration d'espace de noms par défaut  
 Au lieu de déclarer un préfixe d'espace de noms à l'aide de la déclaration `declare namespace`, vous pouvez utiliser la déclaration `declare default element namespace` pour lier un espace de noms par défaut pour des noms d'élément. Dans ce cas, vous ne spécifiez aucun préfixe.  
  
 Dans l'exemple suivant, l'expression désignant le chemin d'accès dans le corps de la requête ne spécifie pas de préfixe d'espace de noms. Par défaut, tous les noms d'élément appartiennent au même espace de noms par défaut, spécifié dans le prologue.  
  
```  
SELECT CatalogDescription.query('  
     declare default element namespace  "https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription";  
        /ProductDescription/Summary   
    ') as Result  
FROM  Production.ProductModel  
WHERE ProductModelID=19   
```  
  
 Vous pouvez déclarer un espace de noms par défaut en utilisant WITH XMLNAMESPACES :  
  
```  
WITH XMLNAMESPACES (DEFAULT 'https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription')  
SELECT CatalogDescription.query('  
        /ProductDescription/Summary   
    ') as Result  
FROM  Production.ProductModel  
WHERE ProductModelID=19   
```  
  
## <a name="see-also"></a>Voir aussi  
 [Ajouter des espaces de noms aux requêtes avec WITH XMLNAMESPACES](../relational-databases/xml/add-namespaces-to-queries-with-with-xmlnamespaces.md)  
  
  
