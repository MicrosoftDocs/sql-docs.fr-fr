---
title: Problèmes courants liés à la configuration de la mise en miroir de bases de données
description: Fournit des informations permettant de résoudre les problèmes de configuration d’une session de mise en miroir de bases de données.
ms.custom: seo-lt-2019
ms.date: 05/17/2016
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: database-mirroring
ms.topic: conceptual
helpviewer_keywords:
- database mirroring [SQL Server], deployment
- endpoints [SQL Server], database mirroring
- database mirroring [SQL Server], troubleshooting
- troubleshooting [SQL Server], database mirroring
ms.assetid: 87d3801b-dc52-419e-9316-8b1f1490946c
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: 4f8b18819666c7cec7c0f50b074a5a9a7ed5a804
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100352225"
---
# <a name="troubleshoot-database-mirroring-configuration-sql-server"></a>Résoudre les problèmes de configuration de mise en miroir de bases de données (SQL Server)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  Cette rubrique fournit des informations destinées à résoudre les problèmes de configuration d'une session de mise en miroir de base de données.  
  
> [!NOTE]  
>  Assurez-vous que vous prenez en charge toutes les [conditions préalables à la mise en miroir de bases de données](../../database-engine/database-mirroring/prerequisites-restrictions-and-recommendations-for-database-mirroring.md).  
  
