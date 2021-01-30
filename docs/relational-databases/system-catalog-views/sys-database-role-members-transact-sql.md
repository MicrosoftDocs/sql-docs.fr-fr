---
description: sys.database_role_members (Transact-SQL)
title: sys.database_role_members (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 01/31/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.database_role_members_TSQL
- sys.database_role_members
- database_role_members_TSQL
- database_role_members
dev_langs:
- TSQL
helpviewer_keywords:
- sys.database_role_members catalog view
ms.assetid: ed1b019d-ca48-4db3-85df-cf6d2db591cf
author: VanMSFT
ms.author: vanto
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: f6ce6c54cfa18d79667c4c844e7771223ee0d3e1
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99209471"
---
# <a name="sysdatabase_role_members-transact-sql"></a>sys.database_role_members (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Retourne une ligne pour chaque membre de chaque rôle de base de données.  Les utilisateurs de base de données, les rôles d’application et d’autres rôles de base de données peuvent être membres d’un rôle de base de données. Pour ajouter des membres à un rôle, utilisez l’instruction [ALTER ROLE](../../t-sql/statements/alter-role-transact-sql.md) avec l' `ADD MEMBER` option. Joignez-vous avec [sys.database_principals](../../relational-databases/system-catalog-views/sys-database-principals-transact-sql.md) pour retourner les noms des `principal_id` valeurs.
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**role_principal_id**|**int**|ID du principal de la base de données du rôle.|  
|**member_principal_id**|**int**|ID du principal de la base de données du membre.|  
  
## <a name="permissions"></a>Autorisations  
 Tout utilisateur peut consulter sa propre appartenance au rôle. Pour afficher les autres appartenances aux rôles, vous devez appartenir au `db_securityadmin` rôle de base de données fixe ou `VIEW DEFINITION` à la base de données.  
  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] Pour plus d'informations, consultez [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="example"></a>Exemple  
 La requête suivante retourne les membres des rôles de base de données.  
  
```  
SELECT DP1.name AS DatabaseRoleName,   
   isnull (DP2.name, 'No members') AS DatabaseUserName   
 FROM sys.database_role_members AS DRM  
 RIGHT OUTER JOIN sys.database_principals AS DP1  
   ON DRM.role_principal_id = DP1.principal_id  
 LEFT OUTER JOIN sys.database_principals AS DP2  
   ON DRM.member_principal_id = DP2.principal_id  
WHERE DP1.type = 'R'
ORDER BY DP1.name;  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Affichages catalogue de sécurité &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/security-catalog-views-transact-sql.md)   
 [Principaux &#40;moteur de base de données&#41;](../../relational-databases/security/authentication-access/principals-database-engine.md)   
 [Affichages catalogue &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)  
[ALTER ROLE (Transact-SQLL)](../../t-sql/statements/alter-role-transact-sql.md)      
[sys.server_role_members (Transact-SQL)](../../relational-databases/system-catalog-views/sys-server-role-members-transact-sql.md)   
  


