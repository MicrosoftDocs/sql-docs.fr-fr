---
description: Spécifier les colonnes calculées dans une table
title: Spécifier les colonnes calculées dans une table | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: table-view-index
ms.topic: conceptual
helpviewer_keywords:
- computed columns, define
ms.assetid: 731a4576-09c1-47f0-a8f6-edd0b55679f4
author: stevestein
ms.author: sstein
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: fb4b3f8a0efb8f62a94177c5a4546e3a30697ccb
ms.sourcegitcommit: 108bc8e576a116b261c1cc8e4f55d0e0713d402c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/25/2021
ms.locfileid: "98766064"
---
# <a name="specify-computed-columns-in-a-table"></a>Spécifier les colonnes calculées dans une table

[!INCLUDE[tsql-appliesto-ss-asdb-xxxx-xxx-md](../../includes/applies-to-version/sql-asdb.md)]

Une colonne calculée est une colonne virtuelle qui n'est pas stockée physiquement dans la table, à moins que la colonne ne soit indiquée comme PERSISTED. Une expression de colonne calculée peut utiliser des données d'autres colonnes afin de calculer une valeur pour la colonne à laquelle elle appartient. Vous pouvez spécifier une expression pour une colonne calculée dans [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] à l'aide de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] ou de [!INCLUDE[tsql](../../includes/tsql-md.md)].

**Dans cette rubrique**

