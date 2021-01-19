---
title: Configurer le routage en lecture seule pour un groupe de disponibilité
description: Routez automatiquement tout le trafic en lecture seule vers un réplica secondaire en configurant le routage en lecture seule pour votre groupe de disponibilité Always On à l’aide de Transact-SQL (T-SQL) ou de PowerShell.
ms.custom: seodec18
ms.date: 02/25/2019
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: how-to
helpviewer_keywords:
- read-only routing
- Availability Groups [SQL Server], readable secondary replicas
- Availability Groups [SQL Server], read-only routing
- readable secondary replicas
- Availability Groups [SQL Server], client connectivity
- Availability Groups [SQL Server], active secondary replicas
ms.assetid: 7bd89ddd-0403-4930-a5eb-3c78718533d4
author: cawrites
ms.author: chadam
ms.openlocfilehash: f7e96df4eba36bbcb3da18a1423b5162aef557a7
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/13/2021
ms.locfileid: "98170801"
---
# <a name="configure-read-only-routing-for-an-always-on-availability-group"></a>Configurer le routage en lecture seule pour un groupe de disponibilité Always On
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  Pour configurer un groupe de disponibilité Always On et prendre en charge le routage en lecture seule dans [!INCLUDE[ssnoversion](../../../includes/ssnoversion-md.md)], vous pouvez utiliser [!INCLUDE[tsql](../../../includes/tsql-md.md)] ou PowerShell. Le *routage en lecture seule* fait référence à la capacité de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] d’acheminer les demandes de connexion en lecture seule applicables à un [réplica secondaire lisible](../../../database-engine/availability-groups/windows/active-secondaries-readable-secondary-replicas-always-on-availability-groups.md) Always On disponible (autrement dit, un réplica configuré pour autoriser des charges de travail en lecture seule lorsqu’il s’exécute sous le rôle secondaire). Pour prendre en charge le routage en lecture seule, le groupe de disponibilité doit posséder un [écouteur de groupe de disponibilité](../../../database-engine/availability-groups/windows/listeners-client-connectivity-application-failover.md). Les clients en lecture seule doivent diriger leurs demandes de connexion à cet écouteur, et les chaînes de connexion du client doivent spécifier l'intention d'application « en lecture seule ». Autrement dit, il doit s’agir de *demandes de connexion d’intention de lecture*.  

Le routage en lecture seule est disponible dans [!INCLUDE[sssql15](../../../includes/sssql16-md.md)] et versions ultérieures.

> [!NOTE]  
>  Pour plus d’informations sur la configuration d’un réplica secondaire lisible, consultez [Configurer l’accès en lecture seule sur un réplica de disponibilité &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/configure-read-only-access-on-an-availability-replica-sql-server.md).  

  
##  <a name="prerequisites"></a><a name="Prerequisites"></a> Conditions préalables  
  
-   Le groupe de disponibilité doit posséder un écouteur. Pour plus d'informations, consultez [Créer ou configurer un écouteur de groupe de disponibilité &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/create-or-configure-an-availability-group-listener-sql-server.md).  
  
-   Un ou plusieurs réplicas de disponibilité doivent être configurés pour recevoir la lecture seule dans le rôle secondaire (autrement dit, pour être des [réplicas secondaires lisibles](../../../database-engine/availability-groups/windows/active-secondaries-readable-secondary-replicas-always-on-availability-groups.md)). Pour plus d’informations, consultez [Configurer l’accès en lecture seule sur un réplica de disponibilité &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/configure-read-only-access-on-an-availability-replica-sql-server.md).  
  
-   Vous devez être connecté à l'instance de serveur qui héberge le réplica principal actuel.  

-   Si vous utilisez une connexion SQL, vérifiez que le compte est correctement configuré. Pour plus d’informations, voir [Gestion des connexions et des tâches des bases de données d’un groupe de disponibilité (SQL Server)](logins-and-jobs-for-availability-group-databases.md).
  
##  <a name="what-replica-properties-do-you-need-to-configure-to-support-read-only-routing"></a><a name="RORReplicaProperties"></a> Quelles sont les propriétés du réplica que vous avez besoin de configurer pour prendre en charge le routage en lecture seule ?  
  
