---
title: Détection et résolution des index fragmentés | Microsoft Docs
description: Cet article explique comment la fragmentation d’index se produit, détecter le volume de fragmentation et identifier la meilleure option pour résoudre la fragmentation d’index à l’aide de T-SQL et SQL Server Management Studio.
ms.custom: ''
ms.date: 03/19/2020
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.technology: table-view-index
ms.topic: conceptual
f1_keywords:
- sql13.swb.index.rebuild.f1
- sql13.swb.indexproperties.fragmentation.f1
- sql13.swb.index.reorg.f1
helpviewer_keywords:
- large object defragmenting
- indexes [SQL Server], reorganizing
- index reorganization [SQL Server]
- reorganizing indexes
- defragmenting large object data types
- index fragmentation [SQL Server]
- index rebuilding [SQL Server]
- rebuilding indexes
- indexes [SQL Server], rebuilding
- defragmenting indexes
- nonclustered indexes [SQL Server], defragmenting
- fragmentation [SQL Server]
- index defragmenting [SQL Server]
- LOB data [SQL Server], defragmenting
- clustered indexes, defragmenting
ms.assetid: a28c684a-c4e9-4b24-a7ae-e248808b31e9
author: pmasl
ms.author: mikeray
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 340160c64a64479fe8d194887c838dab3bb7468c
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/01/2021
ms.locfileid: "100353675"
---
# <a name="resolve-index-fragmentation-by-reorganizing-or-rebuilding-indexes"></a>Remédiez à la fragmentation des index en les réorganisant ou en les regénérant

