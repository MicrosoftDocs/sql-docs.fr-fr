---
description: CREATE ROUTE (Transact-SQL)
title: CREATE ROUTE (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/30/2018
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- CREATE_ROUTE_TSQL
- ROUTE
- CREATE ROUTE
- ROUTE_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- lifetimes [Service Broker]
- routes [Service Broker], creating
- forwarding brokers [SQL Server]
- service names [Service Broker]
- database mirroring [SQL Server], routing
- expired routes [SQL Server]
- activating routes
- CREATE ROUTE statement
ms.assetid: 7e695364-1a98-4cfd-8ebd-137ac5a425b3
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-mi-current||>=sql-server-2016||>=sql-server-linux-2017
ms.openlocfilehash: fc34a3eb5987a7348733755ee69aafda87e16ac3
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/11/2021
ms.locfileid: "98091956"
---
# <a name="create-route-transact-sql"></a>CREATE ROUTE (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-asdbmi-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-asdbmi-xxxx-xxx-md.md )]

  Ajoute un itinéraire à la table de routage de la base de données active. Pour les messages sortants, [!INCLUDE[ssSB](../../includes/sssb-md.md)] détermine l'itinéraire en vérifiant la table de routage dans la base de données locale. Pour les messages de conversations issus d’une autre instance, notamment les messages à transférer, [!INCLUDE[ssSB](../../includes/sssb-md.md)] vérifie les itinéraires dans la base de données **msdb**.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql
CREATE ROUTE route_name  
[ AUTHORIZATION owner_name ]  
WITH    
   [ SERVICE_NAME = 'service_name', ]  
   [ BROKER_INSTANCE = 'broker_instance_identifier' , ]  
   [ LIFETIME = route_lifetime , ]  
   ADDRESS =  'next_hop_address'  
   [ , MIRROR_ADDRESS = 'next_hop_mirror_address' ]  
[ ; ]  
```  
  
## <a name="arguments"></a>Arguments  
 *route_name*  
 Nom de l’itinéraire à créer. Un nouvel itinéraire est créé dans la base de données active et détenu par le principal spécifié dans la clause AUTHORIZATION. Les noms du serveur, de la base de données et du schéma ne peuvent pas être spécifiés. *route_name* doit être un **sysname** valide.  
  
 AUTHORIZATION *owner_name*  
 Définit le propriétaire de l'itinéraire pour l'utilisateur ou le rôle de base de données spécifié. *owner_name* peut correspondre au nom de n’importe quel utilisateur ou rôle valide quand l’utilisateur actif est membre du rôle de base de données fixe **db_owner** ou du rôle serveur fixe **sysadmin**. Sinon, *owner_name* doit être le nom de l’utilisateur actuel, le nom d’un utilisateur pour lequel l’utilisateur actuel a l’autorisation IMPERSONATE ou le nom d’un rôle auquel appartient l’utilisateur actuel. Si cette clause est omise, l'itinéraire appartient à l'utilisateur en cours.  
  
 WITH  
 Introduit les clauses qui définissent l'itinéraire créé.  
  
 SERVICE_NAME = **'** _service\_name_ **'**  
 Spécifie le nom du service distant vers lequel pointe cet itinéraire. *service_name* doit correspondre exactement au nom utilisé par le service distant. [!INCLUDE[ssSB](../../includes/sssb-md.md)] utilise une comparaison octet par octet pour la concordance avec la chaîne *service_name*. En d'autres termes, la comparaison respecte la casse et ne prend pas en compte le classement actuel. Si la clause SERVICE_NAME est omise, cet itinéraire correspond à n'importe quel nom de service, mais la priorité affectée à la correspondance est inférieure à celle d'un itinéraire pour lequel la clause SERVICE_NAME est spécifiée. Un itinéraire dont le nom de service est **« SQL/ServiceBroker/BrokerConfiguration »** est un itinéraire vers un service de notification de la configuration de Service Broker. Un itinéraire vers ce service ne peut pas spécifier d'instance de Service Broker.  
  
 BROKER_INSTANCE = **'** _broker\_instance\_identifier_ **'**  
 Spécifie la base de données qui héberge le service cible. Le paramètre *broker_instance_identifier* doit être l’identificateur de l’instance Service Broker pour la base de données distante et peut être obtenu en exécutant la requête suivante dans la base de données sélectionnée :  
  
```sql  
SELECT service_broker_guid  
FROM sys.databases  
WHERE database_id = DB_ID()  
```  
  
 Si la clause BROKER_INSTANCE est omise, cet itinéraire correspond à n'importe quelle instance Service Broker. La priorité d'un itinéraire qui correspond à n'importe quelle instance de service Broker est plus élevée que celle d'un itinéraire pour lequel une instance service Broker explicite est spécifiée lorsque la conversation ne spécifie aucune instance Service Broker. Pour les conversations spécifiant une instance service Broker, la priorité d'un itinéraire pour lequel une instance Service Broker est spécifiée est plus élevée que celle d'un itinéraire qui correspond à n'importe quelle instance de Service Broker.  
  
 LIFETIME **=** _route\_lifetime_  
 Spécifie la durée, en secondes, pendant laquelle [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] conserve l'itinéraire dans la table de routage. Lorsque la durée de vie expire, l'itinéraire expire et [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] n'en tient plus compte lors de la sélection d'un itinéraire pour une nouvelle conversation. Si cette clause est omise, *route_lifetime* est NULL et l’itinéraire n’expire jamais.  
  
 ADDRESS **='** _next\_hop\_address_ **'**  
Pour SQL Managed Instance, `ADDRESS` doit être local. 

Spécifie l'adresse réseau pour cet itinéraire. *next_hop_address* spécifie une adresse TCP/IP au format suivant :  
  
 **TCP://** { *dns_name* | *netbios_name* | *ip_address* } **:** _port\_number_  
  
 Le *port_number* spécifié doit correspondre au numéro de port du point de terminaison de [!INCLUDE[ssSB](../../includes/sssb-md.md)] pour une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sur l’ordinateur spécifié. Cela peut être obtenu en exécutant la requête ci-après dans la base de données sélectionnée :  
  
```sql  
SELECT tcpe.port  
FROM sys.tcp_endpoints AS tcpe  
INNER JOIN sys.service_broker_endpoints AS ssbe  
   ON ssbe.endpoint_id = tcpe.endpoint_id  
WHERE ssbe.name = N'MyServiceBrokerEndpoint';  
```  
  
 Si le service est hébergé dans une base de données miroir, vous devez également spécifier la clause MIRROR_ADDRESS pour l'autre instance qui héberge une base de données miroir. Sinon, cet itinéraire ne bascule pas sur la base de données miroir.  
  
 Si un itinéraire spécifie **'LOCAL'** pour le paramètre *next_hop_address*, le message est remis à un service de l’instance actuelle de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 Si un itinéraire spécifie **'TRANSPORT'** pour le paramètre *next_hop_address*, l’adresse réseau est déterminée en fonction de l’adresse réseau spécifiée dans le nom du service. Un itinéraire qui spécifie **'TRANSPORT'** ne peut pas spécifier un nom de service ou une instance Service Broker.  
  
 MIRROR_ADDRESS **='** _next\_hop\_mirror\_address_ **'**  
 Spécifie l’adresse réseau d’une base de données miroir et d’une base de données miroir hébergée à l’adresse *next_hop_address*. *next_hop_mirror_address* spécifie une adresse TCP/IP au format suivant :  
  
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
  
## <a name="remarks"></a>Notes  
 La table de routage qui stocke les itinéraires est une table de métadonnées consultable dans la vue de catalogue **sys.routes**. Cet affichage catalogue peut être mis à niveau uniquement à l'aide des instructions CREATE ROUTE, ALTER ROUTE et DROP ROUTE.  
  
 Par défaut, la table de routage de chaque base de données utilisateur ne contient qu'un seul itinéraire. Cet itinéraire est nommée **AutoCreatedLocal**. L’itinéraire spécifie **'LOCAL'** pour le paramètre *next_hop_address* et correspond à n’importe quel nom de service et identificateur d’instance de Service Broker.  
  
 Si un itinéraire spécifie **'TRANSPORT'** pour le paramètre *next_hop_address*, l’adresse réseau est déterminée en fonction du nom du service. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] traite les noms de services qui commencent par une adresse réseau dans un format valide pour *next_hop_address*.  
  
 La table de routage peut contenir un nombre illimité d'itinéraires qui spécifient les mêmes service, adresse réseau et identificateur d'instance de Service Broker. Dans ce cas, [!INCLUDE[ssSB](../../includes/sssb-md.md)] sélectionne un itinéraire à l'aide d'une procédure conçue pour rechercher la correspondance la plus précise entre les informations spécifiées dans la conversation et celles de la table de routage.  
  
 [!INCLUDE[ssSB](../../includes/sssb-md.md)] ne supprime pas les itinéraires qui ont expiré de la table de routage. Un itinéraire expiré peut être activé à l'aide de l'instruction ALTER ROUTE.  
  
 Un itinéraire ne peut pas être un objet temporaire. Les noms d’itinéraires commençant par **#** sont autorisés, mais sont des objets permanents.  
  
## <a name="permissions"></a>Autorisations  
 L’autorisation de création d’un itinéraire revient par défaut aux membres du rôle de base de données fixe **db_ddladmin** ou **db_owner** et aux membres du rôle serveur fixe **sysadmin**.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-creating-a-tcpip-route-by-using-a-dns-name"></a>R. Création d'un itinéraire TCP/IP à l'aide d'un nom DNS  
 L'exemple suivant crée un itinéraire vers le service `//Adventure-Works.com/Expenses`. L'itinéraire spécifie que les messages vers ce service utilisent le protocole TCP jusqu'au port `1234` de l'hôte identifié par le nom DNS `www.Adventure-Works.com`. Le serveur cible remet les messages au fur et à mesure de leur réception à l'instance de Service Broker identifiée par l'identificateur unique `D8D4D268-00A3-4C62-8F91-634B89C1E315`.  
  
```sql 
CREATE ROUTE ExpenseRoute  
    WITH  
    SERVICE_NAME = '//Adventure-Works.com/Expenses',  
    BROKER_INSTANCE = 'D8D4D268-00A3-4C62-8F91-634B89C1E315',  
    ADDRESS = 'TCP://www.Adventure-Works.com:1234' ;  
```  
  
### <a name="b-creating-a-tcpip-route-by-using-a-netbios-name"></a>B. Création d'un itinéraire TCP/IP à l'aide d'un nom NetBIOS  
 L'exemple suivant crée un itinéraire vers le service `//Adventure-Works.com/Expenses`. L'itinéraire spécifie que les messages vers ce service utilisent le protocole TCP jusqu'au port `1234` de l'hôte identifié par le nom NetBIOS `SERVER02`. Au fur et à mesure de leur réception, le serveur cible [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] remet les messages à l'instance de base de données identifiée par l'identificateur unique `D8D4D268-00A3-4C62-8F91-634B89C1E315`.  
  
```sql  
CREATE ROUTE ExpenseRoute  
    WITH   
    SERVICE_NAME = '//Adventure-Works.com/Expenses',  
    BROKER_INSTANCE = 'D8D4D268-00A3-4C62-8F91-634B89C1E315',  
    ADDRESS = 'TCP://SERVER02:1234' ;  
```  
  
### <a name="c-creating-a-tcpip-route-by-using-an-ip-address"></a>C. Création d'un itinéraire TCP/IP à l'aide d'une adresse IP  
 L'exemple suivant crée un itinéraire vers le service `//Adventure-Works.com/Expenses`. L'itinéraire spécifie que les messages vers ce service utilisent le protocole TCP jusqu'au port `1234` de l'hôte identifié par l'adresse IP `192.168.10.2`. Au fur et à mesure de leur réception, le serveur cible [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] remet les messages à l'instance Service Broker identifiée par l'identificateur unique `D8D4D268-00A3-4C62-8F91-634B89C1E315`.  
  
```sql  
CREATE ROUTE ExpenseRoute  
    WITH  
    SERVICE_NAME = '//Adventure-Works.com/Expenses',  
    BROKER_INSTANCE = 'D8D4D268-00A3-4C62-8F91-634B89C1E315',  
    ADDRESS = 'TCP://192.168.10.2:1234' ;  
```  
  
### <a name="d-creating-a-route-to-a-forwarding-broker"></a>D. Création d'un itinéraire vers un Service Broker de transfert  
 L'exemple suivant crée un itinéraire vers le Service Broker de transfert du serveur `dispatch.Adventure-Works.com`. Étant donné que le nom du service et l'identificateur de l'instance de Service Broker ne sont pas spécifiés, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] utilise cet itinéraire pour les services pour lesquels aucun autre itinéraire n'est défini.  
  
```sql  
CREATE ROUTE ExpenseRoute  
    WITH  
    ADDRESS = 'TCP://dispatch.Adventure-Works.com' ;   
```  
  
### <a name="e-creating-a-route-to-a-local-service"></a>E. Création d'un itinéraire vers un service local  
 L'exemple suivant crée un itinéraire vers le service `//Adventure-Works.com/LogRequests` dans la même instance que l'itinéraire.  
  
```sql  
CREATE ROUTE LogRequests  
    WITH  
    SERVICE_NAME = '//Adventure-Works.com/LogRequests',  
    ADDRESS = 'LOCAL' ;  
```  
  
### <a name="f-creating-a-route-with-a-specified-lifetime"></a>F. Création d'un itinéraire avec une durée de vie spécifique  
 L'exemple suivant crée un itinéraire vers le service `//Adventure-Works.com/Expenses`. La durée de vie de l'itinéraire est de `259200` secondes, soit 72 heures.  
  
```sql  
CREATE ROUTE ExpenseRoute  
    WITH  
    SERVICE_NAME = '//Adventure-Works.com/Expenses',  
    LIFETIME = 259200,  
    ADDRESS = 'TCP://services.Adventure-Works.com:1234' ;  
```  
  
### <a name="g-creating-a-route-to-a-mirrored-database"></a>G. Création d'un itinéraire vers une base de données miroir  
 L'exemple suivant crée un itinéraire vers le service `//Adventure-Works.com/Expenses`. Le service est hébergé par une base de données miroir. Les bases de données miroir se trouvent aux adresses suivantes : `services.Adventure-Works.com:1234` et `services-mirror.Adventure-Works.com:1234`.  
  
```sql  
CREATE ROUTE ExpenseRoute  
    WITH  
    SERVICE_NAME = '//Adventure-Works.com/Expenses',  
    BROKER_INSTANCE = '69fcc80c-2239-4700-8437-1001ecddf933',  
    ADDRESS = 'TCP://services.Adventure-Works.com:1234',   
    MIRROR_ADDRESS = 'TCP://services-mirror.Adventure-Works.com:1234' ;  
```  
  
### <a name="h-creating-a-route-that-uses-the-service-name-for-routing"></a>H. Création d'un itinéraire qui utilise le nom du service pour le routage  
 L'exemple suivant crée un itinéraire qui utilise le nom du service pour déterminer l'adresse réseau d'envoi des messages. La priorité d'un itinéraire qui spécifie `'TRANSPORT'` comme adresse réseau est inférieure à celle des autres itinéraires.  
  
```sql  
CREATE ROUTE TransportRoute  
    WITH ADDRESS = 'TRANSPORT' ;  
```  
  
## <a name="see-also"></a>Voir aussi  
 [ALTER ROUTE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-route-transact-sql.md)   
 [DROP ROUTE &#40;Transact-SQL&#41;](../../t-sql/statements/drop-route-transact-sql.md)   
 [EVENTDATA &#40;Transact-SQL&#41;](../../t-sql/functions/eventdata-transact-sql.md)  
  
  
