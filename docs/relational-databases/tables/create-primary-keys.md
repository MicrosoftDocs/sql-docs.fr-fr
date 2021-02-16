---
description: Créer des clés primaires
title: Créer des clés primaires | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: table-view-index
ms.topic: conceptual
helpviewer_keywords:
- primary keys [SQL Server], creating
ms.assetid: 85c623ca-4656-4d70-a9db-ee4d897cd214
author: stevestein
ms.author: sstein
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: debd09257826c625e6eee4d71a81cdb455683f3e
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100344268"
---
# <a name="create-primary-keys"></a>Créer des clés primaires


[!INCLUDE [sqlserver2016-asdb-asdbmi](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi.md)]

Vous pouvez définir une clé primaire dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] à l'aide de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] ou de [!INCLUDE[tsql](../../includes/tsql-md.md)]. La création d’une clé primaire crée automatiquement un index cluster unique correspondant ou, si vous avez spécifié cette option, un index non-cluster.

## <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Avant de commencer

### <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> Limitations et restrictions

- Une table ne peut contenir qu'une seule contrainte PRIMARY KEY.

- Toutes les colonnes définies dans une contrainte PRIMARY KEY doivent avoir la valeur NOT NULL. Si vous ne spécifiez pas la possibilité ou non de valeurs NULL, toutes les colonnes participant à une contrainte PRIMARY KEY sont définies à NOT NULL.

### <a name="security"></a><a name="Security"></a> Sécurité

#### <a name="permissions"></a><a name="Permissions"></a> Autorisations

La création d'une nouvelle table avec une clé primaire nécessite une autorisation CREATE TABLE dans la base de données et une autorisation ALTER pour le schéma dans lequel la table a été créée.

La création d'une clé primaire dans une table existante nécessite l'autorisation ALTER sur la table.

## <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Utilisation de SQL Server Management Studio

### <a name="to-create-a-primary-key"></a>Pour créer une clé primaire

1. Dans l’Explorateur d’objets, cliquez avec le bouton droit sur la table à laquelle vous souhaitez ajouter une contrainte unique et cliquez sur **Conception**.
2. Dans le **Concepteur de tables**, cliquez sur le sélecteur de ligne correspondant à la colonne de base de données que vous voulez définir comme clé primaire. Si vous voulez sélectionner plusieurs colonnes, appuyez sur la touche CTRL et, tout en la maintenant enfoncée, cliquez sur les sélecteurs de ligne des autres colonnes.
3. Cliquez avec le bouton droit sur le sélecteur de ligne de la colonne et cliquez sur **Définir la clé primaire**.

> [!CAUTION]
> Si vous voulez redéfinir la clé primaire, vous devez supprimer toutes les relations avec la clé primaire existante avant de pouvoir créer la nouvelle clé primaire. Un message vous avertira que les relations existantes seront automatiquement supprimées dans le cadre de ce processus.

Une colonne clé primaire est identifiée par un symbole de clé primaire dans son sélecteur de ligne.

Si une clé primaire comporte plusieurs colonnes, les doublons sont autorisés dans une colonne, mais chaque combinaison de valeurs provenant de toutes les colonnes de la clé primaire doit être unique.

Si vous définissez une clé composée, l'ordre des colonnes dans la clé primaire correspond à l'ordre des colonnes de la table. Vous pouvez cependant modifier l'ordre des colonnes après la création de la clé primaire. Pour plus d’informations, consultez [Modifier des clés primaires](../../relational-databases/tables/modify-primary-keys.md).

## <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Utilisation de Transact-SQL

### <a name="to-create-a-primary-key-in-an-existing-table"></a>Pour créer une clé primaire dans une table existante

L'exemple suivant crée une clé principale sur la colonne `TransactionID` dans la base de données AdventureWorks.

```sql
ALTER TABLE Production.TransactionHistoryArchive
   ADD CONSTRAINT PK_TransactionHistoryArchive_TransactionID PRIMARY KEY CLUSTERED (TransactionID);
```

### <a name="to-create-a-primary-key-in-a-new-table"></a>Pour créer une clé primaire dans une nouvelle table

L'exemple suivant crée une table et définit une clé principale sur la colonne `TransactionID` dans la base de données AdventureWorks.

```sql
CREATE TABLE Production.TransactionHistoryArchive1
   (
      TransactionID int IDENTITY (1,1) NOT NULL
      , CONSTRAINT PK_TransactionHistoryArchive_TransactionID PRIMARY KEY CLUSTERED (TransactionID)
   )
;
```

### <a name="to-create-a-primary-key-with-clustered-index-in-a-new-table"></a>Pour créer une clé primaire avec un index cluster dans une nouvelle table

L'exemple suivant crée une table et définit une clé principale sur la colonne `CustomerID` et un index en cluster sur `TransactionID` dans la base de données AdventureWorks.

```sql
-- Create table to add the clustered index
CREATE TABLE Production.TransactionHistoryArchive1
   (
      CustomerID uniqueidentifier DEFAULT NEWSEQUENTIALID()
      , TransactionID int IDENTITY (1,1) NOT NULL
      , CONSTRAINT PK_TransactionHistoryArchive1_CustomerID PRIMARY KEY NONCLUSTERED (CustomerID)
   )
;

-- Now add the clustered index
CREATE CLUSTERED INDEX CIX_TransactionID ON Production.TransactionHistoryArchive1 (TransactionID);
```

## <a name="see-also"></a>Voir aussi

- [ALTER TABLE](../../t-sql/statements/alter-table-transact-sql.md)
- [CREATE TABLE](../../t-sql/statements/create-table-transact-sql.md) 
- [table_constraint](../../t-sql/statements/alter-table-table-constraint-transact-sql.md)
