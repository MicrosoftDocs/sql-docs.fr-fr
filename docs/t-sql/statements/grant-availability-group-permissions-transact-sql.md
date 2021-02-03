---
title: GRANT - Autorisations sur un groupe de disponibilité
description: Octroyez des autorisations sur un groupe de disponibilité AlwaysOn.
titleSuffix: SQL Server (Transact-SQL)
ms.custom: seo-lt-2019
ms.date: 06/12/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
dev_langs:
- TSQL
helpviewer_keywords:
- Availability Groups [SQL Server], permissions
- GRANT statement, availability groups
- granting permissions, [SQL Server], availability groups
- permissions [SQL Server], availability group
ms.assetid: 060eb839-666a-4046-9e1d-5edc9ea75a11
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: d1047980f83de32c5631c7d45673fe743607ebbc
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99208171"
---
# <a name="grant-availability-group-permissions-transact-sql"></a>GRANT (Octroi d'autorisations de groupe de disponibilité) (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Octroie des autorisations sur un groupe de disponibilité AlwaysOn.  
  

 ![Icône du lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql
GRANT permission  [ ,...n ] ON AVAILABILITY GROUP :: availability_group_name  
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
 Spécifie une autorisation qu'il est possible d'accorder sur un groupe de disponibilité. Pour obtenir la liste des autorisations, consultez la section Notes plus loin dans cette rubrique.  
  
 ON AVAILABILITY GROUP **::** _availability_group_name_  
 Spécifie le groupe de disponibilité sur lequel l'autorisation est accordée. Le qualificateur d’étendue ( **::** ) est obligatoire.  
  
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
  
 Des informations sur les groupes de disponibilité sont consultables dans la vue de catalogue [sys.availability_groups &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-availability-groups-transact-sql.md). Des informations sur les autorisations de serveur sont consultables dans la vue de catalogue [sys.server_permissions](../../relational-databases/system-catalog-views/sys-server-permissions-transact-sql.md), et des informations sur les principaux de serveur sont consultables dans la vue de catalogue [sys.server_principals](../../relational-databases/system-catalog-views/sys-server-principals-transact-sql.md).  
  
 Un groupe de disponibilité est un élément sécurisable au niveau serveur. Les autorisations les plus spécifiques et limitées qu'il est possible d'accorder sur un groupe de disponibilité sont répertoriées dans le tableau ci-dessous, avec les autorisations plus générales qui les incluent de manière implicite.  
  
|Autorisations de groupe de disponibilité|Impliquée par une autorisation de groupe de disponibilité|Déduite d'une autorisation de serveur|  
|-----------------------------------|----------------------------------------------|----------------------------------|  
|ALTER|CONTROL|ALTER ANY AVAILABILITY GROUP|  
|CONNECT|CONTROL|CONTROL SERVER|  
|CONTROL|CONTROL|CONTROL SERVER|  
|TAKE OWNERSHIP|CONTROL|CONTROL SERVER|  
|VIEW DEFINITION|CONTROL|VIEW ANY DEFINITION|  
  
 Pour obtenir un graphique de toutes les autorisations de [!INCLUDE[ssDE](../../includes/ssde-md.md)], consultez [Affiche des autorisations du moteur de base de données](https://aka.ms/sql-permissions-poster).  
  
## <a name="permissions"></a>Autorisations  
 Nécessite l’autorisation CONTROL sur le groupe de disponibilité ou l’autorisation ALTER ANY AVAILABILITY GROUP sur le serveur.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-granting-view-definition-permission-on-an-availability-group"></a>R. Octroi de l'autorisation VIEW DEFINITION sur un groupe de disponibilité  
 L'exemple suivant octroie l'autorisation `VIEW DEFINITION` sur le groupe de disponibilité `MyAg` sur la connexion `ZArifin`de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
```sql  
USE master;  
GRANT VIEW DEFINITION ON AVAILABILITY GROUP::MyAg TO ZArifin;  
GO  
```  
  
### <a name="b-granting-take-ownership-permission-with-the-grant-option"></a>B. Octroi d'une autorisation TAKE OWNERSHIP avec l'option GRANT OPTION  
 Dans l'exemple ci-dessous, l'autorisation `TAKE OWNERSHIP` sur le groupe de disponibilité `MyAg` est accordée à l'utilisateur [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]`PKomosinski` avec l'option `GRANT OPTION`.  
  
```sql  
USE master;  
GRANT TAKE OWNERSHIP ON AVAILABILITY GROUP::MyAg TO PKomosinski   
    WITH GRANT OPTION;  
GO  
```  
  
### <a name="c-granting-control-permission-on-an-availability-group"></a>C. Accorder l'autorisation CONTROL sur un groupe de disponibilité  
 L'exemple suivant octroie l'autorisation `CONTROL` sur le groupe de disponibilité `MyAg` à l'utilisateur [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]`PKomosinski`. CONTROL permet un contrôle total des connexions du groupe de disponibilité, même s'il ne s'agit pas du propriétaire du groupe de disponibilité. Pour modifier la propriété, consultez [ALTER AUTHORIZATION &#40;Transact-SQL&#41;](../../t-sql/statements/alter-authorization-transact-sql.md).  
  
```sql  
USE master;  
GRANT CONTROL ON AVAILABILITY GROUP::MyAg TO PKomosinski;  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [REVOKE - Révoquer des autorisations sur un groupe de disponibilité &#40;Transact-SQL&#41;](../../t-sql/statements/revoke-availability-group-permissions-transact-sql.md)   
 [DENY - Refuser des autorisations sur un groupe de disponibilité &#40;Transact-SQL&#41;](../../t-sql/statements/deny-availability-group-permissions-transact-sql.md)   
 [CREATE AVAILABILITY GROUP &#40;Transact-SQL&#41;](../../t-sql/statements/create-availability-group-transact-sql.md)   
 [sys.availability_groups &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-availability-groups-transact-sql.md)   
 [Vues de catalogue des groupes de disponibilité AlwaysOn &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/always-on-availability-groups-catalog-views-transact-sql.md) [Autorisations &#40;moteur de base de données&#41;](../../relational-databases/security/permissions-database-engine.md)   
 [Principaux &#40;moteur de base de données&#41;](../../relational-databases/security/authentication-access/principals-database-engine.md)  
  
  
