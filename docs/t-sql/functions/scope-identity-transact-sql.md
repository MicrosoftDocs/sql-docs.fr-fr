---
description: SCOPE_IDENTITY (Transact-SQL)
title: SCOPE_IDENTITY (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/06/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- SCOPE_IDENTITY
- SCOPE_IDENTITY_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- identity columns [SQL Server], SCOPE_IDENTITY function
- SCOPE_IDENTITY function
- last-inserted identity values
- identity values [SQL Server], last-inserted
ms.assetid: eef24670-059b-4f10-91d4-a67bc1ed12ab
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: bb49f217e40b13b106743da61169eaf35a58ef1c
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99183461"
---
# <a name="scope_identity-transact-sql"></a>SCOPE_IDENTITY (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Renvoie la dernière valeur d'identité insérée dans une colonne d'identité dans la même étendue. Une étendue est un module : procédure stockée, déclencheur, fonction ou lot. Par conséquent, deux instructions sont dans la même étendue si elles se trouvent dans la même procédure stockée ou fonction, ou dans le même lot.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql  
SCOPE_IDENTITY()  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="return-types"></a>Types de retour
 **numeric(38,0)**  
  
## <a name="remarks"></a>Remarques  
 SCOPE_IDENTITY, IDENT_CURRENT et @@IDENTITY sont des fonctions similaires car elles renvoient des valeurs insérées dans des colonnes d’identité.  
  
 IDENT_CURRENT n'est pas limitée par l'étendue et par la session ; elle est limitée à une table spécifiée. IDENT_CURRENT renvoie la valeur générée pour une table spécifique dans n'importe quelle session et n'importe quelle étendue. Pour plus d’informations, consultez [IDENT_CURRENT &#40;Transact-SQL&#41;](../../t-sql/functions/ident-current-transact-sql.md).  
  
 SCOPE_IDENTITY et @@IDENTITY renvoient les dernières valeurs d’identité générées dans une table de la session en cours. Toutefois, SCOPE_IDENTITY renvoie les valeurs insérées uniquement dans l’étendue actuelle. @@IDENTITY n’est pas limitée à une étendue spécifique.  
  
 Par exemple, deux tables T1 et T2 et un déclencheur INSERT sont définis sur T1. Lorsqu'une ligne est insérée dans T1, le déclencheur est activé et insère une ligne dans T2. Ce scénario met en œuvre deux étendues : l'insertion dans T1 et l'insertion dans T2 par le déclencheur.  
  
 Supposons que T1 et T2 comportent des colonnes d’identité, @@IDENTITY et SCOPE_IDENTITY renvoient des valeurs différentes à la fin d’une instruction INSERT dans T1. @@IDENTITY renvoie la dernière valeur de la colonne d’identité insérée dans toutes les étendues au cours de la session en cours. Il s'agit de la valeur insérée dans T2. SCOPE_IDENTITY() renvoie la valeur IDENTITY insérée dans T1. Il s'agit de la dernière insertion qui s'est produite dans la même étendue. La fonction SCOPE_IDENTITY() renvoie la valeur NULL si la fonction est appelée avant qu’une instruction INSERT dans une colonne d’identité soit exécutée dans l’étendue.  
  
 Les instructions et les transactions en échec peuvent modifier l'identité actuelle d'une table et créer des trous dans les valeurs des colonnes d'identité. La valeur d'identité n'est jamais annulée, même si la transaction qui a essayé d'insérer la valeur dans la table n'est pas validée. Par exemple, si une instruction INSERT échoue à cause d'une violation d'identité IGNORE_DUP_KEY, la valeur d'identité actuelle de la table augmente quand même d'une unité.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-using-identity-and-scope_identity-with-triggers"></a>R. Utilisation des fonctions @@IDENTITY et SCOPE_IDENTITY avec des déclencheurs  
 L'exemple suivant crée deux tables, `TZ` et `TY`, ainsi qu'un déclencheur INSERT sur `TZ`. Lorsqu'une ligne est insérée dans la table `TZ`, le déclencheur (`Ztrig`) est activé et insère une ligne dans `TY`.  
  
```sql  
USE tempdb;  
GO  
CREATE TABLE TZ (  
   Z_id  INT IDENTITY(1,1)PRIMARY KEY,  
   Z_name VARCHAR(20) NOT NULL);  
  
INSERT TZ  
   VALUES ('Lisa'),('Mike'),('Carla');  
  
SELECT * FROM TZ;  
```     
Jeu de résultats : Voici à quoi ressemble la table TZ :  
  
```  
Z_id   Z_name  
-------------  
1      Lisa  
2      Mike  
3      Carla  
```  
```sql 
CREATE TABLE TY (  
   Y_id  INT IDENTITY(100,5)PRIMARY KEY,  
   Y_name VARCHAR(20) NULL);  
  
INSERT TY (Y_name)  
   VALUES ('boathouse'), ('rocks'), ('elevator');  
  
SELECT * FROM TY;  
```   
Jeu de résultats : Voici à quoi ressemble TY :  
```  
Y_id  Y_name  
---------------  
100   boathouse  
105   rocks  
110   elevator  
```  

Créez le déclencheur qui insère une ligne dans la table TY lorsqu’une ligne est insérée dans la table TZ.  
```sql  
CREATE TRIGGER Ztrig  
ON TZ  
FOR INSERT AS   
   BEGIN  
   INSERT TY VALUES ('')  
   END;  
```  
Activez le déclencheur et déterminez quelles valeurs d’identité vous obtenez avec les fonctions @@IDENTITY et SCOPE_IDENTITY.   
```sql
INSERT TZ VALUES ('Rosalie');  
  
SELECT SCOPE_IDENTITY() AS [SCOPE_IDENTITY];  
GO  
SELECT @@IDENTITY AS [@@IDENTITY];  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
```
/*SCOPE_IDENTITY returns the last identity value in the same scope. This was the insert on table TZ.*/`  
SCOPE_IDENTITY  
4  

/*@@IDENTITY returns the last identity value inserted to TY by the trigger. 
  This fired because of an earlier insert on TZ.*/
@@IDENTITY  
115  
```  
  
### <a name="b-using-identity-and-scope_identity-with-replication"></a>B. Utilisation des fonctions @@IDENTITY et SCOPE_IDENTITY() avec la réplication  
 Les exemples suivants indiquent comment utiliser `@@IDENTITY` et `SCOPE_IDENTITY()` pour des insertions dans une base de données qui est publiée pour la réplication de fusion. Les deux tables mentionnées à titre d'exemple appartiennent à la base de données exemple [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] : `Person.ContactType` n'est pas publiée et `Sales.Customer` est publiée. La réplication de fusion ajoute des déclencheurs aux tables qui sont publiées. Par conséquent, `@@IDENTITY` peut retourner la valeur de l'insertion dans une table système de réplication au lieu de l'insertion dans une table utilisateur.  
  
 La table `Person.ContactType` a une valeur d’identité maximale de 20. Si vous insérez une ligne dans la table, `@@IDENTITY` et `SCOPE_IDENTITY()` retournent la même valeur.  
  
```sql  
USE AdventureWorks2012;  
GO  
INSERT INTO Person.ContactType ([Name]) VALUES ('Assistant to the Manager');  
GO  
SELECT SCOPE_IDENTITY() AS [SCOPE_IDENTITY];  
GO  
SELECT @@IDENTITY AS [@@IDENTITY];  
GO  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
```  
SCOPE_IDENTITY  
21  
@@IDENTITY  
21
```  
  
 La table `Sales.Customer` possède une valeur d'identité maximale de 29483. Si vous insérez une ligne dans la table, `@@IDENTITY` et `SCOPE_IDENTITY()` retournent des valeurs différentes. `SCOPE_IDENTITY()` retourne la valeur de l'insertion dans la table utilisateur, alors que `@@IDENTITY` retourne la valeur de l'insertion dans la table système de réplication. Utilisez `SCOPE_IDENTITY()` pour les applications qui doivent accéder à la valeur d'identité insérée.  
  
```sql  
INSERT INTO Sales.Customer ([TerritoryID],[PersonID]) VALUES (8,NULL);  
GO  
SELECT SCOPE_IDENTITY() AS [SCOPE_IDENTITY];  
GO  
SELECT @@IDENTITY AS [@@IDENTITY];  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
 ```
 SCOPE_IDENTITY  
 29484  
 @@IDENTITY  
 89
 ```  
  
## <a name="see-also"></a>Voir aussi  
 [@@IDENTITY &#40;Transact-SQL&#41;](../../t-sql/functions/identity-transact-sql.md)  
  
  