|Problème|Résumé|  
|-----------|-------------|  
|Message d'erreur 1418|Ce message [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] indique que l'adresse réseau du serveur ne peut pas être atteinte ou n'existe pas, et il suggère de vérifier le nom de l'adresse réseau puis de réexécuter la commande. |  
|[Comptes (Accounts)](#Accounts)|Traite des conditions nécessaires à la configuration correcte des comptes sous lesquels [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] s'exécute.|  
|[Points de terminaison](#Endpoints)|Traite des conditions nécessaires à la configuration correcte du point de terminaison de mise en miroir de bases de données de chaque instance de serveur.|  
|[SystemAddress](#SystemAddress)|Résume les alternatives pour la spécification du nom système d'une instance de serveur dans une configuration de mise en miroir de bases de données.|  
|[Accès réseau](#NetworkAccess)|Traite de l'exigence selon laquelle chaque instance de serveur doit être autorisée à accéder aux ports de l'autre instance ou des autres instances de serveur par le biais du protocole TCP.|  
|[Préparation de la base de données miroir](#MirrorDbPrep)|Résume les conditions requises pour la préparation de la base de données miroir afin de permettre le démarrage de la mise en miroir.|  
|[Échec d'opération de création de fichier](#FailedCreateFileOp)|Décrit comment réagir à un échec d'opération de création de fichier.|  
|[Démarrage de la mise en miroir à l'aide de Transact-SQL](#StartDbm)|Décrit l’ordre requis pour les instructions ALTER DATABASE *nom_base_de_données* SET PARTNER **='** _serveur_partenaire_ **'** .|  
|[Transactions entre bases de données](#CrossDbTxns)|Un basculement automatique peut entraîner une résolution automatique et éventuellement incorrecte des transactions incertaines. Pour cette raison, la mise en miroir de bases de données ne prend pas en charge les transactions entre bases de données.|  
  
##  <a name="accounts"></a><a name="Accounts"></a> Comptes (Accounts)  
 Les comptes sous lesquels [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] est en cours d'exécution doivent être configurés correctement.  
  
1.  Les comptes possèdent-ils les autorisations appropriées ?  
  
    1.  Si les comptes fonctionnent dans les mêmes comptes de domaine, les risques de problèmes de configuration sont réduits.  
  
    2.  Si les comptes s'exécutent dans des domaines différents ou ne sont pas des comptes de domaine, la connexion d'un compte doit être créée dans la base de données **master** sur l'autre ordinateur et des autorisations CONNECT doivent être octroyées à cette connexion sur le point de terminaison. Pour plus d’informations, consultez [Gérer les métadonnées durant la mise à disposition d’une base de données sur une autre instance de serveur &#40;SQL Server&#41;](../../relational-databases/databases/manage-metadata-when-making-a-database-available-on-another-server.md). Ceci inclut le compte intégré Service réseau.  
  
2.  Si [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] est en cours d'exécution en tant que service utilisant le compte système local, vous devez utiliser des certificats pour l'authentification. Pour plus d'informations, consultez [Utiliser des certificats pour un point de terminaison de mise en miroir de bases de données &#40;Transact-SQL&#41;](../../database-engine/database-mirroring/use-certificates-for-a-database-mirroring-endpoint-transact-sql.md).  
  
##  <a name="endpoints"></a><a name="Endpoints"></a> Endpoints  
 Les points de terminaison doivent être correctement configurés.  
  
1.  Vérifiez que chaque instance de serveur (serveur principal, serveurs miroir et témoin, le cas échéant) a un point de terminaison de mise en miroir de bases de données. Pour plus d’informations, consultez [sys.database_mirroring_endpoints &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-database-mirroring-endpoints-transact-sql.md) et, selon la forme d’authentification, soit [Créer un point de terminaison de mise en miroir de bases de données pour l’authentification Windows &#40;Transact-SQL&#41;](../../database-engine/database-mirroring/create-a-database-mirroring-endpoint-for-windows-authentication-transact-sql.md), soit [Utiliser des certificats pour un point de terminaison de mise en miroir de bases de données &#40;Transact-SQL&#41;](../../database-engine/database-mirroring/use-certificates-for-a-database-mirroring-endpoint-transact-sql.md).  
  
2.  Vérifiez les numéros de ports.  
  
     Pour identifier le port actuellement associé au point de terminaison de mise en miroir de bases de données d’une instance de serveur, utilisez les affichages catalogue [sys.database_mirroring_endpoints](../../relational-databases/system-catalog-views/sys-database-mirroring-endpoints-transact-sql.md) et [sys.tcp_endpoints](../../relational-databases/system-catalog-views/sys-tcp-endpoints-transact-sql.md) .  
  
3.  Pour les problèmes de configuration de la mise en miroir de bases de données qu'il est difficile d'expliquer, nous vous conseillons d'examiner chaque instance de serveur pour déterminer si elle est à l'écoute sur les ports appropriés.   
  
4.  Vérifiez que les points de terminaison sont démarrés (STATE=STARTED). Sur chaque instance de serveur, utilisez l'instruction [!INCLUDE[tsql](../../includes/tsql-md.md)] ci-dessous.  
  
    ```  
    SELECT state_desc FROM sys.database_mirroring_endpoints  
    ```  
  
     Pour plus d’informations sur la colonne **state_desc**, consultez [sys.database_mirroring_endpoints &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-database-mirroring-endpoints-transact-sql.md).  
  
     Pour démarrer un point de terminaison, utilisez l'instruction [!INCLUDE[tsql](../../includes/tsql-md.md)] ci-dessous.  
  
    ```  
    ALTER ENDPOINT Endpoint_Mirroring   
    STATE = STARTED   
    AS TCP (LISTENER_PORT = <port_number>)  
    FOR database_mirroring (ROLE = ALL);  
    GO  
    ```  
  
     Pour plus d’informations, consultez [ALTER ENDPOINT &#40;Transact-SQL&#41;](../../t-sql/statements/alter-endpoint-transact-sql.md).  
  
5.  Vérifiez que le ROLE est correct. Sur chaque instance de serveur, utilisez l'instruction [!INCLUDE[tsql](../../includes/tsql-md.md)] ci-dessous.  
  
    ```  
    SELECT role FROM sys.database_mirroring_endpoints;  
    GO  
    ```  
  
     Pour plus d’informations, consultez [sys.database_mirroring_endpoints &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-database-mirroring-endpoints-transact-sql.md).  
  
6.  La connexion pour le compte de service de l'autre instance de serveur nécessite l'autorisation CONNECT. Assurez-vous que la connexion de l'autre serveur possède l'autorisation CONNECT. Pour déterminer qui possède une autorisation CONNECT pour un point de terminaison, sur chaque instance de serveur, utilisez l'instruction [!INCLUDE[tsql](../../includes/tsql-md.md)] ci-dessous.  
  
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
  
##  <a name="system-address"></a><a name="SystemAddress"></a> Adresse système  
 Comme nom système d'une instance de serveur dans une configuration de mise en miroir de bases de données, vous pouvez utiliser tout nom qui identifie sans ambiguïté le système. L'adresse de serveur peut être un nom système (si les systèmes sont dans le même domaine), un nom de domaine complet ou une adresse IP (de préférence statique). L'utilisation du nom de domaine complet garantit un fonctionnement correct. Pour plus d’informations, consultez [Spécifier une adresse réseau de serveur &#40;mise en miroir de bases de données&#41;](../../database-engine/database-mirroring/specify-a-server-network-address-database-mirroring.md).  
  
##  <a name="network-access"></a><a name="NetworkAccess"></a> Network Access  
 L'accès aux ports de l'autre instance ou des autres instances de serveur doit être autorisé via TCP à chaque instance de serveur. Ceci est d'autant plus important que les instances de serveur peuvent se trouver dans différents domaines, entre lesquels aucune relation d'approbation n'a été établie (domaines non approuvés). Ce qui réduit considérablement les communications entre les instances de serveur.  
  
##  <a name="mirror-database-preparation"></a><a name="MirrorDbPrep"></a> Mirror Database Preparation  
 Que vous démarriez la mise en miroir pour la première fois ou que vous la redémarriez après l'avoir supprimée, vérifiez que la base de données est préparée pour la mise en miroir.  
  
 Lors de la création de la base de données en miroir sur le serveur miroir, assurez-vous que vous restaurez la sauvegarde de la base de données principale en spécifiant le même nom de base de données à l'aide de WITH NORECOVERY. En outre, toutes les sauvegardes du journal créées après la réalisation de cette sauvegarde doivent également être appliquées, à nouveau avec WITH NORECOVERY.  
  
 En outre, nous recommandons, si possible, que le chemin d'accès au fichier (notamment la lettre de lecteur) de la base de données miroir soit identique au chemin d'accès de la base de données principale. Si les chemins d'accès des fichiers doivent différer, par exemple, si la base de données principale se trouve sur le lecteur « F: », mais que le système miroir n'a pas de lecteur F:, vous devez inclure l'option MOVE dans l'instruction RESTORE.  
  
> [!IMPORTANT]  
>  Si vous déplacez les fichiers de base de données pendant la création de la base de données miroir, vous risquez de ne pas pouvoir ajouter ultérieurement de fichiers à la base de données sans suspendre la mise en miroir.  
  
 Si la mise en miroir de bases de données a été arrêtée, toutes les sauvegardes du journal réalisées ultérieurement sur la base de données principale doivent être appliquées à la base de données miroir avant de redémarrer la mise en miroir.  
  
 Pour plus d’informations, consultez [Préparer une base de données miroir pour la mise en miroir &#40;SQL Server&#41;](../../database-engine/database-mirroring/prepare-a-mirror-database-for-mirroring-sql-server.md).  
  
##  <a name="failed-create-file-operation"></a><a name="FailedCreateFileOp"></a> Failed Create-File Operation  
 Pour pouvoir ajouter un fichier sans compromettre une session de mise en miroir, il est nécessaire que le chemin d'accès au fichier existe sur les deux serveurs. Par conséquent, si vous déplacez des fichiers de base de données lors de la création de la base de données miroir, une opération d'ajout de fichier ultérieure peut échouer sur la base de données miroir et entraîner la suspension de la mise en miroir.  
  
 Pour corriger le problème :  
  
1.  Le propriétaire de la base de données doit supprimer la session de mise en miroir et restaurer une sauvegarde complète du groupe de fichiers qui contient le fichier ajouté.  
  
2.  Le propriétaire doit ensuite sauvegarder le fichier journal contenant l'opération d'ajout de fichier sur le serveur principal et restaurer manuellement la sauvegarde de fichier journal sur la base de données miroir à l'aide des options WITH NORECOVERY et WITH MOVE. Cette opération crée le chemin d'accès spécifié sur le serveur miroir et restaure le nouveau fichier à cet emplacement.  
  
3.  Pour préparer la base de données pour une nouvelle session de mise en miroir, le propriétaire doit également restaurer à l'aide de WITH NO RECOVERY toutes les autres sauvegardes de fichiers journaux à partir du serveur principal.  
  
 Pour plus d’informations, consultez [Suppression de la mise en miroir de bases de données &#40;SQL Server&#41;](../../database-engine/database-mirroring/removing-database-mirroring-sql-server.md), [Préparer une base de données miroir pour la mise en miroir&#40;SQL Server&#41;](../../database-engine/database-mirroring/prepare-a-mirror-database-for-mirroring-sql-server.md), [Établir une session de mise en miroir de bases de données au moyen de l’authentification Windows &#40;Transact-SQL&#41;](../../database-engine/database-mirroring/database-mirroring-establish-session-windows-authentication.md), [Utiliser des certificats pour un point de terminaison de mise en miroir de bases de données &#40;Transact-SQL&#41;](../../database-engine/database-mirroring/use-certificates-for-a-database-mirroring-endpoint-transact-sql.md) ou [Établir une session de mise en miroir de bases de données au moyen de l’authentification Windows &#40;SQL Server Management Studio&#41;](../../database-engine/database-mirroring/establish-database-mirroring-session-windows-authentication.md).  
  
##  <a name="starting-mirroring-by-using-transact-sql"></a><a name="StartDbm"></a> Démarrage de la mise en miroir à l'aide de Transact-SQL  
 L’ordre dans lequel les instructions ALTER DATABASE *nom_base_de_données* SET PARTNER **='** _serveur_partenaire_ **'** sont émises est très important.  
  
1.  La première instruction doit être exécutée sur le serveur miroir. Lorsque cette instruction est exécutée, le serveur miroir n'essaie pas de contacter d'autre instance de serveur. Au lieu de cela, le serveur miroir indique à sa base de données de patienter que le serveur miroir soit contacté par le serveur principal.  
  
2.  La seconde instruction ALTER DATABASE doit être exécutée sur le serveur principal. Cette instruction spécifie au serveur principal d'essayer de se connecter au serveur miroir. Une fois cette connexion créée, le miroir essaie de se connecter au serveur principal via une autre connexion.  
  
 Pour plus d’informations, consultez [ALTER DATABASE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql.md).  
  
> [!NOTE]  
>  Pour plus d’informations sur l’utilisation de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] pour commencer la mise en miroir, consultez [Établir une session de mise en miroir de bases de données au moyen de l’authentification Windows &#40;SQL Server Management Studio&#41;](../../database-engine/database-mirroring/establish-database-mirroring-session-windows-authentication.md).  
  
##  <a name="cross-database-transactions"></a><a name="CrossDbTxns"></a> Transactions entre bases de données  
 Lorsqu'une base de données est mise en miroir en mode haute sécurité avec le basculement automatique, un basculement automatique peut entraîner une résolution automatique et éventuellement incorrecte des transactions incertaines. Si un basculement automatique se produit sur l'une des bases de données pendant la validation d'une transaction entre bases de données, des incohérences logiques peuvent survenir entre les bases de données.  
  
 Les types de transactions entre bases de données qui peuvent être affectés par un basculement automatique sont les suivants :  
  
-   Une transaction qui met à jour plusieurs bases de données dans la même instance [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
-   Les transactions qui utilisent [!INCLUDE[msCoName](../../includes/msconame-md.md)] Distributed Transaction Coordinator (MS DTC).  
  
 Pour plus d’informations, consultez [Transactions entre bases de données et transactions distribuées pour des groupes de disponibilité Always On et la mise en miroir de bases de données &#40;SQL Server&#41;](../../database-engine/availability-groups/windows/transactions-always-on-availability-and-database-mirroring.md).  
  
## <a name="see-also"></a>Voir aussi  
 [Configuration de la mise en miroir d’une base de données &#40;SQL Server&#41;](../../database-engine/database-mirroring/setting-up-database-mirroring-sql-server.md)   
 [Sécurité du transport de la mise en miroir de bases de données et des groupes de disponibilité Always On &#40;SQL Server&#41;](../../database-engine/database-mirroring/transport-security-database-mirroring-always-on-availability.md)  
  
  


