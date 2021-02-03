---
description: GRANT – octroi d'autorisations de point de terminaison (Transact-SQL)
title: GRANT - Octroyer des autorisations sur un point de terminaison (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/17/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
dev_langs:
- TSQL
helpviewer_keywords:
- endpoints [SQL Server], permissions
- granting permissions [SQL Server], endpoints
- GRANT statement, endpoints
- permissions [SQL Server], endpoints
ms.assetid: 9eda885c-fc3a-4c9d-8de6-ce07fb35a934
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 7c2a366e55f27475b2bda9e0189ed079f8b796d2
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99205779"
---
# <a name="grant-endpoint-permissions-transact-sql"></a>GRANT – octroi d'autorisations de point de terminaison (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Permet d'accorder des autorisations sur un point de terminaison.  

  
 ![Icône du lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql
GRANT permission  [ ,...n ] ON ENDPOINT :: endpoint_name  
        TO < server_principal >  [ ,...n ]  
    [ WITH GRANT OPTION ]  
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
  
 TO \<server_principal>  
 Spécifie la connexion [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pour laquelle l'autorisation doit être accordée.  
  
 *SQL_Server_login*  
 Spécifie le nom d'une connexion [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 *SQL_Server_login_from_Windows_login*  
 Spécifie le nom d'une connexion [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] créée à partir d'une connexion Windows.  
  
 *SQL_Server_login_from_certificate*  
 Spécifie le nom d'une connexion [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] mappée sur un certificat.  
  
 *SQL_Server_login_from_AsymKey*  
 Spécifie le nom d'une connexion [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] mappée sur une clé asymétrique.  
  
 WITH GRANT OPTION  
 Indique que le principal a également la possibilité d'accorder l'autorisation spécifiée à d'autres principaux.  
  
 AS *SQL_Server_login*  
 Spécifie la connexion [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] à partir de laquelle le principal qui exécute cette requête dérive son droit d'accorder l'autorisation.  
  
## <a name="remarks"></a>Notes  
 Les autorisations dans l’étendue du serveur peuvent être accordées seulement quand la base de données active est **master**.  
  
 Des informations sur les points de terminaison sont consultables dans la vue de catalogue [sys.endpoints](../../relational-databases/system-catalog-views/sys-endpoints-transact-sql.md). Des informations sur les autorisations de serveur sont consultables dans la vue de catalogue [sys.server_permissions](../../relational-databases/system-catalog-views/sys-server-permissions-transact-sql.md), et des informations sur les principaux de serveur sont consultables dans la vue de catalogue [sys.server_principals](../../relational-databases/system-catalog-views/sys-server-principals-transact-sql.md).  
  
 Un point de terminaison est un élément sécurisable au niveau serveur. Les autorisations les plus spécifiques et limitées qu'il est possible d'accorder sur un point de terminaison sont répertoriées dans le tableau ci-dessous, avec les autorisations plus générales qui les incluent de manière implicite.  
  
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
  
### <a name="a-granting-view-definition-permission-on-an-endpoint"></a>R. Octroi d'une autorisation VIEW DEFINITION sur un point de terminaison  
 Dans l'exemple ci-dessous, l'autorisation `VIEW DEFINITION` sur le point de terminaison `Mirror7` est accordée à la connexion [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]`ZArifin`.  
  
```sql  
USE master;  
GRANT VIEW DEFINITION ON ENDPOINT::Mirror7 TO ZArifin;  
GO  
```  
  
### <a name="b-granting-take-ownership-permission-with-the-grant-option"></a>B. Octroi d'une autorisation TAKE OWNERSHIP avec l'option GRANT OPTION  
 Dans l'exemple ci-dessous, l'autorisation `TAKE OWNERSHIP` sur le point de terminaison `Shipping83` est accordée à l'utilisateur [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]`PKomosinski` avec l'option `GRANT OPTION`.  
  
```sql  
USE master;  
GRANT TAKE OWNERSHIP ON ENDPOINT::Shipping83 TO PKomosinski   
    WITH GRANT OPTION;  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [DENY - Refuser des autorisations sur un point de terminaison &#40;Transact-SQL&#41;](../../t-sql/statements/deny-endpoint-permissions-transact-sql.md)   
 [REVOKE - Révoquer des autorisations sur un point de terminaison &#40;Transact-SQL&#41;](../../t-sql/statements/revoke-endpoint-permissions-transact-sql.md)   
 [CREATE ENDPOINT &#40;Transact-SQL&#41;](../../t-sql/statements/create-endpoint-transact-sql.md)   
 [Vues de catalogue des points de terminaison &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/endpoints-catalog-views-transact-sql.md)   
 [sys.endpoints &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-endpoints-transact-sql.md)   
 [Autorisations &#40;moteur de base de données&#41;](../../relational-databases/security/permissions-database-engine.md)   
 [Principaux &#40;moteur de base de données&#41;](../../relational-databases/security/authentication-access/principals-database-engine.md)  
  
  
