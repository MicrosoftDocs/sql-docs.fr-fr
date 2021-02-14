---
title: Fonction distinct-values (XQuery) | Microsoft Docs
description: Découvrez comment utiliser la fonction distinct-values dans un XQuery pour supprimer des valeurs en double d’une séquence.
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
- distinct-values function
- fn:distinct-values function
ms.assetid: f4c2bb53-2bec-4f1a-9c00-cf997fb7ae5b
author: rothja
ms.author: jroth
ms.openlocfilehash: b9a3561c25ca0c64e01c2603b64656320d499518
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100341898"
---
# <a name="functions-on-sequences---distinct-values"></a>Fonctions sur les séquences : distinct-values
[!INCLUDE[sqlserver](../includes/applies-to-version/sqlserver.md)]

  Supprime les valeurs en double de la séquence spécifiée par *$arg*. Si *$arg* est une séquence vide, la fonction retourne la séquence vide.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
fn:distinct-values($arg as xdt:anyAtomicType*) as xdt:anyAtomicType*  
```  
  
## <a name="arguments"></a>Arguments  
 *$arg*  
 Séquence de valeurs atomiques.  
  
## <a name="remarks"></a>Notes  
 Tous les types des valeurs atomisées passées à des **valeurs distinctes ()** doivent être des sous-types du même type de base. Les types de base acceptés sont les types qui prennent en charge l’opération **EQ** . Ces types incluent les trois types numériques de base intégrés, les types de base date/heure et les types xs:string (chaîne), xs:boolean (booléen) et xdt:untypedAtomic (atomique non typé). Les valeurs de type xdt:untypedAtomic sont converties en type xs:string. En cas de mélange de ces types ou si des valeurs d'autres types sont transmis, une erreur statique se produit.  
  
 Le résultat de **distinct-values ()** reçoit le type de base des types transmis, tels que XS : String dans le cas de xdt : untypedAtomic, avec la cardinalité d’origine. Si l'entrée est vide (valeur empty) de façon statique, « empty » est alors implicite et une erreur statique est émise.  
  
 Les valeurs de type xs:string sont comparées au classement de point de codes Unicode par défaut de XQuery.  
  
## <a name="examples"></a>Exemples  
 Cette rubrique fournit des exemples de XQuery relatifs à des instances XML stockées dans différentes colonnes de type **XML** dans la base de données AdventureWorks.  
  
### <a name="a-using-the-distinct-values-function-to-remove-duplicate-values-from-the-sequence"></a>R. Utilisation de la fonction distinct-values() pour supprimer les valeurs en double d'une séquence  
 Dans cet exemple, une instance XML contenant des numéros de téléphone est assignée à une variable de type **XML** . Le XQuery spécifié par rapport à cette variable utilise la fonction **distinct-values ()** pour compiler une liste de numéros de téléphone qui ne contiennent pas de doublons.  
  
```  
declare @x xml  
set @x = '<PhoneNumbers>  
 <Number>111-111-1111</Number>  
 <Number>111-111-1111</Number>  
 <Number>222-222-2222</Number>  
</PhoneNumbers>'  
-- 1st select  
select @x.query('  
  distinct-values( data(/PhoneNumbers/Number) )  
') as result  
```  
  
 Voici le résultat obtenu :  
  
```  
111-111-1111 222-222-2222    
```  
  
 Dans la requête suivante, une séquence de nombres (1, 1, 2) est transmise à la fonction **distinct-values ()** . La fonction supprime ensuite la valeur en double de la séquence et renvoie les deux autres.  
  
```  
declare @x xml  
set @x = ''  
select @x.query('  
  distinct-values((1, 1, 2))  
') as result  
```  
  
 La requête renvoie « 1 2 ».  
  
### <a name="implementation-limitations"></a>Limites de mise en œuvre  
 Les limitations suivantes s'appliquent :  
  
-   La fonction **distinct-values ()** mappe les valeurs entières à XS : Decimal.  
  
-   La fonction **distinct-values ()** ne prend en charge que les types mentionnés précédemment et ne prend pas en charge le mélange de types de base.  
  
-   La fonction **distinct-values ()** sur les valeurs XS : Duration n’est pas prise en charge.  
  
-   L'option syntaxique fournissant un classement n'est pas prise en charge.  
  
## <a name="see-also"></a>Voir aussi  
 [Fonctions XQuery impliquant le type de données xml](../xquery/xquery-functions-against-the-xml-data-type.md)  
  
  
