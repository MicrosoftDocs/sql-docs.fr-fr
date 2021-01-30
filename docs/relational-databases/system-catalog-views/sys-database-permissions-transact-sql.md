---
description: sys.database_permissions (Transact-SQL)
title: sys.database_permissions (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/11/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- database_permissions
- sys.database_permissions_TSQL
- database_permissions_TSQL
- sys.database_permissions
dev_langs:
- TSQL
helpviewer_keywords:
- sys.database_permissions catalog view
ms.assetid: c1e261f8-6cb0-4759-b5f1-5ec233602655
author: VanMSFT
ms.author: vanto
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: a488dcf39ecf3ab0fa6e82264245d38f163f8ed6
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99209476"
---
# <a name="sysdatabase_permissions-transact-sql"></a>sys.database_permissions (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Renvoie une ligne pour chaque autorisation ou chaque autorisation avec exception sur colonne dans la base de données. Pour les colonnes, il existe une ligne pour chaque autorisation différente de l'autorisation correspondante au niveau objet. Si l’autorisation de colonne est identique à l’autorisation d’objet correspondante, il n’y a aucune ligne pour celle-ci et l’autorisation appliquée est celle de l’objet.  
  
> [!IMPORTANT]  
>  Les autorisations au niveau colonne remplacent les autorisations au niveau objet sur la même entité.  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**class**|**tinyint**|Identifie la classe sur laquelle l'autorisation existe.<br /><br /> 0 = Base de données<br />1 = objet ou colonne<br />3 = Schéma<br />4 = Principal de la base de données<br />5 = assembly- **s’applique à**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] et versions ultérieures.<br />6 = Type<br />10 = collection de schémas XML- <br />                      **S’applique à** : [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] et versions ultérieures.<br />15 = type de message : **s’applique à**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] et versions ultérieures.<br />16 = contrat de service- **s’applique à**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] et versions ultérieures.<br />17 = service- **s’applique à**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] et versions ultérieures.<br />18 = liaison de service distant- **s’applique à**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] et versions ultérieures.<br />19 = route- **s’applique à**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] et versions ultérieures.<br />23 = catalogue de texte intégral- **s’applique à**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] et versions ultérieures.<br />24 = clé symétrique- **s’applique à**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] et versions ultérieures.<br />25 = certificat- **s’applique à**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] et versions ultérieures.<br />26 = clé asymétrique- **s’applique à**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] et versions ultérieures.|  
|**class_desc**|**nvarchar(60)**|Description de la classe sur laquelle l'autorisation existe.<br /><br /> DATABASE<br /><br /> OBJECT_OR_COLUMN<br /><br /> SCHEMA<br /><br /> DATABASE_PRINCIPAL<br /><br /> ASSEMBLY<br /><br /> TYPE<br /><br /> XML_SCHEMA_COLLECTION<br /><br /> MESSAGE_TYPE<br /><br /> SERVICE_CONTRACT<br /><br /> SERVICE<br /><br /> REMOTE_SERVICE_BINDING<br /><br /> ROUTE<br /><br /> FULLTEXT_CATALOG<br /><br /> SYMMETRIC_KEYS<br /><br /> CERTIFICATE<br /><br /> ASYMMETRIC_KEY|  
|**major_id**|**int**|ID de l'objet sur lequel l'autorisation existe, interprété en fonction de la classe. En règle générale, le **major_id** est simplement le type d’ID qui s’applique à ce que la classe représente. <br /><br /> 0 = la base de données elle-même <br /><br /> >0 = Object-IDs pour les objets utilisateur <br /><br /> \<0 = Object-IDs pour les objets système |  
|**minor_id**|**int**|ID secondaire de l'objet sur lequel l'autorisation existe, interprété en fonction de la classe. Souvent, le **minor_id** est égal à zéro, car aucune sous-catégorie n’est disponible pour la classe de l’objet. Dans le cas contraire, il s’agit de l’ID de colonne d’une table.|  
|**grantee_principal_id**|**int**|ID du principal de la base de données à laquelle les autorisations sont accordées.|  
|**grantor_principal_id**|**int**|ID du principal de la base de données du fournisseur de ces autorisations.|  
|**type**|**Char (4)**|Type d'autorisation de la base de données. Pour obtenir la liste des types d'autorisations, consultez le tableau ci-dessous.|  
|**permission_name**|**nvarchar(128)**|Nom de l’autorisation.|  
|**state**|**char(1)**|État de l'autorisation :<br /><br /> D = Refusée<br /><br /> R = Révoquée<br /><br /> G = Accordée<br /><br /> W = Accordée avec option Grant|  
|**state_desc**|**nvarchar(60)**|Description de l'état de l'autorisation :<br /><br /> DENY<br /><br /> REVOKE<br /><br /> GRANT<br /><br /> GRANT_WITH_GRANT_OPTION|  

## <a name="database-permissions"></a>Autorisations de base de données   
Les types d’autorisations suivants sont possibles.
  
|Type d'autorisation|Nom de l'autorisation|S'applique à l'élément sécurisable|  
|---------------------|---------------------|--------------------------|  
|AADS |ALTER ANY DATABASE EVENT SESSION |DATABASE |  
|AAMK |ALTER ANY MASK |DATABASE |  
|AEDS |ALTER ANY EXTERNAL DATA SOURCE |DATABASE |  
|AEFF |ALTER ANY EXTERNAL FILE FORMAT |DATABASE |  
|AL|ALTER|APPLICATION ROLE, ASSEMBLY, ASYMMETRIC KEY, CERTIFICATE, CONTRACT, DATABASE, FULLTEXT CATALOG, MESSAGE TYPE, OBJECT, REMOTE SERVICE BINDING, ROLE, ROUTE, SCHEMA, SERVICE, SYMMETRIC KEY, USER, XML SCHEMA COLLECTION|  
|ALAK|ALTER ANY ASYMMETRIC KEY|DATABASE|  
|ALAR|ALTER ANY APPLICATION ROLE|DATABASE|  
|ALAS|ALTER ANY ASSEMBLY|DATABASE|  
|ALCF|ALTER ANY CERTIFICATE|DATABASE|  
|ALDS|ALTER ANY DATASPACE|DATABASE|  
|ALED|ALTER ANY DATABASE EVENT NOTIFICATION|DATABASE|  
|ALFT|ALTER ANY FULLTEXT CATALOG|DATABASE|  
|ALMT|ALTER ANY MESSAGE TYPE|DATABASE|  
|ALRL|ALTER ANY ROLE|DATABASE|  
|ALRT|ALTER ANY ROUTE|DATABASE|  
|ALSB|ALTER ANY REMOTE SERVICE BINDING|DATABASE|  
|ALSC|ALTER ANY CONTRACT|DATABASE|  
|ALSK|ALTER ANY SYMMETRIC KEY|DATABASE|  
|ALSM|ALTER ANY SCHEMA|DATABASE|  
|ALSV|ALTER ANY SERVICE|DATABASE|  
|ALTG|ALTER ANY DATABASE DDL TRIGGER|DATABASE|  
|ALUS|ALTER ANY USER|DATABASE|  
|AUTH|AUTHENTICATE|DATABASE|  
|BADB|BACKUP DATABASE|DATABASE|  
|BALO|BACKUP LOG|DATABASE|  
|CL|CONTROL|APPLICATION ROLE, ASSEMBLY, ASYMMETRIC KEY, CERTIFICATE, CONTRACT, DATABASE, FULLTEXT CATALOG, MESSAGE TYPE, OBJECT, REMOTE SERVICE BINDING, ROLE, ROUTE, SCHEMA, SERVICE, SYMMETRIC KEY, TYPE, USER, XML SCHEMA COLLECTION|  
|CO|CONNECT|DATABASE|  
|CORP|CONNECT REPLICATION|DATABASE|  
|CP|CHECKPOINT|DATABASE|  
|CRAG|CREATE AGGREGATE|DATABASE|  
|CRAK|CREATE ASYMMETRIC KEY|DATABASE|  
|CRAS|CREATE ASSEMBLY|DATABASE|  
|CRCF|CREATE CERTIFICATE|DATABASE|  
|CRDB|CREATE DATABASE|DATABASE|  
|CRDF|CREATE DEFAULT|DATABASE|  
|CRED|CREATE DATABASE DDL EVENT NOTIFICATION|DATABASE|  
|CRFN|CREATE FUNCTION|DATABASE|  
|CRFT|CREATE FULLTEXT CATALOG|DATABASE|  
|CRMT|CREATE MESSAGE TYPE|DATABASE|  
|CRPR|CREATE PROCEDURE|DATABASE|  
|CRQU|CREATE QUEUE|DATABASE|  
|CRRL|CREATE ROLE|DATABASE|  
|CRRT|CREATE ROUTE|DATABASE|  
|CRRU|CREATE RULE|DATABASE|  
|CRSB|CREATE REMOTE SERVICE BINDING|DATABASE|  
|CRSC|CREATE CONTRACT|DATABASE|  
|CRSK|CREATE SYMMETRIC KEY|DATABASE|  
|CRSM|CREATE SCHEMA|DATABASE|  
|CRSN|CREATE SYNONYM|DATABASE|  
|CRSO|**S’applique à** : [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] et versions ultérieures.<br /><br /> CREATE SEQUENCE|DATABASE|  
|CRSV|CREATE SERVICE|DATABASE|  
|CRTB|CREATE TABLE|DATABASE|  
|CRTY|CREATE TYPE|DATABASE|  
|CRVW|CREATE VIEW|DATABASE|  
|CRXS|**S’applique à** : [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] et versions ultérieures.<br /><br /> CREATE XML SCHEMA COLLECTION|DATABASE|  
|DABO |ADMINISTER DATABASE BULK OPERATIONS | DATABASE |
|DL|DELETE|DATABASE, OBJECT, SCHEMA|  
|EAES |EXECUTE ANY EXTERNAL SCRIPT |DATABASE |
|EX|Exécutez|ASSEMBLY, DATABASE, OBJECT, SCHEMA, TYPE, XML SCHEMA COLLECTION|  
|IM|IMPERSONATE|Utilisateur|  
|IN|INSERT|DATABASE, OBJECT, SCHEMA|  
|RC|RECEIVE|OBJECT|  
|RF|REFERENCES|ASSEMBLY, ASYMMETRIC KEY, CERTIFICATE, CONTRACT, DATABASE, FULLTEXT CATALOG, MESSAGE TYPE, OBJECT, SCHEMA, SYMMETRIC KEY, TYPE, XML SCHEMA COLLECTION|  
|SL|SELECT|DATABASE, OBJECT, SCHEMA|  
|SN|SEND|SERVICE|  
|SPLN|SHOWPLAN|DATABASE|  
|SUQN|SUBSCRIBE QUERY NOTIFICATIONS|DATABASE|  
|TO|TAKE OWNERSHIP|ASSEMBLY, ASYMMETRIC KEY, CERTIFICATE, CONTRACT, DATABASE, FULLTEXT CATALOG, MESSAGE TYPE, OBJECT, REMOTE SERVICE BINDING, ROLE, ROUTE, SCHEMA, SERVICE, SYMMETRIC KEY, TYPE, XML SCHEMA COLLECTION|  
|UP|UPDATE|DATABASE, OBJECT, SCHEMA|  
|VW|VIEW DEFINITION|APPLICATION ROLE, ASSEMBLY, ASYMMETRIC KEY, CERTIFICATE, CONTRACT, DATABASE, FULLTEXT CATALOG, MESSAGE TYPE, OBJECT, REMOTE SERVICE BINDING, ROLE, ROUTE, SCHEMA, SERVICE, SYMMETRIC KEY, TYPE, USER, XML SCHEMA COLLECTION|  
|VWCK |VIEW ANY COLUMN ENCRYPTION KEY DEFINITION|DATABASE |  
|VWCM |VIEW ANY COLUMN MASTER KEY DEFINITION|DATABASE |  
|VWCT|VIEW CHANGE TRACKING|TABLE, SCHEMA|  
|VWDS|VIEW DATABASE STATE|DATABASE|  
  
## <a name="permissions"></a>Autorisations  
 Tout utilisateur peut consulter ses propres autorisations. Pour afficher les autorisations d'autres utilisateurs, vous devez disposer de l'autorisation VIEW DEFINITION, ALTER ANY USER, ou de n'importe quelle autorisation sur un utilisateur. Pour afficher les rôles définis par l'utilisateur, vous devez disposer de l'autorisation ALTER ANY ROLE, ou appartenir au rôle (notamment public).  
  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] Pour plus d'informations, consultez [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
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
    
  
## <a name="see-also"></a>Voir aussi  
 [Securables](../../relational-databases/security/securables.md)   
 [Hiérarchie des autorisations &#40;Moteur de base de données&#41;](../../relational-databases/security/permissions-hierarchy-database-engine.md)   
 [Affichages catalogue de sécurité &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/security-catalog-views-transact-sql.md)   
 [Affichages catalogue &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)  
  
  


