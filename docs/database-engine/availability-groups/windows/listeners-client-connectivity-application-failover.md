---
title: Se connecter à un écouteur de groupe de disponibilité
description: Cet article contient des informations sur le processus de connexion à un écouteur de groupe de disponibilité Always On, notamment sur la connexion au réplica principal et à un réplica secondaire en lecture seule, et sur l’utilisation de TLS/SSL et Kerberos.
ms.custom: contperf-fy21q1
ms.date: 02/27/2020
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: how-to
helpviewer_keywords:
- Availability Groups [SQL Server], listeners
- read-only routing
- read-intent connections [AlwaysOn Availability Groups]
- Availability Groups [SQL Server], configuring
- Availability Groups [SQL Server], read-only routing
- Availability Groups [SQL Server], client connectivity
ms.assetid: 76fb3eca-6b08-4610-8d79-64019dd56c44
author: cawrites
ms.author: chadam
ms.openlocfilehash: 145f061ff1926bc9a93244226dbd36b6ffaf9de8
ms.sourcegitcommit: 108bc8e576a116b261c1cc8e4f55d0e0713d402c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/25/2021
ms.locfileid: "98765913"
---
# <a name="connect-to-an-always-on-availability-group-listener"></a>Se connecter à un écouteur de groupe de disponibilité Always On 
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  
Une fois que vous avez terminé la [configuration de l’écouteur du groupe de disponibilité](create-or-configure-an-availability-group-listener-sql-server.md), vous devez mettre à jour la chaîne de connexion utilisée pour vous connecter à l’écouteur du groupe de disponibilité Always On. Cet écouteur route automatiquement le trafic entre votre application et le réplica prévu sans que vous ayez à mettre à jour manuellement la chaîne de connexion après chaque basculement. 
  

##  <a name="connect-to-the-primary-replica"></a><a name="ConnectToPrimary"></a> Se connecter au réplica principal  

Spécifiez le nom DNS de l’écouteur du groupe de disponibilité dans la chaîne de connexion pour établir la connexion au réplica principal en vue de l’accès en lecture-écriture. 

Par exemple, pour vous connecter au réplica principal dans SQL Server Management Studio par le biais de l’écouteur, entrez le nom DNS de l’écouteur dans le champ Nom du serveur : 

![Se connecter à l’écouteur dans SSMS](media/listeners-client-connectivity-application-failover/connect-to-listener-in-ssms.png)


Si le réplica principal est changé durant un basculement, les connexions à l’écouteur existantes sont déconnectées et les nouvelles connexions sont routées vers le nouveau réplica principal.  

Voici un exemple de chaîne de connexion de base pour le fournisseur ADO.NET (System.Data.SqlClient) : 
  
```  
Server=tcp: AGListener,1433;Database=MyDB;Integrated Security=SSPI  
```  

Vous pouvez vérifier à quel réplica vous êtes actuellement connecté par le biais de l’écouteur en exécutant la commande Transact-SQL (T-SQL) suivante :

```sql
SELECT @@SERVERNAME
```

Exemple où SQLVM1 est configuré comme réplica principal : 

![Vérifier la connectivité du réplica](media/listeners-client-connectivity-application-failover/replica-server-name.png)


Vous pouvez toujours vous connecter directement à l’instance de SQL Server en spécifiant le nom d’instance du réplica principal ou secondaire au lieu d’utiliser l’écouteur du groupe de disponibilité. Toutefois, dans ce cas, les nouvelles connexions ne sont plus routées automatiquement vers le nouveau réplica principal actuel. De plus, vous perdez l’avantage du routage en lecture seule, où les connexions spécifiées avec `read-intent` sont automatiquement routées vers le réplica secondaire accessible en lecture. 

##  <a name="connect-to-a-read-only-replica"></a><a name="ConnectToSecondary"></a> Se connecter à un réplica en lecture seule 

Le _routage en lecture seule_ fait référence à la capacité de router automatiquement les connexions entrantes de l’écouteur vers un réplica secondaire accessible en lecture qui est configuré pour autoriser des charges de travail en lecture seule. 

Les connexions sont automatiquement routées vers le réplica en lecture seule si les conditions suivantes sont remplies : 
 
-   Au moins un réplica secondaire est configuré pour l’accès en lecture seule, et chaque réplica secondaire en lecture seule ainsi que le réplica principal sont [configurés pour prendre en charge le routage en lecture seule](configure-read-only-routing-for-an-availability-group-sql-server.md). 

