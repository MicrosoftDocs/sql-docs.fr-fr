---
description: '&#x40;&#x40;FETCH_STATUS (Transact-SQL)'
title: FETCH_STATUS (Transact-SQL)
ms.custom: ''
ms.date: 09/18/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- '@@FETCH_STATUS'
- '@@FETCH_STATUS_TSQL'
dev_langs:
- TSQL
helpviewer_keywords:
- FETCH statement
- status information [SQL Server], FETCH
- '@@FETCH_STATUS function'
ms.assetid: 93659193-e4ff-4dfb-9043-0c4114921b91
author: cawrites
ms.author: chadam
ms.openlocfilehash: ef3c22679bed84906f325095f28155dd34f16a09
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/11/2021
ms.locfileid: "98092319"
---
# <a name="x40x40fetch_status-transact-sql"></a>&#x40;&#x40;FETCH_STATUS (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Cette fonction retourne l’état de la dernière instruction FETCH effectuée sur l’un des curseurs actuellement ouverts par la connexion.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql
@@FETCH_STATUS  
```  

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="return-type"></a>Type de retour  
 **integer**  
  
## <a name="return-value"></a>Valeur de retour  
  
|Valeur retournée|Description|  
|------------------|-----------------|  
|&nbsp;0|L'instruction FETCH a réussi.|  
|-1|L'instruction FETCH a échoué ou la ligne se situait au-delà du jeu de résultats.|  
|-2|La ligne recherchée est manquante.|
|-9|Le curseur n’effectue pas d’opération de récupération (fetch).|  
  
## <a name="remarks"></a>Remarques  
La fonction `@@FETCH_STATUS` étant commune à tous les curseurs de la connexion, utilisez-la avec précaution. Après l’exécution d’une instruction FETCH, le test `@@FETCH_STATUS` doit avoir lieu avant qu’une autre instruction FETCH ne soit effectuée sur un autre curseur. `@@FETCH_STATUS` n’est pas défini tant qu’aucune récupération ne s’est produite sur la connexion.  
  
Supposons, par exemple, qu’un utilisateur exécute une instruction FETCH sur un curseur, puis appelle une procédure stockée qui ouvre et traite les résultats sur un autre curseur. Lorsque le contrôle est retourné à partir de cette procédure stockée, `@@FETCH_STATUS` reflète la dernière instruction FETCH exécutée au sein de la procédure stockée et non celle qui avait eu lieu avant l’appel de la procédure stockée.  
  
Pour récupérer le dernier état de récupération (fetch) d’un curseur spécifique, interrogez la colonne **fetch_status** de la fonction de gestion dynamique **sys.dm_exec_cursors**.  
  
## <a name="examples"></a>Exemples  
Cet exemple utilise `@@FETCH_STATUS` pour contrôler les activités d’un curseur dans une boucle `WHILE`.  
  
```sql  
DECLARE Employee_Cursor CURSOR FOR  
SELECT BusinessEntityID, JobTitle  
FROM AdventureWorks2012.HumanResources.Employee;  
OPEN Employee_Cursor;  
FETCH NEXT FROM Employee_Cursor;  
WHILE @@FETCH_STATUS = 0  
   BEGIN  
      FETCH NEXT FROM Employee_Cursor;  
   END;  
CLOSE Employee_Cursor;  
DEALLOCATE Employee_Cursor;  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Fonctions de curseur &#40;Transact-SQL&#41;](../../t-sql/functions/cursor-functions-transact-sql.md)   
 [FETCH &#40;Transact-SQL&#41;](../../t-sql/language-elements/fetch-transact-sql.md)  
  
  
