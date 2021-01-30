---
description: sp_changearticle (Transact-SQL)
title: sp_changearticle (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 10/28/2015
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_changearticle
- sp_changearticle_TSQL
helpviewer_keywords:
- sp_changearticle
ms.assetid: 24c33ca5-f03a-4417-a267-131ca5ba6bb5
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 22d64091d206161546856e7186958ab60217bcc2
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99203738"
---
# <a name="sp_changearticle-transact-sql"></a>sp_changearticle (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  Modifie les propriétés d'un article dans une publication transactionnelle ou d'instantané. Cette procédure stockée est exécutée sur le serveur de publication dans la base de données de publication.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_changearticle [ [@publication= ] 'publication' ]  
    [ , [ @article= ] 'article' ]  
    [ , [ @property= ] 'property' ]  
    [ , [ @value= ] 'value' ]  
    [ , [ @force_invalidate_snapshot = ] force_invalidate_snapshot ]  
    [ , [ @force_reinit_subscription = ] force_reinit_subscription ]  
    [ , [ @publisher = ] 'publisher' ]  
```  
  
## <a name="arguments"></a>Arguments  
`[ @publication = ] 'publication'` Nom de la publication qui contient l’article. *publication* est de **type sysname**, avec NULL comme valeur par défaut.  
  
`[ @article = ] 'article'` Nom de l’article dont la propriété doit être modifiée. *article* est de **type sysname**, avec NULL comme valeur par défaut.  
  
`[ @property = ] 'property'` Est une propriété d’article à modifier. la *propriété* est **de type nvarchar (100)**.  
  
`[ @value = ] 'value'` Nouvelle valeur de la propriété de l’article. la *valeur* est **de type nvarchar (255)**.  
  
 Le tableau ci-dessous décrit les propriétés des articles et les valeurs de ces propriétés.  
  
|Propriété|Valeurs|Description|  
|--------------|------------|-----------------|  
|**creation_script**||Chemin d'accès et nom d'un script de schéma d'article utilisé pour créer des tables cibles. La valeur par défaut est NULL.|  
|**del_cmd**||Instruction DELETE à exécuter ; à défaut, elle sera élaborée à partir du journal.|  
|**description**||Nouvelle entrée descriptive de l'article.|  
|**dest_object**||Fourni pour la compatibilité ascendante. Utilisez **dest_table**.|  
|**dest_table**||Nouvelle table de destination.|  
|**destination_owner**||Nom du propriétaire de l’objet de destination.|  
|**filter**||Nouvelle procédure stockée à utiliser pour filtrer la table (filtrage horizontal). La valeur par défaut est NULL. Ne peut pas être modifié pour les publications dans la réplication d'égal à égal.|  
|**fire_triggers_on_snapshot**|**true**|Les déclencheurs de l'utilisateur répliqués sont exécutés lorsque l'instantané initial est appliqué.<br /><br /> Remarque : pour que les déclencheurs soient répliqués, la valeur de masque de réplication de *schema_option* doit inclure la valeur **0x100**.|  
||**false**|Les déclencheurs de l'utilisateur répliqués ne sont pas exécutés lorsque l'instantané initial est appliqué.|  
|**identity_range**||Contrôle la taille des plages d'identité affectées à l'abonné. Non pris en charge pour la réplication d'égal à égal.|  
|**ins_cmd**||Instruction INSERT à exécuter ; à défaut, elle sera élaborée à partir du journal.|  
|**pre_creation_cmd**||Définit une commande de précréation pouvant supprimer, effacer ou tronquer la table de destination avant l'application de la synchronisation.|  
||**Aucune**|N'utilise pas de commande.|  
||**drop**|Supprime la table de destination.|  
||**delete**|Détruit la table de destination.|  
||**truncate**|Tronque la table de destination.|  
|**pub_identity_range**||Contrôle la taille des plages d'identité affectées à l'abonné. Non pris en charge pour la réplication d'égal à égal.|  
|**schema_option**||Spécifie le bitmap de l'option de génération de schéma pour l'article considéré. *schema_option* est **de type binaire (8)**. Pour plus d'informations, consultez la section « Notes » plus loin dans cette rubrique.|  
||**0x00**|Désactive les scripts de l'Agent d'instantané.|  
||**0x01**|Génère la création d'objets (CREATE TABLE, CREATE PROCEDURE, etc.).|  
||**0x02**|Génère les procédures stockées qui propagent les modifications pour l'article, si elles sont définies.|  
||**0x04**|Les colonnes d'identité font l'objet d'un script utilisant la propriété IDENTITY.|  
||**0x08**|Réplique les colonnes **timestamp** . Si la valeur n’est pas définie, les colonnes **timestamp** sont répliquées au format **binaire**.|  
||**0x10**|Génère un index cluster correspondant.|  
||**0x20**|Convertit les types de données définis par l'utilisateur (UDT) en types de données de base auprès de l'Abonné. Vous ne pouvez pas utiliser cette option lorsqu'il existe une contrainte CHECK ou DEFAULT sur une colonne de type défini par l'utilisateur (UDT), si une colonne UDT fait partie de la clé primaire, ou si une colonne calculée désigne une colonne UDT. Non pris en charge pour les serveurs de publication Oracle.|  
||**0x40**|Génère les index non-cluster correspondants.|  
||**0x80**|Inclut l'intégrité référentielle déclarée dans les clés primaires.|  
||**0x100**|Réplique les déclencheurs utilisateur, si ceux-ci sont définis, sur un article de table.|  
||**0x200**|Réplique les contraintes FOREIGN KEY. Si la table référencée ne fait pas partie d'une publication, aucune contrainte FOREIGN KEY appliquée à une table publiée n'est répliquée.|  
||**0x400**|Réplique les contraintes CHECK.|  
||**0x800**|Réplique les valeurs par défaut.|  
||**0x1000**|Réplique le classement au niveau des colonnes.|  
||**0x2000**|Réplique les propriétés étendues associées à l'objet source de l'article publié.|  
||**0x4000**|Réplique les clés uniques, si celles-ci sont définies, sur un article de table.|  
||**0x8000**|Réplique la clé primaire et les clés uniques sur un article de table sous forme de contraintes, à l'aide d'instructions ALTER TABLE.<br /><br /> Remarque : cette option est dépréciée. Utilisez à la place **0x80** et **0x4000** .|  
||**0x10000**|Réplique les contraintes CHECK en tant que NOT FOR REPLICATION afin que les contraintes ne soient pas appliquées durant la synchronisation.|  
||**0x20000**|Réplique les contraintes FOREIGN KEY en tant que NOT FOR REPLICATION afin que les contraintes ne soient pas appliquées durant la synchronisation.|  
||**0x40000**|Réplique les groupes de fichiers associés à une table ou un index partitionné.|  
||**0x80000**|Réplique le schéma de partition d'une table partitionnée.|  
||**0x100000**|Réplique le schéma de partition d'un index partitionné.|  
||**0x200000**|Réplique les statistiques d'une table.|  
||**0x400000**|Liaisons par défaut|  
||**0x800000**|Liaisons de règle|  
||**0x1000000**|Index de recherche en texte intégral|  
||**0x2000000**|Les collections de schémas XML liées aux colonnes **XML** ne sont pas répliquées.|  
||**0x4000000**|Réplique les index sur les colonnes **XML** .|  
||**0x8000000**|Crée tout schéma non encore présent chez l'abonné.|  
||**0x10000000**|Convertit les colonnes **XML** en **ntext** sur l’abonné.|  
||**0x20000000**|Convertit les types de données d’objet volumineux (**nvarchar (max)**, **varchar (max)** et **varbinary (max)**) introduits dans en [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] types de données pris en charge sur [!INCLUDE[ssVersion2000](../../includes/ssversion2000-md.md)] .|  
||**0x40000000**|Réplique les autorisations.|  
||**0x80000000**|Tente de supprimer les dépendances envers tous les objets qui ne font pas partie de la publication.|  
||**0x100000000**|Utilisez cette option pour répliquer l’attribut FILESTREAM s’il est spécifié sur des colonnes **varbinary (max)** . Ne spécifiez pas cette option si vous répliquez des tables sur des Abonnés [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)]. La réplication de tables qui possèdent des colonnes FILESTREAM sur [!INCLUDE[ssVersion2000](../../includes/ssversion2000-md.md)] des abonnés n’est pas prise en charge, quelle que soit la façon dont cette option de schéma est définie.<br /><br /> Consultez l’option associée **0x800000000**.|  
||**0x200000000**|Convertit les types de données de date et d’heure (**Date**, **Time**, **DateTimeOffset** et **datetime2**) introduits dans en [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] types de données pris en charge dans les versions antérieures de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .|  
||**0x400000000**|Réplique l'option de compression pour les données et les index. Pour plus d’informations, consultez [Compression de données](../../relational-databases/data-compression/data-compression.md).|  
||**0x800000000**|Définissez cette option pour stocker les données FILESTREAM dans leur propre groupe de fichiers sur l'Abonné. Si cette option n'est pas définie, les données FILESTREAM sont stockées dans le groupe de fichiers par défaut. La réplication ne crée pas de groupes de fichiers ; par conséquent, si vous définissez cette option, vous devez créer le groupe de fichiers avant d'appliquer l'instantané à l'Abonné. Pour plus d’informations sur la création d’objets avant l’application de l’instantané, consultez [exécuter des scripts avant et après l’application de l’instantané](../../relational-databases/replication/snapshot-options.md#execute-scripts-before-and-after-snapshot-is-applied).<br /><br /> Consultez l’option associée **0x100000000**.|  
||**0x1000000000**|Convertit les types définis par l’utilisateur (UDT) common language runtime (CLR) supérieurs à 8000 octets en **varbinary (max)** afin que les colonnes de type UDT puissent être répliquées sur les abonnés qui exécutent [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] .|  
||**0x2000000000**|Convertit le type de données **hierarchyid** en **varbinary (max)** afin que les colonnes de type **hierarchyid** puissent être répliquées sur les abonnés qui exécutent [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] . Pour plus d’informations sur l’utilisation des colonnes **hierarchyid** dans les tables répliquées, consultez [hierarchyid &#40;Transact-SQL&#41;](../../t-sql/data-types/hierarchyid-data-type-method-reference.md).|  
||**0x4000000000**|Réplique tous les index filtrés sur la table. Pour plus d’informations sur les index filtrés, consultez [créer des index filtrés](../../relational-databases/indexes/create-filtered-indexes.md).|  
||**0x8000000000**|Convertit les types de données **Geography** et **Geometry** en **varbinary (max)** afin que les colonnes de ces types puissent être répliquées sur les abonnés qui exécutent [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] .|  
||**0x10000000000**|Réplique les index sur les colonnes de type **Geography** et **Geometry**.|  
||**0x20000000000**|Réplique l'attribut SPARSE pour les colonnes. Pour plus d’informations sur cet attribut, consultez [utiliser des colonnes éparses](../../relational-databases/tables/use-sparse-columns.md).|  
||**0x40000000000**|Activez l’écriture de scripts par l’agent d’instantané pour créer une table optimisée en mémoire sur l’abonné.|  
||**0x80000000000**|Convertit l’index cluster en index non-cluster pour les articles à mémoire optimisée.|  
|**statut**||Spécifie le nouvel état de la propriété.|  
||**dts horizontal partitions**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
||**include column names**|Les noms de colonnes sont inclus dans l'instruction INSERT répliquée.|  
||**no column names**|Les noms de colonnes ne sont pas inclus dans l'instruction INSERT répliquée.|  
||**no dts horizontal partitions**|La partition horizontale pour l'article n'est pas définie par un abonnement transformable.|  
||**Aucune**|Efface toutes les options d’état de la table [sysarticles](../../relational-databases/system-tables/sysarticles-transact-sql.md) et marque l’article comme étant inactif.|  
||**parameters**|Les modifications sont propagées vers l'abonné à l'aide de commandes paramétrables. Il s'agit du paramètre par défaut pour un nouvel article.|  
||**littéraux de chaîne**|Les modifications sont propagées vers l'abonné à l'aide de valeurs littérales de chaîne.|  
|**sync_object**||Nom de la table ou de la vue utilisée pour produire un fichier de sortie de synchronisation. La valeur par défaut est NULL. Non pris en charge pour les serveurs de publication Oracle.|  
|**espace disque logique**||Identifie l'espace de table utilisé par la table de journalisation pour un article publié à partir d'une base de données Oracle. Pour plus d’informations, consultez [Gérer des espaces disque logiques Oracle](../../relational-databases/replication/non-sql/manage-oracle-tablespaces.md).|  
|**threshold**||Valeur de pourcentage qui contrôle le moment où l'Agent de distribution affecte une nouvelle plage d'identité. Non pris en charge pour la réplication d'égal à égal.|  
|**type**||Non pris en charge pour les serveurs de publication Oracle.|  
||**logbased**|Article basé sur le journal.|  
||**logbased manualboth**|Article reposant sur un journal, avec filtre manuel et vue manuelle. Cette option nécessite que les propriétés de *filtre* et de *sync_object* soient également définies. Non pris en charge pour les serveurs de publication Oracle.|  
||**logbased manualfilter**|Article reposant sur un journal, avec filtre manuel. Cette option nécessite que les propriétés de *filtre* et de *sync_object* soient également définies. Non pris en charge pour les serveurs de publication Oracle.|  
||**logbased manualview**|Article reposant sur un journal, avec vue manuelle. Cette option nécessite que la propriété *sync_object* soit également définie. Non pris en charge pour les serveurs de publication Oracle.|  
||**viewlogbased indexés**|Article de vue indexée reposant sur un journal. Non pris en charge pour les serveurs de publication Oracle. Pour ce type d'article, il n'est pas nécessaire de publier la table de base séparément.|  
||**index viewlogbased manualboth**|Article de vue indexée reposant sur un journal avec filtre manuel et vue manuelle. Cette option nécessite que les propriétés de *filtre* et de *sync_object* soient également définies. Pour ce type d'article, il n'est pas nécessaire de publier la table de base séparément. Non pris en charge pour les serveurs de publication Oracle.|  
||**index viewlogbased manualfilter**|Article de vue indexée reposant sur un journal avec filtre manuel. Cette option requiert l' *sync_object* et les propriétés de *filtre* sont également définies. Pour ce type d'article, il n'est pas nécessaire de publier la table de base séparément. Non pris en charge pour les serveurs de publication Oracle.|  
||**index viewlogbased manualview**|Article de vue indexée reposant sur un journal avec vue manuelle. Cette option nécessite que la propriété *sync_object* soit également définie. Pour ce type d'article, il n'est pas nécessaire de publier la table de base séparément. Non pris en charge pour les serveurs de publication Oracle.|  
|**upd_cmd**||Instruction UPDATE à exécuter ; à défaut, elle sera élaborée à partir du journal.|  
|NULL|NULL|Renvoie une liste de propriétés d'articles modifiables.|  
  
`[ @force_invalidate_snapshot = ] force_invalidate_snapshot` Confirme que l’action entreprise par cette procédure stockée peut invalider un instantané existant. *force_invalidate_snapshot* est un **bit**, avec **0** comme valeur par défaut.  
  
 **0** indique que les modifications apportées à l’article n’entraînent pas la non-validité de l’instantané. Si la procédure stockée détecte que la modification requiert un nouvel instantané, une erreur se produit et aucune modification n'est effectuée.  
  
 **1** indique que les modifications apportées à l’article peuvent entraîner la non-validité de l’instantané, et s’il existe des abonnements qui nécessitent un nouvel instantané, donne l’autorisation de marquer l’instantané existant comme obsolète et de générer un nouvel instantané.  
  
 Consultez la section Remarques pour connaître les propriétés dont la modification nécessite la génération d'un nouvel instantané.  
  
`[ @force_reinit_subscription = ]force_reinit_subscription_` Confirme que l’action entreprise par cette procédure stockée peut nécessiter la réinitialisation des abonnements existants. *force_reinit_subscription* est un **bit** avec **0** comme valeur par défaut.  
  
 **0** indique que les modifications apportées à l’article n’entraînent pas la réinitialisation de l’abonnement. Si la procédure stockée détecte que la modification nécessite la réinitialisation des abonnements existants, une erreur se produit et aucune modification n'est effectuée.  
  
 **1** indique que les modifications apportées à l’article entraînent la réinitialisation des abonnements existants et autorise la réinitialisation de l’abonnement.  
  
 Voir la section Remarques pour connaître les propriétés dont la modification requiert la réinitialisation de tous les abonnements existants.  
  
`[ @publisher = ] 'publisher'` Spécifie un serveur de publication non- [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . *Publisher* est de **type sysname**, avec NULL comme valeur par défaut.  
  
> [!NOTE]  
>  l' *éditeur* ne doit pas être utilisé lors de la modification des propriétés d’un article sur un serveur de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] publication.  
  
## <a name="return-code-values"></a>Codet de retour  
 **0** (succès) ou **1** (échec)  
  
## <a name="remarks"></a>Notes  
 **sp_changearticle** est utilisé dans la réplication d’instantané et la réplication transactionnelle.  
  
 Lorsqu’un article appartient à une publication qui prend en charge la réplication transactionnelle d’égal à égal, vous ne pouvez modifier que les propriétés **Description**, **ins_cmd**, **upd_cmd** et **del_cmd** .  
  
 La modification de l’une des propriétés suivantes nécessite la génération d’un nouvel instantané, et vous devez spécifier la valeur **1** pour le paramètre *force_invalidate_snapshot* :  
  
-   **del_cmd**  
  
-   **dest_table**  
  
-   **destination_owner**  
  
-   **ins_cmd**  
  
-   **pre_creation_cmd**  
  
-   **schema_options**  
  
-   **upd_cmd**  
  
 La modification de l’une des propriétés suivantes nécessite la réinitialisation des abonnements existants, et vous devez spécifier la valeur **1** pour le paramètre *force_reinit_subscription* .  
  
-   **del_cmd**  
  
-   **dest_table**  
  
-   **destination_owner**  
  
-   **filter**  
  
-   **ins_cmd**  
  
-   **statut**  
  
-   **upd_cmd**  
  
 Dans une publication existante, vous pouvez utiliser **sp_changearticle** pour modifier un article sans avoir à supprimer et recréer l’intégralité de la publication.  
  
> [!NOTE]  
>  Lors de la modification de la valeur de *schema_option*, le système n’effectue pas de mise à jour au niveau du bit. Cela signifie que lorsque vous définissez *schema_option* à l’aide de **sp_changearticle**, les paramètres de bit existants peuvent être désactivés. Pour conserver les paramètres existants, vous devez effectuer [| (Opérateur or au niveau du bit)](../../t-sql/language-elements/bitwise-or-transact-sql.md) entre la valeur que vous définissez et la valeur actuelle de *schema_option*, qui peut être déterminée en exécutant [sp_helparticle](../../relational-databases/system-stored-procedures/sp-helparticle-transact-sql.md).  
  
## <a name="valid-schema-options"></a>Options de schéma valides  
 Le tableau suivant décrit les valeurs autorisées de *schema_option* en fonction du type de réplication (indiqué dans la partie supérieure) et du type d’article (indiqué dans la première colonne).  
  
| Type de l'article | Type de réplication-transactionnel | Type de réplication-instantané |
| ------------ | -------------------------------- | --------------------------- |
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
>  Pour les publications de mise à jour en attente, la valeur *schema_option* de **0x80** doit être activée. Les valeurs de *schema_option* prises en charge pour les [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] publications non sont : **0x01**, **0x02**, **0x10**, **0x40**, **0x80**, **0x1000** et **0x4000**.  
  
## <a name="example"></a>Exemple  
 [!code-sql[HowTo#sp_changetranarticle](../../relational-databases/replication/codesnippet/tsql/sp-changearticle-transac_1.sql)]  
  
## <a name="permissions"></a>Autorisations  
 Seuls les membres du rôle serveur fixe **sysadmin** ou du rôle de base de données fixe **db_owner** peuvent exécuter **sp_changearticle**.  
  
## <a name="see-also"></a>Voir aussi  
 [Afficher et modifier les propriétés d’un article](../../relational-databases/replication/publish/view-and-modify-article-properties.md)   
 [Changer les propriétés des publications et des articles](../../relational-databases/replication/publish/change-publication-and-article-properties.md)   
 [sp_addarticle &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addarticle-transact-sql.md)   
 [sp_articlecolumn &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-articlecolumn-transact-sql.md)   
 [sp_droparticle &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-droparticle-transact-sql.md)   
 [sp_helparticle &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helparticle-transact-sql.md)   
 [sp_helparticlecolumns &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helparticlecolumns-transact-sql.md)  
  
  
