---
description: USE (Transact-SQL)
title: USE (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 11/28/2016
ms.prod: sql
ms.prod_service: pdw, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- USE_TSQL
- USE
dev_langs:
- TSQL
helpviewer_keywords:
- USE statement
- database context [SQL Server]
- context changes [SQL Server]
- modifying database context
ms.assetid: c05acac8-c063-4770-8e36-d7f71d500b10
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 5d83799db882df09ae57e7225d8c3f52cdb04cb8
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99208761"
---
# <a name="use-transact-sql"></a>USE (Transact-SQL)
[!INCLUDE [sql-asdbmi-pdw](../../includes/applies-to-version/sql-asdbmi-pdw.md)]

  Remplace le contexte de la base de données par la base de données spécifiée ou par l'instantané de la base de données spécifié dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 ![Icône du lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql
USE { database_name }   
[;]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments
 *database_name*  
 Nom de la base de données ou de l'instantané de la base de données vers lequel le contexte de l'utilisateur bascule. Les noms de la base de données et de l’instantané de la base de données doivent se conformer aux règles en vigueur pour les [identificateurs](../../relational-databases/databases/database-identifiers.md).  
  
 Dans [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)], le paramètre database ne peut faire référence qu'à la base de données active. Si une base de données autre que la base de données active est fournie, l’instruction `USE` ne permet pas de basculer entre les bases de données et le code d’erreur 40508 est retourné. Pour changer de bases de données, vous devez vous connecter directement à la base de données. L’instruction USE est marquée comme non applicable à SQL Database en haut de cette page, car même si vous pouvez inclure l’instruction `USE` dans un lot, elle n’a aucun effet.
  
## <a name="remarks"></a>Remarques  
 Lorsqu'un nom d'ouverture de session [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] se connecte à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], il est automatiquement connecté à sa base de données par défaut et acquiert le contexte de sécurité d'un utilisateur de base de données. Si aucun utilisateur de base de données n'a été créé pour le nom d'ouverture de session [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], celui-ci se connecte en tant que guest (invité). Si l'utilisateur de base de données ne bénéficie pas de l'autorisation CONNECT sur la base de données, l'instruction USE échoue. Si aucune base de données par défaut n'a été affectée au nom d'ouverture de session, sa base de données par défaut est définie sur master.  
  
 USE est exécutée à la fois au moment de l'exécution et de la compilation, et prend effet immédiatement. C'est pourquoi les instructions apparaissant dans un traitement après l'exécution de USE sont exécutées dans la base de données spécifiée.  
  
## <a name="permissions"></a>Autorisations  
 Exige l'autorisation CONNECT sur la base de données cible.  
  
## <a name="examples"></a>Exemples  
 L'exemple qui suit remplace le contexte de la base de données par la base de données `AdventureWorks2012`.  
  
```sql  
USE AdventureWorks2012;  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [CREATE LOGIN &#40;Transact-SQL&#41;](../../t-sql/statements/create-login-transact-sql.md)   
 [CREATE USER &#40;Transact-SQL&#41;](../../t-sql/statements/create-user-transact-sql.md)   
 [Principaux &#40;moteur de base de données&#41;](../../relational-databases/security/authentication-access/principals-database-engine.md)   
 [CREATE DATABASE &#40;SQL Server Transact-SQL&#41;](../statements/create-database-transact-sql.md)   
 [DROP DATABASE &#40;Transact-SQL&#41;](../../t-sql/statements/drop-database-transact-sql.md)   
 [EXECUTE &#40;Transact-SQL&#41;](../../t-sql/language-elements/execute-transact-sql.md)  
  
