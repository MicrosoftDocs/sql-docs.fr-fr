---
description: ALTER TABLE index_option (Transact-SQL)
title: " | Microsoft Docs"
ms.custom: ''
ms.date: 06/26/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
dev_langs:
- TSQL
helpviewer_keywords:
- index_option
ms.assetid: 8a14f12d-2fbf-4036-b8b2-8db3354e0eb7
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 1df9583fb18c87693074769b099151281c6136f8
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/01/2021
ms.locfileid: "99237063"
---
# <a name="alter-table-index_option-transact-sql"></a>ALTER TABLE index_option (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Spécifie un ensemble d’options applicables à un index qui fait partie d’une définition de contrainte créée à l’aide d’[ALTER TABLE](../../t-sql/statements/alter-table-transact-sql.md).  
  
 ![Icône du lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql
{   
    PAD_INDEX = { ON | OFF }  
  | FILLFACTOR = fillfactor  
  | IGNORE_DUP_KEY = { ON | OFF }  
  | STATISTICS_NORECOMPUTE = { ON | OFF }  
  | ALLOW_ROW_LOCKS = { ON | OFF }  
  | ALLOW_PAGE_LOCKS = { ON | OFF } 
  | OPTIMIZE_FOR_SEQUENTIAL_KEY = { ON | OFF } 
  | SORT_IN_TEMPDB = { ON | OFF }   
  | ONLINE = { ON | OFF }  
  | MAXDOP = max_degree_of_parallelism  
  | DATA_COMPRESSION = { NONE |ROW | PAGE | COLUMNSTORE | COLUMNSTORE_ARCHIVE }  
      [ ON PARTITIONS ( { <partition_number_expression> | <range> }   
      [ , ...n ] ) ]  
  | ONLINE = { ON [ ( <low_priority_lock_wait> ) ] | OFF }  
}  
  
<range> ::=   
<partition_number_expression> TO <partition_number_expression>  
  
<single_partition_rebuild__option> ::=  
{  
    SORT_IN_TEMPDB = { ON | OFF }  
  | MAXDOP = max_degree_of_parallelism  
  | DATA_COMPRESSION = {NONE | ROW | PAGE | COLUMNSTORE | COLUMNSTORE_ARCHIVE } }  
  | ONLINE = { ON [ ( <low_priority_lock_wait> ) ] | OFF }  
}  
  
<low_priority_lock_wait>::=  
{  
    WAIT_AT_LOW_PRIORITY ( MAX_DURATION = <time> [ MINUTES ] ,   
                           ABORT_AFTER_WAIT = { NONE | SELF | BLOCKERS } )   
}  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments
 PAD_INDEX **=** { ON | **OFF** }  
 **S’applique à** : [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] et versions ultérieures.  
  
 Spécifie le remplissage de l'index. La valeur par défaut est OFF.  
  
 ACTIVÉ  
 Le pourcentage d'espace libre indiqué par FILLFACTOR est appliqué aux pages de niveau intermédiaire de l'index.  
  
 OFF ou *fillfactor* n’est pas spécifié  
 Les pages de niveau intermédiaire sont remplies jusqu'à la presque totalité de la capacité, laissant suffisamment d'espace pour au moins une ligne de la taille maximale possible de l'index, compte tenu du jeu de clés des pages intermédiaires.  
  
 FILLFACTOR **=** _fillfactor_  
 **S’applique à** : [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] et versions ultérieures.  
  
 Spécifie un pourcentage indiquant le taux de remplissage appliqué par le [!INCLUDE[ssDE](../../includes/ssde-md.md)] au niveau feuille de chaque page d'index lors de la création ou de la modification de l'index. La valeur spécifiée doit être un entier compris entre 1 et 100. La valeur par défaut est 0.  
  
