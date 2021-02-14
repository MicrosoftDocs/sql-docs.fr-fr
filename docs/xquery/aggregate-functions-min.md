---
title: Fonction min (XQuery) | Microsoft Docs
description: En savoir plus sur la fonction XQuery min () qui retourne un élément dans une séquence dont la valeur est inférieure à celle de tous les autres.
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
- fn:min function
- min function [XQuery]
ms.assetid: db0b7d94-3fa6-488f-96d6-6a9a7d6eda23
author: rothja
ms.author: jroth
ms.openlocfilehash: 19c4204e84759bab022d0f0765b9d6154d23d003
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100341938"
---
# <a name="aggregate-functions---min"></a>Fonctions d’agrégation : min
[!INCLUDE [SQL Server Azure SQL Database ](../includes/applies-to-version/sqlserver.md)]

  Retourne à partir d’une séquence de valeurs atomiques, *$arg*, un élément dont la valeur est inférieure à celle de tous les autres.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
fn:min($arg as xdt:anyAtomicType*) as xdt:anyAtomicType?  
```  
  
## <a name="arguments"></a>Arguments  
 *$arg*  
 Séquence d'éléments à partir de laquelle la valeur minimale est renvoyée.  
  
## <a name="remarks"></a>Notes  
 Tous les types des valeurs atomisées passées à **min ()** doivent être des sous-types du même type de base. Les types de base acceptés sont les types qui prennent en charge l’opération **gt** . Ces types incluent les trois types numériques de base intégrés, les types de base date/heure et les types xs:string (chaîne), xs:boolean (booléen) et xdt:untypedAtomic (atomique non typé). Les valeurs de type xdt:untypedAtomic sont converties en xs:double. S’il existe un mélange de ces types, ou si d’autres valeurs d’autres types sont passées, une erreur statique est générée.  
  
 Le résultat de **min ()** reçoit le type de base des types transmis, tel que XS : double dans le cas de xdt : untypedAtomic. Si l'entrée est statiquement vide, vide est implicite et une erreur statique est générée.  
  
 La fonction **min ()** retourne une valeur dans la séquence inférieure à toute autre valeur dans la séquence d’entrée. Pour les valeurs xs:string, le classement par défaut des points de code Unicode est utilisé. Si une valeur xdt : untypedAtomic ne peut pas être convertie en XS : double, la valeur est ignorée dans la séquence d’entrée *$arg*. Si l'entrée est une séquence vide calculée de manière dynamique, la séquence vide est renvoyée.  
  
## <a name="examples"></a>Exemples  
 Cette rubrique fournit des exemples de XQuery relatifs à des instances XML stockées dans différentes colonnes de type **XML** dans la base de données AdventureWorks.  
  
### <a name="a-using-the-min-xquery-function-to-find-the-work-center-location-that-has-the-fewest-labor-hours"></a>R. Utilisation de la fonction XQuery min() pour rechercher le poste de travail enregistrant le moins d'heures de main-d'œuvre  
 La requête suivante récupère tous les postes de travail du processus de fabrication du modèle de produit (ProductModelID=7) qui enregistre le moins d'heures de main-d'œuvre. Généralement, comme le montre l'exemple suivant, un seul poste est renvoyé. Si plusieurs postes de travail enregistraient le même nombre minimal de main-d'œuvre, ils seraient tous renvoyés.  
  
```  
select ProductModelID, Name, Instructions.query('  
  declare namespace AWMI=  
    "https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelManuInstructions";  
  for   $Location in /AWMI:root/AWMI:Location  
  where $Location/@LaborHours =  
          min( /AWMI:root/AWMI:Location/@LaborHours )  
return  
  <Location WCID=     "{ $Location/@LocationID }"   
              LaborHrs= "{ $Location/@LaborHours }" />  
  ') as Result   
FROM  Production.ProductModel  
WHERE ProductModelID=7  
```  
  
 Notez les points suivants dans la requête précédente :  
  
-   Le mot clé **namespace** dans le prologue XQuery définit un préfixe d’espace de noms. Ce préfixe est utilisé ultérieurement dans le corps de la requête.  
  
 Le corps XQuery construit le XML qui possède un \<Location> élément avec des attributs wcid et **LaborHrs** .  
  
-   La requête récupère également l'identificateur et le nom du modèle de produit.  
  
 Voici le résultat obtenu :  
  
```  
ProductModelID   Name              Result  
---------------  ----------------  ---------------------------------  
7                HL Touring Frame  <Location WCID="45" LaborHrs="0.5"/>   
```  
  
## <a name="implementation-limitations"></a>Limites de mise en œuvre  
 Les limitations suivantes s'appliquent :  
  
-   La fonction **min ()** mappe tous les entiers à XS : Decimal.  
  
-   La fonction **min ()** sur des valeurs de type xs : Duration n’est pas prise en charge.  
  
-   Les séquences faisant intervenir plusieurs types dérivés de différents types de base ne sont pas prises en charge.  
  
-   L'option syntaxique fournissant un classement n'est pas prise en charge.  
  
## <a name="see-also"></a>Voir aussi  
 [Fonctions XQuery impliquant le type de données xml](../xquery/xquery-functions-against-the-xml-data-type.md)  
  
  
