---
description: DELETE, commande SQL
title: DELETE-commande SQL | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- DELETE [ODBC]
ms.assetid: 0d5bd477-626f-4f22-a05a-f531d9f8c5e7
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 42c5ff48b0ac330c2a305650141ada2255629c1f
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99200509"
---
# <a name="delete---sql-command"></a>DELETE, commande SQL
Marque les enregistrements à supprimer.  
  
 Le pilote ODBC Visual FoxPro prend en charge la syntaxe du langage Visual FoxPro natif pour cette commande. Pour obtenir des informations spécifiques au pilote, consultez les notes.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
DELETE FROM [DatabaseName!]TableName  
   [WHERE FilterCondition1 [AND | OR FilterCondition2 ...]]  
```  
  
## <a name="arguments"></a>Arguments  
 FROM [ *DatabaseName !*] *TableName*  
 Spécifie la table dans laquelle les enregistrements sont marqués pour suppression.  
  
 *DatabaseName !* Spécifie le nom d’une base de données qui contient la table si la base de données conteneur n’est pas la base de données spécifiée avec la source de données. Vous devez inclure le nom d’une base de données qui contient la table si la base de données n’est pas la base de données spécifiée avec la source de données. Insérez le délimiteur du point d’exclamation ( !) après le nom de la base de données et avant le nom de la table.  
  
 WHERE *FilterCondition1*[et &#124; ou *FilterCondition2*...]  
 Spécifie que Visual FoxPro marque uniquement certains enregistrements pour la suppression.  
  
 *FilterCondition* spécifie les critères que les enregistrements doivent remplir pour être marqués pour suppression. Vous pouvez inclure autant de conditions de filtre que vous le souhaitez, en les connectant à l’aide de l’opérateur AND ou OR. Vous pouvez également utiliser l’opérateur NOT pour inverser la valeur d’une expression logique, ou vous pouvez utiliser **Empty**() pour rechercher un champ vide.  
  
## <a name="remarks"></a>Notes  
 Si SET DELETEd a la valeur ON, les enregistrements marqués pour suppression sont ignorés par toutes les commandes qui incluent une étendue.  
  
 DELETE-SQL utilise le verrouillage des enregistrements lors du marquage de plusieurs enregistrements à supprimer dans les tables ouvertes pour un accès partagé. Cela réduit la contention des enregistrements dans les situations multi-utilisateur, mais peut réduire les performances. Pour des performances maximales, ouvrez la table pour une utilisation exclusive.  
  
## <a name="driver-remarks"></a>Remarques sur le pilote  
 Lorsque votre application envoie la suppression de l’instruction SQL ODBC à la source de données, le pilote ODBC Visual FoxPro convertit la commande en commande Visual FoxPro DELETE sans traduction.  
  
## <a name="see-also"></a>Voir aussi  
 [SET DELETED, commande](../../odbc/microsoft/set-deleted-command.md)
