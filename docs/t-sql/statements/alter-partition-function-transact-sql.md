---
description: ALTER PARTITION FUNCTION (Transact-SQL)
title: ALTER PARTITION FUNCTION (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- ALTER PARTITION FUNCTION
- ALTER_PARTITION_FUNCTION_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- splitting partitions [SQL Server]
- partitioned tables [SQL Server], splitting
- ALTER PARTITION FUNCTION statement
- merging partitions [SQL Server]
- partitioned indexes [SQL Server], merging
- partitioned indexes [SQL Server], splitting
- modifying partition functions
- partition functions [SQL Server], modifying
- partitioned tables [SQL Server], merging
ms.assetid: 70866dac-0a8f-4235-8108-51547949ada4
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: c3ae0c860bc31a228ce3a842d7e1ba8a861ea1e5
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/11/2021
ms.locfileid: "98092106"
---
# <a name="alter-partition-function-transact-sql"></a>ALTER PARTITION FUNCTION (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Modifie une fonction de partition en divisant ou en fusionnant ses valeurs limites. Exécuter une instruction ALTER PARTITION FUNCTION permet de fractionner en deux partitions un index ou une partition de table qui utilise la fonction de partition. L’instruction peut également fusionner deux partitions en une seule partition.  
  
> [!CAUTION]  
>  Une même fonction de partition peut être utilisée par plusieurs tables ou index. Ils sont alors tous affectés par ALTER PARTITION FUNCTION en une seule transaction.  
  
![Icône du lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql
  
ALTER PARTITION FUNCTION partition_function_name()  
{   
    SPLIT RANGE ( boundary_value )  
  | MERGE RANGE ( boundary_value )   
} [ ; ]  
```  
  

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments
*partition_function_name*  
Nom de la fonction de partition à modifier.  
  
SPLIT RANGE ( *boundary_value* )  
Ajoute une partition à la fonction de partition. *boundary_value* détermine la plage de la nouvelle partition. Elle doit être différente des plages limites existantes de la fonction de partition. En fonction de la valeur de *boundary_value*, le [!INCLUDE[ssDE](../../includes/ssde-md.md)] divise l’une des plages existantes en deux. Parmi ces deux plages, celle avec la nouvelle valeur *boundary_value* est la nouvelle partition.  
  
Un groupe de fichiers doit exister en ligne. Et le schéma de partition qui utilise la fonction de partition comme NEXT HOLD pour conserver la nouvelle partition doit marquer le groupe de fichiers. Les groupes de fichiers sont alloués aux partitions dans une instruction CREATE PARTITION SCHEME. L’instruction CREATE PARTITION FUNCTION crée moins de partitions que de groupes de fichiers pour les contenir. Une instruction CREATE PARTITION SCHEME peut mettre de côté davantage de groupes de fichiers que nécessaire. Si cela se produit, vous obtiendrez des groupes de fichiers non attribués. En outre, le schéma de partition marque l’un des groupes de fichiers comme NEXT USED. Ce groupe de fichiers accueille la nouvelle partition. S’il n’y a aucun groupe de fichiers que le schéma de partition marque comme NEXT USED, vous devez utiliser une instruction ALTER PARTITION SCHEME. 

L’instruction ALTER PARTITION SCHEME peut ajouter un groupe de fichiers, ou en sélectionner un existant, pour contenir la nouvelle partition. Vous pouvez attribuer un groupe de fichiers contenant déjà des partitions pour accueillir des partitions supplémentaires. Une fonction de partition peut faire partie de plusieurs schémas de partition. Ainsi, tous les schémas de partition qui utilisent la fonction de partition à laquelle vous ajoutez des partitions doivent avoir un groupe de fichiers NEXT USED. À défaut, l’instruction ALTER PARTITION FUNCTION échoue et génère une erreur indiquant le ou les schémas de partition qui sont dépourvus d'un groupe de fichiers NEXT USED.  
  
Si vous créez toutes les partitions dans le même groupe de fichiers, celui-ci est initialement et automatiquement affecté comme le groupe de fichiers NEXT USED. Toutefois, après une opération de fractionnement, il n'y a plus de groupe de fichiers NEXT USED sélectionné. Vous devez affecter explicitement le groupe de fichiers comme groupe de fichiers NEXT USED à l’aide de l’instruction ALTER PARTITION SCHEME, sinon toute division ultérieure échouera.  
  
> [!NOTE]  
>  Limitations d’index columnstore : seules les partitions vides peuvent être fractionnées quand il existe un index columnstore sur la table. Vous devez supprimer ou désactiver l’index columnstore avant d’effectuer cette opération.  
  
MERGE [ RANGE ( *boundary_value*) ]  
Supprime une partition et fusionne les valeurs qui existent dans la partition dans une partition restante. La plage (*boundary_value*) doit être une valeur limite existante de la partition à supprimer. Cet argument supprime le groupe de fichiers qui contenait initialement *boundary_value* du schéma de partition, sauf s’il est utilisé par une partition restante, ou le marque avec la propriété NEXT USED. La partition fusionnée existe dans le groupe de fichiers qui ne contenait pas initialement *boundary_value*. *boundary_value* est une expression constante qui peut référencer des variables (notamment des variables de type défini par l’utilisateur) ou des fonctions (notamment des fonctions définies par l’utilisateur). Elle ne peut pas faire référence à une expression [!INCLUDE[tsql](../../includes/tsql-md.md)]. *boundary_value* doit correspondre au type de données de sa colonne de partitionnement correspondante ou être convertible de façon implicite en celui-ci. Vous ne pouvez pas non plus tronquer *boundary_value* pendant une conversion implicite d’une manière à ce que la taille et la mise à l’échelle de la valeur ne correspondent pas à celles de son paramètre *input_parameter_type* correspondant.  
  
> [!NOTE]  
>  Limitations de l’index columnstore : Vous ne pouvez pas fusionner deux partitions non vides contenant un index columnstore. Vous devez supprimer ou désactiver l’index columnstore avant d’effectuer cette opération.  
  
## <a name="best-practices"></a>Bonnes pratiques  
Conservez toujours les partitions vides aux deux extrémités de la plage de partitions. Conservez les partitions aux deux extrémités afin de garantir que le fractionnement et la fusion de la partition n’entraînent aucun déplacement de données. Le fractionnement de la partition se produit au début et la fusion de partition se produit à la fin. Évitez de fractionner ou fusionner des partitions remplies. Le fractionnement ou la fusion des partitions remplies peut être inefficace. En effet, le fractionnement ou la fusion peut entraîner quatre fois plus de générations de journaux et provoquer également un verrouillage grave.  
  
## <a name="limitations-and-restrictions"></a>Limitations et restrictions  
ALTER PARTITION FUNCTION repartitionne les tables et les index qui utilisent la fonction en une opération atomique unique. Toutefois, cette opération se produit hors ligne et, selon l'étendue du repartitionnement, elle peut s'avérer gourmande en ressources.  
  
L'utilisation de ALTER PARTITION FUNCTION doit uniquement viser à diviser une partition en deux, ou à fusionner deux partitions dans une seule. Pour modifier la façon dont une table est partitionnée (par exemple, de 10 partitions à 5), vous pouvez utiliser l'une des options indiquées ci-dessous. La consommation en ressources de ces options peut varier en fonction de la configuration de votre système :  
  
-   Créez une nouvelle table partitionnée avec la fonction de partition nécessaire. Puis, insérez les données à partir de l’ancienne table dans la nouvelle table en utilisant une instruction INSERT INTO...SELECT FROM.  
  
-   Créez un index cluster partitionné sur un segment.  
  
    > [!NOTE]  
    >  La suppression d'un index cluster partitionné engendre un segment partitionné.  
  
-   Supprimez et reconstruisez un index partitionné existant à l'aide de l'instruction [!INCLUDE[tsql](../../includes/tsql-md.md)] CREATE INDEX avec la clause DROP EXISTING = ON.  
  
-   Exécutez une séquence d'instructions ALTER PARTITION FUNCTION.  
  
Tous les groupes de fichiers concernés par l’instruction ALTER PARTITION FUNCTION doivent être en ligne.  
  
ALTER PARTITION FUNCTION échoue s'il existe un index cluster désactivé dans une table qui utilise la fonction de partition.  
  
[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] n'assure pas la prise en charge de la réplication lors de la modification d'une fonction de partition. Les modifications apportées à une fonction de partition dans la base de données de publication doivent être appliquées manuellement dans la base de données d'abonnement.  
  
## <a name="permissions"></a>Autorisations  
L'instruction ALTER PARTITION FUNCTION peut être exécutée avec les autorisations suivantes :  
  
-   Autorisation ALTER ANY DATASPACE. Cette autorisation est attribuée par défaut aux membres du rôle de serveur fixe **sysadmin** et des rôles de base de données fixes **db_owner** et **db_ddladmin** .  
  
-   Autorisation CONTROL ou ALTER sur la base de données dans laquelle la fonction de partition a été créée.  
  
-   Autorisation CONTROL SERVER ou ALTER ANY DATABASE sur le serveur de la base de données dans laquelle la fonction de partition a été créée.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-splitting-a-partition-of-a-partitioned-table-or-index-into-two-partitions"></a>R. Fractionnement d'une partition de table ou d'index partitionné en deux partitions  
L'exemple suivant crée une fonction de partition pour partitionner une table ou un index en quatre partitions. `ALTER PARTITION FUNCTION` fractionne l'une des partitions en deux pour créer au total cinq partitions.  
  
```sql  
IF EXISTS (SELECT * FROM sys.partition_functions  
    WHERE name = 'myRangePF1')  
DROP PARTITION FUNCTION myRangePF1;  
GO  
CREATE PARTITION FUNCTION myRangePF1 (int)  
AS RANGE LEFT FOR VALUES ( 1, 100, 1000 );  
GO  
--Split the partition between boundary_values 100 and 1000  
--to create two partitions between boundary_values 100 and 500  
--and between boundary_values 500 and 1000.  
ALTER PARTITION FUNCTION myRangePF1 ()  
SPLIT RANGE (500);  
```  
  
### <a name="b-merging-two-partitions-of-a-partitioned-table-into-one-partition"></a>B. Fusion de deux partitions d'une table partitionnée en une seule partition  
L'exemple suivant crée une fonction de partition (identique à celle de l'exemple précédent), puis fusionne deux des partitions en une seule partition, pour un total de trois partitions.  
  
```sql  
IF EXISTS (SELECT * FROM sys.partition_functions  
    WHERE name = 'myRangePF1')  
DROP PARTITION FUNCTION myRangePF1;  
GO  
CREATE PARTITION FUNCTION myRangePF1 (int)  
AS RANGE LEFT FOR VALUES ( 1, 100, 1000 );  
GO  
--Merge the partitions between boundary_values 1 and 100  
--and between boundary_values 100 and 1000 to create one partition  
--between boundary_values 1 and 1000.  
ALTER PARTITION FUNCTION myRangePF1 ()  
MERGE RANGE (100);  
```  
  
## <a name="see-also"></a>Voir aussi  
[Tables et index partitionnés](../../relational-databases/partitions/partitioned-tables-and-indexes.md)   
[CREATE PARTITION FUNCTION &#40;Transact-SQL&#41;](../../t-sql/statements/create-partition-function-transact-sql.md)   
[DROP PARTITION FUNCTION &#40;Transact-SQL&#41;](../../t-sql/statements/drop-partition-function-transact-sql.md)   
[CREATE PARTITION SCHEME &#40;Transact-SQL&#41;](../../t-sql/statements/create-partition-scheme-transact-sql.md)   
[ALTER PARTITION SCHEME &#40;Transact-SQL&#41;](../../t-sql/statements/alter-partition-scheme-transact-sql.md)   
[DROP PARTITION SCHEME &#40;Transact-SQL&#41;](../../t-sql/statements/drop-partition-scheme-transact-sql.md)   
[CREATE INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/create-index-transact-sql.md)   
[ALTER INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/alter-index-transact-sql.md)   
[CREATE TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/create-table-transact-sql.md)   
[sys.partition_functions &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-partition-functions-transact-sql.md)   
[sys.partition_parameters &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-partition-parameters-transact-sql.md) [sys.partition_range_values &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-partition-range-values-transact-sql.md)   
[sys.partitions &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-partitions-transact-sql.md)   
[sys.tables &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-tables-transact-sql.md)   
[sys.indexes &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-indexes-transact-sql.md)   
[sys.index_columns &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-index-columns-transact-sql.md)  
  
  
