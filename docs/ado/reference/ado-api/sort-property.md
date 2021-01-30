---
description: Sort, propriété
title: Sort, propriété | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Recordset15::get_Sort
- Recordset15::put_Sort
- Recordset15::PutSort
- Recordset15::GetSort
- Recordset15::Sort
helpviewer_keywords:
- DESC [ADO]
- ASC [ADO]
- Sort property [ADO]
ms.assetid: 3683ffa0-6f93-4906-9533-ef6942f24f39
author: rothja
ms.author: jroth
ms.openlocfilehash: 57f46248dc23d92752a6354a557285daeaf0a0f8
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99170243"
---
# <a name="sort-property"></a>Sort, propriété
Indique un ou plusieurs noms de champs sur lesquels le [Recordset](./recordset-object-ado.md) est trié, et indique si chaque champ est trié par ordre croissant ou décroissant.  
  
## <a name="settings-and-return-values"></a>Paramètres et valeurs de retour  
 Définit ou retourne une valeur de **chaîne** qui indique les noms de champs dans le **jeu d’enregistrements** sur lequel effectuer le tri. Chaque nom est séparé par une virgule et est éventuellement suivi d’un vide et du mot clé **ASC**, qui trie le champ par ordre croissant, ou **desc**, qui trie le champ dans l’ordre décroissant. Par défaut, si aucun mot clé n’est spécifié, le champ est trié dans l’ordre croissant.  
  
## <a name="remarks"></a>Notes  
 Cette propriété requiert que la propriété [CursorLocation](./cursorlocation-property-ado.md) soit définie sur **adUseClient**. Un index temporaire sera créé pour chaque champ spécifié dans la propriété de **Tri** si un index n’existe pas déjà.  
  
 L’opération de tri est efficace, car les données ne sont pas réorganisées physiquement, mais elles sont simplement accessibles dans l’ordre spécifié par l’index.  
  
 Lorsque la valeur de la propriété **sort** n’est pas une chaîne vide, l’ordre de la propriété de **Tri** est prioritaire par rapport à l’ordre spécifié dans une clause **order by** incluse dans l’instruction SQL utilisée pour ouvrir le **Recordset**.  
  
 Il n’est pas nécessaire d’ouvrir le **jeu d’enregistrements** avant d’accéder à la propriété de **Tri** . elle peut être définie à tout moment après l’instanciation de l’objet **Recordset** .  
  
 Si vous affectez une chaîne vide à la propriété de **Tri** , les lignes sont réinitialisées dans leur ordre d’origine et les index temporaires sont supprimés. Les index existants ne seront pas supprimés.  
  
 Supposons qu’un **Recordset** contienne trois champs nommés *FirstName*, *MiddleInitial* et *LastName*. Affectez à la propriété **sort** la chaîne « `lastName DESC, firstName ASC` », qui ordonne le **jeu d’enregistrements** par nom de famille dans l’ordre décroissant, puis par le prénom dans l’ordre croissant. L’initiale du deuxième prénom est ignorée.  
  
 Aucun champ ne peut être nommé « ASC » ou « DESC », car ces noms sont en conflit avec les mots clés **ASC** et **desc**. Vous pouvez créer un alias pour un champ avec un nom en conflit en utilisant le mot clé **As** dans la requête qui retourne le **Recordset**.  
  
## <a name="applies-to"></a>S'applique à  
 [Recordset, objet (ADO)](./recordset-object-ado.md)  
  
## <a name="see-also"></a>Voir aussi  
 [Sort, exemple de propriété (VB)](./sort-property-example-vb.md)   
 [Sort, exemple de propriété (VC + +)](./sort-property-example-vc.md)   
 [Property-Dynamic d’optimisation (ADO)](./optimize-property-dynamic-ado.md)   
 [SortColumn, propriété (RDS)](../rds-api/sortcolumn-property-rds.md)   
 [SortDirection, propriété (RDS)](../rds-api/sortdirection-property-rds.md)