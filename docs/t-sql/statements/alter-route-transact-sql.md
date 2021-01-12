---
description: ALTER ROUTE (Transact-SQL)
title: ALTER ROUTE (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/30/2018
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- ALTER_ROUTE_TSQL
- ALTER ROUTE
dev_langs:
- TSQL
helpviewer_keywords:
- ALTER ROUTE statement
- dropping routes
- modifying routes
- removing routes
- routes [Service Broker], modifying
ms.assetid: 8dfb7b16-3dac-4e1e-8c97-adf2aad07830
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-mi-current||>=sql-server-2016||>=sql-server-linux-2017
ms.openlocfilehash: 504f1eed0f50a711a671301b10001b44d6097330
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/11/2021
ms.locfileid: "98092035"
---
# <a name="alter-route-transact-sql"></a>ALTER ROUTE (Transact-SQL)
[!INCLUDE [SQL Server - ASDBMI](../../includes/applies-to-version/sql-asdbmi.md)]

  Modifie les informations relatives à un itinéraire existant dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. 

  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql
ALTER ROUTE route_name  
WITH    
  [ SERVICE_NAME = 'service_name' [ , ] ]  
  [ BROKER_INSTANCE = 'broker_instance' [ , ] ]  
  [ LIFETIME = route_lifetime [ , ] ]  
  [ ADDRESS =  'next_hop_address' [ , ] ]  
  [ MIRROR_ADDRESS = 'next_hop_mirror_address' ]  
[ ; ]  
  
```  
  

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments
 *route_name*  
 Nom de l'itinéraire à modifier. Les noms du serveur, de la base de données et du schéma ne peuvent pas être spécifiés.  
  
 WITH  
 Introduit les clauses qui définissent l'itinéraire modifié.  
  
 SERVICE_NAME **='** _service\_name_ **'**  
 Spécifie le nom du service distant vers lequel pointe cet itinéraire. *service_name* doit correspondre exactement au nom utilisé par le service distant. [!INCLUDE[ssSB](../../includes/sssb-md.md)] utilise une comparaison octet par octet pour la concordance avec la chaîne *service_name*. En d'autres termes, la comparaison respecte la casse et ne prend pas en compte le classement actuel. Un itinéraire dont le nom de service est **« SQL/ServiceBroker/BrokerConfiguration »** est un itinéraire vers un service de notification de la configuration de Service Broker. Un itinéraire vers ce service ne peut pas spécifier d'instance de Service Broker.  
  
 Si la clause SERVICE_NAME est omise, le nom de service pour l'itinéraire reste le même.  
  
 BROKER_INSTANCE **='** _broker\_instance_ **'**  
 Spécifie la base de données qui héberge le service cible. Le paramètre *broker_instance* doit être l’identificateur de l’instance Service Broker pour la base de données distante et peut être obtenu en exécutant la requête suivante dans la base de données sélectionnée :  
  
```sql  
SELECT service_broker_guid  
FROM sys.databases  
WHERE database_id = DB_ID();  
```  
  
 Si la clause BROKER_INSTANCE est omise, l'instance de Service Broker pour l'itinéraire reste la même.  
  
> [!NOTE]  
>  Cette option n'est pas disponible dans une base de données autonome.  
  
 LIFETIME **=** _route\_lifetime_  
 Spécifie la durée, en secondes, pendant laquelle [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] conserve l'itinéraire dans la table de routage. Lorsque la durée de vie expire, l'itinéraire expire et [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] n'en tient plus compte lors de la sélection d'un itinéraire pour une nouvelle conversation. Si cette clause est omise, la durée de vie de l'itinéraire reste la même.  
  
 ADDRESS **='** _next\_hop\_address_'  

 Pour Azure SQL Managed Instance, `ADDRESS` doit être local.

 Spécifie l'adresse réseau pour cet itinéraire. *next_hop_address* spécifie une adresse TCP/IP au format suivant :  
  
 **TCP://** { *dns_name* | *netbios_name* |*ip_address* } **:** *port_number*  
  
 Le *port_number* spécifié doit correspondre au numéro de port du point de terminaison de [!INCLUDE[ssSB](../../includes/sssb-md.md)] pour une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sur l’ordinateur spécifié. Cela peut être obtenu en exécutant la requête ci-après dans la base de données sélectionnée :  
  
```sql  
SELECT tcpe.port  
FROM sys.tcp_endpoints AS tcpe  
INNER JOIN sys.service_broker_endpoints AS ssbe  
   ON ssbe.endpoint_id = tcpe.endpoint_id  
WHERE ssbe.name = N'MyServiceBrokerEndpoint';  
```  
  
 Si un itinéraire spécifie **'LOCAL'** pour le paramètre *next_hop_address*, le message est remis à un service de l’instance actuelle de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 Si un itinéraire spécifie **'TRANSPORT'** pour le paramètre *next_hop_address*, l’adresse réseau est déterminée en fonction de l’adresse réseau spécifiée dans le nom du service. Un itinéraire qui spécifie **'TRANSPORT'** peut spécifier un nom de service ou d’instance Service Broker.  
  
 Quand *next_hop_address* est le serveur principal d’une base de données miroir, vous devez également spécifier la clause MIRROR_ADDRESS pour le serveur miroir. Sinon, cet itinéraire ne bascule pas automatiquement sur le serveur miroir.  
  
> [!NOTE]  
>  Cette option n'est pas disponible dans une base de données autonome.  
  
 MIRROR_ADDRESS **='** _next\_hop\_mirror\_address_ **'**  
 Spécifie l’adresse réseau pour le serveur miroir d’une paire mise en miroir dont le serveur principal se trouve à l’adresse *next_hop_address*. *next_hop_mirror_address* spécifie une adresse TCP/IP au format suivant :  
  
 **TCP://** { *dns_name* | *netbios_name* | *ip_address* } **:** *port_number*  
  
 Le *port_number* spécifié doit correspondre au numéro de port du point de terminaison de [!INCLUDE[ssSB](../../includes/sssb-md.md)] pour une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sur l’ordinateur spécifié. Cela peut être obtenu en exécutant la requête ci-après dans la base de données sélectionnée :  
  
```sql  
SELECT tcpe.port  
FROM sys.tcp_endpoints AS tcpe  
INNER JOIN sys.service_broker_endpoints AS ssbe  
   ON ssbe.endpoint_id = tcpe.endpoint_id  
WHERE ssbe.name = N'MyServiceBrokerEndpoint';  
```  
  
 Si la clause MIRROR_ADDRESS est spécifiée, l'itinéraire doit spécifier les clauses SERVICE_NAME et BROKER_INSTANCE. Un itinéraire qui spécifie **'LOCAL'** ou **'TRANSPORT'** pour le paramètre *next_hop_address* ne peut pas spécifier une adresse miroir.  
  
> [!NOTE]  
>  Cette option n'est pas disponible dans une base de données autonome.  
  
## <a name="remarks"></a>Notes  
 La table de routage qui stocke les itinéraires est une table de métadonnées. Elle peut être lue dans la vue de catalogue **sys.routes**. La mise à jour de la table de routage s'effectue uniquement au moyen des instructions CREATE ROUTE, ALTER ROUTE et DROP ROUTE.  
  
 Les clauses qui ne sont pas spécifiées dans la commande ALTER ROUTE restent inchangées. Par conséquent, il n'est pas possible de MODIFIER un itinéraire pour spécifier que ce dernier n'expire pas, qu'il correspond à n'importe quel nom de service ou à n'importe quelle instance de Service Broker. Pour modifier ces caractéristiques d'un itinéraire, vous devez supprimer l'itinéraire existant et en créer un nouveau avec les informations mises à jour.  
  
 Si un itinéraire spécifie **'TRANSPORT'** pour le paramètre *next_hop_address*, l’adresse réseau est déterminée en fonction du nom du service. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] traite les noms de services qui commencent par une adresse réseau dans un format valide pour *next_hop_address*. Les services dont les noms contiennent des adresses réseau valides seront acheminés vers l'adresse réseau spécifiée dans le nom de service.  
  
 La table de routage peut contenir un nombre illimité d'itinéraires qui spécifient les mêmes service, adresse réseau et/ou identificateur d'instance de Service Broker. Dans ce cas, [!INCLUDE[ssSB](../../includes/sssb-md.md)] sélectionne un itinéraire à l'aide d'une procédure conçue pour rechercher la correspondance la plus précise entre les informations spécifiées dans la conversation et celles de la table de routage.  
  
 L'instruction ALTER AUTHORIZATION permet de modifier l'AUTORISATION pour un service.  
  
## <a name="permissions"></a>Autorisations  
 L’autorisation de modification d’un itinéraire revient par défaut au propriétaire de l’itinéraire, aux membres du rôle de base de données fixe **db_ddladmin** ou **db_owner** et aux membres du rôle serveur fixe **sysadmin**.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-changing-the-service-for-a-route"></a>R. Modification du service d'un itinéraire  
 L'exemple suivant l'itinéraire `ExpenseRoute` et le fait pointer vers le service distant `//Adventure-Works.com/Expenses`.  
  
```sql  
ALTER ROUTE ExpenseRoute  
   WITH   
     SERVICE_NAME = '//Adventure-Works.com/Expenses';  
```  
  
### <a name="b-changing-the-target-database-for-a-route"></a>B. Modification de la base de données cible d'un itinéraire  
 L'exemple suivant change la base de données cible de l'itinéraire `ExpenseRoute` en base de données identifiée par l'identificateur unique `D8D4D268-00A3-4C62-8F91-634B89B1E317.`.  
  
```sql  
ALTER ROUTE ExpenseRoute  
   WITH   
     BROKER_INSTANCE = 'D8D4D268-00A3-4C62-8F91-634B89B1E317';  
```  
  
### <a name="c-changing-the-address-for-a-route"></a>C. Modification de l'adresse d'un itinéraire  
 L'exemple suivant change l'adresse réseau de l'itinéraire `ExpenseRoute` en port TCP `1234` sur l'hôte dont l'adresse IP est `10.2.19.72`.  
  
```sql  
ALTER ROUTE ExpenseRoute   
   WITH   
     ADDRESS = 'TCP://10.2.19.72:1234';  
```  
  
### <a name="d-changing-the-database-and-address-for-a-route"></a>D. Modification de la base de données et de l'adresse d'un itinéraire  
 L'exemple suivant change l'adresse réseau de l'itinéraire `ExpenseRoute` en port TCP `1234` sur l'hôte dont le nom DNS est `www.Adventure-Works.com`. Il remplace également la base de données cible par la base de données identifiée par l'identificateur unique `D8D4D268-00A3-4C62-8F91-634B89B1E317`.  
  
```sql  
ALTER ROUTE ExpenseRoute  
   WITH   
     BROKER_INSTANCE = 'D8D4D268-00A3-4C62-8F91-634B89B1E317',  
     ADDRESS = 'TCP://www.Adventure-Works.com:1234';  
```  
  
## <a name="see-also"></a>Voir aussi  
 [CREATE ROUTE &#40;Transact-SQL&#41;](../../t-sql/statements/create-route-transact-sql.md)   
 [DROP ROUTE &#40;Transact-SQL&#41;](../../t-sql/statements/drop-route-transact-sql.md)   
 [EVENTDATA &#40;Transact-SQL&#41;](../../t-sql/functions/eventdata-transact-sql.md)  
  
  
