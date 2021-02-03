---
description: Créer des relations de clé étrangère
title: Créer des relations de clé étrangère | Microsoft Docs
ms.custom: ''
ms.date: 06/19/2020
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: vanto
ms.technology: table-view-index
ms.topic: conceptual
helpviewer_keywords:
- relationships [SQL Server], creating
ms.assetid: 867a54b8-5be4-46e6-9702-49ae6dabf67c
author: stevestein
ms.author: sstein
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: b58c999fd74f7a899e7b700022f57b7171ed39b7
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/01/2021
ms.locfileid: "99234709"
---
# <a name="create-foreign-key-relationships"></a>Créer des relations de clé étrangère


[!INCLUDE [sqlserver2016-asdb-asdbmi](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi.md)]

Cet article explique comment créer des relations de clé étrangère dans [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] à l'aide de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] ou de [!INCLUDE[tsql](../../includes/tsql-md.md)]. Vous créez une relation entre deux tables lorsque vous voulez associer des lignes d'une table à des lignes appartenant à une autre table.

## <a name="permissions"></a>Autorisations

La création d'une nouvelle table avec une clé étrangère nécessite une autorisation [CREATE TABLE](../../t-sql/statements/create-table-transact-sql.md) dans la base de données et une autorisation [ALTER](../../t-sql/statements/alter-schema-transact-sql.md) pour le schéma dans lequel la table a été créée.

La création d'une clé étrangère dans une table existante nécessite l'autorisation [ALTER](../../t-sql/statements/alter-table-transact-sql.md) sur la table.

## <a name="limits-and-restrictions"></a><a name="BeforeYouBegin"></a>Limites et restrictions

- Une contrainte de clé étrangère ne doit pas nécessairement être liée uniquement à une contrainte de clé primaire dans une autre table. Les clés étrangères peuvent également être définies pour référencer les colonnes d’une contrainte UNIQUE dans une autre table.
- Lorsqu'une valeur différente de NULL est entrée dans la colonne d'une contrainte FOREIGN KEY, la valeur doit exister dans la colonne référencée. Dans le cas contraire, le système retourne un message d'erreur signalant une violation de clé étrangère. Pour vous assurer que toutes les valeurs d'une contrainte de clé étrangère composite sont vérifiées, spécifiez NOT NULL pour toutes les colonnes participant à la contrainte.
- Les contraintes FOREIGN KEY ne peuvent faire référence qu'à des tables au sein de la même base de données sur le même serveur. L'intégrité référentielle inter-base de données doit être implémentée via les déclencheurs. Pour plus d’informations, consultez [CREATE TRIGGER](../../t-sql/statements/create-trigger-transact-sql.md).
- Les contraintes FOREIGN KEY peuvent faire référence à une autre colonne dans la même table, ce qui est appelé une auto-référence.
- Une contrainte FOREIGN KEY spécifiée au niveau de la colonne ne peut lister qu'une colonne de référence. Cette colonne doit avoir le même type de données que la colonne pour laquelle la contrainte est définie.
- Une contrainte FOREIGN KEY spécifiée au niveau de la table doit avoir le même nombre de colonnes de référence que le nombre de colonnes de la liste des colonnes de la contrainte. Le type de données de chaque colonne de référence doit également être identique à la colonne de référence correspondante dans la liste des colonnes.
- Le[!INCLUDE[ssDE](../../includes/ssde-md.md)] n’a pas de limite prédéfinie quant au nombre de contraintes de clé étrangère qu’une table peut contenir et qui référencent d’autres tables. Le [!INCLUDE[ssDE](../../includes/ssde-md.md)] ne limite pas non plus le nombre de contraintes de clé étrangère détenues par d’autres tables qui font référence à une table spécifique. Cependant, le nombre réel de contraintes FOREIGN KEY qui peuvent être utilisées est limité par la configuration matérielle et par la conception de la base de données et de l'application. Une table peut référencer au maximum 253 autres tables et colonnes en tant que clés étrangères (références sortantes). [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] et versions ultérieures font passer de 253 à 10 000 le nombre limite des autres tables et colonnes pouvant référencer des colonnes dans une table unique (références entrantes). (Cela nécessite au minimum le niveau de compatibilité 130). Cette augmentation est soumise aux restrictions suivantes :

  - Les références de clés étrangères supérieures à 253 sont prises en charge pour les opérations DELETE et UPDATE DML. Les opérations MERGE ne sont pas prises en charge.
  - Une table comportant une clé étrangère référencée vers elle-même est toujours limitée à 253 références de clés étrangères.
  - Les références de clés étrangères supérieures à 253 ne sont actuellement disponibles ni pour les index columnstore, ni pour les tables à mémoire optimisée, ni pour Stretch Database.

