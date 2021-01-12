---
description: ALTER REMOTE SERVICE BINDING (Transact-SQL)
title: ALTER REMOTE SERVICE BINDING (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- ALTER REMOTE SERVICE BINDING
- ALTER_REMOTE_SERVICE_BINDING_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- remote service bindings [Service Broker], modifying
- ALTER REMOTE SERVICE BINDING statement
- modifying remote service bindings
ms.assetid: ee620b4a-9375-4eaa-a016-69916c9e1e68
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: c671b412d5d9ce34c34fdc49e16215dafc8ec83e
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/11/2021
ms.locfileid: "98092110"
---
# <a name="alter-remote-service-binding-transact-sql"></a>ALTER REMOTE SERVICE BINDING (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Modifie l'utilisateur associé à des liaisons de service distant ou modifie le paramètre d'authentification anonyme pour ces liaisons.  
  
 ![Icône du lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql
ALTER REMOTE SERVICE BINDING binding_name   
   WITH [ USER = <user_name> ] [ , ANONYMOUS = { ON | OFF } ]   
[ ; ]  
```  
  

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments
 *binding_name*  
 Nom des liaisons de service distant à modifier. Les noms du serveur, de la base de données et du schéma ne peuvent pas être spécifiés.  
  
 WITH USER = \<*user_name>*  
 Spécifie l'utilisateur de base de données qui détient le certificat associé au service distant pour ces liaisons. La clé publique de ce certificat est utilisée pour le chiffrement et l'authentification des messages échangés avec le service distant.  
  
 ANONYMOUS  
 Spécifie si l'authentification anonyme est utilisée lors des communications avec le service distant. Si ANONYMOUS = ON, l'authentification anonyme est utilisée et les informations d'identification de l'utilisateur local ne sont pas transférées au service distant. Si ANONYMOUS = OFF (désactivé), les informations d'identification de l'utilisateur sont transférées. Lorsque cette clause est omise, la valeur par défaut est OFF.  
  
## <a name="remarks"></a>Notes  
 La clé publique du certificat associé à *user_name* est utilisée pour authentifier les messages envoyés au service distant et pour chiffrer une clé de session qui sert ensuite à chiffrer la conversation. Le certificat de *user_name* doit correspondre au certificat d’une connexion dans la base de données qui héberge le service distant.  
  
## <a name="permissions"></a>Autorisations  
 Les autorisations de modification des liaisons de service distant sont accordées par défaut au propriétaire de ces liaisons, aux membres du rôle de base de données fixe **db_owner** et aux membres du rôle serveur fixe **sysadmin**.  
  
 L'utilisateur qui exécute l'instruction ALTER REMOTE SERVICE BINDING doit disposer de l'autorisation d'emprunt d'identité pour l'utilisateur spécifié dans l'instruction.  
  
 Si vous voulez modifier l'option AUTHORIZATION pour des liaisons de service distant, utilisez l'instruction ALTER AUTHORIZATION.  
  
## <a name="examples"></a>Exemples  
 L'exemple suivant modifie les liaisons de service distant `APBinding` de façon à chiffrer les messages à l'aide des certificats du compte `SecurityAccount`.  
  
```sql  
ALTER REMOTE SERVICE BINDING APBinding  
    WITH USER = SecurityAccount ;  
```  
  
## <a name="see-also"></a>Voir aussi  
 [CREATE REMOTE SERVICE BINDING &#40;Transact-SQL&#41;](../../t-sql/statements/create-remote-service-binding-transact-sql.md)   
 [DROP REMOTE SERVICE BINDING &#40;Transact-SQL&#41;](../../t-sql/statements/drop-remote-service-binding-transact-sql.md)   
 [EVENTDATA &#40;Transact-SQL&#41;](../../t-sql/functions/eventdata-transact-sql.md)  
  
  
