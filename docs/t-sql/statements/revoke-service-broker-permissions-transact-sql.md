---
description: Autorisations REVOKE dans Service Broker (Transact-SQL)
title: Autorisations REVOKE dans Service Broker (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
dev_langs:
- TSQL
helpviewer_keywords:
- routes [Service Broker], permissions
- Service Broker, permissions
- remote service bindings [Service Broker], permissions
- permissions [Service Broker]
- message types [Service Broker], permissions
- contracts [Service Broker], permissions
- services [Service Broker], permissions
- REVOKE statement, Service Broker
ms.assetid: 70f1d938-97e2-48a4-9bc0-8be9f2f2c36d
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: d28fe5ba46671e38089727735701107a9427ac4e
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99194970"
---
# <a name="revoke-service-broker-permissions-transact-sql"></a>Autorisations REVOKE dans Service Broker (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Révoque les autorisations sur un contrat, un type de message, une liaison de service distant, un itinéraire ou un service [!INCLUDE[ssSB](../../includes/sssb-md.md)].  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql
  
REVOKE [ GRANT OPTION FOR ] permission [ ,...n ] ON  
    {   
       [ CONTRACT :: contract_name ]   
       | [ MESSAGE TYPE :: message_type_name ]    
       | [ REMOTE SERVICE BINDING :: remote_binding_name ]    
       | [ ROUTE :: route_name ]   
       | [ SERVICE :: service_name ]      
        }  
    { TO | FROM } database_principal [ ,...n ]  
    [ CASCADE ]  
    [ AS revoking_principal ]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments
 GRANT OPTION FOR  
 Indique que le droit d'accorder le droit spécifié à d'autres principaux sera révoqué. L’autorisation elle-même ne sera pas révoquée.  
  
> [!IMPORTANT]  
>  Si le principal possède l'autorisation spécifiée sans l'option GRANT, l'autorisation elle-même sera révoquée.  
  
 *permission*  
 Spécifie une autorisation qui peut être révoquée sur un élément sécurisable [!INCLUDE[ssSB](../../includes/sssb-md.md)]. Pour obtenir la liste de ces autorisations, consultez la section Remarques plus loin dans cette rubrique.  
  
 CONTRACT **::**_contract_name_  
 Spécifie le contrat sur lequel l'autorisation est révoquée. Le qualificateur d’étendue **::** est obligatoire.  
  
 MESSAGE TYPE **::**_message_type_name_  
 Spécifie le type de message sur lequel l'autorisation est révoquée. Le qualificateur d’étendue **::** est obligatoire.  
  
 REMOTE SERVICE BINDING **::**_remote_binding_name_  
 Spécifie la liaison de service distant sur laquelle l'autorisation est révoquée. Le qualificateur d’étendue **::** est obligatoire.  
  
 ROUTE **::**_route_name_  
 Spécifie l'itinéraire sur lequel l'autorisation est révoquée. Le qualificateur d’étendue **::** est obligatoire.  
  
 SERVICE **::**_message_type_name_  
 Spécifie le service sur lequel l'autorisation est révoquée. Le qualificateur d’étendue **::** est obligatoire.  
  
 *database_principal*  
 Spécifie le principal pour lequel l'autorisation est révoquée. *database_principal* peut être l’un des éléments suivants :  
  
-   Utilisateur de base de données  
  
-   Rôle de base de données  
  
-   Rôle d'application  
  
-   Utilisateur de base de données mappé à une connexion Windows  
  
-   Utilisateur de base de données mappé à un groupe Windows  
  
-   Utilisateur de base de données mappé à un certificat  
  
-   Utilisateur de base de données mappé à une clé asymétrique  
  
-   Utilisateur de base de données mappé à un principal de serveur  
  
 CASCADE  
 Indique que l'autorisation en cours de révocation est également révoquée sur les principaux auxquels cette autorisation a été accordée ou révoquée par ce principal.  
  
> [!CAUTION]  
>  Une révocation en cascade d'une autorisation accordée avec l'option WITH GRANT OPTION entraîne la révocation des deux options GRANT et DENY de cette autorisation.  
  
 AS *revoking_principal*  
 Spécifie un principal dont le principal qui exécute cette requête dérive son droit de révoquer l'autorisation. *revoking_principal* peut être l’un des éléments suivants :  
  
-   Utilisateur de base de données  
  
-   Rôle de base de données  
  
-   Rôle d'application  
  
-   Utilisateur de base de données mappé à une connexion Windows  
  
-   Utilisateur de base de données mappé à un groupe Windows  
  
-   Utilisateur de base de données mappé à un certificat  
  
-   Utilisateur de base de données mappé à une clé asymétrique  
  
-   Utilisateur de base de données mappé à un principal de serveur  
  
## <a name="remarks"></a>Remarques  
  
## <a name="service-broker-contracts"></a>Contrats Service Broker  
 Un contrat [!INCLUDE[ssSB](../../includes/sssb-md.md)] est un élément sécurisable au niveau base de données, contenu dans la base de données qui est son parent dans la hiérarchie des autorisations. Les autorisations les plus spécifiques et limitées qu’il est possible de révoquer pour un contrat [!INCLUDE[ssSB](../../includes/sssb-md.md)] sont répertoriées dans le tableau ci-dessous, avec les autorisations plus générales, qui les incluent naturellement.  
  
|Autorisation d'un contrat Service Broker|Impliquée par une autorisation d'un contrat Service Broker|Impliquée par une autorisation de base de données|  
|----------------------------------------|---------------------------------------------------|------------------------------------|  
|CONTROL|CONTROL|CONTROL|  
|TAKE OWNERSHIP|CONTROL|CONTROL|  
|ALTER|CONTROL|ALTER ANY CONTRACT|  
|REFERENCES|CONTROL|REFERENCES|  
|VIEW DEFINITION|CONTROL|VIEW DEFINITION|  
  
## <a name="service-broker-message-types"></a>Types de messages Service Broker  
 Un type de message [!INCLUDE[ssSB](../../includes/sssb-md.md)] est un élément sécurisable du niveau base de données, contenu par la base de données qui est son parent dans la hiérarchie des autorisations. Les autorisations les plus spécifiques et limitées qui peuvent être révoquées sur un type de message [!INCLUDE[ssSB](../../includes/sssb-md.md)] sont indiquées dans le tableau suivant, avec les autorisations plus générales qui les incluent de manière implicite.  
  
|Autorisation d'un type de message Service Broker|Impliquée par une autorisation d'un type de message Service Broker|Impliquée par une autorisation de base de données|  
|--------------------------------------------|-------------------------------------------------------|------------------------------------|  
|CONTROL|CONTROL|CONTROL|  
|TAKE OWNERSHIP|CONTROL|CONTROL|  
|ALTER|CONTROL|ALTER ANY MESSAGE TYPE|  
|REFERENCES|CONTROL|REFERENCES|  
|VIEW DEFINITION|CONTROL|VIEW DEFINITION|  
  
## <a name="service-broker-remote-service-bindings"></a>Liaisons de service distant Service Broker  
 Une liaison de service distant [!INCLUDE[ssSB](../../includes/sssb-md.md)] est un élément sécurisable du niveau base de données, contenu par la base de données qui est son parent dans la hiérarchie des autorisations. Les autorisations les plus spécifiques et limitées qui peuvent être révoquées sur une liaison de service distant [!INCLUDE[ssSB](../../includes/sssb-md.md)] sont indiquées dans le tableau suivant, avec les autorisations plus générales qui les incluent de manière implicite.  
  
|Autorisation de liaison de service distant Service Broker|Impliquée par une autorisation de liaison de service distant Service Broker|Impliquée par une autorisation de base de données|  
|------------------------------------------------------|-----------------------------------------------------------------|------------------------------------|  
|CONTROL|CONTROL|CONTROL|  
|TAKE OWNERSHIP|CONTROL|CONTROL|  
|ALTER|CONTROL|ALTER ANY REMOTE SERVICE BINDING|  
|VIEW DEFINITION|CONTROL|VIEW DEFINITION|  
  
## <a name="service-broker-routes"></a>Itinéraires Service Broker  
 Un itinéraire [!INCLUDE[ssSB](../../includes/sssb-md.md)] est un élément sécurisable du niveau base de données, contenu par la base de données qui est son parent dans la hiérarchie des autorisations. Les autorisations les plus spécifiques et limitées qu’il est possible de révoquer pour un itinéraire [!INCLUDE[ssSB](../../includes/sssb-md.md)] sont répertoriées dans le tableau ci-dessous, avec les autorisations plus générales, qui les incluent naturellement.  
  
|Autorisation d'un itinéraire Service Broker|Impliquée par une autorisation d'un itinéraire Service Broker|Impliquée par une autorisation de base de données|  
|-------------------------------------|------------------------------------------------|------------------------------------|  
|CONTROL|CONTROL|CONTROL|  
|TAKE OWNERSHIP|CONTROL|CONTROL|  
|ALTER|CONTROL|ALTER ANY ROUTE|  
|VIEW DEFINITION|CONTROL|VIEW DEFINITION|  
  
### <a name="service-broker-services"></a>Services Service Broker  
 Un service [!INCLUDE[ssSB](../../includes/sssb-md.md)] est un élément sécurisable du niveau base de données, contenu par la base de données qui est son parent dans la hiérarchie des autorisations. Les autorisations les plus spécifiques et limitées qu’il est possible de révoquer pour un service [!INCLUDE[ssSB](../../includes/sssb-md.md)] sont répertoriées dans le tableau ci-dessous, avec les autorisations plus générales, qui les incluent naturellement.  
  
|Autorisation d'un service Service Broker|Impliquée par une autorisation d'un service Service Broker|Impliquée par une autorisation de base de données|  
|---------------------------------------|--------------------------------------------------|------------------------------------|  
|CONTROL|CONTROL|CONTROL|  
|TAKE OWNERSHIP|CONTROL|CONTROL|  
|SEND|CONTROL|CONTROL|  
|ALTER|CONTROL|ALTER ANY SERVICE|  
|VIEW DEFINITION|CONTROL|VIEW DEFINITION|  
  
## <a name="permissions"></a>Autorisations  
 Nécessite des autorisations CONTROL sur le contrat, le type de message, la liaison de service distant, l'itinéraire ou le service [!INCLUDE[ssSB](../../includes/sssb-md.md)].  
  
## <a name="see-also"></a>Voir aussi  
 [Autorisations GRANT dans Service Broker &#40;Transact-SQL&#41;](../../t-sql/statements/grant-service-broker-permissions-transact-sql.md)   
 [Autorisations DENY dans Service Broker &#40;Transact-SQL&#41;](../../t-sql/statements/deny-service-broker-permissions-transact-sql.md)   
 [GRANT &#40;Transact-SQL&#41;](../../t-sql/statements/grant-transact-sql.md)   
 [Autorisations &#40;moteur de base de données&#41;](../../relational-databases/security/permissions-database-engine.md)   
 [Principaux &#40;moteur de base de données&#41;](../../relational-databases/security/authentication-access/principals-database-engine.md)  
  
  
