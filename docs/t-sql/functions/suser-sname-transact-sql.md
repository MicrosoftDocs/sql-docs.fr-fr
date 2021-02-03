---
description: SUSER_SNAME (Transact-SQL)
title: SUSER_SNAME (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/29/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- SUSER_SNAME_TSQL
- SUSER_SNAME
dev_langs:
- TSQL
helpviewer_keywords:
- security identification names [SQL Server]
- logins [SQL Server], users
- SIDs [SQL Server]
- SUSER_SNAME function
- users [SQL Server], logins
- logins [SQL Server], names
- IDs [SQL Server], logins
- identification numbers [SQL Server], logins
- names [SQL Server], logins
ms.assetid: 11ec7d86-d429-4004-a436-da25df9f8761
author: julieMSFT
ms.author: jrasnick
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: aa7f09133dcdb23ae3e6f2d310b745d65ca49a86
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99206293"
---
# <a name="suser_sname-transact-sql"></a>SUSER_SNAME (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Retourne le nom de connexion associé à un numéro d'identification de sécurité (SID).  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql
SUSER_SNAME ( [ server_user_sid ] )   
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments
 *server_user_sid*  
**S’applique à** : [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] et ultérieur
  
 Correspond au numéro facultatif d'identification de sécurité de la connexion. *server_user_sid* est de type **varbinary(85)**. *server_user_sid* peut être le numéro d’identification de sécurité de n’importe quelle connexion [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou de n’importe quel utilisateur ou groupe [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows. Si *server_user_sid* n’est pas spécifié, les informations sur l’utilisateur actuel sont renvoyées. Si le paramètre contient le mot NULL, retourne NULL.  
  
## <a name="return-types"></a>Types de retour  
 **nvarchar(128)**  
  
## <a name="remarks"></a>Remarques  
 La fonction SUSER_SNAME peut être utilisée comme une contrainte DEFAULT dans les fonctions ALTER TABLE ou CREATE TABLE. SUSER_SNAME peut être utilisé dans une liste de sélection, dans une clause WHERE, et dans tous les cas où une expression est autorisée. SUSER_SNAME doit toujours être suivi de parenthèses, même si aucun paramètre n'est spécifié.  
  
 Lorsqu'il est appelé sans argument, SUSER_SNAME retourne le nom du contexte de sécurité actuel. Lorsqu'il est appelé sans argument à l'intérieur d'un traitement qui a changé de contexte en utilisant EXECUTE AS, SUSER_SNAME retourne le nom du contexte qui a fait l'objet d'un emprunt d'identité. Lorsqu'il est appelé à partir d'un contexte faisant l'objet d'un emprunt d'identité, ORIGINAL_LOGIN retourne le nom du contexte d'origine.  
  
## <a name="sssdsfull-remarks"></a>[!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] Notes  
 SUSER_NAME retourne toujours le nom de connexion pour le contexte de sécurité actuel.  
  
 L'instruction SUSER_SNAME ne prend pas en charge l'exécution en utilisant un contexte de sécurité représenté par la clause EXECUTE AS.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-using-suser_sname"></a>R. Utilisation de SUSER_SNAME  
 L'exemple suivant retourne le nom de connexion pour le contexte de sécurité actuel.  
  
```sql
SELECT SUSER_SNAME();  
GO  
```  
  
### <a name="b-using-suser_sname-with-a-windows-user-security-id"></a>B. Utilisation de SUSER_SNAME avec un ID de sécurité utilisateur Windows  
 Cet exemple retourne le nom de connexion associé à un numéro d'identification de sécurité Windows.  
  
**S’applique à** : [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] et ultérieur
  
```sql
SELECT SUSER_SNAME(0x010500000000000515000000a065cf7e784b9b5fe77c87705a2e0000);  
GO  
```  
  
### <a name="c-using-suser_sname-as-a-default-constraint"></a>C. Utilisation de SUSER_SNAME comme contrainte DEFAULT  
 L'exemple suivant utilise `SUSER_SNAME` comme contrainte `DEFAULT` dans une instruction `CREATE TABLE`.  
  
```sql
USE AdventureWorks2012;  
GO  
CREATE TABLE sname_example  
(  
login_sname sysname DEFAULT SUSER_SNAME(),  
employee_id uniqueidentifier DEFAULT NEWID(),  
login_date  datetime DEFAULT GETDATE()  
);   
GO  
INSERT sname_example DEFAULT VALUES;  
GO  
```  
  
### <a name="d-calling-suser_sname-in-combination-with-execute-as"></a>D. Appel de SUSER_SNAME en combinaison avec EXECUTE AS  
 Cet exemple montre le comportement de SUSER_SNAME lorsqu'il est appelé à partir d'un contexte ayant fait l'objet d'un emprunt d'identité.  
  
**S’applique à** : [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] et ultérieur
  
```sql
SELECT SUSER_SNAME();  
GO  
EXECUTE AS LOGIN = 'WanidaBenShoof';  
SELECT SUSER_SNAME();  
REVERT;  
GO  
SELECT SUSER_SNAME();  
GO 
```  
  
 Voici le résultat.  
  
 ```
sa  
WanidaBenShoof  
sa
```  
  
## <a name="examples-sssdwfull-and-sspdw"></a>Exemples : [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] et [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="e-using-suser_sname"></a>E. Utilisation de SUSER_SNAME  
 Cet exemple retourne le nom de connexion correspondant au numéro d'identification de sécurité portant la valeur `0x01`.  
  
```sql
SELECT SUSER_SNAME(0x01);  
GO  
```  
  
### <a name="f-returning-the-current-login"></a>F. Renvoi de la connexion actuelle  
 L’exemple suivant renvoie le nom de connexion de la connexion actuelle.  
  
```sql
SELECT SUSER_SNAME() AS CurrentLogin;  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [SUSER_SID &#40;Transact-SQL&#41;](../../t-sql/functions/suser-sid-transact-sql.md)   
 [Principaux &#40;moteur de base de données&#41;](../../relational-databases/security/authentication-access/principals-database-engine.md)   
 [sys.server_principals &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-server-principals-transact-sql.md)   
 [EXECUTE AS &#40;Transact-SQL&#41;](../../t-sql/statements/execute-as-transact-sql.md)  
  
  

