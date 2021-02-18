---
description: Rôles de base de données fixes de SQL Server Agent
title: Rôles de base de données fixes de SQL Server Agent
ms.custom: seo-lt-2019
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- roles [SQL Server], SQL Server Agent
- SQL Server Agent, roles
- SQLAgentUserRole database role
- SQLAgentReaderRole database role
- multiple roles
- security [SQL Server Agent], fixed database roles
- fixed database roles [SQL Server]
- SQLAgentOperatorRole database role
ms.assetid: 719ce56b-d6b2-414a-88a8-f43b725ebc79
author: markingmyname
ms.author: maghan
ms.reviewer: ''
monikerRange: = azuresqldb-mi-current || >= sql-server-2016
ms.openlocfilehash: 57de6b3b9d4282155af520ba53d8ec7aff96f194
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100346110"
---
# <a name="sql-server-agent-fixed-database-roles"></a>Rôles de base de données fixes de SQL Server Agent
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

> [!IMPORTANT]  
> Dans [Azure SQL Managed Instance](/azure/sql-database/sql-database-managed-instance), la plupart, mais pas toutes les fonctionnalités SQL Server Agent sont actuellement prises en charge. Pour plus d’informations, consultez [Différences T-SQL entre Azure SQL Managed Instance et SQL Server](/azure/sql-database/sql-database-managed-instance-transact-sql-information#sql-server-agent).

[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] possède les rôles fixes de base de données **msdb** suivants, qui permettent aux administrateurs de contrôler plus précisément l’accès à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent. Les rôles sont classés ci-après selon leurs privilèges d'accès, du moins privilégié au plus privilégié :  
  
-   **SQLAgentUserRole**  
  
-   **SQLAgentReaderRole**  
  
-   **SQLAgentOperatorRole**  
  
Quand les utilisateurs qui ne sont pas membres de l’un de ces rôles sont connectés à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] dans [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], le nœud **SQL Server Agent** dans l’Explorateur d’objets n’est pas visible. Pour utiliser **Agent, un utilisateur doit être membre de l’un de ces rôles de base de données fixes ou du rôle serveur fixe** sysadmin [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
## <a name="permissions-of-sql-server-agent-fixed-database-roles"></a>Autorisations des rôles de base de données fixes de SQL Server Agent  
Les autorisations des rôles de base de données de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent sont concentriques les unes avec les autres : les rôles les plus privilégiés héritent des autorisations des rôles les moins privilégiés sur les objets de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent (notamment les alertes, les opérateurs, les travaux, les planifications et les proxys). Par exemple, si les membres du rôle **SQLAgentUserRole** le moins privilégié ont l’autorisation d’accéder au proxy_A, les membres des rôles **SQLAgentReaderRole** et **SQLAgentOperatorRole** ont automatiquement accès à ce proxy, même si l’autorisation ne leur a pas été explicitement accordée. Ceci peut avoir des incidences sur la sécurité qui sont décrites dans les sections suivantes par rapport à chaque rôle.  
  
### <a name="sqlagentuserrole-permissions"></a>Autorisations du rôle SQLAgentUserRole  
**SQLAgentUserRole** est le moins privilégié des rôles de base de données fixes de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent. Il dispose uniquement d'autorisations sur les opérateurs, les travaux locaux et les planifications de travaux. Les membres du rôle **SQLAgentUserRole** disposent uniquement d’autorisations sur les travaux locaux et les planifications des travaux qu’ils possèdent. Ils ne peuvent pas utiliser de travaux multiserveur (travaux de serveur maître et cible) et ne peuvent pas modifier l'appartenance des travaux pour accéder aux travaux qui ne leur appartiennent pas encore. Les membres de **SQLAgentUserRole** peuvent uniquement consulter la liste des proxys disponibles dans la boîte de dialogue **Propriétés de l’étape du travail** de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]. Seul le nœud **Travaux** de l’Explorateur d’objets de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] est visible aux membres de **SQLAgentUserRole**.  
  
