---
title: sp_addarticle (Transact-SQL) | Microsoft Docs
description: Crée un article et l'ajoute à une publication. Cette procédure stockée s’exécute sur le serveur de publication de la base de données de publication.
ms.custom: ''
ms.date: 10/28/2015
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_addarticle
- sp_addarticle_TSQL
helpviewer_keywords:
- sp_addarticle
ms.assetid: 0483a157-e403-4fdb-b943-23c1b487bef0
author: mashamsft
ms.author: mathoma
ms.openlocfilehash: 5ee845eac866c3954ba03080c037179fa76ad519
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99182831"
---
# <a name="sp_addarticle-transact-sql"></a>sp_addarticle (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  Crée un article et l'ajoute à une publication. Cette procédure stockée est exécutée sur le serveur de publication dans la base de données de publication.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_addarticle [ @publication = ] 'publication'   
        , [ @article = ] 'article'   
    [ , [ @source_table = ] 'source_table' ]  
    [ , [ @destination_table = ] 'destination_table' ]   
    [ , [ @vertical_partition = ] 'vertical_partition' ]   
    [ , [ @type = ] 'type' ]   
    [ , [ @filter = ] 'filter' ]   
    [ , [ @sync_object= ] 'sync_object' ]   
        [ , [ @ins_cmd = ] 'ins_cmd' ]   
    [ , [ @del_cmd = ] 'del_cmd' ]   
        [ , [ @upd_cmd = ] 'upd_cmd' ]   
    [ , [ @creation_script = ] 'creation_script' ]   
    [ , [ @description = ] 'description' ]   
    [ , [ @pre_creation_cmd = ] 'pre_creation_cmd' ]   
    [ , [ @filter_clause = ] 'filter_clause' ]   
    [ , [ @schema_option = ] schema_option ]   
    [ , [ @destination_owner = ] 'destination_owner' ]   
    [ , [ @status = ] status ]   
    [ , [ @source_owner = ] 'source_owner' ]   
    [ , [ @sync_object_owner = ] 'sync_object_owner' ]   
    [ , [ @filter_owner = ] 'filter_owner' ]   
    [ , [ @source_object = ] 'source_object' ]   
    [ , [ @artid = ] article_ID  OUTPUT ]   
    [ , [ @auto_identity_range = ] 'auto_identity_range' ]   
    [ , [ @pub_identity_range = ] pub_identity_range ]   
    [ , [ @identity_range = ] identity_range ]   
    [ , [ @threshold = ] threshold ]   
    [ , [ @force_invalidate_snapshot = ] force_invalidate_snapshot ]  
    [ , [ @use_default_datatypes = ] use_default_datatypes  
    [ , [ @identityrangemanagementoption = ] identityrangemanagementoption ]  
    [ , [ @publisher = ] 'publisher' ]  
    [ , [ @fire_triggers_on_snapshot = ] 'fire_triggers_on_snapshot' ]   
```  
  
## <a name="arguments"></a>Arguments  
`[ @publication = ] 'publication'` Nom de la publication qui contient l’article. Le nom doit être unique dans la base de données. *publication* est de **type sysname**, sans valeur par défaut.  
  
`[ @article = ] 'article'` Nom de l’article. Le nom doit être unique dans la publication. *article* est de **type sysname** et n’a pas de valeur par défaut.  
  
`[ @source_table = ] 'source_table'` Ce paramètre est déconseillé ; Utilisez à la place *source_object* .  
  
 *Ce paramètre n’est pas pris en charge pour les serveurs de publication Oracle.*  
  
`[ @destination_table = ] 'destination_table'` Nom de la table de destination (abonnement), si elle est différente de *source_table* ou de la procédure stockée. *destination_table* est de **type sysname**, avec NULL comme valeur par défaut, ce qui signifie que *source_table* est égal à *destination_table * *.*  
  
`[ @vertical_partition = ] 'vertical_partition'` Active et désactive le filtrage de colonne sur un article de table. *vertical_partition* est de valeur **nchar (5)**, avec false comme valeur par défaut.  
  
 **false** indique qu’il n’y a pas de filtrage vertical et publie toutes les colonnes.  
  
 **true** efface toutes les colonnes, à l’exception de la clé primaire déclarée, des colonnes Nullable sans valeur par défaut et des colonnes clés uniques. Les colonnes sont ajoutées à l’aide de [sp_articlecolumn](../../relational-databases/system-stored-procedures/sp-articlecolumn-transact-sql.md).  
  
`[ @type = ] 'type'` Est le type d’article. *type* est de type **sysname** et peut prendre l’une des valeurs suivantes.  
  
|Valeur|Description|  
|-----------|-----------------|  
|**aggregate schema only**|Fonction d'agrégation avec schéma uniquement|  
|**func schema only**|Fonction avec schéma uniquement.|  
|**indexed view logbased**|Article de vue indexée reposant sur un journal. Non pris en charge pour les serveurs de publication Oracle. Pour ce type d'article, il n'est pas nécessaire de publier la table de base séparément.|  
|**indexed view logbased manualboth**|Article de vue indexée reposant sur un journal avec filtre manuel et vue manuelle. Cette option requiert que vous spécifiiez à la fois des *sync_object* et des paramètres de *filtre* . Pour ce type d'article, il n'est pas nécessaire de publier la table de base séparément. Non pris en charge pour les serveurs de publication Oracle.|  
|**indexed view logbased manualfilter**|Article de vue indexée reposant sur un journal avec filtre manuel. Cette option requiert que vous spécifiiez à la fois des *sync_object* et des paramètres de *filtre* . Pour ce type d'article, il n'est pas nécessaire de publier la table de base séparément. Non pris en charge pour les serveurs de publication Oracle.|  
|**indexed view logbased manualview**|Article de vue indexée reposant sur un journal avec vue manuelle. Cette option requiert que vous spécifiiez le paramètre *sync_object* . Pour ce type d'article, il n'est pas nécessaire de publier la table de base séparément. Non pris en charge pour les serveurs de publication Oracle.|  
|**indexed view schema only**|Vue indexée avec schéma uniquement. Pour ce type d'article, la table de base doit également être publiée.|  
|**logbased** (par défaut)|Article basé sur le journal.|  
|**logbased manualboth**|Article reposant sur un journal, avec filtre manuel et vue manuelle. Cette option requiert que vous spécifiiez à la fois des *sync_object* et des paramètres de *filtre* . Non pris en charge pour les serveurs de publication Oracle.|  
|**logbased manualfilter**|Article reposant sur un journal, avec filtre manuel. Cette option requiert que vous spécifiiez à la fois des *sync_object* et des paramètres de *filtre* . Non pris en charge pour les serveurs de publication Oracle.|  
|**logbased manualview**|Article reposant sur un journal, avec vue manuelle. Cette option requiert que vous spécifiiez le paramètre *sync_object* . Non pris en charge pour les serveurs de publication Oracle.|  
|**proc exec**|Réplique l'exécution de la procédure stockée pour tous les abonnés de l'article. Non pris en charge pour les serveurs de publication Oracle. Nous vous recommandons d’utiliser l’option **ProCable proc exec** au lieu de **proc exec**. Pour plus d’informations, consultez la section « types d’articles d’exécution de procédure stockée » dans [publication de l’exécution des procédures stockées dans la réplication transactionnelle](../../relational-databases/replication/transactional/publishing-stored-procedure-execution-in-transactional-replication.md). Non disponible lorsque la capture de données modifiées est activée.|  
|**proc schema only**|Procédure avec schéma uniquement. Non pris en charge pour les serveurs de publication Oracle.|  
|**serializable proc exec**|Réplique l'exécution de la procédure stockée uniquement si l'exécution se fait dans le contexte d'une transaction compatible avec la mise en série. Non pris en charge pour les serveurs de publication Oracle.<br /><br /> La procédure stockée doit également être exécutée à l'intérieur d'une transaction explicite pour que l'exécution de la procédure soit répliquée.|  
|**view schema only**|Vue avec schéma uniquement. Non pris en charge pour les serveurs de publication Oracle. Lorsque vous utilisez cette option, vous devez aussi publier la table de base.|  
  
`[ @filter = ] 'filter'` Procédure stockée (créée avec FOR Replication) utilisée pour filtrer la table horizontalement. *Filter* est de type **nvarchar (386)**, avec NULL comme valeur par défaut. [sp_articleview](../../relational-databases/system-stored-procedures/sp-articleview-transact-sql.md) et [sp_articlefilter](../../relational-databases/system-stored-procedures/sp-articlefilter-transact-sql.md) doivent être exécutés manuellement pour créer la vue et la procédure stockée de filtre. Si la valeur par défaut n'est pas NULL, la procédure de filtre n'est pas créée (cela suppose que la procédure stockée est créée manuellement).  
  
`[ @sync_object = ] 'sync_object'` Nom de la table ou de la vue utilisée pour produire le fichier de données utilisé pour représenter l’instantané de cet article. *sync_object* est de type **nvarchar (386)**, avec NULL comme valeur par défaut. Si la valeur est NULL, [sp_articleview](../../relational-databases/system-stored-procedures/sp-articleview-transact-sql.md) est appelé pour créer automatiquement la vue utilisée pour générer le fichier de sortie. Cela se produit après l’ajout de toute colonne avec [sp_articlecolumn](../../relational-databases/system-stored-procedures/sp-articlecolumn-transact-sql.md). Si la valeur par défaut n'est pas NULL, aucune vue n'est créée (cela suppose que la vue est créée manuellement).  
  
`[ @ins_cmd = ] 'ins_cmd'` Type de commande de réplication utilisé lors de la réplication d’insertions pour cet article. *ins_cmd* est de type **nvarchar (255)** et peut prendre l’une des valeurs suivantes.  
  
|Valeur|Description|  
|-----------|-----------------|  
|**NONE**|Aucune action n'est effectuée.|  
|**CALL sp_MSins_**<br /> **_table_**  (par défaut)<br /><br /> -ou-<br /><br /> **CALL custom_stored_procedure_name**|Appelle l'exécution d'une procédure stockée sur l'Abonné. Pour utiliser cette méthode de réplication, utilisez *schema_option* pour spécifier la création automatique de la procédure stockée, ou créez la procédure stockée spécifiée dans la base de données de destination de chaque abonné de l’article. *custom_stored_procedure* est le nom d’une procédure stockée créée par l’utilisateur. <strong>sp_Msins_ *table*</strong> contient le nom de la table de destination à la place de la partie *_table* du paramètre. Lorsque *destination_owner* est spécifié, il est ajouté au nom de la table de destination. Par exemple, pour la table **ProductCategory** appartenant au schéma de **production** sur l’abonné, le paramètre serait `CALL sp_MSins_ProductionProductCategory` . Pour un article d’une topologie de réplication d’égal à égal, *_table* est ajouté avec une valeur Guid. La spécification d' *custom_stored_procedure* n’est pas prise en charge pour la mise à jour des abonnés.|  
|**SQL** ou null|Réplique une instruction INSERT. Des valeurs sont fournies pour l'instruction INSERT pour l'ensemble des colonnes publiées dans l'article. La commande suivante est répliquée lors des insertions :<br /><br /> `INSERT INTO <table name> VALUES (c1value, c2value, c3value, ..., cnvalue)`|  
  
 Pour plus d’informations, consultez [Spécifier le mode de propagation des modifications des articles transactionnels](../../relational-databases/replication/transactional/transactional-articles-specify-how-changes-are-propagated.md).  
  
`[ @del_cmd = ] 'del_cmd'` Type de commande de réplication utilisé lors de la réplication des suppressions pour cet article. *del_cmd* est de type **nvarchar (255)** et peut prendre l’une des valeurs suivantes.  
  
|Valeur|Description|  
|-----------|-----------------|  
|**NONE**|Aucune action n'est effectuée.|  
|**CALLsp_MSdel_**<br /> **_table_**  (par défaut)<br /><br /> -ou-<br /><br /> **CALL custom_stored_procedure_name**|Appelle l'exécution d'une procédure stockée sur l'Abonné. Pour utiliser cette méthode de réplication, utilisez *schema_option* pour spécifier la création automatique de la procédure stockée, ou créez la procédure stockée spécifiée dans la base de données de destination de chaque abonné de l’article. *custom_stored_procedure* est le nom d’une procédure stockée créée par l’utilisateur. <strong>sp_Msdel_ *table*</strong> contient le nom de la table de destination à la place de la partie *_table* du paramètre. Lorsque *destination_owner* est spécifié, il est ajouté au nom de la table de destination. Par exemple, pour la table **ProductCategory** appartenant au schéma de **production** sur l’abonné, le paramètre serait `CALL sp_MSdel_ProductionProductCategory` . Pour un article d’une topologie de réplication d’égal à égal, *_table* est ajouté avec une valeur Guid. La spécification d' *custom_stored_procedure* n’est pas prise en charge pour la mise à jour des abonnés.|  
|**XCALL sp_MSdel_**<br /> **_Tableau_**<br /><br /> -ou-<br /><br /> **XCALL custom_stored_procedure_name**|Appelle une procédure stockée utilisant des paramètres de type XCALL. Pour utiliser cette méthode de réplication, utilisez *schema_option* pour spécifier la création automatique de la procédure stockée, ou créez la procédure stockée spécifiée dans la base de données de destination de chaque abonné de l’article. La spécification d'une procédure stockée créée par l'utilisateur n'est pas autorisée pour mettre à jour les Abonnés.|  
|**SQL** ou null|Réplique une instruction DELETE. Toutes les valeurs de colonnes clés primaire sont fournies pour l'instruction DELETE. La commande suivante est répliquée lors des suppressions :<br /><br /> `DELETE FROM <table name> WHERE pkc1 = pkc1value AND pkc2 = pkc2value AND pkcn = pkcnvalue`|  
  
 Pour plus d’informations, consultez [Spécifier le mode de propagation des modifications des articles transactionnels](../../relational-databases/replication/transactional/transactional-articles-specify-how-changes-are-propagated.md).  
  
`[ @upd_cmd = ] 'upd_cmd'` Type de commande de réplication utilisé lors de la réplication des mises à jour pour cet article. *upd_cmd* est de type **nvarchar (255)** et peut prendre l’une des valeurs suivantes.  
  
|Valeur|Description|  
|-----------|-----------------|  
|**NONE**|Aucune action n'est effectuée.|  
|**CALL sp_MSupd_**<br /> **_Tableau_**<br /><br /> -ou-<br /><br /> **CALL custom_stored_procedure_name**|Appelle l'exécution d'une procédure stockée sur l'Abonné. Pour utiliser cette méthode de réplication, utilisez *schema_option* pour spécifier la création automatique de la procédure stockée, ou créez la procédure stockée spécifiée dans la base de données de destination de chaque abonné de l’article.|  
|**MCALL sp_MSupd_**<br /> **_Tableau_**<br /><br /> -ou-<br /><br /> **MCALL custom_stored_procedure_name**|Appelle une procédure stockée utilisant des paramètres de type MCALL. Pour utiliser cette méthode de réplication, utilisez *schema_option* pour spécifier la création automatique de la procédure stockée, ou créez la procédure stockée spécifiée dans la base de données de destination de chaque abonné de l’article. *custom_stored_procedure* est le nom d’une procédure stockée créée par l’utilisateur. <strong>sp_Msupd_ *table*</strong> contient le nom de la table de destination à la place de la partie *_table* du paramètre. Lorsque *destination_owner* est spécifié, il est ajouté au nom de la table de destination. Par exemple, pour la table **ProductCategory** appartenant au schéma de **production** sur l’abonné, le paramètre serait `MCALL sp_MSupd_ProductionProductCategory` . Pour un article d’une topologie de réplication d’égal à égal, *_table* est ajouté avec une valeur Guid. La spécification d'une procédure stockée créée par l'utilisateur n'est pas autorisée pour mettre à jour les Abonnés.|  
|**SCALL sp_MSupd_**<br /> **_table_**  (par défaut)<br /><br /> -ou-<br /><br /> **SCALL custom_stored_procedure_name**|Appelle une procédure stockée utilisant des paramètres de type SCALL. Pour utiliser cette méthode de réplication, utilisez *schema_option* pour spécifier la création automatique de la procédure stockée, ou créez la procédure stockée spécifiée dans la base de données de destination de chaque abonné de l’article. *custom_stored_procedure* est le nom d’une procédure stockée créée par l’utilisateur. <strong>sp_Msupd_ *table*</strong> contient le nom de la table de destination à la place de la partie *_table* du paramètre. Lorsque *destination_owner* est spécifié, il est ajouté au nom de la table de destination. Par exemple, pour la table **ProductCategory** appartenant au schéma de **production** sur l’abonné, le paramètre serait `SCALL sp_MSupd_ProductionProductCategory` . Pour un article d’une topologie de réplication d’égal à égal, *_table* est ajouté avec une valeur Guid. La spécification d'une procédure stockée créée par l'utilisateur n'est pas autorisée pour mettre à jour les Abonnés.|  
|**XCALL sp_MSupd_**<br /> **_Tableau_**<br /><br /> -ou-<br /><br /> **XCALL custom_stored_procedure_name**|Appelle une procédure stockée utilisant des paramètres de type XCALL. Pour utiliser cette méthode de réplication, utilisez *schema_option* pour spécifier la création automatique de la procédure stockée, ou créez la procédure stockée spécifiée dans la base de données de destination de chaque abonné de l’article. La spécification d'une procédure stockée créée par l'utilisateur n'est pas autorisée pour mettre à jour les Abonnés.|  
|**SQL** ou null|Réplique une instruction UPDATE. L'instruction UPDATE est fournie pour toutes les valeurs des colonnes et toutes les valeurs de colonne clé primaire. La commande ci-dessous est répliquée lors des mises à jour :<br /><br /> `UPDATE <table name> SET c1 = c1value, SET c2 = c2value, SET cn = cnvalue WHERE pkc1 = pkc1value AND pkc2 = pkc2value AND pkcn = pkcnvalue`|  
  
> [!NOTE]  
>  Les syntaxes CALL, MCALL, SCALL et XCALL déterminent la quantité de données diffusée à l'abonné. La syntaxe CALL transmet toutes les valeurs de toutes les colonnes insérées et supprimées. La syntaxe SCALL transmet uniquement les valeurs des colonnes affectées. La syntaxe XCALL transmet les valeurs de toutes les colonnes, modifiées ou non, y compris leurs valeurs précédentes. Pour plus d’informations, consultez [Spécifier le mode de propagation des modifications des articles transactionnels](../../relational-databases/replication/transactional/transactional-articles-specify-how-changes-are-propagated.md).  
  
`[ @creation_script = ] 'creation_script'` Chemin d’accès et nom d’un script de schéma d’article facultatif utilisé pour créer l’article dans la base de données d’abonnement. *creation_script* est de type **nvarchar (255)**, avec NULL comme valeur par défaut.  
  
`[ @description = ] 'description'` Est une entrée descriptive de l’article. *Description* est de type **nvarchar (255)**, avec NULL comme valeur par défaut.  
  
`[ @pre_creation_cmd = ] 'pre_creation_cmd'` Spécifie ce que le système doit faire s’il détecte un objet existant portant le même nom sur l’abonné lors de l’application de l’instantané pour cet article. *pre_creation_cmd* est de type **nvarchar (10)** et peut prendre l’une des valeurs suivantes.  
  
|Valeur|Description|  
|-----------|-----------------|  
|**Aucune**|N'utilise pas de commande.|  
|**delete**|Supprime les données de la table de destination avant d'appliquer l'instantané. Lorsque l'article est filtré horizontalement, seules les données qui se trouvent dans les colonnes spécifiées par la clause filter sont supprimées. Non pris en charge pour les serveurs de publication Oracle lorsqu'un filtre horizontal est défini.|  
|**Drop** (valeur par défaut)|Supprime la table de destination.|  
|**truncate**|Tronque la table de destination. N'est pas valide pour les abonnés ODBC ou OLE DB.|  
  
`[ @filter_clause = ] 'filter_clause'` Clause de restriction (WHERE) qui définit un filtre horizontal. Quand vous entrez la clause de restriction, omettez le mot clé WHERE. *filter_clause* est de type **ntext**, avec NULL comme valeur par défaut. Pour plus d’informations, consultez [Filtrer des données publiées](../../relational-databases/replication/publish/filter-published-data.md).  
  
`[ @schema_option = ] schema_option` Masque de masque de l’option de génération de schéma pour l’article donné. *schema_option* est de **type binaire (8)** et peut être [| (Opérateur or au niveau du bit)](../../t-sql/language-elements/bitwise-or-transact-sql.md) produit d’une ou plusieurs des valeurs suivantes :  
  
> [!NOTE]  
>  Si la valeur est NULL, le système génère automatiquement une option de schéma valide pour l'article en fonction des autres propriétés de l'article. Le tableau des **options de schéma par défaut** indiqué dans les notes affiche la valeur qui sera choisie en fonction de la combinaison du type d’article et du type de réplication.  
  
|Valeur|Description|  
|-----------|-----------------|  
|**0x00**|Désactive les scripts par le Agent d’instantané et utilise *creation_script*.|  
|**0x01**|Génère le script de création d'objet (CREATE TABLE, CREATE PROCEDURE, etc.). Il s’agit de la valeur par défaut pour les Articles de procédure stockée.|  
|**0x02**|Génère les procédures stockées qui propagent les modifications pour l'article, si elles sont définies.|  
|**0x04**|Les colonnes d'identité font l'objet d'un script utilisant la propriété IDENTITY.|  
|**0x08**|Réplique les colonnes **timestamp** . Si la valeur n’est pas définie, les colonnes **timestamp** sont répliquées au format **binaire**.|  
|**0x10**|Génère un index cluster correspondant. Même si cette option n'est pas activée, les index relatifs aux clés primaires et aux contraintes uniques sont générés s'ils sont déjà définis sur une table publiée.|  
|**0x20**|Convertit les types de données définis par l'utilisateur (UDT) en types de données de base auprès de l'Abonné. Vous ne pouvez pas utiliser cette option lorsqu'il existe une contrainte CHECK ou DEFAULT sur une colonne de type défini par l'utilisateur (UDT), si une colonne UDT fait partie de la clé primaire, ou si une colonne calculée désigne une colonne UDT. *Non pris en charge pour les serveurs de publication Oracle*.|  
|**0x40**|Génère les index non-cluster correspondants. Même si cette option n'est pas activée, les index relatifs aux clés primaires et aux contraintes uniques sont générés s'ils sont déjà définis sur une table publiée.|  
|**0x80**|Réplique les contraintes de clé primaire. Tous les index associés à la contrainte sont également répliqués, même si les options **0x10** et **0x40** ne sont pas activées.|  
|**0x100**|Réplique les déclencheurs utilisateur, si ceux-ci sont définis, sur un article de table. *Non pris en charge pour les serveurs de publication Oracle*.|  
|**0x200**|Réplique les contraintes de clés étrangères. Si la table référencée ne fait pas partie d'une publication, aucune contrainte de clé étrangère appliquée à une table publiée n'est répliquée. *Non pris en charge pour les serveurs de publication Oracle*.|  
|**0x400**|Réplique les contraintes de vérification. *Non pris en charge pour les serveurs de publication Oracle*.|  
|**0x800**|Réplique les valeurs par défaut. *Non pris en charge pour les serveurs de publication Oracle*.|  
|**0x1000**|Réplique le classement au niveau des colonnes.<br /><br /> **Remarque :** Cette option doit être définie pour les serveurs de publication Oracle afin d’activer les comparaisons sensibles à la casse.|  
|**0x2000**|Réplique les propriétés étendues associées à l'objet source de l'article publié. *Non pris en charge pour les serveurs de publication Oracle*.|  
|**0x4000**|Réplique les contraintes UNIQUE. Tous les index associés à la contrainte sont également répliqués, même si les options **0x10** et **0x40** ne sont pas activées.|  
|**0x8000**|Cette option n'est pas valide pour les serveurs de publication [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)].|  
|**0x10000**|Réplique les contraintes CHECK en tant que NOT FOR REPLICATION afin que les contraintes ne soient pas appliquées durant la synchronisation.|  
|**0x20000**|Réplique les contraintes FOREIGN KEY en tant que NOT FOR REPLICATION afin que les contraintes ne soient pas appliquées durant la synchronisation.|  
|**0x40000**|Réplique les groupes de fichiers associés à une table ou un index partitionné.|  
|**0x80000**|Réplique le schéma de partition d'une table partitionnée.|  
|**0x100000**|Réplique le schéma de partition d'un index partitionné.|  
|**0x200000**|Réplique les statistiques d'une table.|  
|**0x400000**|Liaisons par défaut|  
|**0x800000**|Liaisons de règle|  
|**0x1000000**|Index de recherche en texte intégral|  
|**0x2000000**|Les collections de schémas XML liées aux colonnes **XML** ne sont pas répliquées.|  
|**0x4000000**|Réplique les index sur les colonnes **XML** .|  
|**0x8000000**|Crée tout schéma non encore présent chez l'abonné.|  
|**0x10000000**|Convertit les colonnes **XML** en **ntext** sur l’abonné.|  
|**0x20000000**|Convertit les types de données d’objet volumineux (**nvarchar (max)**, **varchar (max)** et **varbinary (max)**) introduits dans en [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] types de données pris en charge sur [!INCLUDE[ssVersion2000](../../includes/ssversion2000-md.md)] .|  
|**0x40000000**|Réplique les autorisations.|  
|**0x80000000**|Tente de supprimer les dépendances envers tous les objets qui ne font pas partie de la publication.|  
|**0x100000000**|Utilisez cette option pour répliquer l’attribut FILESTREAM s’il est spécifié sur des colonnes **varbinary (max)** . Ne spécifiez pas cette option si vous répliquez des tables sur des Abonnés [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)]. La réplication de tables qui possèdent des colonnes FILESTREAM sur [!INCLUDE[ssVersion2000](../../includes/ssversion2000-md.md)] des abonnés n’est pas prise en charge, quelle que soit la façon dont cette option de schéma est définie.<br /><br /> Consultez l’option associée **0x800000000**.|  
|**0x200000000**|Convertit les types de données de date et d’heure (**Date**, **Time**, **DateTimeOffset** et **datetime2**) introduits dans en [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] types de données pris en charge dans les versions antérieures de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .|  
|**0x400000000**|Réplique l'option de compression pour les données et les index. Pour plus d’informations, consultez [Compression de données](../../relational-databases/data-compression/data-compression.md).|  
|**0x800000000**|Définissez cette option pour stocker les données FILESTREAM dans leur propre groupe de fichiers sur l'Abonné. Si cette option n'est pas définie, les données FILESTREAM sont stockées dans le groupe de fichiers par défaut. La réplication ne crée pas de groupes de fichiers ; par conséquent, si vous définissez cette option, vous devez créer le groupe de fichiers avant d'appliquer l'instantané à l'Abonné. Pour plus d’informations sur la création d’objets avant l’application de l’instantané, consultez [exécuter des scripts avant et après l’application de l’instantané](../../relational-databases/replication/snapshot-options.md#execute-scripts-before-and-after-snapshot-is-applied).<br /><br /> Consultez l’option associée **0x100000000**.|  
|**0x1000000000**|Convertit les types définis par l’utilisateur (UDT) common language runtime (CLR) supérieurs à 8000 octets en **varbinary (max)** afin que les colonnes de type UDT puissent être répliquées sur les abonnés qui exécutent [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] .|  
|**0x2000000000**|Convertit le type de données **hierarchyid** en **varbinary (max)** afin que les colonnes de type **hierarchyid** puissent être répliquées sur les abonnés qui exécutent [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] . Pour plus d’informations sur l’utilisation des colonnes **hierarchyid** dans les tables répliquées, consultez [hierarchyid &#40;Transact-SQL&#41;](../../t-sql/data-types/hierarchyid-data-type-method-reference.md).|  
|**0x4000000000**|Réplique tous les index filtrés sur la table. Pour plus d’informations sur les index filtrés, consultez [créer des index filtrés](../../relational-databases/indexes/create-filtered-indexes.md).|  
|**0x8000000000**|Convertit les types de données **Geography** et **Geometry** en **varbinary (max)** afin que les colonnes de ces types puissent être répliquées sur les abonnés qui exécutent [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] .|  
|**0x10000000000**|Réplique les index sur les colonnes de type **Geography** et **Geometry**.|  
|**0x20000000000**|Réplique l'attribut SPARSE pour les colonnes. Pour plus d’informations sur cet attribut, consultez [utiliser des colonnes éparses](../../relational-databases/tables/use-sparse-columns.md).|  
|**0x40000000000**|Activez l’écriture de scripts par l’agent d’instantané pour créer une table optimisée en mémoire sur l’abonné.|  
|**0x80000000000**|Convertit l’index cluster en index non-cluster pour les articles à mémoire optimisée.|  
|**0x400000000000**|Réplique tous les index ColumnStore non cluster sur la ou les tables|  
|**0x800000000000**|Réplique tous les index ColumnStore non cluster flitered sur la ou les tables.|  
|NULL|La réplication définit automatiquement *schema_option* sur une valeur par défaut, dont la valeur dépend d’autres propriétés de l’article. Le tableau « Options de schéma par défaut » de la section Notes montre les options de schéma par défaut en fonction du type d'article et du type de réplication.<br /><br /> La valeur par défaut pour les publications non- [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] est **0x050D3**.|  
  
 Toutes les valeurs de *schema_option* ne sont pas valides pour chaque type de réplication et de type d’article. Le tableau des **options de schéma valides** de la section Notes montre les options de schéma valides qui peuvent être choisies en fonction de la combinaison du type d’article et du type de réplication.  
  
`[ @destination_owner = ] 'destination_owner'` Nom du propriétaire de l’objet de destination. *destination_owner* est de **type sysname**, avec NULL comme valeur par défaut. Lorsque *destination_owner* n’est pas spécifié, le propriétaire est spécifié automatiquement en fonction des règles suivantes :  
  
|Condition|Propriétaire de l'objet de destination|  
|---------------|------------------------------|  
|La publication utilise la copie en bloc en mode natif pour générer l'instantané initial, qui prend uniquement en charge des Abonnés [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|La valeur par défaut est *source_owner*.|  
|Publié à partir d'un serveur de publication non [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|Devient par défaut propriétaire de la base de données de destination.|  
|La publication utilise la copie en bloc en mode caractère pour générer l'instantané initial, qui prend en charge des Abonnés non [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|Non assigné.|  
  
 Pour prendre en charge les abonnés non- [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , *destination_owner* doit avoir la valeur null.  
  
`[ @status = ] status` Spécifie si l’article est actif et des options supplémentaires pour la façon dont les modifications sont propagées. l' *État* est **tinyint** et peut être [| (Opérateur or au niveau du bit)](../../t-sql/language-elements/bitwise-or-transact-sql.md) produit d’une ou plusieurs de ces valeurs.  
  
|Valeur|Description|  
|-----------|-----------------|  
|**1**|Article actif|  
|**8**|Inclut le nom de colonne dans les instructions INSERT.|  
|**16** (par défaut)|Utilise des instructions paramétrables.|  
|**24**|Inclut le nom de colonne dans les instructions INSERT et utilise des instructions paramétrables.|  
|**64**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
  
 Par exemple, un article actif utilisant des instructions paramétrables posséderait la valeur 17 dans cette colonne. La valeur **0** signifie que l’article est inactif et qu’aucune propriété supplémentaire n’est définie.  
  
`[ @source_owner = ] 'source_owner'` Propriétaire de l’objet source. *source_owner* est de **type sysname**, avec NULL comme valeur par défaut. *source_owner* doit être spécifié pour les serveurs de publication Oracle.  
  
`[ @sync_object_owner = ] 'sync_object_owner'` Propriétaire de la vue qui définit l’article publié. *sync_object_owner* est de **type sysname**, avec NULL comme valeur par défaut.  
  
`[ @filter_owner = ] 'filter_owner'` Propriétaire du filtre. *filter_owner* est de **type sysname**, avec NULL comme valeur par défaut.  
  
`[ @source_object = ] 'source_object'` Objet de base de données à publier. *source_object* est de **type sysname**, avec NULL comme valeur par défaut. Si *source_table* a la valeur null, *source_object* ne peut pas être null. *source_object* doit être utilisé à la place de *source_table*. Pour plus d’informations sur les types d’objets qui peuvent être publiés à l’aide de la réplication transactionnelle ou d’instantané, consultez [publier des données et des objets de base de données](../../relational-databases/replication/publish/publish-data-and-database-objects.md).  
  
`[ @artid = ] _article_ID_ OUTPUT` ID d’article du nouvel article. *article_id* est de **type int** avec NULL comme valeur par défaut et il s’agit d’un paramètre OUTPUT.  
  
`[ @auto_identity_range = ] 'auto_identity_range'` Active et désactive la gestion automatique des plages d’identité sur une publication au moment de sa création. *auto_identity_range* est de type **nvarchar (5)** et peut prendre l’une des valeurs suivantes :  
  
|Valeur|Description|  
|-----------|-----------------|  
|**true**|Active la gestion automatique des plages d'identité|  
|**false**|Désactive la gestion automatique des plages d'identité|  
|NULL (valeur par défaut)|La gestion des plages d’identité est définie par *identityrangemanagementoption*.|  
  
> [!NOTE]  
>  *auto_identity_range* est déconseillé et n’est fourni qu’à des fins de compatibilité descendante. Vous devez utiliser *identityrangemanagementoption* pour spécifier les options de gestion des plages d’identité. Pour plus d’informations, consultez [ Répliquer des colonnes d’identité](../../relational-databases/replication/publish/replicate-identity-columns.md).  
  
`[ @pub_identity_range = ] pub_identity_range` Contrôle la taille de la plage sur le serveur de publication si l’article a *identityrangemanagementoption* défini sur **auto** ou *auto_identity_range* défini sur **true**. *pub_identity_range* est de type **bigint**, avec NULL comme valeur par défaut. *Non pris en charge pour les serveurs de publication Oracle*.  
  
`[ @identity_range = ] identity_range` Contrôle la taille de la plage sur l’abonné si l’article a *identityrangemanagementoption* défini sur **auto** ou *auto_identity_range* défini sur **true**. *identity_range* est de type **bigint**, avec NULL comme valeur par défaut. Utilisé lorsque *auto_identity_range* a la valeur **true**. *Non pris en charge pour les serveurs de publication Oracle*.  
  
`[ @threshold = ] threshold` Valeur de pourcentage qui contrôle le moment où l’Agent de distribution affecte une nouvelle plage d’identité. Lorsque le pourcentage de valeurs spécifié dans *seuil* est utilisé, la agent de distribution crée une nouvelle plage d’identité. *Threshold* est de type **bigint**, avec NULL comme valeur par défaut. Utilisé lorsque *identityrangemanagementoption* a la valeur **auto** ou *auto_identity_range* a la valeur **true**. *Non pris en charge pour les serveurs de publication Oracle*.  
  
`[ @force_invalidate_snapshot = ] force_invalidate_snapshot` Confirme que l’action entreprise par cette procédure stockée peut invalider un instantané existant. *force_invalidate_snapshot* est un **bit**, avec 0 comme valeur par défaut.  
  
 **0** indique que l’ajout d’un article n’entraîne pas la non-validité de l’instantané. Si la procédure stockée détecte que la modification requiert un nouvel instantané, un message d'erreur est généré et aucune modification n'est effectuée.  
  
 **1** indique que l’ajout d’un article peut entraîner la non-validité de l’instantané. si des abonnements nécessitant un nouvel instantané sont nécessaires, accorde l’autorisation de marquer l’instantané existant comme obsolète et de générer un nouvel instantané.  
  
`[ @use_default_datatypes = ] use_default_datatypes` Indique si les mappages de type de données de colonne par défaut sont utilisés lors de la publication d’un article à partir d’un serveur de publication Oracle. *use_default_datatypes* est de bits, avec 1 comme valeur par défaut.  
  
 **1** = les mappages de colonne d’article par défaut sont utilisés. Les mappages de type de données par défaut peuvent être affichés en exécutant [sp_getdefaultdatatypemapping](../../relational-databases/system-stored-procedures/sp-getdefaultdatatypemapping-transact-sql.md).  
  
 **0** = les mappages de colonne d’article personnalisés sont définis, et par conséquent [sp_articleview](../../relational-databases/system-stored-procedures/sp-articleview-transact-sql.md) n’est pas appelé par **sp_addarticle**.  
  
 Lorsque *use_default_datatypes* a la valeur **0**, vous devez exécuter [sp_changearticlecolumndatatype](../../relational-databases/system-stored-procedures/sp-changearticlecolumndatatype-transact-sql.md) une fois pour chaque mappage de colonne modifié par rapport à la valeur par défaut. Une fois que tous les mappages de colonnes personnalisées ont été définis, vous devez exécuter [sp_articleview](../../relational-databases/system-stored-procedures/sp-articleview-transact-sql.md).  
  
> [!NOTE]  
>  Ce paramètre ne doit être utilisé que pour les serveurs de publication Oracle. L’affectation de la valeur **0** à *use_default_datatypes* pour un serveur de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] publication génère une erreur.  
  
`[ @identityrangemanagementoption = ] identityrangemanagementoption` Spécifie comment la gestion des plages d’identité est gérée pour l’article. *identityrangemanagementoption* est de type **nvarchar (10)** et peut prendre l’une des valeurs suivantes.  
  
|Valeur|Description|  
|-----------|-----------------|  
|**Aucune**|La réplication n'explicite pas la gestion des plages d'identité. Cette option est recommandée uniquement pour la compatibilité ascendante avec des versions antérieures de SQL Server. Non autorisé pour la réplication d'homologue.|  
|**Manuelle**|Marque la colonne d'identité en utilisant NOT FOR REPLICATION pour activer la gestion manuelle des plages d'identité.|  
|**Auto**|Spécifie la gestion automatique des plages d'identité.|  
|NULL (valeur par défaut)|La valeur par défaut est **None** lorsque la valeur de *auto_identity_range* n’est pas **true**. La valeur par défaut est **Manual** dans une topologie d’égal à égal par défaut (*auto_identity_range* est ignoré).|  
  
 Pour la compatibilité descendante, lorsque la valeur de *identityrangemanagementoption* est null, la valeur de *auto_identity_range* est vérifiée. Toutefois, lorsque la valeur de *identityrangemanagementoption* n’est pas null, la valeur de *auto_identity_range* est ignorée.  
  
 Pour plus d’informations, consultez [ Répliquer des colonnes d’identité](../../relational-databases/replication/publish/replicate-identity-columns.md).  
  
`[ @publisher = ] 'publisher'` Spécifie un serveur de publication non- [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . *Publisher* est de **type sysname**, avec NULL comme valeur par défaut.  
  
> [!NOTE]  
>  l' *éditeur* ne doit pas être utilisé lors de l’ajout d’un article à un serveur de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] publication.  
  
`[ @fire_triggers_on_snapshot = ] 'fire_triggers_on_snapshot'` Indique si les déclencheurs utilisateur répliqués sont exécutés lorsque l’instantané initial est appliqué. *fire_triggers_on_snapshot* est de type **nvarchar (5)**, avec false comme valeur par défaut. **true** signifie que les déclencheurs utilisateur sur une table répliquée sont exécutés lorsque l’instantané est appliqué. Pour que les déclencheurs soient répliqués, la valeur de masque de réplication de *schema_option* doit inclure la valeur **0x100**.  
  
## <a name="return-code-values"></a>Codet de retour  
 **0** (succès) ou **1** (échec)  
  
## <a name="remarks"></a>Notes  
 **sp_addarticle** est utilisé dans la réplication d’instantané ou dans la réplication transactionnelle.  
  
 Par défaut, la réplication ne publie aucune colonne dans la table source lorsque le type de données de colonne n'est pas pris en charge par la réplication. Si vous devez publier une telle colonne, vous devez exécuter [sp_articlecolumn](../../relational-databases/system-stored-procedures/sp-articlecolumn-transact-sql.md) pour ajouter la colonne.  
  
 Lors de l'ajout d'un article à une publication qui prend en charge la réplication transactionnelle d'homologue à homologue, les restrictions suivantes s'appliquent :  
  
-   Les instructions paramétrables doivent être spécifiées pour tous les articles logbased. Vous devez inclure **16** dans la valeur d' *État* .  
  
-   Le nom et le propriétaire de la table de destination doivent correspondre à la table source.  
  
-   Il est impossible de filtrer l'article horizontalement ou verticalement.  
  
-   La gestion automatique des plages d'identité n'est pas prise en charge. Vous devez spécifier la valeur Manual pour *identityrangemanagementoption*.  
  
-   S’il existe une colonne **timestamp** dans la table, vous devez inclure 0x08 dans *schema_option* pour répliquer la colonne comme **timestamp**.  
  
-   La valeur **SQL** ne peut pas être spécifiée pour *ins_cmd*, *upd_cmd* et *del_cmd*.  
  
 Pour plus d'informations, consultez [Peer-to-Peer Transactional Replication](../../relational-databases/replication/transactional/peer-to-peer-transactional-replication.md).  
  
 Lorsque vous publiez des objets, leurs définitions sont copiées sur les abonnés. Si vous publiez un objet de base de données qui dépend d'un ou de plusieurs autres objets, vous devez publier tous les objets référencés. Par exemple, si vous publiez une vue qui dépend d'une table, vous devez publier la table également.  
  
 Si *vertical_partition* a la valeur **true**, **sp_addarticle** diffère la création de la vue jusqu’à ce que [sp_articleview](../../relational-databases/system-stored-procedures/sp-articleview-transact-sql.md) soit appelé (après l’ajout de la dernière [sp_articlecolumn](../../relational-databases/system-stored-procedures/sp-articlecolumn-transact-sql.md) ).  
  
 Si la publication autorise la mise à jour des abonnements et que la table publiée n’a pas de colonne **uniqueidentifier** , **sp_addarticle** ajoute automatiquement une colonne **uniqueidentifier** à la table.  
  
 Lors de la réplication vers un abonné qui n’est pas une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (réplication hétérogène), seules les [!INCLUDE[tsql](../../includes/tsql-md.md)] instructions sont prises en charge pour les commandes **Insert**, **Update** et **Delete** .  
  
 Lorsque l'Agent de lecture du journal s'exécute, l'ajout d'un article à une publication d'égal à égal peut provoquer un interblocage entre l'Agent de lecture du journal et le processus qui ajoute l'article. Pour éviter ce problème, avant d'ajouter un article à une publication d'égal à égal, utilisez le moniteur de réplication afin d'arrêter l'Agent de lecture du journal sur le nœud où vous ajoutez l'article. Redémarrez l'Agent de lecture du journal après l'ajout de l'article.  
  
 Lorsque vous définissez `@del_cmd = 'NONE'` ou `@ins_cmd = 'NONE'` , la propagation des commandes de **mise à jour** peut également être affectée en n’envoyant pas ces commandes lorsqu’une mise à jour limitée se produit. (Une mise à jour associée est un type d'instruction UPDATE du serveur de publication qui réplique comme une paire de DELETE/INSERT sur l'abonné.)  
  
## <a name="default-schema-options"></a>Options de schéma par défaut  
 Ce tableau décrit la valeur par défaut définie par la réplication si *schema_options* n’est pas spécifié par l’utilisateur, où cette valeur dépend du type de réplication (indiqué en haut) et du type d’article (dans la première colonne).  
  
|Type de l'article|Réplication transactionnelle|Réplication d'instantané|  
|------------------|----------------------|------|  
|**aggregate schema only**|**0x01**|**0x01**|  
|**func schema only**|**0x01**|**0x01**|  
|**indexed view schema only**|**0x01**|**0x01**|  
|**indexed view logbased**|**0x30F3**|**0x3071**|  
|**indexed view logbase manualboth**|**0x30F3**|**0x3071**|  
|**indexed view logbased manualfilter**|**0x30F3**|**0x3071**|  
|**indexed view logbased manualview**|**0x30F3**|**0x3071**|  
|**logbased**|**0x30F3**|**0x3071**|  
|**logbased manualfilter**|**0x30F3**|**0x3071**|  
|**logbased manualview**|**0x30F3**|**0x3071**|  
|**proc exec**|**0x01**|**0x01**|  
|**proc schema only**|**0x01**|**0x01**|  
|**serializable proc exec**|**0x01**|**0x01**|  
|**view schema only**|**0x01**|**0x01**|  
  
> [!NOTE]
>  Si une publication est activée pour la mise à jour en file d’attente, une valeur *schema_option* de **0x80** est ajoutée à la valeur par défaut indiquée dans le tableau. La *schema_option* par défaut pour une publication non- [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] est **0x050D3**.  
  
## <a name="valid-schema-options"></a>Options de schéma valides  
 Ce tableau décrit les valeurs autorisées de *schema_option* en fonction du type de réplication (indiqué dans la partie supérieure) et du type d’article (indiqué dans la première colonne).  
  
|Type de l'article|Réplication transactionnelle|Réplication d'instantané|  
|------------------|----------------------|------|  
|**logbased**|Toutes les options|Toutes les options, sauf **0x02**|  
|**logbased manualfilter**|Toutes les options|Toutes les options, sauf **0x02**|  
|**logbased manualview**|Toutes les options|Toutes les options, sauf **0x02**|  
|**indexed view logbased**|Toutes les options|Toutes les options, sauf **0x02**|  
|**indexed view logbased manualfilter**|Toutes les options|Toutes les options, sauf **0x02**|  
|**indexed view logbased manualview**|Toutes les options|Toutes les options, sauf **0x02**|  
|**indexed view logbase manualboth**|Toutes les options|Toutes les options, sauf **0x02**|  
|**proc exec**|**0x01**, **0x20**, **0x2000**, **0x400000**, **0x800000**, **0x2000000**, **0x8000000**, **0x10000000**, **0x20000000**, **0x40000000** et **0x80000000**|**0x01**, **0x20**, **0x2000**, **0x400000**, **0x800000**, **0x2000000**, **0x8000000**, **0x10000000**, **0x20000000**, **0x40000000** et **0x80000000**|  
|**serializable proc exec**|**0x01**, **0x20**, **0x2000**, **0x400000**, **0x800000**, **0x2000000**, **0x8000000**, **0x10000000**, **0x20000000**, **0x40000000** et **0x80000000**|**0x01**, **0x20**, **0x2000**, **0x400000**, **0x800000**, **0x2000000**, **0x8000000**, **0x10000000**, **0x20000000**, **0x40000000** et **0x80000000**|  
|**proc schema only**|**0x01**, **0x20**, **0x2000**, **0x400000**, **0x800000**, **0x2000000**, **0x8000000**, **0x10000000**, **0x20000000**, **0x40000000** et **0x80000000**|**0x01**, **0x20**, **0x2000**, **0x400000**, **0x800000**, **0x2000000**, **0x8000000**, **0x10000000**, **0x20000000**, **0x40000000** et **0x80000000**|  
|**view schema only**|**0x01**, **0x010**, **0x020**, **0x040**, **0x0100**, **0x2000**, **0x40000**, , **0x200000**, **0x400000**, **0x800000**, **0x2000000**, **0x8000000**, **0x40000000** et **0x80000000**|**0x01**, **0x010**, **0x020**, **0x040**, **0x0100**, **0x2000**, **0x40000**, , **0x200000**, **0x400000**, **0x800000**, **0x2000000**, **0x8000000**, **0x40000000** et **0x80000000**|  
|**func schema only**|**0x01**, **0x20**, **0x2000**, **0x400000**, **0x800000**, **0x2000000**, **0x8000000**, **0x10000000**, **0x20000000**, **0x40000000** et **0x80000000**|**0x01**, **0x20**, **0x2000**, **0x400000**, **0x800000**, **0x2000000**, **0x8000000**, **0x10000000**, **0x20000000**, **0x40000000** et **0x80000000**|  
|**indexed view schema only**|**0x01**, **0x010**, **0x020**, **0x040**, **0x0100**, **0x2000**, **0x40000**, , **0x200000**, **0x400000**, **0x800000**, **0x2000000**, **0x8000000**, **0x40000000** et **0x80000000**|**0x01**, **0x010**, **0x020**, **0x040**, **0x0100**, **0x2000**, **0x40000**, , **0x200000**, **0x400000**, **0x800000**, **0x2000000**, **0x8000000**, **0x40000000** et **0x80000000**|  
  
> [!NOTE]
>  Pour les publications de mise à jour en attente, les valeurs *schema_option* de **0x8000** et **0x80** doivent être activées. Les valeurs de *schema_option* prises en charge pour les [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] publications non sont : **0x01**, **0x02**, **0x10**, **0x40**, **0x80**, **0x1000**, **0x4000** et **0x8000**.  
  
## <a name="example"></a>Exemple  
 [!code-sql[HowTo#sp_AddTranArticle](../../relational-databases/replication/codesnippet/tsql/sp-addarticle-transact-sql_1.sql)]  
  
## <a name="permissions"></a>Autorisations  
 Seuls les membres du rôle serveur fixe **sysadmin** ou du rôle de base de données fixe **db_owner** peuvent exécuter **sp_addarticle**.  
  
## <a name="see-also"></a>Voir aussi  
 [Define an Article](../../relational-databases/replication/publish/define-an-article.md)   
 [sp_articlecolumn &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-articlecolumn-transact-sql.md)   
 [sp_articlefilter &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-articlefilter-transact-sql.md)   
 [sp_articleview &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-articleview-transact-sql.md)   
 [sp_changearticle &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-changearticle-transact-sql.md)   
 [sp_droparticle &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-droparticle-transact-sql.md)   
 [sp_helparticle &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helparticle-transact-sql.md)   
 [sp_helparticlecolumns &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helparticlecolumns-transact-sql.md)   
 [Procédures stockées de réplication &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/replication-stored-procedures-transact-sql.md)   
 [Publier des données et des objets de base de données](../../relational-databases/replication/publish/publish-data-and-database-objects.md)  
  
  
