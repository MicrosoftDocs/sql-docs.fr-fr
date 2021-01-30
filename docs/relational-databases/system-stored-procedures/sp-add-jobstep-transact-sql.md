---
description: sp_add_jobstep (Transact-SQL)
title: sp_add_jobstep (Transact-SQL)
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_add_jobstep_TSQL
- sp_add_jobstep
helpviewer_keywords:
- sp_add_jobstep
ms.assetid: 97900032-523d-49d6-9865-2734fba1c755
author: markingmyname
ms.author: maghan
ms.custom: ''
ms.date: 03/15/2017
ms.openlocfilehash: 93d4818a1cffc14452bdf25bab8be6e1fc9a1a50
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99206753"
---
# <a name="sp_add_jobstep-transact-sql"></a>sp_add_jobstep (Transact-SQL)

[!INCLUDE [SQL Server - ASDBMI](../../includes/applies-to-version/sql-asdbmi.md)]

Ajoute une étape (opération) à un travail de l’agent SQL.

![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)

> [!IMPORTANT]
> Dans [Azure SQL Managed instance](/azure/sql-database/sql-database-managed-instance), la plupart des types de travaux SQL Server agent ne sont pas pris en charge. Pour plus d’informations, consultez [Différences T-SQL entre Azure SQL Managed Instance et SQL Server](/azure/sql-database/sql-database-managed-instance-transact-sql-information#sql-server-agent).

## <a name="syntax"></a>Syntaxe

```sql
sp_add_jobstep [ @job_id = ] job_id | [ @job_name = ] 'job_name'
     [ , [ @step_id = ] step_id ]
     { , [ @step_name = ] 'step_name' }
     [ , [ @subsystem = ] 'subsystem' ]
     [ , [ @command = ] 'command' ]
     [ , [ @additional_parameters = ] 'parameters' ]
          [ , [ @cmdexec_success_code = ] code ]
     [ , [ @on_success_action = ] success_action ]
          [ , [ @on_success_step_id = ] success_step_id ]
          [ , [ @on_fail_action = ] fail_action ]
          [ , [ @on_fail_step_id = ] fail_step_id ]
     [ , [ @server = ] 'server' ]
     [ , [ @database_name = ] 'database' ]
     [ , [ @database_user_name = ] 'user' ]
     [ , [ @retry_attempts = ] retry_attempts ]
     [ , [ @retry_interval = ] retry_interval ]
     [ , [ @os_run_priority = ] run_priority ]
     [ , [ @output_file_name = ] 'file_name' ]
     [ , [ @flags = ] flags ]
     [ , { [ @proxy_id = ] proxy_id
         | [ @proxy_name = ] 'proxy_name' } ]
```  
  
## <a name="arguments"></a>Arguments

`[ @job_id = ] job_id` Numéro d’identification du travail auquel ajouter l’étape. *job_id* est de type **uniqueidentifier**, avec NULL comme valeur par défaut.

`[ @job_name = ] 'job_name'` Nom du travail auquel ajouter l’étape. *job_name* est de **type sysname**, avec NULL comme valeur par défaut.

> [!NOTE]
> *Job_id* ou *job_name* doivent être spécifiés, mais ne peuvent pas être spécifiés.

`[ @step_id = ] step_id` Numéro d’identification de séquence de l’étape de travail. Les numéros d’identification de l’étape commencent à **1** et s’incrémentent sans intervalles. Si une étape est insérée dans une séquence existante, les numéros de cette séquence sont ajustés automatiquement. Une valeur est fournie si *step_id* n’est pas spécifié. *step_id* est de **type int**, avec NULL comme valeur par défaut.

`[ @step_name = ] 'step_name'` Nom de l’étape. *step_name* est de **type sysname**, sans valeur par défaut.

`[ @subsystem = ] 'subsystem'` Sous-système utilisé par le [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] service agent pour exécuter la *commande*. *Subsystem* est de type **nvarchar (40)** et peut prendre l’une des valeurs suivantes.

|Valeur|Description|
|-----------|-----------------|
|«**ActiveScripting**»|Script actif<br /><br /> **\*\* Important \*\*** [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)]|
|'**CmdExec**'|Commande du système d'exécution ou programme exécutable|
|«**Distribution**»|Travail de l'Agent de distribution de réplication|
|«**Instantané**»|Travail de l'Agent d'instantané de réplication|
|'**Logreader**'|Travail de l'Agent de lecture du journal de réplications|
|«**Fusionner**»|Travail de l'Agent de fusion de réplication|
|«**QueueReader**»|Travail de l'Agent de lecture de la file d'attente de réplication|
|«**ANALYSISQUERY**»|Requête Analysis Services (MDX, DMX).|
|«**ANALYSISCOMMAND**»|Commande Analysis Services (XMLA).|
|«**SSIS**»|[!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] Exécution du package|  
|«**PowerShell**»|script PowerShell|  
|'**Tsql**' (valeur par défaut)|Instruction [!INCLUDE[tsql](../../includes/tsql-md.md)]|

