---
description: Définir les options d'index
title: Définir les options d’index | Microsoft Docs
ms.custom: ''
ms.date: 06/26/2019
ms.prod: sql
ms.prod_service: table-view-index, sql-database
ms.reviewer: ''
ms.technology: table-view-index
ms.topic: conceptual
helpviewer_keywords:
- ALLOW_ROW_LOCKS option
- SORT_IN_TEMPDB option
- DROP_EXISTING clause
- large data, indexes
- PAD_INDEX
- STATISTICS_NORECOMPUTE
- MAXDOP index option, setting
- index options [SQL Server]
- MAXDOP index option
- IGNORE_DUP_KEY option
- ALLOW_PAGE_LOCKS option
- OPTIMIZE_FOR_SEQUENTIAL_KEY option
- ONLINE
ms.assetid: 7969af33-e94c-41f7-ab89-9d9a2747cd5c
author: MikeRayMSFT
ms.author: mikeray
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: ab6e8a4195ec78c3ae9593991d7c376464925cdb
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100353621"
---
# <a name="set-index-options"></a>Définir les options d'index

[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Cette rubrique explique comment modifier les propriétés d'un index dans [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] à l'aide de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] ou de [!INCLUDE[tsql](../../includes/tsql-md.md)].

 **Dans cet article**

- **Avant de commencer :**

   [Limitations et restrictions](#Restrictions)

   [Sécurité](#Security)

- **Pour modifier les propriétés d'un index, utilisez :**

   [SQL Server Management Studio](#SSMSProcedure)

   [Transact-SQL](#TsqlProcedure)

## <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Avant de commencer

### <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> Limitations et restrictions

- Les options suivantes sont immédiatement appliquées à l’index à l’aide de la clause SET de l’instruction ALTER INDEX : ALLOW_PAGE_LOCKS, ALLOW_ROW_LOCKS, OPTIMIZE_FOR_SEQUENTIAL_KEY, IGNORE_DUP_KEY, et STATISTICS_NORECOMPUTE.
- Les options suivantes peuvent être définies lorsque vous reconstruisez un index à l'aide de ALTER INDEX REBUILD ou de CREATE INDEX WITH DROP_EXISTING : PAD_INDEX, FILLFACTOR, SORT_IN_TEMPDB, IGNORE_DUP_KEY, STATISTICS_NORECOMPUTE, ONLINE, ALLOW_ROW_LOCKS, ALLOW_PAGE_LOCKS, MAXDOP, and DROP_EXISTING (CREATE INDEX uniquement).

### <a name="security"></a><a name="Security"></a> Sécurité

#### <a name="permissions"></a><a name="Permissions"></a> Autorisations

Nécessite une autorisation ALTER sur la table ou la vue.

## <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Utilisation de SQL Server Management Studio

### <a name="to-modify-the-properties-of-an-index-in-table-designer"></a>Pour modifier les propriétés d'un index dans le Concepteur de tables

1. Dans l’Explorateur d’objets, cliquez sur le signe plus (+) pour développer la base de données qui contient la table sur laquelle vous souhaitez modifier les propriétés d’un index.
2. Cliquez sur le signe plus (+) pour développer le dossier **Tables** .
3. Cliquez avec le bouton droit sur la table sur laquelle vous voulez modifier les propriétés d’un index et sélectionnez **Conception**.
4. Dans le menu **Concepteur de tables** , cliquez sur **Index/Clés**.
5. Sélectionnez l'index à modifier. Ses propriétés apparaîtront dans la grille principale.
6. Modifiez les paramètres de l'ensemble de propriétés pour personnaliser l'index.
7. Cliquez sur **Fermer**.
8. Dans le menu **Fichier** , sélectionnez **Enregistrer**_nom_table_.

### <a name="to-modify-the-properties-of-an-index-in-object-explorer"></a>Pour modifier les propriétés d'un index dans l'Explorateur d'objets

1. Dans l’Explorateur d’objets, cliquez sur le signe plus (+) pour développer la base de données qui contient la table sur laquelle vous souhaitez modifier les propriétés d’un index.
2. Cliquez sur le signe plus (+) pour développer le dossier **Tables** .
3. Cliquez sur le signe plus (+) pour développer la table sur laquelle vous souhaitez modifier les propriétés d’un index.
4. Cliquez sur le signe plus (+) pour développer le dossier **Index** .
5. Cliquez avec le bouton droit sur l’index dont vous voulez modifier les propriétés, puis sélectionnez **Propriétés**.
6. Sous **Sélectionner une page**, sélectionnez **Options**.
7. Modifiez les paramètres de l'ensemble de propriétés pour personnaliser l'index.
8. Pour ajouter, supprimer ou déplacer une colonne d’index, sélectionnez la page **Général** dans la boîte de dialogue **Propriétés de l’index -** _nom_index_. Pour plus d'informations, consultez [Index Properties F1 Help](../../relational-databases/indexes/index-properties-f1-help.md)

## <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Utilisation de Transact-SQL

### <a name="to-see-the-properties-of-all-the-indexes-in-a-table"></a>Pour afficher les propriétés de tous les index d'une table

L’exemple suivant affiche les propriétés de tous les index dans une table de la base de données AdventureWorks.

```sql
SELECT i.name AS index_name
   , i.type_desc
   , i.is_unique
   , ds.type_desc AS filegroup_or_partition_scheme
   , ds.name AS filegroup_or_partition_scheme_name
   , i.ignore_dup_key
   , i.is_primary_key
   , i.is_unique_constraint
   , i.fill_factor
   , i.is_padded
   , i.is_disabled
   , i.allow_row_locks
   , i.allow_page_locks
   , i.has_filter
   , i.filter_definition
FROM sys.indexes AS i
   INNER JOIN sys.data_spaces AS ds
      ON i.data_space_id = ds.data_space_id
   WHERE is_hypothetical = 0 AND i.index_id <> 0
       AND i.object_id = OBJECT_ID('HumanResources.Employee')
;
```

#### <a name="to-set-the-properties-of-an-index"></a>Pour définir les propriétés d'un index

Les exemples suivants définissent les propriétés des index dans la base de données AdventureWorks.

[!code-sql[IndexDDL#AlterIndex4](../../relational-databases/indexes/codesnippet/tsql/set-index-options_1.sql)]

[!code-sql[IndexDDL#AlterIndex2](../../relational-databases/indexes/codesnippet/tsql/set-index-options_2.sql)]

Pour plus d’informations, consultez [ALTER INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/alter-index-transact-sql.md).
