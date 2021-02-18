---
description: Gérer les étapes de travail
title: Gérer les étapes de travail
ms.custom: seo-lt-2019
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- job steps [SQL Server replication]
- job steps [SQL Server Agent]
- jobs [SQL Server Agent], Integration Services package step
- executable programs as job steps
- operating systems [SQL Server], job steps
- Transact-SQL job step
- job steps [Transact-SQL]
- Integration Services packages, job steps
- replication job steps [SQL Server]
- logs [SQL Server], jobs
- SQL Server Agent jobs, job steps
- ActiveX scripting jobs [SQL Server]
- job steps [Analysis Services]
ms.assetid: 51352afc-a0a4-428b-8985-f9e58bb57c31
author: markingmyname
ms.author: maghan
ms.reviewer: ''
monikerRange: = azuresqldb-mi-current || >= sql-server-2016
ms.openlocfilehash: 8892ff46f9c6c9fd382b39284cbc4bc81449a5d9
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100355183"
---
# <a name="manage-job-steps"></a>Gérer les étapes de travail
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

> [!IMPORTANT]  
> Dans [Azure SQL Managed Instance](/azure/sql-database/sql-database-managed-instance), la plupart, mais pas toutes les fonctionnalités SQL Server Agent sont actuellement prises en charge. Pour plus d’informations, consultez [Différences T-SQL entre Azure SQL Managed Instance et SQL Server](/azure/sql-database/sql-database-managed-instance-transact-sql-information#sql-server-agent).

Une étape du travail est une action exécutée par le travail sur une base de données ou un serveur. Chaque travail doit posséder au moins une étape de travail. Les étapes de travail peuvent être :  
  
-   Des programmes exécutables et des commandes du système d'exploitation  
  
-   [!INCLUDE[tsql](../../includes/tsql-md.md)] Des instructions, notamment des procédures stockées ou des procédures stockées étendues  
  
-   Des scripts PowerShell  
  
-   [!INCLUDE[msCoName](../../includes/msconame_md.md)] Des scripts ActiveX  
  
-   Des tâches de réplication  
  
-   [!INCLUDE[ssASnoversion](../../includes/ssasnoversion_md.md)] Des tâches  
  
-   [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] Des packages  
  
Chaque étape de travail s'exécute dans un contexte de sécurité spécifique. Si l'étape de travail spécifie un proxy, elle s'exécute dans le contexte des informations d'identification du proxy. Dans le cas inverse, l'étape de travail s'exécute dans le contexte du compte du service [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent. Seuls les membres du rôle de serveur fixe sysadmin sont autorisés à créer des travaux qui ne spécifient pas explicitement de proxy.  
  
Les étapes de travail étant exécutées dans le contexte d’un utilisateur [!INCLUDE[msCoName](../../includes/msconame_md.md)] Windows spécifique, cet utilisateur doit disposer des autorisations et de la configuration nécessaires à l’exécution de l’étape de travail. Par exemple, si vous créez un travail qui requiert une lettre de lecteur ou un chemin d'accès UNC (Universal Naming Convention), les étapes du travail peuvent être exécutées sous votre compte d'utilisateur Windows pendant que les tâches sont testées. Toutefois, l'utilisateur Windows associé à l'étape de travail doit par ailleurs disposer des autorisations nécessaires, des configurations de lettres de lecteurs appropriées ou des droits d'accès au lecteur requis. Sinon, l'étape de travail échoue. Pour éviter ce problème, vérifiez que le proxy associé à chaque étape de travail dispose des autorisations nécessaires pour la tâche correspondant à l'étape de travail. Pour plus d’informations, consultez [Sécurité et protection (moteur de base de données)](../../relational-databases/security/security-center-for-sql-server-database-engine-and-azure-sql-database.md).  
  
## <a name="job-step-logs"></a>Journaux d'étapes de travail  
[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent peut écrire le résultat de certaines étapes de travail dans un fichier du système d’exploitation ou dans la table sysjobstepslogs de la base de données msdb. Les types d'étapes de travail suivants peuvent écrire les résultats sur les deux destinations :  
  
-   Des programmes exécutables et des commandes du système d'exploitation  
  
-   Instructions[!INCLUDE[tsql](../../includes/tsql-md.md)] .  
  
-   [!INCLUDE[ssASnoversion](../../includes/ssasnoversion_md.md)] Des tâches  
  
Seules les étapes de travail exécutées par des utilisateurs membres du rôle de serveur fixe sysadmin peuvent écrire le résultat des étapes de travail dans des fichiers du système d'exploitation. Si les étapes de travail sont exécutées par des utilisateurs membres du rôle de base de données fixe SQLAgentUserRole, SQLAgentReaderRole ou SQLAgentOperatorRole dans la base de données msdb, le résultat de ces étapes de travail peut être écrit uniquement dans la table sysjobstepslogs.  
  
Les journaux d'étapes de travail sont automatiquement supprimés dès lors que les travaux ou les étapes de travail sont supprimés.  
  
> [!NOTE]  
> La journalisation des tâches de réplication et des étapes de travail des packages [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] est assurée par leur sous-système respectif. Vous ne pouvez pas utiliser l'Agent [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pour configurer la journalisation des étapes de travail pour ces types d'étapes de travail.  
  
## <a name="executable-programs-and-operating-system-commands-as-job-steps"></a>Programmes exécutables et commandes du système d'exploitation en tant qu'étapes de travail  
Les programmes exécutables et les commandes du système d'exploitation peuvent être utilisés en tant qu'étapes de travail. Ces fichiers peuvent posséder les extension bat, .cmd, .com ou .exe.  
  
Lorsque vous utilisez un programme exécutable ou une commande du système d'exploitation en tant qu'étape de travail, vous devez spécifier :  
  
-   le code de sortie du processus renvoyé en cas de succès de la commande ;  
  
-   Commande à exécuter. S'il s'agit d'exécuter une commande du système d'exploitation, vous spécifiez simplement la commande elle-même. Dans le cas d’un programme externe, il s’agit du nom du programme et des arguments à transmettre au programme, par exemple : **C:\Program Files\Microsoft SQL Server\100\Tools\Binn\sqlcmd.exe -e -q "sp_who"**  
  
    > [!NOTE]  
    > Vous devez indiquer le chemin d'accès complet au fichier exécutable si ce dernier ne réside pas dans un répertoire spécifié dans le chemin système ou le chemin d'accès de l'utilisateur sous le nom duquel l'étape de travail est exécutée.  
  
## <a name="transact-sql-job-steps"></a>Étapes de travail Transact-SQL  
Lorsque vous créez une étape de travail [!INCLUDE[tsql](../../includes/tsql-md.md)] , vous devez :  
  
-   identifier la base de données dans laquelle le travail va être exécuté ;  
  
-   taper l'instruction [!INCLUDE[tsql](../../includes/tsql-md.md)] à exécuter. L'instruction peut faire appel à une procédure stockée ou à une procédure stockée étendue.  
  
Vous pouvez éventuellement ouvrir un fichier [!INCLUDE[tsql](../../includes/tsql-md.md)] existant en tant que commande pour l'étape de travail.  
  
[!INCLUDE[tsql](../../includes/tsql-md.md)] Les étapes de travail n’utilisent pas les proxies de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent. Au lieu de cela, l'étape de travail s'exécute au nom de son propriétaire ou sous le compte du service Agent [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , si le propriétaire de l'étape de travail est membre du rôle de serveur sysadmin. Les membres du rôle serveur fixe sysadmin peuvent également préciser que les étapes de travail [!INCLUDE[tsql](../../includes/tsql-md.md)] s’exécutent sous le contexte d’un autre utilisateur par le biais du paramètre *database_user_name* de la procédure stockée sp_add_jobstep. Pour plus d’informations, consultez [sp_add_jobstep (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-add-jobstep-transact-sql.md).  
  
> [!NOTE]  
> Une même étape de travail [!INCLUDE[tsql](../../includes/tsql-md.md)] peut contenir plusieurs lots. [!INCLUDE[tsql](../../includes/tsql-md.md)] Les étapes de travail peuvent intégrer des commandes GO incorporées.  
  
## <a name="powershell-scripting-job-steps"></a>Étapes de travail de scripts PowerShell  
Lorsque vous créez une étape de travail de script PowerShell, vous devez spécifier l'un des deux éléments suivants comme commande pour l'étape :  
  
-   Le texte d'un script PowerShell.  
  
-   Un fichier de script PowerShell existant à ouvrir.  
  
Le sous-système [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent PowerShell ouvre une session PowerShell et charge les composants logiciels enfichables [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] PowerShell. Le script PowerShell utilisé comme commande d’étape de travail peut faire référence aux applets de commande et au fournisseur [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] PowerShell. Pour plus d’informations sur l’écriture de scripts PowerShell à l’aide des composants logiciels enfichables [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] PowerShell, consultez [SQL Server PowerShell](../../powershell/sql-server-powershell.md).  
  
## <a name="activex-scripting-job-steps"></a>Étapes de travail ActiveX Scripting  
  
> [!IMPORTANT]
> L’étape de travail de scripts ActiveX sera supprimé de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent dans une version ultérieure de [!INCLUDE[msCoName](../../includes/msconame_md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Évitez d'utiliser cette fonctionnalité dans de nouveaux travaux de développement, et prévoyez de modifier les applications qui utilisent actuellement cette fonctionnalité.  
  
Lorsque vous créez une étape de travail ActiveX Scripting, vous devez :  
  
-   identifier le langage de script dans lequel l'étape de travail est écrite ;  
  
-   écrire le script ActiveX.  
  
Vous pouvez également ouvrir un fichier de script ActiveX existant en tant que commande pour l'étape du travail. Les commandes de script ActiveX peuvent aussi être compilées en externe (par exemple, à l'aide de Microsoft Visual Basic), puis exécutées en tant que programmes exécutables.  
  
Lorsqu'une commande d'étape de travail est un script ActiveX, vous pouvez utiliser l'objet SQLActiveScriptHost pour imprimer le résultat dans le journal d'historique des étapes de travail ou créer des objets COM. SQLActiveScriptHost est un objet global introduit par le système hôte d'Agent [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] dans l'espace de noms de script. L’objet a deux méthodes (Print et CreateObject). L'exemple suivant illustre le fonctionnement du script ActiveX dans Visual Basic Scripting Edition (VBScript).  
  
```  
' VBScript example for ActiveX Scripting job step  
' Create a Dmo.Server object. The object connects to the  
' server on which the script is running.  
  
Set oServer = CreateObject("SQLDmo.SqlServer")  
oServer.LoginSecure = True  
oServer.Connect "(local)"  
'Disconnect and destroy the server object  
oServer.DisConnect  
Set oServer = nothing  
```  
  
## <a name="replication-job-steps"></a>Étapes de travail de réplication  
Lorsque vous créez des publications et des abonnements par le biais de la réplication, des travaux de réplication sont créés par défaut. Le type de travail créé dépend du type de réplication (instantané, transactionnelle ou fusion) et des options utilisées.  
  
Les étapes de travail de réplication activent l'un des agents de réplication suivants :  
  
-   Agent d'instantané (travail Snapshot)  
  
-   Agent de lecture du journal (travail LogReader)  
  
-   Agent de distribution (travail de distribution)  
  
-   Agent de fusion (travail Merge)  
  
-   Agent de lecture de file d'attente (travail QueueReader)  
  
Lorsque la réplication est configurée, vous pouvez spécifier l'un des trois modes d'exécution suivants pour les agents de réplication : en continu après le démarrage de l'Agent [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , à la demande, ou selon un programme. Pour plus d’informations sur les agents de réplication, consultez [Présentation des Agents de réplication](../../relational-databases/replication/agents/replication-agents-overview.md).  
  
## <a name="analysis-services-job-steps"></a>Étapes de travail Analysis Services  
[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent prend en charge deux types distincts d’étapes de travail Analysis Services : les étapes de travail de commande et les étapes de travail de requête.  
  
### <a name="analysis-services-command-job-steps"></a>Étapes de travail de commande Analysis Services  
Lorsque vous créez une étape de travail de commande [!INCLUDE[ssASnoversion](../../includes/ssasnoversion_md.md)] , vous devez :  
  
-   identifier le serveur de bases de données OLAP sur lequel l'étape de travail doit s'exécuter ;  
  
-   taper l'instruction à exécuter. L’instruction doit être une méthode XML pour [!INCLUDE[ssASnoversion](../../includes/ssasnoversion_md.md)] **Execute**. L’instruction ne doit pas contenir d’enveloppe SOAP complète ou de méthode XML pour [!INCLUDE[ssASnoversion](../../includes/ssasnoversion_md.md)] **Discover**. Notez que, bien que [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] prenne en charge les enveloppes SOAP (Simple Object Access Protocol) complètes et la méthode **Discover** , ce n’est pas le cas pour les étapes de travail de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent.  
  
### <a name="analysis-services-query-job-steps"></a>Étapes de travail de requête Analysis Services  
Lorsque vous créez une étape de travail de requête [!INCLUDE[ssASnoversion](../../includes/ssasnoversion_md.md)] , vous devez :  
  
-   identifier le serveur de bases de données OLAP sur lequel l'étape de travail doit s'exécuter ;  
  
-   taper l'instruction à exécuter. L'instruction doit être une requête MDX (Multidimensional Expressions).  
  
Pour plus d’informations sur MDX, consultez [Principes de base des instructions MDX (MDX)](/analysis-services/multidimensional-models/mdx/mdx-query-fundamentals-analysis-services?viewFallbackFrom=sql-server-ver15).  
  
## <a name="integration-services-packages"></a>Packages Integration Services  
Lorsque vous créez une étape de travail de package [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] , vous devez effectuer les étapes suivantes :  
  
-   identifier la source du package ;  
  
-   identifier l'emplacement du package ;  
  
-   identifier les fichiers de configuration, dans la mesure où le package requiert des fichiers de configuration ;  
  
-   identifier les fichiers de commandes, dans la mesure où le package requiert des fichiers de commandes ;  
  
-   identifier la vérification à utiliser pour le package. Par exemple, vous pouvez préciser que le package doit être signé ou posséder un ID de package spécifique ;  
  
-   identifier les sources de données du package ;  
  
-   identifier les fournisseurs d'informations du package ;  
  
-   préciser les variables et les valeurs à définir préalablement à l'exécution du package ;  
  
-   identifier les options d'exécution ;  
  
-   ajouter ou modifier les options de ligne de commande.  
  
Notez que si vous avez déployé le package dans le catalogue SSIS et que vous spécifiez **Catalogue SSIS** comme source du package, la plupart de ces informations de configuration sont obtenues automatiquement à partir du package. Sous l’onglet **Configuration** , vous pouvez spécifier l’environnement, les valeurs de paramètres, les valeurs de gestionnaire de connexions, les substitutions de propriété, et si le package s’exécute dans un environnement 32 bits.  
  
Pour plus d’informations sur la création d’étapes de travail qui exécutent des packages [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] , consultez [Travaux de SQL Server Agent pour les packages](../../integration-services/packages/sql-server-agent-jobs-for-packages.md).  
  
## <a name="related-tasks"></a>Tâches associées  
  
|Description|Rubrique|  
|-|-|  
|Décrit comment créer une étape de travail avec un programme exécutable.|[Créer une étape de travail CmdExec](../../ssms/agent/create-a-cmdexec-job-step.md)|  
|Décrit comment réinitialiser les autorisations de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent.|[Configurer un utilisateur de manière à créer et à gérer des travaux de l'Agent SQL Server](../../ssms/agent/configure-a-user-to-create-and-manage-sql-server-agent-jobs.md)|  
|Décrit comment créer une étape de travail [!INCLUDE[tsql](../../includes/tsql-md.md)] .|[Créer une étape de travail Transact-SQL](../../ssms/agent/create-a-transact-sql-job-step.md)|  
|Décrit comment définir les options des étapes de travail Transact-SQL de l'Agent Microsoft [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .|[Définir les options d'une étape de travail Transact-SQL](../../ssms/agent/define-transact-sql-job-step-options.md)|  
|Décrit comment créer une étape de travail de script ActiveX.|[Créer une étape de travail de script ActiveX](../../ssms/agent/create-an-activex-script-job-step.md)|  
|Décrit comment créer et définir les étapes de travail de l'Agent [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] qui exécutent les commandes et requêtes Analysis Services de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .|[Créer une étape de travail Analysis Services](../../ssms/agent/create-an-analysis-services-job-step.md)|  
|Décrit quelle mesure [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] doit exécuter si une défaillance se produit pendant l'exécution d'une tâche.|[Définir un flux en cas de réussite ou d'échec de l'étape de travail](../../ssms/agent/set-job-step-success-or-failure-flow.md)|  
|Décrit comment afficher les détails d'une étape de travail dans la boîte de dialogue Propriétés de l'étape du travail.|[View Job Step Information](../../ssms/agent/view-job-step-information.md)|  
|Décrit comment supprimer un journal d'étapes de travail de l'Agent [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .|[Supprimer un journal d’étapes de travail](../../ssms/agent/delete-a-job-step-log.md)|  
  
## <a name="see-also"></a>Voir aussi  
[sysjobstepslogs (Transact-SQL)](../../relational-databases/system-tables/dbo-sysjobstepslogs-transact-sql.md)  
[Créer des travaux](../../ssms/agent/create-jobs.md)  
[sp_add_job (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-add-job-transact-sql.md)  
