---
description: REVOKE – révocation d'autorisations de point de terminaison (Transact-SQL)
title: REVOKE - Révoquer des autorisations sur un point de terminaison (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/10/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
dev_langs:
- TSQL
helpviewer_keywords:
- endpoints [SQL Server], permissions
- REVOKE statement, endpoints
- permissions [SQL Server], endpoints
ms.assetid: 826f513e-9ad0-46b9-87ad-7525713638c8
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 7a672938bacfa9555ea988c9bc68621a4aab9155
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99209887"
---
# <a name="revoke-endpoint-permissions-transact-sql"></a>REVOKE – révocation d'autorisations de point de terminaison (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Permet de révoquer des autorisations accordées ou refusées sur un point de terminaison.  
  
 ![Icône du lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql
REVOKE [ GRANT OPTION FOR ] permission [ ,...n ]   
    ON ENDPOINT :: endpoint_name  
    { FROM | TO } <server_principal> [ ,...n ]  
    [ CASCADE ]  
    [ AS SQL_Server_login ]   
  
<server_principal> ::=   
        SQL_Server_login  
    | SQL_Server_login_from_Windows_login   
    | SQL_Server_login_from_certificate   
    | SQL_Server_login_from_AsymKey  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments
 *permission*  
 Spécifie une autorisation qui peut être accordée sur un point de terminaison. Pour obtenir la liste des autorisations, consultez la section Notes plus loin dans cette rubrique.  
  
 ON ENDPOINT **::** _endpoint_name_  
 Spécifie le point de terminaison sur lequel l'autorisation est accordée. Le qualificateur d’étendue ( **::** ) est obligatoire.  
  
 { FROM | TO } \<server_principal> spécifie la connexion [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pour laquelle l’autorisation est révoquée.  
  
 *SQL_Server_login*  
 Spécifie le nom d'une connexion [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 *SQL_Server_login_from_Windows_login*  
 Spécifie le nom d'une connexion [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] créée à partir d'une connexion Windows.  
  
 *SQL_Server_login_from_certificate*  
 Spécifie le nom d'une connexion [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] mappée sur un certificat.  
  
 *SQL_Server_login_from_AsymKey*  
 Spécifie le nom d'une connexion [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] mappée sur une clé asymétrique.  
  
 GRANT OPTION  
 Indique que le droit d'accorder l'autorisation spécifiée à d'autres principaux sera révoqué. L'autorisation elle-même ne sera pas révoquée.  
  
> [!IMPORTANT]  
>  Si le principal possède l'autorisation spécifiée sans l'option GRANT, l'autorisation elle-même sera révoquée.  
  
 CASCADE  
 Indique que l'autorisation en cours de révocation est également révoquée sur les principaux auxquels cette autorisation a été accordée ou révoquée par ce principal.  
  
> [!CAUTION]  
>  Une révocation en cascade d'une autorisation accordée avec l'option WITH GRANT OPTION entraîne la révocation des deux options GRANT et DENY de cette autorisation.  
  
 AS *SQL_Server_login*  
 Spécifie la connexion [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] à partir de laquelle le principal qui exécute cette requête dérive son droit de révoquer l'autorisation.  
  
## <a name="remarks"></a>Notes  
 Les autorisations dans l’étendue du serveur peuvent être révoquées seulement quand la base de données active est **master**.  
  
 Des informations sur les points de terminaison sont consultables dans la vue de catalogue [sys.endpoints](../../relational-databases/system-catalog-views/sys-endpoints-transact-sql.md). Des informations sur les autorisations de serveur sont consultables dans la vue de catalogue [sys.server_permissions](../../relational-databases/system-catalog-views/sys-server-permissions-transact-sql.md), et des informations sur les principaux de serveur sont consultables dans la vue de catalogue [sys.server_principals](../../relational-databases/system-catalog-views/sys-server-principals-transact-sql.md).  
  
 Un point de terminaison est un élément sécurisable au niveau serveur. Les autorisations les plus spécifiques et limitées qu'il est possible de révoquer sur un point de terminaison sont répertoriées dans le tableau ci-dessous, avec les autorisations plus générales qui les incluent de manière implicite.  
  
|Autorisation de point de terminaison|Déduite d'une autorisation de point de terminaison|Déduite d'une autorisation de serveur|  
|-------------------------|------------------------------------|----------------------------------|  
|ALTER|CONTROL|ALTER ANY ENDPOINT|  
|CONNECT|CONTROL|CONTROL SERVER|  
|CONTROL|CONTROL|CONTROL SERVER|  
|TAKE OWNERSHIP|CONTROL|CONTROL SERVER|  
|VIEW DEFINITION|CONTROL|VIEW ANY DEFINITION|  
  
## <a name="permissions"></a>Autorisations  
 Requiert l'autorisation CONTROL sur le point de terminaison ou l'autorisation ALTER ANY ENDPOINT sur le serveur.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-revoking-view-definition-permission-on-an-endpoint"></a>R. Révocation d'une autorisation VIEW DEFINITION sur un point de terminaison  
 Dans l'exemple ci-dessous, l'autorisation `VIEW DEFINITION` sur le point de terminaison `Mirror7` est révoquée pour la connexion [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]`ZArifin`.  
  
```sql  
USE master;  
REVOKE VIEW DEFINITION ON ENDPOINT::Mirror7 FROM ZArifin;  
GO  
```  
  
### <a name="b-revoking-take-ownership-permission-with-the-cascade-option"></a>B. Révocation d'une autorisation TAKE OWNERSHIP avec l'option CASCADE  
 Dans l’exemple ci-dessous, l’autorisation `TAKE OWNERSHIP` sur le point de terminaison `Shipping83` est révoquée pour l’utilisateur [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]`PKomosinski` et pour tous les principaux auxquels `PKomosinski` a accordé l’autorisation `TAKE OWNERSHIP` sur `Shipping83`.  
  
```sql  
USE master;  
REVOKE TAKE OWNERSHIP ON ENDPOINT::Shipping83 FROM PKomosinski   
    CASCADE;  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Autorisations GRANT sur point de terminaison &#40;Transact-SQL&#41;](../../t-sql/statements/grant-endpoint-permissions-transact-sql.md)   
 [DENY - Refuser des autorisations sur un point de terminaison &#40;Transact-SQL&#41;](../../t-sql/statements/deny-endpoint-permissions-transact-sql.md)   
 [CREATE ENDPOINT &#40;Transact-SQL&#41;](../../t-sql/statements/create-endpoint-transact-sql.md)   
 [Vues de catalogue des points de terminaison &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/endpoints-catalog-views-transact-sql.md)   
 [sys.endpoints &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-endpoints-transact-sql.md)   
 [Autorisations &#40;moteur de base de données&#41;](../../relational-databases/security/permissions-database-engine.md)   
 [Principaux &#40;moteur de base de données&#41;](../../relational-databases/security/authentication-access/principals-database-engine.md)  
  
  

