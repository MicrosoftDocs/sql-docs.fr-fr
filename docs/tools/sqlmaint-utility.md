---
title: sqlmaint (utilitaire)
description: Dans SQL Server, utilisez sqlmaint pour exécuter des vérifications DBCC, sauvegarder une base de données et son journal des transactions, mettre à jour des statistiques et reconstruire des index.
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.technology: tools-other
ms.topic: conceptual
helpviewer_keywords:
- database maintenance plans [SQL Server]
- sqlmaint utility
- maintaining databases [SQL Server]
- backups [SQL Server], sqlmaint utility
- command prompt utilities [SQL Server], sqlmaint
- maintenance plans [SQL Server], command prompt
- backing up [SQL Server], sqlmaint utility
ms.assetid: 937a9932-4aed-464b-b97a-a5acfe6a50de
author: markingmyname
ms.author: maghan
ms.openlocfilehash: c822bd2acbd92bcfd9b3008c78c3ccef244f3c88
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100338554"
---
# <a name="sqlmaint-utility"></a>sqlmaint (utilitaire)
[!INCLUDE[sqlserver](../includes/applies-to-version/sqlserver.md)]
  L’utilitaire **sqlmaint** exécute un ensemble spécifique d’opérations de maintenance sur une ou plusieurs bases de données. Utilisez **sqlmaint** pour exécuter des vérifications DBCC, sauvegarder une base de données et son journal des transactions, mettre à jour des statistiques et reconstruire des index. Toutes les activités de maintenance de base de données produisent un rapport qui peut être envoyé vers un fichier texte, un fichier HTML ou un compte de messagerie déterminé. **sqlmaint** exécute les plans de maintenance de base de données créés avec des versions précédentes de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]. Pour exécuter des plans de maintenance [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] à partir de l’invite de commandes, utilisez [l’utilitaire dtexec](../integration-services/packages/dtexec-utility.md).  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteDepNextAvoid](../includes/ssnotedepnextavoid-md.md)] Utilisez plutôt la fonction plan de maintenance de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] . Pour plus d’informations sur les plans de maintenance, consultez [Plans de maintenance](../relational-databases/maintenance-plans/maintenance-plans.md).  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sqlmaint   
