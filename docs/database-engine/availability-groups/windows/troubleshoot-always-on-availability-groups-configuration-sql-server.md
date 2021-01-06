---
title: Problèmes courants liés aux groupes de disponibilité et procédures de résolution
description: Résoudre les problèmes courants de configuration des instances de serveur pour les groupes de disponibilité Always On sur SQL Server.
ms.custom: seo-lt-2019
ms.date: 05/17/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: conceptual
helpviewer_keywords:
- troubleshooting [SQL Server], deploying
- Availability Groups [SQL Server], troubleshooting
- Availability Groups [SQL Server], configuring
ms.assetid: 8c222f98-7392-4faf-b7ad-5fb60ffa237e
author: cawrites
ms.author: chadam
ms.openlocfilehash: 0fbdfdced5e0563e86775e75aaf187d6f26a0a38
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/17/2020
ms.locfileid: "97641515"
---
# <a name="troubleshoot-always-on-availability-groups-configuration-sql-server"></a>Résoudre des problèmes de configuration des groupes de disponibilité Always On (SQL Server)
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]

  Cette rubrique fournit des informations pour vous aider à résoudre les problèmes courants de configuration des instances de serveur pour [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)]. Problèmes courants de configuration : [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] est désactivé, les comptes sont incorrectement configurés, le point de terminaison de mise en miroir de bases de données n'existe pas, le point de terminaison est inaccessible (erreur SQL Server 1418), l'accès réseau n'existe pas et échec d'une commande de jointure de base de données (erreur SQL Server 35250).  
  
> [!NOTE]  
>  Assurez-vous que les conditions préalables requises pour [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] sont réunies. Pour plus d’informations, consultez [Conditions préalables requises, restrictions et recommandations pour les groupes de disponibilité Always On &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/prereqs-restrictions-recommendations-always-on-availability.md).  
  
 **Dans cette rubrique :**  
  
