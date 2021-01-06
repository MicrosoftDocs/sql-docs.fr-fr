---
title: Spécifier l’URL du point de terminaison pour le réplica de disponibilité
description: Découvrez comment spécifier l’URL du point de terminaison lors de l’ajout ou de la modification d’un réplica dans un groupe de disponibilité Always On sur SQL Server.
ms.custom: seo-lt-2019
ms.date: 05/17/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: conceptual
helpviewer_keywords:
- endpoints [SQL Server], AlwaysOn Availability Groups
- Availability Groups [SQL Server], configuring
- Availability Groups [SQL Server], endpoint
- Endpoint URLs (HADR)
ms.assetid: d7520c13-a8ee-4ddc-9e9a-54cd3d27ef1c
author: cawrites
ms.author: chadam
ms.openlocfilehash: 56da078fe1133461cee4ff7069f07034d3543c79
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/17/2020
ms.locfileid: "97643989"
---
# <a name="specify-endpoint-url---adding-or-modifying-availability-replica"></a>Spécifier l’URL de point de terminaison - Ajout ou modification d’un réplica de disponibilité
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  Pour héberger un réplica de disponibilité pour un groupe de disponibilité, une instance de serveur doit posséder un point de terminaison de mise en miroir de bases de données. L'instance de serveur utilise ce point de terminaison pour écouter les messages [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] des réplicas de disponibilité hébergés par d'autres instances de serveur. Pour définir un réplica de disponibilité pour un groupe de disponibilité, vous devez spécifier l'URL de point de terminaison de l'instance de serveur qui hébergera le réplica. L’*URL de point de terminaison* identifie le protocole de transport du point de terminaison de mise en miroir de bases de données (TCP), l’adresse système de l’instance de serveur et le numéro de port associé au point de terminaison.  
  
> [!NOTE]  
>  Le terme « URL de point de terminaison » est synonyme du terme « adresse réseau du serveur » qui est utilisé dans l'interface utilisateur et la documentation de la mise en miroir de bases de données.  
  
  
##  <a name="syntax-for-an-endpoint-url"></a><a name="SyntaxOfURL"></a> Syntaxe pour une URL de point de terminaison  
 La syntaxe d'une URL de point de terminaison est la suivante :  
  
 TCP <strong>://</strong> *\<system-address>* <strong>:</strong> *\<port>*  
  
 where  
  