- **Avant de commencer :**

   [Limitations et restrictions](#Limitations)

   [Sécurité](#Security)

- **Pour spécifier une colonne calculée à l'aide de :**

   [SQL Server Management Studio](#SSMSProcedure)

   [Transact-SQL](#TsqlProcedure)

## <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Avant de commencer

### <a name="limitations-and-restrictions"></a><a name="Limitations"></a> Limitations et restrictions

- Une colonne calculée ne peut pas être utilisée en tant que définition de contrainte DEFAULT ou FOREIGN KEY ou avec une définition de contrainte NOT NULL. Toutefois, si sa valeur est définie par une expression déterministe et que le type de données du résultat est autorisé dans les colonnes d'index, elle peut être utilisée en tant que colonne clé dans un index ou composante d'une contrainte PRIMARY KEY ou UNIQUE quelconque. Par exemple, si la table possède les colonnes d'entiers a et b, la colonne calculée a + b peut être indexée, contrairement à la colonne calculée a + DATEPART(dd, GETDATE()) dont la valeur est susceptible d'évoluer au fil des appels.
- Une colonne calculée ne peut pas être la cible d'une instruction INSERT ou UPDATE.

### <a name="security"></a><a name="Security"></a> Sécurité

#### <a name="permissions"></a><a name="Permissions"></a> Autorisations

Requiert une autorisation ALTER sur la table.

## <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Utilisation de SQL Server Management Studio

### <a name="to-add-a-new-computed-column"></a><a name="NewColumn"></a> Pour ajouter une nouvelle colonne calculée

1. Dans l' **Explorateur d'objets**, développez la table à laquelle vous voulez ajouter une nouvelle colonne calculée. Cliquez avec le bouton droit sur **Colonnes** et sélectionnez **Nouvelle colonne**.
2. Entrez le nom de la colonne et acceptez le type de données par défaut (**nchar**(10)). Le [!INCLUDE[ssDE](../../includes/ssde-md.md)] détermine le type de données de la colonne calculée en appliquant les règles de priorité des types de données aux expressions spécifiées dans la formule. Par exemple, si la formule fait référence à une colonne de type **money** et à une colonne de type **int**, la colonne calculée est de type **money** , car ce type de données a la priorité la plus élevée. Pour plus d’informations, consultez [Priorités des types de données &#40;Transact-SQL&#41;](../../t-sql/data-types/data-type-precedence-transact-sql.md).
3. Dans l'onglet **Propriétés des colonnes** , développez la propriété **Spécification de la colonne calculée** .
4. Dans la propriété enfant **(Formule)** , entrez l’expression pour cette colonne dans la cellule de grille située à droite. Par exemple, dans une colonne `SalesTotal` , la formule que vous écrivez peut être `SubTotal+TaxAmt+Freight`, qui ajoute la valeur dans ces colonnes pour chaque ligne de la table.

   > [!IMPORTANT]
   > Lorsqu'une formule combine deux expressions de type de données différents, les règles de priorité des types de données spécifient que le type ayant une priorité plus faible est converti dans un type ayant une priorité plus élevée. Si la conversion n'est pas prise en charge en tant que conversion implicite, l'erreur «`Error validating the formula for column column_name.`» est retournée. Utilisez la fonction CAST ou CONVERT pour résoudre le conflit de type de données. Par exemple, si une colonne de type **nvarchar** est associée à une colonne de type **int**, le type entier doit être converti en **nvarchar** comme indiqué dans cette formule `('Prod'+CONVERT(nvarchar(23),ProductID))`. Pour plus d’informations, consultez [CAST et CONVERT &#40;Transact-SQL&#41;](../../t-sql/functions/cast-and-convert-transact-sql.md).

5. Indiquez si les données doivent être enregistrées en choisissant **Oui** ou **Non** dans la liste déroulante de la propriété enfant **Is Persisted** .

6. Dans le menu **Fichier**, cliquez sur **Enregistrer** _nom de la table_.

#### <a name="to-add-a-computed-column-definition-to-an-existing-column"></a>Pour ajouter une définition de colonne calculée à une colonne existante

1. Dans l’ **Explorateur d’objets**, cliquez avec le bouton droit sur la table contenant la colonne à modifier et développez **Colonnes** .
2. Cliquez avec le bouton droit sur la colonne dans laquelle vous voulez spécifier une formule de colonne calculée et cliquez sur **Supprimer**. Cliquez sur **OK**.
3. Ajoutez une nouvelle colonne et spécifiez la formule de colonne calculée en suivant la procédure précédente pour ajouter une nouvelle colonne calculée.

## <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Utilisation de Transact-SQL

### <a name="to-add-a-computed-column-when-creating-a-table"></a>Pour ajouter une colonne calculée lors de la création d'une table

L'exemple suivant crée une table avec une colonne calculée qui multiplie la valeur de la colonne `QtyAvailable` par la valeur de la colonne `UnitPrice`.

```sql
CREATE TABLE dbo.Products
   (
      ProductID int IDENTITY (1,1) NOT NULL
      , QtyAvailable smallint
      , UnitPrice money
      , InventoryValue AS QtyAvailable * UnitPrice
    )
;
-- Insert values into the table.
INSERT INTO dbo.Products (QtyAvailable, UnitPrice)
   VALUES (25, 2.00), (10, 1.5)
;
-- Display the rows in the table.
SELECT ProductID, QtyAvailable, UnitPrice, InventoryValue
FROM dbo.Products
;
```

### <a name="to-add-a-new-computed-column-to-an-existing-table"></a>Pour ajouter une nouvelle colonne calculée dans une table existante

L'exemple suivant ajoute une nouvelle colonne à la table créée dans l'exemple précédent.

```sql
ALTER TABLE dbo.Products ADD RetailValue AS (QtyAvailable * UnitPrice * 1.5)
;
```

Ajoutez éventuellement l’argument PERSISTED pour stocker physiquement les valeurs calculées dans la table :

```sql
ALTER TABLE dbo.Products ADD RetailValue AS (QtyAvailable * UnitPrice * 1.5) PERSISTED
;
```

### <a name="to-change-an-existing-column-to-a-computed-column"></a>Pour modifier une colonne existante en une colonne calculée

L'exemple suivant modifie la colonne ajoutée dans l'exemple précédent.

```sql
ALTER TABLE dbo.Products DROP COLUMN RetailValue
;
GO
ALTER TABLE dbo.Products ADD RetailValue AS (QtyAvailable * UnitPrice * 1.5)
;
```

Pour plus d’informations, consultez [ALTER TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-table-transact-sql.md).
