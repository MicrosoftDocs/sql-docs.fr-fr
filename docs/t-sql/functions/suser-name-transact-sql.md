---
description: SUSER_NAME (Transact-SQL)
title: SUSER_NAME (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 12/12/2020
ms.prod: sql
ms.prod_service: sql-data-warehouse, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- SUSER_NAME
- SUSER_NAME_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- security identification names [SQL Server]
- logins [SQL Server], users
- identification names for logins [SQL Server]
- users [SQL Server], logins
- SUSER_NAME function
- logins [SQL Server], names
- names [SQL Server], logins
ms.assetid: ae598d9f-9baa-49b8-b1c1-042854206de4
author: VanMSFT
ms.author: vanto
monikerRange: =azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: b163b8aae38c389b8b20a6853fce685cac0dfd8e
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99200279"
---
# <a name="suser_name-transact-sql"></a>SUSER_NAME (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

Retourne le nom d'identification de l'utilisateur pour la connexion.  
  
![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql
SUSER_NAME ( [ server_user_id ] )   
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments
_server\_user\_id_  
Correspond au numéro d'identification de la connexion de l'utilisateur. _server\_user\_id_, facultatif, est de type **int**. _server\_user\_id_ peut être le numéro d’identification d’une connexion [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou d’un utilisateur ou d’un groupe Windows [!INCLUDE[msCoName](../../includes/msconame-md.md)] quelconque qui a l’autorisation de se connecter à une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Si _server\_user\_id_ n’est pas spécifié, le nom d’identification de connexion de l’utilisateur actuel est renvoyé. Si le paramètre contient le mot NULL, il retourne NULL.  
  
## <a name="return-types"></a>Types de retour  
**nvarchar(128)**  
  
## <a name="remarks"></a>Remarques  
Dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 7.0, le numéro d'identification de sécurité (SID, Security Identification Number) remplace le numéro d'identification de l'utilisateur du serveur (SUID, Server User Identification Number).  
  
La fonction SUSER_NAME renvoie un nom de connexion seulement pour une connexion comportant une entrée dans la table système **syslogins**.  
  
SUSER_NAME peut être utilisé dans une liste de sélection, dans une clause WHERE et partout où une expression est autorisée. Utilisez des parenthèses après SUSER_NAME, même si aucun paramètre n’est spécifié.  

> [!NOTE]
> Bien que la fonction SUSER_NAME soit prise en charge sur Azure SQL Database, l’utilisation d’*Execute As* avec SUSER_NAME n’est pas prise en charge sur Azure SQL Database. 
  
## <a name="examples"></a>Exemples  
Dans l'exemple suivant, la procédure retourne le nom d'identification de la connexion utilisateur `1`.  
  
```sql
SELECT SUSER_NAME(1);  
```  
  
## <a name="see-also"></a>Voir aussi  
[SUSER_ID &#40;Transact-SQL&#41;](../../t-sql/functions/suser-id-transact-sql.md)   
[Principaux &#40;moteur de base de données&#41;](../../relational-databases/security/authentication-access/principals-database-engine.md)  
  
  
