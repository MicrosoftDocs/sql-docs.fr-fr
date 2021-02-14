---
title: Fonction ceiling (XQuery) | Microsoft Docs
description: Découvrez comment utiliser la fonction XQuery Ceiling () pour retourner le plus petit nombre sans une partie fractionnaire qui n’est pas inférieure à la valeur de l’argument de fonction.
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: sql
ms.reviewer: ''
ms.technology: xml
ms.topic: language-reference
dev_langs:
- XML
helpviewer_keywords:
- fn:ceiling function
- ceiling function [XQuery]
ms.assetid: 594f1dd0-3c27-41b3-b809-9ce6714c5a97
author: rothja
ms.author: jroth
ms.openlocfilehash: 1ea2dbaab164c57acf9e9bc166ee431e80783648
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100341582"
---
# <a name="numeric-values-functions---ceiling"></a>Fonctions de valeurs numériques : ceiling 
[!INCLUDE [SQL Server Azure SQL Database ](../includes/applies-to-version/sqlserver.md)]

  Renvoie le nombre le plus petit sans portion décimale qui n'est pas inférieur à la valeur de cet argument. Si l'argument est une séquence vide, la fonction renvoie la séquence vide.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
fn:ceiling ( $arg as numeric?) as numeric?  
```  
  
## <a name="arguments"></a>Arguments  
 *$arg*  
 Nombre à laquelle s'applique la fonction.  
  
## <a name="remarks"></a>Notes  
 Si le type de *$arg* est l’un des trois types numériques de base, **XS : float**, **XS : double** ou **XS : Decimal**, le type de retour est le même que le type de *$arg* .  
  
 Si le type de *$arg* est un type dérivé de l’un des types numériques, le type de retour est le type numérique de base.  
  
 Si l’entrée des fonctions FN : Floor, fn : Ceiling ou FN : Round est **xdt : untypedAtomic**, elle est implicitement convertie en **XS : double**.  
  
 Tout autre type génère une erreur statique.  
  
## <a name="examples"></a>Exemples  
 Cette rubrique fournit des exemples de XQuery relatifs à des instances XML stockées dans différentes colonnes de type **XML** dans la base de données AdventureWorks.  
  
### <a name="a-using-the-ceiling-xquery-function"></a>R. Utilisation de la fonction ceiling() de XQuery  
 Pour le modèle de produit 7, cette requête renvoie une liste des postes de travail que compte le processus de fabrication du modèle de produit. Pour chaque poste de travail, la requête renvoie l'ID, les heures de main-d'œuvre et la taille des lots, le cas échéant. La requête utilise la fonction **Ceiling** pour retourner les heures de main-d’œuvre en tant que valeurs de type **Decimal**.  
  
```  
SELECT ProductModelID, Instructions.query('  
declare namespace AWMI="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelManuInstructions";   
     for $i in /AWMI:root/AWMI:Location  
     return   
       <Location LocationID="{ $i/@LocationID }"   
                   LaborHrs="{ ceiling($i/@LaborHours) }" >  
                    {   
                      $i/@LotSize  
                    }    
       </Location>  
') AS Result  
FROM Production.ProductModel  
WHERE ProductModelID=7  
```  
  
 Notez les points suivants dans la requête précédente :  
  
-   Le préfixe d'espace de noms AWMI signifie Adventure Works Manufacturing Instructions (instructions de fabrication d'Adventure Works). Ce préfixe fait référence à l'espace de noms utilisé dans le document interrogé.  
  
-   **Instructions** est une colonne de type **XML** . Par conséquent, la [méthode Query () (type de données XML)](../t-sql/xml/query-method-xml-data-type.md) est utilisée pour spécifier XQuery. L'instruction XQuery est spécifiée comme argument de la méthode query.  
  
-   **pour... Return** est une construction de boucle. Dans la requête, la boucle **for** identifie une liste d' \<Location> éléments. Pour chaque poste de travail, l’instruction **Return** dans la boucle **for** décrit le XML à générer :  
  
    -   \<Location>Élément qui a des attributs LocationID et LaborHrs. L'expression correspondante entre accolades ({ }) récupère les valeurs requises à partir du document.  
  
    -   L’expression {$ i/@LotSize } extrait l’attribut de volume du document, le cas échéant.  
  
    -   Voici le résultat obtenu :  
  
```  
ProductModelID Result    
-------------- ------------------------------------------------------  
7      <Location LocationID="10" LaborHrs="3" LotSize="100"/>  
       <Location LocationID="20" LaborHrs="2" LotSize="1"/>     
       <Location LocationID="30" LaborHrs="1" LotSize="1"/>     
       <Location LocationID="45" LaborHrs="1" LotSize="20"/>  
       <Location LocationID="60" LaborHrs="3" LotSize="1"/>     
       <Location LocationID="60" LaborHrs="4" LotSize="1"/>  
```  
  
### <a name="implementation-limitations"></a>Limites de mise en œuvre  
 Les limitations suivantes s'appliquent :  
  
-   La fonction **Ceiling ()** associe toutes les valeurs entières à XS : Decimal.  
  
## <a name="see-also"></a>Voir aussi  
 [Fonction Floor &#40;XQuery&#41;](../xquery/numeric-values-functions-floor.md)   
 [Fonction Round &#40;XQuery&#41;](../xquery/numeric-values-functions-round.md)  
  
  