> [!IMPORTANT]  
> Avant d’accorder l’accès aux proxys aux membres **des** **rôles de base de données de l’Agent** [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], pensez aux incidences que cela pourrait avoir sur la sécurité. Les rôles **SQLAgentReaderRole** et **SQLAgentOperatorRole** sont automatiquement membres du rôle **SQLAgentUserRole**. Ceci signifie que les membres de **SQLAgentReaderRole** et de **SQLAgentOperatorRole** ont accès à tous les proxys de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent auxquels **SQLAgentUserRole** peut accéder et qu’ils peuvent les utiliser.  
  
Le tableau suivant récapitule les autorisations de **SQLAgentUserRole** sur les objets de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent.  
  
|Action|Opérateurs|Travaux locaux<br /><br />(travaux lui appartenant uniquement)|Calendriers de travaux<br /><br />(planifications lui appartenant uniquement)|Proxies|  
|----------|-------------|-----------------------------------|-------------------------------------------|-----------|  
|Créer/modifier/supprimer|Non|Oui<br /><br />Ne peut pas modifier l’appartenance des travaux.|Oui|Non |  
|Afficher la liste (énumérer)|Oui<br /><br />Peut obtenir la liste des opérateurs disponibles à utiliser dans **sp_notify_operator** et dans la boîte de dialogue **Propriétés du travail** de Management Studio.|Oui|Oui|Oui<br /><br />Liste de proxys uniquement disponibles dans la boîte de dialogue **Propriétés de l’étape du travail** de Management Studio.|  
|Activer/désactiver|Non|Oui|Oui|Non applicable|  
|Afficher les propriétés|Non |Oui|Oui|Non |  
|Exécuter/arrêter/démarrer|Non applicable|Oui|Non applicable|Non applicable|  
|Afficher l’historique des travaux|Non applicable|Oui|Non applicable|Non applicable|  
|Supprimer l'historique des travaux|Non applicable|No<br /><br />Pour supprimer l’historique des travaux qui leur appartiennent, les membres de **SQLAgentUserRole** doivent disposer explicitement de l’autorisation EXECUTE sur **sp_purge_jobhistory** . Ils ne peuvent supprimer l'historique d'aucun autre travail.|Non applicable|Non applicable|  
|Attacher/détacher|Non applicable|Non applicable|Oui|Non applicable|  
  
### <a name="sqlagentreaderrole-permissions"></a>Autorisations du rôle SQLAgentReaderRole  
**SQLAgentReaderRole** inclut toutes les autorisations de **SQLAgentUserRole** , ainsi que les autorisations permettant d’afficher la liste des travaux multiserveur disponibles, leurs propriétés et leur historique. Les membres de ce rôle peuvent également afficher la liste de tous les travaux et planifications de travaux disponibles et de leurs propriétés, pas uniquement la liste des travaux et des planifications de travaux dont ils sont propriétaires. Les membres de **SQLAgentReaderRole** ne peuvent pas modifier l’appartenance des travaux pour obtenir l’accès aux travaux qui ne leur appartiennent pas encore. Seul le nœud **Travaux** de l’Explorateur d’objets de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] est visible aux membres de **SQLAgentReaderRole**.  
  
> [!IMPORTANT]  
> Avant d’accorder un accès proxy aux membres **des rôles** [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] **Agentdatabaseroles**, pensez aux incidences que cela pourrait avoir sur la sécurité. Les membres de **SQLAgentReaderRole** sont automatiquement membres de **SQLAgentUserRole**. Ceci signifie que les membres de **SQLAgentReaderRole** ont accès à tous les proxys de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent auxquels **SQLAgentUserRole** peut accéder et qu’ils peuvent les utiliser.  
  
Le tableau suivant récapitule les autorisations de **SQLAgentReaderRole** sur les objets de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent.  
  
