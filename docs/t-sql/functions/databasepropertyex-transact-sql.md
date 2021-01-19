---
description: DATABASEPROPERTYEX (Transact-SQL)
title: DATABASEPROPERTYEX (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 04/23/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- DATABASEPROPERTYEX
- DATABASEPROPERTYEX_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- DATABASEPROPERTYEX function
- displaying database properties
- database properties [SQL Server]
ms.assetid: 8a9e0ffb-28b5-4640-95b2-a54e3e5ad941
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 096d215aa0962b3a580cdd43a337075aab638964
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/13/2021
ms.locfileid: "98171141"
---
# <a name="databasepropertyex-transact-sql"></a>DATABASEPROPERTYEX (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Pour une base de données spécifiée dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], cette fonction retourne le paramètre actuel de l’option ou de la propriété de base de données spécifiée.
  
![Icône du lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql
DATABASEPROPERTYEX ( database , property )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments
*database*  
Expression spécifiant le nom de la base de données pour laquelle `DATABASEPROPERTYEX` retourne des informations sur la propriété nommée. Le type de données de *database* est **nvarchar(128)** .  

Pour [!INCLUDE[ssSDS](../../includes/sssds-md.md)], `DATABASEPROPERTYEX` a besoin du nom de la base de données active. Elle retourne la valeur NULL pour toutes les propriétés si un nom de base de données différent lui est donné.
  
*property*  
Expression spécifiant le nom de la propriété de base de données à retourner. *property* possède le type de données **varchar (128)** et prend en charge l’une des valeurs contenues dans ce tableau :
  
> [!NOTE]  
>  Si la base de données n’a pas encore démarré, les appels à `DATABASEPROPERTYEX` retournent la valeur NULL si `DATABASEPROPERTYEX` récupère ces valeurs par un accès direct à la base de données, au lieu d’une récupération à partir des métadonnées. Une base de données avec l’option AUTO_CLOSE définie sur ON, ou bien en mode hors connexion, est définie comme étant « non démarrée ».  
  
|Propriété|Description|Valeur retournée|  
|---|---|---|
|Classement|Nom du classement par défaut de la base de données|Nom du classement<br /><br /> NULL : la base de données n’est pas démarrée.<br /><br /> Type de données de base : **nvarchar(128)**|  
|ComparisonStyle|Style de comparaison Windows du classement. Pour générer une bitmap pour la valeur ComparisonStyle terminée, utilisez les valeurs de style suivantes :<br /><br /> Ignorer la casse : 1<br /><br /> Ignorer les accents : 2<br /><br /> Ignorer le type de caractères Kana : 65536<br /><br /> Ignorer la largeur : 131 072<br /><br /> <br /><br /> Par exemple, la valeur par défaut 196609 est le résultat de la combinaison des options permettant d'ignorer la casse, le type de caractères Kana et la largeur.|Retourne le style de comparaison.<br /><br /> Renvoie 0 pour tous les classements binaires.<br /><br /> Type de données de base : **int**|  
|Édition|Édition de la base de données ou couche de service.|**S’applique à** : [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)], [!INCLUDE[ssSDW](../../includes/sssdw-md.md)].<br /><br /> <br /><br /> Usage général<br /><br /> Critique pour l’entreprise<br /><br /> De base<br /><br /> Standard<br /><br /> Premium<br /><br /> Système (pour la base de données master)<br /><br /> NULL : la base de données n’est pas démarrée.<br /><br /> Type de données de base : **nvarchar**(64)|  
|IsAnsiNullDefault|La base de données suit les règles ISO d'autorisation des valeurs Null.|1 : TRUE<br /><br /> 0 : FALSE<br /><br /> NULL : entrée non valide<br /><br /> Type de données de base : **int**|  
|IsAnsiNullsEnabled|Toutes les comparaisons à une valeur NULL produisent le résultat UNKNOWN (inconnu).|1 : TRUE<br /><br /> 0 : FALSE<br /><br /> NULL : entrée non valide<br /><br /> Type de données de base : **int**|  
|IsAnsiPaddingEnabled|Les chaînes sont complétées à la même longueur avant leur comparaison ou insertion.|1 : TRUE<br /><br /> 0 : FALSE<br /><br /> NULL : entrée non valide<br /><br /> Type de données de base : **int**|  
|IsAnsiWarningsEnabled|SQL Server émet des messages d’erreur ou d’avertissement quand des conditions d’erreur standard se présentent.|1 : TRUE<br /><br /> 0 : FALSE<br /><br /> NULL : entrée non valide<br /><br /> Type de données de base : **int**|  
|IsArithmeticAbortEnabled|Les requêtes s’arrêtent quand une erreur liée à un dépassement de capacité ou une division par zéro se produit pendant leur exécution.|1 : TRUE<br /><br /> 0 : FALSE<br /><br /> NULL : entrée non valide<br /><br /> Type de données de base : **int**|  
|IsAutoClose|La base de données est fermée correctement et ses ressources sont libérées après la fin de session du dernier utilisateur.|1 : TRUE<br /><br /> 0 : FALSE<br /><br /> NULL : entrée non valide<br /><br /> Type de données de base : **int**|  
|IsAutoCreateStatistics|L'optimiseur de requête crée des statistiques de colonnes uniques, selon les besoins, pour améliorer les performances des requêtes.|1 : TRUE<br /><br /> 0 : FALSE<br /><br /> NULL : entrée non valide<br /><br /> Type de données de base : **int**|  
|IsAutoCreateStatisticsIncremental|Les statistiques de colonnes uniques créées automatiquement sont incrémentielles quand cela est possible.|**S’applique à** : [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] et versions ultérieures.<br /><br /> 1 : TRUE<br /><br /> 0 : FALSE<br /><br /> NULL : entrée non valide<br /><br /> Type de données de base : **int**|  
|IsAutoShrink|Les fichiers de base de données peuvent faire l'objet d'une réduction périodique automatique.|1 : TRUE<br /><br /> 0 : FALSE<br /><br /> NULL : entrée non valide<br /><br /> Type de données de base : **int**|  
|IsAutoUpdateStatistics|Quand une requête utilise des statistiques existantes potentiellement périmées, l’optimiseur de requête met à jour ces statistiques.|1 : TRUE<br /><br /> 0 : FALSE<br /><br /> NULL : entrée non valide<br /><br /> Type de données de base : **int**|
|IsClone|La base de données est une copie de schéma et de statistiques uniquement d’une base de données utilisateur créée avec DBCC CLONEDATABASE. Pour plus d’informations, consultez l’[article du Support Microsoft](https://support.microsoft.com/help/3177838).|**S’applique à** : [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] SP2 et versions ultérieures.<br /><br /> 1 : TRUE<br /><br /> 0 : FALSE<br /><br /> NULL : entrée non valide<br /><br /> Type de données de base : **int**| 
|IsCloseCursorsOnCommitEnabled|Dès lors qu’une transaction est validée, tous les curseurs ouverts se ferment.|1 : TRUE<br /><br /> 0 : FALSE<br /><br /> NULL : entrée non valide<br /><br /> Type de données de base : **int**|  
|IsFulltextEnabled|L'indexation sémantique et de texte intégral est activée pour la base de données.|**S’applique à** : [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] et versions ultérieures.<br /><br /> <br /><br /> 1 : TRUE<br /><br /> 0 : FALSE<br /><br /> NULL : entrée non valide<br /><br /> Type de données de base : **int**<br /><br /> **Remarque :** La valeur de cette propriété est désormais sans effet. Les bases de données utilisateur sont toujours activées pour la recherche en texte intégral. Cette propriété sera supprimée dans une future version de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Évitez d’utiliser cette propriété dans un nouveau travail de développement et modifiez dès que possible les applications qui l’utilisent actuellement.|  
|IsInStandBy|La base de données est en ligne en lecture seule, avec la restauration du journal autorisée.|1 : TRUE<br /><br /> 0 : FALSE<br /><br /> NULL : entrée non valide<br /><br /> Type de données de base : **int**|  
|IsLocalCursorsDefault|Les déclarations de curseurs prennent la valeur LOCAL par défaut.|1 : TRUE<br /><br /> 0 : FALSE<br /><br /> NULL : entrée non valide<br /><br /> Type de données de base : **int**|  
|IsMemoryOptimizedElevateToSnapshotEnabled|L’isolation SNAPSHOT permet d’accéder aux tables à mémoire optimisée quand le paramètre de session TRANSACTION ISOLATION LEVEL est défini sur READ COMMITTED, READ UNCOMMITTED ou un niveau d’isolation inférieur.|**S’applique à** : [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] et versions ultérieures.<br /><br /> <br /><br /> 1 : TRUE<br /><br /> 0 : FALSE<br /><br /> Type de données de base : **int**|  
|IsMergePublished|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] prend en charge la publication de tables de base de données pour la réplication de fusion, si la réplication est installée.|1 : TRUE<br /><br /> 0 : FALSE<br /><br /> NULL : entrée non valide<br /><br /> Type de données de base : **int**|  
|IsNullConcat|Un opérande de concaténation Null produit NULL.|1 : TRUE<br /><br /> 0 : FALSE<br /><br /> NULL : entrée non valide<br /><br /> Type de données de base : **int**|  
|IsNumericRoundAbortEnabled|Des erreurs sont générées quand une perte de précision se produit dans des expressions.|1 : TRUE<br /><br /> 0 : FALSE<br /><br /> NULL : entrée non valide<br /><br /> Type de données de base : **int**|  
|IsParameterizationForced|L'option SET de base de données PARAMETERIZATION a la valeur FORCED.|1 : TRUE<br /><br /> 0 : FALSE<br /><br /> NULL : entrée non valide|  
|IsQuotedIdentifiersEnabled|Les guillemets doubles sont autorisés dans les identificateurs.|1 : TRUE<br /><br /> 0 : FALSE<br /><br /> NULL : entrée non valide<br /><br /> Type de données de base : **int**|  
|IsPublished|Si la réplication est installée, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] prend en charge la publication de tables de base de données pour la réplication transactionnelle ou de capture instantanée.|1 : TRUE<br /><br /> 0 : FALSE<br /><br /> NULL : entrée non valide<br /><br /> Type de données de base : **int**|  
|IsRecursiveTriggersEnabled|Le fonctionnement récursif des déclencheurs est activé.|1 : TRUE<br /><br /> 0 : FALSE<br /><br /> NULL : entrée non valide<br /><br /> Type de données de base : **int**|  
|IsSubscribed|La base de données est abonnée à une publication.|1 : TRUE<br /><br /> 0 : FALSE<br /><br /> NULL : entrée non valide<br /><br /> Type de données de base : **int**|  
|IsSyncWithBackup|La base de données est soit une base de données publiée, soit une base de données de distribution, et elle prend en charge une restauration qui n’interrompra pas la réplication transactionnelle.|1 : TRUE<br /><br /> 0 : FALSE<br /><br /> NULL : entrée non valide<br /><br /> Type de données de base : **int**|  
|IsTornPageDetectionEnabled|Le [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] détecte les opérations d'E/S interrompues à la suite d'une coupure de courant ou de toute autre panne du système.|1 : TRUE<br /><br /> 0 : FALSE<br /><br /> NULL : entrée non valide<br /><br /> Type de données de base : **int**| 
|IsVerifiedClone|La base de données est une copie de schéma et de statistiques uniquement d’une base de données utilisateur créée avec l’option WITH VERIFY_CLONEDB de DBCC CLONEDATABASE. Pour plus d’informations, consultez cet [article du Support Microsoft](https://support.microsoft.com/help/3177838).|**S’applique à** : À compter de [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] SP2.<br /><br /> <br /><br /> 1 : TRUE<br /><br /> 0 : FALSE<br /><br /> NULL : entrée non valide<br /><br /> Type de données de base : **int**| 
|IsXTPSupported|Indique si la base de données prend en charge l’option OLTP en mémoire, à savoir la création et l’utilisation de tables à mémoire optimisée et de modules compilés en mode natif.<br /><br /> Spécifique à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] :<br /><br /> La propriété IsXTPSupported est indépendante de l’existence de tout groupe de fichiers MEMORY_OPTIMIZED_DATA, qui est nécessaire pour la création d’objets OLTP en mémoire.|**S’applique à** : [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] et versions ultérieures) et [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].<br /><br /> 1 : TRUE<br /><br /> 0 : FALSE<br /><br /> NULL : entrée non valide, erreur ou non applicable<br /><br /> Type de données de base : **int**|  
|LastGoodCheckDbTime|Date et heure de la dernière exécution réussie de DBCC CHECKDB sur la base de données spécifiée. <sup>1</sup> Si DBCC CHECKDB n’a pas été exécuté sur une base de données, 1900-01-01 00:00:00.000 est retourné.|**S’applique à** : [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] depuis SP2.</br>[!INCLUDE[sssqlv14](../../includes/sssqlv14-md.md)] à compter de la mise à jour cumulative 9 (CU9).</br>[!INCLUDE[sssqlv15](../../includes/sssqlv15-md.md)] ou une version ultérieure.</br>Azure SQL Database.<br/><br/>Valeur DateHeure<br /><br /> NULL : entrée non valide<br /><br /> Type de données de base : **datetime**| 
|LCID|Identificateur de paramètres régionaux (LCID) Windows de classement.|Valeur LCID (au format décimal).<br /><br /> Type de données de base : **int**|  
|MaxSizeInBytes|Taille maximale de la base de données, en octets.|**S’applique à** : [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)], [!INCLUDE[ssSDW](../../includes/sssdw-md.md)].<br /><br />[Azure SQL Database et Azure Synapse Analytics](/azure/sql-database/sql-database-single-database-scale#dtu-based-purchasing-model) : la valeur est basée sur SLO, sauf en cas d’achat de stockage supplémentaire.<br /><br />[vCore](/azure/sql-database/sql-database-single-database-scale#vcore-based-purchasing-model) – La valeur est en incréments de 1 Go jusqu’à la taille maximale.<br /><br />NULL : la base de données n’est pas démarrée<br /><br /> Type de données de base : **bigint**|  
|Récupération|Mode de récupération de base de données|FULL : Mode de restauration complète<br /><br /> BULK_LOGGED : mode de récupération utilisant les journaux de transactions<br /><br /> SIMPLE : Mode de récupération simple<br /><br /> Type de données de base : **nvarchar(128)**|  
|ServiceObjective|Décrit le niveau de performance de la base de données dans [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)] ou [!INCLUDE[ssSDW](../../includes/sssdw-md.md)].|Celui-ci peut avoir l'une des valeurs suivantes :<br /><br /> NULL : base de données non démarrée<br /><br /> Shared (pour l'édition Web/Business)<br /><br /> De base<br /><br /> S0<br /><br /> S1<br /><br /> S2<br /><br /> S3<br /><br /> P1<br /><br /> P2<br /><br /> P3<br /><br /> ElasticPool<br /><br /> Système (pour base de données master)<br /><br /> Type de données de base : **nvarchar(32)**|  
|ServiceObjectiveId|ID de l'objectif de service dans la [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)].|**uniqueidentifier** qui identifie l’objectif de service.|  
|SQLSortOrder|ID d'ordre de tri [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pris en charge dans les versions antérieures de SQL Server.|0 : la base de données utilise le classement Windows<br /><br /> >0 : ID d’ordre de tri [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]<br /><br /> NULL : entrée non valide ou base de données non démarrée<br /><br /> Type de données de base : **tinyint**|  
|Statut|État de la base de données.|ONLINE : la base de données est disponible pour la requête.<br /><br /> **Remarque :** La fonction peut retourner un état ONLINE pendant que la base de données s’ouvre et qu’elle n’a pas encore été récupérée. Pour déterminer si une base de données ONLINE peut accepter les connexions, interrogez la propriété Collation de **DATABASEPROPERTYEX**. La base de données ONLINE peut accepter les connexions quand le classement de base de données retourne une valeur non NULL. Pour les bases de données AlwaysOn, interrogez les colonnes database_state ou database_state_desc de `sys.dm_hadr_database_replica_states`.<br /><br /> OFFLINE : la base de données a été explicitement mise hors connexion.<br /><br /> RESTORING : la restauration de la base de données a démarré.<br /><br /> RECOVERING : la récupération de la base de données a démarré et cette-dernière n’est pas encore prête pour les requêtes.<br /><br /> SUSPECT : la base de données n’a pas été récupérée.<br /><br /> EMERGENCY : la base de données se trouve dans un état d’urgence en lecture seule. L'accès est limité aux membres sysadmin.<br /><br /> Type de données de base : **nvarchar(128)**|  
|Updateability|Indique si les données peuvent être modifiées.|READ_ONLY : la base de données prend en charge les opérations de lecture de données, mais pas les modifications de données.<br /><br /> READ_WRITE : la base de données prend en charge les opérations de lecture et les modifications de données.<br /><br /> Type de données de base : **nvarchar(128)**|  
|UserAccess|Définit les utilisateurs autorisés à accéder à la base de données.|SINGLE_USER : un seul utilisateur db_owner, dbcreator ou sysadmin à la fois<br /><br /> RESTRICTED_USER : uniquement les membres des rôles db_owner, dbcreator ou sysadmin<br /><br /> MULTI_USER : tous les utilisateurs<br /><br /> Type de données de base : **nvarchar(128)**|  
|Version|Numéro de version interne du code [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] avec lequel la base de données a été créée. [!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|Numéro de version : la base de données est ouverte.<br /><br /> NULL : la base de données n’a pas démarré.<br /><br /> Type de données de base : **int**| 

<br/>   

> [!NOTE]  
> <sup>1</sup> Pour les bases de données qui font partie d’un groupe de disponibilité, `LastGoodCheckDbTime` retourne la date et l’heure de la dernière exécution réussie de DBCC CHECKDB sur le réplica principal, quel que soit le réplica à partir duquel vous avez exécuté la commande. 

## <a name="return-types"></a>Types de retour
**sql_variant**
  
## <a name="exceptions"></a>Exceptions  
Retourne NULL en cas d’erreur ou si un appelant n’est pas autorisé à voir l’objet.
  
Dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], un utilisateur peut voir uniquement les métadonnées des éléments sécurisables qui lui appartiennent ou pour lesquels il dispose d'un droit d'accès. Cela signifie que les fonctions intégrées générant des métadonnées, comme `OBJECT_ID`, peuvent retourner NULL si l’utilisateur ne dispose d’aucune autorisation sur l’objet. Pour plus d’informations, consultez [Configuration de la visibilité des métadonnées](../../relational-databases/security/metadata-visibility-configuration.md).
  
