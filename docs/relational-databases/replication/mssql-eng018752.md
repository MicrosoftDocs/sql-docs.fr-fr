---
description: MSSQL_ENG018752
title: MSSQL_ENG018752 | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
helpviewer_keywords:
- MSSQL_ENG018752 error
ms.assetid: 405b2655-acb4-4e15-bcc6-b8f86bb22b37
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-mi-current||>=sql-server-2016
ms.openlocfilehash: 00d3e098ca0b3686a96be5741d2800973d632147
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99205619"
---
# <a name="mssql_eng018752"></a>MSSQL_ENG018752
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]
    
## <a name="message-details"></a>Détails du message  
  
|Attribut|Valeur|  
|-|-|  
|Nom du produit|SQL Server|  
|ID de l’événement|18752|  
|Source de l’événement|MSSQLSERVER|  
|Composant|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]|  
|Nom symbolique||  
|Texte du message|Un seul Agent de lecture du journal ou une seule procédure liée au journal (sp_repldone, sp_replcmds et sp_replshowcmds) peut se connecter à une base de données à la fois. Si vous avez exécuté une procédure liée au journal, supprimez la connexion à travers laquelle fut exécutée la procédure ou exécutez sp_replflush sur cette connexion avant de démarrer l'Agent de lecture du journal ou d'exécuter toute autre procédure liée au journal.|  
  
## <a name="explanation"></a>Explication  
 Plusieurs connexions tentent actuellement d'exécuter l'une des procédures suivantes : **sp_repldone**, **sp_replcmds** ou **sp_replshowcmds**. Les procédures stockées [sp_repldone &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-repldone-transact-sql.md) et [sp_replcmds &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-replcmds-transact-sql.md) sont utilisées par l’Agent de lecture du journal pour détecter et mettre à jour les informations relatives aux transactions répliquées dans une base de données publiée. La procédure stockée [sp_replshowcmds &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-replshowcmds-transact-sql.md) est utilisée pour résoudre certains problèmes rencontrés en réplication transactionnelle.  
  
 Cette erreur se produit dans les circonstances suivantes :  
  
-   Si l'Agent de lecture du journal d'une base de données publiée est en cours d'exécution et si un deuxième Agent de lecture de journal tente de s'exécuter sur la même base de données, cette erreur est émise pour le second agent et apparaît dans l'historique de l'agent.  
  
     Dans les situations où plusieurs agents sont impliqués, il est possible que l'un d'entre eux résulte d'un processus orphelin.  
  
-   Si l'Agent de lecture du journal d'une base de données publiée est démarré et qu'un utilisateur exécute **sp_repldone**, **sp_replcmds** ou **sp_replshowcmds** sur la même base de données, cette erreur est émise dans l'application où la procédure stockée a été exécutée (par exemple **sqlcmd**).  
  
-   Si aucun Agent de lecture de journal n'est en cours d'exécution sur une base de données publiée et si un utilisateur exécute **sp_repldone**, **sp_replcmds** ou **sp_replshowcmds** , puis omet de fermer la connexion sur laquelle la procédure a été exécutée, cette erreur est générée lorsque l'Agent de lecture de journal tente de se connecter à la base de données.  
  
## <a name="user-action"></a>Action de l'utilisateur  
 La procédure suivante peut vous aider à résoudre ce problème. Si l'une des étapes permet à l'Agent de lecture de journal de démarrer sans erreur, il n'est pas nécessaire d'exécuter le reste de la procédure.  
  
-   Vérifiez dans l'historique de l'Agent de lecture de journal s'il y a d'autres erreurs qui pourraient être cause de cette erreur. Pour obtenir des informations sur l’affichage de l’état de l’agent et des détails de l’erreur dans le moniteur de réplication, consultez [Afficher des informations et effectuer des tâches avec le moniteur de réplication](../../relational-databases/replication/monitor/view-information-and-perform-tasks-replication-monitor.md).  
  
-   Recherchez dans le résultat de [sp_who &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-who-transact-sql.md) s’il existe des numéros spécifiques d’identification de processus (SPID) relatifs à la base de données publiée. Fermez toute connexion susceptible d'avoir exécuté **sp_repldone**, **sp_replcmds** ou **sp_replshowcmds**.  
  
-   Redémarrez l'Agent de lecture du journal. Pour plus d’informations, consultez [Démarrer et arrêter un Agent de réplication &#40;SQL Server Management Studio&#41;](../../relational-databases/replication/agents/start-and-stop-a-replication-agent-sql-server-management-studio.md).  
  
-   Redémarrez le service de l'Agent [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (mettez-le hors ligne ou en ligne dans un cluster) sur le serveur de distribution. S'il est possible qu'une tâche planifiée ait exécuté **sp_repldone**, **sp_replcmds** ou **sp_replshowcmds** à partir d'autres instances [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , redémarrez également l'Agent [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pour ces instances. Pour plus d’informations, consultez [Démarrer, arrêter ou suspendre le service SQL Server Agent](../../ssms/agent/start-stop-or-pause-the-sql-server-agent-service.md).  
  
-   Exécutez [sp_replflush &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-replflush-transact-sql.md) sur le serveur de publication sur la base de données de publication, puis redémarrez l’Agent de lecture du journal.  
  
-   Si l'erreur continue de se produire, augmentez le facteur de journalisation de l'agent et spécifiez un fichier de sortie pour le journal. En fonction du contexte de l'erreur, cette action peut fournir des pistes conduisant à l'erreur et/ou à d'autres messages d'erreur.  
  
## <a name="see-also"></a>Voir aussi  
 [Guide de référence des erreurs et des événements &#40;réplication&#41;](../../relational-databases/replication/errors-and-events-reference-replication.md)   
 [Agent de lecture du journal des réplications](../../relational-databases/replication/agents/replication-log-reader-agent.md)  
  