> [!NOTE]  
>  Les facteurs de remplissage de valeur 0 et 100 sont en tout point identiques.  
  
 IGNORE_DUP_KEY **=** { ON | **OFF** }  
 Spécifie le type de réponse quand une opération d’insertion essaie d’insérer des valeurs de clés en double dans un index unique. L'option IGNORE_DUP_KEY s'applique uniquement aux opérations d'insertion après la création ou la régénération de l'index. Cette option n’a aucun effet lors de l’exécution de [CREATE INDEX](../../t-sql/statements/create-index-transact-sql.md), d’[ALTER INDEX](../../t-sql/statements/alter-index-transact-sql.md) ou d’[UPDATE](../../t-sql/queries/update-transact-sql.md). La valeur par défaut est OFF.  
  
 ACTIVÉ  
 Un message d’avertissement s’affiche quand des valeurs de clé en double sont insérées dans un index unique. Seules les lignes qui violent la contrainte d’unicité échouent.  
  
 OFF  
 Une erreur s’affiche quand des valeurs de clé en double sont insérées dans un index unique. L’intégralité de l’opération INSERT est restaurée.  
  
 IGNORE_DUP_KEY ne peut pas être activé (ON) dans le cas d’index créés sur une vue, d’index non uniques, d’index XML, d’index spatiaux et d’index filtrés.  
  
 Pour afficher IGNORE_DUP_KEY, utilisez [sys.indexes](../../relational-databases/system-catalog-views/sys-indexes-transact-sql.md).  
  
 Dans la syntaxe de compatibilité descendante, WITH IGNORE_DUP_KEY est équivalent à WITH IGNORE_DUP_KEY = ON.  
  
 STATISTICS_NORECOMPUTE **=** { ON | **OFF** }  
 Indique si les statistiques sont recalculées. La valeur par défaut est OFF.  
  
 ACTIVÉ  
 Les statistiques obsolètes ne sont pas recalculées automatiquement.  
  
 OFF  
 La mise à jour automatique des statistiques est activée.  
  
 ALLOW_ROW_LOCKS **=** { **ON** | OFF }  
 **S’applique à** : [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] et versions ultérieures.  
  
 Indique si les verrous de ligne sont autorisés ou non. La valeur par défaut est ON.  
  
 ACTIVÉ  
 Les verrous de ligne sont autorisés lors de l'accès à l'index. Le [!INCLUDE[ssDE](../../includes/ssde-md.md)] détermine le moment où les verrous de ligne sont utilisés.  
  
 OFF  
 Les verrous de ligne ne sont pas utilisés.  
  
 ALLOW_PAGE_LOCKS **=** { **ON** | OFF }  
 **S’applique à** : [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] et versions ultérieures.  
  
 Indique si les verrous de page sont autorisés. La valeur par défaut est ON.  
  
 ACTIVÉ  
 Les verrous de page sont autorisés lors de l'accès à l'index. Le [!INCLUDE[ssDE](../../includes/ssde-md.md)] détermine le moment où les verrous de page sont utilisés.  
  
 OFF  
 Les verrous de page ne sont pas utilisés.  

 OPTIMIZE_FOR_SEQUENTIAL_KEY = {ON | **OFF** }

**S’applique à** : [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)] et versions ultérieures.