[-?] |  
[  
     [-S server_name[\instance_name]]  
     [-U login_ID [-P password]]  
     {  
          [-D database_name | -PlanName name | -PlanID guid ]  
          [-Rpt text_file]  
          [-To operator_name]  
          [-HtmlRpt html_file [-DelHtmlRpt <time_period>] ]  
          [-RmUnusedSpace threshold_percentfree_percent]  
          [-CkDB | -CkDBNoIdx]  
          [-CkAl | -CkAlNoIdx]  
          [-CkCat]  
          [-UpdOptiStats sample_percent]  
          [-RebldIdx free_space]  
          [-SupportComputedColumn]  
          [-WriteHistory]  
          [  
               {-BkUpDB [backup_path] | -BkUpLog [backup_path] }  
               {-BkUpMedia  
                    {DISK [  
                           [-DelBkUps <time_period>]   
                           [-CrBkSubDir ]   
                           [-UseDefDir ]   
                          ]  
                     | TAPE   
                    }  
               }  
               [-BkUpOnlyIfClean]  
               [-VrfyBackup]  
          ]  
     }  
]  
<time_period> ::=  
number[minutes | hours | days | weeks | months]  
```  
  
## <a name="arguments"></a>Arguments  
 Les paramètres et leurs valeurs doivent être séparés par un espace. Par exemple, il doit exister un espace entre **-S** et *server_name*.  
  
 **-?**  
 Spécifie que le diagramme de syntaxe pour l’utilitaire **sqlmaint** doit être retourné. Ce paramètre doit être utilisé seul.  
  
 **-S** _server_name_[ **\\** _instance\_name_]  
 Spécifie l’instance cible de [!INCLUDE[msCoName](../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]. Spécifiez _server\_name_ pour vous connecter à l’instance par défaut de [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] sur ce serveur. Spécifiez _server\_name_ **\\** _instance\_name_ pour vous connecter à une instance nommée de [!INCLUDE[ssDE](../includes/ssde-md.md)] sur ce serveur. Si aucun serveur n’est spécifié, **sqlmaint** se connecte à l’instance par défaut de [!INCLUDE[ssDE](../includes/ssde-md.md)] sur l’ordinateur local.  
  
 **-U** _login_ID_  
 Spécifie l'ID de connexion à utiliser lors de la connexion au serveur. Si celui-ci n’est pas fourni, **sqlmaint** tente d’utiliser l’authentification [!INCLUDE[msCoName](../includes/msconame-md.md)] Windows. Si *login_ID* contient des caractères spéciaux, il doit être encadré par des guillemets doubles. Ceux-ci sont facultatifs dans tous les autres cas.  
  
> [!IMPORTANT]  
>  Lorsque c'est possible, utilisez l'authentification Windows.  
  
 **-P** _password_  
 Spécifie le mot de passe de l’ID de connexion. Uniquement valide si le paramètre **-U** est également fourni. Si le *password* contient des caractères spéciaux, il doit être encadré par des guillemets doubles ; ceux-ci sont facultatifs dans tous les autres cas.  
  
> [!IMPORTANT]  
>  Le mot de passe n'est pas masqué. Lorsque c'est possible, utilisez l'authentification Windows.  
  
 **-D** _database_name_  
 Spécifie le nom de la base de données dans laquelle l'opération de maintenance doit être effectuée. Si *database_name* contient des caractères spéciaux, il doit être encadré par des guillemets doubles. Ceux-ci sont facultatifs dans tous les autres cas.  
  
 **-PlanName** _name_  
 Spécifie le nom du plan de maintenance de base de données défini par l'Assistant Plan de maintenance de base de données. La seule information du plan utilisée par **sqlmaint** est la liste des bases de données qui y figurent. Toutes les activités de maintenance spécifiées dans les autres paramètres de **sqlmaint** s’appliquent à cette liste de bases de données.  
  
 **-PlanID** _guid_  
 Spécifie l'identificateur global unique (GUID) d'un plan de maintenance de base de données à l'aide de l'Assistant Plan de maintenance de base de données. La seule information du plan utilisée par **sqlmaint** est la liste des bases de données qui y figurent. Toutes les activités de maintenance spécifiées dans les autres paramètres de **sqlmaint** s’appliquent à cette liste de bases de données. Cela doit correspondre à une valeur plan_id dans msdb.dbo.sysdbmaintplans.  
  
 **-Rpt** _text_file_  
 Spécifie le chemin et le nom complet du fichier dans lequel le rapport doit être créé. Le rapport est aussi créé à l'écran. Le rapport conserve les informations de version en ajoutant une date au nom de fichier. La date est générée comme suit : à la fin du nom de fichier, mais avant le point, sous la forme _ *yyyyMMddhhmm*. *yyyy* = année, *MM* = mois, *dd* = jour, *hh* = heure, *mm* = minute.  
  
 Si vous exécutez l'utilitaire à 10h23 le 1er décembre 1996, avec la valeur *text_file* suivante :  
  
```  
c:\Program Files\Microsoft SQL Server\Mssql\Backup\AdventureWorks2012_maint.rpt  
```  
  
 Le nom du fichier créé est :  
  
```  
c:\Program Files\Microsoft SQL Server\Mssql\Backup\AdventureWorks2012_maint_199612011023.rpt  
```  
  
 Le nom de fichier complet UNC (Universal Naming Convention) est requis pour *text_file* lorsque **sqlmaint** accède à un serveur distant.  
  
 **-To**  _operator_name_  
 Spécifie l'opérateur auquel le rapport créé sera envoyé par l'intermédiaire de SQL Mail.  
  
 **-HtmlRpt** _html_file_  
 Spécifie le chemin et le nom complets du fichier dans lequel un rapport HTML doit être créé. **sqlmaint** crée le nom de fichier en lui ajoutant une chaîne de type _ *yyyyMMddhhmm* de la même manière que pour le paramètre **-Rpt** .  
  
 Le nom de fichier complet UNC est requis pour *html_file* lorsque **sqlmaint** accède à un serveur distant.  
  
 **-DelHtmlRpt** \<*time_period*>  
 Spécifie que tous les rapports HTML du répertoire des rapports doivent être supprimés si l'intervalle de temps écoulé depuis la création du fichier de rapport excède la valeur \<*time_period*>. **-DelHtmlRpt** recherche les fichiers dont les noms correspondent au modèle créé à partir du paramètre *html_file*. Si *html_file* est c:\Program Files\Microsoft SQL Server\Mssql\Backup\AdventureWorks2012_maint.htm, **-DelHtmlRpt** provoque la suppression par **sqlmaint** de tous les fichiers dont le nom correspond au modèle C:\Program Files\Microsoft SQL Server\Mssql\Backup\AdventureWorks2012_maint\*.htm et qui sont antérieurs à \<*time_period*>.  
  
 **-RmUnusedSpace** _threshold_percent free_percent_  
 Spécifie que l’espace inutilisé est retiré de la base de données spécifiée dans **-D**. Cette option est uniquement utile pour les bases de données dont la configuration prévoit une croissance automatique. *Threshold_percent* indique, en Mo, la taille qu’une base de données doit atteindre pour que **sqlmaint** tente d’en supprimer l’espace de données inutilisé. Si la base de données est plus petite que *threshold_percent*, aucune action n’est exécutée. *Free_percent* spécifie l’espace qui doit rester libre dans la base de données, exprimé sous la forme d’un pourcentage de la taille finale de celle-ci. Prenons l’exemple d’une base de données de 200 Mo contenant 100 Mo de données. Si la valeur 10 est affectée à *free_percent* , la base de données a pour taille finale 110 Mo. La base de données n’est pas étendue si elle est inférieure à la valeur de *free_percent* augmentée de la quantité de données contenue dans la base de données. Prenons l’exemple d’une base de données de 108 Mo contenant 100 Mo de données. Si la valeur 10 est affectée à *free_percent* , la base de données ne passera pas à 110 Mo, mais conserve sa taille de 108 Mo.  
  
 **-CkDB** |  **-CkDBNoIdx**  
 Spécifie qu’une instruction DBCC CHECKDB ou une instruction DBCC CHECKDB avec l’option NOINDEX est exécutée dans la base de données indiquée dans **-D**. Pour plus d'informations, consultez DBCC CHECKDB.  
  
 Un avertissement est enregistré dans *text_file* si la base de données est en cours d’utilisation lorsque **sqlmaint** est exécuté.  
  
 **-CkAl** |  **-CkAlNoIdx**  
 Spécifie qu’une instruction DBCC CHECKALLOC avec l’option NOINDEX est exécutée dans la base de données indiquée dans **-D**. Pour plus d’informations, consultez [DBCC CHECKALLOC &#40;Transact-SQL&#41;](../t-sql/database-console-commands/dbcc-checkalloc-transact-sql.md).  
  
 **-CkCat**  
 Spécifie qu’une instruction DBCC CHECKCATALOG (Transact-SQL) est exécutée dans la base de données indiquée dans **-D**. Pour plus d’informations, consultez [DBCC CHECKCATALOG &#40;Transact-SQL&#41;](../t-sql/database-console-commands/dbcc-checkcatalog-transact-sql.md).  
  
 **-UpdOptiStats** _sample_percent_  
 Spécifie que l'instruction suivante doit être exécutée sur chacune des tables de la base de données :  
  
```  
UPDATE STATISTICS table WITH SAMPLE sample_percent PERCENT;  
```  
  
 Si les tables contiennent des colonnes calculées, vous devez également spécifier l’argument **-SupportedComputedColumn** lorsque vous utilisez **-UpdOptiStats**.  
  
 Pour plus d’informations, consultez [UPDATE STATISTICS &#40;Transact-SQL&#41;](../t-sql/statements/update-statistics-transact-sql.md).  
  
 **-RebldIdx** _free_space_  
 Spécifie que les index sur les tables de la base de données cible doivent être reconstruits sur base d’un pourcentage *free_space* inversement proportionnel au facteur de remplissage. Par exemple, si le pourcentage *free_space* est égal à 30, le facteur de remplissage utilisé est 70. Si le pourcentage indiqué dans *free_space* a comme valeur 100, les index sont reconstruits sur base du facteur de remplissage d’origine.  
  
 Si les index se trouvent sur des colonnes calculées, vous devez également spécifier l’argument **-SupportComputedColumn** lorsque vous utilisez **-RebldIdx**.  
  
 **-SupportComputedColumn**  
 Doit être spécifié pour l’exécution de commandes de maintenance DBCC avec **sqlmaint** sur des colonnes calculées.  
  
 **-WriteHistory**  
 Spécifie que chaque action de maintenance effectuée par **sqlmaint** fait l’objet d’une entrée dans msdb.dbo.sysdbmaintplan_history. Si **-PlanName** ou **-PlanID** est spécifié, les entrées de sysdbmaintplan_history utilisent l’ID du plan spécifié. Si **-D** est spécifié, l’ID du plan est remplacé par des zéros dans les entrées de sysdbmaintplan_history.  
  
 **-BkUpDB** [ *backup_path*] |  **-BkUpLog** [ *backup_path* ]  
 Spécifie une action de sauvegarde. **-BkUpDb** sauvegarde l’ensemble de la base de données. **-BkUpLog** sauvegarde uniquement le journal des transactions.  
  
 *backup_path* spécifie le répertoire pour la sauvegarde. *backup_path* n’est pas nécessaire si **-UseDefDir** est également spécifié et il est remplacé par la valeur de **-UseDefDir** si toutes deux sont spécifiées. La sauvegarde peut être placée à une adresse de répertoire ou de périphérique à bande (par exemple, \\\\.\TAPE0). Le nom de fichier pour la sauvegarde d'une base de données est créé automatiquement comme suit :  
  
```  
dbname_db_yyyyMMddhhmm.BAK  
  