[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Cet article explique comment la défragmentation d’index se produit et décrit son impact sur les performances des requêtes. Une fois que vous avez déterminé le [volume de fragmentation présent sur un index](#detecting-the-amount-of-fragmentation), vous pouvez procéder à la défragmentation en [réorganisant un index](#reorganize-an-index) ou [regénérant un index](#rebuild-an-index) en exécutant des commandes Transact-SQL dans l’outil que vous choisissez ou en utilisant SQL Server Management Studio.

## <a name="index-fragmentation-overview"></a>Vue d’ensemble de la fragmentation d’index

Qu’est-ce que la fragmentation d’index et pourquoi dois-je m’en soucier :

- La fragmentation intervient lorsque des index possèdent des pages dans lesquelles l’organisation logique au sein de l’index (reposant sur la valeur de la clé de celui-ci) ne correspond pas à l’organisation physique dans les pages d’index.
- Le [!INCLUDE[ssde_md](../../includes/ssde_md.md)] modifie automatiquement des index quand des opérations d’insertion, de mise à jour ou de suppression sont effectuées sur les données sous-jacentes. Par exemple, l’ajout de lignes dans une table peut entraîner le fractionnement des pages existantes dans les index rowstore afin de libérer de l’espace pour l’insertion de nouvelles valeurs de clés. Au fil des modifications, les informations figurant dans l'index sont éparpillées dans la base de données (fragmentée). La fragmentation intervient lorsque des index possèdent des pages dans lesquelles l'organisation logique (reposant sur la valeur de la clé) ne correspond pas à l'organisation physique dans le fichier de données.
- Les index fortement fragmentés peuvent dégrader les performances des requêtes, car des E/S supplémentaires sont requises pour localiser les données vers lesquelles pointe l’index. Davantage d’E/S provoquent une réponse lente de votre application, en particulier quand des opérations d’analyse sont impliquées.

## <a name="detecting-the-amount-of-fragmentation"></a>Détection du volume de fragmentation

La première étape pour choisir la méthode de défragmentation d’index à utiliser consiste à analyser l’index pour évaluer son degré de fragmentation. Vous détectez la fragmentation de façon différente pour les index rowstore et les index columnstore.

> [!NOTE]
> Il est particulièrement important d’examiner la fragmentation de l’index ou du segment de mémoire après avoir supprimé une grande quantité de données. Pour les segments de mémoire, en cas de mises à jour fréquentes, il peut également être nécessaire d’examiner la fragmentation pour éviter la prolifération des enregistrements de transfert. Pour plus d’informations sur les segments de mémoire, consultez [Segments de mémoire (tables sans index cluster)](../../relational-databases/indexes/heaps-tables-without-clustered-indexes.md#heap-structures). 

### <a name="detecting-fragmentation-of-rowstore-indexes"></a>Détection de la fragmentation sur les index rowstore

En utilisant [sys.dm_db_index_physical_stats](../../relational-databases/system-dynamic-management-views/sys-dm-db-index-physical-stats-transact-sql.md), vous pouvez détecter la fragmentation dans un index spécifique, dans tous les index d’une table ou d’une vue indexée, dans tous les index d’une base de données ou dans tous les index de l’ensemble des bases de données. Pour les index partitionnés, **sys.dm_db_index_physical_stats** procure aussi des informations de fragmentation pour chaque partition.

Le jeu de résultats retourné par **sys.dm_db_index_physical_stats** inclut les colonnes suivantes :

|Colonne|Description|
|------------|-----------------|
|**avg_fragmentation_in_percent**|Pourcentage de fragmentation logique (pages non ordonnées dans un index).|
|**fragment_count**|Nombre de fragments (pages de feuille consécutives physiquement) dans l'index.|
|**avg_fragment_size_in_pages**|Nombre moyen de pages dans un fragment d'un index.|

Une fois le degré de fragmentation connu, utilisez le tableau suivant pour identifier la méthode la mieux adaptée pour supprimer la fragmentation : [INDEX REORGANIZE](#reorganize-an-index) ou [INDEX](#rebuild-an-index).

|Valeur **avg_fragmentation_in_percent**|Instruction corrective|
|-----------------------------------------------|--------------------------|
|> 5 % et < = 30 % <sup>1</sup>|ALTER INDEX REORGANIZE|
|> 30 % <sup>1</sup>|ALTER INDEX REBUILD WITH (ONLINE = ON) <sup>2</sup>|

<sup>1</sup> Ces valeurs vous aident à déterminer le moment auquel vous devez basculer entre `ALTER INDEX REORGANIZE` et `ALTER INDEX REBUILD`. Toutefois, les valeurs réelles peuvent varier d'un cas à l'autre. Il est important que vous fassiez des essais pour déterminer le meilleur seuil pour votre environnement.      

> [!TIP] 
> Par exemple, si un index donné est principalement utilisé pour les opérations d’analyse, la suppression de la fragmentation peut améliorer les performances de ces opérations. L’amélioration des performances peut être moins perceptible pour les index qui sont utilisés principalement pour les opérations de recherche.    
De même, la suppression de la fragmentation dans un segment de mémoire (une table sans index cluster) est particulièrement utile pour les opérations d’analyse d’index non-cluster, mais n’a que peu d’effet dans les opérations de recherche.

<sup>2</sup> La reconstruction d’un index peut être exécutée en ligne ou hors connexion. La réorganisation d'un index s'effectue toujours en ligne. Pour obtenir le même niveau de disponibilité qu'avec l'option de réorganisation, vous devez reconstruire les index en ligne. Pour plus d’informations, consultez [INDEX](#rebuild-an-index) et [Exécuter des opérations en ligne sur les index](../../relational-databases/indexes/perform-index-operations-online.md).

Les index avec une fragmentation inférieure ou égale à 5 % n’ont pas besoin d’être défragmentés, car l’avantage de la suppression d’un volume de fragmentation aussi réduit est quasiment toujours largement contrebalancé par le coût lié au processeur induit pour réorganiser ou regénérer l’index. Par ailleurs, la regénération ou la réorganisation de petits index rowstore ne réduit généralement la fragmentation réelle. Jusqu’à [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] (compris), le [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] alloue de l’espace à l’aide d’étendues mixtes. Par conséquent, les pages de petits index sont parfois stockées sur des étendues mixtes. Les extensions mixtes sont partagées par huit objets maximum ; par conséquent, la fragmentation dans un petit index peut ne pas être réduite après sa réorganisation ou sa reconstruction. Consultez également [Considérations spécifiques à la regénération d’index rowstore](#considerations-specific-to-rebuilding-rowstore-indexes). Pour plus d’informations sur les étendues, consultez [Guide d’architecture des pages et des étendues](../../relational-databases/pages-and-extents-architecture-guide.md#extents).

### <a name="detecting-fragmentation-of-columnstore-indexes"></a>Détection de la fragmentation sur les index columnstore

En utilisant [sys.dm_db_column_store_row_group_physical_stats](../../relational-databases/system-dynamic-management-views/sys-dm-db-column-store-row-group-physical-stats-transact-sql.md), vous pouvez déterminer le pourcentage de lignes supprimées dans un index, ce qui constitue une mesure raisonnable pour la fragmentation d’un rowgroup dans un index columnstore. Utilisez ces informations pour calculer la fragmentation dans un index spécifique, dans tous les index d’une table, dans tous les index d’une base de données ou dans tous les index de toutes les bases de données.

Le jeu de résultats retourné par **sys.dm_db_column_store_row_group_physical_stats** inclut les colonnes suivantes :

|Colonne|Description|
|------------|-----------------|
|**total_rows**|Nombre de lignes stockées physiquement dans le groupe de lignes. Pour les groupes de lignes compressés, cela comprend les lignes qui sont marquées comme supprimées.|
|**deleted_rows**|Nombre de lignes physiquement stockées dans un groupe de lignes compressé marqué pour suppression. 0 pour les groupes de lignes qui se trouvent dans le magasin delta.|

Utilisez ces informations retournées pour calculer la fragmentation de l’index à l’aide de la formule suivante :

```
100*(ISNULL(deleted_rows,0))/NULLIF(total_rows,0)
```

Une fois le degré de fragmentation d’index connu, utilisez le tableau suivant pour identifier la méthode la mieux adaptée pour supprimer la fragmentation : [INDEX REORGANIZE](#reorganize-an-index) ou [INDEX](#rebuild-an-index).

|**fragmentation calculée en pourcentage**|S’applique à cette version|Instruction corrective|
|-----------------------------------------------|--------------------------|--------------------------|
|> = 20 %|[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] et [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)]|ALTER INDEX REBUILD|
|> = 20 %|À compter de [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)]|ALTER INDEX REORGANIZE|

### <a name="to-check-the-fragmentation-of-a-rowstore-index-using-tsql"></a>Pour vérifier la fragmentation d’un index rowstore avec [!INCLUDE[tsql](../../includes/tsql-md.md)]

L’exemple suivant recherche le pourcentage moyen de fragmentation de tous les index dans la table `HumanResources.Employee` de la base de données`AdventureWorks2016`.

```sql
SELECT a.object_id, object_name(a.object_id) AS TableName,
    a.index_id, name AS IndedxName, avg_fragmentation_in_percent
FROM sys.dm_db_index_physical_stats
    (DB_ID (N'AdventureWorks2016_EXT')
        , OBJECT_ID(N'HumanResources.Employee')
        , NULL
        , NULL
        , NULL) AS a
INNER JOIN sys.indexes AS b
    ON a.object_id = b.object_id
    AND a.index_id = b.index_id;
GO
```

L'instruction précédente retourne un jeu de résultats similaire au suivant.

```
object_id   TableName    index_id    IndexName                                             avg_fragmentation_in_percent
----------- ------------ ----------- ----------------------------------------------------- ------------------------------
1557580587  Employee     1           PK_Employee_BusinessEntityID                          0
1557580587  Employee     2           IX_Employee_OrganizationalNode                        0
1557580587  Employee     3           IX_Employee_OrganizationalLevel_OrganizationalNode    0
1557580587  Employee     5           AK_Employee_LoginID                                   66.6666666666667
1557580587  Employee     6           AK_Employee_NationalIDNumber                          50
1557580587  Employee     7           AK_Employee_rowguid                                   0

(6 row(s) affected)
```

Pour plus d’informations, consultez [sys.dm_db_index_physical_stats](../../relational-databases/system-dynamic-management-views/sys-dm-db-index-physical-stats-transact-sql.md).

### <a name="to-check-the-fragmentation-of-a-columnstore-index-using-transact-sql"></a>Vérification de la fragmentation d’un index columnstore avec Transact-SQL

L’exemple suivant recherche le pourcentage moyen de fragmentation de tous les index dans la table `dbo.FactResellerSalesXL_CCI` de la base de données`AdventureWorksDW2016`.

```sql
SELECT i.object_id,
    object_name(i.object_id) AS TableName,
    i.index_id,
    i.name AS IndexName,
    100*(ISNULL(SUM(CSRowGroups.deleted_rows),0))/NULLIF(SUM(CSRowGroups.total_rows),0) AS 'Fragmentation'
FROM sys.indexes AS i  
INNER JOIN sys.dm_db_column_store_row_group_physical_stats AS CSRowGroups
    ON i.object_id = CSRowGroups.object_id
    AND i.index_id = CSRowGroups.index_id
WHERE object_name(i.object_id) = 'FactResellerSalesXL_CCI'
GROUP BY i.object_id, i.index_id, i.name
ORDER BY object_name(i.object_id), i.name;
```

L'instruction précédente retourne un jeu de résultats similaire au suivant.

```
object_id   TableName                   index_id    IndexName                       Fragmentation
----------- --------------------------- ----------- ------------------------------- ---------------
114099447   FactResellerSalesXL_CCI     1           IndFactResellerSalesXL_CCI      0

(1 row(s) affected)
```

### <a name="check-index-fragmentation-using-sql-server-management-studio"></a>Vérifier la fragmentation d’un index avec SQL Server Management Studio

> [!NOTE]
> [!INCLUDE[ssManStudio](../../includes/ssManStudio-md.md)] ne peut pas être utilisé pour calculer la fragmentation des index columnstore dans SQL Server et ne peut pas être utilisé pour calculer la fragmentation des index dans Azure SQL Database. Utilisez [l’exemple](#to-check-the-fragmentation-of-a-columnstore-index-using-transact-sql) [!INCLUDE[tsql](../../includes/tsql-md.md)] précédent.

1. Dans l’Explorateur d’objets, développez la base de données qui contient la table sur laquelle vous souhaitez vérifier une fragmentation d’index.
2. Développez le dossier **Tables** .
3. Développez la table sur laquelle vous souhaitez vérifier une fragmentation d’index.
4. Développez le dossier **Index** .
5. Cliquez avec le bouton droit sur l’index dont vous voulez vérifier la fragmentation, puis sélectionnez **Propriétés**.
6. Sous **Sélectionner une page**, sélectionnez **Fragmentation**.

Les informations suivantes sont disponibles dans la page **Fragmentation** :

|Valeur|Description|
|---|---|
|**Remplissage de la page**|Indique le remplissage moyen des pages d'index, sous forme de pourcentage. 100% signifie que les pages d'index sont totalement remplies. 50% signifie qu'en moyenne, chaque page d'index est remplie à moitié.|
|**Fragmentation totale**|Pourcentage de fragmentation logique. Cette valeur indique le nombre de pages dans un index qui ne sont pas stockées dans l'ordre.|
|**Taille moyenne de ligne**|Taille moyenne d’une ligne de niveau feuille.|
|**Profondeur**|Nombre de niveaux dans l’index, notamment le niveau feuille.|
|**Enregistrements transférés**|Nombre d'enregistrements d'un segment qui contiennent des pointeurs avant vers un autre emplacement de données. (Cet état se produit pendant une mise à jour, lorsque l'espace disponible est insuffisant pour stocker la nouvelle ligne à l'emplacement d'origine.)|
|**Lignes fantômes**|Nombre de lignes marquées pour la suppression qui ne sont pas encore supprimées. Ces lignes seront supprimées par un thread de nettoyage, lorsque le serveur n'est pas occupé. Cette valeur ne comprend pas les lignes qui sont conservées à cause d'une transaction d'isolement d'instantané en suspens.|
|**Type d'index**|Type de l'index. Les valeurs possibles sont **Index cluster**, **Index non-cluster** et **XML primaire**. Les tables peuvent également être stockées en tant que segment (sans index), mais dans ce cas la page Propriétés de l'index est impossible à ouvrir.|
|**Lignes de niveau feuille**|Nombre de lignes de niveau feuille.|
|**Taille maximale de ligne**|Taille maximale des lignes de niveau feuille.|
|**Taille minimale de ligne**|Taille minimale des lignes de niveau feuille.|
|**Pages**|Nombre total de pages de données.|
|**ID de partition**|ID de partition de l'arbre B (B-tree) qui contient l'index.|
|**Enregistrement de version fantôme**|Nombre d'enregistrements fantômes étant conservés en raison d'une transaction d'isolement d'instantané en attente.|

## <a name="defragmenting-indexes-by-rebuilding-or-reorganizing-the-index"></a>Défragmentation des index en les regénérant ou en les réorganisant

Pour défragmenter un index fragmenté, utilisez l’une des méthodes suivantes :

- Réorganisation de l’index
- Regénération de l’index

> [!NOTE]
> Dans le cas d’index partitionnés reposant sur un schéma de partition, vous pouvez utiliser les méthodes suivantes sur la totalité ou sur une partition unique d’un index.

### <a name="reorganize-an-index"></a>Réorganiser un index

La réorganisation d’un index utilise des ressources système minimes et est une opération en ligne. En d’autres termes, les verrous de tables bloquants à long terme ne sont pas conservés, ce qui permet aux requêtes et aux mises à jour de la table sous-jacente de se poursuivre pendant la transaction `ALTER INDEX REORGANIZE`.

- Pour les [index rowstore](clustered-and-nonclustered-indexes-described.md), [!INCLUDE[ssde_md](../../includes/ssde_md.md)] défragmente le niveau feuille des index cluster et non cluster sur les tables et les vues en réorganisant physiquement les pages de niveau feuille afin de suivre l’ordre logique des nœuds terminaux (de gauche à droite). La réorganisation compacte également les pages d’index en fonction de la valeur du facteur de remplissage de l’index. Pour afficher le paramètre du facteur de remplissage, utilisez [sys.indexes](../../relational-databases/system-catalog-views/sys-indexes-transact-sql.md). Pour obtenir des exemples de syntaxe, consultez [Exemples : réorganisation d’index rowstore](../../t-sql/statements/alter-index-transact-sql.md#examples-rowstore-indexes).

- Lors de l’utilisation des [index columnstore](columnstore-indexes-overview.md), le deltastore peut se retrouver avec plusieurs petits rowgroups après l’insertion, la mise à jour et la suppression des données au fil du temps. La réorganisation d’un index columnstore force tous les rowgroups dans le columnstore, puis combine les rowgroups en un plus petit nombre de rowgroups avec davantage de lignes. L’opération de réorganisation supprime également les lignes qui ont été supprimées du columnstore. La réorganisation nécessite initialement des ressources processeur supplémentaires pour compresser les données, ce qui peut ralentir les performances globales du système. Toutefois, dès que les données sont compressées, les performances des requêtes s’améliorent. Pour obtenir des exemples de syntaxe, consultez [Exemples : réorganisation d’index columnstore](../../t-sql/statements/alter-index-transact-sql.md#examples-columnstore-indexes).

### <a name="rebuild-an-index"></a>Regénérer un index

La reconstruction d'un index entraîne sa suppression puis sa recréation. En fonction du type d’index et de la version de [!INCLUDE[ssde_md](../../includes/ssde_md.md)], une opération de regénération peut être effectuée en ligne ou hors connexion. Pour la syntaxe T-SQL, consultez [ALTER INDEX REBUILD](../../t-sql/statements/alter-index-transact-sql.md#rebuilding-indexes)

- Pour les [index rowstore](clustered-and-nonclustered-indexes-described.md), la regénération permet d’éviter toute fragmentation, de libérer de l’espace disque en compactant les pages d’après le paramètre du facteur de remplissage spécifié ou déjà existant et de réorganiser les lignes de l’index en pages contiguës. Si `ALL` est précisé, tous les index sur la table sont supprimés puis reconstruits en une seule transaction. Il n’est pas nécessaire de supprimer les contraintes de clé étrangère au préalable. Lorsque de la reconstruction d'index contenant au moins 128 étendues, le [!INCLUDE[ssDE](../../includes/ssde-md.md)] diffère les désallocations de pages ainsi que les verrous qui y sont associés jusqu'à ce que la transaction soit validée. Pour obtenir des exemples de syntaxe, consultez [Exemples : réorganisation d’index rowstore](../../t-sql/statements/alter-index-transact-sql.md#examples-rowstore-indexes).

- Pour les [index columnstore](columnstore-indexes-overview.md), la regénération permet d’éviter toute fragmentation, de déplacer toutes les lignes dans le columnstore et de libérer de l’espace disque en supprimant physiquement les lignes qui ont été logiquement supprimées de la table. 
  
  > [!TIP]
  > À compter de [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)], la régénération de l’index columnstore n’est généralement pas nécessaire, car `REORGANIZE` effectue l’essentiel de la régénération en arrière-plan sous la forme d’une opération en ligne. 
  
  Pour obtenir des exemples de syntaxe, consultez [Exemples : régénérer ColumnStore](../../t-sql/statements/alter-index-transact-sql.md#examples-columnstore-indexes).

### <a name="permissions"></a><a name="Permissions"></a> Autorisations

Nécessite l’autorisation `ALTER` sur la table ou la vue. L’utilisateur doit être membre d’au moins l’un des rôles suivants :

- Rôle de base de données **db_ddladmin**<sup>1</sup>
- Rôle de base de données **db_owner**
- Rôle serveur **sysadmin**

<sup>1</sup> Le rôle de base de données **db_ddladmin** est le [moins privilégié](/windows-server/identity/ad-ds/plan/security-best-practices/implementing-least-privilege-administrative-models).

### <a name="remove-fragmentation-using-ssmanstudiofull"></a><a name="SSMSProcedureReorg"></a> Supprimer la fragmentation à l’aide de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]

#### <a name="to-reorganize-or-rebuild-an-index"></a>Pour réorganiser ou reconstruire un index

1. Dans l'Explorateur d'objets, développez la base de données qui contient la table sur laquelle vous souhaitez réorganiser un index.
2. Développez le dossier **Tables** .
3. Développez la table sur laquelle vous souhaitez réorganiser un index.
4. Développez le dossier **Index** .
5. Cliquez avec le bouton droit sur l’index que vous souhaitez réorganiser et sélectionnez **Réorganiser**.
6. Dans la boîte de dialogue **Réorganiser les index** , vérifiez que l'index correct figure dans la grille **Index à réorganiser** , puis cliquez sur **OK**.
7. Cochez la case **Compacter les données de la colonne d’objets volumineux** pour indiquer que toutes les pages qui contiennent des données LOB seront aussi compactées.
8. Cliquez sur **OK**.

#### <a name="to-reorganize-all-indexes-in-a-table"></a>Pour réorganiser tous les index d'une table

1. Dans l'Explorateur d'objets, développez la base de données qui contient la table sur laquelle vous souhaitez réorganiser les index.
2. Développez le dossier **Tables** .
3. Développez la table sur laquelle vous souhaitez réorganiser les index.
4. Cliquez avec le bouton droit sur le dossier **Index** , puis sélectionnez **Réorganiser tout**.
5. Dans la boîte de dialogue **Réorganiser les index** , vérifiez que les index corrects sont dans **Index à réorganiser**. Pour supprimer un index de la grille **Index à réorganiser** , sélectionnez l'index et appuyez sur la touche SUPPR.
6. Cochez la case **Compacter les données de la colonne d’objets volumineux** pour indiquer que toutes les pages qui contiennent des données LOB seront aussi compactées.
7. Cliquez sur **OK**.

#### <a name="to-rebuild-an-index"></a>Pour reconstruire un index

1. Dans l'Explorateur d'objets, développez la base de données qui contient la table sur laquelle vous souhaitez réorganiser un index.
2. Développez le dossier **Tables** .
3. Développez la table sur laquelle vous souhaitez réorganiser un index.
4. Développez le dossier **Index** .
5. Cliquez avec le bouton droit sur l’index que vous souhaitez réorganiser et sélectionnez **Regénérer**.
6. Dans la boîte de dialogue **Reconstruire les index** , vérifiez que l'index correct figure dans la grille **Index à reconstruire** , puis cliquez sur **OK**.
7. Cochez la case **Compacter les données de la colonne d’objets volumineux** pour indiquer que toutes les pages qui contiennent des données LOB seront aussi compactées.
8. Cliquez sur **OK**.

### <a name="remove-fragmentation-using-tsql"></a><a name="TsqlProcedureReorg"></a> Supprimer la fragmentation à l’aide de [!INCLUDE[tsql](../../includes/tsql-md.md)]

> [!NOTE]
> Pour obtenir plus d’exemples sur l’utilisation de [!INCLUDE[tsql](../../includes/tsql-md.md)] pour régénérer ou réorganiser des index, consultez [Exemples ALTER INDEX : index columnstore](../../t-sql/statements/alter-index-transact-sql.md#examples-columnstore-indexes) et [Exemples ALTER INDEX : index rowstore](../../t-sql/statements/alter-index-transact-sql.md#examples-rowstore-indexes).

#### <a name="to-reorganize-a-fragmented-index"></a>Pour réorganiser un index fragmenté

L’exemple suivant réorganise l’index `IX_Employee_OrganizationalLevel_OrganizationalNode` sur la table `HumanResources.Employee` de la base de données `AdventureWorks2016`.

```sql
ALTER INDEX IX_Employee_OrganizationalLevel_OrganizationalNode
    ON HumanResources.Employee
    REORGANIZE;
```

L’exemple suivant réorganise l’index columnstore `IndFactResellerSalesXL_CCI` sur la table `dbo.FactResellerSalesXL_CCI` de la base de données `AdventureWorksDW2016`.

```sql
-- This command will force all CLOSED and OPEN rowgroups into the columnstore.
ALTER INDEX IndFactResellerSalesXL_CCI
    ON FactResellerSalesXL_CCI
    REORGANIZE WITH (COMPRESS_ALL_ROW_GROUPS = ON);
```

#### <a name="to-reorganize-all-indexes-in-a-table"></a>Pour réorganiser tous les index d'une table

L’exemple suivant réorganise tous les index sur la table `HumanResources.Employee` de la base de données `AdventureWorks2016`.

```sql
ALTER INDEX ALL ON HumanResources.Employee
   REORGANIZE;
```

#### <a name="to-rebuild-a-fragmented-index"></a>Pour reconstruire un index fragmenté

L'exemple suivant reconstruit un seul index portant sur la table `Employee` de la base de données `AdventureWorks2016`.

[!code-sql[IndexDDL#AlterIndex1](../../relational-databases/indexes/codesnippet/tsql/reorganize-and-rebuild-i_1.sql)]

#### <a name="to-rebuild-all-indexes-in-a-table"></a>Pour reconstruire tous les index d'une table

L’exemple suivant régénère tous les index associés à la table dans la base de données `AdventureWorks2016` à l’aide du mot clé `ALL`. Trois options sont spécifiées.

[!code-sql[IndexDDL#AlterIndex2](../../relational-databases/indexes/codesnippet/tsql/reorganize-and-rebuild-i_2.sql)]

Pour plus d’informations, consultez [ALTER INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/alter-index-transact-sql.md).

#### <a name="automatic-index-and-statistics-management"></a>Gestion automatique des index et des statistiques

Tirez parti de solutions comme [Adaptive Index Defrag](https://github.com/Microsoft/tigertoolbox/tree/master/AdaptiveIndexDefrag) pour gérer automatiquement la défragmentation des index et les mises à jour des statistiques pour une ou plusieurs bases de données. Cette procédure choisit automatiquement s’il faut reconstruire ou réorganiser un index en fonction de son niveau de fragmentation, entre autres, et mettre à jour les statistiques avec un seuil linéaire.

## <a name="considerations-specific-to-rebuilding-rowstore-indexes"></a>Considérations spécifiques à la regénération d’index columnstore

La regénération d’un index cluster regénère automatiquement tout index non cluster qui référence la clé de clustering, si les identificateurs physiques ou logiques contenus dans les enregistrements d’index non cluster doivent être modifiés.

Les scénarios suivants forcent la regénération automatique de tous les index non cluster rowstore sur une table :

- Création d’un index cluster sur une table
- Suppression d’un index cluster, ce qui aboutit au stockage de la table en tant que segment de mémoire
- Modification de la clé de clustering pour inclure ou exclure des colonnes

Les scénarios suivants ne nécessitent pas la regénération automatique de tous les index non cluster rowstore sur une table :

- Recréation d’un index cluster unique
- Recréation d’un index cluster non unique
- Modification du schéma d’index, telle que l’application d’un schéma de partitionnement à un index cluster ou le déplacement de l’index cluster vers un autre groupe de fichiers

> [!IMPORTANT]
> Un index ne peut pas être réorganisé ou reconstruit si le groupe de fichiers dans lequel il se trouve est hors connexion ou en lecture seule. Si le mot clé ALL est spécifié et que des index se trouvent dans un groupe de fichiers hors connexion ou en lecture seule, l'instruction échoue.
>
> En cas de régénération d’un index, le média physique doit avoir suffisamment d’espace pour stocker deux copies de l’index. Lorsque la régénération est terminée, [!INCLUDE[ssde_md](../../includes/ssde_md.md)] supprime l’index d’origine.

Si `ALL` est spécifié avec l’instruction `ALTER INDEX`, les index relationnels, aussi bien en cluster que non cluster, et les index XML de la table sont réorganisés.

## <a name="considerations-specific-to-rebuilding-a-columnstore-index"></a>Considérations spécifiques à la régénération d’un index columnstore

Lors de la régénération d’un index columnstore, [!INCLUDE[ssde_md](../../includes/ssde_md.md)] lit toutes les données de ’'index columnstore d’origine, y compris le magasin delta. Associe les données dans de nouveaux rowgroups, et compresse les rowgroups dans le columnstore. [!INCLUDE[ssde_md](../../includes/ssde_md.md)] défragmente le columnstore en supprimant physiquement les lignes qui ont été logiquement supprimées de la table. Les octets supprimés sont récupérés sur le disque.

> [!NOTE]
> La réorganisation d’un index columnstore à l’aide de [!INCLUDE[ssManStudio](../../includes/ssManStudio-md.md)] combine COMPRESSED rowgroups ensemble, mais ne force pas tous les rowgroups à être compressés dans le columnstore. Les rowgroups CLOSED sont compressés, mais les rowgroups OPEN ne sont pas compressés dans le columnstore. Pour compresser de force toutes les rowgroups, utilisez l’exemple [!INCLUDE[tsql](../../includes/tsql-md.md)][ci-dessous](#TsqlProcedureReorg).

> [!NOTE]
> À partir de [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)], le moteur de tuple est aidé par une tâche de fusion en arrière-plan qui compresse automatiquement les rowgroups delta OPEN plus petits qui existent depuis un certain temps, tel que déterminé par un seuil interne, ou qui fusionne les rowgroups COMPRESSED à partir desquels un grand nombre de lignes a été supprimé. Cela améliore la qualité de l’index columnstore dans le temps.    
> Pour plus d’informations sur les termes et les concepts columnstore, consultez [Index Columnstore : Vue d’ensemble](../../relational-databases/indexes/columnstore-indexes-overview.md).

### <a name="rebuild-a-partition-instead-of-the-entire-table"></a>Regénérer une partition au lieu de la table entière

- Reconstruire un index columnstore cluster entier prend beaucoup de temps si l'index est volumineux, et cela nécessite suffisamment d'espace disque pour stocker une copie supplémentaire de l'index pendant la reconstruction. Généralement il est nécessaire de reconstruire que la dernière partition utilisée.
- Pour les tables partitionnées, vous n'avez pas besoin de reconstruire l'index columnstore entier, car la fragmentation se produira probablement uniquement dans les partitions modifiées récemment. Les tables de faits et de dimension volumineuses sont généralement partitionnées pour exécuter des opérations de sauvegarde et de gestion sur les segments de la table.

### <a name="rebuild-a-partition-after-heavy-dml-operations"></a>Regénérer une partition après des opérations DML lourdes

La regénération d’une partition permet de la défragmenter et de réduire le stockage sur disque. La regénération supprime toutes les lignes marquées pour suppression du columnstore et déplace tous les rowgroups du deltastore dans le columnstore. Plusieurs rowgroups dans le deltastore peuvent contenir chacun moins d’un million de lignes.

### <a name="rebuild-a-partition-after-loading-data"></a>Regénérer une partition après le chargement des données

La regénération d’une partition après le chargement des données garantit que toutes les données sont stockées dans le columnstore. Lorsque des processus simultanés chargent moins 100 000 lignes chacun dans la même partition en même temps, la partition peut se retrouver avec plusieurs magasins delta. La regénération déplace toutes les lignes du deltastore dans le columnstore.

## <a name="considerations-specific-to-reorganizing-a-columnstore-index"></a>Considérations spécifiques à la réorganisation d’un index columnstore

Lors de la réorganisation des index columnstore, [!INCLUDE[ssde_md](../../includes/ssde_md.md)] compresse chaque rowgroup delta CLOSED dans le columnstore en tant que rowgroup compressé. À compter de [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] et dans [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)], la commande `REORGANIZE` effectue les optimisations de défragmentation supplémentaires suivantes en ligne :

- Supprime physiquement les lignes d’un rowgroup quand au moins 10 % des lignes ont été supprimées de façon logique. Les octets supprimés sont récupérés sur le support physique. Par exemple, si un rowgroup compressé de 1 million de lignes a 100 000 lignes supprimées, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] efface les lignes supprimées et recompresse le rowgroup avec 900 000 lignes. Il économise du stockage en effaçant les lignes supprimées.

- Associe un ou plusieurs rowgroups compressés pour augmenter les lignes par rowgroup jusqu’à la valeur maximale de 1 048 576 lignes. Par exemple, si vous importez en bloc 5 lots de 102 400 lignes, vous obtenez 5 rowgroups compressés. Si vous exécutez REORGANIZE, ces rowgroups sont fusionnés dans 1 rowgroup compressé de 512 000 lignes. Cela suppose qu’il n’existe aucune limitation de mémoire ni de taille de dictionnaire.

- Pour les rowgroups dans lesquels au moins 10 % des lignes ont été supprimées de manière logique, [!INCLUDE[ssde_md](../../includes/ssde_md.md)] tente d’associer ce rowgroup à un ou plusieurs rowgroups. Par exemple, le rowgroup 1 est compressé avec 500 000 lignes et le rowgroup 21 est compressé avec un maximum de 1 048 576 lignes. Le rowgroup 21 a 60 % des lignes supprimées, ce qui laisse 409 830 lignes. [!INCLUDE[ssde_md](../../includes/ssde_md.md)] favorise la combinaison de ces deux rowgroups pour compresser un nouveau rowgroup qui contient 909 830 lignes.

Après avoir exécuté des charges de données, vous pouvez avoir plusieurs petits rowgroups dans le magasin delta. Vous pouvez utiliser l’instruction `ALTER INDEX REORGANIZE` pour forcer tous les rowgroups dans le columnstore, puis pour combiner les rowgroups en un plus petit nombre de rowgroups avec plusieurs lignes. L’opération de réorganisation supprimera également les lignes qui ont été supprimées du columnstore.

## <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> Limitations et restrictions

Les index rowstore possédant plus de 128 extensions sont régénérés en deux phases distinctes : une phase logique et une phase physique. Dans la phase logique, les unités d'allocation utilisées par l'index sont signalées comme devant être désallouées, les lignes de données sont copiées et triées, puis elles sont déplacées vers les nouvelles unités d'allocation ayant été créées pour stocker l'index reconstruit. Dans la phase physique, les unités d'allocation préalablement signalées pour être désallouées sont supprimées physiquement dans des transactions courtes qui interviennent en arrière-plan et nécessitent peu de verrous. Pour plus d’informations sur les étendues, consultez [Guide d’architecture des pages et des étendues](../../relational-databases/pages-and-extents-architecture-guide.md).

L’instruction `ALTER INDEX REORGANIZE` a besoin du fichier de données contenant l’index pour disposer d’espace, car l’opération peut uniquement allouer des pages de travail temporaires sur le même fichier, et non dans un autre fichier du groupe de fichiers. De ce fait, même si le groupe de fichiers a des pages libres, l’utilisateur peut toujours rencontrer l’erreur 1105 : `Could not allocate space for object '###' in database '###' because the '###' filegroup is full. Create disk space by deleting unneeded files, dropping objects in the filegroup, adding additional files to the filegroup, or setting autogrowth on for existing files in the filegroup.`

> [!WARNING]
> La création et la reconstruction des index non alignés sur une table contenant plus de 1 000 partitions sont possibles, mais ne sont pas prises en charge. Ces opérations peuvent entraîner une dégradation des performances ou une consommation de mémoire excessive. Microsoft recommande d’utiliser uniquement des [index alignés](../partitions/partitioned-tables-and-indexes.md#aligned-index) lorsque le nombre de partitions est supérieur à 1 000.

Un index ne peut pas être réorganisé ou régénéré si le groupe de fichiers dans lequel il se trouve est **hors connexion** ou **en lecture seule**. Si le mot clé `ALL` est spécifié et qu’un ou plusieurs index se trouvent dans un groupe de fichiers hors connexion ou en lecture seule, l’instruction échoue.

Statistiques :

- Quand un index est **créé** ou **regénéré**, les statistiques sont créées ou mises à jour par l’analyse de toutes les lignes de la table. En revanche, à partir de [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)], les statistiques ne sont pas créées ou mises à jour par l’analyse de toutes les lignes de la table au moment où un index partitionné est créé ou reconstruit. Au lieu de cela, l’optimiseur de requête se sert de l’algorithme d’échantillonnage par défaut pour générer ces statistiques. Pour obtenir des statistiques sur les index partitionnés en analysant toutes les lignes de la table, utilisez [CREATE STATISTICS](../../t-sql/statements/create-statistics-transact-sql.md) ou [UPDATE STATISTICS](../../t-sql/statements/update-statistics-transact-sql.md) avec la clause `FULLSCAN`.

- Quand un index est **réorganisé**, les statistiques ne sont pas mises à jour.

Un index ne peut pas être réorganisé lorsque `ALLOW_PAGE_LOCKS` est désactivé (OFF).

Jusqu’à [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)], la régénération d’un index columnstore en cluster est une opération hors connexion. Le moteur de base de données doit acquérir un verrou exclusif sur la table ou la partition lorsque la regénération se produit. Les données sont hors connexion et indisponibles pendant la régénération, même si vous utilisez `NOLOCK`, l’isolation de capture instantanée de lecture validée (RCSI) ou l’isolation de capture instantanée.
À compter de [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)], un index columnstore en cluster peut être régénéré à l’aide de l’option `ONLINE = ON`.

Pour une table Azure Synapse Analytics (anciennement [!INCLUDE[ssSDW](../../includes/sssdw-md.md)]) avec un index columnstore cluster ordonné, `ALTER INDEX REBUILD` trie à nouveau les données à l’aide de TempDB. Analysez TempDB pendant les opérations de régénération. Si vous avez besoin de davantage d’espace TempDB, vous pouvez effectuer une montée en puissance de l’entrepôt de données. Réeffectuez un scale down une fois la reconstruction d’index terminée.

Pour une table Azure Synapse Analytics (anciennement [!INCLUDE[ssSDW](../../includes/sssdw-md.md)]) avec un index columnstore cluster ordonné, `ALTER INDEX REORGANIZE` ne trie pas à nouveau les données. Pour trier à nouveau l’utilisation de données `ALTER INDEX REBUILD`.

## <a name="using-index-rebuild-to-recover-from-hardware-failures"></a>Utilisation d’INDEX REBUILD pour récupérer suite à des défaillances matérielles

Dans les versions précédentes de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], vous aviez parfois la possibilité de régénérer un index non cluster de lignes afin de corriger les incohérences dues à des défaillances matérielles.
À compter de [!INCLUDE[ssKatmai](../../includes/ssKatmai-md.md)], vous pouvez toujours réparer de telles incohérences entre l’index et l’index cluster en régénérant un index non cluster hors connexion. Toutefois, vous ne pouvez pas réparer les incohérences d’un index non cluster en regénérant l’index en ligne. En effet, le mécanisme de regénération en ligne utilise l’index non cluster existant comme base pour la regénération et propage de ce fait l’incohérence. La reconstruction de l'index hors connexion peut parfois imposer une analyse de l'index cluster (ou segment de mémoire) et éliminer ainsi l'incohérence. Afin de garantir une reconstruction à partir de l’index cluster, supprimez puis recréez l’index non-cluster. Comme pour les versions précédentes, nous vous recommandons d'éliminer les incohérences en restaurant les données concernées à partir d'une sauvegarde. Toutefois, il est possible que vous puissiez réparer les incohérences d'un index en reconstruisant l'index non cluster hors connexion. Pour plus d’informations, consultez [DBCC CHECKIDENT &#40;Transact-SQL&#41;](../../t-sql/database-console-commands/dbcc-checkdb-transact-sql.md).

## <a name="see-also"></a>Voir aussi

- [Guide de conception et d’architecture d’index SQL Server](../../relational-databases/sql-server-index-design-guide.md)
- [Exécuter des opérations en ligne sur les index](../../relational-databases/indexes/perform-index-operations-online.md)
- [ALTER INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/alter-index-transact-sql.md)
- [Adaptive Index Defrag](https://github.com/Microsoft/tigertoolbox/tree/master/AdaptiveIndexDefrag)
- [CREATE STATISTICS &#40;Transact-SQL&#41;](../../t-sql/statements/create-statistics-transact-sql.md)
- [UPDATE STATISTICS &#40;Transact-SQL&#41;](../../t-sql/statements/update-statistics-transact-sql.md)
- [Performances des requêtes d’index columnstore](../../relational-databases/indexes/columnstore-indexes-query-performance.md)
- [Bien démarrer avec columnstore pour l’analytique opérationnelle en temps réel](../../relational-databases/indexes/get-started-with-columnstore-for-real-time-operational-analytics.md)
- [Index columnstore pour l’entreposage des données](../../relational-databases/indexes/columnstore-indexes-data-warehouse.md)
- [Index columnstore et la stratégie de fusion de rowgroups](/archive/blogs/sqlserverstorageengine/columnstore-index-merge-policy-for-reorganize)