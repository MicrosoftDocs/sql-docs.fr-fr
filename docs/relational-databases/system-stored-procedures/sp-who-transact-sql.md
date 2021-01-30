---
description: sp_who (Transact-SQL)
title: sp_who (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_who_TSQL
- sp_who
dev_langs:
- TSQL
helpviewer_keywords:
- sp_who
ms.assetid: 132dfb08-fa79-422e-97d4-b2c4579c6ac5
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: db60af487d1ee7b7475b0b4d6ec1d8f67decb1ca
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99201795"
---
# <a name="sp_who-transact-sql"></a>sp_who (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Fournit des informations sur les utilisateurs, les sessions et les processus actuels dans une instance du [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] . Les informations peuvent être filtrées afin de retourner uniquement les processus qui ne sont pas inactifs, ou qui appartiennent à un utilisateur ou à une session spécifique.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_who [ [ @loginame = ] 'login' | session ID | 'ACTIVE' ]  
```  
  
## <a name="arguments"></a>Arguments  
`[ @loginame = ] 'login' | session ID | 'ACTIVE'` Est utilisé pour filtrer le jeu de résultats.  
  
 *login* est de **type sysname** et identifie les processus appartenant à une connexion particulière.  
  
 l' *ID de session* est un numéro d’identification de session appartenant à l' [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] instance. l' *ID de session* est **smallint**.  
  
 **Active** exclut les sessions en attente de la prochaine commande de l’utilisateur.  
  
 Si aucune valeur n'est fournie, la procédure répertorie toutes les sessions appartenant à l'instance.  
  
## <a name="return-code-values"></a>Codet de retour  
 0 (réussite) ou 1 (échec)  
  
## <a name="result-sets"></a>Jeux de résultats  
 **sp_who** retourne un jeu de résultats avec les informations suivantes.  
  
|Colonne|Type de données|Description|  
|------------|---------------|-----------------|  
|**spid**|**smallint**|ID de la session.|  
|**ECID**|**smallint**|ID du contexte d'exécution d'un thread donné associé à un ID de session spécifique.<br /><br /> ECID = {0, 1, 2, 3,... *n*}, où 0 représente toujours le thread principal ou parent et {1, 2, 3,... *n*} représente les sous-threads.|  
|**statut**|**nchar(30)**|État du processus Les valeurs possibles sont les suivantes :<br /><br /> **dormant**. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] réinitialise la session.<br /><br /> **en cours d’exécution**. La session exécute un ou plusieurs traitements. Lorsque la fonctionnalité MARS (Multiple Active Result Sets) est activée, une session peut exécuter plusieurs traitements. Pour plus d’informations, consultez [Utilisation de MARS &#40;Multiple Active Result Sets&#41;](../../relational-databases/native-client/features/using-multiple-active-result-sets-mars.md).<br /><br /> **arrière-plan**. La session exécute une tâche en arrière-plan, telle qu'une détection de blocage.<br /><br /> **restauration**. Un processus d'annulation est en cours dans la session.<br /><br /> **en attente**. La session attend qu'un thread de travail soit disponible.<br /><br /> **exécutable**. La tâche de la session figure dans la file d'attente exécutable d'un planificateur lors de l'attente de l'obtention d'un quantum.<br /><br /> **spinloop**. La tâche de la session attend qu'un verrouillage total de l'UC se libère.<br /><br /> **suspendu**. La session attend la fin d'un événement, tel qu'une E/S.|  
|**loginame**|**nchar(128)**|Nom de connexion associé à ce processus particulier|  
|**hostname**|**nchar(128)**|Nom de l'hôte ou de l'ordinateur pour chaque processus|  
|**blk**|**Char (5)**|ID de session du processus bloquant, s'il en existe un. Dans les autres cas, cette colonne a la valeur NULL.<br /><br /> Lorsqu'une transaction associée à un ID de session spécifié est bloquée par une transaction distribuée orpheline, cette colonne renvoie la valeur « -2 » pour la transaction orpheline qui bloque.|  
|**@**|**nchar(128)**|Base de données dont se sert le processus|  
|**cmd**|**nchar(16)**|[!INCLUDE[ssDE](../../includes/ssde-md.md)] commande ( [!INCLUDE[tsql](../../includes/tsql-md.md)] instruction, [!INCLUDE[ssDE](../../includes/ssde-md.md)] processus interne, etc.) en cours d’exécution pour le processus. Dans SQL Server 2019, le type de données a changé en **nchar (26)**.|  
|**request_id**|**int**|ID des demandes s'exécutant dans une session spécifique|  
  
 En cas de traitement parallèle, des sous-threads sont créés pour l'lD de session spécifique. Le thread principal est indiqué sous la forme `spid = <xxx>` et `ecid =0`. Les autres sous-threads ont le même `spid = <xxx>` , mais avec **ECID** > 0.  
  
## <a name="remarks"></a>Notes  
 Un processus bloquant, qui peut disposer d'un verrou exclusif, est un processus qui conserve les ressources dont un autre processus a besoin.  
  
 Toutes les transactions distribuées orphelines reçoivent la valeur d'ID de session « -2 ». Les transactions distribuées orphelines sont des transactions distribuées qui ne sont associées à aucun ID de session. Pour plus d’informations, consultez [Utiliser les transactions marquées pour récupérer des bases de données associées uniformément &#40;mode de récupération complète&#41;](../../relational-databases/backup-restore/use-marked-transactions-to-recover-related-databases-consistently.md).  
  
 Interrogez la colonne **is_user_process** de sys.dm_exec_sessions pour séparer les processus système des processus utilisateur.  
  
## <a name="permissions"></a>Autorisations  
 Nécessite l'autorisation VIEW SERVER STATE sur le serveur pour voir toutes les sessions en cours d'exécution dans l'instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Dans le cas contraire, l'utilisateur ne voit que la session en cours.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-listing-all-current-processes"></a>R. Affichage de tous les processus en cours  
 L'exemple suivant utilise `sp_who` sans paramètres pour donner la liste de tous les utilisateurs actuels.  
  
```  
USE master;  
GO  
EXEC sp_who;  
GO  
```  
  
### <a name="b-listing-a-specific-users-process"></a>B. Affichage des processus d'un utilisateur spécifique  
 L'exemple suivant montre comment afficher des informations sur un utilisateur actuel par nom de connexion.  
  
```  
USE master;  
GO  
EXEC sp_who 'janetl';  
GO  
```  
  
### <a name="c-displaying-all-active-processes"></a>C. Affichage de tous les processus actifs  
  
```  
USE master;  
GO  
EXEC sp_who 'active';  
GO  
```  
  
### <a name="d-displaying-a-specific-process-identified-by-a-session-id"></a>D. Affichage d'un processus spécifique identifié par un ID de session  
  
```  
USE master;  
GO  
EXEC sp_who '10' --specifies the process_id;  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [sp_lock &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-lock-transact-sql.md)   
 [ Processus desys.sys&#40;&#41;Transact-SQL ](../../relational-databases/system-compatibility-views/sys-sysprocesses-transact-sql.md)   
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