`[ @command = ] 'command'` Commandes qui doivent être exécutées par le service **SQLServerAgent** par le biais du *sous-système*. la *commande* est de type **nvarchar (max)**, avec NULL comme valeur par défaut. L'Agent SQL Server effectue une substitution de jetons qui offre la même souplesse que les variables lorsque vous écrivez des logiciels.

> [!IMPORTANT]
> Une macro d’échappement doit désormais accompagner tous les jetons utilisés dans les étapes de travail, sinon ces étapes de travail échouent. De plus, vous devez désormais mettre les noms de jeton entre parenthèses et placer un signe dollar (`$`) au début de la syntaxe du jeton. Par exemple :
>
> `$(ESCAPE_` *macro name* `(DATE))`  

Pour plus d’informations sur ces jetons et la mise à jour de vos étapes de travail afin d’utiliser la nouvelle syntaxe de jeton, consultez [utiliser des jetons dans les étapes de travail](../../ssms/agent/use-tokens-in-job-steps.md).

> [!IMPORTANT]
> Tout utilisateur Windows qui dispose des autorisations d'écriture dans le journal des événements Windows peut accéder aux étapes de travail activées par les alertes WMI ou par les alertes de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent. Pour éviter ce risque de sécurité, les jetons de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent qui peuvent être utilisés dans des travaux activés par des alertes sont désactivés par défaut. Ces jetons sont : **A-DBN**, **A-SVR**, **A-ERR**, **A-SEV**, **A-MSG** et **WMI(** _propriété_ **)** . Notez que dans cette version, l'utilisation des jetons est étendue toutes les alertes.
>
> Si vous devez utiliser ces jetons, assurez-vous d'abord que seuls les membres des groupes de sécurité Windows approuvés, comme le groupe Administrateurs, disposent des autorisations d'écriture pour le journal d'événements de l'ordinateur sur lequel [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] réside. Ensuite, pour activer ces jetons, cliquez avec le bouton droit sur **SQL Server Agent** dans l’Explorateur d’objets, sélectionnez **Propriétés**, puis dans la page **Système d’alerte** qui s’affiche, sélectionnez l’option **Remplacer les jetons pour toutes les réponses de travaux aux alertes** .

`[ @additional_parameters = ] 'parameters'`[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)] *Parameters* est de type **ntext**, avec NULL comme valeur par défaut.

`[ @cmdexec_success_code = ] code` La valeur retournée par une commande de sous-système **CmdExec** pour indiquer que la *commande* a été exécutée avec succès. *code* est de **type int**, avec **0** comme valeur par défaut.

`[ @on_success_action = ] success_action` Action à exécuter si l’étape se déroule correctement. *success_action* est de **type tinyint** et peut prendre l’une des valeurs suivantes.
  
|Valeur|Description (action)|  
|-----------|----------------------------|  
|**1** (par défaut)|Quitter avec succès|  
|**2**|Quitter avec échec|  
|**3**|Passez à l'étape suivante|  
|**4**|Aller à l’étape *on_success_step_id*|  

