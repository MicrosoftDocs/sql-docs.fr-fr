---
description: sp_fulltext_table (Transact-SQL)
title: sp_fulltext_table (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine, sql-data-warehouse
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_fulltext_table_TSQL
- sp_fulltext_table
dev_langs:
- TSQL
helpviewer_keywords:
- sp_fulltext_table
ms.assetid: a765f311-07fc-4af3-b74c-e9a027fbecce
author: markingmyname
ms.author: maghan
monikerRange: =azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 18f807181005e20e94a15a8c4c95b43e82ecbb44
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99211331"
---
# <a name="sp_fulltext_table-transact-sql"></a>sp_fulltext_table (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-asdw-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-asdw-xxx-md.md)]

  Active ou désactive l'indexation de texte intégral pour une table.  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)] Utilisez plutôt [CREATE FULLTEXT INDEX](../../t-sql/statements/create-fulltext-index-transact-sql.md), [ALTER FULLTEXT INDEX](../../t-sql/statements/alter-fulltext-index-transact-sql.md)et [DROP FULLTEXT INDEX](../../t-sql/statements/drop-fulltext-index-transact-sql.md) .  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_fulltext_table   
   [ @tabname= ] 'qualified_table_name'           
   , [ @action= ] 'action'   
   [   
   , [ @ftcat= ] 'fulltext_catalog_name'           
   , [ @keyname= ] 'unique_index_name'   
   ]  
```  
  
## <a name="arguments"></a>Arguments  
`[ @tabname = ] 'qualified_table_name'` Est un nom de table en une ou deux parties. La table doit déjà exister dans la base de données actuelle. *qualified_table_name* est de type **nvarchar (517)**, sans valeur par défaut.  
  
`[ @action = ] 'action'` Action à exécuter. *action* est de type **nvarchar (50)**, sans valeur par défaut et peut prendre l’une des valeurs suivantes.  
  
|Valeur|Description|  
|-----------|-----------------|  
|**Créer**|Crée les métadonnées pour un index de recherche en texte intégral pour la table référencée par *qualified_table_name* et spécifie que les données de l’index de recherche en texte intégral pour cette table doivent résider dans *fulltext_catalog_name*. Cette action désigne également l’utilisation de *unique_index_name* comme colonne clé de texte intégral. Cet index unique doit déjà exister et être défini sur une colonne de la table.<br /><br /> Une recherche en texte intégral ne peut pas être réalisée vis à vis de cette table tant que le catalogue de texte intégral n'est pas rempli.|  
|**Déplacez**|Supprime les métadonnées de l’index de recherche en texte intégral pour *qualified_table_name*. Si cet index est actif, il est automatiquement désactivé avant d'être supprimé. Il n'est pas nécessaire de supprimer les colonnes avant de supprimer l'index de texte intégral.|  
|**Activer**|Active la possibilité de collecter des données d’index de recherche en texte intégral pour *qualified_table_name*, après qu’elles ont été désactivées. Une colonne au moins doit faire partie de l'index de texte intégral pour pouvoir activer cette option.<br /><br /> Un index de texte intégral est automatiquement activé (au niveau du remplissage) dès l'ajout de la première colonne à indexer. Si la dernière colonne est supprimée de l'index, celui-ci devient inactif. Si le suivi des modifications est activé, l'activation d'un index inactif démarre un nouveau remplissage.<br /><br /> Notez que cela ne remplit pas réellement l’index de recherche en texte intégral, mais inscrit simplement la table dans le catalogue de texte intégral dans le système de fichiers afin que les lignes de *qualified_table_name* puissent être récupérées lors du remplissage de l’index de recherche en texte intégral suivant.|  
|**Désactivation**|Désactive l’index de recherche en texte intégral pour *qualified_table_name* afin que les données d’index de recherche en texte intégral ne puissent plus être collectées pour le *qualified_table_name*. Les métadonnées de l'index de texte intégral sont conservées et la table peut être réactivée.<br /><br /> Si le suivi des modifications est activé, la désactivation d'un index actif gèle l'état de l'index : tout remplissage en cours est arrêté et aucune modification supplémentaire n'est diffusée à l'index.|  
|**start_change_tracking**|Démarre un remplissage incrémentiel de l'index de texte intégral. Si la table ne dispose pas de données d'une colonne de type timestamp, démarre un remplissage complet de l'index de texte intégral. Démarre le suivi des modifications apportées à la table.<br /><br /> Le suivi des modifications de texte intégral n’effectue pas le suivi des opérations WRITETEXT ou UPDATETEXT effectuées sur des colonnes indexées en texte intégral qui sont de type **image**, **Text** ou **ntext**.|  
|**stop_change_tracking**|Arrête le suivi des modifications apportées à la table.|  
|**update_index**|Diffuse le jeu courant des modifications suivies à l'index de texte intégral.|  
|**start_background_updateindex**|Démarre la diffusion instantanée des modifications suivies à l'index de texte intégral.|  
|**stop_background_updateindex**|Arrête la diffusion instantanée des modifications suivies à l'index de texte intégral.|  
|**start_full**|Démarre un remplissage complet de l'index de texte intégral de la table.|  
|**start_incremental**|Démarre un remplissage incrémentiel de l'index de texte intégral de la table.|  
|**Stop**|Arrête un remplissage complet ou incrémentiel.|  
  
`[ @ftcat = ] 'fulltext_catalog_name'` Nom de catalogue de texte intégral valide et existant pour une action **Create** . Pour toutes les autres actions, ce paramètre doit avoir la valeur NULL. *fulltext_catalog_name* est de **type sysname**, avec NULL comme valeur par défaut.  
  
`[ @keyname = ] 'unique_index_name'` Est un index non null de colonne unique valide sur *qualified_table_name* pour une action **Create** . Pour toutes les autres actions, ce paramètre doit avoir la valeur NULL. *unique_index_name* est de **type sysname**, avec NULL comme valeur par défaut.  
  
## <a name="return-code-values"></a>Codet de retour  
 0 (réussite) ou 1 (échec)  
  
## <a name="result-sets"></a>Jeux de résultats  
 None  
  
## <a name="remarks"></a>Notes  
 Après la désactivation d’un index de recherche en texte intégral pour une table particulière, l’index de recherche en texte intégral existant reste en place jusqu’à la prochaine alimentation complète. Toutefois, cet index n’est pas utilisé car [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] bloque les requêtes sur les tables désactivées.  
  
 Si la table est réactivée et que l'index n'a pas été rempli à nouveau, l'ancien index est toujours disponible pour les requêtes sur les colonnes qui permettent la recherche en texte intégral et qui existent déjà, à l'exception des nouvelles colonnes. Les données des colonnes supprimées sont récupérables par les requêtes dans lesquelles une recherche en texte intégral sur toute une colonne est spécifiée.  
  
 Une fois qu’une table a été définie pour l’indexation de texte intégral, basculement de la colonne clé unique de texte intégral d’un type de données vers un autre, soit en modifiant le type de données de cette colonne, soit en remplaçant la clé unique de texte intégral d’une colonne par une autre, sans repeuplement complet peut entraîner un échec lors d’une requête ultérieure et en renvoyant le *message* d’erreur : «la conversion en type data_type *a* échoué pour la valeur key_value Pour éviter cela, supprimez la définition de texte intégral pour cette table à l’aide de l’action **Drop** de **sp_fulltext_table** et redéfinissez-la à l’aide de **sp_fulltext_table** et **sp_fulltext_column**.  
  
 La taille de la colonne clé de texte intégral doit être de 900 octets ou moins. Pour des raisons de performances, il est recommandé de limiter au minimum la taille de la colonne clé.  
  
## <a name="permissions"></a>Autorisations  
 Seuls les membres du rôle serveur fixe **sysadmin** , **db_owner** et les rôles de base de données fixes **db_ddladmin** , ou un utilisateur disposant d’autorisations de référence sur le catalogue de texte intégral peuvent exécuter **sp_fulltext_table**.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-enabling-a-table-for-full-text-indexing"></a>R. Activation d'une table pour l'indexation de texte intégral  
 L'exemple ci-dessous crée des métadonnées d'index de texte intégral pour la table `Document` de la base de données `AdventureWorks`. `Cat_Desc` est un catalogue de texte intégral. `PK_Document_DocumentID` est un index à colonne unique sur `Document`.  
  
```  
USE AdventureWorks2012;  
GO  
EXEC sp_fulltext_table 'Production.Document', 'create', 'Cat_Desc', 'PK_Document_DocumentID';  
--Add some columns  
EXEC sp_fulltext_column 'Production.Document','DocumentSummary','add';  
-- Activate the full-text index  
EXEC sp_fulltext_table 'Production.Document','activate';  
GO  
```  
  
### <a name="b-activating-and-propagating-track-changes"></a>B. Activation et propagation du suivi des modifications  
 L'exemple ci-dessous active et commence la propagation des modifications suivies à l'index de texte intégral.  
  
```  
USE AdventureWorks2012;  
GO  
EXEC sp_fulltext_table 'Production.Document', 'Start_change_tracking';  
EXEC sp_fulltext_table 'Production.Document', 'Start_background_updateindex';  
GO  
```  
  
### <a name="c-removing-a-full-text-index"></a>C. Suppression d'un index de texte intégral  
 L'exemple ci-dessous supprime les métadonnées d'index de texte intégral pour la table `Document` de la base de données `AdventureWorks`.  
  
```  
USE AdventureWorks2012;  
GO  
EXEC sp_fulltext_table 'Production.Document', 'drop';  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [INDEXPROPERTY &#40;Transact-SQL&#41;](../../t-sql/functions/indexproperty-transact-sql.md)   
 [OBJECTPROPERTY &#40;Transact-SQL&#41;](../../t-sql/functions/objectproperty-transact-sql.md)   
 [sp_help_fulltext_tables &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-help-fulltext-tables-transact-sql.md)   
 [sp_help_fulltext_tables_cursor &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-help-fulltext-tables-cursor-transact-sql.md)   
 [sp_helpindex &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helpindex-transact-sql.md)   
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)   
 [Procédures stockées de recherche en texte intégral et de recherche sémantique &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/full-text-search-and-semantic-search-stored-procedures-transact-sql.md)  
  
  