```  
  
 where  
  
-   *dbname* est le nom de la base de données actuellement sauvegardée.  
  
-   *yyyyMMddhhmm* correspond à la date et à l’heure de l’opération de sauvegarde où *yyyy* = année, *MM* = mois, *dd* = jour, *hh* = jour et *mm* = minute.  
  
 Le nom de fichier d'une transaction de sauvegarde est automatiquement créé sous la forme suivante :  
  
```  
dbname_log_yyyymmddhhmm.BAK  
  
```  
  
 Si vous utilisez le paramètre **-BkUpDB** , vous devez aussi spécifier le support de sauvegarde à l’aide du paramètre **-BkUpMedia** .  
  
 **-BkUpMedia**  
 Précise le type de support utilisé pour la sauvegarde, DISK ou TAPE.  
  
 **DISK**  
 Spécifie que le support de sauvegarde est un disque.  
  
 **-DelBkUps**< *time_period* >  
 Pour les sauvegardes sur disque, spécifie que tous les fichiers de sauvegarde du répertoire de sauvegarde doivent être supprimés si l'intervalle de temps écoulé depuis la création de la sauvegarde excède la valeur indiquée dans \<*time_period*>.  
  
 **-CrBkSubDir**  
 Pour les sauvegardes sur disque, spécifie qu’un sous-répertoire doit être créé dans le répertoire [*backup_path*] ou dans le répertoire de sauvegarde par défaut si **-UseDefDir** est également spécifié. Le nom du sous-répertoire est généré à partir du nom de base de données spécifié dans **-D**. **-CrBkSubDir** offre un moyen aisé de placer toutes les sauvegardes de différentes bases de données dans des sous-répertoires séparés sans devoir modifier le paramètre *backup_path* .  
  
 **-UseDefDir**  
 Pour les sauvegardes sur disque, spécifie que le fichier de sauvegarde doit être créé dans le répertoire de sauvegarde par défaut. **UseDefDir** est prioritaire sur *backup_path* si ces deux paramètres sont spécifiés. Avec une installation de [!INCLUDE[msCoName](../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] par défaut, le répertoire de sauvegarde par défaut est C:\Program Files\Microsoft SQL Server\MSSQL10_50.MSSQLSERVER\MSSQL\Backup.  
  
 **TAPE**  
 Spécifie que le support de sauvegarde est une bande.  
  
 **-BkUpOnlyIfClean**  
 Spécifie que la sauvegarde a lieu uniquement si les contrôles **-Ck** spécifiés n’ont rencontré aucun problème au niveau des données. Les actions de maintenance s'exécutent dans le même ordre que celui dans lequel elles apparaissent dans la ligne de commande. Spécifiez les paramètres **-CkDB**, **-CkDBNoIdx**, **-CkAl**, **-CkAlNoIdx**, **-CkTxtAl** ou **-CkCat** avant le(s) paramètre(s) **-BkUpDB**/ **-BkUpLog** si vous allez également spécifier **-BkUpOnlyIfClean** ou la sauvegarde a lieu, même si la vérification signale des problèmes ou pas.  
  
 **-VrfyBackup**  
 Spécifie que RESTORE VERIFYONLY est exécuté sur la sauvegarde lorsque celle-ci est terminée.  
  
 *nombre*[**minutes**\| **heures**\| **jour**\| **semaines**\| **mois**]  
 Spécifie l'intervalle de temps utilisé pour déterminer si un rapport ou un fichier de sauvegarde est suffisamment ancien pour être supprimé. *number* est un entier suivi (sans espace) d’une unité de temps. Exemples valides :  
  
-   **12weeks**  
  
-   **3months**  
  
-   **15days**  
  
 Si seul le paramètre *number* est spécifié, la partie de la date sélectionnée par défaut est **semaines**.  
  
## <a name="remarks"></a>Notes  
 L’utilitaire **sqlmaint** effectue des opérations de maintenance sur une ou plusieurs bases de données. Si **-D** est spécifié, les opérations spécifiées par ailleurs portent uniquement sur la base de données indiquée. Si **-PlanName** ou **-PlanID** est spécifié, la seule information récupérée du plan de maintenance indiqué par **sqlmaint** est la liste des bases de données contenues dans le plan. Toutes les opérations spécifiées dans les autres paramètres de **sqlmaint** s’appliquent à chaque base de données figurant sur la liste extraite du plan, l’utilitaire **sqlmaint** n’effectuant aucune opération de maintenance définie dans le plan lui-même.  
  
 L’utilitaire **sqlmaint** retourne 0 en cas de réussite et 1 en cas d’échec. L'échec de l'exécution est signalé :  
  
-   Les opérations de maintenance échouent dans les ces suivants :  
  
-   Si les vérifications **-CkDB**, **-CkDBNoIdx**, **-CkAl**, **-CkAlNoIdx**, **-CkTxtAl** ou **-CkCat** trouvent des problèmes sur les données.  
  
-   En cas de panne générale.  
  
## <a name="permissions"></a>Autorisations  
 L’utilitaire **sqlmaint** peut être exécuté par tout utilisateur Windows disposant de l’autorisation **Lecture et exécution** sur `sqlmaint.exe`, qui est stockée par défaut dans le dossier `x:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER1\MSSQL\Binn` . En outre, la connexion [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] spécifiée avec **-login_ID** doit disposer des autorisations [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] requises pour effectuer l’action spécifiée. Si la connexion à [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] utilise l'authentification Windows, le nom d'ouverture de session [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] mappé à l'utilisateur Windows authentifié doit disposer des autorisations [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] requises pour effectuer l'action spécifiée.  
  
 Par exemple, l’utilisation de l’argument **-BkUpDB** nécessite l’autorisation d’exécution de l’instruction BACKUP. L’utilisation de l’argument **-UpdOptiStats** nécessite l’autorisation d’exécution de l’instruction UPDATE STATISTICS. Pour plus d'informations, consultez la section « Autorisations » des rubriques correspondantes de la documentation en ligne.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-performing-dbcc-checks-on-a-database"></a>R. Réalisation de vérifications DBCC sur une base de données  
  
```  
sqlmaint -S MyServer -D AdventureWorks2012 -CkDB -CkAl -CkCat -Rpt C:\MyReports\AdvWks_chk.rpt  
```  
  
### <a name="b-updating-statistics-using-a-15-sample-in-all-databases-in-a-plan-also-shrink-any-of-the-database-that-have-reached-110-mb-to-having-only-10-free-space"></a>B. Mise à jour des statistiques avec un échantillonnage de 15 % dans toutes les bases de données d'un plan. Compactage des bases de données ayant atteint 110 Mo de sorte qu'elles aient seulement 10 % d'espace libre  
  
```  
sqlmaint -S MyServer -PlanName MyUserDBPlan -UpdOptiStats 15 -RmUnusedSpace 110 10  
```  
  
### <a name="c-backing-up-all-the-databases-in-a-plan-to-their-individual-subdirectories-in-the-default-xprogram-filesmicrosoft-sql-servermssql13mssqlservermssqlbackup-directory-also-delete-any-backups-older-than-2-weeks"></a>C. Sauvegarde de toutes les bases de données d’un plan dans leurs sous-répertoires respectifs du répertoire par défaut x:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\Backup. Suppression de toute sauvegarde antérieure à 2 semaines  
  
```  
sqlmaint -S MyServer -PlanName MyUserDBPlan -BkUpDB -BkUpMedia DISK -UseDefDir -CrBkSubDir -DelBkUps 2weeks  
```  
  
### <a name="d-backing-up-a-database-to-the-default-xprogram-filesmicrosoft-sql-servermssql13mssqlservermssqlbackup-directory"></a>D. Sauvegarde d’une base de données dans le répertoire par défaut x:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\Backup.  
  
```  
sqlmaint -S MyServer -BkUpDB -BkUpMedia DISK -UseDefDir  
```  
  
## <a name="see-also"></a>Voir aussi  
 [BACKUP &#40;Transact-SQL&#41;](../t-sql/statements/backup-transact-sql.md)   
 [UPDATE STATISTICS &#40;Transact-SQL&#41;](../t-sql/statements/update-statistics-transact-sql.md)  
  
  
