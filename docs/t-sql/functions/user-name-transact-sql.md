---
description: USER_NAME (Transact-SQL)
title: USER_NAME (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- USER_NAME
- USER_NAME_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- usernames [SQL Server]
- IDs [SQL Server], databases
- USER_NAME function
- users [SQL Server], database username
- names [SQL Server], database users
- identification numbers [SQL Server], databases
- database usernames [SQL Server]
ms.assetid: ab32d644-4228-449a-9ef0-5a975c305775
author: VanMSFT
ms.author: vanto
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 0ba6cb306d78a8c03e95c2d8e235ede1cf8126d7
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99210670"
---
# <a name="user_name-transact-sql"></a>USER_NAME (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Renvoie le nom d'utilisateur de base de données à partir du numéro d'identification spécifié.  
  
 ![Icône Lien d’article](../../database-engine/configure-windows/media/topic-link.gif "Icône Lien d’article") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql  
USER_NAME ( [ id ] )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments
 *id*  
 Numéro d'identification associé à un utilisateur de base de données. *id* est de type **int**. Les parenthèses sont obligatoires.  
  
## <a name="return-types"></a>Types de retour  
 **nvarchar(128)**  
  
## <a name="remarks"></a>Remarques  
 Si *id* est omis, l’utilisateur actuel dans le contexte actuel est pris en compte. Si le paramètre contient le mot NULL, retourne NULL. Lorsque USER_NAME est appelé sans spécifier un *id* après une instruction EXECUTE AS, USER_NAME renvoie le nom de l’utilisateur impersonné. Si un principal Windows accède à la base de données par l'intermédiaire de son appartenance à un groupe, USER_NAME renvoie le nom du principal Windows à la place du groupe.  
 
> [!NOTE]
> Bien que la fonction USER_NAME soit prise en charge sur Azure SQL Database, l’utilisation d’*Execute As* avec USER_NAME n’est pas prise en charge sur Azure SQL Database. 
  
## <a name="examples"></a>Exemples  
  
### <a name="a-using-user_name"></a>R. Utilisation de USER_NAME  
 L'exemple suivant retourne le nom de l'utilisateur ayant l'ID `13`.  
  
```sql  
SELECT USER_NAME(13);  
GO  
```  
  
### <a name="b-using-user_name-without-an-id"></a>B. Utilisation de USER_NAME sans ID  
 Le code exemple suivant recherche le nom de l'utilisateur actuel sans spécifier un ID.  
  
```sql  
SELECT USER_NAME();  
GO  
```  
  
 Jeu de résultats (pour un utilisateur membre du rôle serveur fixe sysadmin) :  
  
 ```
------------------------------  
dbo  
  
(1 row(s) affected)
```  
  
### <a name="c-using-user_name-in-the-where-clause"></a>C. Utilisation de USER_NAME dans la clause WHERE  
 Le code exemple suivant recherche dans `sysusers` la ligne dans laquelle le nom est égal au résultat de l'application de la fonction système `USER_NAME` à l'utilisateur identifié par le numéro `1`.  
  
```sql  
SELECT name FROM sysusers WHERE name = USER_NAME(1);  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 ```
name  
------------------------------  
dbo  
  
(1 row(s) affected)
```  
  
### <a name="d-calling-user_name-during-impersonation-with-execute-as"></a>D. Appel de USER_NAME pendant un emprunt d'identité avec EXECUTE AS  
 Le code exemple suivant illustre le comportement de `USER_NAME` pendant l'emprunt d'identité.  
  
```sql  
SELECT USER_NAME();  
GO  
EXECUTE AS USER = 'Zelig';  
GO  
SELECT USER_NAME();  
GO  
REVERT;  
GO  
SELECT USER_NAME();  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 ```
DBO  
Zelig  
DBO
```  
  
## <a name="examples-sssdwfull-and-sspdw"></a>Exemples : [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] et [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="e-using-user_name-without-an-id"></a>E. Utilisation de USER_NAME sans ID  
 Le code exemple suivant recherche le nom de l'utilisateur actuel sans spécifier un ID.  
  
```sql  
SELECT USER_NAME();  
```  
  
 Voici le jeu de résultats pour un utilisateur actuellement connecté.  
  
```  
------------------------------   
User7                              
```  
  
### <a name="f-using-user_name-in-the-where-clause"></a>F. Utilisation de USER_NAME dans la clause WHERE  
 Le code exemple suivant recherche dans `sysusers` la ligne dans laquelle le nom est égal au résultat de l'application de la fonction système `USER_NAME` à l'utilisateur identifié par le numéro `1`.  
  
```sql  
SELECT name FROM sysusers WHERE name = USER_NAME(1);  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
name                             
------------------------------   
User7                              
```  
  
## <a name="see-also"></a>Voir aussi  
 [ALTER TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-table-transact-sql.md)   
 [CREATE TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/create-table-transact-sql.md)   
 [CURRENT_TIMESTAMP &#40;Transact-SQL&#41;](../../t-sql/functions/current-timestamp-transact-sql.md)   
 [CURRENT_USER &#40;Transact-SQL&#41;](../../t-sql/functions/current-user-transact-sql.md)   
 [SESSION_USER &#40;Transact-SQL&#41;](../../t-sql/functions/session-user-transact-sql.md)   
 [Fonctions système &#40;Transact-SQL&#41;](../../relational-databases/system-functions/system-functions-category-transact-sql.md)   
 [SYSTEM_USER &#40;Transact-SQL&#41;](../../t-sql/functions/system-user-transact-sql.md)  
  
