---
description: UPDATE, commande SQL
title: Commande UPDATE-SQL | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- update [ODBC]
ms.assetid: ff1e0331-c060-4304-b280-039725b45f63
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 3a3e38ad98f69faf2ed6288c767d43167314465b
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99209826"
---
# <a name="update---sql-command"></a>UPDATE, commande SQL
Met à jour des enregistrements dans une table avec de nouvelles valeurs.  
  
 Le pilote ODBC Visual FoxPro prend en charge la syntaxe du langage Visual FoxPro natif pour cette commande. Pour obtenir des informations spécifiques au pilote, consultez la **section Remarques** sur le pilote.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
UPDATE [DatabaseName1!]TableName1  
SET Column_Name1 = eExpression1  
   [, Column_Name2 = eExpression2 ...]  
   WHERE FilterCondition1 [AND | OR FilterCondition2 ...]  
```  
  
## <a name="arguments"></a>Arguments  
 MISE à jour [ *DatabaseName1 !*] *TableName1*  
 Spécifie la table dans laquelle les enregistrements sont mis à jour avec de nouvelles valeurs.  
  
 *DatabaseName1!* Spécifie le nom d’une base de données autre que la base de données spécifiée avec la source de données contenant la table. Vous devez inclure le nom de la base de données contenant la table si celle-ci n’est pas la base de données actuelle. Insérez le délimiteur du point d’exclamation ( !) après le nom de la base de données et avant le nom de la table.  
  
 Set *Column_Name1* =  *eExpression1*[, *Column_Name2* =  *eExpression2*  
 Spécifie les colonnes mises à jour et leurs nouvelles valeurs. Si vous omettez la clause WHERE, chaque ligne de la colonne est mise à jour avec la même valeur.  
  
 WHERE *FilterCondition1*[et &#124; ou *FilterCondition2*...]  
 Spécifie les enregistrements mis à jour avec les nouvelles valeurs.  
  
 *FilterCondition* spécifie les critères que les enregistrements doivent remplir pour être mis à jour avec de nouvelles valeurs. Vous pouvez inclure autant de conditions de filtre que vous le souhaitez, en les connectant à l’aide de l’opérateur AND ou OR. Vous pouvez également utiliser l’opérateur NOT pour inverser la valeur d’une expression logique, ou vous pouvez utiliser **Empty**() pour rechercher un champ vide.  
  
## <a name="remarks"></a>Notes  
 UPDATE-SQL ne peut mettre à jour que les enregistrements d’une seule table.  
  
 Contrairement à Replace, UPDATE-SQL utilise le verrouillage des enregistrements lors de la mise à jour de plusieurs enregistrements dans des tables ouvertes pour un accès partagé. Cela réduit la contention des enregistrements dans les situations multi-utilisateur, mais peut réduire les performances. Pour des performances maximales, ouvrez la table pour une utilisation exclusive ou utilisez **troupeau**() pour verrouiller la table.  
  
## <a name="driver-remarks"></a>Remarques sur le pilote  
 Lorsque votre application envoie la mise à jour de l’instruction SQL ODBC à la source de données, le pilote ODBC Visual FoxPro convertit la commande en commande Visual FoxProUPDATE sans traduction.  
  
## <a name="see-also"></a>Voir aussi  
 [DELETE-commande SQL](../../odbc/microsoft/delete-sql-command.md)   
 [INSERT, commande SQL](../../odbc/microsoft/insert-sql-command.md)