`[ @on_success_step_id = ] success_step_id` ID de l’étape du travail à exécuter si l’étape se déroule correctement et que *success_action* a la valeur **4**. *success_step_id* est de **type int**, avec **0** comme valeur par défaut.

`[ @on_fail_action = ] fail_action` Action à exécuter si l’étape échoue. *fail_action* est de **type tinyint** et peut prendre l’une des valeurs suivantes.

|Valeur|Description (action)|  
|-----------|----------------------------|  
|**1**|Quitter avec succès|  
|**2** (par défaut)|Quitter avec échec|  
|**3**|Passez à l'étape suivante|  
|**4**|Aller à l’étape *on_fail_step_id*|  

`[ @on_fail_step_id = ] fail_step_id` ID de l’étape de ce travail à exécuter si l’étape échoue et *fail_action* a la valeur **4**. *fail_step_id* est de **type int**, avec **0** comme valeur par défaut.  

`[ @server = ] 'server'`[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)] *Server* est de type **nvarchar (30)**, avec NULL comme valeur par défaut.  

`[ @database_name = ] 'database'` Nom de la base de données dans laquelle exécuter une [!INCLUDE[tsql](../../includes/tsql-md.md)] étape. *Database est de* **type sysname**, avec NULL comme valeur par défaut. dans ce cas, la base de données **Master** est utilisée. Les noms placés entre crochets ([ ]) ne sont pas autorisés. Pour une étape de travail ActiveX, la *base de données* est le nom du langage de script que l’étape utilise.  

`[ @database_user_name = ] 'user'` Nom du compte d’utilisateur à utiliser lors de l’exécution d’une [!INCLUDE[tsql](../../includes/tsql-md.md)] étape. *User* est de **type sysname**, avec NULL comme valeur par défaut. Lorsque *User* a la valeur null, l’étape s’exécute dans le contexte utilisateur du propriétaire de la tâche sur la *base de données*.  L'Agent SQL Server inclut ce paramètre uniquement si le propriétaire du travail est un sysadmin SQL Server. Dans ce cas, l'étape Transact-SQL donnée est exécutée dans le contexte du nom d'utilisateur SQL Server spécifié. Si le propriétaire du travail n’est pas un SQL Server sysadmin, l’étape Transact-SQL est toujours exécutée dans le contexte de la connexion qui détient ce travail, et le @database_user_name paramètre est ignoré.  

`[ @retry_attempts = ] retry_attempts` Nombre de nouvelles tentatives à utiliser en cas d’échec de cette étape. *retry_attempts* est de **type int**, avec **0** comme valeur par défaut, qui n’indique aucune nouvelle tentative.  

`[ @retry_interval = ] retry_interval` Durée, en minutes, entre chaque tentative. *retry_interval* est de **type int**, avec **0** comme valeur par défaut, qui indique un intervalle de **0** minute.  

`[ @os_run_priority = ] run_priority` Réservé.

`[ @output_file_name = ] 'file_name'` Nom du fichier dans lequel la sortie de cette étape est enregistrée. *file_name* est de type **nvarchar (200)**, avec NULL comme valeur par défaut. *file_name* pouvez inclure un ou plusieurs des jetons listés sous *Command*. Ce paramètre est valide uniquement avec les commandes qui s’exécutent sur les sous [!INCLUDE[tsql](../../includes/tsql-md.md)] -systèmes, **CmdExec**, **PowerShell**, [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] ou [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] .  

`[ @flags = ] flags` Est une option qui contrôle le comportement. *Flags* est de **type int** et peut prendre l’une des valeurs suivantes.  