Spécifie s’il faut optimiser ou pas la contention d’insertion de la dernière page. La valeur par défaut est OFF. Consultez la section [clés séquentielles](./create-index-transact-sql.md#sequential-keys) de la page CREATE INDEX pour plus d’informations.
 
 SORT_IN_TEMPDB **=** { ON | **OFF** }  
 **S’applique à** : [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] et versions ultérieures.  
  
 Indique si les résultats du tri doivent être stockés dans **tempdb**. La valeur par défaut est OFF.  
  
 ACTIVÉ  
 Les résultats intermédiaires du tri utilisés pour créer l’index sont stockés dans **tempdb**. Cela peut réduire le temps nécessaire pour créer un index si **tempdb** ne se trouve pas sur le même groupe de disques que la base de données utilisateur. Toutefois, une plus grande quantité d'espace disque est alors utilisée lors de la création de l'index.  
  
 OFF  
 Les résultats de tri intermédiaires sont stockés dans la même base de données que l'index.  
  
 ONLINE **=** { ON | **OFF** }  
 **S’applique à** : [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] et versions ultérieures.  
  
 Indique si les tables sous-jacentes et les index associés sont disponibles pour les requêtes et la modification de données pendant l'opération d'index. La valeur par défaut est OFF. REBUILD peut être effectué en tant qu'opération ONLINE.  
  
> [!NOTE]  
>  Les index uniques non cluster ne peuvent pas être créés en ligne. Il s'agit des index qui sont créés en raison d'une contrainte UNIQUE ou PRIMARY KEY.  
  
 ACTIVÉ  
 Les verrous de table à long terme ne sont pas maintenus pendant la durée de l'opération d'index. Lors de la principale phase de l'indexation, seul le verrou de partage intentionnel (IS, Intent Share) est maintenu sur la table source. Ceci permet d'exécuter les requêtes ou les mises à jour dans la table sous-jacente et ses index. Au début de l'opération, un verrou partagé (S, Shared) est placé sur l'objet source pendant une période de temps très courte. À la fin de l'opération, pendant une période de temps très courte, un verrou partagé (S, Shared) est placé sur la source si un index non cluster est créé, ou bien un verrou de SCH-M (Modification du schéma) est placé lorsqu'un index cluster est créé ou supprimé en ligne et lorsqu'un index cluster ou non cluster est régénéré. Bien que les verrous d'index en ligne soient des verrous de métadonnées courtes, le verrou Sch-M doit notamment attendre que toutes les transactions bloquantes soient terminées sur cette table. Pendant le temps d'attente, le verrou Sch-M bloque toutes les autres transactions qui attendent derrière ce verrou en cas d'accès à la même table. ONLINE ne peut pas prendre la valeur ON si un index est en cours de création sur une table locale temporaire.  
  
> [!NOTE]  
>  La reconstruction d’index en ligne peut définir les options *low_priority_lock_wait* décrites plus loin dans cette section. *low_priority_lock_wait* gère la priorité du verrou S ou Sch-M pendant la reconstruction d’index en ligne.  
  
 OFF  
 Des verrous de table sont appliqués pendant l'opération d'indexation. Cela empêche tous les utilisateurs d'accéder à la table sous-jacente pendant la durée de l'opération. Une opération d'indexation hors ligne qui crée, régénère ou supprime un index cluster, ou régénère ou supprime un index non cluster, acquiert un verrou de modification de schéma (Sch-M) sur la table. Cela empêche tous les utilisateurs d'accéder à la table sous-jacente pendant la durée de l'opération. Une opération d'indexation hors ligne qui crée un index non cluster acquiert un verrou partagé (S, Shared) sur la table. Cela empêche la mise à jour de la table sous-jacente, mais autorise les opérations de lecture, telles que des instructions SELECT.  
  
 Pour plus d’informations, consultez [Fonctionnement des opérations d’index en ligne](../../relational-databases/indexes/how-online-index-operations-work.md).  
  
> [!NOTE]
>  Les opérations d’index en ligne ne sont pas disponibles dans toutes les éditions de [!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Pour obtenir la liste des fonctionnalités prises en charge par les éditions de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], consultez [Fonctionnalités prise en charge par les éditions de SQL Server 2016](~/sql-server/editions-and-supported-features-for-sql-server-2016.md).  
  
 MAXDOP **=** _max_degree_of_parallelism_  
 **S’applique à** : [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] et versions ultérieures.  
  
 Remplace l’option de configuration **max degree of parallelism** pendant la durée de l’opération d’index. Pour plus d’informations, consultez [Configurer l’option de configuration du serveur max degree of parallelism](../../database-engine/configure-windows/configure-the-max-degree-of-parallelism-server-configuration-option.md). Utilisez MAXDOP pour limiter le nombre de processeurs utilisés dans une exécution de plan parallèle. Le nombre maximal de processeurs est égal à 64.  
  
 *max_degree_of_parallelism* peut avoir la valeur :  
  
 - 1 - Supprime la génération de plans parallèles.  
 - \>1 - Limite au nombre spécifié le nombre maximal de processeurs utilisés dans une opération d’index parallèle.  
 - 0 (valeur par défaut) - Utilise le nombre réel de processeurs ou un nombre de processeurs inférieur en fonction de la charge de travail actuelle du système.  
  
 Pour plus d’informations, consultez [Configurer des opérations d’index parallèles](../../relational-databases/indexes/configure-parallel-index-operations.md).  
  
> [!NOTE]
>  Les opérations d’index parallèles ne sont pas disponibles dans toutes les éditions de [!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Pour obtenir la liste des fonctionnalités prises en charge par les éditions de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], consultez [Fonctionnalités prise en charge par les éditions de SQL Server 2016](~/sql-server/editions-and-supported-features-for-sql-server-2016.md).  
  
 DATA_COMPRESSION  
 **S’applique à** : [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] et versions ultérieures.  
  
 Spécifie l'option de compression de données pour la table, le numéro de partition ou la plage de partitions spécifiés. Les options disponibles sont les suivantes :  
  
 Aucune  
 La table ou les partitions spécifiées ne sont pas compressées. S'applique uniquement aux tables rowstore ; ne s'applique pas aux tables columnstore.  
  
 ROW  
 La table ou les partitions spécifiées sont compressées au moyen de la compression de ligne. S'applique uniquement aux tables rowstore ; ne s'applique pas aux tables columnstore.  
  
 PAGE  
 La table ou les partitions spécifiées sont compressées au moyen de la compression de page. S'applique uniquement aux tables rowstore ; ne s'applique pas aux tables columnstore.  
  
 COLUMNSTORE  
 **S’applique à** : [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] et versions ultérieures.  
  
 S'applique uniquement aux tables columnstore. COLUMNSTORE spécifie qu'il faut décompresser une partition compressée à l'aide de l'option COLUMNSTORE_ARCHIVE. Quand les données sont restaurées, l’index COLUMNSTORE continue à être compressé à l’aide de la compression de columnstore utilisée pour toutes les tables columnstore.  
  
 COLUMNSTORE_ARCHIVE  
 **S’applique à** : [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] et versions ultérieures.  
  
 S'applique uniquement aux tables columnstore, qui sont des tables stockées avec un index cluster columnstore. COLUMNSTORE_ARCHIVE compresse davantage la partition spécifiée en une plus petite taille. Peut être utilisé pour l'archivage, ou d'autres situations qui nécessitent moins de stockage et supportent plus de temps pour le stockage et la récupération.  
  
 Pour plus d’informations sur la compression, consultez [Compression des données](../../relational-databases/data-compression/data-compression.md).  
  
ON PARTITIONS **(** {\<partition_number_expression> | \<range>} [ **,** ...*n*] **)** **S’applique à**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] et versions ultérieures.  
  
 Spécifie les partitions auxquelles le paramètre DATA_COMPRESSION s'applique. Si la table n’est pas partitionnée, l’argument ON PARTITIONS génère une erreur. Si la clause ON PARTITIONS n’est pas fournie, l’option DATA_COMPRESSION s’applique à toutes les partitions d’une table partitionnée.  
  
