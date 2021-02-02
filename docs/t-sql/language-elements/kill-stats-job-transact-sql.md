---
description: KILL STATS JOB (Transact-SQL)
title: KILL STATS JOB (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/27/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- KILL STATS JOB
- KILL_STATS_JOB_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- ending statistics update jobs [SQL Server]
- stopping statistics update jobs
- terminating statistics update jobs
- asynchronous statistics updates [SQL Server]
- KILL STATS JOB statement
- statistics update jobs [SQL Server]
ms.assetid: 55a8f9f1-3259-45c0-8ab9-60b9c088b4b4
author: cawrites
ms.author: chadam
ms.openlocfilehash: d52e417bef5c998d381ee9720d626d39430d8fee
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99113124"
---
# <a name="kill-stats-job-transact-sql"></a>KILL STATS JOB (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Met fin à un travail de mise à jour asynchrone des statistiques dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql
KILL STATS JOB job_id   
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments
 *job_id*  
 Champ job_id renvoyé par la vue de gestion dynamique sys.dm_exec_background_job_queue du travail.  
  
## <a name="remarks"></a>Notes  
 L'argument job_id n'est pas lié à session_id ou à l'unité de travail utilisée dans d'autres formes de l'instruction KILL.  
  
## <a name="permissions"></a>Autorisations  
 L'utilisateur doit bénéficier de l'autorisation VIEW SERVER STATE pour accéder aux données de la vue de gestion dynamique sys.dm_exec_background_job_queue.  
  
 Les autorisations de l'instruction KILL STATS JOB sont octroyées par défaut aux membres des rôles de base de données fixes sysadmin et processadmin et ne sont pas transmissibles.  
  
## <a name="examples"></a>Exemples  
 L’exemple suivant montre comment mettre fin à la mise à jour des statistiques associée à un travail où *job_id* = `53`.  
  
```sql  
KILL STATS JOB 53;  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [KILL &#40;Transact-SQL&#41;](../../t-sql/language-elements/kill-transact-sql.md)   
 [KILL QUERY NOTIFICATION SUBSCRIPTION &#40;Transact-SQL&#41;](../../t-sql/language-elements/kill-query-notification-subscription-transact-sql.md)   
 [sys.dm_exec_background_job_queue &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-background-job-queue-transact-sql.md)   
 [Statistiques](../../relational-databases/statistics/statistics.md)  
  
  
