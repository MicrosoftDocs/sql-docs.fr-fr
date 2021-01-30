---
description: sp_change_users_login (Transact-SQL)
title: sp_change_users_login (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 12/13/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_change_users_login
- sp_change_users_login_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_change_users_login
ms.assetid: 1554b39f-274b-4ef8-898e-9e246b474333
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 109b2a6aa015f8f66a327714264aa986060f9a0c
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99203718"
---
# <a name="sp_change_users_login-transact-sql"></a>sp_change_users_login (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Mappe un utilisateur de base de données existant à un compte de connexion [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. 
  
 > [!IMPORTANT]
 > [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)] Utilisez à la place [ALTER USER](../../t-sql/statements/alter-user-transact-sql.md) .  
  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_change_users_login [ @Action = ] 'action'   
    [ , [ @UserNamePattern = ] 'user' ]   
    [ , [ @LoginName = ] 'login' ]   
    [ , [ @Password = ] 'password' ]  
[;]  
```  
  
## <a name="arguments"></a>Arguments  
 [ @Action =] '*action*'  
 Décrit l'action à effectuer par la procédure. *action* est **de type varchar (10)**. l' *action* peut prendre l’une des valeurs suivantes.  
  
|Valeur|Description|  
|-----------|-----------------|  
|**Auto_Fix**|Lie une entrée d'utilisateur de l'affichage catalogue système sys.database_principals de la base de données active à une connexion [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] du même nom. Le compte de connexion de même nom est créé s'il n'existe pas déjà. Examinez le résultat de l’instruction **Auto_Fix** pour confirmer que le lien correct est effectivement effectué. Évitez d’utiliser des **Auto_Fix** dans des situations de sécurité.<br /><br /> Lorsque vous utilisez **Auto_Fix**, vous devez spécifier l' *utilisateur* et le *mot de passe* si la connexion n’existe pas déjà, sinon vous devez spécifier l' *utilisateur* , mais le *mot de passe* sera ignoré. la *connexion* doit avoir la valeur null. l' *utilisateur* doit être un utilisateur valide dans la base de données actuelle. Le compte de connexion ne doit être associé à aucun autre utilisateur.|  
|**Report**|Dresse la liste des utilisateurs qui ne sont pas liés à un compte de connexion dans la base de données active et indique les identificateurs de sécurité (SID) correspondants. l' *utilisateur*, la *connexion* et le *mot de passe* doivent avoir la valeur null ou ne pas être spécifiés.<br /><br /> Pour remplacer l’option de rapport par une requête qui utilise les tables système, comparez les entrées dans **sys.server_prinicpals** avec les entrées dans **sys.database_principals**.|  
|**Update_One**|Lie l' *utilisateur* spécifié dans la base de données active à une [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] *connexion* existante. l' *utilisateur* et la *connexion* doivent être spécifiés. le *mot de passe* doit avoir la valeur null ou n’est pas spécifié.|  
  
 [ @UserNamePattern =] '*utilisateur*'  
 Nom d'un utilisateur dans la base de données active. *User* est de **type sysname**, avec NULL comme valeur par défaut.  
  
 [ @LoginName =] «*connexion*»  
 Est le nom d'un compte de connexion [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. *login* est de type **sysname**, avec NULL comme valeur par défaut.  
  
 [ @Password =] '*mot_de_passe*'  
 Mot de passe affecté à une nouvelle [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] connexion créée en spécifiant **Auto_Fix**. Si une connexion correspondante existe déjà, l’utilisateur et la connexion sont mappés et le *mot de passe* est ignoré. S’il n’existe pas de connexion correspondante, sp_change_users_login crée une nouvelle [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] connexion et attribue le *mot de* passe à la nouvelle connexion. *Password est de* **type sysname** et ne doit pas avoir la valeur null.  
  
> **IMPORTANT** Utilisez toujours un [mot de passe fort !](../../relational-databases/security/strong-passwords.md)
  
## <a name="return-code-values"></a>Codet de retour  
 0 (réussite) ou 1 (échec)  
  
## <a name="result-sets"></a>Jeux de résultats  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|UserName|**sysname**|Nom de l'utilisateur de la base de données.|  
|UserSID|**varbinary(85)**|Identificateur de sécurité de l'utilisateur.|  
  
## <a name="remarks"></a>Notes  
 Utilisez sp_change_users_login pour lier un utilisateur de la base de données active à une connexion [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Si le compte de connexion d'un utilisateur a changé, utilisez sp_change_users_login pour lier l'utilisateur au nouveau compte de connexion sans perdre les autorisations de l'utilisateur. La nouvelle *connexion* ne peut pas être sa et l' *utilisateur* ne peut pas être dbo, guest ou un utilisateur INFORMATION_SCHEMA.  
  
 La procédure sp_change_users_login ne peut pas être utilisée pour mapper des utilisateurs de base de données sur des principaux, des certificats ou des clés asymétriques de niveau Windows.  
  
 sp_change_users_login ne peut pas être utilisé avec une connexion [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] créée à partir d'un principal Windows, ni avec un utilisateur créé à l'aide de CREATE USER WITHOUT LOGIN.  
  
 La procédure sp_change_users_login ne peut pas être exécutée dans une transaction définie par l'utilisateur.  
  
## <a name="permissions"></a>Autorisations  
 Nécessite l'appartenance au rôle de base de données fixe db_owner. Seuls les membres du rôle serveur fixe sysadmin peuvent spécifier l’option **Auto_Fix** .  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-showing-a-report-of-the-current-user-to-login-mappings"></a>R. Affichage d'un rapport des mappages utilisateur-connexion en cours  
 Cet exemple produit un rapport des utilisateurs définis dans la base de données active et de leurs identificateurs de sécurité (SID).  
  
```  
EXEC sp_change_users_login 'Report';  
```  
  
### <a name="b-mapping-a-database-user-to-a-new-sql-server-login"></a>B. Mappage d'un utilisateur de la base de données à une nouvelle connexion SQL Server  
 Dans l'exemple suivant, un utilisateur de la base de données est associé à un nouveau compte de connexion [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. L'utilisateur `MB-Sales`, associé au départ à un autre compte de connexion, est mappé sur le nouveau compte `MaryB`.  
  
```  
--Create the new login.  
CREATE LOGIN MaryB WITH PASSWORD = '982734snfdHHkjj3';  
GO  
--Map database user MB-Sales to login MaryB.  
USE AdventureWorks2012;  
GO  
EXEC sp_change_users_login 'Update_One', 'MB-Sales', 'MaryB';  
GO  
```  
  
### <a name="c-automatically-mapping-a-user-to-a-login-creating-a-new-login-if-it-is-required"></a>C. Mappage automatique d'un utilisateur à une connexion en créant une nouvelle connexion si nécessaire  
 L’exemple suivant montre comment utiliser `Auto_Fix` pour mapper un utilisateur existant à un compte de connexion du même nom, ou pour créer la [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] connexion `Mary` avec le mot de passe `B3r12-3x$098f6` si la connexion `Mary` n’existe pas.  
  
```  
USE AdventureWorks2012;  
GO  
EXEC sp_change_users_login 'Auto_Fix', 'Mary', NULL, 'B3r12-3x$098f6';  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Procédures stockées de sécurité &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/security-stored-procedures-transact-sql.md)   
 [CREATE LOGIN &#40;Transact-SQL&#41;](../../t-sql/statements/create-login-transact-sql.md)   
 [sp_adduser &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-adduser-transact-sql.md)   
 [sp_helplogins &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helplogins-transact-sql.md)   
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)   
 [sys.database_principals &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-database-principals-transact-sql.md)  
  
  
