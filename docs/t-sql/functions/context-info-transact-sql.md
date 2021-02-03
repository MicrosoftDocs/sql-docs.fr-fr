---
description: CONTEXT_INFO  (Transact-SQL)
title: CONTEXT_INFO  (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/24/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- CONTEXT_INFO_TSQL
- CONTEXT_INFO
dev_langs:
- TSQL
helpviewer_keywords:
- CONTEXT_INFO function
- Multiple Active Result Sets
- context information [SQL Server]
- MARS [SQL Server]
- session context information [SQL Server]
ms.assetid: 571320f5-7228-4b0e-9d01-ab732d2d1eab
author: cawrites
ms.author: chadam
ms.openlocfilehash: 1bd11d94547782ffee117e6ed883158423c52180
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99184225"
---
# <a name="context_info--transact-sql"></a>CONTEXT_INFO  (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Cette fonction renvoie la valeur **context_info** définie pour la session ou le traitement actif, ou dérivée à l’aide de l’instruction [SET CONTEXT_INFO](../../t-sql/statements/set-context-info-transact-sql.md).
  
![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql
CONTEXT_INFO()  
```  

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="return-value"></a>Valeur renvoyée
La valeur **context_info**.
  
Si **context_info** n’était pas défini :
-   [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] retourne une valeur NULL.  
-   [!INCLUDE[ssSDS](../../includes/sssds-md.md)] retourne un GUID unique spécifique à la session.  
  
## <a name="remarks"></a>Remarques  
La fonctionnalité MARS (Multiple Active Result Sets) permet aux applications d'exécuter simultanément plusieurs traitements ou requêtes sur la même connexion. Lorsqu'un des traitements d’une connexion MARS exécute la procédure SET CONTEXT_INFO, la fonction `CONTEXT_INFO` renvoie la nouvelle valeur de contexte lorsque la fonction `CONTEXT_INFO` est exécutée dans le même traitement que l'instruction SET. Si la fonction `CONTEXT_INFO` s’exécute dans un ou plusieurs autres traitements de la connexion, la fonction `CONTEXT_FUNCTION` ne retourne pas la nouvelle valeur, sauf si ces traitements ont démarré après l’achèvement du traitement qui a exécuté l’instruction SET.
  
## <a name="permissions"></a>Autorisations  
Ne nécessite aucune autorisation particulière. Les vues système suivantes stockent les informations de contexte, mais l’interrogation directe de ces vues requiert les autorisations SELECT et VIEW SERVER STATE :
- **sys.dm_exec_requests**
- **sys.dm_exec_sessions**
- **sys.sysprocesses**
  
## <a name="examples"></a>Exemples  
Cet exemple simple attribue à `0x1256698456` la valeur **context_info**, puis utilise la fonction `CONTEXT_INFO` pour extraire la valeur.
  
```sql
SET CONTEXT_INFO 0x1256698456;  
GO  
SELECT CONTEXT_INFO();  
GO  
```  
  
## <a name="see-also"></a>Voir aussi
[SET CONTEXT_INFO &#40;Transact-SQL&#41;](../../t-sql/statements/set-context-info-transact-sql.md)
  
  
