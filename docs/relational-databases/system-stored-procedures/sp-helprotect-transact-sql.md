---
description: sp_helprotect (Transact-SQL)
title: sp_helprotect (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_helprotect
- sp_helprotect_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_helprotect
ms.assetid: faaa3e40-1c95-43c2-9fdc-c61a1d3cc0c3
author: markingmyname
ms.author: maghan
ms.openlocfilehash: f64ca2d34d11811bec87c1e4e378b424065299c4
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99211929"
---
# <a name="sp_helprotect-transact-sql"></a>sp_helprotect (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Retourne un rapport contenant des informations sur les autorisations utilisateur concernant un objet ou des autorisations d'instruction, dans la base de données active.  
  
> [!IMPORTANT]  
>  **sp_helprotect** ne retourne pas d’informations sur les éléments sécurisables introduits dans [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] . Utilisez à la place [sys.database_permissions](../../relational-databases/system-catalog-views/sys-database-permissions-transact-sql.md) et [fn_builtin_permissions](../../relational-databases/system-functions/sys-fn-builtin-permissions-transact-sql.md) .  
  
 Ne répertorie pas les autorisations qui sont toujours affectées aux rôles serveur fixes ou aux rôles de base de données fixes. N'inclut pas les identifiants de connexion ou les utilisateurs qui reçoivent des autorisations en fonction de leur appartenance à un rôle.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_helprotect [ [ @name = ] 'object_statement' ]   
     [ , [ @username = ] 'security_account' ]   
     [ , [ @grantorname = ] 'grantor' ]   
     [ , [ @permissionarea = ] 'type' ]  
```  
  
## <a name="arguments"></a>Arguments  
`[ @name = ] 'object_statement'` Nom de l’objet dans la base de données active, ou une instruction, qui dispose des autorisations de rapport. *object_statement* est de type **nvarchar (776)**, avec NULL comme valeur par défaut, qui retourne toutes les autorisations d’objet et d’instruction. Si sa valeur est un objet (table, vue, procédure stockée ou procédure stockée étendue), ce doit être un objet valide dans la base de données en cours. Le nom de l’objet peut inclure un qualificateur de propriétaire sous la forme _propriétaire_**.** _objet_.  
  
 Si *object_statement* est une instruction, il peut s’agir d’une instruction CREATE.  
  
`[ @username = ] 'security_account'` Nom du principal pour lequel des autorisations sont retournées. *security_account* est de **type sysname**, avec NULL comme valeur par défaut, qui retourne tous les principaux de la base de données active. *security_account* doit exister dans la base de données active.  
  
`[ @grantorname = ] 'grantor'` Nom du principal qui a accordé des autorisations. *Granter* est de **type sysname**, avec NULL comme valeur par défaut, qui retourne toutes les informations relatives aux autorisations accordées par un principal de la base de données.  
  
`[ @permissionarea = ] 'type'` Chaîne de caractères qui indique s’il faut afficher les autorisations relatives aux objets (chaîne de caractères **o**), les autorisations d’instruction (chaîne de caractères **s**) ou les deux (**système d’exploitation**). le *type* est **varchar (10)**, avec le **système d’exploitation** comme valeur par défaut. le *type* peut être n’importe quelle combinaison de **o** et de **s**, avec ou sans virgules ou espaces entre **o** et **s**.  
  
## <a name="return-code-values"></a>Codet de retour  
 0 (réussite) ou 1 (échec)  
  
## <a name="result-sets"></a>Jeux de résultats  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**Propriétaire**|**sysname**|Nom du propriétaire de l’objet.|  
|**Object**|**sysname**|Nom de l'objet.|  
|**Voient**|**sysname**|Nom du principal auquel des autorisations sont accordées.|  
|**Fournisseur d'autorisations**|**sysname**|Nom du principal qui a accordé des autorisations au bénéficiaire spécifié.|  
|**ProtectType**|**nvarchar(10**|Nom du type de protection :<br /><br /> GRANT REVOKE|  
|**Action**|**nvarchar(60)**|Nom de l’autorisation. Les instructions valides d'autorisation varient selon le type d'objet.|  
|**Colonne**|**sysname**|Type d'autorisation :<br /><br /> All = l'autorisation couvre toutes les colonnes en cours de l'objet.<br /><br /> New = l'autorisation couvre toutes les nouvelles colonnes susceptibles d'être modifiées (à l'aide de l'instruction ALTER) pour l'objet ultérieurement.<br /><br /> All+New = combinaison de All et New.<br /><br /> Retourne un point si le type d'autorisation ne s'applique pas aux colonnes.|  
  
## <a name="remarks"></a>Notes  
 Tous les paramètres de la procédure ci-dessous sont facultatifs. Si elle est exécutée sans paramètre, la procédure `sp_helprotect` affiche toutes les autorisations qui ont été accordées ou refusées dans la base de données active.  
  
 Si certains paramètres seulement sont spécifiés, utilisez les paramètres nommés pour identifier le paramètre spécifique ou `NULL` comme espace réservé. Par exemple, pour signaler toutes les autorisations du propriétaire de la base de données des fournisseurs (`dbo`), exécutez le code suivant :  
  
```  
EXEC sp_helprotect NULL, NULL, dbo;  
```  
  
 ou  
  
```  
EXEC sp_helprotect @grantorname = 'dbo';  
```  
  
 Le rapport de sortie est trié par catégorie d'autorisation, propriétaire, objet, bénéficiaire, fournisseur, catégorie du type de protection, type de protection, action et ID séquentiel de colonne.  
  
## <a name="permissions"></a>Autorisations  
 Nécessite l'appartenance au rôle **public** .  
  
 Les informations retournées sont sujettes à des restrictions d'accès aux métadonnées. Les entités sur lesquelles le principal ne possède pas d'autorisation n'apparaissent pas. Pour plus d'informations, consultez [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-listing-the-permissions-for-a-table"></a>R. Répertorier les autorisations pour une table  
 Dans l'exemple ci-dessous, les autorisations relatives à la table `titles` sont répertoriées.  
  
```  
EXEC sp_helprotect 'titles';  
```  
  
### <a name="b-listing-the-permissions-for-a-user"></a>B. Répertorier les autorisations pour un utilisateur  
 Dans l'exemple ci-dessous, toutes les autorisations que l'utilisateur `Judy` possède dans la base de données active sont répertoriées.  
  
```  
EXEC sp_helprotect NULL, 'Judy';  
```  
  
### <a name="c-listing-the-permissions-granted-by-a-specific-user"></a>C. Répertorier les autorisations accordées par un utilisateur spécifique  
 Dans l'exemple ci-dessous, toutes les autorisations qui ont été accordées par l'utilisateur `Judy` dans la base de données active sont répertoriées et `NULL` est utilisé comme espace réservé pour les paramètres manquants.  
  
```  
EXEC sp_helprotect NULL, NULL, 'Judy';  
```  
  
### <a name="d-listing-the-statement-permissions-only"></a>D. Répertorier uniquement les autorisations d'instruction  
 Dans l'exemple ci-dessous, toutes les autorisations d'instruction de la base de données active sont répertoriées et `NULL` est utilisé comme espace réservé pour les paramètres manquants.  
  
```  
EXEC sp_helprotect NULL, NULL, NULL, 's';   
```  
  
### <a name="e-listing-the-permissions-for-a-create-statement"></a>e. Liste des autorisations pour une instruction CREATE  
 L'exemple suivant dresse la liste de tous les utilisateurs disposant de l'autorisation CREATE TABLE.  
  
```  
EXEC sp_helprotect @name = 'CREATE TABLE';  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Procédures stockées de sécurité &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/security-stored-procedures-transact-sql.md)   
 [DENY &#40;Transact-SQL&#41;](../../t-sql/statements/deny-transact-sql.md)   
 [GRANT &#40;Transact-SQL&#41;](../../t-sql/statements/grant-transact-sql.md)   
 [REVOKE &#40;Transact-SQL&#41;](../../t-sql/statements/revoke-transact-sql.md)   
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