## <a name="remarks"></a>Notes  
`DATABASEPROPERTYEX` retourne un seul paramètre de propriété à la fois. Pour afficher plusieurs paramètres de propriété, utilisez la vue de catalogue [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md).
  
## <a name="examples"></a>Exemples  
  
### <a name="a-retrieving-the-status-of-the-auto_shrink-database-option"></a>R. Récupération de l'état de l'option de base de données AUTO_SHRINK  
Cet exemple retourne l’état de l’option de base de données AUTO_SHRINK pour la base de données `AdventureWorks`.
  
```sql
SELECT DATABASEPROPERTYEX('AdventureWorks2014', 'IsAutoShrink');  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)] Cela indique que la base de données AUTO_SHRINK est désactivée.
  
```
------------------  
0  
```  
  
### <a name="b-retrieving-the-default-collation-for-a-database"></a>B. Récupération du classement par défaut d'une base de données  
Cet exemple retourne plusieurs attributs de la base de données `AdventureWorks`.
  
```sql
SELECT   
    DATABASEPROPERTYEX('AdventureWorks2014', 'Collation') AS Collation,  
    DATABASEPROPERTYEX('AdventureWorks2014', 'Edition') AS Edition,  
    DATABASEPROPERTYEX('AdventureWorks2014', 'ServiceObjective') AS ServiceObjective,  
    DATABASEPROPERTYEX('AdventureWorks2014', 'MaxSizeInBytes') AS MaxSizeInBytes  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```
Collation                     Edition        ServiceObjective  MaxSizeInBytes  
----------------------------  -------------  ----------------  --------------  
SQL_Latin1_General_CP1_CI_AS  DataWarehouse  DW1000            5368709120  
```  
  
## <a name="see-also"></a>Voir aussi
[ALTER DATABASE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql.md)  
[États d’une base de données](../../relational-databases/databases/database-states.md)  
[sys.databases &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md)  
[sys.database_files &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-database-files-transact-sql.md)  
[SERVERPROPERTY &#40;Transact-SQL&#41;](../../t-sql/functions/serverproperty-transact-sql.md)
  
  
