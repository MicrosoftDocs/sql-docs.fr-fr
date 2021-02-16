---
description: Contraintes d’arête de graphe
title: Contraintes d’arête de graphe | Microsoft Docs
ms.custom: ''
ms.date: 09/09/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: table-view-index
ms.topic: conceptual
helpviewer_keywords:
- edge constraints [SQL Server]
- CONNECTION constraints
- edge constraints [Azure SQL Database]
- graph edge constraints
- SQL Graph
author: shkale-msft
ms.author: shkale
monikerRange: '>=sql-server-2017||>=sql-server-linux-2017||=azuresqldb-mi-current||=azuresqldb-current'
ms.openlocfilehash: 972e0a1cf391f321426398e94997740712580305
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100346714"
---
# <a name="edge-constraints"></a>Contraintes d’arête

[!INCLUDE[sql-asdb](../../includes/applies-to-version/sql-asdb.md)]

Les contraintes Edge peuvent être utilisées pour appliquer l’intégrité des données et une sémantique spécifique sur les tables Edge dans une base de données de graphe [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].

## <a name="edge-constraints"></a><a name="Connection"></a> Contraintes d’arête

Dans la première version des fonctionnalités de graphe, les tables d’arêtes n’appliquaient rien pour les points de terminaison de l’arête. Autrement dit, une arête dans une base de données de graphe pouvait connecter n’importe quel nœud à n’importe quel autre nœud, quel que soit leur type.

Cette version introduit des contraintes d’arête, qui permettent aux utilisateurs d’ajouter des contraintes à leurs tables d’arêtes, appliquant ainsi une sémantique spécifique et préservant l’intégrité des données. Lorsqu’une nouvelle arête est ajoutée à une table d’arêtes avec des contraintes d’arête, le [!INCLUDE[ssDE](../../includes/ssde-md.md)] fait en sorte que les nœuds que l’arête essaie de connecter existent dans les tables de nœuds appropriées. Il est également garanti qu’un nœud ne peut pas être supprimé, s’il est encore référencé par une arête.

### <a name="edge-constraint-clauses"></a>Clauses de contrainte d’arête

Chaque contrainte d’arête se compose d’une ou plusieurs clauses de contrainte d’arête. Une clause de contrainte d’arête est la paire de nœuds FROM et TO que l’arête donnée pouvait connecter.

Considérez que vous avez les nœuds `Product` et `Customer` dans votre graphe et que vous utilisez l’arête `Bought` pour connecter ces nœuds. La clause de contrainte d’arête spécifie la paire de nœuds FROM et TO, et la direction de l’arête. Dans ce cas, la clause de contrainte d’arête sera `Customer` TO `Product`. Autrement dit, l’insertion d’une arête `Bought` allant d’un `Customer` à `Product` sera autorisée. Les tentatives d’insertion d’une arête allant de `Product` à `Customer` échouent.

- Une clause de contrainte d’arête contient une paire de tables de nœuds FROM et TO sur laquelle la contrainte d’arête est appliquée.
- Les utilisateurs peuvent spécifier plusieurs clauses de contrainte d’arête par contrainte d’arête, lesquelles seront appliquées en tant que disjonction.
- Si plusieurs contraintes d’arête sont créées sur une table d’arêtes unique, les arêtes doivent satisfaire TOUTES les contraintes à autoriser.

### <a name="indexes-on-edge-constraints"></a>Index sur les contraintes d’arête

La création d’une contrainte d’arête ne crée pas automatiquement un index correspondant sur les colonnes `$from_id` et `$to_id` dans la table d’arêtes. La création manuelle d’un index sur une paire `$from_id`, `$to_id` est recommandée si vous avez des requêtes de recherche de points ou une charge de travail OLTP.

