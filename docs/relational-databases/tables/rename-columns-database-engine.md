---
description: Renommer des colonnes (moteur de base de données)
title: Renommer des colonnes (moteur de base de données) | Microsoft Docs
ms.custom: ''
ms.date: 08/03/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: table-view-index
ms.topic: conceptual
helpviewer_keywords:
- columns [SQL Server], names
- renaming columns
- column names [SQL Server]
ms.assetid: 7c71ec9f-0180-4398-b32a-4bfb7592e75d
author: stevestein
ms.author: sstein
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: ef2568fbb27a2139896cb18fd99fb9e65c403d8f
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99212028"
---
# <a name="rename-columns-database-engine"></a>Renommer des colonnes (moteur de base de données)


[!INCLUDE [sqlserver2016-asdb-asdbmi](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi.md)]

Vous pouvez renommer une colonne de table dans [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] à l'aide de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] ou de [!INCLUDE[tsql](../../includes/tsql-md.md)].

**Dans cette rubrique**

- **Avant de commencer :**

   [Limitations et restrictions](#Restrictions)

   [Sécurité](#Security)

- **Pour renommer des colonnes, utilisez :**

   [SQL Server Management Studio](#SSMSProcedure)

   [Transact-SQL](#TsqlProcedure)

## <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Avant de commencer

### <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> Limitations et restrictions

Si vous renommez une colonne, les références à cette colonne ne sont pas automatiquement renommées. Vous devez modifier manuellement tout objet qui référence la colonne renommée. Par exemple, si vous renommez une colonne de table et si cette colonne est référencée dans un déclencheur, vous devez modifier le déclencheur pour refléter le nouveau nom de colonne. Utilisez [sys.sql_expression_dependencies](../../relational-databases/system-catalog-views/sys-sql-expression-dependencies-transact-sql.md) pour obtenir la liste des dépendances de l’objet avant de le renommer.

### <a name="security"></a><a name="Security"></a> Sécurité

#### <a name="permissions"></a><a name="Permissions"></a> Autorisations

Requiert une autorisation ALTER sur l'objet.

## <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Utilisation de SQL Server Management Studio

### <a name="to-rename-a-column-using-object-explorer"></a>Pour renommer une colonne à l'aide de l'Explorateur d'objets

1. Dans l' **Explorateur d'objets**, connectez-vous à une instance du [!INCLUDE[ssDE](../../includes/ssde-md.md)].
2. Dans **l’Explorateur d’objets**, cliquez avec le bouton droit sur la table dans laquelle vous souhaitez renommer des colonnes et choisissez **Renommer**.
3. Tapez une nouvelle colonne.

### <a name="to-rename-a-column-using-table-designer"></a>Pour renommer une colonne à l'aide du Concepteur de tables

1. Dans **l’Explorateur d’objets**, cliquez avec le bouton droit sur la table dans laquelle vous souhaitez renommer des colonnes et choisissez **Conception**.
2. Sous **Nom de la colonne**, sélectionnez le nom que vous souhaitez modifier et tapez-en un nouveau.
3. Dans le menu **Fichier**, cliquez sur **Enregistrer** _nom de la table_.

> [!NOTE]
> Vous pouvez également modifier le nom d'une colonne sous l'onglet **Propriétés des colonnes** . Sélectionnez la colonne dont vous souhaitez modifier le nom et tapez une nouvelle valeur pour **Nom**.

## <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Utilisation de Transact-SQL

**Pour renommer une colonne**

### <a name="to-rename-a-column"></a>Pour renommer une colonne

L'exemple suivant renomme la colonne `TerritoryID` dans la table `Sales.SalesTerritory` en `TerrID` dans la base de données AdventureWorks.

```sql
EXEC sp_rename 'Sales.SalesTerritory.TerritoryID', 'TerrID', 'COLUMN';
```

Pour plus d’informations, consultez [sp_rename &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-rename-transact-sql.md).
