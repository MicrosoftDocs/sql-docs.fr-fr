---
title: Fonction concat (XQuery) | Microsoft Docs
description: En savoir plus sur la fonction XQuery Concat () qui retourne une chaîne créée en concaténant zéro ou plusieurs chaînes spécifiées comme arguments.
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
- fn:concat function
- concat function [XQuery]
ms.assetid: d50afd20-a297-445e-be9e-13b48017e7ca
author: rothja
ms.author: jroth
ms.openlocfilehash: 3ad92315e20ae314fb9a22ddea1241f379ecc90f
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100341918"
---
# <a name="functions-on-string-values---concat"></a>Fonctions sur les valeurs de chaîne : oncat
[!INCLUDE [SQL Server Azure SQL Database ](../includes/applies-to-version/sqlserver.md)]

  Accepte zéro ou plusieurs chaînes comme arguments et renvoie une chaîne créée suite à la concaténation des valeurs de chacun de ces arguments.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
fn:concat ($string as xs:string?  
           ,$string as xs:string?  
           [, ...]) as xs:string  
```  
  
## <a name="arguments"></a>Arguments  
 *$string*  
 Chaîne facultative à concaténer.  
  
## <a name="remarks"></a>Notes  
 La fonction requiert au moins deux arguments. Si un argument est une séquence vide, elle est traitée comme une chaîne de longueur zéro.  
  
## <a name="supplementary-characters-surrogate-pairs"></a>Caractères supplémentaires (paires de substitution)  
 Le comportement de la paire de substitution dans des fonctions XQuery dépend du niveau de compatibilité de la base de données et, dans certains cas, de l'URI de l'espace de noms par défaut des fonctions. Pour plus d’informations, consultez la section « les fonctions XQuery prennent en charge les substitutions » dans la rubrique [modifications avec rupture des fonctionnalités de moteur de base de données dans SQL Server 2016](../database-engine/breaking-changes-to-database-engine-features-in-sql-server-2016.md). Consultez également [niveau de compatibilité ALTER database &#40;&#41;Transact-SQL ](../t-sql/statements/alter-database-transact-sql-compatibility-level.md) et la [prise en charge d’Unicode et du classement](../relational-databases/collations/collation-and-unicode-support.md).  
  
## <a name="examples"></a>Exemples  
 Cette rubrique fournit des exemples de XQuery relatifs à des instances XML stockées dans différentes colonnes de type **XML** de l’exemple de base de données AdventureWorks.  
  
### <a name="a-using-the-concat-xquery-function-to-concatenate-strings"></a>R. Utilisation de la fonction XQuery concat() pour concaténer des chaînes  
 Pour un modèle de produit spécifique, cette requête renvoie une chaîne obtenue d'après la concaténation de la période et de la description de la garantie. Dans le document de description du catalogue, l' `Warranty` élément <> est constitué de <`WarrantyPeriod`> et <`Description` éléments enfants.  
  
```  
WITH XMLNAMESPACES (  
'https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription' AS pd,  
'https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelWarrAndMain' AS wm)  
SELECT CatalogDescription.query('  
    <Product   
        ProductModelID= "{ (/pd:ProductDescription/@ProductModelID)[1] }"  
        ProductModelName = "{ sql:column("PD.Name") }" >  
        {   
          concat( string((/pd:ProductDescription/pd:Features/wm:Warranty/wm:WarrantyPeriod)[1]), "-",  
                  string((/pd:ProductDescription/pd:Features/wm:Warranty/wm:Description)[1]))   
         }   
     </Product>  
 ') as Result  
FROM Production.ProductModel PD  
WHERE  PD.ProductModelID=28  
  
```  
  
 Notez les points suivants dans la requête précédente :  
  
-   Dans la clause SELECT, CatalogDescription est une colonne de type **XML** . Par conséquent, la [méthode Query () (type de données XML)](../t-sql/xml/query-method-xml-data-type.md), instructions. Query (), est utilisée. L'instruction XQuery est spécifiée comme argument de la méthode query.  
  
-   Le document sur lequel porte la requête utilise des espaces de noms. Par conséquent, le mot clé **namespace** est utilisé pour définir le préfixe de l’espace de noms. Pour plus d’informations, consultez [prologue XQuery](../xquery/modules-and-prologs-xquery-prolog.md).  
  
 Voici le résultat obtenu :  
  
```  
<Product ProductModelID="28" ProductModelName="Road-450">1 year-parts and labor</Product>  
```  
  
 La requête précédente récupère les informations se rapportant à un produit spécifique. La requête suivante récupère les mêmes informations mais pour tous les produits pour lesquels il existe des descriptions de catalogue XML. La méthode **exist ()** du type de données **XML** dans la clause WHERE retourne la valeur true si le document XML dans les lignes a un <`ProductDescription` élément>.  
  
```  
WITH XMLNAMESPACES (  
'https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription' AS pd,  
'https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelWarrAndMain' AS wm)  
  
SELECT CatalogDescription.query('  
    <Product   
        ProductModelID= "{ (/pd:ProductDescription/@ProductModelID)[1] }"   
        ProductName = "{ sql:column("PD.Name") }" >  
        {   
          concat( string((/pd:ProductDescription/pd:Features/wm:Warranty/wm:WarrantyPeriod)[1]), "-",  
                  string((/pd:ProductDescription/pd:Features/wm:Warranty/wm:Description)[1]))   
         }   
     </Product>  
 ') as Result  
FROM Production.ProductModel PD  
WHERE CatalogDescription.exist('//pd:ProductDescription ') = 1  
  
```  
  
 Notez que la valeur booléenne retournée par la méthode **exist ()** du type **XML** est comparée à 1.  
  
### <a name="implementation-limitations"></a>Limites de mise en œuvre  
 Les limitations suivantes s'appliquent :  
  
-   La fonction **concat ()** dans SQL Server accepte uniquement les valeurs de type xs : String. Les autres valeurs doivent être explicitement converties en xs:string ou en xdt:untypedAtomic.  
  
## <a name="see-also"></a>Voir aussi  
 [Fonctions XQuery impliquant le type de données xml](../xquery/xquery-functions-against-the-xml-data-type.md)  
  
  
