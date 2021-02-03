---
description: CREATE REMOTE SERVICE BINDING (Transact-SQL)
title: CREATE REMOTE SERVICE BINDING (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- CREATE REMOTE SERVICE BINDING
- SERVICE_BINDING_TSQL
- CREATE REMOTE SERVICE
- REMOTE_TSQL
- CREATE_REMOTE_SERVICE_BINDING_TSQL
- CREATE_REMOTE_SERVICE_TSQL
- BINDING
- SERVICE BINDING
- BINDING_TSQL
- CREATE_REMOTE_TSQL
- REMOTE_SERVICE_TSQL
- CREATE REMOTE
- REMOTE SERVICE
- REMOTE_SERVICE_BINDING_TSQL
- REMOTE
- REMOTE SERVICE BINDING
dev_langs:
- TSQL
helpviewer_keywords:
- binding remote service [Service Broker]
- dialog security [Service Broker]
- end-to-end security [Service Broker]
- security [Service Broker], remote service bindings
- CREATE REMOTE SERVICE BINDING statement
- conversation security [Service Broker]
- remote service bindings [Service Broker], creating
ms.assetid: 4165c404-4d50-4063-9a6e-6e267d309376
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 13abf5a16b800bc856d482fbc525a833b9d7e872
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99188515"
---
# <a name="create-remote-service-binding-transact-sql"></a>CREATE REMOTE SERVICE BINDING (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Crée une liaison qui définit les informations d'identification de sécurité à utiliser pour démarrer une conversation avec un service distant.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql
CREATE REMOTE SERVICE BINDING binding_name   
   [ AUTHORIZATION owner_name ]   
   TO SERVICE 'service_name'   
   WITH  USER = user_name [ , ANONYMOUS = { ON | OFF } ]  
[ ; ]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments
 *binding_name*  
 Nom de la liaison de service distant à créer. Les noms du serveur, de la base de données et du schéma ne peuvent pas être spécifiés. *binding_name* doit être un **sysname** valide.  
  
 AUTHORIZATION *owner_name*  
 Affecte au propriétaire de la liaison le rôle ou l'utilisateur de base de données spécifié. Quand l’utilisateur actuel est **dbo** ou **sa**, l’argument *owner_name* peut être le nom de n’importe quel utilisateur ou rôle valide. Sinon, *owner_name* doit être le nom de l’utilisateur actuel, le nom d’un utilisateur pour lequel l’utilisateur actuel possède des autorisations IMPERSONATE ou le nom d’un rôle auquel appartient l’utilisateur actuel.  
  
 TO SERVICE '*service_name*'  
 Spécifie le service distant à lier à l'utilisateur identifié dans la clause WITH USER.  
  
 USER = *user_name*  
 Spécifie le principal de la base de données qui possède le certificat associé au service distant identifié par la clause TO SERVICE. Ce certificat est utilisé pour le chiffrement et l'authentification des messages échangés avec le service distant.  
  
 ANONYMOUS  
 Spécifie si l'authentification anonyme est utilisée lors des communications avec le service distant. Si ANONYMOUS = ON, l’authentification anonyme est utilisée et les opérations dans la base de données distante sont effectuées en tant que membre du rôle de base de données fixe **public**. Si ANONYMOUS = OFF, les opérations de la base de données distante sont exécutées en tant qu'utilisateur spécifique dans cette base de données. Lorsque cette clause est omise, la valeur par défaut est OFF.  
  
## <a name="remarks"></a>Notes  
 [!INCLUDE[ssSB](../../includes/sssb-md.md)] utilise une liaison de service distant afin de localiser le certificat à utiliser pour une nouvelle conversation. La clé publique du certificat associé à *user_name* est utilisée pour authentifier les messages envoyés au service distant et pour chiffrer une clé de session qui sert ensuite à chiffrer la conversation. Le certificat de *user_name* doit correspondre au certificat d’un utilisateur de la base de données qui héberge le service distant.  
  
 Une liaison de service distant n'est nécessaire que pour démarrer des services qui communiquent avec des services cibles en dehors de l'instance [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Une base de données qui héberge un service d'initialisation doit contenir des liaisons de service distant pour tous les services cibles en dehors de l'instance [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Une base de données qui héberge un service cible n'a pas besoin de contenir des liaisons de service distant pour les services d'initialisation qui communiquent avec le service cible. Lorsque les services initiateur et cible sont dans la même instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], aucune liaison de service distant n'est nécessaire. Toutefois, si une liaison de service distant est présente à l’endroit où le *service_name* spécifié pour TO SERVICE correspond au nom du service local, [!INCLUDE[ssSB](../../includes/sssb-md.md)] utilise cette liaison.  
  
 Si ANONYMOUS = ON, le service initiateur se connecte au service cible comme membre du rôle de base de données fixe **public**. Par défaut, les membres de ce rôle n'ont pas l'autorisation de se connecter à une base de données. Pour qu’un message puisse être envoyé, la base de données cible doit accorder l’autorisation CONNECT du rôle **public** pour la base de données et l’autorisation SEND pour le service cible.  
  
 Lorsqu'un utilisateur possède plusieurs certificats actuellement valides et marqués comme étant AVAILABLE FOR BEGIN_DIALOG, [!INCLUDE[ssSB](../../includes/sssb-md.md)] sélectionne celui qui est doté de la date d'expiration la plus lointaine.  
  
## <a name="permissions"></a>Autorisations  
 Les autorisations de création d’une liaison de service distant sont octroyées par défaut à l’utilisateur spécifié dans la clause USER, aux membres du rôle de base de données fixe **db_owner**, aux membres du rôle de base de données fixe **db_ddladmin** et aux membres du rôle serveur fixe **sysadmin**.  
  
 L'utilisateur qui exécute l'instruction CREATE REMOTE SERVICE BINDING doit avoir l'autorisation d'emprunt d'identité pour le principal spécifié dans l'instruction.  
  
 Une liaison de service distant ne peut pas être un objet temporaire. Les noms de liaison de service distant commençant par **#** sont autorisés, mais ce sont des objets permanents.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-creating-a-remote-service-binding"></a>R. Création d’une liaison de service distant  
 L'exemple suivant crée une liaison pour le service `//Adventure-Works.com/services/AccountsPayable`. [!INCLUDE[ssSB](../../includes/sssb-md.md)] utilise le certificat appartenant au principal de base de données `APUser` pour s'authentifier auprès du service distant et échanger la clé de chiffrement de la session avec le service distant.  
  
```sql  
CREATE REMOTE SERVICE BINDING APBinding  
    TO SERVICE '//Adventure-Works.com/services/AccountsPayable'  
    WITH USER = APUser ;  
```  
  
### <a name="b-creating-a-remote-service-binding-using-anonymous-authentication"></a>B. Création d'une liaison de service distant avec une authentification anonyme  
 L'exemple suivant crée une liaison pour le service `//Adventure-Works.com/services/AccountsPayable`. [!INCLUDE[ssSB](../../includes/sssb-md.md)] utilise le certificat appartenant au principal de base de données `APUser` pour échanger la clé de chiffrement de la session avec le service distant. Il ne s'authentifie pas auprès du service distant. Dans la base de données qui héberge le service distant, les messages sont remis en tant qu’utilisateur **guest**.  
  
```sql  
CREATE REMOTE SERVICE BINDING APBinding  
    TO SERVICE '//Adventure-Works.com/services/AccountsPayable'  
    WITH USER = APUser, ANONYMOUS=ON ;  
```  
  
## <a name="see-also"></a>Voir aussi  
 [ALTER REMOTE SERVICE BINDING &#40;Transact-SQL&#41;](../../t-sql/statements/alter-remote-service-binding-transact-sql.md)   
 [DROP REMOTE SERVICE BINDING &#40;Transact-SQL&#41;](../../t-sql/statements/drop-remote-service-binding-transact-sql.md)   
 [EVENTDATA &#40;Transact-SQL&#41;](../../t-sql/functions/eventdata-transact-sql.md)  
  
  
