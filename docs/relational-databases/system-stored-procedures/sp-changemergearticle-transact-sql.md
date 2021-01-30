---
description: sp_changemergearticle (Transact-SQL)
title: sp_changemergearticle (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 11/09/2015
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_changemergearticle_TSQL
- sp_changemergearticle
helpviewer_keywords:
- sp_changemergearticle
ms.assetid: 0dc3da5c-4af6-45be-b5f0-074da182def2
author: markingmyname
ms.author: maghan
ms.openlocfilehash: b504644f5b96cc631a6352d71f2b387e8040b5a0
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99159888"
---
# <a name="sp_changemergearticle-transact-sql"></a>sp_changemergearticle (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Modifie les propriétés d'un article de fusion. Cette procédure stockée est exécutée sur le serveur de publication dans la base de données de publication.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_changemergearticle [ @publication = ] 'publication'  
        , [ @article = ] 'article'  
    [ , [ @property = ] 'property' ]  
    [ , [ @value = ] 'value' ]  
    [ , [ @force_invalidate_snapshot = ] force_invalidate_snapshot ]  
    [ , [ @force_reinit_subscription = ] force_reinit_subscription ]  
```  
  
## <a name="arguments"></a>Arguments  
`[ @publication = ] 'publication'` Nom de la publication dans laquelle l’article existe. *publication* est de **type sysname**, sans valeur par défaut.  
  
`[ @article = ] 'article'` Nom de l’article à modifier. *article* est de **type sysname** et n’a pas de valeur par défaut.  
  
`[ @property = ] 'property'` Propriété à modifier pour l’article et la publication donnés. la *propriété* est de type **nvarchar (30)** et peut prendre l’une des valeurs indiquées dans le tableau.  
  
`[ @value = ] 'value'` Nouvelle valeur de la propriété spécifiée. la *valeur* est de type **nvarchar (1000)** et peut prendre l’une des valeurs indiquées dans le tableau.  
  
 Le tableau ci-dessous décrit les propriétés des articles et les valeurs de ces propriétés.  
  
|Propriété|Valeurs|Description|  
|--------------|------------|-----------------|  
|**allow_interactive_resolver**|**true**|Permet d'utiliser un outil de résolution interactif pour l'article.|  
||**false**|Désactive l'utilisation d'un outil de résolution interactif pour l'article.|  
|**article_resolver**||Outil de résolution personnalisé pour l'article S’applique uniquement à un article de table.|  
|**check_permissions** (bitmap)|**0x00**|Les autorisations au niveau de la table ne sont pas vérifiées.|  
||**0x10**|Les autorisations au niveau de la table sont vérifiées dans le serveur de publication avant que les instructions INSERT effectuées sur l'Abonné ne soient appliquées dans le serveur de publication.|  
||**0x20**|Les autorisations au niveau de la table sont vérifiées dans le serveur de publication avant que les instructions UPDATE effectuées sur l'Abonné ne soient appliquées dans le serveur de publication.|  
||**0x40**|Les autorisations au niveau de la table sont vérifiées sur le serveur de publication avant que les instructions DELETE effectuées sur l'Abonné ne soient appliquées dans le serveur de publication.|  
|**column_tracking**|**true**|Active le suivi au niveau de la colonne. S’applique uniquement à un article de table.<br /><br /> Remarque : le suivi au niveau des colonnes ne peut pas être utilisé lors de la publication de tables contenant plus de 246 colonnes.|  
||**false**|Désactive le suivi au niveau de la colonne et conserve la détection des conflits au niveau de la ligne. S’applique uniquement à un article de table.|  
|**compensate_for_errors**|**true**|Des actions de compensation sont effectuées lorsque des erreurs se produisent au cours de la synchronisation. Pour plus d’informations, consultez [sp_addmergearticle](../../relational-databases/system-stored-procedures/sp-addmergearticle-transact-sql.md).|  
||**false**|Les actions de compensation ne sont pas effectuées, ce qui est le comportement par défaut. Pour plus d’informations, consultez [sp_addmergearticle](../../relational-databases/system-stored-procedures/sp-addmergearticle-transact-sql.md).<br /><br /> Important bien que les données des lignes affectées puissent sembler non conformes, dès que vous résolvez les erreurs, les modifications peuvent être appliquées et les données convergent. **\* \* \* \*** Si la table source d’un article est déjà publiée dans une autre publication, la valeur de *compensate_for_errors* doit être la même pour les deux articles.|  
|**creation_script**||Chemin d'accès et nom d'un script de schéma d'article facultatif utilisé pour créer l'article dans la base de données d'abonnement.|  
|**delete_tracking**|**true**|Les instructions DELETE sont répliquées, ce qui est le comportement par défaut.|  
||**false**|Les instructions DELETE ne sont pas répliquées.<br /><br /> Un paramètre **\* \* \* important \*** **delete_tracking** la **valeur false** entraîne une non-convergence et les lignes supprimées doivent être supprimées manuellement.|  
|**description**||Entrée descriptive de l'article|  
|**destination_owner**||Nom du propriétaire de l’objet dans la base de données d’abonnement, si ce n’est pas **dbo**.|  
|**identity_range**||**bigint** qui spécifie la taille de plage à utiliser lors de l’affectation de nouvelles valeurs d’identité si l’article a **identityrangemanagementoption** défini sur **auto** ou **auto_identity_range** défini sur **true**. S'applique à un article de table uniquement. Pour plus d’informations, consultez la section « réplication de fusion » de l’article [répliquer des colonnes d’identité](../../relational-databases/replication/publish/replicate-identity-columns.md).|  
|**identityrangemanagementoption**|**Manuelle**|Désactive la gestion automatique des plages d'identité. Marque les colonnes d'identité en utilisant NOT FOR REPLICATION pour permettre la gestion manuelle des plages d'identité. Pour plus d’informations, consultez [ Répliquer des colonnes d’identité](../../relational-databases/replication/publish/replicate-identity-columns.md).|  
||**Aucune**|Désactive toute la gestion des plages d'identité.|  
|**logical_record_level_conflict_detection**|**true**|Un conflit est détecté si des modifications sont apportées à un point quelconque de l'enregistrement logique. Requiert que **logical_record_level_conflict_resolution** ait la valeur **true**.|  
||**false**|La détection de conflit par défaut est utilisée comme spécifié par **column_tracking**.|  
|**logical_record_level_conflict_resolution**|**true**|L'enregistrement logique gagnant complet remplace l'enregistrement logique perdant.|  
||**false**|Les lignes gagnantes ne sont pas limitées à l'enregistrement logique.|  
|**partition_options**|**0**|Le filtrage de l'article est statique ou il ne produit pas un sous-ensemble unique de données pour chaque partition, c'est-à-dire une partition en « chevauchement ».|  
||**1**|Les partitions se chevauchent, et les mises à jour DML effectuées sur l'Abonné ne peuvent pas modifier la partition à laquelle une ligne appartient.|  
||**2**|Le filtrage de l'article produit des partitions qui ne se chevauchent pas, mais plusieurs abonnés peuvent recevoir la même partition.|  
||**3**|Le filtrage de l'article produit des partitions qui ne se chevauchent pas et qui sont uniques pour chaque abonnement.<br /><br /> Remarque : Si vous spécifiez une valeur de **3** pour **partition_options**, il ne peut y avoir qu’un seul abonnement pour chaque partition de données de cet article. Si un deuxième abonnement est créé dans lequel le critère de filtrage du nouvel abonnement produit la même partition que l'abonnement existant, ce dernier est supprimé.|  
|**pre_creation_command**|**Aucune**|Si la table existe déjà côté abonné, aucune action n'est effectuée.|  
||**delete**|Entraîne une suppression basée sur la clause WHERE dans le filtre de sous-ensemble.|  
||**drop**|Supprime la table avant de la recréer.|  
||**truncate**|Tronque la table de destination.|  
|**processing_order**||**entier** qui indique l’ordre de traitement des articles dans une publication de fusion.|  
|**pub_identity_range**||**bigint** qui spécifie la taille de plage allouée à un abonné avec un abonnement serveur si l’article a **identityrangemanagementoption** défini sur **auto** ou **auto_identity_range** défini sur **true**. Cette plage d'identité est réservée à un Abonné de republication qui peut l'allouer à ses propres Abonnés. S'applique à un article de table uniquement. Pour plus d’informations, consultez la section « réplication de fusion » de l’article [répliquer des colonnes d’identité](../../relational-databases/replication/publish/replicate-identity-columns.md).|  
|**published_in_tran_pub**|**true**|L'article est également publié dans une publication transactionnelle.|  
||**false**|L'article n'est pas également publié dans une publication transactionnelle.|  
|**resolver_info**||Permet de définir les informations supplémentaires requises par un outil de résolution personnalisé. Certains outils de résolution [!INCLUDE[msCoName](../../includes/msconame-md.md)] nécessitent une colonne en guise d'entrée. **resolver_info** est de type **nvarchar (255)**, avec NULL comme valeur par défaut. Pour plus d’informations, consultez [Programmes de résolution COM Microsoft](../../relational-databases/replication/merge/advanced-merge-replication-conflict-com-based-resolvers.md).|  
|**schema_option** (bitmap)||Pour plus d'informations, consultez la section « Notes » plus loin dans cette rubrique.|  
||**0x00**|Désactive les scripts par le Agent d’instantané et utilise le script fourni dans **creation_script**.|  
||**0x01**|Génère le script de création d'objet (CREATE TABLE, CREATE PROCEDURE, etc.).|  
||**0x10**|Génère un index cluster correspondant.|  
||**0x20**|Convertit les types de données définis par l'utilisateur en types de données de base auprès de l'Abonné. Cette option ne peut pas être utilisée lorsqu’il existe une contrainte CHECK ou DEFAULT sur une colonne de type défini par l’utilisateur (UDT), si une colonne UDT fait partie de la clé primaire ou si une colonne calculée fait référence à une colonne UDT.|  
||**0x40**|Génère les index non-cluster correspondants.|  
||**0x80**|Inclut l'intégrité référentielle déclarée dans les clés primaires.|  
||**0x100**|Réplique les déclencheurs utilisateur, si ceux-ci sont définis, sur un article de table.|  
||**0x200**|Réplique les contraintes FOREIGN KEY. Si la table référencée ne fait pas partie d'une publication, aucune contrainte FOREIGN KEY appliquée à une table publiée n'est répliquée.|  
||**0x400**|Réplique les contraintes CHECK.|  
||**0x800**|Réplique les valeurs par défaut.|  
||**0x1000**|Réplique le classement au niveau des colonnes.|  
||**0x2000**|Réplique les propriétés étendues associées à l'objet source de l'article publié.|  
||**0x4000**|Réplique les clés uniques, si celles-ci sont définies, sur un article de table.|  
||**0x8000**|Génère des instructions ALTER TABLE lors de la création d'un script de contraintes.|  
||**0x10000**|Réplique les contraintes CHECK en tant que NOT FOR REPLICATION afin que les contraintes ne soient pas appliquées durant la synchronisation.|  
||**0x20000**|Réplique les contraintes FOREIGN KEY en tant que NOT FOR REPLICATION afin que les contraintes ne soient pas appliquées durant la synchronisation.|  
||**0x40000**|Réplique les groupes de fichiers associés à une table ou un index partitionné.|  
||**0x80000**|Réplique le schéma de partition d'une table partitionnée.|  
||**0x100000**|Réplique le schéma de partition d'un index partitionné.|  
||**0x200000**|Réplique les statistiques d'une table.|  
||**0x400000**|Réplique les liaisons par défaut|  
||**0x800000**|Réplique des liaisons de règle.|  
||**0x1000000**|Réplique l'index de texte intégral.|  
||**0x2000000**|Les collections de schémas XML liées aux colonnes **XML** ne sont pas répliquées.|  
||**0x4000000**|Réplique les index sur les colonnes **XML** .|  
||**0x8000000**|Crée tout schéma non encore présent chez l'abonné.|  
||**0x10000000**|Convertit les colonnes **XML** en **ntext** sur l’abonné.|  
||**0x20000000**|Convertit les types de données d’objet volumineux (**nvarchar (max)**, **varchar (max)** et **varbinary (max)**) introduits dans en [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] types de données pris en charge sur [!INCLUDE[ssVersion2000](../../includes/ssversion2000-md.md)] .|  
||**0x40000000**|Réplique les autorisations.|  
||**0x80000000**|Tente de supprimer les dépendances envers tous les objets qui ne font pas partie de la publication.|  
||**0x100000000**|Utilisez cette option pour répliquer l’attribut FILESTREAM s’il est spécifié sur des colonnes **varbinary (max)** . Ne spécifiez pas cette option si vous répliquez des tables sur des Abonnés [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)]. La réplication de tables qui possèdent des colonnes FILESTREAM sur [!INCLUDE[ssVersion2000](../../includes/ssversion2000-md.md)] des abonnés n’est pas prise en charge, quelle que soit la façon dont cette option de schéma est définie. Consultez l’option associée **0x800000000**.|  
||**0x200000000**|Convertit les types de données de date et d’heure (**Date**, **Time**, **DateTimeOffset** et **datetime2**) introduits dans en [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] types de données pris en charge dans les versions antérieures de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .|  
||**0x400000000**|Réplique l'option de compression pour les données et les index. Pour plus d’informations, consultez [Compression de données](../../relational-databases/data-compression/data-compression.md).|  
||**0x800000000**|Définissez cette option pour stocker les données FILESTREAM dans leur propre groupe de fichiers sur l'Abonné. Si cette option n'est pas définie, les données FILESTREAM sont stockées dans le groupe de fichiers par défaut. La réplication ne crée pas de groupes de fichiers ; par conséquent, si vous définissez cette option, vous devez créer le groupe de fichiers avant d'appliquer l'instantané à l'Abonné. Pour plus d’informations sur la création d’objets avant l’application de l’instantané, consultez [exécuter des scripts avant et après l’application de l’instantané](../../relational-databases/replication/snapshot-options.md#execute-scripts-before-and-after-snapshot-is-applied).<br /><br /> Consultez l’option associée **0x100000000**.|  
||**0x1000000000**|Convertit les types définis par l’utilisateur (UDT) common language runtime (CLR) en **varbinary (max)** afin que les colonnes de type UDT puissent être répliquées sur les abonnés qui exécutent [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] .|  
||**0x2000000000**|Convertit le type de données **hierarchyid** en **varbinary (max)** afin que les colonnes de type **hierarchyid** puissent être répliquées sur les abonnés qui exécutent [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] . Pour plus d’informations sur l’utilisation des colonnes **hierarchyid** dans les tables répliquées, consultez [hierarchyid &#40;Transact-SQL&#41;](../../t-sql/data-types/hierarchyid-data-type-method-reference.md).|  
||**0x4000000000**|Réplique tous les index filtrés sur la table. Pour plus d’informations sur les index filtrés, consultez [créer des index filtrés](../../relational-databases/indexes/create-filtered-indexes.md).|  
||**0x8000000000**|Convertit les types de données **Geography** et **Geometry** en **varbinary (max)** afin que les colonnes de ces types puissent être répliquées sur les abonnés qui exécutent [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] .|  
||**0x10000000000**|Réplique les index sur les colonnes de type **Geography** et **Geometry**.|  
||NULL|Le système génère automatiquement une option de schéma valide pour l'article.|  
|**statut**|**active**|Exécution du script de traitement initial pour publier la table.|  
||**unsynced**|Le script de traitement initial servant à publier la table sera exécuté lors de la prochaine exécution de l'Agent d'instantané.|  
|**stream_blob_columns**|**true**|Une optimisation de flux de données est utilisée lors de la réplication de colonnes d'objets binaires volumineux (BLOB). Toutefois, certaines fonctionnalités de réplication de fusion, telles que les enregistrements logiques, peuvent tout de même empêcher d'utiliser l'optimisation de flux. *stream_blob_columns* a la valeur true lorsque FileStream est activé. Cela permet la réplication des données FILESTREAM dans le but d'optimiser et de réduire l'utilisation de la mémoire. Pour forcer les Articles de la table FILESTREAM à ne pas utiliser le streaming d’objets BLOB, affectez à *stream_blob_columns* la valeur false.<br /><br /> Important l’activation de cette optimisation de mémoire peut nuire aux performances de la agent de fusion lors de la synchronisation. **\* \* \* \*** Cette option ne doit être utilisée que lors de la réplication de colonnes contenant des mégaoctets de données.|  
||**false**|L'optimisation n'est pas utilisée lors de la réplication de colonnes BLOB.|  
|**subscriber_upload_options**|**0**|Aucune restriction sur les mises à jour effectuées sur un Abonné disposant d'un abonnement client ; les modifications sont téléchargées sur le serveur de publication. La modification de cette propriété peut exiger la réinitialisation des Abonnés existants.|  
||**1**|Les modifications sont autorisées sur un Abonné disposant d'un abonnement client, mais elles ne sont pas téléchargées sur le serveur de publication.|  
||**2**|Les modifications ne sont pas autorisées sur un Abonné disposant d'un abonnement client.|  
|**subset_filterclause**||Clause WHERE spécifiant le filtrage horizontal. S’applique uniquement à un article de table.<br /><br /> Important pour des raisons de performances, nous vous recommandons de ne pas appliquer de fonctions aux noms de colonnes dans les clauses de filtre de lignes paramétrable, telles que. **\* \* \* \*** `LEFT([MyColumn]) = SUSER_SNAME()` Si vous utilisez [HOST_NAME](../../t-sql/functions/host-name-transact-sql.md) dans une clause de filtre et que vous remplacez la valeur de HOST_NAME, vous devrez peut-être convertir les types de données à l’aide de [Convert](../../t-sql/functions/cast-and-convert-transact-sql.md). Pour plus d’informations sur les meilleures pratiques pour ce cas, consultez la section « substitution de la valeur HOST_NAME () » dans [filtres de lignes paramétrables](../../relational-databases/replication/merge/parameterized-filters-parameterized-row-filters.md).|  
|**threshold**||Valeur de pourcentage utilisée pour les abonnés exécutant [!INCLUDE[ssEW](../../includes/ssew-md.md)] ou des versions antérieures de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . le **seuil** contrôle le moment où l’agent de fusion affecte une nouvelle plage d’identité. Lorsque le pourcentage de valeurs spécifié dans le seuil est utilisé, l'Agent de fusion crée une nouvelle plage d'identité. Utilisé lorsque **identityrangemanagementoption** a la valeur **auto** ou **auto_identity_range** a la valeur **true**. S'applique à un article de table uniquement. Pour plus d’informations, consultez la section « réplication de fusion » de l’article [répliquer des colonnes d’identité](../../relational-databases/replication/publish/replicate-identity-columns.md).|  
|**verify_resolver_signature**|**1**|La signature numérique d'un outil de résolution personnalisé est vérifiée pour déterminer s'il provient d'une source approuvée.|  
||**0**|La signature numérique d'un outil de résolution personnalisé n'est pas vérifiée pour déterminer s'il provient d'une source approuvée.|  
|NULL (par défaut)||Retourne la liste des valeurs prises en charge pour la *propriété*.|  
  
`[ @force_invalidate_snapshot = ] force_invalidate_snapshot` Confirme que l’action entreprise par cette procédure stockée peut invalider un instantané existant. *force_invalidate_snapshot* est un **bit**, avec **0** comme valeur par défaut.  
  
 **0** indique que les modifications apportées à l’article de fusion n’entraînent pas la non-validité de l’instantané. Si la procédure stockée détecte que la modification requiert un nouvel instantané, une erreur se produit et aucune modification n'est effectuée.  
  
 **1** signifie que les modifications apportées à l’article de fusion peuvent entraîner la non-validité de l’instantané, et s’il existe des abonnements qui nécessitent un nouvel instantané, accorde l’autorisation de marquer l’instantané existant comme obsolète et de générer un nouvel instantané.  
  
 Consultez la section Remarques pour connaître les propriétés dont la modification nécessite la génération d'un nouvel instantané.  
  
`[ @force_reinit_subscription = ] force_reinit_subscription` Confirme que l’action entreprise par cette procédure stockée peut nécessiter la réinitialisation des abonnements existants. *force_reinit_subscription* est un **bit**, avec **0** comme valeur par défaut.  
  
 **0** indique que les modifications apportées à l’article de fusion n’entraînent pas la réinitialisation de l’abonnement. Si la procédure stockée détecte que la modification nécessite la réinitialisation des abonnements existants, une erreur se produit et aucune modification n'est effectuée.  
  
 **1** signifie que les modifications apportées à l’article de fusion entraînent la réinitialisation des abonnements existants et autorisent la réinitialisation de l’abonnement.  
  
 Voir la section Remarques pour connaître les propriétés dont la modification requiert la réinitialisation de tous les abonnements existants.  
  
## <a name="return-code-values"></a>Codet de retour  
 **0** (succès) ou **1** (échec)  
  
## <a name="remarks"></a>Notes  
 **sp_changemergearticle** est utilisé dans la réplication de fusion.  
  
 Étant donné que **sp_changemergearticle** est utilisé pour modifier les propriétés d’article qui ont été initialement spécifiées à l’aide de [sp_addmergearticle](../../relational-databases/system-stored-procedures/sp-addmergearticle-transact-sql.md), consultez [sp_addmergearticle](../../relational-databases/system-stored-procedures/sp-addmergearticle-transact-sql.md) pour plus d’informations sur ces propriétés.  
  
 La modification des propriétés suivantes requiert la génération d’un nouvel instantané, et vous devez spécifier la valeur **1** pour le paramètre *force_invalidate_snapshot* :  
  
-   **check_permissions**  
  
-   **column_tracking**  
  
-   **destination_owner**  
  
-   **pre_creation_command**  
  
-   **schema_options**  
  
-   **subset_filterclause**  
  
 La modification des propriétés suivantes nécessite la réinitialisation des abonnements existants, et vous devez spécifier la valeur **1** pour le paramètre *force_reinit_subscription* :  
  
-   **check_permissions**  
  
-   **column_tracking**  
  
-   **destination_owner**  
  
-   **pre_creation_command**  
  
-   **identityrangemanagementoption**  
  
-   **subscriber_upload_options**  
  
-   **subset_filterclause**  
  
-   **creation_script**  
  
-   **schema_option**  
  
-   **logical_record_level_conflict_detection**  
  
-   **logical_record_level_conflict_resolution**  
  
 Lorsque vous spécifiez la valeur 3 pour **partition_options**, les métadonnées sont nettoyées chaque fois que le agent de fusion s’exécute et que l’instantané partitionné expire plus rapidement. Lorsque vous utilisez cette option, pensez à activer l'instantané partitionné requis par l'abonné. Pour plus d'informations, voir [Snapshots for Merge Publications with Parameterized Filters](../../relational-databases/replication/create-a-snapshot-for-a-merge-publication-with-parameterized-filters.md).  
  
 Lors de la définition de la propriété **column_tracking** , si la table est déjà publiée dans d’autres publications de fusion, le suivi de colonne doit être identique à la valeur utilisée par les articles existants basés sur cette table. Ce paramètre concerne uniquement les articles de table.  
  
 Si plusieurs publications publient des articles basés sur la même table sous-jacente, la modification de la propriété **delete_tracking** ou de la propriété **compensate_for_errors** pour un article entraîne la même modification dans les autres articles qui sont basés sur la même table.  
  
 Si la connexion d'accès/le compte d'utilisateur du serveur de publication utilisé par le processus de fusion ne possède pas les autorisations de table appropriées, les modifications non valides sont enregistrées en tant que conflits.  
  
 Lors de la modification de la valeur de **schema_option**, le système n’effectue pas de mise à jour au niveau du bit. Cela signifie que lorsque vous définissez **schema_option** à l’aide de **sp_changemergearticle**, les paramètres de bit existants peuvent être désactivés. Pour conserver les paramètres existants, vous devez effectuer des [& (au niveau du bit)](../../t-sql/language-elements/bitwise-and-transact-sql.md) entre la valeur que vous définissez et la valeur actuelle de *schema_option*, qui peut être déterminée en exécutant [sp_helpmergearticle](../../relational-databases/system-stored-procedures/sp-helpmergearticle-transact-sql.md).  
  
> [!CAUTION]  
>  Lorsque vous avez plusieurs articles d’une publication (peut-être des centaines) et que vous exécutez **sp_changemergearticle** pour l’un des articles, l’exécution peut prendre beaucoup de temps.  
  
## <a name="valid-schema-option-table"></a>Tableau des options de schéma valides  
 Le tableau suivant décrit les valeurs *schema_option* autorisées, en fonction du type de l’article.  
  
|Type de l'article|Valeurs de l'option de schéma|  
|------------------|--------------------------|  
|**func schema only**|**0x01** et **0x2000**|  
|**indexed view schema only**|**0x01**, **0x040**, **0x0100**, **0x2000**, **0x40000**, **0x1000000** et **0x200000**|  
|**proc schema only**|**0x01** et **0x2000**|  
|**table**|Toutes les options.|  
|**view schema only**|**0x01**, **0x040**, **0x0100**, **0x2000**, **0x40000**, **0x1000000** et **0x200000**|  
  
## <a name="example"></a>Exemple  
 [!code-sql[HowTo#sp_changemergearticle](../../relational-databases/replication/codesnippet/tsql/sp-changemergearticle-tr_1.sql)]  
  
## <a name="permissions"></a>Autorisations  
 Seuls les membres du rôle serveur fixe **sysadmin** ou du rôle de base de données fixe **db_owner** peuvent exécuter **sp_changemergearticle**.  
  
## <a name="see-also"></a>Voir aussi  
 [Afficher et modifier les propriétés d’un article](../../relational-databases/replication/publish/view-and-modify-article-properties.md)   
 [Changer les propriétés des publications et des articles](../../relational-databases/replication/publish/change-publication-and-article-properties.md)   
 [sp_addmergearticle &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addmergearticle-transact-sql.md)   
 [sp_dropmergearticle &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-dropmergearticle-transact-sql.md)   
 [sp_helpmergearticle &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helpmergearticle-transact-sql.md)   
 [Procédures stockées de réplication &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/replication-stored-procedures-transact-sql.md)  
  
  
