---
description: Supprimer des colonnes d'une table
title: Supprimer des colonnes d’une table | Microsoft Docs
ms.custom: ''
ms.date: 04/11/2017
ms.prod: sql
ms.prod_service: table-view-index, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: table-view-index
ms.topic: conceptual
helpviewer_keywords:
- columns [SQL Server], deleting
- removing columns
- deleting columns
- dropping columns
ms.assetid: 0d8f6e4f-bc71-4fa3-8615-74249c8e072d
author: stevestein
ms.author: sstein
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 8fb537c503ec66c368c6c7e12e4740d9db1224aa
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99160278"
---
# <a name="delete-columns-from-a-table"></a>Supprimer des colonnes d'une table

[!INCLUDE [sqlserver2016-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi-asa-pdw.md)]

Cette rubrique explique comment supprimer des colonnes de table dans [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] à l'aide de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] ou [!INCLUDE[tsql](../../includes/tsql-md.md)].

> [!CAUTION]
> Quand vous supprimez une colonne dans une table, la colonne et toutes les données qu’elle contient sont supprimées.

 **Dans cette rubrique**

- **Avant de commencer :**

   [Limitations et restrictions](#Restrictions)

   [Sécurité](#Security)

- **Pour supprimer une colonne d'une table à l'aide de :**

   [SQL Server Management Studio](#SSMSProcedure)

   [Transact-SQL](#TsqlProcedure)

## <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Avant de commencer

### <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> Limitations et restrictions

Vous ne pouvez pas supprimer une colonne qui a une contrainte CHECK. Vous devez d'abord supprimer la contrainte.

Vous ne pouvez pas supprimer une colonne qui a des contraintes PRIMARY KEY ou FOREIGN KEY ou d'autres dépendances, sauf en utilisant le Concepteur de tables. Si vous utilisez l'Explorateur d'objets ou [!INCLUDE[tsql](../../includes/tsql-md.md)], vous devez d'abord supprimer toutes les dépendances à la colonne.

### <a name="security"></a><a name="Security"></a> Sécurité

#### <a name="permissions"></a><a name="Permissions"></a> Autorisations

Requiert une autorisation ALTER sur la table.

## <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Utilisation de SQL Server Management Studio

### <a name="to-delete-columns-by-using-object-explorer"></a>Pour supprimer des colonnes à l'aide de l'Explorateur d'objets

1. Dans l' **Explorateur d'objets**, connectez-vous à une instance du [!INCLUDE[ssDE](../../includes/ssde-md.md)].
2. Dans l’**Explorateur d’objets**, recherchez la table dans laquelle vous souhaitez supprimer des colonnes, puis développez-la pour exposer les noms des colonnes.
3. Cliquez avec le bouton droit sur la colonne à supprimer, puis choisissez **Supprimer**.
4. Dans la boîte de dialogue **Supprimer un objet** , cliquez sur **OK**.

Si la colonne contient des contraintes ou d'autres dépendances, un message d'erreur s'affichera dans la boîte de dialogue **Supprimer un objet** . Résolvez l'erreur en supprimant les contraintes référencées.

### <a name="to-delete-columns-by-using-table-designer"></a>Pour supprimer des colonnes à l'aide du Concepteur de tables

1. Dans l’ **Explorateur d’objets**, cliquez avec le bouton droit sur la table dans laquelle vous souhaitez supprimer des colonnes et choisissez **Conception**.
2. Cliquez avec le bouton droit sur la colonne à supprimer et, dans le menu contextuel, cliquez sur **Supprimer une colonne** .
3. Si les colonnes à supprimer participent à une relation (FOREIGN KEY ou PRIMARY KEY), un message vous demande confirmation avant la suppression des colonnes sélectionnées et de leurs relations. Cliquez sur **Oui**.

## <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Utilisation de Transact-SQL

### <a name="to-delete-columns"></a>Pour supprimer des colonnes

L'exemple suivant vous indique comment supprimer une colonne.

```sql
ALTER TABLE dbo.doc_exb DROP COLUMN column_b;
```

Si la colonne contient des contraintes ou d'autres dépendances, un message d'erreur est retourné. Résolvez l'erreur en supprimant les contraintes référencées.

Pour obtenir des exemples supplémentaires, consultez [ALTER TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-table-transact-sql.md).

## <a name="FollowUp"></a>
