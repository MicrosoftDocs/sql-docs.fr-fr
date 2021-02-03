---
description: USER (Transact-SQL)
title: USER (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- USER
- USER_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- usernames [SQL Server]
- system-supplied usernames [SQL Server]
- USER function
- users [SQL Server], database names
- names [SQL Server], database users
- database usernames [SQL Server]
ms.assetid: 82bbbd94-870c-4c43-9ed9-d9abc767a6be
author: julieMSFT
ms.author: jrasnick
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: e81222292aa0bae66b96f5864d2ac7957f9c7742
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99210650"
---
# <a name="user-transact-sql"></a>USER (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Permet d'insérer dans une table une valeur système si aucune valeur par défaut n'a été spécifiée pour le nom de l'utilisateur actuel.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql  
USER  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="return-types"></a>Types de retour
 **nvarchar(128)**  
  
## <a name="remarks"></a>Remarques  
 Cette instruction offre la même fonctionnalité que la fonction système USER_NAME.  
  
 Utilisez la fonction USER avec les contraintes DEFAULT dans les instructions CREATE TABLE ou ALTER TABLE, ou utilisez-la comme une fonction standard.  
  
 USER retourne toujours le nom du contexte en cours. Lorsqu'elle est appelée après une instruction EXECUTE AS, USER retourne le nom du contexte représenté.  
  
 Si un principal Windows accède à la base de données du fait de son appartenance à un groupe, USER retourne le nom du principal Windows au lieu du nom du groupe.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-using-user-to-return-the-database-user-name"></a>R. Utilisation de USER pour retourner le nom d'utilisateur de la base de données  
 L'exemple suivant déclare une variable `char`, lui attribue la valeur actuelle de USER et l'imprime avec une description de texte.  
  
```sql
DECLARE @usr CHAR(30)  
SET @usr = user  
SELECT 'The current user''s database username is: '+ @usr  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 ```
-----------------------------------------------------------------------  
The current user's database username is: dbo  
  
(1 row(s) affected)
```  
  
### <a name="b-using-user-with-default-constraints"></a>B. Utilisation de USER avec les contraintes DEFAULT  
 L'exemple suivant crée une table faisant appel à `USER` en tant que contrainte `DEFAULT` pour le vendeur d'une ligne ventes.  
  
```sql
USE AdventureWorks2012;  
GO  
CREATE TABLE inventory22  
(  
 part_id INT IDENTITY(100, 1) NOT NULL,  
 description VARCHAR(30) NOT NULL,  
 entry_person VARCHAR(30) NOT NULL DEFAULT USER   
)  
GO  
INSERT inventory22 (description)  
VALUES ('Red pencil')  
INSERT inventory22 (description)  
VALUES ('Blue pencil')  
INSERT inventory22 (description)  
VALUES ('Green pencil')  
INSERT inventory22 (description)  
VALUES ('Black pencil')  
INSERT inventory22 (description)  
VALUES ('Yellow pencil')  
GO  
```  
  
 Voici la requête permettant de sélectionner toutes les informations de la table `inventory22` :  
  
```sql
SELECT * FROM inventory22 ORDER BY part_id;  
GO  
```  
  
 Voici le jeu de résultats (remarquez la valeur `entry-person`) :  
  
 ```
part_id     description                    entry_person
----------- ------------------------------ -------------------------
100         Red pencil                     dbo
101         Blue pencil                    dbo
102         Green pencil                   dbo
103         Black pencil                   dbo
104         Yellow pencil                  dbo
  
(5 row(s) affected)
```  
  
### <a name="c-using-user-in-combination-with-execute-as"></a>C. Utilisation de USER avec EXECUTE AS  
 L'exemple suivant montre le comportement de `USER` quand cette instruction est appelée à l'intérieur d'une session représentée.  
  
```sql
SELECT USER;  
GO  
EXECUTE AS USER = 'Mario';  
GO  
SELECT USER;  
GO  
REVERT;  
GO  
SELECT USER;  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 ```
DBO
Mario
DBO
```  
  
## <a name="see-also"></a>Voir aussi  
 [ALTER TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-table-transact-sql.md)   
 [CREATE TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/create-table-transact-sql.md)   
 [CURRENT_TIMESTAMP &#40;Transact-SQL&#41;](../../t-sql/functions/current-timestamp-transact-sql.md)   
 [CURRENT_USER &#40;Transact-SQL&#41;](../../t-sql/functions/current-user-transact-sql.md)   
 [Fonctions de sécurité &#40;Transact-SQL&#41;](../../t-sql/functions/security-functions-transact-sql.md)   
 [SESSION_USER &#40;Transact-SQL&#41;](../../t-sql/functions/session-user-transact-sql.md)   
 [SYSTEM_USER &#40;Transact-SQL&#41;](../../t-sql/functions/system-user-transact-sql.md)   
 [USER_NAME &#40;Transact-SQL&#41;](../../t-sql/functions/user-name-transact-sql.md)  
  
  

