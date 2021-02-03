---
description: IDENTITY (Fonction) (Transact-SQL)
title: IDENTITY (fonction) (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- IDENTITY_TSQL
- IDENTITY
dev_langs:
- TSQL
helpviewer_keywords:
- IDENTITY function
- SELECT statement [SQL Server], IDENTITY function
- inserting identity columns
- columns [SQL Server], creating
- identity columns [SQL Server], IDENTITY function
ms.assetid: ebec77eb-fc02-4feb-b6c5-f0098d43ccb6
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: bad6595806a3b50cc0634cecbae2c4db098799ba
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99189925"
---
# <a name="identity-function-transact-sql"></a>IDENTITY (Fonction) (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  S’utilise uniquement dans une instruction SELECT avec une clause INTO *table* pour insérer une colonne d’identité dans une nouvelle table. Bien qu'elles soient similaires, la fonction IDENTITY n'est pas identique à la propriété IDENTITY qui est utilisée avec CREATE TABLE et ALTER TABLE.  
  
> [!NOTE]  
>  Pour créer un numéro à incrémentation automatique qui peut être utilisé dans plusieurs tables ou être appelé par des applications sans faire référence à une table, consultez [Numéros de séquence](../../relational-databases/sequence-numbers/sequence-numbers.md).  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql
IDENTITY (data_type [ , seed , increment ] ) AS column_name  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments
 *data_type*  
 Type de données de la colonne d'identité. Tous les types de données de la catégorie de type entier sont valides pour une colonne d’identité, à l’exception des types de données **bit** et **decimal**.  
  
 *seed*  
 Entier à affecter à la première ligne de la table. La valeur d’identité suivante est affectée à chaque ligne suivante. Cette valeur est égale à la dernière valeur IDENTITY augmentée de la valeur *increment*. Si vous ne spécifiez ni *seed* ni *increment*, les deux arguments reçoivent par défaut la valeur 1.  
  
 *increment*  
 Entier à ajouter à la valeur *seed* pour des lignes successives de la table.  
  
 *column_name*  
 Nom de la colonne qui doit être insérée dans la nouvelle table.  
  
## <a name="return-types"></a>Types de retour  
 Retourne le même type que *data_type*.  
  
## <a name="remarks"></a>Remarques  
 Étant donné que cette fonction crée une colonne dans une table, vous devez spécifier pour la colonne un nom figurant dans la liste de sélection de l'une des manières suivantes :  
  
```sql  
--(1)  
SELECT IDENTITY(int, 1,1) AS ID_Num  
INTO NewTable  
FROM OldTable;  
  
--(2)  
SELECT ID_Num = IDENTITY(int, 1, 1)  
INTO NewTable  
FROM OldTable;  
```  
  
## <a name="examples"></a>Exemples  
 L'exemple suivant insère toutes les lignes de la table `Contact` de la base de données [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)] dans une nouvelle table appelée `NewContact`. La fonction IDENTITY est utilisée pour démarrer les numéros d'identification à 100 au lieu de 1 dans la table `NewContact`.  
  
```sql  
USE AdventureWorks2012;  
GO  
IF OBJECT_ID (N'Person.NewContact', N'U') IS NOT NULL  
    DROP TABLE Person.NewContact;  
GO  
ALTER DATABASE AdventureWorks2012 SET RECOVERY BULK_LOGGED;  
GO  
SELECT  IDENTITY(smallint, 100, 1) AS ContactNum,  
        FirstName AS First,  
        LastName AS Last  
INTO Person.NewContact  
FROM Person.Person;  
GO  
ALTER DATABASE AdventureWorks2012 SET RECOVERY FULL;  
GO  
SELECT ContactNum, First, Last FROM Person.NewContact;  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [CREATE TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/create-table-transact-sql.md)   
 [@@IDENTITY &#40;Transact-SQL&#41;](../../t-sql/functions/identity-transact-sql.md)   
 [IDENTITY, propriété &#40;Transact-SQL&#41;](../../t-sql/statements/create-table-transact-sql-identity-property.md)   
 [SELECT @local_variable &#40;Transact-SQL&#41;](../../t-sql/language-elements/select-local-variable-transact-sql.md)   
 [DBCC CHECKIDENT &#40;Transact-SQL&#41;](../../t-sql/database-console-commands/dbcc-checkident-transact-sql.md)   
 [sys.identity_columns &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-identity-columns-transact-sql.md)  
  
  