-   La chaîne de connexion référence une base de données impliquée dans le groupe de disponibilité. Une alternative serait que la connexion utilisée ait la base de données configurée comme base de données par défaut. Pour plus d’informations, consultez [cet article sur le fonctionnement de l’algorithme avec le routage en lecture seule](/archive/blogs/mattn/calculating-read_only_routing_url-for-alwayson).

-   La chaîne de connexion fait référence à un écouteur de groupe de disponibilité, et l’intention de l’application de la connexion entrante est définie en lecture seule (par exemple, à l’aide du mot clé **Application Intent=ReadOnly** dans les chaînes de connexion ODBC ou OLEDB, ou dans les attributs ou les propriétés de connexion). 

L’attribut d’intention d’application est stocké dans la session du client lors de la connexion. L’instance de SQL Server traite ensuite cette intention et détermine ce qu’il faut faire, selon la configuration du groupe de disponibilité et l’état en lecture-écriture actuel de la base de données cible dans le réplica secondaire.  

Par exemple, pour vous connecter à un réplica en lecture seule dans SQL Server Management Studio, sélectionnez **Options** dans la boîte de dialogue **Se connecter au serveur**, sélectionnez l’onglet **Paramètres de connexion supplémentaires**, puis spécifiez `ApplicationIntent=ReadOnly` dans la zone de texte :

![Connexion en lecture seule dans SSMS](media/listeners-client-connectivity-application-failover/read-only-intent-in-ssms.png)
  
Voici un exemple de chaîne de connexion pour le fournisseur ADO.NET (System.Data.SqlClient) qui indique l’intention d’application en lecture seule : 

```  
Server=tcp:AGListener;Database=AdventureWorks;Integrated Security=SSPI;ApplicationIntent=ReadOnly  
```  
  
Pour plus d’informations, consultez [Configurer l’accès en lecture seule sur un réplica de disponibilité &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/configure-read-only-access-on-an-availability-replica-sql-server.md)  

## <a name="non-default-port"></a>Port non défini comme port par défaut

Quand vous créez un écouteur, vous spécifiez le port que l’écouteur doit utiliser. Si le port est le port par défaut 1433, vous n’avez pas à spécifier un numéro de port pour vous connecter à l’écouteur. En revanche, si le port à utiliser n’est pas le port 1433, vous devez spécifier le port dans la chaîne de connexion au format `listenername,port`, comme dans cet exemple :

![Se connecter sur un port autre que celui par défaut](media/listeners-client-connectivity-application-failover/specify-port-in-ssms.png)

Voici un exemple de chaîne de connexion pour le fournisseur ADO.NET (System.Data.SqlClient) qui spécifie un port autre que le port par défaut pour l’écouteur : 

```  
Server=tcp:AGListener,1445;Database=AdventureWorks;Integrated Security=SSPI 
```  

##  <a name="bypass-listeners"></a><a name="BypassAGl"></a> Ignorer les écouteurs  

Alors que les écouteurs de groupe de disponibilité permettent la prise en charge de la redirection de basculement et le routage en lecture seule, les connexions clientes ne sont pas requises pour les utiliser. Une connexion cliente peut également faire directement référence à l'instance de SQL Server au lieu de se connecter à l'écouteur du groupe de disponibilité.  
  
Pour l’instance de SQL Server, cela ne fait aucune différence si une connexion est établie à l’aide de l’écouteur du groupe de disponibilité ou avec un autre point de terminaison d’instance.  L'instance de SQL Server vérifie l'état de la base de données ciblée, puis autorise ou interdit la connectivité, selon la configuration du groupe de disponibilité et l'état actuel de la base de données sur l'instance.  Par exemple, si une application cliente se connecte directement à une instance de port SQL Server et se connecte à une base de données cible hébergée dans un groupe de disponibilité, et que la base de données cible se trouve à l’état primaire et en ligne, la connectivité aboutit.  Si la base de données cible est hors connexion ou dans un état de transition, la connectivité à la base de données échoue.  
  
Autrement, lors d'une migration depuis une mise en miroir de bases de données vers [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)], les applications peuvent spécifier la chaîne de connexion de mise en miroir de bases de données dans la mesure où il existe un seul réplica secondaire et qu'il n'autorise pas les connexions utilisateur. 

