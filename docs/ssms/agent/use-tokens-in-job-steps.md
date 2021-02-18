---
description: Utiliser des jetons dans les étapes d'un travail
title: Utiliser des jetons dans les étapes d'un travail
ms.custom: seo-lt-2019
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- job steps [SQL Server Agent]
- security [SQL Server Agent], enabling alert job step tokens
- SQL Server Agent jobs, job steps
- tokens [SQL Server]
- escape macros [SQL Server Agent]
ms.assetid: 105bbb66-0ade-4b46-b8e4-f849e5fc4d43
author: markingmyname
ms.author: maghan
ms.reviewer: ''
monikerRange: = azuresqldb-mi-current || >= sql-server-2016
ms.openlocfilehash: 7463f708819d772ef8d9e94a8f8b58ca4a7e4f7a
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100344686"
---
# <a name="use-tokens-in-job-steps"></a>Utiliser des jetons dans les étapes d'un travail
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

> [!IMPORTANT]  
> Dans [Azure SQL Managed Instance](/azure/sql-database/sql-database-managed-instance), la plupart, mais pas toutes les fonctionnalités SQL Server Agent sont actuellement prises en charge. Pour plus d’informations, consultez [Différences T-SQL entre Azure SQL Managed Instance et SQL Server](/azure/sql-database/sql-database-managed-instance-transact-sql-information#sql-server-agent).

[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent vous permet d’utiliser des jetons dans des scripts d’étape de travail [!INCLUDE[tsql](../../includes/tsql-md.md)] . Ces jetons avec lesquels vous rédigez des étapes de travail vous offrent la même flexibilité que les variables lors de l'écriture de programmes logiciels. Après que vous avez inséré un jeton dans un script d'étape de travail, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent le remplace au moment de l'exécution avant que le sous-système [!INCLUDE[tsql](../../includes/tsql-md.md)] n'exécute l'étape de travail.  
  
  
## <a name="understanding-using-tokens"></a>Utilisation de jetons  
  
> [!IMPORTANT]  
> Tout utilisateur Windows qui dispose des autorisations d'écriture dans le journal des événements Windows peut accéder aux étapes de travail activées par les alertes WMI ou par les alertes de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent. Pour éviter ce risque de sécurité, les jetons de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent qui peuvent être utilisés dans des travaux activés par des alertes sont désactivés par défaut. Ces jetons sont : **A-DBN**, **A-SVR**, **A-ERR**, **A-SEV**, **A-MSG** et **WMI(** _propriété_ **)** . Notez que dans cette version, l'utilisation des jetons est étendue toutes les alertes.  
>   
> Si vous devez utiliser ces jetons, assurez-vous d'abord que seuls les membres des groupes de sécurité Windows approuvés, comme le groupe Administrateurs, disposent des autorisations d'écriture pour le journal d'événements de l'ordinateur sur lequel [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] réside. Ensuite, pour activer ces jetons, cliquez avec le bouton droit sur **SQL Server Agent** dans l’Explorateur d’objets, sélectionnez **Propriétés**, puis dans la page **Système d’alerte** qui s’affiche, sélectionnez l’option **Remplacer les jetons pour toutes les réponses de travaux aux alertes** .  
  
[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent : Le remplacement du jeton est simple et efficace, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent remplace le jeton par une valeur de chaîne littérale exacte. Tous les jetons respectent la casse. Vous devez tenir compte de ce changement dans vos étapes de travail et nommer correctement les jetons utilisés, ou bien convertir la chaîne de remplacement en type de données correct.  
  
Par exemple, vous pouvez utiliser l'instruction suivante pour imprimer le nom de la base de données dans une étape de travail :  
  
`PRINT N'Current database name is $(ESCAPE_SQUOTE(A-DBN))' ;`  
  
Dans cet exemple, la macro **ESCAPE_SQUOTE** est insérée avec le jeton **A-DBN** . Au moment de l’exécution, le jeton **A-DBN** est remplacé par le nom de base de données approprié. La macro d'échappement interprète comme des caractères d'échappement tous les guillemets simples transmis par inadvertance à la chaîne de remplacement des jetons. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent remplace un guillemet simple par un guillemet double dans la chaîne finale.  
  
Par exemple, si la chaîne transmise en vue de remplacer le jeton est `AdventureWorks2012'SELECT @@VERSION --`, la commande exécutée par l'étape de travail [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent sera la suivante :  
  
`PRINT N'Current database name is AdventureWorks2012''SELECT @@VERSION --' ;`  
  
Dans ce cas, l'instruction insérée, `SELECT @@VERSION`, n'est pas exécutée. À la place, le guillemet simple supplémentaire incite le serveur à analyser l'instruction insérée en tant que chaîne. Si la chaîne de remplacement de jetons ne contient aucun guillemet simple, aucun caractère n'est interprété comme caractère d'échappement et l'étape de travail contenant le jeton s'exécute comme prévu.  
  
Pour déboguer l'usage de jetons dans vos étapes de travail, utilisez des instructions d'impression, telles que `PRINT N'$(ESCAPE_SQUOTE(SQLDIR))'` , et enregistrez la sortie des étapes de travail dans un fichier ou une table. Ouvrez la page **Avancé** de la boîte de dialogue **Propriétés de l’étape du travail** pour spécifier une table ou un fichier de sortie pour les étapes de travail.  
  
## <a name="sql-server-agent-tokens-and-macros"></a>Jetons et macros de SQL Server Agent  
Les tableaux suivants répertorient et décrivent les jetons et les macros pris en charge par [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent.  
  
### <a name="sql-server-agent-tokens"></a>Jetons de SQL Server Agent  
  
|par jeton|Description|  
|---------|---------------|  
|**(A-DBN)**|Nom de la base de données. Si le travail est exécuté par une alerte, la valeur du nom de la base de données remplace automatiquement ce jeton dans l'étape de travail.|  
|**(A-SVR)**|Nom du serveur. Si le travail est exécuté par une alerte, la valeur du nom du serveur remplace automatiquement ce jeton dans l'étape de travail.|  
|**(A-ERR)**|Numéro d’erreur. Si le travail est exécuté par une alerte, la valeur du numéro d'erreur remplace automatiquement ce jeton dans l'étape de travail.|  
|**(A-SEV)**|Gravité de l'erreur Si le travail est exécuté par une alerte, la valeur de la gravité de l'erreur remplace automatiquement ce jeton dans l'étape de travail.|  
|**(A-MSG)**|Texte du message. Si le travail est exécuté par une alerte, la valeur du texte du message remplace automatiquement ce jeton dans l'étape de travail.|  
|**(JOBNAME)**|Nom du travail. Ce jeton est disponible uniquement sur SQL Server 2016 et ultérieur.|  
|**(STEPNAME)**|Nom de l'étape. Ce jeton est disponible uniquement sur SQL Server 2016 et ultérieur.|  
|**(DATE)**|Date actuelle (au format AAAAMMJJ).|  
|**(INST)**|Nom de l’instance. Pour une instance par défaut, ce jeton aura le nom de l'instance par défaut : MSSQLSERVER.|  
|**(JOBID)**|ID de travail.|  
|**(MACH)**|Nom de l’ordinateur.|  
|**(MSSA)**|Nom du service SQLServerAgent principal.|  
|**(OSCMD)**|Préfixe du programme utilisé pour exécuter les étapes de travail **CmdExec** .|  
|**(SQLDIR)**|Répertoire d'installation de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . La valeur par défaut est : C:\Program Files\Microsoft SQL Server\MSSQL.|  
|**(SQLLOGDIR)**|Jeton de remplacement pour le chemin du dossier des fichiers journaux des erreurs SQL Server, par exemple, $(ESCAPE_SQUOTE(SQLLOGDIR)). Ce jeton est disponible uniquement sur SQL Server 2014 et ultérieur.|  
|**(STEPCT)**|Nombre de fois où cette étape a été exécutée (ne tient pas compte des nouvelles tentatives). Peut être utilisé par la commande d'étape pour forcer l'arrêt d'une boucle multi-étape.|  
|**(STEPID)**|Numéro d'identification de l'étape.|  
|**(SRVR)**|Nom de l'ordinateur sur lequel [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]est exécuté. Si l'instance [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] est une instance nommée, le nom de l'instance est compris.|  
|**(TIME)**|Heure actuelle (au format HHMMSS).|  
|**(STRTTM)**|Heure (au format HHMMSS) à laquelle l'exécution du travail a débuté.|  
|**(STRTDT)**|Date (au format AAAAMMJJ) à laquelle l'exécution du travail a débuté.|  
|**(WMI(** _propriété_ **))**|Pour les travaux qui s’exécutent en réponse à des alertes WMI, la valeur de la propriété est spécifiée par *propriété*. Par exemple, `$(WMI(DatabaseName))` fournit la valeur de la propriété **DatabaseName** pour l’événement WMI qui a entraîné l’exécution de l’alerte.|  
  
### <a name="sql-server-agent-escape-macros"></a>Macros d'échappement de SQL Server Agent  
  
|Macros d'échappement|Description|  
|-----------------|---------------|  
|**$(ESCAPE_SQUOTE(** _nom\_jeton_ **))**|Interprète les guillemets simples (') comme caractères d'échappement dans la chaîne de remplacement des jetons. Remplace un guillemet simple par un guillemet double.|  
|**$(ESCAPE_DQUOTE(** _nom\_jeton_ **))**|Interprète les guillemets doubles (") comme caractères d'échappement dans la chaîne de remplacement des jetons. Remplace un guillemet double par deux guillemets doubles.|  
|**$(ESCAPE_RBRACKET(** _nom\_jeton_ **))**|Interprète les crochets droits (]) comme caractères d'échappement dans la chaîne de remplacement des jetons. Remplace un crochet droit par deux crochets droits.|  
|**$(ESCAPE_NONE(** _nom\_jeton_ **))**|Remplace le jeton sans interpréter de caractères comme caractères d'échappement dans la chaîne. La macro est fournie pour des raisons de prise en charge de la compatibilité descendante dans des environnements où l'usage de chaînes de remplacement de jetons est généralement réservé à des utilisateurs approuvés. Pour plus d'informations, consultez la section « Mise à jour des étapes de travail pour l'utilisation de macros » plus loin dans cette rubrique.|  
  
## <a name="updating-job-steps-to-use-macros"></a>Mise à jour des étapes de travail pour l'utilisation de macros  
La table suivante décrit comment le remplacement des jetons est contrôlé par [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent. Pour activer/désactiver le remplacement des jetons, cliquez avec le bouton droit sur **SQL Server Agent** dans l’Explorateur d’objets, sélectionnez **Propriétés**, puis dans la page **Système d’alerte** , cochez ou décochez la case **Remplacer les jetons pour toutes les réponses de travaux aux alertes** .  
  
|Syntaxe des jetons|Remplacement de jetons d'alerte activé|Remplacement de jetons d'alerte désactivé|  
|----------------|------------------------------|-------------------------------|  
|Macro ESCAPE utilisée|Tous les jetons des travaux sont remplacés avec succès.|Les jetons activés par des alertes ne sont pas remplacés. Ces jetons sont **A-DBN**, **A-SVR**, **A-ERR**, **A-SEV**, **A-MSG** et **WMI(** _propriété_ **)** . Les autres jetons statiques sont remplacés comme il se doit.|  
|Aucune macro ESCAPE utilisée|Échec de tous les travaux contenant des jetons.|Échec de tous les travaux contenant des jetons.|  
  
## <a name="token-syntax-update-examples"></a>Exemples de mise à jour de syntaxe de jeton  
  
### <a name="a-using-tokens-in-non-nested-strings"></a>R. Utilisation de jetons dans des chaînes non imbriquées  
L'exemple suivant explique comment mettre à jour un script non imbriqué simple avec la macro d'échappement appropriée. Avant d'exécuter le script de mise à jour, le script d'étape de travail suivant utilise un jeton d'étape de travail pour imprimer le nom de base de données correct :  
  
`PRINT N'Current database name is $(A-DBN)' ;`  
  
Après l'exécution du script de mise à jour, une `ESCAPE_NONE` macro est insérée avant le jeton `A-DBN` . Du fait que les guillemets simples sont utilisés pour délimiter la chaîne d'impression, vous devez mettre à jour l'étape de travail en insérant la macro `ESCAPE_SQUOTE` comme suit :  
  
`PRINT N'Current database name is $(ESCAPE_SQUOTE(A-DBN))' ;`  
  
### <a name="b-using-tokens-in-nested-strings"></a>B. Utilisation de jetons dans des chaînes imbriquées  
Dans les scripts d'étape de travail où les jetons sont utilisés dans des chaînes ou des instructions imbriquées, les instructions imbriquées doivent être réécrites sous forme de plusieurs instructions avant d'insérer les macros d'échappement appropriées.  
  
Par exemple, imaginez l'étape de travail suivante qui utilise le jeton `A-MSG` et n'a pas été mise à jour avec une macro d'échappement :  
  
`PRINT N'Print ''$(A-MSG)''' ;`  
  
Après l'exécution du script de mise à jour, une macro `ESCAPE_NONE` est insérée avec le jeton. Néanmoins, dans ce cas, il vous faudrait réécrire le script sans avoir recours à l'imbrication comme illustré ci-après, puis insérer la macro `ESCAPE_SQUOTE` pour interpréter correctement en tant que caractères d'échappement les délimiteurs susceptibles d'être transmis à la chaîne de remplacement des jetons :  
  
```sql
DECLARE @msgString nvarchar(max);
SET @msgString = '$(ESCAPE_SQUOTE(A-MSG))';
SET @msgString = QUOTENAME(@msgString,'''');
PRINT N'Print ' + @msgString;
```
  
Notez également dans cet exemple que la fonction QUOTENAME définit le guillemet.  
  
### <a name="c-using-tokens-with-the-escape_none-macro"></a>C. Utilisation de jetons avec la macro ESCAPE_NONE  
L'exemple suivant s'inscrit dans un script chargé d'extraire le `job_id` dans la table `sysjobs` et utilise le jeton `JOBID` pour remplir la variable `@JobID` déclarée plus tôt dans le script en tant que type de données binaire. Notez que, du fait qu'aucun délimiteur n'est requis pour les types de données binaires, la macro `ESCAPE_NONE` est utilisée dans le jeton `JOBID` . Vous n'avez pas besoin de mettre ce travail à jour après avoir exécuté le script de mise à niveau.  
  
```sql
SELECT * FROM msdb.dbo.sysjobs  
WHERE @JobID = CONVERT(uniqueidentifier, $(ESCAPE_NONE(JOBID)));
```
  
## <a name="see-also"></a>Voir aussi  
[Implémenter des travaux](../../ssms/agent/implement-jobs.md)  
[Gérer les étapes de travail](../../ssms/agent/manage-job-steps.md)  