\<partition_number_expression> peut être spécifié des manières suivantes :  
  
-   Spécifiez le numéro d'une partition, par exemple : ON PARTITIONS (2).  
-   Spécifiez des numéros de partition pour plusieurs partitions individuelles séparées par des virgules, par exemple : ON PARTITIONS (1, 5).  
-   Spécifiez à la fois des plages et des partitions individuelles, par exemple : ON PARTITIONS (2, 4, 6 TO 8).  
  
\<range> peut être spécifié sous la forme de numéros de partitions séparés par le mot TO, par exemple : ON PARTITIONS (6 TO 8).  
  
 Pour définir des types différents de compression de données pour des partitions différentes, spécifiez plusieurs fois l'option DATA_COMPRESSION, par exemple :  
  
```sql  
--For rowstore tables  
REBUILD WITH   
(  
DATA_COMPRESSION = NONE ON PARTITIONS (1),   
DATA_COMPRESSION = ROW ON PARTITIONS (2, 4, 6 TO 8),   
DATA_COMPRESSION = PAGE ON PARTITIONS (3, 5)  
)  
  
--For columnstore tables  
REBUILD WITH   
(  
DATA_COMPRESSION = COLUMNSTORE ON PARTITIONS (1, 3, 5),   
DATA_COMPRESSION = COLUMNSTORE_ARCHIVE ON PARTITIONS (2, 4, 6 TO 8)  
)  
```  
  