- Les contraintes FOREIGN KEY ne sont pas appliquées dans les tables temporaires.
- Si une clé étrangère est définie sur une colonne avec le type de données CLR défini par l'utilisateur, l'implémentation du type doit prendre en charge le tri binaire. Pour plus d’informations, consultez [Types CLR définis par l’utilisateur](../../relational-databases/clr-integration-database-objects-user-defined-types/clr-user-defined-types.md).
- Une colonne de type **varchar(max)** ne peut participer à une contrainte FOREIGN KEY que si la clé primaire qu’elle référence est également définie comme étant de type **varchar(max)** .

## <a name="create-a-foreign-key-relationship-in-table-designer"></a>Créer une relation de clé étrangère dans le Concepteur de tables

### <a name="using-sql-server-management-studio"></a>Utilisation de SQL Server Management Studio

1. Dans l’Explorateur d’objets, cliquez avec le bouton droit sur la table qui se trouve du côté clé étrangère de la relation et cliquez sur **Conception**.

   La table s'ouvre dans le [**Concepteur de tables**](../../ssms/visual-db-tools/design-tables-visual-database-tools.md).
2. Dans le menu **Concepteur de tables** , cliquez sur **Relations**.
3. Dans la boîte de dialogue **Relations de clé étrangère** , cliquez sur **Ajouter**.

   La relation s'affiche dans la liste **Relation sélectionné** e avec un nom fourni par le système au format FK_\<*tablename*>_\<*tablename*>, où  *est le nom de la table* de clé étrangère.
4. Cliquez sur la relation dans la liste **Relation sélectionnée** .
5. Cliquez sur **Spécification de tables et colonnes** dans la grille affichée à droite et cliquez sur le bouton de sélection ( **...** ), à droite de la propriété.
6. Dans la liste déroulante **Clé primaire** de la boîte de dialogue **Tables et colonnes** , choisissez la table qui sera du côté clé primaire de la relation.
7. Dans la grille située au-dessous, choisissez les colonnes qui participent à la clé primaire de la table. Dans la cellule de la grille située à droite de chaque colonne, choisissez la colonne clé étrangère correspondante dans la table de clé étrangère.

   Le **Concepteur de tables** propose un nom pour la relation. Pour changer ce nom, modifiez le contenu de la zone de texte **Nom de la relation** .
8. Choisissez **OK** pour créer la relation.
9. Fermez la fenêtre du concepteur de tables et **enregistrez** vos modifications pour que la modification de la relation de clé étrangère prennent effet.

## <a name="create-a-foreign-key-in-a-new-table"></a>Créer une clé étrangère dans une nouvelle table

### <a name="using-transact-sql"></a>Utilisation de Transact-SQL

L'exemple suivant crée une table et définit une contrainte de clé étrangère sur la colonne `TempID` qui fait référence à la colonne `SalesReasonID` dans la table `Sales.SalesReason` de la base de données AdventureWorks. Les clauses ON DELETE CASCADE et ON UPDATE CASCADE sont utilisées pour garantir que les modifications apportées à la table `Sales.SalesReason` sont automatiquement propagées dans la table `Sales.TempSalesReason` .    

```sql
CREATE TABLE Sales.TempSalesReason 
   (
      TempID int NOT NULL, Name nvarchar(50)
      , CONSTRAINT PK_TempSales PRIMARY KEY NONCLUSTERED (TempID)
      , CONSTRAINT FK_TempSales_SalesReason FOREIGN KEY (TempID)
        REFERENCES Sales.SalesReason (SalesReasonID)
        ON DELETE CASCADE
        ON UPDATE CASCADE
   )
;
```

## <a name="create-a-foreign-key-in-an-existing-table"></a>Créer une clé étrangère dans une table existante

### <a name="using-transact-sql"></a>Utilisation de Transact-SQL
L'exemple suivant crée une clé étrangère sur la colonne `TempID` et fait référence à la colonne `SalesReasonID` dans la table `Sales.SalesReason` de la base de données AdventureWorks.

```sql
ALTER TABLE Sales.TempSalesReason
   ADD CONSTRAINT FK_TempSales_SalesReason FOREIGN KEY (TempID)
      REFERENCES Sales.SalesReason (SalesReasonID)
      ON DELETE CASCADE
      ON UPDATE CASCADE
;
```

## <a name="next-steps"></a>Étapes suivantes

Pour plus d'informations, consultez les pages suivantes :

- [Contraintes de clé primaire et de clé étrangère](primary-and-foreign-key-constraints.md)
- [GRANT - Autorisations sur une base de données](../../t-sql/statements/grant-database-permissions-transact-sql.md)
- [ALTER TABLE](../../t-sql/statements/alter-table-transact-sql.md)
- [CREATE TABLE](../../t-sql/statements/create-table-transact-sql.md)
- [ALTER TABLE table_constraint](../../t-sql/statements/alter-table-table-constraint-transact-sql.md).