|Action|Opérateurs|Travaux locaux|Travaux multiserveur|Calendriers de travaux|Proxies|  
|----------|-------------|--------------|--------------------|-----------------|-----------|  
|Créer/modifier/supprimer|No|Oui (travaux lui appartenant uniquement)<br /><br />Ne peut pas modifier l’appartenance des travaux.|No|Oui (planifications lui appartenant uniquement)|No|  
|Afficher la liste (énumérer)|Oui<br /><br />Peut obtenir la liste des opérateurs disponibles à utiliser dans **sp_notify_operator** et dans la boîte de dialogue **Propriétés du travail** de Management Studio.|Oui|Oui|Oui|Oui<br /><br />Liste de proxys uniquement disponibles dans la boîte de dialogue **Propriétés de l’étape du travail** de Management Studio.|  
|Activer/désactiver|No|Oui (travaux lui appartenant uniquement)|No|Oui (planifications lui appartenant uniquement)|Non applicable|  
|Afficher les propriétés|Non |Oui|Oui|Oui|Non |  
|Modifier les propriétés|No|Oui (travaux lui appartenant uniquement)|No|Oui (planifications lui appartenant uniquement)|No|  
|Exécuter/arrêter/démarrer|Non applicable|Oui (travaux lui appartenant uniquement)|Non|Non applicable|Non applicable|  
|Afficher l’historique des travaux|Non applicable|Oui|Oui|Non applicable|Non applicable|  
|Supprimer l'historique des travaux|Non applicable|No<br /><br />Pour supprimer l’historique des travaux qui leur appartiennent, les membres de **SQLAgentReaderRole** doivent disposer explicitement de l’autorisation EXECUTE sur **sp_purge_jobhistory** . Ils ne peuvent supprimer l'historique d'aucun autre travail.|Non|Non applicable|Non applicable|  
|Attacher/détacher|Non applicable|Non applicable|Non applicable|Oui (planifications lui appartenant uniquement)|Non applicable|  
  
### <a name="sqlagentoperatorrole-permissions"></a>Autorisations du rôle SQLAgentOperatorRole  
**SQLAgentOperatorRole** est le plus privilégié des rôles de base de données fixes de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent. Il inclut toutes les autorisations des rôles **SQLAgentUserRole** et **SQLAgentReaderRole**. Les membres de ce rôle peuvent également afficher les propriétés pour les opérateurs et les proxys, ainsi que dresser la liste des proxys disponibles et des alertes sur le serveur.  
  
Les membres de **SQLAgentOperatorRole** disposent d’autorisations supplémentaires sur les travaux locaux et les planifications. Ils peuvent exécuter, arrêter ou démarrer tous les travaux locaux, ainsi que supprimer l'historique de n'importe quel travail local sur le serveur. Ils peuvent également activer ou désactiver tous les travaux locaux et les planifications sur le serveur. Pour ce faire, les membres de ce rôle doivent utiliser les procédures stockées **sp_update_job** et **sp_update_schedule**. Seuls les paramètres qui spécifient le nom ou l’identificateur du travail ou de la planification et le paramètre **\@enabled** peuvent être spécifiés par les membres de **SQLAgentOperatorRole**. S'ils spécifient d'autres paramètres, l'exécution de ces procédures stockées échoue. Les membres de **SQLAgentOperatorRole** ne peuvent pas modifier l’appartenance des travaux pour obtenir l’accès aux travaux qui ne leur appartiennent pas encore.  
  
Les nœuds **Travaux**, **Alertes**, **Opérateurs** et **Proxies** dans l’Explorateur d’objets de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] sont visibles aux membres de **SQLAgentOperatorRole**. Seul le nœud **Journaux d’erreur** n’est pas visible aux membres de ce rôle.  
  
> [!IMPORTANT]  
> Avant d’accorder un accès proxy aux membres **des rôles** [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] **Agentdatabaseroles**, pensez aux incidences que cela pourrait avoir sur la sécurité. Les membres de **SQLAgentOperatorRole** sont automatiquement membres de **SQLAgentUserRole** et **SQLAgentReaderRole**. Ceci signifie que les membres de **SQLAgentOperatorRole** ont accès à tous les proxys de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent auxquels **SQLAgentUserRole** ou **SQLAgentReaderRole** peut accéder et qu’ils peuvent les utiliser.  
  