|Section|Description|  
|-------------|-----------------|  
|[Les groupes de disponibilité Always On ne sont pas activés](#IsHadrEnabled)|Si une instance de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] n'est pas activée pour [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)], elle ne prend pas en charge la création de groupe de disponibilité et ne peut héberger aucun réplica de disponibilité.|  
|[Comptes (Accounts)](#Accounts)|Traite des conditions nécessaires à la configuration correcte des comptes sous lesquels [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] s'exécute.|  
|[Points de terminaison](#Endpoints)|Explique comment diagnostiquer les problèmes liés au point de terminaison de mise en miroir de bases de données d'une instance de serveur.|  
|[Nom système](#SystemName)|Résume les solutions possibles pour spécifier le nom système d'une instance de serveur dans une URL de point de terminaison.|  
|[Accès réseau](#NetworkAccess)|Traite de l'exigence selon laquelle chaque instance de serveur qui héberge un réplica de disponibilité doit être autorisée à accéder au port de chacune des autres instances de serveur par le biais du protocole TCP.|  
|[Accès au point de terminaison (erreur SQL Server 1418)](#Msg1418)|Contient des informations sur ce message d'erreur [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] .|  
|[Échec de jointure de base de données (erreur SQL Server 35250)](#JoinDbFails)|Décrit les causes possibles et la résolution d'une impossibilité de joindre des bases de données secondaires à un groupe de disponibilité du fait que la connexion au réplica principal n'est pas active.|  
|[Le routage en lecture seule ne fonctionne pas correctement](#ROR)||  
|[Tâches associées](#RelatedTasks)|Contient une liste de rubriques orientées tâche dans la documentation en ligne de [!INCLUDE[ssCurrent](../../../includes/sscurrent-md.md)] qui sont particulièrement appropriée au dépannage d'une configuration de groupe de disponibilité.|  
|[Contenu connexe](#RelatedContent)|Contient une liste de ressources pertinentes externes à la documentation en ligne de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] .|  
  
##  <a name="always-on-availability-groups-is-not-enabled"></a><a name="IsHadrEnabled"></a> Les groupes de disponibilité Always On ne sont pas activés  
 La fonctionnalité [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] doit être activée sur chacune des instances de [!INCLUDE[ssCurrent](../../../includes/sscurrent-md.md)]. Pour plus d’informations, consultez [Activer et désactiver les groupes de disponibilité Always On &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/enable-and-disable-always-on-availability-groups-sql-server.md).  
  
##  <a name="accounts"></a><a name="Accounts"></a> Comptes (Accounts)  
 Les comptes sous lesquels [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] est en cours d'exécution doivent être configurés correctement.  
  
1.  Les comptes possèdent-ils les autorisations appropriées ?  
  
    1.  Si les partenaires s'exécutent sous le même compte d'utilisateur de domaine, les noms d'accès d'utilisateur corrects existent automatiquement dans les deux bases de données **master** . Cela simplifie la configuration de sécurité de la base de données et est recommandé.  
  
    2.  Si deux instances de serveur s'exécutent sous des comptes différents, la connexion de chaque compte doit être créée dans la base de données **master** sur l'instance de serveur distante, et cette connexion doit disposer des autorisations CONNECT pour se connecter au point de terminaison de mise en miroir de bases de données de cette instance de serveur. Pour plus d’informations, consultez [Configurer des comptes de connexion pour la mise en miroir de bases de données ou les groupes de disponibilité Always On &#40;SQL Server&#41;](../../../database-engine/database-mirroring/set-up-login-accounts-database-mirroring-always-on-availability.md).  
  
2.  Si [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] s'exécute en tant que compte intégré, tel que Système local, Service local ou Service réseau, ou comme compte qui n'appartient pas au domaine, vous devez utiliser des certificats pour l'authentification de point de terminaison. Si vos comptes de service utilisent des comptes de domaine au sein du même domaine, vous pouvez choisir d'accorder l'accès CONNECT pour chaque compte de service sur tous les emplacements de réplica ou vous pouvez utiliser des certificats. Pour plus d’informations, consultez [Utiliser des certificats pour un point de terminaison de mise en miroir de bases de données &#40;Transact-SQL&#41;](../../../database-engine/database-mirroring/use-certificates-for-a-database-mirroring-endpoint-transact-sql.md).  
  
##  <a name="endpoints"></a><a name="Endpoints"></a> Endpoints  
 Les points de terminaison doivent être correctement configurés.  
  
1.  Vérifiez que chaque instance de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] devant héberger un réplica de disponibilité (chaque *emplacement de réplica*) dispose d’un point de terminaison de mise en miroir de bases de données. Pour déterminer si un point de terminaison de mise en miroir de bases de données existe sur une instance de serveur donnée, utilisez l’affichage catalogue [sys.database_mirroring_endpoints](../../../relational-databases/system-catalog-views/sys-database-mirroring-endpoints-transact-sql.md). Pour plus d’informations, consultez [Créer un point de terminaison de mise en miroir de bases de données pour l’authentification Windows &#40;Transact-SQL&#41;](../../../database-engine/database-mirroring/create-a-database-mirroring-endpoint-for-windows-authentication-transact-sql.md) ou [Autoriser un point de terminaison de mise en miroir de bases de données à utiliser des certificats pour les connexions sortantes &#40;Transact-SQL&#41;](../../../database-engine/database-mirroring/database-mirroring-use-certificates-for-outbound-connections.md).  
  
2.  Vérifiez les numéros de ports.  
  
     Pour identifier le port actuellement associé au point de terminaison de mise en miroir de bases de données d'une instance de serveur, utilisez l'instruction [!INCLUDE[tsql](../../../includes/tsql-md.md)] suivante :  
  
    ```  
    SELECT type_desc, port FROM sys.tcp_endpoints;  
    GO  
    ```  
  
3.  Pour les problèmes de configuration [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] qu'il est difficile d'expliquer, nous vous conseillons d'examiner chaque instance de serveur pour déterminer si elle est à l'écoute sur les ports appropriés.  
  
4.  Vérifiez que les points de terminaison sont démarrés (STATE=STARTED). Sur chaque instance de serveur, utilisez l'instruction [!INCLUDE[tsql](../../../includes/tsql-md.md)] suivante :  
  
    ```  
    SELECT state_desc FROM sys.database_mirroring_endpoints  
    ```  
  
     Pour plus d’informations sur la colonne **state_desc**, consultez [sys.database_mirroring_endpoints &#40;Transact-SQL&#41;](../../../relational-databases/system-catalog-views/sys-database-mirroring-endpoints-transact-sql.md).  
  
     Pour démarrer un point de terminaison, utilisez l'instruction [!INCLUDE[tsql](../../../includes/tsql-md.md)] suivante :  
  
    ```  
    ALTER ENDPOINT Endpoint_Mirroring   
    STATE = STARTED   
    AS TCP (LISTENER_PORT = <port_number>)  
    FOR database_mirroring (ROLE = ALL);  
    GO  
    ```  
  
     Pour plus d’informations, consultez [ALTER ENDPOINT &#40;Transact-SQL&#41;](../../../t-sql/statements/alter-endpoint-transact-sql.md).  
  
5.  Assurez-vous que la connexion de l'autre serveur possède l'autorisation CONNECT. Pour déterminer qui possède une autorisation CONNECT pour un point de terminaison, sur chaque instance de serveur, utilisez l'instruction [!INCLUDE[tsql](../../../includes/tsql-md.md)] suivante :  
  
    ```  
    SELECT 'Metadata Check';  
    SELECT EP.name, SP.STATE,   
       CONVERT(nvarchar(38), suser_name(SP.grantor_principal_id))   
          AS GRANTOR,   
       SP.TYPE AS PERMISSION,  
       CONVERT(nvarchar(46),suser_name(SP.grantee_principal_id))   
          AS GRANTEE   
       FROM sys.server_permissions SP , sys.endpoints EP  
       WHERE SP.major_id = EP.endpoint_id  
       ORDER BY Permission,grantor, grantee;   
    GO  
  
    ```  
  
##  <a name="system-name"></a><a name="SystemName"></a> System Name  
 Comme nom système d'une instance de serveur dans une URL de point de terminaison, vous pouvez utiliser tout nom qui identifie sans ambiguïté le système. L'adresse de serveur peut être un nom système (si les systèmes sont dans le même domaine), un nom de domaine complet ou une adresse IP (de préférence statique). L'utilisation du nom de domaine complet garantit un fonctionnement correct. Pour plus d’informations, consultez [Spécifier l’URL de point de terminaison lors de l’ajout ou lors de la modification d’un réplica de disponibilité &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/specify-endpoint-url-adding-or-modifying-availability-replica.md).  
  
##  <a name="network-access"></a><a name="NetworkAccess"></a> Network Access  
 Chaque instance de serveur qui héberge un réplica de disponibilité doit être autorisée à accéder au port de chacune de l'autre instance de serveur par le biais du protocole TCP. Ceci est d'autant plus important que les instances de serveur peuvent se trouver dans différents domaines, entre lesquels aucune relation d'approbation n'a été établie (domaines non approuvés).  
  
##  <a name="endpoint-access-sql-server-error-1418"></a><a name="Msg1418"></a> Accès au point de terminaison (erreur SQL Server 1418)  
 Ce message [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] indique que l'adresse réseau du serveur spécifiée dans l'URL de point de terminaison ne peut pas être atteinte ou n'existe pas, et il suggère de vérifier le nom de l'adresse réseau puis de réexécuter la commande.  
  
##  <a name="join-database-fails-sql-server-error-35250"></a><a name="JoinDbFails"></a> Échec de jointure de base de données (erreur SQL Server 35250)  
 Cette section décrit les causes possibles et la résolution d'un problème de jointure de bases de données secondaires au groupe de disponibilité du fait que la connexion au réplica principal n'est pas active.  
  
 **Résolution :**  
  
1.  Vérifiez le paramètre du pare-feu pour voir s'il permet la communication de port du point de terminaison entre les instances de serveur qui hébergent le réplica principal et le réplica secondaire (port 5022 par défaut).  
  
2.  Vérifiez si le compte de service réseau dispose de l'autorisation de connexion au point de terminaison.  
  
##  <a name="read-only-routing-is-not-working-correctly"></a><a name="ROR"></a> Le routage en lecture seule ne fonctionne pas correctement  
 Vérifiez les paramètres suivants des valeurs de configuration et corrigez-les si nécessaire.  
  
|Sur...|Action|Commentaires|Lien|  
|---------|------------|--------------|----------|  
|Réplica principal actuel|Assurez-vous que l'écouteur du groupe de disponibilité est en ligne.|**Pour vérifier si l'écouteur en ligne :**<br /><br /> `SELECT * FROM sys.dm_tcp_listener_states;`<br /><br /> **Pour redémarrer un écouteur hors connexion :**<br /><br /> `ALTER AVAILABILITY GROUP myAG RESTART LISTENER 'myAG_Listener';`|[sys.dm_tcp_listener_states &#40;Transact-SQL&#41;](../../../relational-databases/system-dynamic-management-views/sys-dm-tcp-listener-states-transact-sql.md)<br /><br /> [ALTER AVAILABILITY GROUP &#40;Transact-SQL&#41;](../../../t-sql/statements/alter-availability-group-transact-sql.md)|  
|Réplica principal actuel|Vérifiez que l'option READ_ONLY_ROUTING_LIST contient uniquement les instances de serveur qui hébergent un réplica secondaire accessible en lecture.|**Pour identifier des réplicas secondaires accessibles en lecture :** sys.availability_replicas (colonne **secondary_role_allow_connections_desc** )<br /><br /> **Pour consulter une liste de routage en lecture seule :** sys.availability_read_only_routing_lists<br /><br /> **Pour modifier une liste de routage en lecture seule :** ALTER AVAILABILITY GROUP|[sys.availability_replicas &#40;Transact-SQL&#41;](../../../relational-databases/system-catalog-views/sys-availability-replicas-transact-sql.md)<br /><br /> [sys.availability_read_only_routing_lists &#40;Transact-SQL&#41;](../../../relational-databases/system-catalog-views/sys-availability-read-only-routing-lists-transact-sql.md)<br /><br /> [ALTER AVAILABILITY GROUP &#40;Transact-SQL&#41;](../../../t-sql/statements/alter-availability-group-transact-sql.md)|  
|Chaque réplica de read_only_routing_list|Assurez-vous que le Pare-feu Windows ne bloque pas le port READ_ONLY_ROUTING_URL.|-|[Configurer un pare-feu Windows pour accéder au moteur de base de données](../../../database-engine/configure-windows/configure-a-windows-firewall-for-database-engine-access.md)|  
|Chaque réplica de read_only_routing_list|Dans le gestionnaire de configuration de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] , vérifiez que :<br /><br /> La connectivité à distance SQL Server est activée.<br /><br /> TCP/IP est activé<br /><br /> Les adresses IP sont configurées correctement.|-|[Afficher ou modifier des propriétés de serveur &#40;SQL Server&#41;](../../../database-engine/configure-windows/view-or-change-server-properties-sql-server.md)<br /><br /> [Configurer un serveur pour écouter un port TCP spécifique &#40;SQL Server Configuration Manager&#41;](../../../database-engine/configure-windows/configure-a-server-to-listen-on-a-specific-tcp-port.md)|  
|Chaque réplica de read_only_routing_list|Vérifiez que READ_ONLY_ROUTING_URL (TCP <strong>://</strong>*system-address*<strong>:</strong>*port*) contient le nom de domaine complet (FQDN) et le numéro de port corrects.|-|[Calcul de l’URL de routage en lecture seule pour Always On](/archive/blogs/mattn/calculating-read_only_routing_url-for-alwayson)<br /><br /> [sys.availability_replicas &#40;Transact-SQL&#41;](../../../relational-databases/system-catalog-views/sys-availability-replicas-transact-sql.md)<br /><br /> [ALTER AVAILABILITY GROUP &#40;Transact-SQL&#41;](../../../t-sql/statements/alter-availability-group-transact-sql.md)|  
|Système client|Vérifiez que le pilote client prend en charge le routage en lecture seule.|-|[Connectivité client Always On &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/always-on-client-connectivity-sql-server.md)|  
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> Tâches associées  
  
-   [Création et configuration des groupes de disponibilité &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/creation-and-configuration-of-availability-groups-sql-server.md)  
  
-   [Créer un point de terminaison de mise en miroir de bases de données pour l’authentification Windows &#40;Transact-SQL&#41;](../../../database-engine/database-mirroring/create-a-database-mirroring-endpoint-for-windows-authentication-transact-sql.md)  
  
-   [Spécifier l’URL de point de terminaison lors de l’ajout ou lors de la modification d’un réplica de disponibilité &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/specify-endpoint-url-adding-or-modifying-availability-replica.md)  
  
-   [Préparer manuellement une base de données secondaire pour un groupe de disponibilité &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/manually-prepare-a-secondary-database-for-an-availability-group-sql-server.md)  
  
-   [Résoudre une opération d’ajout de fichier ayant échoué &#40;groupes de disponibilité Always On&#41;](../../../database-engine/availability-groups/windows/troubleshoot-a-failed-add-file-operation-always-on-availability-groups.md)  
  
-   [Gestion des connexions et des travaux pour les bases de données d’un groupe de disponibilité &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/logins-and-jobs-for-availability-group-databases.md)  
  
-   [Gérer les métadonnées durant la mise à disposition d’une base de données sur une autre instance de serveur &#40;SQL Server&#41;](../../../relational-databases/databases/manage-metadata-when-making-a-database-available-on-another-server.md)  
  
##  <a name="related-content"></a><a name="RelatedContent"></a> Contenu associé  
  
-   [Afficher les événements et journaux pour un cluster de basculement](https://technet.microsoft.com/library/cc772342\(WS.10\).aspx)  
  
-   [Applets de commande de cluster de basculement Get-ClusterLog](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/ee461045(v=technet.10))  
  
-   [Blog de l’équipe SQL Server Always On : Blog officiel de l’équipe SQL Server Always On](/archive/blogs/sqlalwayson/)  
  
## <a name="see-also"></a>Voir aussi  
 [Sécurité du transport de la mise en miroir de bases de données et des groupes de disponibilité Always On &#40;SQL Server&#41;](../../../database-engine/database-mirroring/transport-security-database-mirroring-always-on-availability.md)   
 [Configuration du réseau client](../../../database-engine/configure-windows/client-network-configuration.md)   
 [Conditions préalables requises, restrictions et recommandations pour les groupes de disponibilité Always On &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/prereqs-restrictions-recommendations-always-on-availability.md)  
  