**\<single_partition_rebuild__option>**  
 Dans la plupart des cas, la reconstruction d'un index reconstruit toutes les partitions d'un index partitionné. Les options suivantes, lorsqu'elles sont appliquées à une partition unique, ne reconstruisent pas toutes les partitions.  
  
-   SORT_IN_TEMPDB  
-   MAXDOP  
-   DATA_COMPRESSION  
  
**low_priority_lock_wait**  
 **S’applique à** : [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] et versions ultérieures.  
  
 Un **SWITCH** ou une reconstruction d’index en ligne se termine dès qu’il n’y a aucune opération bloquante pour cette table. *WAIT_AT_LOW_PRIORITY* indique que si l’opération **SWITCH** ou l’opération de reconstruction d’index en ligne ne peut pas être fermée immédiatement, elle attend. L’opération contient des verrous de faible priorité, ce qui permet à d’autres opérations qui contiennent des verrous en conflit avec les instructions DDL de continuer. L’omission de l’option **WAIT AT LOW PRIORITY** équivaut à `WAIT_AT_LOW_PRIORITY ( MAX_DURATION = 0 minutes, ABORT_AFTER_WAIT = NONE)`.  
  
MAX_DURATION = *time* [**MINUTES** ]  
 Temps d’attente (valeur entière spécifiée en minutes) que le **SWITCH** ou le verrou de reconstruction d’index en ligne qui doit être acquis attend lors de l’exécution de la commande DDL. Le SWITCH ou l'opération de reconstruction de l'index en ligne tente de terminer immédiatement. Si l’opération est bloquée pendant la durée **MAX_DURATION**, l’une des actions **ABORT_AFTER_WAIT** est exécutée. La durée **MAX_DURATION** est toujours spécifiée en minutes, et le mot **MINUTES** peut être omis.  
  
ABORT_AFTER_WAIT = [**NONE** | **SELF** | **BLOCKERS** } ]  
 Aucune  
 Continue le **SWITCH** ou l’opération de reconstruction d’index en ligne sans changer la priorité du verrou (avec une priorité normale).  
  
SELF  
 Quitte le **SWITCH** ou l’opération DDL de reconstruction d’index en ligne actuellement exécuté sans effectuer aucune action.  
  
BLOCKERS  
 Arrête toutes les transactions utilisateur qui bloquent actuellement le **SWITCH** ou l’opération DDL de reconstruction d’index en ligne afin que l’opération puisse continuer.  
 BLOCKERS nécessite l’autorisation **ALTER ANY CONNECTION**.  
  
## <a name="remarks"></a>Notes  
 Pour obtenir une description complète des options d’index, consultez [CREATE INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/create-index-transact-sql.md).  
  
## <a name="see-also"></a>Voir aussi  
 [ALTER TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-table-transact-sql.md)   
 [column_constraint &#40;Transact-SQL&#41;](../../t-sql/statements/alter-table-column-constraint-transact-sql.md)   
 [computed_column_definition &#40;Transact-SQL&#41;](../../t-sql/statements/alter-table-computed-column-definition-transact-sql.md)   
 [table_constraint &#40;Transact-SQL&#41;](../../t-sql/statements/alter-table-table-constraint-transact-sql.md)  
  
 
