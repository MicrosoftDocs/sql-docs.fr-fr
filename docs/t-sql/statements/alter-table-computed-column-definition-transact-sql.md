---
description: ALTER TABLE computed_column_definition (Transact-SQL)
title: computed_column_definition (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 05/05/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
dev_langs:
- TSQL
helpviewer_keywords:
- ALTER TABLE statement
ms.assetid: 746eabda-3b4f-4940-b0b5-1c379f5cf7a5
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 1ba855c43f68cc06cf8c9b1d5fc89168713cfaa2
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/11/2021
ms.locfileid: "98100221"
---
# <a name="alter-table-computed_column_definition-transact-sql"></a>ALTER TABLE computed_column_definition (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Spécifie les propriétés d’une colonne calculée qui est ajoutée à une table à l’aide d’[ALTER TABLE](../../t-sql/statements/alter-table-transact-sql.md).  
  
 ![Icône du lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql
column_name AS computed_column_expression  
[ PERSISTED [ NOT NULL ] ]  
[   
    [ CONSTRAINT constraint_name ]  
    { PRIMARY KEY | UNIQUE }  
        [ CLUSTERED | NONCLUSTERED ]  
        [ WITH FILLFACTOR = fillfactor ]  
        [ WITH ( <index_option> [, ...n ] ) ]  
        [ ON { partition_scheme_name ( partition_column_name ) | filegroup   
            | "default" } ]  
    | [ FOREIGN KEY ]   
        REFERENCES ref_table [ ( ref_column ) ]   
        [ ON DELETE { NO ACTION | CASCADE } ]   
        [ ON UPDATE { NO ACTION } ]   
        [ NOT FOR REPLICATION ]   
    | CHECK [ NOT FOR REPLICATION ] ( logical_expression )  
]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments
*column_name*  
 Nom de la colonne à ajouter, modifier ou supprimer. *column_name* peut comporter entre 1 et 128 caractères. Pour les nouvelles colonnes, *column_name* peut être omis pour les colonnes créées avec un type de données **timestamp**. Si aucun *column_name* n’est spécifié pour une colonne de type de données **timestamp**, le **timestamp** du nom est utilisé.  
  
*computed_column_expression*  
 Expression définissant la valeur d'une colonne calculée. Une colonne calculée est une colonne virtuelle qui n'est pas physiquement stockée dans la table, mais traitée à partir d'une expression utilisant d'autres colonnes figurant dans la même table. Une expression doit générer une valeur. Par exemple, une colonne calculée peut avoir la définition suivante : cost AS price * qty. Autre exemple avec des opérateurs au niveau du bit : is_finalised AS is_checked | is_approved. L’expression peut être un nom de colonne non calculée, une constante, une fonction, une variable et toute combinaison de ces éléments reliés par un ou plusieurs opérateurs. L'expression ne peut pas être une condition de recherche, une sous-requête ou comporter un type de données d'alias.  
  
 Les colonnes calculées peuvent être utilisées dans les listes de sélections, dans les clauses WHERE ou ORDER BY ou chaque fois qu'une expression régulière peut être utilisée, à l'exception des cas suivants :  
  
-   Une colonne calculée ne peut pas être utilisée en tant que définition de contrainte DEFAULT ou FOREIGN KEY ou avec une définition de contrainte NOT NULL. Toutefois, si sa valeur est définie par une expression déterministe et que le type de données du résultat est autorisé dans les colonnes d'index, elle peut être utilisée en tant que colonne clé dans un index ou composante d'une contrainte PRIMARY KEY ou UNIQUE quelconque.  
  
     Par exemple, si la table possède les colonnes d'entiers a et b, la colonne calculée a + b peut être indexée, contrairement à la colonne calculée a + DATEPART(dd, GETDATE()) dont la valeur est susceptible d'évoluer au fil des appels.  
  
-   Une colonne calculée ne peut pas être la cible d'une instruction INSERT ou UPDATE.  
  
    > [!NOTE]  
    >  Chaque ligne dans une table pouvant avoir des valeurs différentes pour les colonnes impliquées dans la colonne calculée, il est possible que la colonne calculée n'ait pas le même résultat pour chaque ligne.  
  
PERSISTED  
 Spécifie que le [!INCLUDE[ssDE](../../includes/ssde-md.md)] stockera physiquement les valeurs calculées dans la table et qu'il les mettra à jour lorsque les autres colonnes dont dépend la colonne calculée seront actualisées. Le marquage d'une colonne calculée comme PERSISTED permet de créer un index sur une colonne calculée déterministe, mais pas précise. Pour plus d'informations, consultez [Indexes on Computed Columns](../../relational-databases/indexes/indexes-on-computed-columns.md). Les colonnes calculées utilisées comme colonnes de partitionnement d’une table partitionnée doivent être explicitement marquées comme PERSISTED. *computed_column_expression* doit être déterministe quand PERSISTED est spécifié. 

NULL | NOT NULL  
 Indique si les valeurs NULL sont autorisées dans la colonne. NULL n'est pas strictement une contrainte, mais peut être spécifié comme NOT NULL. Il est possible de spécifier NOT NULL pour des colonnes calculées seulement si PERSISTED est également spécifié.  
  
CONSTRAINT  
 Spécifie le début de la définition d'une contrainte PRIMARY KEY ou UNIQUE.  
  
*constraint_name*  
 Nouvelle contrainte. Les noms de contrainte doivent suivre les règles des [identificateurs](../../relational-databases/databases/database-identifiers.md), sauf que le nom ne peut pas commencer par un signe dièse (#). Si *constraint_name* n’est pas spécifié, un nom généré par le système est affecté à la contrainte.  
  
PRIMARY KEY  
 Contrainte garantissant l’intégrité de l’entité d’une ou de plusieurs colonnes spécifiées au moyen d’un index unique. Une seule contrainte PRIMARY KEY peut être créée par table.  
  
UNIQUE  
 Contrainte assurant l'intégrité de l'entité d'une colonne ou de plusieurs colonnes spécifiques à l'aide d'un seul index.  
  
CLUSTERED et NONCLUSTERED  
 Spécifie qu'un index, cluster ou non-cluster, est créé pour la contrainte PRIMARY KEY ou UNIQUE. La valeur par défaut des contraintes PRIMARY KEY est CLUSTERED. La valeur par défaut des contraintes UNIQUE est, elle, NONCLUSTERED.  
  
 Si une contrainte ou un index cluster existe déjà pour la table, vous ne pouvez pas spécifier CLUSTERED. Dans ce cas, l'option par défaut d'une contrainte PRIMARY KEY devient NONCLUSTERED.  
  
WITH FILLFACTOR =*fillfactor*  
 Spécifie le remplissage par le [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] des pages d'index utilisées pour stocker les données d'index. Les valeurs *fillfactor* spécifiées par l’utilisateur doivent être comprises entre 1 et 100. Si aucune valeur n'est spécifiée, la valeur par défaut est 0.  
  
> [!IMPORTANT]  
>  Dans la documentation, l’indication que WITH FILLFACTOR = *fillfactor* constitue l’unique option d’indexation s’appliquant aux contraintes PRIMARY KEY ou UNIQUE est maintenue dans un but de compatibilité ascendante, mais ne sera plus indiquée ainsi dans les versions à venir. D’autres options d’index peuvent être précisées dans la clause [index_option &#40;Transact-SQL&#41;](../../t-sql/statements/alter-table-index-option-transact-sql.md) d’ALTER TABLE.  
  
FOREIGN KEY REFERENCES  
 Contrainte qui assure l'intégrité référentielle des données des colonnes. Avec les contraintes FOREIGN KEY, il faut que chaque valeur de la colonne existe dans la ou les colonnes référencées correspondantes de la table référencée. Les contraintes FOREIGN KEY ne peuvent référencer que des colonnes qui sont des contraintes PRIMARY KEY ou UNIQUE dans la table référencée ou des colonnes référencées dans un UNIQUE INDEX sur la table référencée. Les clés étrangères des colonnes calculées doivent également être marquées comme PERSISTED.  
  
*ref_table*  
 Nom de la table référencée par la contrainte FOREIGN KEY.  
  
( *ref_column* )  
 Colonne de la table référencée par la contrainte FOREIGN KEY.  
  
ON DELETE { **NO ACTION** | CASCADE }  
 Spécifie l'action qui se produit dans les lignes de la table, si ces lignes comportent une relation référentielle et que la ligne référencée est supprimée de la table parente. La valeur par défaut est NO ACTION.  
  
NO ACTION  
 Le [!INCLUDE[ssDE](../../includes/ssde-md.md)] déclenche une erreur et la suppression de la ligne dans la table parent est annulée.

CASCADE  
 Les lignes correspondantes sont supprimées de la table de référence pour celles supprimées de la table parent.  
  
 Par exemple, dans la base de données [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)], la table ProductVendor possède une relation référentielle avec la table Vendor. La clé étrangère ProductVendor.BusinessEntityID fait référence à la clé primaire Vendor.BusinessEntityID.  
  
 Si une instruction DELETE est exécutée sur une ligne de la table Vendor et qu’une action ON DELETE CASCADE est spécifiée pour ProductVendor.BusinessEntityID, le [!INCLUDE[ssDE](../../includes/ssde-md.md)] vérifie la présence d’une ou de plusieurs lignes dépendantes dans la table ProductVendor. S'il existe des lignes dépendantes dans la table ProductVendor, celles-ci sont supprimées, outre la ligne référencée dans la table Vendor.  
  
 En revanche, si la valeur NO ACTION est spécifiée, le [!INCLUDE[ssDE](../../includes/ssde-md.md)] génère une erreur et restaure l’action de suppression sur la ligne de la table Vendor quand au moins une ligne de la table ProductVendor y fait référence.  
  
 Ne spécifiez pas CASCADE si la table est incluse dans une publication de fusion qui utilise des enregistrements logiques. Pour plus d’informations sur les enregistrements logiques, consultez [Regrouper les modifications apportées à des lignes connexes à l’aide d’enregistrements logiques](../../relational-databases/replication/merge/group-changes-to-related-rows-with-logical-records.md).  
  
ON UPDATE { **NO ACTION** }  
 Spécifie l'action qui se produit dans les lignes de la table créée lorsque ces lignes comportent une relation référentielle et que la ligne référencée est mise à jour dans la table parente. Quand la valeur NO ACTION est spécifiée, le [!INCLUDE[ssDE](../../includes/ssde-md.md)] génère une erreur et restaure l’action de mise à jour sur la ligne de la table Vendor si au moins une ligne de la table ProductVendor y fait référence.  
  
NOT FOR REPLICATION  
 **S’applique à** : [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] et versions ultérieures.  
  
 Peut être indiqué aussi bien pour les contraintes FOREIGN KEY que CHECK. Si cette clause est spécifiée pour une contrainte, la contrainte n'est pas appliquée lorsque les agents de réplication effectuent des opérations d'insertion, de mise à jour ou de suppression.  
  
CHECK  
 Contrainte qui assure l'intégrité du domaine en limitant les valeurs possibles pouvant être entrées dans une ou plusieurs colonnes. Les contraintes CHECK des colonnes calculées doivent également être marquées comme PERSISTED.  
  
*logical_expression*  
 Expression logique qui retourne TRUE ou FALSE. L'expression ne peut pas contenir une référence à un type de données d'alias.  
  
ON { *partition_scheme_name*(*partition_column_name*) | *filegroup*| "default"}  
 **S’applique à** : [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] et versions ultérieures.  
  
 Spécifie le lieu de stockage de l'index créé pour la contrainte. Si *partition_scheme_name* est spécifié, l’index est partitionné et les partitions qui en découlent sont mappées aux groupes de fichiers indiqués par *partition_scheme_name*. Si *filegroup* est spécifié, l’index est créé dans le groupe de fichiers nommé. Si "default" est indiqué ou si l'option ON n'est pas du tout spécifiée, l'index est créé dans le même groupe de fichiers que celui de la table. Si l'option ON est spécifiée lors de l'ajout d'un index cluster pour une contrainte PRIMARY KEY ou UNIQUE, la table dans son intégralité est déplacée dans le groupe de fichiers spécifié lorsque l'index cluster est créé.  
  
> [!NOTE]  
>  L'élément « default » n'est pas un mot clé dans ce contexte. Il s'agit de l'identificateur du groupe de fichiers par défaut et il doit être délimité, comme dans ON "default" ou ON [default]. Si "default" est spécifié, l'option QUOTED_IDENTIFIER doit être activée (ON) pour la session active. Il s'agit du paramètre par défaut. Pour plus d’informations, consultez [SET QUOTED_IDENTIFIER &#40;Transact-SQL&#41;](../../t-sql/statements/set-quoted-identifier-transact-sql.md).  
  
## <a name="remarks"></a>Notes  
 Chaque contrainte PRIMARY KEY et UNIQUE génère un index. Quel que soit le nombre de contraintes UNIQUE et PRIMARY KEY, le nombre d'index sur la table ne peut en aucun cas dépasser 999 index cluster et 1 index cluster.  
  
## <a name="see-also"></a>Voir aussi  
 [ALTER TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-table-transact-sql.md)  
