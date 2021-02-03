---
description: DROP BROKER PRIORITY (Transact-SQL)
title: DROP BROKER PRIORITY (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- DROP_BROKER_PRIORITY_TSQL
- DROP BROKER PRIORITY
dev_langs:
- TSQL
helpviewer_keywords:
- DROP BROKER PRIORITY statement
ms.assetid: 09ee6c5b-af94-4a4b-a0e2-f9eac50e43aa
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 5bb14e6314d9749f469c2084abad86ecfd5e51b3
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99203553"
---
# <a name="drop-broker-priority-transact-sql"></a>DROP BROKER PRIORITY (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Supprime une priorité de conversation de la base de données active.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql
DROP BROKER PRIORITY ConversationPriorityName  
[;]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments
 *ConversationPriorityName*  
 Spécifie le nom de la priorité de conversation à supprimer.  
  
## <a name="remarks"></a>Notes  
 Lorsque vous supprimez une priorité de conversation, toutes les conversations existantes continuent de fonctionner avec les niveaux de priorité qui leur ont été attribués par la priorité de conversation.  
  
## <a name="permissions"></a>Autorisations  
 L'autorisation de création d'une priorité de conversation est accordée par défaut aux membres du rôle de base de données fixe db_ddladmin ou db_owner, ainsi qu'aux membres du rôle serveur fixe sysadmin. Nécessite l'autorisation ALTER sur la base de données.  
  
## <a name="examples"></a>Exemples  
 L'exemple suivant supprime la priorité de conversation nommée `InitiatorAToTargetPriority`.  
  
```sql  
DROP BROKER PRIORITY InitiatorAToTargetPriority;
```  
  
## <a name="see-also"></a>Voir aussi  
 [ALTER BROKER PRIORITY &#40;Transact-SQL&#41;](../../t-sql/statements/alter-broker-priority-transact-sql.md)   
 [CREATE BROKER PRIORITY &#40;Transact-SQL&#41;](../../t-sql/statements/create-broker-priority-transact-sql.md)   
 [sys.conversation_priorities &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-conversation-priorities-transact-sql.md)  
  
  
