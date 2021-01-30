---
description: sys.database_principals (Transact-SQL)
title: sys.database_principals (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 10/27/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- database_principals
- database_principals_TSQL
- sys.database_principals
- sys.database_principals_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.database_principals catalog view
ms.assetid: 8cb239e9-eb8c-4109-9cec-0d35de95fa0e
author: VanMSFT
ms.author: vanto
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: da1fff5e32ff60e17414cea92c48ec517de97bbc
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99159899"
---
# <a name="sysdatabase_principals-transact-sql"></a>sys.database_principals (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Retourne une ligne pour chaque principal de sécurité d'une base de données dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**name**|**sysname**|Nom du principal, unique dans la base de données.|  
|**principal_id**|**int**|ID du principal, unique dans la base de données.|  
|**type**|**char(1)**|Type de principal :<br /><br /> A = rôle d'application<br /><br /> C = utilisateur mappé à un certificat<br /><br /> E = utilisateur externe de Azure Active Directory<br /><br /> G = groupe Windows<br /><br /> K = utilisateur mappé à une clé asymétrique<br /><br /> R = rôle de base de données<br /><br /> S = utilisateur SQL<br /><br /> U = utilisateur Windows<br /><br /> X = groupe externe du groupe Azure Active Directory ou des applications|  
|**type_desc**|**nvarchar(60)**|Description du type de principal.<br /><br /> APPLICATION_ROLE<br /><br /> CERTIFICATE_MAPPED_USER<br /><br /> EXTERNAL_USER<br /><br /> WINDOWS_GROUP<br /><br /> ASYMMETRIC_KEY_MAPPED_USER<br /><br /> DATABASE_ROLE<br /><br /> SQL_USER<br /><br /> WINDOWS_USER<br /><br /> EXTERNAL_GROUPS|  
|**default_schema_name**|**sysname**|Nom à utiliser lorsque le nom SQL ne spécifie pas de schéma. Null pour les principaux qui ne sont pas de type S, U ou A.|  
|**create_date**|**datetime**|Heure de création du principal.|  
|**modify_date**|**datetime**|Heure de la dernière modification du principal.|  
|**owning_principal_id**|**int**|Identificateur du principal qui possède ce principal. Tous les rôles de base de données fixes sont détenus par **dbo** par défaut.|  
|**sid**|**varbinary(85)**|SID (identificateur de sécurité) du principal.  NULL pour SYS et INFORMATION SCHEMAS.|  
|**is_fixed_role**|**bit**|Avec la valeur 1, cette ligne représente une entrée pour un des rôles de base de données fixes : db_owner, db_accessadmin, db_datareader, db_datawriter, db_ddladmin, db_securityadmin, db_backupoperator, db_denydatareader, db_denydatawriter.|  
|**authentication_type**|**int**|**S’applique à** : [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] et versions ultérieures.<br /><br /> Signifie le type d'authentification. Voici les valeurs possibles et leurs descriptions.<br /><br /> 0 : aucune authentification<br />1 : authentification de l’instance<br />2 : authentification de base de données<br />3 : authentification Windows<br />4 : authentification Azure Active Directory|  
|**authentication_type_desc**|**nvarchar(60)**|**S’applique à** : [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] et versions ultérieures.<br /><br /> Description du type d'authentification. Voici les valeurs possibles et leurs descriptions.<br /><br /> NONE : aucune authentification<br />INSTANCE : authentification de l’instance<br />BASE de données : authentification de base de données<br />WINDOWS : authentification Windows<br />EXTERNE : authentification Azure Active Directory|  
|**default_language_name**|**sysname**|**S’applique à** : [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] et versions ultérieures.<br /><br /> Signifie la langue par défaut de ce principal.|  
|**default_language_lcid**|**int**|**S’applique à** : [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] et versions ultérieures.<br /><br /> Signifie le LCID par défaut de ce principal.|  
|**allow_encrypted_value_modifications**|**bit**|**S’applique à** : [!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] et versions ultérieures, [!INCLUDE[ssSDS_md](../../includes/sssds-md.md)].<br /><br /> Supprime les contrôles de métadonnées de chiffrement sur le serveur dans les opérations de copie en bloc. Cela permet à l’utilisateur de copier en bloc des données chiffrées à l’aide de Always Encrypted, entre des tables ou des bases de données, sans déchiffrer les données. La valeur par défaut est OFF. |      
  
## <a name="remarks"></a>Notes  
 Les propriétés *PasswordLastSetTime* sont disponibles sur toutes les configurations prises en charge de SQL Server, mais les autres propriétés sont uniquement disponibles lorsque SQL Server s’exécute sur Windows Server 2003 ou version ultérieure et que CHECK_POLICY et CHECK_EXPIRATION sont activés. Pour plus d’informations, consultez [stratégie de mot de passe](../../relational-databases/security/password-policy.md) .
Les valeurs de la principal_id peuvent être réutilisées dans le cas où les principaux ont été supprimés et qu’il n’est donc pas garanti qu’elles soient toujours plus nombreuses.
  
## <a name="permissions"></a>Autorisations  
 Tout utilisateur peut consulter son nom d'utilisateur, les utilisateurs système et les rôles de base de données fixes. Pour voir les autres utilisateurs, vous devez disposer de l'autorisation ALTER ANY USER, ou d'une autorisation sur l'utilisateur. Pour afficher les rôles définis par l'utilisateur, vous devez disposer de l'autorisation ALTER ANY ROLE, ou appartenir au rôle.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-listing-all-the-permissions-of-database-principals"></a>A : Énumération de toutes les autorisations des principaux de base de données  
 La requête suivante énumère les autorisations accordées ou refusées explicitement aux principaux de base de données.  
  
> [!IMPORTANT]  
>  Les autorisations de rôles de base de données fixes n'apparaissent pas dans sys.database_permissions. Par conséquent, les principaux de base de données peuvent avoir des autorisations supplémentaires non répertoriées ici.  
  
```  
SELECT pr.principal_id, pr.name, pr.type_desc,   
    pr.authentication_type_desc, pe.state_desc, pe.permission_name  
FROM sys.database_principals AS pr  
JOIN sys.database_permissions AS pe  
    ON pe.grantee_principal_id = pr.principal_id;  
```  
  
### <a name="b-listing-permissions-on-schema-objects-within-a-database"></a>B. Énumération des autorisations sur les objets de schéma dans une base de données  
 La requête suivante joint sys.database_principals et sys.database_permissions à sys.objects et sys.schemas pour répertorier les autorisations accordées ou refusées sur des objets de schéma spécifiques.  
  
```  
SELECT pr.principal_id, pr.name, pr.type_desc,   
    pr.authentication_type_desc, pe.state_desc,   
    pe.permission_name, s.name + '.' + o.name AS ObjectName  
FROM sys.database_principals AS pr  
JOIN sys.database_permissions AS pe  
    ON pe.grantee_principal_id = pr.principal_id  
JOIN sys.objects AS o  
    ON pe.major_id = o.object_id  
JOIN sys.schemas AS s  
    ON o.schema_id = s.schema_id;  
```  
  
## <a name="examples-sssdwfull-and-sspdw"></a>Exemples : [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] et [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="c-listing-all-the-permissions-of-database-principals"></a>C : liste de toutes les autorisations des principaux de base de données  
 La requête suivante énumère les autorisations accordées ou refusées explicitement aux principaux de base de données.  
  
> [!IMPORTANT]  
>  Les autorisations des rôles de base de données fixes n’apparaissent pas dans `sys.database_permissions` . Par conséquent, les principaux de base de données peuvent avoir des autorisations supplémentaires non répertoriées ici.  
  
```  
SELECT pr.principal_id, pr.name, pr.type_desc,   
    pr.authentication_type_desc, pe.state_desc, pe.permission_name  
FROM sys.database_principals AS pr  
JOIN sys.database_permissions AS pe  
    ON pe.grantee_principal_id = pr.principal_id;  
```  
  
### <a name="d-listing-permissions-on-schema-objects-within-a-database"></a>D : énumération des autorisations sur les objets de schéma dans une base de données  
 La requête suivante joint `sys.database_principals` et `sys.database_permissions` à `sys.objects` et `sys.schemas` pour répertorier les autorisations accordées ou refusées à des objets de schéma spécifiques.  
  
```  
SELECT pr.principal_id, pr.name, pr.type_desc,   
    pr.authentication_type_desc, pe.state_desc,   
    pe.permission_name, s.name + '.' + o.name AS ObjectName  
FROM sys.database_principals AS pr  
JOIN sys.database_permissions AS pe  
    ON pe.grantee_principal_id = pr.principal_id  
JOIN sys.objects AS o  
    ON pe.major_id = o.object_id  
JOIN sys.schemas AS s  
    ON o.schema_id = s.schema_id;  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Principaux &#40;moteur de base de données&#41;](../../relational-databases/security/authentication-access/principals-database-engine.md)   
 [sys.server_principals &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-server-principals-transact-sql.md)   
 [Affichages catalogue de sécurité &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/security-catalog-views-transact-sql.md)   
 [Utilisateurs de base de données à relation contenant-contenu-rendre votre base de données portable](../../relational-databases/security/contained-database-users-making-your-database-portable.md)   
 [Connexion à SQL Database avec l’authentification Azure Active Directory](/azure/azure-sql/database/authentication-aad-overview)  
  