-   Pour chaque réplica secondaire lisible qui doit prendre en charge le routage en lecture seule, vous devez spécifier une *URL de routage en lecture seule*. Cette URL est effective uniquement lorsque le réplica local s'exécute sous le rôle secondaire. L'URL de routage en lecture seule doit être spécifiée par réplica, si nécessaire. Chaque URL de routage en lecture seule est utilisée pour acheminer les demandes de connexion d'intention de lecture vers un réplica secondaire lisible spécifique. En général, à chaque réplica secondaire lisible est affecté une URL de routage en lecture seule.  
  
     Pour plus d’informations sur le calcul de l’URL de routage en lecture seule pour un réplica de disponibilité, consultez [Calcul de l’URL de routage en lecture seule pour Always On](https://web.archive.org/web/20170512023255/https://blogs.msdn.microsoft.com/mattn/2012/04/25/calculating-read_only_routing_url-for-alwayson/)
  
-   Pour chaque réplica de disponibilité qui doit prendre en charge le routage en lecture seule lorsqu’il est le réplica principal, vous devez spécifier une *liste de routage en lecture seule*. Une liste de routage en lecture seule donnée est effective uniquement lorsque le réplica local s'exécute sous le rôle principal. Cette liste doit être spécifiée par réplica, si nécessaire. En général, chaque liste de routage en lecture seule contient toutes les URL de routage en lecture seule, avec l'URL du réplica local à la fin de la liste.  
  
    > [!NOTE]  
    >  Les demandes de connexion d’intention de lecture sont acheminées à la première entrée disponible sur la liste de routage en lecture seule du réplica principal actuel. Toutefois, l’équilibrage de charge entre les réplicas en lecture seule est pris en charge. Pour plus d’informations, consultez [Configurer l’équilibrage de charge entre des réplicas en lecture seule](../../../database-engine/availability-groups/windows/configure-read-only-routing-for-an-availability-group-sql-server.md#loadbalancing).  
  
> [!NOTE]  
>  Pour plus d’informations sur les écouteurs de groupe de disponibilité et sur le routage en lecture seule, consultez [Écouteurs de groupe de disponibilité, connectivité client et basculement d’application &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/listeners-client-connectivity-application-failover.md).  
  
##  <a name="permissions"></a><a name="Permissions"></a> Autorisations  
  
|Tâche|Autorisations|  
|----------|-----------------|  
|Pour configurer des réplicas lors de la création d'un groupe de disponibilité|Requiert l’appartenance au rôle serveur fixe **sysadmin** et l’autorisation de serveur CREATE AVAILABILITY GROUP, l’autorisation ALTER ANY AVAILABILITY GROUP ou l’autorisation CONTROL SERVER.|  
|Pour modifier un réplica de disponibilité :|Requiert l'autorisation ALTER AVAILABILITY GROUP sur le groupe de disponibilité, l'autorisation CONTROL AVAILABILITY GROUP, l'autorisation ALTER ANY AVAILABILITY GROUP ou l'autorisation CONTROL SERVER.|  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Utilisation de Transact-SQL  
  
### <a name="configure-a-read-only-routing-list"></a>Configurer une liste de routage en lecture seule  
 Procédez comme suit pour configurer le routage en lecture seule à l’aide de Transact-SQL. Pour obtenir un exemple de code, consultez [Exemple (Transact-SQL)](#TsqlExample), plus loin dans cette section.  
  
1.  Connectez-vous à l'instance de serveur qui héberge le réplica principal.  
  
2.  Si vous spécifiez un réplica pour un nouveau groupe de disponibilité, utilisez l'instruction [CREATE AVAILABILITY GROUP](../../../t-sql/statements/create-availability-group-transact-sql.md)[!INCLUDE[tsql](../../../includes/tsql-md.md)] . Si vous ajoutez ou modifiez un réplica pour un groupe de disponibilité existant, utilisez l'instruction [ALTER AVAILABILITY GROUP](../../../t-sql/statements/alter-availability-group-transact-sql.md)[!INCLUDE[tsql](../../../includes/tsql-md.md)] .  
  
    -   Pour configurer le routage en lecture seule pour le rôle secondaire, dans la clause ADD REPLICA ou MODIFY REPLICA WITH, spécifiez l'option SECONDARY_ROLE, comme suit :  
  
         SECONDARY_ROLE **(** READ_ONLY_ROUTING_URL **='** TCP **://** _system-address_ **:** _port_ **')**  
  
         Les paramètres de l'URL de routage en lecture sont les suivants :  
  
         *system-address*  
         Chaîne, telle qu'un nom de système, un nom de domaine complet ou une adresse IP, qui identifie de manière unique l'ordinateur de destination.  
  
         *port*  
         Numéro de port utilisé par le moteur de base de données de l'instance de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] .  
  
         Par exemple :   `SECONDARY_ROLE (READ_ONLY_ROUTING_URL = N'TCP://COMPUTER01.contoso.com:1433')`  
  
         Dans une clause MODIFY REPLICA, ALLOW_CONNECTIONS est facultatif si le réplica est déjà configuré pour autoriser les connexions en lecture seule.  
  
         Pour plus d’informations, consultez [Calcul de l’URL de routage en lecture seule pour Always On](/archive/blogs/mattn/calculating-read_only_routing_url-for-alwayson).  
  
    -   Pour configurer le routage en lecture seule pour le rôle principal, dans la clause ADD REPLICA ou MODIFY REPLICA WITH, spécifiez l'option PRIMARY_ROLE, comme suit :  
  
         PRIMARY_ROLE **(** READ_ONLY_ROUTING_LIST **=('** _serveur_ **'** [ **,** ...*n* ] **))**  
  
         où *server* indique une instance de serveur qui héberge un réplica secondaire en lecture seule dans le groupe de disponibilité.  
  
         Par exemple :   `PRIMARY_ROLE (READ_ONLY_ROUTING_LIST=('Server1','Server2'))`  
  
        > [!NOTE]  
        >  Vous devez définir le routage en lecture seule avant de configurer la liste de routage en lecture seule.  
  
###  <a name="configure-load-balancing-across-read-only-replicas"></a><a name="loadbalancing"></a> Configurer l’équilibrage de charge entre des réplicas en lecture seule  
 Depuis [!INCLUDE[ssSQL15](../../../includes/sssql16-md.md)], vous pouvez configurer l’équilibrage de charge sur un ensemble de réplicas en lecture seule. Auparavant, le routage en lecture seule dirigeait toujours le trafic vers le premier réplica disponible dans la liste de routage. Pour tirer parti de cette fonctionnalité, utilisez un seul niveau de parenthèses imbriquées autour des instances de serveur **READ_ONLY_ROUTING_LIST** dans les commandes **CREATE AVAILABILITY GROUP** ou **ALTER AVAILABILITY GROUP** .  
  
 Par exemple, la liste de routage suivante équilibre la charge de la demande de connexion d’intention de lecture entre deux réplicas en lecture seule : `Server1` et `Server2`. Les parenthèses imbriquées autour de ces serveurs identifient l’ensemble dont la charge est équilibrée. Si aucun réplica n’est disponible dans cet ensemble, la fonctionnalité tente de se connecter séquentiellement aux autres réplicas, `Server3` et `Server4`, dans la liste de routage en lecture seule.  
  
```sql  
READ_ONLY_ROUTING_LIST = (('Server1','Server2'), 'Server3', 'Server4')  
```  
  
 Notez que chaque entrée de la liste de routage peut représenter un ensemble de réplicas en lecture seule dont la charge est équilibrée. l’exemple ci-dessous illustre ce cas de figure.  
  
```sql  
READ_ONLY_ROUTING_LIST = (('Server1','Server2'), ('Server3', 'Server4', 'Server5'), 'Server6')  
```  
  
 Seul un niveau de parenthèses imbriquées est pris en charge.  
  
###  <a name="example-transact-sql"></a><a name="TsqlExample"></a> Exemple (Transact-SQL)  
 L'exemple suivant modifie deux réplicas de disponibilité d'un groupe de disponibilité existant, `AG1` pour prendre en charge le routage en lecture seule si un de ces réplicas détient actuellement le rôle principal. Pour identifier les instances de serveur qui hébergent le réplica de disponibilité, cet exemple spécifie les noms d’instance `COMPUTER01` et `COMPUTER02`.  
  
```  
ALTER AVAILABILITY GROUP [AG1]  
 MODIFY REPLICA ON  
N'COMPUTER01' WITH   
(SECONDARY_ROLE (ALLOW_CONNECTIONS = READ_ONLY));  
ALTER AVAILABILITY GROUP [AG1]  
 MODIFY REPLICA ON  
N'COMPUTER01' WITH   
(SECONDARY_ROLE (READ_ONLY_ROUTING_URL = N'TCP://COMPUTER01.contoso.com:1433'));  
  
ALTER AVAILABILITY GROUP [AG1]  
 MODIFY REPLICA ON  
N'COMPUTER02' WITH   
(SECONDARY_ROLE (ALLOW_CONNECTIONS = READ_ONLY));  
ALTER AVAILABILITY GROUP [AG1]  
 MODIFY REPLICA ON  
N'COMPUTER02' WITH   
(SECONDARY_ROLE (READ_ONLY_ROUTING_URL = N'TCP://COMPUTER02.contoso.com:1433'));  
  
ALTER AVAILABILITY GROUP [AG1]   
MODIFY REPLICA ON  
N'COMPUTER01' WITH   
(PRIMARY_ROLE (READ_ONLY_ROUTING_LIST=('COMPUTER02','COMPUTER01')));  
  
ALTER AVAILABILITY GROUP [AG1]   
MODIFY REPLICA ON  
N'COMPUTER02' WITH   
(PRIMARY_ROLE (READ_ONLY_ROUTING_LIST=('COMPUTER01','COMPUTER02')));  
GO  
  
```  
  
##  <a name="using-powershell"></a><a name="PowerShellProcedure"></a> Utilisation de PowerShell  
  
### <a name="configure-a-read-only-routing-list"></a>Configurer une liste de routage en lecture seule  
 Procédez comme suit pour configurer le routage en lecture seule à l’aide de PowerShell. Pour obtenir un exemple de code, consultez [Exemple (PowerShell)](#PSExample), plus loin dans cette section.  
  
1.  Définissez la valeur par défaut (**cd**) sur l’instance de serveur qui héberge le réplica principal.  
  
2.  Quand vous ajoutez un réplica de disponibilité à un groupe de disponibilité, utilisez l’applet de commande **New-SqlAvailabilityReplica** . Quand vous modifiez un réplica de disponibilité existant, utilisez l’applet de commande **Set-SqlAvailabilityReplica** . Les paramètres pertinents sont les suivants :  
  
    -   Pour configurer le routage en lecture seule pour le rôle secondaire, spécifiez le paramètre **ReadonlyRoutingConnectionUrl"** _url_ **"** .  
  
         où *url* est le nom de domaine complet (FQDN) de la connectivité et le port à utiliser lors de l’acheminement vers le réplica pour les connexions en lecture seule. Par exemple : `-ReadonlyRoutingConnectionUrl "TCP://DBSERVER8.manufacturing.Adventure-Works.com:7024"`  
  
         Pour plus d’informations, consultez [Calcul de l’URL de routage en lecture seule pour Always On](/archive/blogs/mattn/calculating-read_only_routing_url-for-alwayson).  
  
    -   Pour configurer l’accès à la connexion pour le rôle principal, spécifiez **ReadonlyRoutingList"** _server_ **"** [ **,** ...*n* ], où *server* identifie une instance de serveur qui héberge un réplica secondaire en lecture seule dans le groupe de disponibilité. Par exemple : `-ReadOnlyRoutingList "SecondaryServer","PrimaryServer"`  
  
        > [!NOTE]  
        >  Vous devez définir l'URL de routage en lecture seule d'un réplica avant de configurer sa liste de routage en lecture seule.  
  
    > [!NOTE]  
    >  Pour voir la syntaxe d’une applet de commande, utilisez l’applet de commande **Get-Help** dans l’environnement [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] PowerShell. Pour en savoir plus, voir [Get Help SQL Server PowerShell](../../../powershell/sql-server-powershell.md).  
  
### <a name="set-up-and-use-the-sql-server-powershell-provider"></a>Configurer et utiliser le fournisseur PowerShell SQL Server  
  
-   [Fournisseur SQL Server PowerShell](../../../powershell/sql-server-powershell-provider.md)  
  
-   [Get Help SQL Server PowerShell](../../../powershell/sql-server-powershell.md)  
  
###  <a name="example-powershell"></a><a name="PSExample"></a> Exemple (PowerShell)  
 L'exemple suivant configure le réplica principal et un réplica secondaire dans un groupe de disponibilité pour le routage en lecture seule. D'abord, l'exemple affecte une URL de routage en lecture seule à chaque réplica. Il définit ensuite la liste de routage en lecture seule sur le réplica principal. Les connexions avec la propriété « ReadOnly » définie dans la chaîne de connexion sont redirigées vers le réplica secondaire. Si ce réplica secondaire n’est pas lisible (comme le détermine le paramètre **ConnectionModeInSecondaryRole** ), la connexion est renvoyée vers le réplica principal.  
  
```  
Set-Location SQLSERVER:\SQL\PrimaryServer\default\AvailabilityGroups\MyAg  
$primaryReplica = Get-Item "AvailabilityReplicas\PrimaryServer"  
$secondaryReplica = Get-Item "AvailabilityReplicas\SecondaryServer"  
  
Set-SqlAvailabilityReplica -ReadOnlyRoutingConnectionUrl "TCP://PrimaryServer.domain.com:1433" -InputObject $primaryReplica  
Set-SqlAvailabilityReplica -ReadOnlyRoutingConnectionUrl "TCP://SecondaryServer.domain.com:1433" -InputObject $secondaryReplica  
Set-SqlAvailabilityReplica -ReadOnlyRoutingList "SecondaryServer","PrimaryServer" -InputObject $primaryReplica  
```  
  
##  <a name="follow-up-after-configuring-read-only-routing"></a><a name="FollowUp"></a> Suivi : Après la configuration du routage en lecture seule  
 Une fois le réplica principal actuel et les réplicas secondaires lisibles configurés pour prendre en charge le routage en lecture seule dans les deux rôles, les réplicas secondaires lisibles peuvent recevoir des demandes de connexion d'intention de lecture des clients qui se connectent via l'écouteur du groupe de disponibilité.  
  
> [!TIP]  
>  Quand vous utilisez [bcp Utility](../../../tools/bcp-utility.md) ou [sqlcmd Utility](../../../tools/sqlcmd-utility.md), vous pouvez spécifier l’accès en lecture seule à n’importe quel réplica secondaire qui est activé pour l’accès en lecture seule en spécifiant le commutateur **-K ReadOnly** .  
  
###  <a name="requirements-and-recommendations-for-client-connection-strings"></a><a name="ConnStringReqsRecs"></a> Exigences et recommandations pour les chaînes de connexion clientes  
 Pour qu'une application cliente utilise le routage en lecture seule, sa chaîne de connexion doit respecter les conditions suivantes :  
  
-   Utiliser le protocole TCP.  
  
-   Définir l'attribut/propriété d'intention de l'application en lecture seule.  
  
-   Référencer l'écouteur d'un groupe de disponibilité qui est configuré pour prendre en charge le routage en lecture seule.  
  
-   Référencer une base de données dans ce groupe de disponibilité.  
  
 En outre, nous recommandons que les chaînes de connexion autorisent le basculement de sous-réseaux multiples, ce qui permet de prendre en charge un thread client parallèle pour chaque réplica sur chaque sous-réseau. Cela réduit le temps de reconnexion du client après un basculement.  
  
 La syntaxe d'une chaîne de connexion dépend du fournisseur SQL Server utilisé par l'application. L'exemple de chaîne de connexion suivant pour le fournisseur de données .NET Framework Data Provider 4.0.2 pour SQL Server illustre les parties d'une chaîne de connexion requises et recommandées pour utiliser le routage en lecture seule.  
  
```  
Server=tcp:MyAgListener,1433;Database=Db1;IntegratedSecurity=SSPI;ApplicationIntent=ReadOnly;MultiSubnetFailover=True  
```  
  
 Pour plus d’informations sur l’intention de l’application en lecture seule et le routage en lecture seule, consultez [Écouteurs de groupe de disponibilité, connectivité client et basculement d’application &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/listeners-client-connectivity-application-failover.md).  
  
### <a name="if-read-only-routing-is-not-working-correctly"></a>Si le routage en lecture seule ne fonctionne pas correctement  
 Pour plus d’informations sur la résolution des problèmes liés à la configuration du routage en lecture seule, consultez [Le routage en lecture seule ne fonctionne pas correctement](../../../database-engine/availability-groups/windows/troubleshoot-always-on-availability-groups-configuration-sql-server.md#ROR).  
  
##  <a name="next-steps"></a><a name="RelatedTasks"></a> Étapes suivantes 
**Pour consulter les configurations de routage en lecture seule**  
  
-   [sys.availability_read_only_routing_lists &#40;Transact-SQL&#41;](../../../relational-databases/system-catalog-views/sys-availability-read-only-routing-lists-transact-sql.md)  
  
-   [sys.availability_replicas &#40;Transact-SQL&#41;](../../../relational-databases/system-catalog-views/sys-availability-replicas-transact-sql.md) (colonne **read_only_routing_url**)  
  
**Pour configurer l'accès à la connexion du client**  
  
-   [Créer ou configurer un écouteur de groupe de disponibilité &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/create-or-configure-an-availability-group-listener-sql-server.md)  
  
-   [Configurer l’accès en lecture seule sur un réplica de disponibilité &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/configure-read-only-access-on-an-availability-replica-sql-server.md)  
  
**Pour utiliser les chaînes de connexion dans des applications**  
  
-   [Prise en charge des fonctionnalités de récupération d'urgence, haute disponibilité par SQL Server Native Client](../../../relational-databases/native-client/features/sql-server-native-client-support-for-high-availability-disaster-recovery.md)  
  
-   [Utilisation de mots clés de chaîne de connexion avec SQL Server Native Client](../../../relational-databases/native-client/applications/using-connection-string-keywords-with-sql-server-native-client.md)  
  
**Blogs :**  
  
-    [Calcul de l’URL de routage en lecture seule pour Always On](/archive/blogs/mattn/calculating-read_only_routing_url-for-alwayson)  
  
-    [Blogs de l’équipe SQL Server Always On : Blog officiel de l’équipe SQL Server Always On](/archive/blogs/sqlalwayson/)  
  
-    [Blogs des ingénieurs du Service clientèle et du Support technique de SQL Server](/archive/blogs/psssql/)  
  
**Livres blancs :**  
  
-    [Livres blancs de Microsoft pour SQL Server 2012](https://social.technet.microsoft.com/wiki/contents/articles/13146.white-paper-gallery-for-sql-server.aspx#[Category]SQLServer2012)  
  
-    [Livres blancs de l'équipe de consultants clients de SQL Server](https://techcommunity.microsoft.com/t5/DataCAT/bg-p/DataCAT/)  

**Contenu supplémentaire**

- [Vue d’ensemble des groupes de disponibilité Always On (SQL Server)](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)   

- [Secondaires actifs : Réplicas secondaires accessibles en lecture &#40;groupes de disponibilité Always On&#41;](../../../database-engine/availability-groups/windows/active-secondaries-readable-secondary-replicas-always-on-availability-groups.md)   

- [À propos de l’accès de la connexion client aux réplicas de disponibilité &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/about-client-connection-access-to-availability-replicas-sql-server.md)   
 
- [Écouteurs de groupe de disponibilité, connectivité client et basculement d’application &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/listeners-client-connectivity-application-failover.md)