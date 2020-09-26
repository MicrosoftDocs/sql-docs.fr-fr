---
description: DROP EVENT SESSION (Transact-SQL)
title: DROP EVENT SESSION (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- DROP_EVENT_SESSION_TSQL
- DROP EVENT SESSION
dev_langs:
- TSQL
helpviewer_keywords:
- event sessions [SQL Server]
- DROP EVENT SESSION statement
ms.assetid: 92eabe4b-24e2-43b1-978c-31a199964b90
author: MightyPen
ms.author: genemi
ms.openlocfilehash: 9f26a89ed0da4a6521a339cd837e81b6c5b3369a
ms.sourcegitcommit: 197a6ffb643f93592edf9e90b04810a18be61133
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/26/2020
ms.locfileid: "91378814"
---
# <a name="drop-event-session-transact-sql"></a>DROP EVENT SESSION (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Supprime une session d'événements.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql    
DROP EVENT SESSION event_session_name  
ON SERVER  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments
 *event_session_name*  
 Nom d'une session d'événements existante.  
  
## <a name="remarks"></a>Remarques  
 Lorsque vous supprimez une session d'événements, toutes les informations de configuration, telles que les cibles et les paramètres de session, sont totalement supprimées.  
  
## <a name="permissions"></a>Autorisations  
 Nécessite l’autorisation `ALTER ANY EVENT SESSION`.  
  
## <a name="examples"></a>Exemples  
L'exemple suivant montre comment supprimer une session d'événements.  
  
```sql  
DROP EVENT SESSION evt_spin_lock_diagnosis ON SERVER;
GO
```  
  
## <a name="see-also"></a>Voir aussi  
 [CREATE EVENT SESSION &#40;Transact-SQL&#41;](../../t-sql/statements/create-event-session-transact-sql.md)   
 [ALTER EVENT SESSION &#40;Transact-SQL&#41;](../../t-sql/statements/alter-event-session-transact-sql.md)   
 [sys.server_event_sessions &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-server-event-sessions-transact-sql.md)  
  
  
