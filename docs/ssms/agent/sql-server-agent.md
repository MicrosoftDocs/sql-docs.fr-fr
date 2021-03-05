---
description: SQL Server Agent
title: SQL Server Agent
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- SQL Server Agent, about SQL Server Agent
- automatic administration steps
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 03/03/2021
monikerRange: = azuresqldb-mi-current || >= sql-server-2016
ms.openlocfilehash: 751a627530b7d1017bfd8f0f89aa626ea84bc35c
ms.sourcegitcommit: ca81fc9e45fccb26934580f6d299feb0b8ec44b7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/05/2021
ms.locfileid: "102186536"
---
# <a name="sql-server-agent"></a>SQL Server Agent

[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

> [!IMPORTANT]
> Dans [Azure SQL Managed Instance](/azure/sql-database/sql-database-managed-instance), la plupart, mais pas toutes les fonctionnalités SQL Server Agent sont actuellement prises en charge. Pour plus d’informations, consultez [Différences T-SQL entre Azure SQL Managed Instance et SQL Server](/azure/sql-database/sql-database-managed-instance-transact-sql-information#sql-server-agent).

SQL Server Agent est un service Microsoft Windows qui exécute des tâches d’administration planifiées, qui sont appelées *travaux* dans SQL Server.

## <a name="benefits-of-sql-server-agent"></a><a name="Benefits"></a>Avantages de SQL Server Agent

SQL Server Agent utilise SQL Server pour stocker des informations sur les travaux. Les travaux contiennent une ou plusieurs étapes de travail. Chaque étape contient sa propre tâche, par exemple, la sauvegarde d'une base de données.

SQL Server Agent peut exécuter un travail selon une planification, en réponse à un événement spécifique ou sur demande. Par exemple, si vous sauvegardez l’ensemble des serveurs de l’entreprise chaque jour ouvrable après les horaires de bureau, vous pouvez automatiser cette tâche. Planifiez l’exécution de la sauvegarde après 22h00 du lundi au vendredi. Si la sauvegarde rencontre un problème, SQL Server Agent peut enregistrer l’événement et vous en avertir.

> [!NOTE]
> Par défaut, le service SQL Server Agent est désactivé quand SQL Server est installé, sauf si l’utilisateur choisit explicitement de démarrer automatiquement le service.

## <a name="sql-server-agent-components"></a><a name="Components"></a>Composants de SQL Server Agent

SQL Server Agent utilise les composants ci-après pour définir les tâches à exécuter et quand, et pour signaler si elles ont réussi ou échoué.

### <a name="jobs"></a>travaux

Un *travail* est une série d’actions spécifiées effectuées par SQL Server Agent. Utilisez des travaux pour définir une tâche d’administration qui peut être exécutée une ou plusieurs fois, et dont la réussite ou l’échec sont surveillés. Un travail peut s’exécuter sur un serveur local ou sur plusieurs serveurs distants.

> [!IMPORTANT]
> Les travaux SQL Server Agent qui s’exécutent au moment d’un événement de basculement sur une instance de cluster de basculement SQL Server ne reprennent pas sur un autre nœud de cluster de basculement après le basculement. Les travaux SQL Server Agent qui s’exécutent au moment où un nœud Hyper-V est mis en pause ne reprennent pas si la pause provoque un basculement vers un autre nœud. Les travaux qui commencent mais ne peuvent pas se terminer à cause d'un événement de basculement sont enregistrés comme commencés, mais n'affichent pas d'entrées de journal supplémentaires pour l'achèvement ou l'échec. Les travaux SQL Server Agent dans ces scénarios apparaissent comme ne s’étant jamais terminés.

Vous pouvez exécuter un travail :

- en fonction d'une ou plusieurs planifications ;

- en réponse à une ou plusieurs alertes ;

- en exécutant la procédure stockée sp_start_job.

Chaque action d'un travail est appelée *étape du travail*. Par exemple, une étape du travail peut être l’exécution d’une instruction Transact\-SQL, l’exécution d’un package SSIS ou l’envoi d’une commande à un serveur Analysis Services. Les étapes du travail sont gérées dans le cadre d'un travail.

Chaque étape s'exécute dans un contexte de sécurité spécifique. Pour les étapes du travail qui utilisent Transact\-SQL, utilisez l’instruction EXECUTE AS pour définir le contexte de l’étape du travail. Pour les autres types d'étapes, utilisez un compte proxy.

### <a name="schedules"></a>Planifications

Une *planification* programme l'exécution d'un travail. Plusieurs travaux peuvent s’exécuter sur la même planification, et plusieurs planifications peuvent appliquer le même travail. Une planification peut définir les conditions suivantes pour l’heure d’exécution d’un travail :

- À chaque démarrage de SQL Server Agent.

- Quand l’utilisation du processeur atteint le niveau d’inactivité que vous avez défini.

- ponctuellement, à une date et une heure spécifiques ;

- Selon une planification récurrente.

Pour plus d’informations, consultez [Créer des planifications et les attacher à des travaux](../../ssms/agent/create-and-attach-schedules-to-jobs.md).

### <a name="alerts"></a>Alertes

Une *alerte* est une réponse automatique à un événement donné. Par exemple, un événement peut être un travail qui démarre ou des ressources système qui atteignent un seuil spécifique. Vous définissez les conditions selon lesquelles une alerte est déclenchée.

Une alerte peut être une réponse à :

- Événements SQL Server

- Conditions des performances de SQL Server

- Des événements Windows Management Instrumentation sur l'ordinateur qui exécute SQL Server Agent.

Une alerte peut :

- prévenir un ou plusieurs opérateurs ;

- Exécuter une tâche

Pour plus d’informations, consultez [Alertes](../../ssms/agent/alerts.md).  

### <a name="operators"></a>Opérateurs

Un *opérateur* définit les informations de contact pour une personne responsable de la maintenance d’une ou plusieurs instances de SQL Server. Dans certaines sociétés, les responsabilités d'opérateur sont affectées à une seule personne. Dans les entreprises qui ont de nombreux serveurs, plusieurs personnes peuvent se partager les responsabilités d'opérateur. Un opérateur ne contient pas d’informations de sécurité et ne définit pas de principal de sécurité.  

SQL Server peut envoyer des alertes aux opérateurs via...

- Messagerie électronique

- Radiomessagerie (via e-mail)

- **net send**

> [!NOTE]
> Pour envoyer des notifications en utilisant **net send**, le service Windows Messenger doit être actif sur l’ordinateur où SQL Server Agent se trouve.

> [!IMPORTANT]
> Les options de récepteur de radiomessagerie et **net send** seront supprimées de SQL Server Agent dans une version future de SQL Server. Évitez d'utiliser ces fonctionnalités dans une nouvelle tâche de développement et prévoyez de modifier les applications qui les utilisent actuellement.  

Pour envoyer des notifications aux opérateurs par e-mail ou par radiomessagerie, vous devez configurer SQL Server Agent pour qu’il utilise Database Mail. Pour plus d’informations, consultez [Messagerie de base de données](../../relational-databases/database-mail/database-mail.md).

Un opérateur peut être l'alias d'un groupe d'individus. De cette façon, tous les membres de cet alias ne sont pas avertis en même temps. Pour plus d’informations, consultez [Opérateurs](../../ssms/agent/operators.md).

## <a name="security-for-sql-server-agent-administration"></a><a name="Security"></a>Sécurité pour l'administration de SQL Server Agent

SQL Server Agent utilise les rôles de base de données fixes **SQLAgentUserRole**, **SQLAgentReaderRole** et **SQLAgentOperatorRole** de la base de données **msdb** pour contrôler l’accès à SQL Server Agent pour les utilisateurs qui ne sont pas membres du rôle serveur fixe **sysadmin**. Outre ces rôles de base de données fixes, les sous-systèmes et les proxys aident les administrateurs de base de données à garantir que chaque étape de travail est exécutée avec les autorisations minimales nécessaires à la réalisation de cette tâche.  

### <a name="roles"></a>Rôles

Les membres des rôles de base de données fixes **SQLAgentUserRole**, **SQLAgentReaderRole** et **SQLAgentOperatorRole** de **msdb** et les membres du rôle serveur fixe **sysadmin** ont accès à SQL Server Agent. Un utilisateur qui n’appartient à aucun de ces rôles ne peut pas utiliser SQL Server Agent. Pour plus d’informations sur les rôles utilisés par SQL Server Agent, consultez [Implémenter la sécurité de SQL Server Agent](../../ssms/agent/implement-sql-server-agent-security.md).

### <a name="subsystems"></a>Sous-systèmes

Un sous-système est un objet prédéfini qui représente la fonctionnalité disponible pour une étape de travail. Chaque proxy a accès à un ou plusieurs sous-systèmes. Les sous-systèmes assurent la sécurité en délimitant l'accès aux fonctionnalités mises à la disposition d'un proxy. Chaque étape de travail s’exécute dans le contexte d’un proxy, à l’exception des étapes de travail Transact\-SQL. Les étapes de travail Transact\-SQL utilisent la commande EXECUTE AS pour définir le contexte de sécurité sur le propriétaire du travail.  

SQL Server définit les sous-systèmes listés dans le tableau suivant :  

|Nom du sous-système|Description|
|--------------|-----------|
|Script ActiveX Microsoft|Permet d'exécuter une étape de travail de script ActiveX<br /><br />**Avertissement** Le sous-système de script ActiveX sera supprimé de SQL Server Agent dans une version future de Microsoft SQL Server. Évitez d'utiliser cette fonctionnalité dans de nouveaux travaux de développement, et prévoyez de modifier les applications qui utilisent actuellement cette fonctionnalité.|  
|Système d’exploitation (**CmdExec**)|Permet de lancer un programme exécutable.|
|PowerShell|Exécutez une étape de travail de scripts PowerShell.|
|Serveur de distribution de réplication|Permet d'exécuter une étape de travail qui active l'Agent de distribution.|
|Fusion de réplication|Permet d'exécuter une étape de travail qui active l'Agent de fusion.|
|Agent de lecture de file d'attente de réplication|Permet d'exécuter une étape de travail qui active l'Agent de lecture de la file d'attente de réplication.|
|Instantané de réplication|Permet d'exécuter une étape de travail qui active l'Agent d'instantané des réplications.|
|Agent de lecture du journal des transactions de réplication|Permet d'exécuter une étape de travail qui active l'Agent de lecture du journal des réplications.|
|Commandes Analysis Services|Exécuter une commande Analysis Services.|
|Requête Analysis Services|Exécuter une requête Analysis Services.|
|Exécution de package SSIS|Exécuter un package SSIS.|

> [!NOTE]
> Comme les étapes de travail Transact\-SQL n’utilisent pas de proxys, il n’existe pas de sous-système SQL Server Agent pour les étapes de travail Transact\-SQL.  

SQL Server Agent applique les restrictions des sous-systèmes même si le principal de sécurité du proxy a généralement l’autorisation d’exécuter cette tâche dans l’étape de travail. Par exemple, un proxy pour un utilisateur qui est membre du rôle serveur fixe sysadmin ne peut pas exécuter une étape de travail SSIS, à moins que le proxy ait accès au sous-système SSIS, même si l’utilisateur peut exécuter des packages SSIS.  

### <a name="proxies"></a>Proxies

SQL Server Agent utilise des proxys pour gérer les contextes de sécurité. Un proxy peut être utilisé dans plusieurs étapes de travail. Les membres du rôle de serveur fixe **sysadmin** peuvent créer des proxys.  

Chaque proxy correspond à des informations d'identification de sécurité. Chaque proxy peut être associé à un ensemble de sous-systèmes et de connexions. Le proxy peut être utilisé uniquement pour les étapes de travail qui utilisent un sous-système associé au proxy. Pour créer une étape de travail qui utilise un proxy spécifique, le propriétaire du travail doit utiliser une connexion associée à ce proxy ou être membre d’un rôle sans restriction d’accès aux proxys. Les membres du rôle de serveur fixe **sysadmin** disposent d'un accès sans restriction aux proxys. Les membres des rôles **SQLAgentUserRole**, **SQLAgentReaderRole** ou **SQLAgentOperatorRole** peuvent utiliser uniquement les proxys pour lesquels un accès spécifique leur a été accordé. Chaque utilisateur membre de l’un des rôles de base de données fixes de SQL Server Agent doit disposer d’un accès à des proxys spécifiques de façon à pouvoir créer les étapes de travail qui les utilisent.  

## <a name="related-tasks"></a>Tâches associées

Utilisez les étapes suivantes pour configurer SQL Server Agent de façon à automatiser l’administration de SQL Server :

1. Identifiez les tâches administratives et les événements de serveur se produisant régulièrement et déterminez si ces tâches ou ces événements peuvent être administrés par programme. Une tâche convient à l'automatisation si elle implique une séquence d'étapes prévisibles et se produit à un moment spécifique ou en réponse à un événement particulier.

2. Définissez un ensemble de travaux, de planifications, d’alertes et d’opérateurs en utilisant SQL Server Management Studio, des scripts Transact\-SQL ou SQL Server Management Objects (SMO). Pour plus d’informations, consultez [Créer des travaux](../../ssms/agent/create-jobs.md).  

3. Exécutez les travaux SQL Server Agent que vous avez définis.

> [!NOTE]
> Pour l’instance par défaut de SQL Server, le service SQL Server est nommé SQLSERVERAGENT. Pour les instances nommées, le service SQL Server Agent est nommé *SQLAgent$nom_instance*.

Si vous exécutez plusieurs instances de SQL Server, vous pouvez utiliser l’administration multiserveur pour automatiser des tâches courantes dans toutes les instances. Pour plus d’informations, consultez [Administration automatisée à l’échelle d’une entreprise](../../ssms/agent/automated-administration-across-an-enterprise.md).

Utilisez les tâches suivantes pour démarrer avec SQL Server Agent :

|Description|Rubrique|
|-----------|-----|
|Explique comment configurer SQL Server Agent.|[Configurer l'Agent SQL Server](../../ssms/agent/configure-sql-server-agent.md)|  
|Explique comment démarrer, arrêter et interrompre le service SQL Server Agent.|[Démarrer, arrêter ou suspendre le service SQL Server Agent](../../ssms/agent/start-stop-or-pause-the-sql-server-agent-service.md)|
|Décrit les considérations à prendre en compte lors de la spécification d'un compte pour le service SQL Server Agent.|[Sélectionner un compte pour le service SQL Server Agent](../../ssms/agent/select-an-account-for-the-sql-server-agent-service.md)|
|Explique comment utiliser le journal des erreurs de SQL Server Agent.|[Journal des erreurs de l'Agent SQL Server](../../ssms/agent/sql-server-agent-error-log.md)|  
|Explique comment utiliser des objets de performances.|[Utiliser des objets de performance](../../ssms/agent/use-performance-objects.md)|
|Décrit l’Assistant Plan de maintenance, un utilitaire que vous utilisez pour créer des travaux, des alertes et des opérateurs afin d’automatiser l’administration d’une instance de SQL Server.|[Utiliser l'Assistant Plan de maintenance](../../relational-databases/maintenance-plans/use-the-maintenance-plan-wizard.md)|
|Explique comment automatiser des tâches administratives à l'aide de l'Agent SQL Server.|[Tâches d’administration automatisée &#40;SQL Server Agent&#41;](../../ssms/agent/automated-administration-tasks-sql-server-agent.md)|

## <a name="nosqlps"></a>NOSQLPS

[!INCLUDE [sql-server-powershell-no-sqlps](../../includes/sql-server-powershell-no-sqlps.md)]

## <a name="see-also"></a>Voir aussi

- [Configuration de la surface d'exposition](../../relational-databases/security/surface-area-configuration.md)
- [SQL Server Agent avec PowerShell](../../powershell/sql-server-powershell.md#sql-server-agent)