### <a name="on-delete-referential-actions-on-edge-constraints"></a>Actions référentielles ON DELETE sur des contraintes d’arête
Les actions en cascade sur une contrainte d’arête permettent aux utilisateurs de définir les actions que le moteur de base de données effectue quand un utilisateur supprime le ou les nœuds auxquels l’arête donnée est reliée. Les actions référentielles suivantes peuvent être définies :  
*NO ACTION*   
Le moteur de base de données génère une erreur quand vous essayez de supprimer un nœud relié à des arêtes.  

*CASCADE*   
Lorsqu’un nœud relié à des arêtes est supprimé de la base de données, les arêtes sont supprimées.  

## <a name="working-with-edge-constraints"></a>Utilisation des contraintes d’arête

Vous pouvez définir une contrainte d’arête dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] à l'aide de [!INCLUDE[tsql](../../includes/tsql-md.md)]. Une contrainte d’arête peut être définie sur une table d’arêtes de graphe uniquement. Pour créer, supprimer ou modifier une contrainte d’arête, vous devez disposer de l’autorisation **ALTER** sur la table.

### <a name="create-edge-constraints"></a>Créer des contraintes d’arête

Les exemples suivants vous montrent comment créer des contraintes d’arête sur des tables nouvelles ou existantes

#### <a name="to-create-an-edge-constraint-on-a-new-edge-table"></a>Pour créer une contrainte d’arête sur une nouvelle table d’arêtes

L’exemple suivant crée une contrainte d’arête sur la table d’arêtes **bought**.

```sql
-- CREATE node and edge tables
CREATE TABLE Customer
   (
      ID INTEGER PRIMARY KEY
      ,CustomerName VARCHAR(100)
   )
AS NODE;
GO
CREATE TABLE Product
   (
      ID INTEGER PRIMARY KEY
      ,ProductName VARCHAR(100)
   )
AS NODE;
GO
CREATE TABLE bought
   (
      PurchaseCount INT
         ,CONSTRAINT EC_BOUGHT CONNECTION (Customer TO Product) ON DELETE NO ACTION
   )
   AS EDGE;
   ```

#### <a name="defining-referential-actions-on-a-new-edge-table"></a>Définition d’actions référentielles sur une nouvelle table d’arêtes 

L’exemple suivant crée une contrainte d’arête sur la table d’arêtes **bought** et définit l’action référentielle DELETE CASCADE. 

```sql
-- CREATE node and edge tables
CREATE TABLE Customer
   (
      ID INTEGER PRIMARY KEY
      ,CustomerName VARCHAR(100)
   )
AS NODE;
GO
CREATE TABLE Product
   (
      ID INTEGER PRIMARY KEY
      ,ProductName VARCHAR(100)
   )
AS NODE;
GO
CREATE TABLE bought
   (
      PurchaseCount INT
         ,CONSTRAINT EC_BOUGHT CONNECTION (Customer TO Product) ON DELETE CASCADE
   )
   AS EDGE;
   ```

#### <a name="to-add-edge-constraint-to-an-existing-edge-table"></a>Pour ajouter une contrainte d’arête à une table d’arêtes existante

L’exemple suivant utilise ALTER TABLE pour ajouter une contrainte d’arête à la table d’arêtes **bought**.

```sql
-- CREATE node and edge tables
CREATE TABLE Customer
   (
      ID INTEGER PRIMARY KEY,
      , CustomerName VARCHAR(100)
   )
   AS NODE;
CREATE TABLE Product
   (
      ID INTEGER PRIMARY KEY
      , ProductName VARCHAR(100)
   )
   AS NODE;
GO
CREATE TABLE bought
   (
      PurchaseCount INT
   )
   AS EDGE;
GO
ALTER TABLE bought ADD CONSTRAINT EC_BOUGHT1 CONNECTION (Customer TO Product);
```  

#### <a name="create-a-new-edge-constraint-on-existing-edge-table-with-additional-edge-constraint-clauses"></a>Créer une nouvelle contrainte d’arête sur une table d’arêtes existante, avec des clauses de contrainte d’arête supplémentaires

L’exemple suivant utilise la commande `ALTER TABLE` pour ajouter une nouvelle contrainte d’arête avec des clauses de contrainte d’arête supplémentaires sur la table d’arêtes **bought**.
  
