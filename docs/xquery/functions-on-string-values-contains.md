---
title: Contains, fonction (XQuery) | Microsoft Docs
description: En savoir plus sur l’utilisation de la fonction contains dans un XQuery pour déterminer si une valeur de chaîne spécifiée contient la valeur de sous-chaîne spécifiée.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: sql
ms.reviewer: ''
ms.technology: xml
ms.topic: language-reference
dev_langs:
- XML
helpviewer_keywords:
- contains function (XQuery)
- fn:contains function
ms.assetid: 2c88c015-04fc-429b-84b2-835596a28b65
author: rothja
ms.author: jroth
ms.openlocfilehash: 18f57d57e1a944a1be66a880231533e6cf34493e
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100344969"
---
# <a name="functions-on-string-values---contains"></a>Fonctions sur les valeurs de chaîne : contains
[!INCLUDE [SQL Server Azure SQL Database ](../includes/applies-to-version/sqlserver.md)]

  Retourne une valeur de type xs : Boolean indiquant si la valeur de *$arg 1* contient une valeur de chaîne spécifiée par *$arg 2*.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
fn:contains ($arg1 as xs:string?, $arg2 as xs:string?) as xs:boolean?  
```  
  
## <a name="arguments"></a>Arguments  
 *$arg 1*  
 Valeur de chaîne à tester.  
  
 *$arg 2*  
 Sous-chaîne à rechercher.  
  
## <a name="remarks"></a>Notes  
 Si la valeur de *$arg 2* est une chaîne de longueur nulle, la fonction retourne la **valeur true**. Si la valeur de *$arg 1* est une chaîne de longueur nulle et que la valeur de *$arg 2* n’est pas une chaîne de longueur nulle, la fonction retourne **false**.  
  
 Si la valeur de *$arg 1* ou *$arg 2* est la séquence vide, l’argument est traité comme une chaîne de longueur nulle.  
  
 La fonction contains() utilise le classement des points de code Unicode par défaut de XQuery pour la comparaison des chaînes.  
  
 La valeur de sous-chaîne spécifiée pour *$arg 2* doit être inférieure ou égale à 4000 caractères. Si la valeur spécifiée est supérieure à 4000 caractères, une condition d’erreur dynamique se produit et la fonction Contains () retourne une séquence vide au lieu d’une valeur booléenne **true** ou **false**. [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] ne déclenche pas d'erreurs dynamiques sur les expressions XQuery.  
  
 Pour pouvoir effectuer des comparaisons sans respect de la casse, vous pouvez utiliser les fonctions [majuscules](../xquery/functions-on-string-values-upper-case.md) ou minuscules.  
  
## <a name="supplementary-characters-surrogate-pairs"></a>Caractères supplémentaires (paires de substitution)  
 Le comportement de la paire de substitution dans des fonctions XQuery dépend du niveau de compatibilité de la base de données et, dans certains cas, de l'URI de l'espace de noms par défaut des fonctions. Pour plus d’informations, consultez la section « les fonctions XQuery prennent en charge les substitutions » dans la rubrique [modifications avec rupture des fonctionnalités de moteur de base de données dans SQL Server 2016](../database-engine/breaking-changes-to-database-engine-features-in-sql-server-2016.md). Consultez également [niveau de compatibilité ALTER database &#40;&#41;Transact-SQL ](../t-sql/statements/alter-database-transact-sql-compatibility-level.md) et la [prise en charge d’Unicode et du classement](../relational-databases/collations/collation-and-unicode-support.md).  
  
## <a name="examples"></a>Exemples  
 Cette rubrique fournit des exemples XQuery sur des instances XML stockées dans différentes colonnes de type XML dans la base de données AdventureWorks.  
  
### <a name="a-using-the-contains-xquery-function-to-search-for-a-specific-character-string"></a>R. Utilisation de la fonction contains() XQuery pour rechercher une chaîne de caractères spécifique.  
 La requête suivante recherche des produits qui contiennent le mot Aerodynamic dans les descriptions résumées. La requête retourne le ProductID et l' `Summary` élément <> pour ces produits.  
  
```  
--The product model description document uses  
--namespaces. The WHERE clause uses the exit()  
--method of the xml data type. Inside the exit method,  
--the XQuery contains()function is used to  
--determine whether the <Summary> text contains the word  
--Aerodynamic.   
  
USE AdventureWorks  
GO  
WITH XMLNAMESPACES ('https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription' AS pd)  
SELECT ProductModelID, CatalogDescription.query('  
      <Prod>  
         { /pd:ProductDescription/@ProductModelID }  
         { /pd:ProductDescription/pd:Summary }  
      </Prod>  
 ') as Result  
FROM Production.ProductModel  
where CatalogDescription.exist('  
   /pd:ProductDescription/pd:Summary//text()  
    [contains(., "Aerodynamic")]') = 1  
```  
  
 Résultats  
  
 `ProductModelID Result`  
  
 `-------------- ---------`  
  
 `28     <Prod ProductModelID="28">`  
  
 `<pd:Summary xmlns:pd=`  
  
 `"https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription">`  
  
 `<p1:p xmlns:p1="http://www.w3.org/1999/xhtml">`  
  
 `A TRUE multi-sport bike that offers streamlined riding and`  
  
 `a revolutionary design. Aerodynamic design lets you ride with`  
  
 `the pros, and the gearing will conquer hilly roads.</p1:p>`  
  
 `</pd:Summary>`  
  
 `</Prod>`  
  
## <a name="see-also"></a>Voir aussi  
 [Fonctions XQuery impliquant le type de données xml](../xquery/xquery-functions-against-the-xml-data-type.md)  
  
  