|Valeur|Description|  
|-----------|-----------------|  
|**0** (valeur par défaut)|Écrasement du fichier de sortie|  
|**2**|Ajout au fichier de sortie|  
|**4**|Écriture de la sortie de l'étape d'un travail [!INCLUDE[tsql](../../includes/tsql-md.md)] dans l'historique des étapes.|  
|**8**|Écriture du journal dans la table (remplace l'historique existant)|  
|**16**|Écriture du journal dans la table (s'ajoute à l'historique existant)|  
|**32**|Écriture de toute la sortie dans l'historique des travaux|  
|**64**|Création d'un événement Windows à utiliser comme signal pour l'étape de travail Cmd à abandonner|  

`[ @proxy_id = ] proxy_id` Numéro d’identification du proxy sous lequel l’étape de travail s’exécute. *proxy_id* est de type **int**, avec NULL comme valeur par défaut. Si aucun *proxy_id* n’est spécifié, aucun *proxy_name* n’est spécifié et aucun *user_name* n’est spécifié, l’étape de travail s’exécute en tant que compte de service pour [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] l’agent.  

`[ @proxy_name = ] 'proxy_name'` Nom du proxy sous lequel l’étape de travail s’exécute. *proxy_name* est de type **sysname**, avec NULL comme valeur par défaut. Si aucun *proxy_id* n’est spécifié, aucun *proxy_name* n’est spécifié et aucun *user_name* n’est spécifié, l’étape de travail s’exécute en tant que compte de service pour [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] l’agent.  

## <a name="return-code-values"></a>Codet de retour

**0** (succès) ou **1** (échec)

## <a name="result-sets"></a>Jeux de résultats

None

## <a name="remarks"></a>Notes

**sp_add_jobstep** doit être exécuté à partir de la base de données **msdb** .  

SQL Server Management Studio offre un moyen simple et graphique de gérer les tâches, et est recommandé pour la création et la gestion de l'infrastructure de travail.  

Par défaut, une étape de travail s’exécute en tant que compte de service pour [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] l’agent, sauf si un autre proxy est spécifié. Une exigence de ce compte doit être un membre du rôle de sécurité fixe **sysadmin** .

Un proxy peut être identifié par *proxy_name* ou *proxy_id*.

## <a name="permissions"></a>Autorisations

 Par défaut, les membres du rôle serveur fixe **sysadmin** peuvent exécuter cette procédure stockée. Les autres utilisateurs doivent disposer de l'un des rôles de base de données fixes suivants de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent dans la base de données **msdb** :

- **SQLAgentUserRole**

- **SQLAgentReaderRole**

- **SQLAgentOperatorRole**

Pour en savoir plus sur les autorisations de ces rôles, consultez [Rôles de base de données fixes de l'Agent SQL Server](../../ssms/agent/sql-server-agent-fixed-database-roles.md).

Le créateur de l'étape de travail doit disposer des droits d'accès au proxy pour cette étape. Les membres du rôle serveur fixe **sysadmin** ont accès à tous les proxys. Pour les autres utilisateurs, les droits d'accès à un proxy doivent être octroyés explicitement.

## <a name="examples"></a>Exemples

L'exemple suivant montre la création de l'étape d'un travail permettant de modifier l'accès à une base de données appelée Sales et de lui affecter le mode d'accès en lecture seule. En outre, cet exemple indique 5 tentatives de reprises, chacune d'elles étant exécutée toutes les 5 minutes.

> [!NOTE]
> Cet exemple suppose que le `Weekly Sales Data Backup` travail existe déjà.  

```sql
USE msdb;
GO
EXEC sp_add_jobstep
    @job_name = N'Weekly Sales Data Backup',
    @step_name = N'Set database to read only',
    @subsystem = N'TSQL',
    @command = N'ALTER DATABASE SALES SET READ_ONLY',
    @retry_attempts = 5,
    @retry_interval = 5 ;
GO
```

## <a name="next-steps"></a>Étapes suivantes

- [Afficher ou modifier les travaux](../../ssms/agent/view-or-modify-jobs.md)
- [sp_add_job &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-add-job-transact-sql.md)
- [sp_add_schedule &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-add-schedule-transact-sql.md)
- [sp_delete_jobstep &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-delete-jobstep-transact-sql.md)
- [sp_help_job &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-help-job-transact-sql.md)
- [sp_help_jobstep &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-help-jobstep-transact-sql.md)
- [sp_update_jobstep &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-update-jobstep-transact-sql.md)
- [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)