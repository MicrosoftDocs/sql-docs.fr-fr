---
description: ALTER INDEX (Transact-SQL)
title: ALTER INDEX (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/21/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- ALTER INDEX
- ALTER_INDEX_TSQL
dev_langs:
- t-sql
helpviewer_keywords:
- indexes [SQL Server], reorganizing
- ALTER INDEX statement
- indexes [SQL Server], disabling
- online index operations
- index reorganization [SQL Server]
- ALLOW_ROW_LOCKS option
- ALL keyword
- reorganizing indexes
- constraints [SQL Server], indexes
- row locks [SQL Server]
- index rebuilding [SQL Server]
- rebuilding indexes
- locking [SQL Server], indexes
- partitioned indexes [SQL Server], rebuilding
- defragmenting indexes
- disabling indexes
- XML indexes [SQL Server], modifying
- index modifications [SQL Server]
- indexes [SQL Server], modifying
- index options [SQL Server]
- modifying indexes
- index disabling [SQL Server]
- MAXDOP index option, ALTER INDEX statement
- spatial indexes [SQL Server], modifying
- indexes [SQL Server], options
- ALLOW_PAGE_LOCKS option
- page locks [SQL Server]
- index rebuild [SQL Server]
- index reorganize [SQL Server]
ms.assetid: b796c829-ef3a-405c-a784-48286d4fb2b9
author: pmasl
ms.author: pelopes
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 2ec8737d01b52db828dbe95cd1252d8913128c90
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100339308"
---
# <a name="alter-index-transact-sql"></a>ALTER INDEX (Transact-SQL)

[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Modifie une table ou un index d’affichage (rowstore, columnstore ou XML) existant en désactivant, en régénérant ou en réorganisant l’index d’une part, ou en définissant les options portant sur l’index d’autre part.  
  
 ![Icône du lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql
-- Syntax for SQL Server and Azure SQL Database
  
ALTER INDEX { index_name | ALL } ON <object>  
{  
      REBUILD {  
            [ PARTITION = ALL ] [ WITH ( <rebuild_index_option> [ ,...n ] ) ]   
          | [ PARTITION = partition_number [ WITH ( <single_partition_rebuild_index_option> ) [ ,...n ] ]  
      }  
    | DISABLE  
    | REORGANIZE  [ PARTITION = partition_number ] [ WITH ( <reorganize_option>  ) ]  
    | SET ( <set_index_option> [ ,...n ] )   
    | RESUME [WITH (<resumable_index_options>,[...n])]
    | PAUSE
    | ABORT
}  
[ ; ]  
  
<object> ::=   
{  
    { database_name.schema_name.table_or_view_name | schema_name.table_or_view_name | table_or_view_name }  
}  
  
<rebuild_index_option > ::=  
{  
      PAD_INDEX = { ON | OFF }  
    | FILLFACTOR = fillfactor   
    | SORT_IN_TEMPDB = { ON | OFF }  
    | IGNORE_DUP_KEY = { ON | OFF }  
    | STATISTICS_NORECOMPUTE = { ON | OFF }  
    | STATISTICS_INCREMENTAL = { ON | OFF }  
    | ONLINE = {   
          ON [ ( <low_priority_lock_wait> ) ]   
        | OFF } 
    | RESUMABLE = { ON | OFF } 
    | MAX_DURATION = <time> [MINUTES}     
    | ALLOW_ROW_LOCKS = { ON | OFF }  
    | ALLOW_PAGE_LOCKS = { ON | OFF }  
    | MAXDOP = max_degree_of_parallelism  
    | DATA_COMPRESSION = { NONE | ROW | PAGE | COLUMNSTORE | COLUMNSTORE_ARCHIVE }   
        [ ON PARTITIONS ( {<partition_number> [ TO <partition_number>] } [ , ...n ] ) ]  
}  
  
<single_partition_rebuild_index_option> ::=  
{  
      SORT_IN_TEMPDB = { ON | OFF }  
    | MAXDOP = max_degree_of_parallelism  
    | RESUMABLE = { ON | OFF } 
    | MAX_DURATION = <time> [MINUTES}     
    | DATA_COMPRESSION = { NONE | ROW | PAGE | COLUMNSTORE | COLUMNSTORE_ARCHIVE } }  
    | ONLINE = { ON [ ( <low_priority_lock_wait> ) ] | OFF }  
}  
  
<reorganize_option>::=  
{  
       LOB_COMPACTION = { ON | OFF }  
    |  COMPRESS_ALL_ROW_GROUPS =  { ON | OFF}  
}  
  
<set_index_option>::=  
{  
      ALLOW_ROW_LOCKS = { ON | OFF }  
    | ALLOW_PAGE_LOCKS = { ON | OFF }  
    | OPTIMIZE_FOR_SEQUENTIAL_KEY = { ON | OFF }
    | IGNORE_DUP_KEY = { ON | OFF }  
    | STATISTICS_NORECOMPUTE = { ON | OFF }  
    | COMPRESSION_DELAY= { 0 | delay [Minutes] }  
}  

<resumable_index_option> ::=
 { 
    MAXDOP = max_degree_of_parallelism
    | MAX_DURATION =<time> [MINUTES]
    | <low_priority_lock_wait>  
 }
 
<low_priority_lock_wait>::=  
{  
    WAIT_AT_LOW_PRIORITY ( MAX_DURATION = <time> [ MINUTES ] ,   
                          ABORT_AFTER_WAIT = { NONE | SELF | BLOCKERS } )  
}  

```  
  
```syntaxsql
-- Syntax for Azure Synapse Analytics and Parallel Data Warehouse 
  
ALTER INDEX { index_name | ALL }  
    ON   [ schema_name. ] table_name  
{  
      REBUILD {  
            [ PARTITION = ALL [ WITH ( <rebuild_index_option> ) ] ] 
          | [ PARTITION = partition_number [ WITH ( <single_partition_rebuild_index_option> )] ] 
      }  
    | DISABLE  
    | REORGANIZE [ PARTITION = partition_number ]  
}  
[;]  

<rebuild_index_option > ::=   
{  
    DATA_COMPRESSION = { COLUMNSTORE | COLUMNSTORE_ARCHIVE }
        [ ON PARTITIONS ( {<partition_number> [ TO <partition_number>] } [ , ...n ] ) ]   
}

<single_partition_rebuild_index_option > ::=   
{  
    DATA_COMPRESSION = { COLUMNSTORE | COLUMNSTORE_ARCHIVE }  
}  
  
```

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments

 *index_name*  
 Nom de l'index. Les noms d'index doivent être uniques dans une table ou une vue, mais ne doivent pas être nécessairement uniques dans une base de données. Les noms d’index doivent se conformer aux règles régissant les [identificateurs](../../relational-databases/databases/database-identifiers.md).  
  
 ALL  
 Indique tous les index associés à une table ou à une vue indépendamment du type d'index. L'ajout de l'option ALL provoque un échec de l'instruction si des index se trouvent dans un groupe de fichiers hors connexion ou en lecture seule ou si l'opération spécifiée n'est pas autorisée sur certains types d'index. Le tableau suivant répertorie les types d'opérations ainsi que les types d'index non autorisés.  
  
|Utilisation du mot clé ALL avec cette opération|Entraîne un échec si la table possède des|  
|----------------------------------------|----------------------------------------|  
|REBUILD WITH ONLINE = ON|Index XML<br /><br /> Index spatial<br /><br /> Index columnstore : **S’applique à :** [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (à compter de [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]) et [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].|  
|REBUILD PARTITION = *partition_number*|Index non partitionné, index XML, index spatial ou index désactivé|  
|REORGANIZE|Index pour lesquels ALLOW_PAGE_LOCKS a la valeur OFF|  
|REORGANIZE PARTITION = *partition_number*|Index non partitionné, index XML, index spatial ou index désactivé|  
|IGNORE_DUP_KEY = ON|Index XML<br /><br /> Index spatial<br /><br /> Index columnstore : **S’applique à :** [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (à compter de [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]) et [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].|  
|ONLINE = ON|Index XML<br /><br /> Index spatial<br /><br /> Index columnstore : **S’applique à :** [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (à compter de [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]) et [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].|
|RESUMABLE = ON| Les index pouvant être repris ne sont pas pris en charge avec le mot clé **All**. <br /><br /> **S’applique à** : [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (à compter de [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)]) et [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]. |   
  
> [!WARNING]
> Pour plus d’informations sur les opérations d’index pouvant être effectuées en ligne, consultez [Instructions pour les opérations d’index en ligne](../../relational-databases/indexes/guidelines-for-online-index-operations.md).

 Si ALL est spécifié avec PARTITION = *partition_number*, tous les index doivent être alignés. Ceci revient à les partitionner d'après des fonctions de partition équivalentes. L’utilisation de ALL avec PARTITION entraîne la reconstruction ou la réorganisation de toutes les partitions d’index qui ont le même *partition_number*. Pour plus d'informations sur les index partitionnés, consultez [Partitioned Tables and Indexes](../../relational-databases/partitions/partitioned-tables-and-indexes.md).  
  
 *database_name*  
 Nom de la base de données.  
  
 *schema_name*  
 Nom du schéma auquel la table ou la vue appartient.  
  
 *table_or_view_name*  
 Nom de la table ou de la vue associée à l'index. Pour afficher un rapport des index relatifs à un objet, utilisez la vue de catalogue [sys.indexes](../../relational-databases/system-catalog-views/sys-indexes-transact-sql.md).  
  
 [!INCLUDE[ssSDS](../../includes/sssds-md.md)] prend en charge le format de nom en trois parties nom_bd.[nom_schéma].nom_table_ou_vue quand nom_bd est la base de données active ou la base de données tempdb et quand nom_table_ou_vue commence par #.  
  
 REBUILD [ WITH **(** \<rebuild_index_option> [ **,** ... *n*] **)** ]  
  
**S’applique à** : [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (à compter de [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]) et [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]. 

Index à reconstruire d'après les mêmes colonnes, le même type d'index, le même attribut assurant son unicité ainsi que le même ordre de tri. Cette clause équivaut à [DBCC DBREINDEX](../../t-sql/database-console-commands/dbcc-dbreindex-transact-sql.md). REBUILD permet de réactiver un index désactivé. La reconstruction portant sur un index cluster n'entraîne pas celle des index non cluster associés, à moins que le mot clé ALL ne soit spécifié. Si les options d’index ne sont pas spécifiées, leurs valeurs existantes stockées dans [sys.indexes](../../relational-databases/system-catalog-views/sys-indexes-transact-sql.md) sont celles qui sont appliquées. Si une valeur d’option d’index n’est pas stockée dans **sys.indexes**, celle indiquée par défaut dans la définition de l’argument de l’option s’applique.  
  
 Si l'option ALL est indiquée et que la table sous-jacente correspond à un segment de mémoire, l'opération de reconstruction n'a alors aucun effet sur la table. Tous les index non cluster associés à la table sont donc reconstruits.  
  
 L'opération de reconstruction peut être consignée dans un journal au minimum si le mode de récupération de base de données est défini sur Utilisant les journaux de transactions ou sur Simple.  
  
> [!NOTE]
> Si vous reconstruisez un index XML primaire, la table utilisateur sous-jacente devient indisponible pour toute la durée de l'opération d'index.  
 
Pour les index columnstore, l’opération de reconstruction :  
  
-  N’utilise pas l’ordre de tri.  
-  Acquiert un verrou exclusif sur la table ou la partition lorsque la reconstruction se produit.  Les données sont « hors connexion » et indisponibles pendant la reconstruction, même si vous utilisez NOLOCK, RCSI ou SI.  
-  Recompresse toutes les données dans le columnstore. Il existe deux copies de l'index columnstore pendant la reconstruction. Lorsque la reconstruction est terminée, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] supprime l'index columnstore d'origine.  

Pour plus d’informations, consultez [Réorganiser et reconstruire des index](../../relational-databases/indexes/reorganize-and-rebuild-indexes.md). 
  
PARTITION  

**S’applique à** : [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (à compter de [!INCLUDE[ssKatmai](../../includes/ssKatmai-md.md)]) et [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].  
  
 Indique que seule une partition d'un index doit être reconstruite ou réorganisée. PARTITION ne peut pas être spécifié si *index_name* n’est pas un index partitionné.  
  
 PARTITION = ALL reconstruit toutes les partitions.  
  
> [!WARNING]
> La création et la reconstruction des index non alignés sur une table contenant plus de 1 000 partitions sont possibles, mais ne sont pas prises en charge. Ces opérations peuvent entraîner une dégradation des performances ou une consommation de mémoire excessive. Microsoft vous recommande d’utiliser uniquement des index alignés lorsque le nombre de partitions est supérieur à 1 000.  
  
 *partition_number*  
   
**S’applique à** : [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (à compter de [!INCLUDE[ssKatmai](../../includes/ssKatmai-md.md)]) et [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].
  
 Numéro de partition d'un index partitionné à reconstruire ou à réorganiser. *partition_number* est une expression de constante qui peut référencer des variables. Cela inclut les fonctions ou variables de types définies par l'utilisateur et les fonctions définies par l'utilisateur, mais exclut l'instruction [!INCLUDE[tsql](../../includes/tsql-md.md)]. *partition_number* doit exister, sinon l’instruction échoue.  
  
 WITH **(** \<single_partition_rebuild_index_option> **)**  
   
**S’applique à** : [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (à compter de [!INCLUDE[ssKatmai](../../includes/ssKatmai-md.md)]) et [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].  
  
 `SORT_IN_TEMPDB`, `MAXDOP` et `DATA_COMPRESSION` sont les options qui peuvent être spécifiées lors de la régénération d’une partition unique (PARTITION = *partition_number*). Les index XML ne peuvent pas être indiqués dans une opération de reconstruction d'une seule partition.  
  
 DISABLE  
 Marque l'index comme désactivé et non disponible pour être utilisé par le [!INCLUDE[ssDE](../../includes/ssde-md.md)]. Tout index peut être désactivé. La définition d'un index désactivé est conservé dans le catalogue système sans y inclure de données sous-jacentes. Désactiver un index cluster permet d'éviter l'accès aux données de la table sous-jacente par les utilisateurs. Pour activer un index, utilisez ALTER INDEX REBUILD ou CREATE INDEX WITH DROP_EXISTING. Pour plus d’informations, consultez [Désactiver les index et les contraintes](../../relational-databases/indexes/disable-indexes-and-constraints.md) et [Activer les index et les contraintes](../../relational-databases/indexes/enable-indexes-and-constraints.md).  
  
 Opération REORGANIZE sur un index **rowstore**  
 Pour les index rowstore, REORGANIZE spécifie la réorganisation du niveau feuille d’index. L’opération REORGANIZE est :  
  
-   Toujours effectuée en ligne. En d'autres termes, les verrous de tables bloquants à long terme ne sont pas conservés, ce qui permet aux requêtes et aux mises à jour de la table sous-jacente de se poursuivre pendant la transaction ALTER INDEX REORGANIZE.
-   Non autorisée sur un index désactivé.
-   Non autorisée quand ALLOW_PAGE_LOCKS est OFF.
-   Non restaurée quand elle est effectuée au sein d’une transaction qui, elle, est restaurée.

> [!NOTE]
> Quand ALTER INDEX REORGANIZE utilise des transactions explicites (par exemple, ALTER INDEX dans une instruction BEGIN TRAN ... COMMIT/ROLLBACK) au lieu du mode de transaction implicite par défaut, le comportement de verrouillage de REORGANIZE devient plus restrictif et peut entraîner un blocage. Pour plus d’informations sur les transactions implicites, consultez [SET IMPLICIT_TRANSACTIONS &#40;Transact-SQL&#41;](../../t-sql/statements/set-implicit-transactions-transact-sql.md).

Pour plus d’informations, consultez [Réorganiser et reconstruire des index](../../relational-databases/indexes/reorganize-and-rebuild-indexes.md). 

REORGANIZE WITH **(** LOB_COMPACTION = { **ON** | OFF } **)**  
 S’applique aux index rowstore.  
  
LOB_COMPACTION = ON  
  
-   Spécifie de compacter toutes les pages qui contiennent des données des types de données LOB (large object) suivants : image, text, ntext, varchar(max), nvarchar(max), varbinary(max) et xml. Le compactage de ces données peut réduire la taille des données sur le disque.  
-   Pour un index cluster, toutes les colonnes LOB qui sont contenues dans la table sont compactées.  
-   Pour un index non-cluster, toutes les colonnes LOB qui sont des colonnes non-clés (incluses) dans l’index sont compactées.  
-   REORGANIZE ALL effectue une opération LOB_COMPACTION sur tous les index. Pour chaque index, toutes les colonnes LOB de l’index cluster, la table sous-jacente ou les colonnes incluses dans un index non-cluster sont compactées.  
  
LOB_COMPACTION = OFF  
  
-   Les pages contenant des données d'objet volumineux ne sont pas compactées.  
-   La valeur OFF n'a aucun effet sur un segment de mémoire.  
  
 Opération REORGANIZE sur un index **columnstore**  
 Pour les index columnstore, REORGANIZE compresse chaque rowgroup delta CLOSED dans le columnstore en tant que rowgroup compressé. L’opération REORGANIZE s’effectue toujours en ligne. En d'autres termes, les verrous de tables bloquants à long terme ne sont pas conservés, ce qui permet aux requêtes et aux mises à jour de la table sous-jacente de se poursuivre pendant la transaction ALTER INDEX REORGANIZE. Pour plus d’informations, consultez [Réorganiser et reconstruire des index](../../relational-databases/indexes/reorganize-and-rebuild-indexes.md). 
  
-   REORGANIZE n’est pas nécessaire pour déplacer les rowgroups delta CLOSED dans les rowgroups compressés. Le moteur de tuple (TM) en arrière-plan se réveille régulièrement pour compresser les rowgroups delta CLOSED. Nous recommandons d’utiliser REORGANIZE lorsque le moteur de tuple est en retard. REORGANIZE peut compresser des rowgroups de manière plus radicale.  
-   Pour compresser tous les rowgroups OPEN et CLOSED, consultez l’option `REORGANIZE WITH (COMPRESS_ALL_ROW_GROUPS)` dans cette section.  
  
Pour les index columnstore dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (à compter de [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)]) et [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)], REORGANIZE effectue les optimisations de défragmentation supplémentaires suivantes en ligne :  
  
-   Supprime physiquement les lignes d’un rowgroup quand au moins 10 % des lignes ont été supprimées de façon logique. Les octets supprimés sont récupérés sur le support physique. Par exemple, si un rowgroup compressé de 1 million de lignes a 100 000 lignes supprimées, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] efface les lignes supprimées et recompresse le rowgroup avec 900 000 lignes. Il économise du stockage en effaçant les lignes supprimées.  
  
-   Associe un ou plusieurs rowgroups compressés pour augmenter les lignes par rowgroup jusqu’à la valeur maximale de 1 048 576 lignes. Par exemple, si vous importez en bloc 5 lots de 102 400 lignes, vous obtenez 5 rowgroups compressés. Si vous exécutez REORGANIZE, ces rowgroups sont fusionnés dans 1 rowgroup compressé de 512 000 lignes. Cela suppose qu’il n’existe aucune limitation de mémoire ni de taille de dictionnaire.  
  
-   Pour les rowgroups dans lesquels au moins 10 % des lignes ont été supprimées de manière logique, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] tente d’associer ce rowgroup à un ou plusieurs rowgroups. Par exemple, le rowgroup 1 est compressé avec 500 000 lignes et le rowgroup 21 est compressé avec un maximum de 1 048 576 lignes. Le rowgroup 21 a 60 % des lignes supprimées, ce qui laisse 409 830 lignes. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] favorise la combinaison de ces deux rowgroups pour compresser un nouveau rowgroup qui contient 909 830 lignes.  
  
REORGANIZE WITH ( COMPRESS_ALL_ROW_GROUPS = { ON | **OFF** } )  
 S’applique aux index columnstore. 

 **S’applique à :** [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (à compter de [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)]) et [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].

COMPRESS_ALL_ROW_GROUPS offre un moyen de forcer les rowgroups delta OPEN ou CLOSED dans le columnstore. Avec cette option, il n’est pas nécessaire de reconstruire l’index columnstore pour vider les rowgroups delta.  Ceci, combiné aux autres fonctionnalités de défragmentation par suppression et fusion, permet d’éviter de devoir reconstruire l’index dans la plupart des cas.    

-   ON force tous les rowgroups dans le columnstore, quels que soient la taille et l’état (CLOSE ou OPEN).  
-   OFF force tous les rowgroups CLOSED dans le columnstore.  

Pour plus d’informations, consultez [Réorganiser et reconstruire des index](../../relational-databases/indexes/reorganize-and-rebuild-indexes.md). 

SET **(** \<set_index option> [ **,** ... *n*] **)**  
 Indique des options d'index sans pour autant reconstruire ou réorganiser l'index. SET ne peut pas être spécifié pour un index désactivé.  
  
PAD_INDEX = { ON | OFF }  
   
**S’applique à** : [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (à compter de [!INCLUDE[ssKatmai](../../includes/ssKatmai-md.md)]) et [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].  

 Spécifie le remplissage de l'index. La valeur par défaut est OFF.  
  
 ACTIVÉ  
 Le pourcentage d'espace libre indiqué par FILLFACTOR est appliqué aux pages de niveau intermédiaire de l'index. Si FILLFACTOR n'est pas spécifié alors que PAD_INDEX a la valeur ON, la valeur du facteur de remplissage stocké dans [sys.indexes](../../relational-databases/system-catalog-views/sys-indexes-transact-sql.md) est utilisée.  
  
 OFF ou *fillfactor* n’est pas spécifié  
 Les pages de niveau intermédiaire sont remplies jusqu'à la presque totalité de la capacité. Cela laisse suffisamment d'espace libre pour au moins une ligne de la taille maximale que l'index peut occuper, d'après un ensemble de clés sur les pages intermédiaires.  
  
 Pour plus d’informations, consultez [CREATE INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/create-index-transact-sql.md).  
  
FILLFACTOR = *fillfactor*  
 
 **S’applique à** : [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (à compter de [!INCLUDE[ssKatmai](../../includes/ssKatmai-md.md)]) et [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].
  
 Spécifie un pourcentage indiquant le taux de remplissage appliqué par le [!INCLUDE[ssDE](../../includes/ssde-md.md)] au niveau feuille de chaque page d'index lors de la création ou de la modification de l'index. *fillfactor* doit être une valeur entière comprise entre 1 et 100. La valeur par défaut est 0. Les taux de remplissage 0 et 100 sont identiques en tous points.  
  
 Un paramètre FILLFACTOR explicite ne s'applique que lors de la première création ou reconstruction de l'index. Le [!INCLUDE[ssDE](../../includes/ssde-md.md)] ne conserve pas dynamiquement dans les pages le pourcentage d'espace libre défini. Pour plus d’informations, consultez [CREATE INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/create-index-transact-sql.md).  
  
 Pour afficher le paramètre du facteur de remplissage, utilisez **sys.indexes**.  
  
> [!IMPORTANT]
> La création ou la modification d'un index cluster avec une valeur de FILLFACTOR affecte la quantité de l'espace de stockage occupé par les données, car le [!INCLUDE[ssDE](../../includes/ssde-md.md)] redistribue les données lorsqu'il crée l'index en question.  
  
 SORT_IN_TEMPDB = { ON | **OFF** }  
 
**S’applique à** : [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (à compter de [!INCLUDE[ssKatmai](../../includes/ssKatmai-md.md)]) et [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].  
  
 Indique si les résultats du tri doivent être stockés dans **tempdb**. La valeur par défaut est OFF, à l’exception d’Azure SQL Database Hyperscale. Pour toutes les opérations de nouvelle création d’index dans Hyperscale, SORT_IN_TEMPDB est toujours ON quelle que soit l’option spécifiée, sauf si la regénération de l’index pouvant être reprise est utilisée.  
  
 ACTIVÉ  
 Les résultats intermédiaires du tri utilisés pour créer l’index sont stockés dans **tempdb**. Si **tempdb** ne se trouve pas sur le même groupe de disques que la base de données utilisateur, le temps nécessaire à la création de l’index peut être plus court. Toutefois, une plus grande quantité d'espace disque est alors utilisée lors de la création de l'index.  
  
 OFF  
 Les résultats de tri intermédiaires sont stockés dans la même base de données que l'index.  
  
 Si une opération de tri n'est pas requise ou si le tri peut s'effectuer en mémoire, l'option SORT_IN_TEMPDB est ignorée.  
  
 Pour plus d’informations, consultez [Option SORT_IN_TEMPDB pour les index](../../relational-databases/indexes/sort-in-tempdb-option-for-indexes.md).  
  
 IGNORE_DUP_KEY **=** { ON | OFF }  
 Spécifie la réponse d'erreur lorsqu'une opération d'insertion essaie d'insérer des valeurs de clés en double dans un index unique. L'option IGNORE_DUP_KEY s'applique uniquement aux opérations d'insertion après la création ou la régénération de l'index. La valeur par défaut est OFF.  
  
 ACTIVÉ  
 Un message d'avertissement s'affichera lorsque des valeurs de clé en double sont insérées dans un index unique. Seules les lignes qui violent la contrainte d'unicité échouent.  
  
 OFF  
 Un message d'erreur s'affichera lorsque des valeurs de clé en double sont insérées dans un index unique. L'intégralité de l'opération INSERT sera restaurée.  
  
 IGNORE_DUP_KEY ne peut pas être défini sur ON pour les index créés sur une vue, les index non uniques, les index XML, les index spatiaux et les index filtrés.  
  
 Pour afficher IGNORE_DUP_KEY, utilisez [sys.indexes](../../relational-databases/system-catalog-views/sys-indexes-transact-sql.md).  
  
 Dans la syntaxe de compatibilité descendante, WITH IGNORE_DUP_KEY est équivalent à WITH IGNORE_DUP_KEY = ON.  
  
 STATISTICS_NORECOMPUTE **=** { ON | OFF }  
 Spécifie si les statistiques de distribution sont recalculées. La valeur par défaut est OFF.  
  
 ACTIVÉ  
 Les statistiques obsolètes ne sont pas recalculées automatiquement.  
  
 OFF  
 La mise à jour automatique des statistiques est activée.  
  
 Pour restaurer la mise à jour automatique des statistiques, affectez la valeur OFF à STATISTICS_NORECOMPUTE ou exécutez UPDATE STATISTICS sans la clause NORECOMPUTE.  
  
> [!IMPORTANT]
> Si vous désactivez le recalcul automatique des statistiques de distribution, il se peut que cela empêche l’optimiseur de requête de choisir les plans d’exécution optimaux pour les requêtes impliquant la table.  
  
STATISTICS_INCREMENTAL = { ON | **OFF** }  

**S’applique à** : [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (à compter de [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)]) et [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].

Si la valeur **ON** est définie, les statistiques sont créées par partition. Si la valeur est **OFF**, l’arborescence des statistiques est supprimée et [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] recalcule les statistiques. La valeur par défaut est **OFF**.  
  
 Si les statistiques par partition ne sont pas prises en charge, l'option est ignorée et un avertissement est généré. Les statistiques incrémentielles ne sont pas prises en charge pour les types de statistiques suivants :  
  
-   statistiques créées avec des index qui ne sont pas alignés sur les partitions avec la table de base ;  
-   statistiques créées sur les bases de données secondaires lisibles Always On ;  
-   statistiques créées sur les bases de données en lecture seule ;  
-   statistiques créées sur les index filtrés ;  
-   statistiques créées sur les affichages ;  
-   statistiques créées sur les tables internes ;  
-   statistiques créées avec les index spatiaux ou les index XML.  
  
 ONLINE **=** { ON | **OFF** } \<as applies to rebuild_index_option>  
 Indique si les tables sous-jacentes et les index associés sont disponibles pour les requêtes et la modification de données pendant l'opération d'index. La valeur par défaut est OFF.  
  
 Pour un index XML ou un index spatial, seul `ONLINE = OFF` est pris en charge et si ONLINE est activé (ON), une erreur est générée.  
  
> [!IMPORTANT]
> Les opérations d’index en ligne ne sont pas disponibles dans toutes les éditions de [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Pour obtenir la liste des fonctionnalités prises en charge par les éditions de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], consultez [Éditions et fonctionnalités prises en charge pour [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)]](../../sql-server/editions-and-supported-features-for-sql-server-2016.md) et [Éditions et fonctionnalités prises en charge pour SQL Server 2017](../../sql-server/editions-and-components-of-sql-server-2017.md).  
  
 ACTIVÉ  
 Les verrous de table à long terme ne sont pas maintenus pendant la durée de l'opération d'index. Lors de la principale phase de l'indexation, seul le verrou de partage intentionnel (IS, Intent Share) est maintenu sur la table source. Cela permet aux requêtes ou aux mises à jour effectuées dans la table et les index sous-jacents de continuer. Au début de l'opération, un verrou partagé (S, Shared) est très brièvement maintenu sur l'objet source. À la fin de l'opération, un verrou partagé (S) est très brièvement maintenu sur la source si un index non cluster est en cours de création ou bien un verrou SCH-M (Modification du schéma) est acquis lorsqu'un index cluster est créé ou supprimé en ligne ou lorsqu'un index cluster ou non cluster est en cours de reconstruction. ONLINE ne peut pas prendre la valeur ON si un index est en cours de création sur une table locale temporaire.  
  
 OFF  
 Des verrous de table sont appliqués pendant l'opération d'indexation. Une opération d'index hors connexion qui crée, reconstruit ou supprime un index cluster, spatial ou XML, ou qui reconstruit ou supprime un index non cluster, acquiert un verrou Sch-M (Modification du schéma) sur la table. Cela empêche tous les utilisateurs d'accéder à la table sous-jacente pendant la durée de l'opération. Une opération d'indexation hors ligne qui crée un index non cluster acquiert un verrou partagé (S, Shared) sur la table. Cela empêche la mise à jour de la table sous-jacente, mais autorise les opérations de lecture, telles que des instructions SELECT.  
  
Pour plus d'informations, consultez [Perform Index Operations Online](../../relational-databases/indexes/perform-index-operations-online.md).  
  
Les index, notamment les index portant sur des tables temporaires globales, ne peuvent pas être régénérés en ligne, à l’exception des index suivants :
- Index XML
- Index de table temporaire locale
- Index cluster unique de départ sur une vue
- Index columnstore
- Idex cluster si la table sous-jacente contient des types de données LOB (**image**, **ntext**, **text**) et des types de données spatiales
- Les colonnes **varchar(max)** et **varbinary(max)** ne peuvent pas faire partie d’un index. Dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (à compter de [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]) et dans [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)], quand une table contient des colonnes **varchar(max)** ou **varbinary(max)** , un index cluster contenant d’autres colonnes peut être généré ou régénéré à l’aide de l’option **ONLINE**. [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] n’autorise pas l’option **ONLINE** quand la table de base contient des colonnes **varchar(max)** ou **varbinary(max)**

Pour plus d’informations, consultez [Fonctionnement des opérations d’index en ligne](../../relational-databases/indexes/how-online-index-operations-work.md).

RESUMABLE **=** { ON | **OFF**}

**S’applique à** : [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (à compter de [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)]) et [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].   

 Spécifie si une opération d’index en ligne peut être reprise.

 ON L’opération de l’index peut être reprise.

 OFF L’opération de l’index ne peut pas être reprise.

MAX_DURATION **=** *heure* [**MINUTES**] utilisé avec **RESUMABLE = ON** (nécessite **ONLINE = ON**).

**S’applique à** : [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (à compter de [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)]) et [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]. 

Indique le temps (valeur entière spécifiée en minutes) pendant lequel une opération d’index en ligne pouvant être reprise est exécutée avant d’être mise en pause. 

> [!IMPORTANT]
> Pour plus d’informations sur les opérations d’index pouvant être effectuées en ligne, consultez [Instructions pour les opérations d’index en ligne](../../relational-databases/indexes/guidelines-for-online-index-operations.md).

> [!NOTE] 
> Les régénérations d’index en ligne pouvant être reprise ne sont pas prises en charge sur les index columnstore.

ALLOW_ROW_LOCKS **=** { **ON** | OFF }  

**S’applique à** : [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (à compter de [!INCLUDE[ssKatmai](../../includes/ssKatmai-md.md)]) et [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].  
  
 Indique si les verrous de ligne sont autorisés ou non. La valeur par défaut est ON.  
  
 ACTIVÉ  
 Les verrous de ligne sont autorisés lors de l'accès à l'index. Le [!INCLUDE[ssDE](../../includes/ssde-md.md)] détermine le moment où les verrous de ligne sont utilisés.  
  
 OFF  
 Les verrous de ligne ne sont pas utilisés.  
  
ALLOW_PAGE_LOCKS **=** { **ON** | OFF }  
  
**S’applique à** : [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (à compter de [!INCLUDE[ssKatmai](../../includes/ssKatmai-md.md)]) et [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].
  
 Indique si les verrous de page sont autorisés. La valeur par défaut est ON.  
  
 ACTIVÉ  
 Les verrous de page sont autorisés lors de l'accès à l'index. Le [!INCLUDE[ssDE](../../includes/ssde-md.md)] détermine le moment où les verrous de page sont utilisés.  
  
 OFF  
 Les verrous de page ne sont pas utilisés.  
  
> [!NOTE]
> Un index ne peut pas être réorganisé lorsque ALLOW_PAGE_LOCKS est désactivé (OFF).  

 OPTIMIZE_FOR_SEQUENTIAL_KEY = {ON | **OFF** }

**S’applique à** : [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (à compter de [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)]) et [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].

Spécifie s’il faut optimiser ou pas la contention d’insertion de la dernière page. La valeur par défaut est OFF. Consultez la section [clés séquentielles](./create-index-transact-sql.md#sequential-keys) de la page CREATE INDEX pour plus d’informations.

 MAXDOP **=** max_degree_of_parallelism  
 
**S’applique à** : [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (à compter de [!INCLUDE[ssKatmai](../../includes/ssKatmai-md.md)]) et [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].  
  
 Remplace l’option de configuration **max degree of parallelism** pendant la durée de l’opération d’index. Pour plus d’informations, consultez [Configurer l’option de configuration du serveur max degree of parallelism](../../database-engine/configure-windows/configure-the-max-degree-of-parallelism-server-configuration-option.md). Utilisez MAXDOP pour limiter le nombre de processeurs utilisés dans une exécution de plan parallèle. Le nombre maximal de processeurs est égal à 64.  
  
> [!IMPORTANT]
>  Bien que l'option MAXDOP soit prise en charge syntaxiquement pour tous les index XML, pour un index spatial ou un XML primaire, ALTER INDEX utilise actuellement seulement un processeur unique.  
  
 *max_degree_of_parallelism* peut avoir la valeur :  
  
 1  
 Supprime la création de plans parallèles.  
  
 \>1  
 Limite au nombre spécifié le nombre maximal de processeurs utilisés dans le traitement en parallèle des index.  
  
 0 (valeur par défaut)  
 Utilise le nombre réel de processeurs ou un nombre de processeurs inférieur en fonction de la charge de travail actuelle du système.  
  
 Pour plus d’informations, consultez [Configurer des opérations d’index parallèles](../../relational-databases/indexes/configure-parallel-index-operations.md).  
  
> [!NOTE]
> Les opérations d'index parallèles ne sont pas disponibles dans toutes les éditions de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Pour obtenir la liste des fonctionnalités prises en charge par les éditions de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], consultez [Fonctionnalités prises en charge par les éditions de SQL Server 2016](../../sql-server/editions-and-components-of-sql-server-2016.md).  
  
COMPRESSION_DELAY **=** { **0** |*duration [Minutes]* }  

**S’applique à :** [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (à compter de [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)])  
  
 Pour une table sur disque, le délai spécifie le nombre minimal de minutes pendant lesquelles un rowgroup delta à l’état CLOSED doit rester dans le rowgroup delta avant que [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] puisse le compresser dans le rowgroup compressé. Étant donné que les tables sur disque ne suivent pas les durées des opérations d’insertion et de mise à jour sur chaque ligne, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] applique le délai aux rowgroups delta qui ont l’état CLOSED.  
  
 La valeur par défaut est 0 minute.  
  
 Pour obtenir des recommandations concernant l’utilisation de COMPRESSION_DELAY, consultez [Prise en main de Columnstore pour l’analytique opérationnelle en temps réel](../../relational-databases/indexes/get-started-with-columnstore-for-real-time-operational-analytics.md).  
  
 DATA_COMPRESSION  
 Spécifie l'option de compression de données pour l'index, le numéro de partition ou la plage de partitions spécifiés. Les options disponibles sont les suivantes :  
  
 Aucune  
 L'index ou les partitions spécifiées ne sont pas compressés. Ne s'applique pas aux index columnstore.  
  
 ROW  
 L'index ou les partitions spécifiées sont compressés au moyen de la compression de ligne. Ne s'applique pas aux index columnstore.  
  
 PAGE  
 L'index ou les partitions spécifiées sont compressés au moyen de la compression de page. Ne s'applique pas aux index columnstore.  
  
 COLUMNSTORE  
   
**S’applique à** : [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (à compter de [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)]) et [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].
  
 S'applique uniquement aux index columnstore, y compris aux index columnstore non cluster et cluster. COLUMNSTORE spécifie qu'il faut décompresser l'index ou les partitions spécifiées compressés à l'aide de l'option COLUMNSTORE_ARCHIVE. Lorsque les données sont restaurées, elles continuent à être compressées à l'aide de la compression columnstore utilisée pour tous les index columnstore.  
  
 COLUMNSTORE_ARCHIVE  
  
**S’applique à** : [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (à compter de [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)]) et [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].
  
 S'applique uniquement aux index columnstore, y compris aux index columnstore non cluster et cluster. COLUMNSTORE_ARCHIVE compressera davantage la partition spécifiée en une plus petite taille. Peut être utilisé pour l'archivage, ou d'autres situations qui nécessitent moins de stockage et supportent plus de temps pour le stockage et la récupération.  
  
 Pour plus d’informations sur la compression, consultez [Compression des données](../../relational-databases/data-compression/data-compression.md).  
  
 ON PARTITIONS **(** { \<partition_number_expression> | \<range> } [ **,** ...n] **)**  
    
**S’applique à** : [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (à compter de [!INCLUDE[ssKatmai](../../includes/ssKatmai-md.md)]) et [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]. 
  
 Spécifie les partitions auxquelles le paramètre DATA_COMPRESSION s'applique. Si l'index n'est pas partitionné, l'argument ON PARTITIONS générera une erreur. Si la clause ON PARTITIONS n'est pas fournie, l'option DATA_COMPRESSION s'applique à toutes les partitions d'un index partitionné.  
  
 \<partition_number_expression> peut être spécifié des manières suivantes :  
  
-   Spécifie le numéro d’une partition, par exemple : `ON PARTITIONS (2)`.  
-   Spécifie des numéros de partition pour plusieurs partitions individuelles séparées par des virgules, par exemple : `ON PARTITIONS (1, 5)`.  
-   Spécifie à la fois des plages et des partitions individuelles : `ON PARTITIONS (2, 4, 6 TO 8)`.  
  
 \<range> peut être spécifié sous la forme de numéros de partitions séparés par le mot TO, par exemple : `ON PARTITIONS (6 TO 8)`.  
  
 Pour définir des types différents de compression de données pour des partitions différentes, spécifiez plusieurs fois l'option DATA_COMPRESSION, par exemple :  
  
```sql  
REBUILD WITH   
(  
DATA_COMPRESSION = NONE ON PARTITIONS (1),   
DATA_COMPRESSION = ROW ON PARTITIONS (2, 4, 6 TO 8),   
DATA_COMPRESSION = PAGE ON PARTITIONS (3, 5)  
);  
```  
  
 ONLINE **=** { ON  | **OFF** } \<as applies to single_partition_rebuild_index_option>  
 Spécifie si un index ou une partition d’index d'une table sous-jacente peut être reconstruit en ligne ou hors connexion. Si **REBUILD** est effectué en ligne (**ON**), les données de cette table sont disponibles pour les requêtes et la modification de données pendant l'opération d’index.  La valeur par défaut est **OFF**.  
  
 ACTIVÉ  
 Les verrous de table à long terme ne sont pas maintenus pendant la durée de l'opération d'index. Lors de la principale phase de l'indexation, seul le verrou de partage intentionnel (IS, Intent Share) est maintenu sur la table source. Un verrou S sur la table est nécessaire au début de la reconstruction de l’index tandis qu’un verrou Sch-M sur la table l’est à la fin de la reconstruction de l’index en ligne. Bien que les deux verrous soient des verrous de métadonnées courtes, le verrou Sch-M doit notamment attendre que toutes les transactions bloquantes soient terminées. Pendant le temps d'attente, le verrou Sch-M bloque toutes les autres transactions qui attendent derrière ce verrou en cas d'accès à la même table.  
  
> [!NOTE]
>  La reconstruction d’index en ligne peut définir les options *low_priority_lock_wait* décrites plus loin dans cette section.  
  
 OFF  
 Des verrous de table sont appliqués pendant l'opération d'indexation. Cela empêche tous les utilisateurs d'accéder à la table sous-jacente pendant la durée de l'opération.  
  
 WAIT_AT_LOW_PRIORITY utilisé avec **ONLINE=ON** uniquement.  
 
**S’applique à** : [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (à compter de [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)]) et [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].
  
 Une reconstruction d'index en ligne doit attendre les opérations de blocage sur cette table. **WAIT_AT_LOW_PRIORITY** indique que l’opération de reconstruction de l’index en ligne doit attendre les verrous de faible priorité, en permettant à d’autres opérations de continuer pendant que l’opération de construction de l’index en ligne patiente. Si l’option **WAIT AT LOW PRIORITY** est omise, cela équivaut à WAIT_AT_LOW_PRIORITY (MAX_DURATION = 0 minutes, ABORT_AFTER_WAIT = NONE). Pour plus d’informations, consultez [WAIT_AT_LOW_PRIORITY](alter-index-transact-sql.md). 
  
 MAX_DURATION = *time* [**MINUTES**]  
  
**S’applique à** : [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (à compter de [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)]) et [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].
  
 Temps d'attente (valeur entière spécifiée en minutes) pendant lequel les verrous de reconstruction d'index en ligne devront attendre avec une faible priorité lors de l'exécution de la commande DDL. Si l’opération est bloquée pendant le temps **MAX_DURATION**, l’une des actions **ABORT_AFTER_WAIT** est exécutée. La durée **MAX_DURATION** est toujours spécifiée en minutes, et le mot **MINUTES** peut être omis.  
 
 ABORT_AFTER_WAIT = [**NONE** | **SELF** | **BLOCKERS** } ]  
   
**S’applique à** : [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (à compter de [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)]) et [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].
  
 Aucune  
 Continuez à attendre le verrou avec la priorité normale.  
  
 SELF  
 Quittez l'opération DDL de reconstruction de l'index en ligne actuellement exécutée sans effectuer aucune action.  
  
 BLOCKERS  
 Annulez toutes les transactions utilisateur qui bloquent l'opération DDL de reconstruction de l'index en ligne afin que l'opération puisse continuer. Avec l’option **BLOCKERS**, la connexion doit avoir l’autorisation **ALTER ANY CONNECTION**.  
 
 RESUME 
 
**S’applique à** : [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (à compter de [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)]) et [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].  

Reprenez une opération d’index mise en pause manuellement ou suite à une défaillance.

MAX_DURATION utilisé avec **RESUMABLE=ON**
 
**S’applique à** : [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (à compter de [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)]) et [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].

Temps (valeur entière spécifiée en minutes) pendant lequel l’opération d’index en ligne pouvant être reprise est exécutée après avoir été reprise. Une fois que le délai expire, l’opération pouvant être reprise est mise en pause si elle est toujours en cours d’exécution.

WAIT_AT_LOW_PRIORITY utilisé avec **RESUMABLE=ON** et **ONLINE = ON**.  
  
**S’applique à** : [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (à compter de [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)]) et [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]. 
  
 La reprise d’une reconstruction d’index en ligne après une pause doit attendre les opérations de blocage sur cette table. **WAIT_AT_LOW_PRIORITY** indique que l’opération de reconstruction de l’index en ligne doit attendre les verrous de faible priorité, en permettant à d’autres opérations de continuer pendant que l’opération de construction de l’index en ligne patiente. Si l’option **WAIT AT LOW PRIORITY** est omise, cela équivaut à WAIT_AT_LOW_PRIORITY (MAX_DURATION = 0 minutes, ABORT_AFTER_WAIT = NONE). Pour plus d’informations, consultez [WAIT_AT_LOW_PRIORITY](alter-index-transact-sql.md). 

PAUSE
 
**S’applique à** : [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (à compter de [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)]) et [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]. 
  
Mettez en pause une opération de reconstruction d’index en ligne pouvant être reprise.

ABORT

**S’applique à** : [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (à compter de [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)]) et [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].   

Abandonnez une opération d’index en cours d’exécution ou en pause qui a été déclarée comme pouvant être reprise. Vous devez exécuter explicitement une commande **ABORT** pour mettre fin à une opération de reconstruction d’index pouvant être reprise. L’échec ou la mise en pause d’une opération d’index pouvant être reprise n’arrête pas son exécution, mais la laisse dans un état de pause indéterminée.
  
## <a name="remarks"></a>Notes  
ALTER INDEX ne peut pas être utilisé pour recréer la partition d'un index ou le déplacer vers un autre groupe de fichiers. Cette instruction ne peut pas être utilisée pour modifier la définition de l'index, comme l'ajout ou la suppression de colonnes ou la modification de l'ordre des colonnes. Exécutez CREATE INDEX avec la clause DROP_EXISTING afin de procéder aux opérations suivantes.  
  
Si une option n'est pas spécifiée de façon explicite, le paramètre actuel s'applique. Par exemple, si un paramètre FILLFACTOR n'est pas indiqué dans la clause REBUILD, la valeur de facteur de remplissage stockée dans le catalogue système est utilisée lors du processus de reconstruction. Pour voir les paramètres des options d’index actuels, utilisez [sys.indexes](../../relational-databases/system-catalog-views/sys-indexes-transact-sql.md).  
  
Les valeurs de `ONLINE`, `MAXDOP` et `SORT_IN_TEMPDB` ne sont pas stockées dans le catalogue système. À moins qu'elle ne soit précisée dans l'instruction d'index, la valeur par défaut de l'option est alors utilisée.
  
Sur les ordinateurs multiprocesseurs, à la manière d’autres requêtes, `ALTER INDEX REBUILD` utilise automatiquement davantage de processeurs pour effectuer les opérations d’analyse et de tri associées à la modification de l’index. Lorsque vous exécutez `ALTER INDEX REORGANIZE`, avec ou sans LOB_COMPACTION, la valeur de **degré maximal de parallélisme** correspond à une opération de thread unique. Pour plus d’informations, consultez [Configurer des opérations d’index parallèles](../../relational-databases/indexes/configure-parallel-index-operations.md).  
  
> [!IMPORTANT]
> Un index ne peut pas être réorganisé ou reconstruit si le groupe de fichiers dans lequel il se trouve est hors connexion ou en lecture seule. Si le mot clé ALL est spécifié et que des index se trouvent dans un groupe de fichiers hors connexion ou en lecture seule, l'instruction échoue.  
  
## <a name="rebuilding-indexes"></a><a name="rebuilding-indexes"></a> Reconstruction des index  
La reconstruction d'un index entraîne sa suppression puis sa recréation. Ceci permet d'éviter toute fragmentation, de libérer de l'espace disque en compactant les pages d'après le paramètre du facteur de remplissage spécifié ou déjà existant et en retriant les lignes de l'index en pages contiguës. Quand ALL est précisé, tous les index sur la table sont supprimés puis reconstruits en une seule transaction. Il n’est pas nécessaire de supprimer les contraintes de clé étrangère au préalable. Lorsque de la reconstruction d'index contenant au moins 128 étendues, le [!INCLUDE[ssDE](../../includes/ssde-md.md)] diffère les désallocations de pages ainsi que les verrous qui y sont associés jusqu'à ce que la transaction soit validée.  
 
Pour plus d’informations, consultez [Réorganiser et reconstruire des index](../../relational-databases/indexes/reorganize-and-rebuild-indexes.md). 
  
## <a name="reorganizing-indexes"></a><a name="reorganizing-indexes"></a> Réorganisation des index
La réorganisation d'un index utilise des ressources système minimes. En effet, elle défragmente le niveau feuille des index cluster et non cluster sur les tables et les vues en retriant les pages de niveau feuille de façon physique afin de resuivre l'ordre logique, c'est-à-dire de gauche à droite, des nœuds. Cette opération compacte également les pages d'index. Le compactage s'appuie sur la valeur du facteur de remplissage existante. 
  
Si `ALL` est spécifié, les index relationnels, aussi bien cluster que non cluster, et les index XML de la table sont réorganisés. Certaines restrictions s’appliquent quand ALL est spécifié ; consultez la définition de ALL dans la section Arguments de cet article.  
  
Pour plus d’informations, consultez [Réorganiser et reconstruire des index](../../relational-databases/indexes/reorganize-and-rebuild-indexes.md).  

> [!IMPORTANT]
> Pour une table [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)] avec un index Columnstore ordonné en cluster, `ALTER INDEX REORGANIZE` ne trie pas les données à nouveau. Pour trier à nouveau l’utilisation de données `ALTER INDEX REBUILD`.
  
## <a name="disabling-indexes"></a><a name="disabling-indexes"></a> Désactivation des index  
Désactiver un index permet d'éviter l'accès à l'index, et dans le cas d'index cluster, aux données de la table sous-jacente par les utilisateurs. La définition de l'index est conservée dans le catalogue système. Désactiver un index, qu'il soit non cluster ou cluster, sur une vue supprime physiquement les données de l'index. Désactiver un index cluster permet d'éviter l'accès aux données mais celles-ci ne sont plus mises à jour dans l'arborescence binaire (appelé également arbre B) jusqu'à ce que l'index soit supprimé ou reconstruit. Pour afficher l’état d’un index, qu’il soit activé ou désactivé, lancez une requête sur la colonne **is_disabled** dans la vue de catalogue **sys.indexes**.  
  
Si une table est dans une publication de réplication transactionnelle, vous ne pouvez pas désactiver les index qui sont associés à des colonnes clés primaire. Ces index sont requis par la réplication. Pour désactiver un index, vous devez d'abord supprimer la table de la réplication. Pour plus d’informations, consultez [Publier des données et des objets de base de données](../../relational-databases/replication/publish/publish-data-and-database-objects.md).  
  
Utilisez les instructions `ALTER INDEX REBUILD` ou `CREATE INDEX WITH DROP_EXISTING` pour activer l’index. La reconstruction d'un index cluster désactivé ne peut être effectuée si l'option ONLINE est activée (ON). Pour plus d’informations, consultez [Désactiver les index et contraintes](../../relational-databases/indexes/disable-indexes-and-constraints.md).  
  
## <a name="setting-options"></a>Configuration des options  
Vous pouvez définir les options `ALLOW_ROW_LOCKS`, `ALLOW_PAGE_LOCKS`, `OPTIMIZE_FOR_SEQUENTIAL_KEY`, `IGNORE_DUP_KEY` et `STATISTICS_NORECOMPUTE` pour un index spécifié sans régénérer ou réorganiser cet index. Les valeurs modifiées sont immédiatement appliquées à l'index. Pour afficher ces paramètres, utilisez **sys.indexes**. Pour plus d’informations, consultez [Définir les options d’index](../../relational-databases/indexes/set-index-options.md).  
  
### <a name="row-and-page-locks-options"></a>Options de verrous de ligne et de page  
Si `ALLOW_ROW_LOCKS = ON` et `ALLOW_PAGE_LOCK = ON`, les verrous au niveau de la ligne, de la page et de la table sont autorisés au moment où vous accédez à l’index. Le [!INCLUDE[ssDE](../../includes/ssde-md.md)] choisit le verrou approprié et peut promouvoir un verrou de ligne ou de page en verrou de table.  
  
Si `ALLOW_ROW_LOCKS = OFF` et `ALLOW_PAGE_LOCK = OFF`, seul un verrou au niveau de la table est autorisé au moment où vous accédez à l’index.  
  
Si ALL est indiqué lors de la définition des options de verrouillage de ligne ou de page, les paramètres s'appliquent à tous les index. Si la table sous-jacente correspond à un segment de mémoire, les paramètres s'appliquent des façons suivantes :  
  
|Option|Détails|
|------|-------|
|ALLOW_ROW_LOCKS = ON ou OFF|S'applique au segment de mémoire et à tout index non cluster qui lui est associé.|  
|ALLOW_PAGE_LOCKS = ON|S'applique au segment de mémoire et à tout index non cluster qui lui est associé.|  
|ALLOW_PAGE_LOCKS = OFF|Verrou entier pour les index non cluster. En d'autres termes, tous les verrous de page ne sont pas autorisés sur les index non cluster. En ce qui concerne le segment de mémoire, seul les verrous partagé (P), de mise à jour (M) et exclusifs (E) ne sont pas autorisés. Le [!INCLUDE[ssDE](../../includes/ssde-md.md)] peut toujours acquérir un verrou de page intentionnel (IS, IU ou IX) à des fins internes.|  
  
## <a name="online-index-operations"></a><a name="online-index-operations"></a> Opérations d’index en ligne  
Si vous reconstruisez un index et que l'option ONLINE est activée (ON), les objets, les tables et les index associés sous-jacents sont disponibles pour permettre les requêtes et la modification de données. Vous pouvez également reconstruire en ligne une partie d'un index résidant sur une partition unique. Les verrous de tables exclusifs ne sont maintenus que pour une durée courte lors du processus de modification.  
  
La réorganisation d'un index s'effectue toujours en ligne. Elle ne conserve pas les verrous à long terme et ne bloque pas ainsi les requêtes ou les mises à jour en cours d'exécution.  
  
Vous pouvez lancer des opérations d'index en ligne simultanées sur une même table ou partition de table, mais uniquement dans les cas suivants :  
  
-   Création de plusieurs index non cluster.  
-   réorganisation de différents index sur une même table ;  
-   réorganisation de différents index lors de la reconstruction d'index ne se chevauchant pas et portant sur une même table.  
  
Toutes les autres opérations en ligne sur les index exécutées en même temps échouent. Par exemple, vous ne pouvez pas reconstruire de façon concurrente plusieurs index portant sur une même table ou créer d'index lors de la reconstruction d'un index existant portant sur la même table.  

### <a name="resumable-index-operations"></a><a name="resumable-indexes"></a> Opérations d’index pouvant être reprises

**S’applique à** : [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (à compter de [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)]) et [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]. 

La reconstruction d’index en ligne est spécifiée comme pouvant être reprise avec l’option RESUMABLE=ON. 
-  L’option RESUMABLE n’est pas persistante dans les métadonnées d’un index donné et s’applique uniquement à la durée d’une instruction DDL actuelle. C’est pourquoi la clause RESUMABLE=ON doit être spécifiée explicitement pour permettre la reprise.
-  L’option MAX_DURATION est prise en charge pour l’option RESUMABLE = ON ou l’option d’argument **low_priority_lock_wait**. 
   -  MAX_DURATION pour l’option RESUMABLE spécifie l’intervalle de temps pour un index en cours de reconstruction. Une fois que cette durée est utilisée, la reconstruction d’index est mise en pause ou termine son exécution. L’utilisateur décide quand une reconstruction d’un index en pause peut être reprise. La **durée** en minutes de MAX_DURATION doit être supérieure à 0 minute et inférieur ou égale à une semaine (7 \* 24 \* 60 = 10080 minutes). Si la mise en pause d’une opération d’index est longue, les performances DML d’une table spécifique et la capacité de disque de la base de données risquent d’être impactées parce que les deux index, l’original et celui nouvellement créé, ont besoin d’espace disque et doivent être mis à jour au cours des opérations DML. Si l’option MAX_DURATION est omise, l’opération d’index se poursuit jusqu'à ce qu’elle se termine ou qu’une défaillance se produise. 
   -  L’option d’argument \<low_priority_lock_wait> vous permet de décider comment l’opération d’index peut continuer quand elle est bloquée sur le verrou SCH-M.
 
-  La réexécution de l’instruction ALTER INDEX REBUILD d’origine avec les mêmes paramètres reprend une opération de reconstruction d’index mise en pause. Vous pouvez également reprendre une opération de reconstruction d’index en pause en exécutant l’instruction ALTER INDEX RESUME.
-  L’option SORT_IN_TEMPDB=ON n’est pas prise en charge pour l’index pouvant être repris. 
-  Il n’est pas possible d’exécuter la commande DDL avec RESUMABLE=ON dans une transaction explicite (ne peut pas faire partie d’un block begin tran ... commit).
-  Seules les opérations d’index qui sont mises en pause peuvent être reprises.
-  Quand vous reprenez une opération d’index qui est en pause, vous pouvez remplacer la valeur MAXDOP par une nouvelle valeur.  Si MAXDOP n’est pas spécifié lors de la reprise d’une opération d’index qui est en pause, la dernière valeur MAXDOP est prise. Si l’option MAXDOP n’est pas du tout spécifiée pour l’opération de reconstruction d’index, la valeur par défaut est prise.
- Pour mettre tout de suite en pause l’opération d’index, vous pouvez arrêter la commande en cours (Ctrl-C) ou vous pouvez exécuter la commande ALTER INDEX PAUSE ou la commande KILL *session_id*. Une fois la commande en pause, elle peut être reprise avec l’option RESUME.
-  La commande ABORT met fin à la session qui a hébergé la reconstruction d’index d’origine et abandonne l’opération d’index  
-  Aucune autre ressource n’est nécessaire pour la reconstruction d’index pouvant être reprise sauf pour
   -    De l’espace supplémentaire nécessaire pour conserver l’index en cours de construction, y compris le temps lorsque l’index est en pause
   -    Un état DDL empêchant toute modification DDL
-  Le nettoyage des éléments fantômes s’exécutera pendant la phase de pause de l’index, mais sera mis en pause pendant l’exécution de l’index   
Les fonctionnalités suivantes sont désactivées pour les opérations de reconstruction d’index pouvant être reprises
   -    La reconstruction d’un index désactivé n’est pas prise en charge avec RESUMABLE=ON
   -    La commande ALTER INDEX REBUILD ALL
   -    ALTER TABLE avec la reconstruction d’index  
   -    Il n’est pas possible d’exécuter la commande DDL avec « RESUMABLE=ON » dans une transaction explicite (ne peut pas faire partie d’un block begin tran ... commit)
   -    La reconstruction d’un index qui a calculé ou horodaté (TIMESTAMP) la ou les colonnes en tant que colonnes clés.
-   Si la table de base contient une ou plusieurs colonnes LOB, la reconstruction d’index cluster pouvant être reprise nécessite un verrou Sch-M au début de cette opération 

> [!NOTE]
> La commande DDL s’exécute tant qu’elle n’a pas fini, n’est pas mise en pause ou n’a pas échoué. Si la commande est en pause, une erreur s’affiche indiquant que l’opération a été mise en pause et que la création d’index n’a pas été effectuée. Vous trouverez plus d’informations sur l’état d’index actuel dans [sys.index_resumable_operations](../../relational-databases/system-catalog-views/sys-index-resumable-operations.md). Comme avant, en cas de défaillance, une erreur s’affiche également. 

 Pour plus d'informations, consultez [Perform Index Operations Online](../../relational-databases/indexes/perform-index-operations-online.md).  
  
 ### <a name="wait_at_low_priority-with-online-index-operations"></a>WAIT_AT_LOW_PRIORITY avec les opérations d’index en ligne  
  
 Pour exécuter l'instruction DDL pour une reconstruction d'index en ligne, toutes les transactions bloquantes actives qui s'exécutent sur une table particulière doivent être terminées. Lorsque la reconstruction d'index en ligne s'exécute, elle bloque toutes les nouvelles transactions qui sont prêtes à s'exécuter sur cette table. Bien que la durée du verrou pour la reconstruction de l'index en ligne soit très courte, le fait d'attendre que toutes les transactions ouvertes sur une table spécifique soient exécutées, et le fait de bloquer les nouvelles transactions qui doivent démarrer, peuvent avoir un impact important sur le débit et provoquer un ralentissement ou un délai d'attente des charges de travail, limitant considérablement l'accès à la table sous-jacente. L’option **WAIT_AT_LOW_PRIORITY** permet aux administrateurs de base de données de gérer les verrous S et Sch-M nécessaires pour les reconstructions d’index en ligne, et de sélectionner l’une des 3 options. Dans les trois cas, si aucune activité n’est bloquante pendant le temps d’attente ((MAX_DURATION = n [minutes])), la reconstruction d’index en ligne est exécutée immédiatement, sans attendre, et l’instruction DDL est effectuée.  
  
## <a name="spatial-index-restrictions"></a>Restrictions des index spatiaux  
 Lorsque vous reconstruisez un index spatial, la table utilisateur sous-jacente est indisponible pour toute la durée de l'opération d'index car l'index spatial détient un verrou de schéma.  
  
 La contrainte PRIMARY KEY dans la table utilisateur ne peut pas être modifiée alors qu'un index spatial est défini sur une colonne de cette table. Pour modifier la contrainte PRIMARY KEY, commencez par supprimer chaque index spatial de la table. Après avoir modifié la contrainte PRIMARY KEY, vous pouvez recréer chaque index spatial.  
  
 Dans une opération individuelle de reconstruction de partition, vous ne pouvez pas spécifier d'index spatial. Toutefois, vous pouvez spécifier des index spatiaux dans une reconstruction de partition complète.  
  
 Pour modifier des options spécifiques à un index spatial, telles que BOUNDING_BOX ou GRID, vous pouvez utiliser une instruction CREATE SPATIAL INDEX qui spécifie DROP_EXISTING = ON ou supprimer l'index spatial et en créer un nouveau. Pour obtenir un exemple, consultez [CREATE SPATIAL INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/create-spatial-index-transact-sql.md).  
  
## <a name="data-compression"></a>Data Compression  
 Pour plus d’informations sur la compression de données, consultez [Compression des données](../../relational-databases/data-compression/data-compression.md).  
  
 Pour évaluer dans quelle mesure le changement de compression PAGE et ROW affecte une table, un index ou une partition, utilisez la procédure stockée [sp_estimate_data_compression_savings](../../relational-databases/system-stored-procedures/sp-estimate-data-compression-savings-transact-sql.md).  
  
Les restrictions suivantes s'appliquent aux index partitionnés :  
  
-   Lorsque vous utilisez ALTER INDEX ALL ..., vous ne pouvez pas modifier le paramètre de compression d’une partition unique si la table comporte des index non alignés.  
-   L’instruction ALTER INDEX \<index> ... REBUILD PARTITION ... reconstruit la partition spécifiée de l’index.  
-   L’instruction ALTER INDEX \<index> ... REBUILD WITH ... reconstruit toutes les partitions de l’index.  
  
## <a name="statistics"></a>Statistiques  
 Quand vous exécutez **ALTER INDEX ALL ...** sur une table, seules les statistiques associées aux index sont mises à jour. Les statistiques automatiques ou manuelles créées sur la table (au lieu d'un index) ne sont pas mises à jour.  
  
## <a name="permissions"></a>Autorisations  
 Pour pouvoir exécuter l'instruction ALTER INDEX, vous devez obligatoirement bénéficier au minimum d'autorisations nécessaires pour exécuter les instructions ALTER sur la table ou la vue.  
  
## <a name="version-notes"></a>Notes de version  
  
-  [!INCLUDE[ssSDS](../../includes/sssds-md.md)] n’utilise pas les options de filegroup et filestream.  
-  Les index columnstore ne sont pas disponibles avant [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]. 
-  Les opérations d’index pouvant être reprises sont disponibles à compter de [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]   
  
## <a name="basic-syntax-example"></a>Exemple de syntaxe de base :   
  
```sql 
ALTER INDEX index1 ON table1 REBUILD;  
  
ALTER INDEX ALL ON table1 REBUILD;  
  
ALTER INDEX ALL ON dbo.table1 REBUILD;  
```

## <a name="examples-columnstore-indexes"></a>Exemples : Index Columnstore  
 Ces exemples s’appliquent aux index columnstore.  
  
### <a name="a-reorganize-demo"></a>R. Démonstration REORGANIZE  
 Cet exemple montre comment fonctionne la commande ALTER INDEX REORGANIZE.  Il crée une table qui contient plusieurs rowgroups et montre comment REORGANIZE fusionne les rowgroups.  
  
```sql  
-- Create a database   
CREATE DATABASE [ columnstore ];  
GO  
  
-- Create a rowstore staging table  
CREATE TABLE [ staging ] (  
     AccountKey              int NOT NULL,  
     AccountDescription      nvarchar (50),  
     AccountType             nvarchar(50),  
     AccountCodeAlternateKey     int  
     )  
  
-- Insert 10 million rows into the staging table.   
DECLARE @loop int  
DECLARE @AccountDescription varchar(50)  
DECLARE @AccountKey int  
DECLARE @AccountType varchar(50)  
DECLARE @AccountCode int  
  
SELECT @loop = 0  
BEGIN TRAN  
    WHILE (@loop < 300000)   
      BEGIN  
        SELECT @AccountKey = CAST (RAND()*10000000 as int);  
        SELECT @AccountDescription = 'accountdesc ' + CONVERT(varchar(20), @AccountKey);  
        SELECT @AccountType = 'AccountType ' + CONVERT(varchar(20), @AccountKey);  
        SELECT @AccountCode =  CAST (RAND()*10000000 as int);  
  
        INSERT INTO  staging VALUES (@AccountKey, @AccountDescription, @AccountType, @AccountCode);  
  
        SELECT @loop = @loop + 1;  
    END  
COMMIT  
  
-- Create a table for the clustered columnstore index  
  
CREATE TABLE cci_target (  
     AccountKey              int NOT NULL,  
     AccountDescription      nvarchar (50),  
     AccountType             nvarchar(50),  
     AccountCodeAlternateKey int  
     )  
  
-- Convert the table to a clustered columnstore index named inxcci_cci_target;  
CREATE CLUSTERED COLUMNSTORE INDEX idxcci_cci_target ON cci_target;  
```  
  
 Utilisez l’option TABLOCK pour insérer des lignes en parallèle. Depuis [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)], l’opération INSERT INTO peut s’exécuter en parallèle lorsque TABLOCK est utilisé.  
  
```sql  
INSERT INTO cci_target WITH (TABLOCK) 
SELECT TOP 300000 * FROM staging;  
```  
  
 Exécutez cette commande pour voir les rowgroups delta OPEN. Le nombre de rowgroups dépend du degré de parallélisme.  
  
```sql  
SELECT *   
FROM sys.dm_db_column_store_row_group_physical_stats   
WHERE object_id  = object_id('cci_target');  
```  
  
 Exécutez cette commande pour forcer tous les rowgroups CLOSED et OPEN dans le columnstore.  
  
```sql  
ALTER INDEX idxcci_cci_target ON cci_target REORGANIZE WITH (COMPRESS_ALL_ROW_GROUPS = ON);  
```  
  
 Réexécutez cette commande et vous verrez que les plus petits rowgroups sont fusionnés dans un rowgroup compressé.  
  
```sql  
ALTER INDEX idxcci_cci_target ON cci_target REORGANIZE WITH (COMPRESS_ALL_ROW_GROUPS = ON);  
```  
  
### <a name="b-compress-closed-delta-rowgroups-into-the-columnstore"></a>B. Compression des rowgroups delta CLOSED dans le columnstore  
 Cet exemple utilise l’option REORGANIZE pour compresser chaque rowgroup delta CLOSED dans le columnstore en tant que rowgroup compressé.   Ce n’est pas nécessaire, mais utile quand le moteur de tuple ne compresse pas les rowgroups CLOSED assez rapidement.  
  
```sql  
-- Uses AdventureWorksDW  
-- REORGANIZE all partitions  
ALTER INDEX cci_FactInternetSales2 ON FactInternetSales2 REORGANIZE;  
  
-- REORGANIZE a specific partition  
ALTER INDEX cci_FactInternetSales2 ON FactInternetSales2 REORGANIZE PARTITION = 0;  
```  
  
### <a name="c-compress-all-open-and-closed-delta-rowgroups-into-the-columnstore"></a>C. Compression de tous les rowgroups delta OPEN et CLOSED dans le columnstore  
 **S’applique à :** [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (à compter de [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)]) et [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]. 
  
 La commande REORGANIZE WITH (COMPRESS_ALL_ROW_GROUPS = ON) compresse chaque rowgroup delta OPEN et CLOSED dans le columnstore en tant que rowgroup compressé. Cette opération vide le deltastore et force la compression de toutes les lignes dans le columnstore. Elle est utile en particulier après l’exécution de nombreuses opérations d’insertion, dans la mesure où ces opérations stockent les lignes dans un ou plusieurs rowgroups delta.  
  
 REORGANIZE combine les rowgroups pour remplir les rowgroups avec un nombre maximal de lignes de \<= 1 024 576. Par conséquent, quand vous compressez tous les rowgroups OPEN et CLOSED, vous ne vous retrouvez pas avec un grand nombre de rowgroups compressés qui contiennent uniquement quelques lignes. Vous souhaitez que les rowgroups soient aussi remplis que possible pour réduire la taille compressée et améliorer les performances des requêtes.  
  
```sql  
-- Uses AdventureWorksDW2016  
-- Move all OPEN and CLOSED delta rowgroups into the columnstore.  
ALTER INDEX cci_FactInternetSales2 ON FactInternetSales2 REORGANIZE WITH (COMPRESS_ALL_ROW_GROUPS = ON);  
  
-- For a specific partition, move all OPEN AND CLOSED delta rowgroups into the columnstore  
ALTER INDEX cci_FactInternetSales2 ON FactInternetSales2 REORGANIZE PARTITION = 0 WITH (COMPRESS_ALL_ROW_GROUPS = ON);  
```  
  
### <a name="d-defragment-a-columnstore-index-online"></a>D. Défragmentation d’un index columnstore en ligne  
 Ne s’applique pas à : [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] et [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)].  
  
 Depuis [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)], REORGANIZE ne fait pas que compresser les rowgroups delta dans le columnstore. Il procède aussi à la défragmentation en ligne. Tout d’abord, il réduit la taille du columnstore en effaçant physiquement les lignes supprimées quand au moins 10 % des lignes d’un rowgroup ont été supprimées.  Ensuite, il combine les rowgroups ensemble pour former des rowgroups plus grands qui peuvent contenir jusqu’à 1 024 576 lignes par rowgroup.  Tous les rowgroups qui sont modifiés sont de nouveau compressés.  
  
> [!NOTE]
> Depuis [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)], la reconstruction d’un index columnstore n’est plus nécessaire dans la plupart des cas, car REORGANIZE efface physiquement les lignes supprimées et fusionne les rowgroups. L’option COMPRESS_ALL_ROW_GROUPS force tous les rowgroups delta OPEN ou CLOSED dans le columnstore, ce qui ne pouvait être fait qu’avec une reconstruction. REORGANIZE est en ligne et se produit en arrière-plan afin que les requêtes puissent continuer quand l’opération a lieu.  
  
```sql  
-- Uses AdventureWorks  
-- Defragment by physically removing rows that have been logically deleted from the table, and merging rowgroups.  
ALTER INDEX cci_FactInternetSales2 ON FactInternetSales2 REORGANIZE;  
```  
  
### <a name="e-rebuild-a-clustered-columnstore-index-offline"></a>E. Reconstruction d’un index cluster columnstore hors connexion  
S’applique à : [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (à compter de [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)])   
  
> [!TIP]
> Depuis [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] et dans [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)], nous recommandons d’utiliser ALTER INDEX REORGANIZE au lieu de ALTER INDEX REBUILD.  
  
> [!NOTE]
> Dans [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] et [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)], REORGANIZE est uniquement utilisé pour compresser les rowgroups CLOSED dans le columnstore. La seule façon d’effectuer des opérations de défragmentation et de forcer tous les rowgroups delta dans le columnstore est de reconstruire l’index.  
  
 Cet exemple montre comment reconstruire un index cluster columnstore et forcer tous les rowgroups delta dans le columnstore. Cette première étape prépare une table FactInternetSales2 avec un index columnstore cluster et insère les quatre premières colonnes.  
  
```sql  
-- Uses AdventureWorksDW  
  
CREATE TABLE dbo.FactInternetSales2 (  
    ProductKey [int] NOT NULL,   
    OrderDateKey [int] NOT NULL,   
    DueDateKey [int] NOT NULL,   
    ShipDateKey [int] NOT NULL);  
  
CREATE CLUSTERED COLUMNSTORE INDEX cci_FactInternetSales2  
ON dbo.FactInternetSales2;  
  
INSERT INTO dbo.FactInternetSales2  
SELECT ProductKey, OrderDateKey, DueDateKey, ShipDateKey  
FROM dbo.FactInternetSales;  
  
SELECT * FROM sys.column_store_row_groups;  
```  
  
 Le résultat montre qu’il y a un rowgroup OPEN, ce qui signifie que [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] attend que d'autres lignes soient ajoutées avant de fermer le rowgroup et de déplacer les données vers le columnstore. L’instruction suivante reconstruit l’index cluster columnstore, ce qui force toutes les lignes dans le columnstore.  
  
```sql  
ALTER INDEX cci_FactInternetSales2 ON FactInternetSales2 REBUILD;  
SELECT * FROM sys.column_store_row_groups;  
```  
  
 Le résultat de l'instruction SELECT indique que le rowgroup est COMPRESSED, ce qui signifie que les segments de colonne du rowgroup sont compressés et stockés dans l'index columnstore.  
  
### <a name="f-rebuild-a-partition-of-a-clustered-columnstore-index-offline"></a>F. Reconstruction d’une partition d’un index cluster columnstore hors connexion  
 **S’applique à** : [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (à compter de [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)])  
 
 Pour reconstruire une partition d’un grand index cluster columnstore, utilisez ALTER INDEX REBUILD avec l’option de partition. Cet exemple reconstruit la partition 12. Depuis [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)], nous recommandons de remplacer REBUILD par REORGANIZE.  
  
```sql  
ALTER INDEX cci_fact3   
ON fact3  
REBUILD PARTITION = 12;  
```  
  
### <a name="g-change-a-clustered-columstore-index-to-use-archival-compression"></a>G. Changement d’un index cluster columstore pour utiliser la compression d’archivage  
 Ne s’applique pas à : [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]  
  
 Vous pouvez choisir de réduire encore plus la taille d’un index cluster columnstore en utilisant l’option de compression de données COLUMNSTORE_ARCHIVE. C’est pratique pour les données plus anciennes que vous souhaitez conserver sur un stockage plus économique. Nous recommandons de l’utiliser uniquement sur les données auxquelles vous n’accédez pas souvent, car la décompression est plus lente qu’avec la compression COLUMNSTORE normale.  
  
 L'exemple suivant reconstruit un index columnstore cluster pour utiliser la compression d'archivage, puis montre comment supprimer la compression d'archivage. Le résultat final utilise uniquement la compression columnstore.  
  
```sql  
--Prepare the example by creating a table with a clustered columnstore index.  
CREATE TABLE SimpleTable (  
    ProductKey [int] NOT NULL,   
    OrderDateKey [int] NOT NULL,   
    DueDateKey [int] NOT NULL,   
    ShipDateKey [int] NOT NULL  
);  
  
CREATE CLUSTERED INDEX cci_SimpleTable ON SimpleTable (ProductKey);  
  
CREATE CLUSTERED COLUMNSTORE INDEX cci_SimpleTable  
ON SimpleTable  
WITH (DROP_EXISTING = ON);  
  
--Compress the table further by using archival compression.  
ALTER INDEX cci_SimpleTable ON SimpleTable  
REBUILD  
WITH (DATA_COMPRESSION = COLUMNSTORE_ARCHIVE);  
  
--Remove the archive compression and only use columnstore compression.  
ALTER INDEX cci_SimpleTable ON SimpleTable  
REBUILD  
WITH (DATA_COMPRESSION = COLUMNSTORE);  
GO  
```  
  
## <a name="examples-rowstore-indexes"></a>Exemples : index Rowstore  
  
### <a name="a-rebuilding-an-index"></a>R. Reconstruction d'un index  
 L'exemple suivant reconstruit un seul index portant sur la table `Employee` de la base de données [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)].  
  
```sql  
ALTER INDEX PK_Employee_EmployeeID ON HumanResources.Employee REBUILD;  
```  
  
### <a name="b-rebuilding-all-indexes-on-a-table-and-specifying-options"></a>B. Reconstruction de tous les index d'une table et indication des options  
 L’exemple suivant spécifie le mot clé ALL. Il reconstruit tous les index associés à la table Production.Product dans la base de données [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)]. Trois options sont spécifiées.  
  
**S’applique à** : [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (à compter de [!INCLUDE[ssKatmai](../../includes/ssKatmai-md.md)]) et [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].  
  
```sql  
ALTER INDEX ALL ON Production.Product  
REBUILD WITH (FILLFACTOR = 80, SORT_IN_TEMPDB = ON, STATISTICS_NORECOMPUTE = ON);  
```  
  
L'exemple suivant ajoute l'option ONLINE incluant l'option de verrou de faible priorité, et ajoute l'option de compression de ligne.  
  
**S’applique à** : [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (à compter de [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)]) et [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].  
  
```sql  
ALTER INDEX ALL ON Production.Product  
REBUILD WITH   
(  
    FILLFACTOR = 80,   
    SORT_IN_TEMPDB = ON,  
    STATISTICS_NORECOMPUTE = ON,  
    ONLINE = ON ( WAIT_AT_LOW_PRIORITY ( MAX_DURATION = 4 MINUTES, ABORT_AFTER_WAIT = BLOCKERS ) ),   
    DATA_COMPRESSION = ROW  
);  
```  
  
### <a name="c-reorganizing-an-index-with-lob-compaction"></a>C. Réorganisation d'un index avec compactage LOB  
 L'exemple suivant réorganise un seul index cluster dans la base de données [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)]. L'index contenant un type de données LOB au niveau de la feuille, l'instruction compacte par la même occasion toutes les pages contenant les données LOB. Notez que l’indication de l’option WITH (LOB_COMPACTION = ON) n’est pas nécessaire car la valeur par défaut est ON.  
  
```sql  
ALTER INDEX PK_ProductPhoto_ProductPhotoID ON Production.ProductPhoto REORGANIZE WITH (LOB_COMPACTION = ON);  
```  
  
### <a name="d-setting-options-on-an-index"></a>D. Définition des options d'un index  
 L'exemple suivant définit plusieurs options sur l'index `AK_SalesOrderHeader_SalesOrderNumber` dans la base de données [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)].  
  
**S’applique à** : [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (à compter de [!INCLUDE[ssKatmai](../../includes/ssKatmai-md.md)]) et [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].  
  
```sql  
ALTER INDEX AK_SalesOrderHeader_SalesOrderNumber ON  
    Sales.SalesOrderHeader  
SET (  
    STATISTICS_NORECOMPUTE = ON,  
    IGNORE_DUP_KEY = ON,  
    ALLOW_PAGE_LOCKS = ON  
    ) ;  
GO
```  
  
### <a name="e-disabling-an-index"></a>E. Désactivation d'un index  
 L'exemple suivant désactive un index non cluster sur la table `Employee` dans la base de données [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)].  
  
```sql  
ALTER INDEX IX_Employee_ManagerID ON HumanResources.Employee DISABLE;
```  
  
### <a name="f-disabling-constraints"></a>F. Désactivation des contraintes  
 L’exemple suivant désactive une contrainte PRIMARY KEY en désactivant l’index PRIMARY KEY dans la base de données [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)]. La contrainte FOREIGN KEY portant sur la table sous-jacente est automatiquement désactivée et un avertissement s'affiche.  
  
```sql  
ALTER INDEX PK_Department_DepartmentID ON HumanResources.Department DISABLE;  
```  
  
Le jeu de résultats retourne l'avertissement suivant.  
  
```  
Warning: Foreign key 'FK_EmployeeDepartmentHistory_Department_DepartmentID'  
on table 'EmployeeDepartmentHistory' referencing table 'Department'  
was disabled as a result of disabling the index 'PK_Department_DepartmentID'.
```  
  
### <a name="g-enabling-constraints"></a>G. Activation des contraintes  
 L'exemple suivant active les contraintes PRIMARY KEY et FOREIGN KEY désactivées dans l'exemple F.  
  
La contrainte PRIMARY KEY est activée lors de la reconstruction de l'index PRIMARY KEY.  
  
```sql  
ALTER INDEX PK_Department_DepartmentID ON HumanResources.Department REBUILD;  
```  
  
La contrainte FOREIGN KEY est ensuite activée.  
  
```sql  
ALTER TABLE HumanResources.EmployeeDepartmentHistory  
CHECK CONSTRAINT FK_EmployeeDepartmentHistory_Department_DepartmentID;  
GO  
```  
  
### <a name="h-rebuilding-a-partitioned-index"></a>H. Reconstruction d'un index partitionné  
 L'exemple suivant reconstruit une seule partition, celle portant le numéro de partition `5`, de l'index partitionné `IX_TransactionHistory_TransactionDate` dans la base de données [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)]. La partition 5 est reconstruite en ligne et le délai d'attente de 10 minutes pour le verrou de priorité basse s'applique séparément à chaque verrou acquis par l'opération de reconstruction d'index. Si pendant ce temps, le verrou ne peut pas être obtenu pour terminer la reconstruction de l'index complet, l'instruction de l'opération de reconstruction est interrompue.  
  
**S’applique à** : [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (à compter de [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)]) et [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].  
  
```sql  
-- Verify the partitioned indexes.  
SELECT *  
FROM sys.dm_db_index_physical_stats (DB_ID(),OBJECT_ID(N'Production.TransactionHistory'), NULL , NULL, NULL);  
GO  
--Rebuild only partition 5.  
ALTER INDEX IX_TransactionHistory_TransactionDate  
ON Production.TransactionHistory  
REBUILD Partition = 5   
   WITH (ONLINE = ON (WAIT_AT_LOW_PRIORITY (MAX_DURATION = 10 minutes, ABORT_AFTER_WAIT = SELF)));  
GO  
```  
  
### <a name="i-changing-the-compression-setting-of-an-index"></a>I. Modification du paramètre de compression d'un index  
 L'exemple suivant reconstruit un index sur une table rowstore non partitionnée.  
  
```sql
ALTER INDEX IX_INDEX1   
ON T1  
REBUILD   
WITH (DATA_COMPRESSION = PAGE);  
GO  
```  
  
Pour obtenir d’autres exemples de compression de données, consultez [Compression de données](../../relational-databases/data-compression/data-compression.md).  
 
### <a name="j-online-resumable-index-rebuild"></a>J. Reconstruction d’index pouvant être reprise en ligne

**S’applique à** : [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (à compter de [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)]) et [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].   

 Les exemples suivants montrent comment utiliser une reconstruction d’index pouvant être reprise en ligne. 

1. Exécutez une reconstruction d’index en ligne en tant qu’opération pouvant être reprise avec MAXDOP=1.

   ```sql
   ALTER INDEX test_idx on test_table REBUILD WITH (ONLINE=ON, MAXDOP=1, RESUMABLE=ON) ;
   ```

2. La réexécution de la même commande (voir ci-dessus) après qu’une opération d’index a été mise en pause reprend automatiquement l’opération de reconstruction d’index.

3. Exécutez une reconstruction d’index en ligne en tant qu’opération pouvant être reprise avec MAX_DURATION défini sur 240 minutes.

   ```sql
   ALTER INDEX test_idx on test_table REBUILD WITH (ONLINE=ON, RESUMABLE=ON, MAX_DURATION=240) ; 
   ```
4. Mettez en pause une reconstruction d’index en ligne pouvant être reprise en cours d’exécution.

   ```sql
   ALTER INDEX test_idx on test_table PAUSE ;
   ```   
5. Reprenez une reconstruction d’index en ligne pour une reconstruction d’index qui a été exécutée en tant qu’opération pouvant être reprise en spécifiant une nouvelle valeur pour MAXDOP de 4.

   ```sql
   ALTER INDEX test_idx on test_table RESUME WITH (MAXDOP=4) ;
   ```
6. Reprenez une opération de reconstruction d’index en ligne pour une reconstruction d’index en ligne qui a été exécutée comme pouvant être reprise. Définissez MAXDOP avec la valeur 2 et la durée d’exécution de l’index en cours d’exécution pouvant être repris avec la valeur 240 minutes. Si un index est bloqué sur le verrou, attendez 10 minutes puis tuez tous les bloqueurs. 

   ```sql
      ALTER INDEX test_idx on test_table  
         RESUME WITH (MAXDOP=2, MAX_DURATION= 240 MINUTES, 
         WAIT_AT_LOW_PRIORITY (MAX_DURATION=10, ABORT_AFTER_WAIT=BLOCKERS)) ;
   ```      
7. Abandonnez l’opération de reconstruction d’index pouvant être reprise qui est en cours d’exécution ou mise en pause.

   ```sql
   ALTER INDEX test_idx on test_table ABORT ;
   ``` 
  
## <a name="see-also"></a>Voir aussi  
[Guide de conception et d’architecture d’index SQL Server](../../relational-databases/sql-server-index-design-guide.md)     
[Exécuter des opérations d’index en ligne](../../relational-databases/indexes/perform-index-operations-online.md)    
[CREATE INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/create-index-transact-sql.md)     
[CREATE SPATIAL INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/create-spatial-index-transact-sql.md)   
[CREATE XML INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/create-xml-index-transact-sql.md)   
[DROP INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/drop-index-transact-sql.md)   
[Désactiver les index et les contraintes](../../relational-databases/indexes/disable-indexes-and-constraints.md)   
[Index XML &#40;SQL Server&#41;](../../relational-databases/xml/xml-indexes-sql-server.md)   
[Réorganiser et reconstruire des index](../../relational-databases/indexes/reorganize-and-rebuild-indexes.md)   
[sys.dm_db_index_physical_stats &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-index-physical-stats-transact-sql.md)   
[EVENTDATA &#40;Transact-SQL&#41;](../../t-sql/functions/eventdata-transact-sql.md)    