-   *\<system-address>* est une chaîne qui identifie sans ambiguïté le système informatique cible. En règle générale, l'adresse de serveur est un nom système (si les systèmes sont dans le même domaine), un nom de domaine complet ou une adresse IP :  
  
    -   Étant donné que les nœuds du cluster WSFC (clustering de basculement Windows Server) figurent dans le même domaine, vous pouvez utiliser le nom du système informatique, par exemple `SYSTEM46`.  
  
    -   Pour pouvoir utiliser une adresse IP, elle doit être unique dans votre environnement. Nous vous recommandons d'utiliser une adresse IP seulement si elle est statique. L'adresse IP peut être une adresse IP Version 4 (IPv4) ou IP Version 6 (IPv6). Une adresse IPv6 doit être placée entre crochets, par exemple : **[** _<adresse_IPv6>_ **]** .  
  
         Pour connaître l'adresse IP d'un système, à l'invite de commandes Windows, entrez la commande **ipconfig** .  
  
    -   L'utilisation du nom de domaine complet garantit un fonctionnement correct. Il s'agit d'une chaîne d'adresse définie localement qui prend des formes différentes dans des emplacements différents. Souvent, mais pas systématiquement, un nom de domaine complet correspond à un nom composé qui inclut le nom de l'ordinateur et une série de segments de domaine séparés par des points, de la forme :  
  
         _nom_ordinateur_ **.** _segment_domaine_[... **.** _segment_domaine_]  
  
         où *nom_ordinateur* correspond au nom réseau de l’ordinateur qui exécute l’instance de serveur et *segment_domaine*[... **.** _segment_domaine_] représente les autres informations de domaine du serveur ; par exemple : `localinfo.corp.Adventure-Works.com`.  
  
         Le contenu et le nombre de segments de domaine sont déterminés au sein de la société ou de l'organisation. Pour plus d'informations, consultez [Recherche du nom de domaine complet](#Finding_FQDN), plus loin dans cette rubrique.  
  
-   *\<port>* est le numéro de port utilisé par le point de terminaison de mise en miroir de l'instance de serveur partenaire.  
  
     Un point de terminaison de mise en miroir de bases de données peut utiliser tout port disponible sur le système informatique. Chaque numéro de port doit être associé à un seul point de terminaison, et chaque point de terminaison est associé à une seule instance de serveur ; ainsi, différentes instances de serveurs sur le même serveur écoutent sur différents points de terminaison dotés de différents ports. Par conséquent, le port que vous précisez dans l'URL de point de terminaison lorsque vous spécifiez un réplica de disponibilité dirigera toujours les messages entrants à l'instance de serveur dont le point de terminaison est associé à ce port.  
  
     Dans l'URL de point de terminaison, seul le numéro du port identifie l'instance de serveur associée au point de terminaison de mise en miroir sur l'ordinateur cible. L'illustration suivante présente les URL de point de terminaison de deux instances de serveurs sur un même ordinateur. L'instance par défaut utilise le port `7022` et l'instance nommée utilise le port `7033`. L'URL de point de terminaison de ces deux instances de serveur sont, respectivement : `TCP://MYSYSTEM.Adventure-works.MyDomain.com:7022` et `TCP://MYSYSTEM.Adventure-works.MyDomain.com:7033`. Notez que l'adresse ne contient pas le nom de l'instance du serveur.  
  
     ![Adresses réseau de serveur d’une instance par défaut](../../../database-engine/availability-groups/windows/media/dbm-2-instances-ports-1-system.gif "Adresses réseau de serveur d’une instance par défaut")  
  
     Pour identifier le port actuellement associé au point de terminaison de mise en miroir de bases de données d'une instance de serveur, utilisez l'instruction [!INCLUDE[tsql](../../../includes/tsql-md.md)] suivante :  
  
    ```  
    SELECT type_desc, port FROM sys.TCP_endpoints  
    ```  
  
     Recherchez la ligne dont la valeur **type_desc** est « DATABASE_MIRRORING » et utilisez le numéro de port correspondant.  
  
### <a name="examples"></a>Exemples  
  
#### <a name="a-using-a-system-name"></a>R. Utilisation d'un nom système  
 L'URL de point de terminaison suivante spécifie un nom système, `SYSTEM46`, et le port `7022`.  
  
 `TCP://SYSTEM46:7022`  
  
#### <a name="b-using-a-fully-qualified-domain-name"></a>B. Utilisation d'un nom de domaine complet  
 L'URL de point de terminaison suivante spécifie un nom de domaine complet, `DBSERVER8.manufacturing.Adventure-Works.com`, et le port `7024`.  
  
 `TCP://DBSERVER8.manufacturing.Adventure-Works.com:7024`  
  
#### <a name="c-using-ipv4"></a>C. Utilisation d'une adresse IPv4  
 L'URL de point de terminaison suivante spécifie une adresse IPv4, `10.193.9.134`, et le port `7023`.  
  
 `TCP://10.193.9.134:7023`  
  
#### <a name="d-using-ipv6"></a>D. Utilisation d'une adresse IPv6  
 L'URL de point de terminaison suivante contient une adresse IPv6, `2001:4898:23:1002:20f:1fff:feff:b3a3`, et le port `7022`.  
  
 `TCP://[2001:4898:23:1002:20f:1fff:feff:b3a3]:7022`  
  
##  <a name="finding-the-fully-qualified-domain-name-of-a-system"></a><a name="Finding_FQDN"></a> Recherche du nom de domaine complet d'un système  
 Pour rechercher le nom de domaine complet d'un système, à l'invite de commandes Windows de ce système, entrez :  
  
 **IPCONFIG /ALL**  
  
 Pour former le nom de domaine complet, concaténez les valeurs de *<host_name>* et *<Primary_Dns_Suffix>* de la manière suivante :  
  
 _<host_name>_ **.** _<Suffixe_DNS_principal>_  
  
 Par exemple, la configuration IP  
  
 `Host Name  .  .  .  .  .  .  : MYSERVER`  
  
 `Primary Dns Suffix  .  .  .  : mydomain.Adventure-Works.com`  
  
 équivaut au nom de domaine complet suivant :  
  
 `MYSERVER.mydomain.Adventure-Works.com`  
  
> [!NOTE]  
>  Pour obtenir des informations supplémentaires sur un nom de domaine complet, contactez votre administrateur système.  
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> Tâches associées  
 **Pour configurer un point de terminaison de mise en miroir de bases de données**  
  
-   [Créer un point de terminaison de mise en miroir de bases de données pour les groupes de disponibilité Always On &#40;SQL Server PowerShell&#41;](../../../database-engine/availability-groups/windows/database-mirroring-always-on-availability-groups-powershell.md)  
  
-   [Créer un point de terminaison de mise en miroir de bases de données pour l’authentification Windows &#40;Transact-SQL&#41;](../../../database-engine/database-mirroring/create-a-database-mirroring-endpoint-for-windows-authentication-transact-sql.md)  
  
-   [Utiliser des certificats pour un point de terminaison de mise en miroir de bases de données &#40;Transact-SQL&#41;](../../../database-engine/database-mirroring/use-certificates-for-a-database-mirroring-endpoint-transact-sql.md)  
  
    -   [Autoriser un point de terminaison de mise en miroir de bases de données à utiliser des certificats pour les connexions sortantes &#40;Transact-SQL&#41;](../../../database-engine/database-mirroring/database-mirroring-use-certificates-for-outbound-connections.md)  
  
    -   [Autoriser un point de terminaison de mise en miroir de bases de données à utiliser des certificats pour les connexions entrantes &#40;Transact-SQL&#41;](../../../database-engine/database-mirroring/database-mirroring-use-certificates-for-inbound-connections.md)  
  
-   [Spécifier une adresse réseau de serveur &#40;mise en miroir de bases de données&#41;](../../../database-engine/database-mirroring/specify-a-server-network-address-database-mirroring.md)  
  
-   Spécifier l'URL de point de terminaison lors de l'ajout ou lors de la modification d'un réplica de disponibilité (SQL Server)  
  
-   [Résoudre des problèmes de configuration des groupes de disponibilité Always On &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/troubleshoot-always-on-availability-groups-configuration-sql-server.md)  
  
 **Pour afficher des informations sur le point de terminaison de mise en miroir de bases de données**  
  
-   [sys.database_mirroring_endpoints &#40;Transact-SQL&#41;](../../../relational-databases/system-catalog-views/sys-database-mirroring-endpoints-transact-sql.md)  
  
 **Pour ajouter un réplica de disponibilité**  
  
-   [Ajouter un réplica secondaire à un groupe de disponibilité &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/add-a-secondary-replica-to-an-availability-group-sql-server.md)  
  
-   [Joindre un réplica secondaire à un groupe de disponibilité &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/join-a-secondary-replica-to-an-availability-group-sql-server.md)  
  
##  <a name="related-content"></a><a name="RelatedContent"></a> Contenu associé  
  
-   [Guide de solutions Microsoft SQL Server Always On pour la haute disponibilité et la récupération d’urgence](/previous-versions/sql/sql-server-2012/hh781257(v=msdn.10))  
  
## <a name="see-also"></a>Voir aussi  
 [Création et configuration des groupes de disponibilité &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/creation-and-configuration-of-availability-groups-sql-server.md)   
 [Vue d’ensemble des groupes de disponibilité Always On &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)   
 [CREATE ENDPOINT &#40;Transact-SQL&#41;](../../../t-sql/statements/create-endpoint-transact-sql.md)  
  