```sql
-- CREATE node and edge tables
CREATE TABLE Customer
   (
      ID INTEGER PRIMARY KEY
      , CustomerName VARCHAR(100)
   )
   AS NODE;
GO
CREATE TABLE Supplier
   (
      ID INTEGER PRIMARY KEY
      , SupplierName VARCHAR(100)
   )
   AS NODE;
GO
CREATE TABLE Product
   (
      ID INTEGER PRIMARY KEY
      , ProductName VARCHAR(100)
   )
   AS NODE;
GO
CREATE TABLE bought
   (
      PurchaseCount INT
      , CONSTRAINT EC_BOUGHT CONNECTION (Customer TO Product)
   )
   AS EDGE;
-- Drop the existing edge constraint first and then create a new one.
ALTER TABLE bought DROP CONSTRAINT EC_BOUGHT;
GO
-- User ALTER TABLE to create a new edge constraint.
ALTER TABLE bought ADD CONSTRAINT EC_BOUGHT1 CONNECTION (Customer TO Product, Supplier TO Product);
```  

Dans l’exemple précédent, il existe deux clauses de contrainte d’arête dans la contrainte *EC_BOUGHT1*, une qui connecte **Customer** à **Product** et l’autre qui connecte **Supplier** à **Product**. Ces deux clauses sont appliquées en disjonction. Autrement dit, une arête donnée doit satisfaire une de ces deux clauses pour être autorisée dans la table d’arêtes.

#### <a name="creating-a-new-edge-constraint-on-existing-edge-table-with-new-edge-constraint-clause"></a>Créer une nouvelle contrainte d’arête sur une table d’arêtes existante, avec une nouvelle clause de contrainte d’arête

L’exemple suivant utilise la commande `ALTER TABLE` pour ajouter une nouvelle contrainte d’arête avec une nouvelle clause de contrainte d’arête sur la table d’arêtes **bought**.
  
 ```sql
-- CREATE node and edge tables
CREATE TABLE Customer
   (
      ID INTEGER PRIMARY KEY
      , CustomerName VARCHAR(100)
   )
   AS NODE;
GO
CREATE TABLE Supplier
   (
      ID INTEGER PRIMARY KEY
      , SupplierName VARCHAR(100)
   )
   AS NODE;
GO
CREATE TABLE Product
   (
      ID INTEGER PRIMARY KEY
      , ProductName VARCHAR(100)
   )
   AS NODE;
GO
CREATE TABLE bought
   (
      PurchaseCount INT,
         CONSTRAINT EC_BOUGHT CONNECTION (Customer TO Product)
   )
   AS EDGE;
GO
ALTER TABLE bought ADD CONSTRAINT EC_BOUGHT1 CONNECTION (Supplier TO Product);
 ```  

Dans l’exemple précédent, nous avons créé deux contraintes d’arête distinctes sur la table d’arêtes **bought**, *EC_BOUGHT* et *EC_BOUGHT1*. Ces deux contraintes d’arête ont des clauses de contrainte d’arête différentes. Si une table d’arêtes fait l’objet de plusieurs contraintes d’arête, une arête donnée doit satisfaire **TOUTES** les contraintes d’arête pour être autorisée dans la table d’arêtes. Dans la mesure où aucune arête ne peut satisfaire à la fois *EC_BOUGHT* et *EC_BOUGHT1* ici, la table d’arêtes **bought** doit rester vide.

Pour que les insertions réussissent dans cette table d’arêtes, l’une des contraintes d’arête doit être supprimée, ou les deux doivent être supprimées et une nouvelle contrainte d’arête doit être créée, contenant les deux clauses de contrainte d’arête.

```sql
-- Drop the existing edge constraint first and then create a new one.
ALTER TABLE bought DROP CONSTRAINT EC_BOUGHT;
GO
ALTER TABLE bought DROP CONSTRAINT EC_BOUGHT1;
GO
ALTER TABLE bought ADD CONSTRAINT EC_BOUGHT_NEW CONNECTION (Customer TO Product, Supplier TO Product);
GO
```  

