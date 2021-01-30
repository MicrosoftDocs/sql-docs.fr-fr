---
description: sys.server_permissions (Transact-SQL)
title: sys.server_permissions (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 09/20/2019
ms.prod: sql
ms.prod_service: database-engine, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.server_permissions_TSQL
- sys.server_permissions
- server_permissions
- server_permissions_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.server_permissions catalog view
ms.assetid: 7d78bf17-6c64-4166-bd0b-9e9e20992136
author: VanMSFT
ms.author: vanto
monikerRange: '>=aps-pdw-2016||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: b30e7adeb6ad09417b9ab02cb01610774d09a148
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99159046"
---
# <a name="sysserver_permissions-transact-sql"></a>sys.server_permissions (Transact-SQL)
[!INCLUDE [sql-asdbmi-pdw](../../includes/applies-to-version/sql-asdbmi-pdw.md)]

  Retourne une ligne pour chaque autorisation de niveau serveur.  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**class**|**tinyint**|Identifie la classe d'éléments sur laquelle l'autorisation existe.<br /><br /> 100 = serveur<br /><br /> 101 = principal serveur<br /><br /> 105 = point de terminaison|  
|**class_desc**|**nvarchar(60)**|Description de la classe sur laquelle l'autorisation existe. Une des valeurs suivantes :<br /><br /> **SERVEURS**<br /><br /> **SERVER_PRINCIPAL**<br /><br /> **POSTE**|  
|**major_id**|**int**|ID de l'élément sécurisable sur lequel l'autorisation existe, interprété selon la classe. Il s'agit généralement de l'ID qui s'applique à ce que la classe représente. L'interprétation des éléments non standard s'effectue ainsi :<br /><br /> 100 = toujours 0|  
|**minor_id**|**int**|ID secondaire d'un élément sur lequel l'autorisation existe, interprété selon la classe.|  
|**grantee_principal_id**|**int**|ID du principal du serveur auquel les autorisations sont accordées.|  
|**grantor_principal_id**|**int**|ID du principal du serveur de la personne qui accorde ces autorisations.|  
|**type**|**Char (4)**|Type d'autorisation serveur. Pour obtenir la liste des types d'autorisations, consultez le tableau ci-dessous.|  
|**permission_name**|**nvarchar(128)**|Nom de l’autorisation.|  
|**state**|**char(1)**|État de l'autorisation :<br /><br /> D = Refusée<br /><br /> R = Révoquée<br /><br /> G = Accordée<br /><br /> W = GRANT WITH GRANT OPTION|  
|**state_desc**|**nvarchar(60)**|Description de l'état de l'autorisation :<br /><br /> DENY<br /><br /> REVOKE<br /><br /> GRANT<br /><br /> GRANT_WITH_GRANT_OPTION|  
  
|Type d'autorisation|Nom de l'autorisation|S'applique à l'élément sécurisable|  
|---------------------|---------------------|--------------------------|  
|AAES|ALTER ANY EVENT SESSION|SERVER|
|ADBO|ADMINISTER BULK OPERATIONS|SERVER|  
|AL|ALTER|ENDPOINT, LOGIN|  
|ALAA|ALTER ANY SERVER AUDIT|SERVER|
|ALAG|ALTER ANY AVAILABILITY GROUP|SERVER|
|ALCD|ALTER ANY CREDENTIAL|SERVER|  
|ALCO|ALTER ANY CONNECTION|SERVER|  
|ALDB|ALTER ANY DATABASE|SERVER|  
|ALES|ALTER ANY EVENT NOTIFICATION|SERVER|  
|ALHE|ALTER ANY ENDPOINT|SERVER|  
|ALLG|ALTER ANY LOGIN|SERVER|  
|ALLS|ALTER ANY LINKED SERVER|SERVER|  
|ALRS|ALTER RESOURCES|SERVER|
|ALSR|ALTER ANY SERVER ROLE|SERVER|  
|ALSS|ALTER SERVER STATE|SERVER|  
|ALST|ALTER SETTINGS|SERVER|  
|ALTR|ALTER TRACE|SERVER|  
|AUTH|AUTHENTICATE SERVER|SERVER|
|CADB|CONNECT ANY DATABASE|SERVER|  
|CL|CONTROL|ENDPOINT, LOGIN|  
|CL|CONTROL SERVER|SERVER|  
|CO|CONNECT|ENDPOINT|  
|COSQ|CONNECT SQL|SERVER|
|CRAC|Créer un groupe de disponibilité|SERVER|  
|CRDB|CREATE ANY DATABASE|SERVER|  
|CRDE|CREATE DDL EVENT NOTIFICATION|SERVER|  
|CRHE|CREATE ENDPOINT|SERVER|
|CRSR|CREATE SERVER ROLE|SERVER|  
|CRTE|CREATE TRACE EVENT NOTIFICATION|SERVER|
|IAL|IMPERSONATE ANY LOGIN|SERVER|  
|IM|IMPERSONATE|Connexion|  
|SHDN|SHUTDOWN|SERVER|
|SUS|SELECT ALL USER SECURABLES|SERVER|
|TO|TAKE OWNERSHIP|ENDPOINT|  
|VW|VIEW DEFINITION|ENDPOINT, LOGIN|  
|VWAD|VIEW ANY DEFINITION|SERVER|  
|VWDB|VIEW ANY DATABASE|SERVER|  
|VWSS|VIEW SERVER STATE|SERVER|  
|XA|EXTERNAL ACCESS|SERVER|
|XU|UNSAFE ASSEMBLY|SERVER|
  
## <a name="permissions"></a>Autorisations  
 Tout utilisateur peut consulter ses propres autorisations. Pour afficher les autorisations d'autres connexions, vous devez disposer de l'autorisation VIEW DEFINITION, ALTER ANY LOGIN, ou de n'importe quelle autorisation sur une connexion. Pour afficher les rôles serveur définis par l'utilisateur, vous devez disposer de l'autorisation ALTER ANY SERVER ROLE, ou appartenir au rôle.  
  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] Pour plus d'informations, consultez [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="examples"></a>Exemples  
 La requête suivante répertorie les autorisations explicitement accordées ou refusées aux principaux de serveur.  
  
> [!IMPORTANT]  
> Les autorisations des rôles serveur fixes n'apparaissent pas dans sys.server_permissions. Par conséquent, les principaux de serveur peuvent avoir des autorisations supplémentaires non répertoriées ici.  
  
```  
SELECT pr.principal_id, pr.name, pr.type_desc,   
    pe.state_desc, pe.permission_name   
FROM sys.server_principals AS pr   
JOIN sys.server_permissions AS pe   
    ON pe.grantee_principal_id = pr.principal_id;  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Affichages catalogue de sécurité &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/security-catalog-views-transact-sql.md)   
 [Securables](../../relational-databases/security/securables.md)   
 [Affichages catalogue &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [Autorisations &#40;moteur de base de données&#41;](../../relational-databases/security/permissions-database-engine.md)   
 [Hiérarchie des autorisations &#40;moteur de base de données&#41;](../../relational-databases/security/permissions-hierarchy-database-engine.md)  
