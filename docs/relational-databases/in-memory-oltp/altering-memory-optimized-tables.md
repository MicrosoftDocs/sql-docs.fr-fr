---
title: Modification des tables à mémoire optimisée | Microsoft Docs
description: Découvrez comment l’instruction ALTER TABLE effectue des modifications de schéma et d’index sur les tables à mémoire optimisée. Combinez des opérations ADD, DROP et ALTER dans une instruction unique.
ms.custom: ''
ms.date: 10/22/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: in-memory-oltp
ms.topic: conceptual
ms.assetid: 690b70b7-5be1-4014-af97-54e531997839
author: MightyPen
ms.author: genemi
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: da12d83efd43c7ca6113348adee8e3bde14d9472
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/14/2020
ms.locfileid: "97465470"
---
# <a name="altering-memory-optimized-tables"></a>Modification des tables à mémoire optimisée

[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Vous pouvez effectuer des modifications de schéma et d’index sur les tables optimisées en mémoire en utilisant l’instruction ALTER TABLE. Dans SQL Server 2016 et Azure SQL Database, les opérations ALTER TABLE sur des tables à mémoire optimisée sont HORS CONNEXION, ce qui signifie que la table n’est pas disponible pour les requêtes quand l’opération est en cours. Dans le cadre de cette modification, l’application de base de données peut continuer à s’exécuter, mais toute opération accédant à la table est bloquée jusqu’à la fin du processus de modification. Il est possible de combiner plusieurs opérations ADD, DROP ou ALTER dans une seule instruction ALTER TABLE.

> [!IMPORTANT]
> Azure SQL Managed Instance ne prend pas en charge les tables à mémoire optimisée dans le niveau de service Usage général.
  
## <a name="alter-table"></a>ALTER TABLE  

La syntaxe ALTER TABLE permet d’apporter des modifications au schéma de la table, et d’ajouter, de supprimer et de régénérer des index. Les index sont considérés comme faisant partie intégrante de la définition de table :  
  
- La syntaxe ALTER TABLE… ADD/DROP/ALTER INDEX est uniquement prise en charge pour les tables optimisées en mémoire.  
  
- En l’absence d’instruction ALTER TABLE, les instructions [CREATE INDEX](../../t-sql/statements/create-index-transact-sql.md), [DROP INDEX](../../t-sql/statements/drop-index-transact-sql.md), [ALTER INDEX](../../t-sql/statements/alter-index-transact-sql.md) et [PAD_INDEX](../../t-sql/statements/alter-table-index-option-transact-sql.md) ne sont pas prises en charge pour les index sur les tables à mémoire optimisée.  
  
Les types de modifications pris en charge sont les suivants :  
  
- modification du nombre de compartiments ;  
  
- ajout et suppression d’un index ;  
  
- modification, ajout et suppression d’une colonne ;  
  
- ajout et suppression d’une contrainte.  
  
 Pour plus d’informations sur la fonctionnalité ALTER TABLE et sur la syntaxe complète, consultez [ALTER TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-table-transact-sql.md).  
  
## <a name="schema-bound-dependency"></a>Dépendance liée au schéma

 Les procédures stockées compilées en mode natif doivent être liées au schéma, ce qui signifie qu’elles ont une dépendance liée au schéma vis-à-vis des tables optimisées en mémoire auxquelles elles accèdent et des colonnes qu’elles référencent. Une dépendance liée au schéma est une relation entre deux entités qui empêche l’entité référencée d’être supprimée ou modifiée de façon incompatible tant que l’entité de référence existe.  
  
 Par exemple, si une procédure stockée compilée en mode natif et liée au schéma fait référence à une colonne *c1* de la table *mytable*, la colonne *c1* ne peut pas être supprimée. De même, s’il existe une telle procédure avec une instruction INSERT sans liste de colonnes (par exemple, `INSERT INTO dbo.mytable VALUES (...)`), alors aucune colonne de la table ne peut être supprimée.  

## <a name="logging-of-alter-table-on-memory-optimized-tables"></a>Journalisation de l’instruction ALTER TABLE sur des tables optimisées en mémoire

Sur une table optimisée en mémoire, la plupart des scénarios ALTER TABLE s’exécutent désormais en parallèle, d’où une optimisation des écritures dans le journal des transactions. L’optimisation est obtenue par la journalisation des seules modifications de métadonnées dans le journal des transactions. Cependant, les opérations ALTER TABLE suivantes sont effectuées en mode à thread unique et ne sont pas optimisées pour la journalisation.

L’opération monothread dans ce cas consigne dans le journal des transactions l’intégralité du contenu de la table modifiée. Voici la liste des opérations à thread unique :

- Modifier ou ajouter une colonne pour utiliser un type d’objet volumineux (LOB) : nvarchar(max), varchar(max) ou varbinary(max).

- Ajouter ou supprimer un index COLUMNSTORE.

- Presque tout ce qui affecte une [colonne hors ligne](../../relational-databases/in-memory-oltp/supported-data-types-for-in-memory-oltp.md).

  - Provoquer le déplacement d’une colonne sur une ligne en mode hors ligne.
  - Provoquer le déplacement d’une colonne en mode hors ligne sur une ligne.
  - Créer une colonne hors ligne.
  - *Exception :* L’allongement d’une colonne déjà hors ligne est journalisé de la manière optimisée.
  
## <a name="examples"></a>Exemples

L’exemple ci-après modifie le nombre de compartiments d’un index de hachage existant. Cette opération régénère l’index de hachage avec le nouveau nombre de compartiments, tandis que les autres propriétés de l’index de hachage restent identiques.  

```sql
ALTER TABLE Sales.SalesOrderDetail_inmem
       ALTER INDEX imPK_SalesOrderDetail_SalesOrderID_SalesOrderDetailID  
              REBUILD WITH (BUCKET_COUNT=67108864);  
GO
```

L’exemple ci-après ajoute une colonne avec une contrainte NOT NULL et une définition DEFAULT, et utilise WITH VALUES afin de spécifier des valeurs pour chaque ligne existante de la table. Si l'option WITH VALUES n'est pas utilisée, chaque ligne a la valeur NULL dans la nouvelle colonne.  

```sql
ALTER TABLE Sales.SalesOrderDetail_inmem  
       ADD Comment NVARCHAR(100) NOT NULL DEFAULT N'' WITH VALUES;  
GO
```

L’exemple ci-après ajoute une contrainte de clé primaire à une colonne existante.  

```sql
CREATE TABLE dbo.UserSession (
   SessionId int not null,
   UserId int not null,
   CreatedDate datetime2 not null,
   ShoppingCartId int,
   index ix_UserId nonclustered hash (UserId) with (bucket_count=400000)
)
WITH (MEMORY_OPTIMIZED=ON, DURABILITY=SCHEMA_ONLY) ;  
GO  
  
ALTER TABLE dbo.UserSession  
       ADD CONSTRAINT PK_UserSession PRIMARY KEY NONCLUSTERED (SessionId);  
GO
```

L’exemple suivant supprime un index.  

```sql
ALTER TABLE Sales.SalesOrderDetail_inmem  
       DROP INDEX ix_ModifiedDate;  
GO
```  

L’exemple suivant ajoute un index.  

```sql  
ALTER TABLE Sales.SalesOrderDetail_inmem  
       ADD INDEX ix_ModifiedDate (ModifiedDate);  
GO  
```  

L’exemple suivant ajoute plusieurs colonnes, avec un index et des contraintes.  

```sql
ALTER TABLE Sales.SalesOrderDetail_inmem  
       ADD    CustomerID int NOT NULL DEFAULT -1 WITH VALUES,  
              ShipMethodID int NOT NULL DEFAULT -1 WITH VALUES,  
              INDEX ix_Customer (CustomerID);  
GO  
```

<a name="logging-of-alter-table-on-memory-optimized-tables-124"></a>

## <a name="see-also"></a>Voir aussi  

[Tables à mémoire optimisée](./sample-database-for-in-memory-oltp.md)