Pour qu’une arête donnée soit autorisée dans l’arête **bought**, elle doit satisfaire l’une des clauses de contrainte d’arête figurant dans la contrainte *EC_BOUGHT_NEW*. Par conséquent, toute arête qui essaie de connecter des nœuds valides **Customer** à **Product** ou **Supplier** à **Product**, sera autorisée.

### <a name="delete-edge-constraints"></a>Supprimer des contraintes d’arête

L'exemple suivant identifie d'abord le nom de la contrainte d’arête, puis supprime la contrainte.  
  
```sql
-- CREATE node and edge tables
CREATE TABLE Customer
   (
      ID INTEGER PRIMARY KEY
      , CustomerName VARCHAR(100)
   )
   AS NODE;
GO
CREATE TABLE Product
   (
      ID INTEGER PRIMARY KEY
      , ProductName VARCHAR(100)
   ) AS NODE;
GO
CREATE TABLE bought
   (
      PurchaseCount INT
      , CONSTRAINT EC_BOUGHT CONNECTION (Customer TO Product)
    )
    AS EDGE;
GO
-- Return the name of edge constraint.
SELECT name  
   FROM sys.edge_constraints  
   WHERE type = 'EC' AND parent_object_id = OBJECT_ID('bought');  
GO  
-- Delete the primary key constraint.  
ALTER TABLE bought
DROP CONSTRAINT EC_BOUGHT;
```  

### <a name="modify-edge-constraints"></a>Modifier des contraintes d’arête

Pour modifier une contrainte d’arête à l’aide de Transact-SQL, vous devez d’abord supprimer la contrainte d’arête existante, puis la recréer avec la nouvelle définition.


### <a name="view-edge-constraints"></a>Afficher des contraintes d’arête

[!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] Pour plus d'informations, consultez [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).

L'exemple retourne toutes les contraintes d’arête et leurs propriétés pour la table d’arêtes `bought` dans la base de données tempdb.  

```sql
-- CREATE node and edge tables
CREATE TABLE Customer
   (
      ID INTEGER PRIMARY KEY
      , CustomerName VARCHAR(100)
   )
   AS NODE;
GO
CREATE TABLE Supplier
   (
      ID INTEGER PRIMARY KEY
      , SupplierName VARCHAR(100)
   )
   AS NODE;
   GO
CREATE TABLE Product
   (
      ID INTEGER PRIMARY KEY
      , ProductName VARCHAR(100)
   )
   AS NODE;

-- CREATE edge table with edge constraints.
CREATE TABLE bought
   (
      PurchaseCount INT
      , CONSTRAINT EC_BOUGHT CONNECTION (Customer TO Product, Supplier TO Product)
   )
   AS EDGE;

-- Query sys.edge_constraints and sys.edge_constraint_clauses to view
-- edge constraint properties.
SELECT
   EC.name AS edge_constraint_name
   , OBJECT_NAME(EC.parent_object_id) AS edge_table_name
   , OBJECT_NAME(ECC.from_object_id) AS from_node_table_name
   , OBJECT_NAME(ECC.to_object_id) AS to_node_table_name
   , is_disabled
   , is_not_trusted
FROM sys.edge_constraints EC
   INNER JOIN sys.edge_constraint_clauses ECC
   ON EC.object_id = ECC.object_id
WHERE EC.parent_object_id = object_id('bought');
```  

## <a name="related-tasks"></a>Tâches associées

[CREATE TABLE (SQL Graph)](../../t-sql/statements/create-table-sql-graph.md)  
[ALTER TABLE table_constraint](../../t-sql/statements/alter-table-table-constraint-transact-sql.md)  

Pour plus d’informations sur la technologie de graphe dans SQL Server, consultez [Traitement des graphes avec SQL Server et Azure SQL Database](../graphs/sql-graph-overview.md).