Le tableau suivant récapitule les autorisations de **SQLAgentOperatorRole** sur les objets de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent.  
  
|Action|Alertes|Opérateurs|Travaux locaux|Travaux multiserveur|Calendriers de travaux|Proxies|  
|----------|----------|-------------|--------------|--------------------|-----------------|-----------|  
|Créer/modifier/supprimer|Non|Non |Oui (travaux lui appartenant uniquement)<br /><br />Ne peut pas modifier l’appartenance des travaux.|No|Oui (planifications lui appartenant uniquement)|No|  
|Afficher la liste (énumérer)|Oui|Oui<br /><br />Peut obtenir la liste des opérateurs disponibles à utiliser dans **sp_notify_operator** et dans la boîte de dialogue **Propriétés du travail** de Management Studio.|Oui|Oui|Oui|Oui|  
|Activer/désactiver|Non|Non|Oui<br /><br />Les membres de **SQLAgentOperatorRole** peuvent activer ou désactiver les travaux locaux qui ne leur appartiennent pas en utilisant la procédure stockée **sp_update_job** et en attribuant des valeurs aux paramètres **\@enabled** et **\@job_id** (ou **\@job_name**). Si un membre de ce rôle spécifie d'autres paramètres pour cette procédure stockée, l'exécution de cette dernière échoue.|Non |Oui<br /><br />Les membres de **SQLAgentOperatorRole** peuvent activer ou désactiver les planifications qui ne leur appartiennent pas en utilisant la procédure stockée **sp_update_schedule** et en attribuant des valeurs aux paramètres **\@enabled** et **\@schedule_id** (ou **\@name**). Si un membre de ce rôle spécifie d'autres paramètres pour cette procédure stockée, l'exécution de cette dernière échoue.|Non applicable|  
|Afficher les propriétés|Oui|Oui|Oui|Oui|Oui|Oui|  
|Modifier les propriétés|Non|Non |Oui (travaux lui appartenant uniquement)|No|Oui (planifications lui appartenant uniquement)|No|  
|Exécuter/arrêter/démarrer|Non applicable|Non applicable|Oui|Non|Non applicable|Non applicable|  
|Afficher l’historique des travaux|Non applicable|Non applicable|Oui|Oui|Non applicable|Non applicable|  
|Supprimer l'historique des travaux|Non applicable|Non applicable|Oui|Non|Non applicable|Non applicable|  
|Attacher/détacher|Non applicable|Non applicable|Non applicable|Non applicable|Oui (planifications lui appartenant uniquement)|Non applicable|  
  
## <a name="assigning-users-multiple-roles"></a>Assignation de plusieurs rôles aux utilisateurs  
Les membres du rôle serveur fixe **sysadmin** ont accès à toutes les fonctionnalités de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent. Si un utilisateur n’est pas membre du rôle **sysadmin** , mais qu’il est membre de plusieurs rôles de base de données fixes de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent, il est important de prendre en compte le modèle concentrique des autorisations de ces rôles. Étant donné que les rôles plus privilégiés contiennent toujours toutes les autorisations des rôles moins privilégiés, un utilisateur qui est membre de plusieurs rôles a automatiquement les autorisations associées au rôle le plus privilégié dont il est membre.  
  
## <a name="see-also"></a>Voir aussi  
[Implémenter la sécurité de l'Agent SQL Server](../../ssms/agent/implement-sql-server-agent-security.md)  
[sp_update_job (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-update-job-transact-sql.md)  
[sp_update_schedule (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-update-schedule-transact-sql.md)  
[sp_notify_operator (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-notify-operator-transact-sql.md)  
[sp_purge_jobhistory (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-purge-jobhistory-transact-sql.md)  