##  <a name="database-mirroring-connection-strings"></a><a name="DbmConnectionString"></a> Chaînes de connexion de mise en miroir de bases de données 
 Si un groupe de disponibilité possède uniquement un réplica secondaire et est configuré avec ALLOW_CONNECTIONS = READ_ONLY ou ALLOW_CONNECTIONS = NONE pour le réplica secondaire, les clients peuvent se connecter au réplica principal à l'aide d'une chaîne de connexion de mise en miroir de bases de données. Cette approche peut être utile lors de la migration d'une application existante depuis la mise en miroir de bases de données vers un groupe de disponibilité, tant que vous limitez le groupe de disponibilité à deux réplicas de disponibilité (un réplica principal et un réplica secondaire). Si vous ajoutez des réplicas secondaires supplémentaires, vous devrez créer un écouteur de groupe de disponibilité pour le groupe de disponibilité et mettre à jour vos applications pour utiliser le nom DNS de l'écouteur du groupe de disponibilité.  
  
 Lors de l'utilisation de chaînes de connexion de mise en miroir de bases de données, le client peut soit utiliser [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client, soit le fournisseur de données .NET Framework pour [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. La chaîne de connexion fournie par un client doit fournir au minimum le nom d'une instance de serveur, le *nom du partenaire initial*, pour identifier l'instance de serveur qui héberge initialement le réplica de disponibilité auquel vous envisagez de vous connecter. Éventuellement, la chaîne de connexion peut également fournir le nom d'une autre instance de serveur, le *nom du partenaire de basculement*, pour identifier l'instance de serveur qui héberge initialement le réplica secondaire comme nom de partenaire de basculement.  
  
 Pour plus d’informations sur les chaînes de connexion de mise en miroir de base de données, consultez [Connecter des clients à une session de mise en miroir de bases de données &#40;SQL Server&#41;](../../../database-engine/database-mirroring/connect-clients-to-a-database-mirroring-session-sql-server.md).  
  
  
##  <a name="multi-subnet-failovers"></a><a name="SupportAgMultiSubnetFailover"></a> Basculements de plusieurs sous-réseaux  
 
 Si vous utilisez des bibliothèques clientes qui prennent en charge l’option de connexion MultiSubnetFailover dans la chaîne de connexion, vous pouvez optimiser le basculement du groupe de disponibilité vers un sous-réseau différent en définissant MultiSubnetFailover sur « True » ou « Yes », selon la syntaxe du fournisseur que vous utilisez.  
  
> [!NOTE]  
>  Nous vous recommandons de définir ce paramètre à la fois pour les connexions à un seul ou à plusieurs sous-réseaux aux écouteurs de groupes de disponibilité et aux noms d'instance de cluster de basculement SQL Server.  L'activation de cette option ajoute des optimisations supplémentaires, même pour les scénarios de sous-réseau unique.  
  
 L’option de connexion **MultiSubnetFailover** fonctionne uniquement avec le protocole réseau TCP et elle est prise en charge uniquement lors de la connexion à un écouteur de groupe de disponibilité et pour n’importe quel nom de réseau virtuel se connectant à [!INCLUDE[ssnoversion](../../../includes/ssnoversion-md.md)].  
  
 Voici un exemple de chaîne de connexion du fournisseur ADO.NET (System.Data.SqlClient) qui permet le basculement de plusieurs sous-réseaux :  
  
```  
Server=tcp:AGListener,1433;Database=AdventureWorks;Integrated Security=SSPI; MultiSubnetFailover=True  
```  
  
 L’option de connexion **MultiSubnetFailover** doit prendre la valeur **True** même si le groupe de disponibilité s’étend sur un seul sous-réseau.  Cela vous permet de préconfigurer de nouveaux clients afin de prendre en charge la future couverture de sous-réseaux, sans recourir à de futures modifications de la chaîne de connexion du client. Par ailleurs, cela permet d'optimiser les performances de basculement pour les basculements au sein d'un seul sous-réseau.  Alors que l’option de connexion **MultiSubnetFailover** n’est pas obligatoire, elle offre l’avantage d’un basculement de sous-réseau plus rapide.  Cela est dû au fait que le pilote client essaie d'ouvrir un socket TCP pour chaque adresse IP en parallèle associée au groupe de disponibilité.  Le pilote client attend une réponse correcte de la première adresse IP et, ceci fait, l'utilise pour la connexion.  
  
##  <a name="listeners--tlsssl-certificates"></a><a name="SSLcertificates"></a> Écouteurs et certificats TLS/SSL  

Lors de la connexion à un écouteur de groupe de disponibilité, si les instances participantes de SQL Server utilisent des certificats TLS/SSL en même temps que le chiffrement de session, le pilote client de la connexion doit prendre en charge le nom SAN (Subject Alternate Name) dans le certificat TLS/SSL afin de forcer le chiffrement.  La prise en charge de pilotes SQL Server pour le nom SAN de certificat est planifiée pour ADO.NET (SqlClient), Microsoft JDBC et SQL Native Client (SNAC).  
  
Un certificat X.509 doit être configuré pour chaque nœud serveur participant dans le cluster de basculement, avec une liste de tous les écouteurs de groupe de disponibilité définis dans le nom SAN du certificat. 

Le format des valeurs du certificat est le suivant : 

```  
CN = Server.FQDN  
SAN = Server.FQDN,Listener1.FQDN,Listener2.FQDN
```

Par exemple, si vous avez les valeurs suivantes : 

```
Servername: Win2019   
Instance: SQL2019   
AG: AG2019   
Listener: Listener2019   
Domain: contoso.com  (which is also the FQDN)
```

Pour un cluster WSFC qui a un seul groupe de disponibilité, le certificat doit avoir le nom de domaine complet (FQDN) du serveur et le nom de domaine complet de l’écouteur : 

```
CN: Win2019.contoso.com
SAN: Win2019.contoso.com, Listener2019.contoso.com 
```

Avec cette configuration, vos connexions seront chiffrées lors de la connexion à l’instance (`WIN2019\SQL2019`) ou à l’écouteur (`Listener2019`). 

En fonction de la configuration de la mise en réseau, il existe un petit sous-ensemble de clients qui peuvent également avoir besoin d’ajouter le NetBIOS au réseau SAN. Dans ce cas, les valeurs du certificat doivent être : 

```
CN: Win2019.contoso.com
SAN: Win2019,Win2019.contoso.com,Listener2019,Listener2019.contoso.com
```

Si le cluster WSFC dispose de trois écouteurs de groupe de disponibilité, par exemple : Listener1, Listener2, Listener3

Les valeurs de certificat doivent être les suivantes : 

```
CN: Win2019.contoso.com
SAN: Win2019.contoso.com,Listener1.contoso.com,Listener2.contoso.com,Listener3.contoso.com
```
  
  
##  <a name="listeners-and-kerberos-spns"></a><a name="SPNs"></a> Écouteurs et Kerberos (SPN) 

Un administrateur de domaine doit configurer un nom de principal de service (SPN) dans Active Directory pour chaque écouteur du groupe de disponibilité afin d’activer Kerberos pour les connexions clientes vers l’écouteur. Lors de l’inscription du SPN, vous devez utiliser le compte de service de l’instance de serveur qui héberge le réplica de disponibilité. Pour que le SPN fonctionne sur tous les réplicas, le même compte de service doit être utilisé pour toutes les instances du cluster WSFC qui héberge le groupe de disponibilité.  
  
 Utilisez l’outil de ligne de commande **setspn** Windows pour configurer le SPN.  Par exemple, pour configurer un SPN pour un groupe de disponibilité nommé `AG1listener.Adventure-Works.com` hébergé dans un ensemble d'instances de SQL Server, toutes configurées pour s'exécuter sous le compte de domaine `corp\svclogin2`:  
  
```  
setspn -A MSSQLSvc/AG1listener.Adventure-Works.com:1433 corp\svclogin2  
```  
  
 Pour plus d'informations sur l'inscription manuelle d'un SPN pour SQL Server, consultez [Inscrire un nom de principal du service pour les connexions Kerberos](../../../database-engine/configure-windows/register-a-service-principal-name-for-kerberos-connections.md).  
  


## <a name="next-steps"></a>Étapes suivantes

Une fois que vous êtes connecté à l’écouteur, vous pouvez décharger les [charges de travail en lecture seule](overview-of-always-on-availability-groups-sql-server.md) et les [sauvegardes](configure-backup-on-availability-replicas-sql-server.md) vers le réplica secondaire afin d’améliorer les performances. Vous pouvez également examiner différentes [stratégies de supervision des groupes de disponibilité](monitoring-of-availability-groups-sql-server.md) pour garantir l’intégrité de votre groupe de disponibilité. 

Pour plus d’informations sur les groupes de disponibilité, consultez [Vue d’ensemble des groupes de disponibilité Always On &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md).