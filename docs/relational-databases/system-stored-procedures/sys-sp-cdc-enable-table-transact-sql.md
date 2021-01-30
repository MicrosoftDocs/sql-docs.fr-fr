---
description: sys.sp_cdc_enable_table (Transact-SQL)
title: sys.sp_cdc_enable_table (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.sp_cdc_enable_table_TSQL
- sp_cdc_enable_table_TSQL
- sys.sp_cdc_enable_table
- sp_cdc_enable_table
dev_langs:
- TSQL
helpviewer_keywords:
- change data capture [SQL Server], enabling tables
- sys.sp_cdc_enable_table
- sp_cdc_enable_table
ms.assetid: 26150c09-2dca-46ad-bb01-3cb3165bcc5d
author: markingmyname
ms.author: maghan
ms.openlocfilehash: fe8e347cb82fa1b89ad03ebb152a8d0257f0985b
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99205988"
---
# <a name="syssp_cdc_enable_table-transact-sql"></a>sys.sp_cdc_enable_table (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Active la capture de données modifiées pour la table source spécifiée dans la base de données actuelle. Lorsqu'une table est activée pour la capture de données modifiées, un enregistrement de chaque opération DML (Data Manipulation Language) appliquée à la table est écrit dans le journal des transactions. Le processus de capture de données modifiées extrait ces informations du journal et les écrit dans les tables de modifications accédées à l'aide d'un ensemble de fonctions.  
  
 La capture des modifications de données n’est pas disponible dans toutes les éditions de [!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Pour obtenir la liste des fonctionnalités prises en charge par les éditions de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], consultez [Fonctionnalités prise en charge par les éditions de SQL Server 2016](~/sql-server/editions-and-supported-features-for-sql-server-2016.md).  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sys.sp_cdc_enable_table   
  [ @source_schema = ] 'source_schema',   
  [ @source_name = ] 'source_name' ,  [,[ @capture_instance = ] 'capture_instance' ]  
  [,[ @supports_net_changes = ] supports_net_changes ]  
  , [ @role_name = ] 'role_name'  
  [,[ @index_name = ] 'index_name' ]  
  [,[ @captured_column_list = ] 'captured_column_list' ]  
  [,[ @filegroup_name = ] 'filegroup_name' ]  
  [,[ @allow_partition_switch = ] 'allow_partition_switch' ]  
  [;]  
```  
  
## <a name="arguments"></a>Arguments  
`[ @source_schema = ] 'source_schema'` Nom du schéma auquel appartient la table source. *source_schema* est de **type sysname**, sans valeur par défaut et ne peut pas avoir la valeur null.  
  
`[ @source_name = ] 'source_name'` Nom de la table source sur laquelle activer la capture de données modifiées. *source_name* est de **type sysname**, sans valeur par défaut et ne peut pas avoir la valeur null.  
  
 *source_name* doit exister dans la base de données active. Les tables dans le schéma **CDC** ne peuvent pas être activées pour la capture de données modifiées.  
  
`[ @role_name = ] 'role_name'` Nom du rôle de base de données utilisé pour la passerelle pour l’accès aux données modifiées. *role_name* est de **type sysname** et doit être spécifié. En cas de définition explicitement sur NULL, aucun rôle de régulation n'est utilisé pour limiter l'accès aux données de modifications.  
  
 Si le rôle existe actuellement, il est utilisé. Si le rôle n'existe pas, une tentative est faite pour créer un rôle de base de données avec le nom spécifié. L'espace blanc est retiré à la droite de la chaîne dans le nom de rôle avant d'essayer de créer le rôle. Si l'appelant n'est pas autorisé à créer un rôle dans la base de données, la procédure stockée échoue.  
  
`[ @capture_instance = ] 'capture_instance'` Nom de l’instance de capture utilisée pour nommer les objets de capture de données modifiées spécifiques à l’instance. *capture_instance* est de **type sysname** et ne peut pas avoir la valeur null.  
  
 S’il n’est pas spécifié, le nom est dérivé du nom de schéma source plus le nom de la table source au format *schemaname_sourcename*. *capture_instance* ne peut pas dépasser 100 caractères et doit être unique dans la base de données. Qu’il soit spécifié ou dérivé, *capture_instance* est supprimé de tout espace blanc à droite de la chaîne.  
  
 Une table source peut avoir un maximum de deux instances de capture. Pour plus d’informations, consultez [sys.sp_cdc_help_change_data_capture &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sys-sp-cdc-help-change-data-capture-transact-sql.md).  
  
`[ @supports_net_changes = ] supports_net_changes` Indique si la prise en charge de l’interrogation des modifications nettes doit être activée pour cette instance de capture. *supports_net_changes* est de **bits** avec 1 comme valeur par défaut si la table a une clé primaire ou si la table a un index unique identifié à l’aide du @index_name paramètre. Sinon, le paramètre a comme valeur par défaut 0.  
  
 S'il a la valeur 0, seules les fonctions de prise en charge d'interrogation de toutes les modifications sont générées.  
  
 S'il a la valeur 1, les fonctions nécessaires pour interroger les modifications nettes sont générées également.  
  
 Si *supports_net_changes* a la valeur 1, *index_name* doit être spécifié ou la table source doit avoir une clé primaire définie.  
  
`[ @index_name = ] 'index_name_'` Nom d’un index unique à utiliser pour identifier de manière unique les lignes dans la table source. *index_name* est de **type sysname** et peut avoir la valeur null. S’il est spécifié, *index_name* doit être un index unique valide sur la table source. Si *index_name* est spécifié, les colonnes d’index identifiées sont prioritaires sur les colonnes de clé primaire définies comme identificateur de ligne unique pour la table.  
  
`[ @captured_column_list = ] 'captured_column_list'` Identifie les colonnes de la table source qui doivent être incluses dans la table de modifications. *captured_column_list* est de type **nvarchar (max)** et peut avoir la valeur null. Si la valeur est NULL, toutes les colonnes sont incluses dans la table de modifications.  
  
 Les noms de colonnes doivent être des colonnes valides dans la table source. Les colonnes définies dans un index de clé primaire, ou les colonnes définies dans un index référencé par *index_name* doivent être incluses.  
  
 *captured_column_list* est une liste de noms de colonnes séparés par des virgules. Des noms de colonnes individuels dans la liste peuvent être cités en utilisant des guillemets doubles ("") ou des crochets ([]). Si un nom de colonne contient une virgule incorporée, il doit être entouré de guillemets.  
  
 *captured_column_list* ne peut pas contenir les noms de colonnes réservés suivants : **_ _ $ start_lsn**, **_ _ $ end_lsn**, **_ _ $ seqval**, **_ _ $ Operation** et **_ _ $ update_mask**.  
  
`[ @filegroup_name = ] 'filegroup_name'` Groupe de fichiers à utiliser pour la table de modifications créée pour l’instance de capture. *filegroup_name* est de **type sysname** et peut avoir la valeur null. S’il est spécifié, *filegroup_name* doit être défini pour la base de données actuelle. Si la valeur est NULL, le groupe de fichiers par défaut est utilisé.  
  
 Nous recommandons de créer un groupe de fichiers séparé pour les tables de modifications de capture des données modifiées.  
  
`[ @allow_partition_switch = ] 'allow_partition_switch'` Indique si la commande SWITCH PARTITION d’ALTER TABLE peut être exécutée sur une table qui est activée pour la capture de données modifiées. *allow_partition_switch* est de **bits**, avec 1 comme valeur par défaut.  
  
 Pour les tables non partitionnées, le paramètre de commutation est toujours 1 et le paramètre réel est ignoré. Si la commutation est définie explicitement sur 0 pour une table non partitionnée, un avertissement 22857 est émis pour indiquer que le paramètre de commutation a été ignoré. Si la commutation est définie explicitement sur 0 pour une table partitionnée, un avertissement 22356 est émis pour indiquer que les opérations de commutation de partition sur la table source seront rejetées. Pour finir, si le paramètre de commutation est défini explicitement sur 1 ou autorisé à avoir comme valeur par défaut 1 et que la table active est partitionnée, un avertissement 22855 est émis pour indiquer que les commutations de partition ne seront pas bloquées. Si une commutation de partition se produit, la capture de données modifiées n'effectue pas le suivi des modifications qui résultent de la commutation. Cela provoquera des incohérences de données lors de la consommation des données de modifications.  
  
> [!IMPORTANT]  
>  SWITCH PARTITION est une opération de métadonnées, mais elle entraîne des modifications de données. Les modifications de données associées à cette opération ne sont pas capturées dans les tables de modifications de capture de données modifiées. Pensez à une table qui comprend trois partitions et à laquelle des modifications sont apportées. Le processus de capture effectuera le suivi des insertions, mises à jour et suppressions exécutées par l'utilisateur sur cette table. Toutefois, si une partition est extraite dans une autre table (par exemple, pour effectuer une suppression en bloc), les lignes déplacées dans le cadre de cette opération ne seront pas capturées en tant que lignes supprimées dans la table de modifications. De la même façon, si une nouvelle partition qui comprend des lignes préremplies est ajoutée à la table, ces lignes ne se seront pas répercutées dans la table de modifications. Cette situation peut aboutir à des données incohérentes lorsque les modifications sont utilisées par une application et appliquées à une destination.  
  
## <a name="return-code-values"></a>Codet de retour  
 **0** (succès) ou **1** (échec)  
  
## <a name="result-sets"></a>Jeux de résultats  
 None  
  
## <a name="remarks"></a>Notes  
 Pour pouvoir activer une table pour la capture de données modifiées, la base de données doit être activée. Pour déterminer si la base de données est activée pour la capture de données modifiées, interrogez la colonne **is_cdc_enabled** de l’affichage catalogue [sys. databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md) . Pour activer la base de données, utilisez la procédure stockée [sys.sp_cdc_enable_db](../../relational-databases/system-stored-procedures/sys-sp-cdc-enable-db-transact-sql.md) .  
  
 Lorsque la capture de données modifiées est activée pour une table, une table de modifications et une ou deux fonctions de requêtes sont générées. La table de modifications sert de base de données de référentiel pour les modifications de table source extraites du journal des transactions par le processus de capture. Les fonctions de requête sont utilisées pour extraire des données de la table de modifications. Les noms de ces fonctions sont dérivés du paramètre *capture_instance* des manières suivantes :  
  
-   Fonction All changes : **cdc.fn_cdc_get_all_changes_<capture_instance>**  
  
-   Fonction net changes : **cdc.fn_cdc_get_net_changes_<capture_instance>**  
  
 **sys.sp_cdc_enable_table** crée également les travaux de capture et de nettoyage pour la base de données si la table source est la première table de la base de données à activer pour la capture de données modifiées et qu’il n’existe aucune publication transactionnelle pour la base de données. Elle affecte la valeur 1 à la colonne **is_tracked_by_cdc** de l’affichage catalogue [sys. tables](../../relational-databases/system-catalog-views/sys-tables-transact-sql.md) .  
  
> [!NOTE]  
>  Il n'est pas nécessaire que l'agent [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] soit en cours d'exécution lorsque la capture de données modifiées est activée pour une table. Toutefois, le processus de capture ne traitera pas le journal des transactions et n'écrira pas d'entrées dans la table de modifications si l'Agent [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ne s'exécute pas.  
  
## <a name="permissions"></a>Autorisations  
 Requiert l’appartenance au rôle de base de données fixe **db_owner** .  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-enabling-change-data-capture-by-specifying-only-required-parameters"></a>R. Activation de la capture des données modifiées en spécifiant uniquement les paramètres requis  
 L'exemple suivant active la capture des données modifiées pour la table `HumanResources.Employee`. Seuls les paramètres requis sont spécifiés.  
  
```  
USE AdventureWorks2012;  
GO  
EXECUTE sys.sp_cdc_enable_table  
    @source_schema = N'HumanResources'  
  , @source_name = N'Employee'  
  , @role_name = N'cdc_Admin';  
GO  
```  
  
### <a name="b-enabling-change-data-capture-by-specifying-additional-optional-parameters"></a>B. Activation de la capture de données modifiées en spécifiant des paramètres facultatifs supplémentaires  
 L'exemple suivant active la capture des données modifiées pour la table `HumanResources.Department`. Tous les paramètres sauf `@allow_partition_switch` sont spécifiés.  
  
```  
USE AdventureWorks2012;  
GO  
EXEC sys.sp_cdc_enable_table  
    @source_schema = N'HumanResources'  
  , @source_name = N'Department'  
  , @role_name = N'cdc_admin'  
  , @capture_instance = N'HR_Department'   
  , @supports_net_changes = 1  
  , @index_name = N'AK_Department_Name'   
  , @captured_column_list = N'DepartmentID, Name, GroupName'   
  , @filegroup_name = N'PRIMARY';  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [sys.sp_cdc_disable_table &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sys-sp-cdc-disable-table-transact-sql.md)   
 [sys.sp_cdc_help_change_data_capture &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sys-sp-cdc-help-change-data-capture-transact-sql.md)   
 [cdc.fn_cdc_get_all_changes_&#60;capture_instance&#62;  &#40;Transact-SQL&#41;](../../relational-databases/system-functions/cdc-fn-cdc-get-all-changes-capture-instance-transact-sql.md)   
 [cdc.fn_cdc_get_net_changes_&#60;capture_instance&#62; &#40;Transact-SQL&#41;](../../relational-databases/system-functions/cdc-fn-cdc-get-net-changes-capture-instance-transact-sql.md)   
 [sys.sp_cdc_help_jobs &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sys-sp-cdc-help-jobs-transact-sql.md)  
  
  
