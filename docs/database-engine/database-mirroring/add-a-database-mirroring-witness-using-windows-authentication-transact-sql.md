---
title: Configurer un témoin
description: 'Explique comment configurer un témoin de mise en miroir de bases de données avec l’authentification Windows à l’aide de Transact-SQL. '
ms.custom: seo-lt-2019
ms.date: 03/07/2017
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: database-mirroring
ms.topic: conceptual
helpviewer_keywords:
- witness [SQL Server], establishing
- Windows authentication [SQL Server]
- database mirroring [SQL Server], witness
ms.assetid: bf5e87df-91a4-49f9-ae88-2a6dcf644510
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: a5702769994efd65abeeb330205a2a83362fee6e
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100353249"
---
# <a name="add-a-database-mirroring-witness-using-windows-authentication-transact-sql"></a>Ajouter un témoin de mise en miroir de bases de données à l'aide de l'authentification Windows (Transact-SQL)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  Pour configurer un témoin pour une base de données, le propriétaire de la base de données attribue à une instance de moteur de base de données le rôle de serveur témoin. L'instance de serveur témoin peut être exécutée sur le même ordinateur que l'instance de serveur principal ou miroir, mais cela réduit alors considérablement la robustesse du basculement automatique.  
  
 Nous vous recommandons fortement de faire résider le témoin sur un ordinateur séparé. Un serveur donné peut participer à plusieurs sessions simultanées de mise en miroir de bases de données avec des partenaires identiques ou différents. Un serveur donné peut être partenaire dans certaines sessions et témoin dans d'autres.  
  
 Le témoin est destiné uniquement au mode haute sécurité avec basculement automatique. Avant de définir un témoin, nous vous recommandons fortement de vérifier que la propriété SAFETY a la valeur active FULL.  
  
> [!IMPORTANT]  
>  Nous vous recommandons de configurer la mise en miroir de bases de données durant les heures creuses, car cela peut affecter les performances.  
  
## <a name="establish-a-witness"></a>Établir un témoin  
  
1.  Sur l'instance de serveur témoin, assurez-vous qu'un point de terminaison existe pour la mise en miroir de bases de données. Quel que soit le nombre de sessions de mise en miroir à prendre en charge, l'instance de serveur ne doit disposer que d'un seul point de terminaison de mise en miroir de bases de données. Si vous voulez utiliser cette instance de serveur exclusivement comme témoin dans des sessions de mise en miroir de bases de données, attribuez le rôle de témoin au point de terminaison (ROLE **=** WITNESS). Si vous souhaitez utiliser cette instance de serveur comme partenaire dans une ou plusieurs sessions de mise en miroir de bases de données, attribuez le rôle ALL au point de terminaison.  
  
     Pour exécuter une instruction SET WITNESS, la session de mise en miroir de bases de données doit déjà être démarrée (entre les partenaires) et la valeur STATE du point de terminaison du témoin doit être STARTED.  
  
     Pour savoir si l'instance de serveur témoin possède son point de terminaison de mise en miroir de bases de données et pour connaître son rôle et son état, utilisez sur cette instance l'instruction Transact-SQL suivante :  
  
    ```  
    SELECT role_desc, state_desc FROM sys.database_mirroring_endpoints  
    ```  
  
    > [!IMPORTANT]  
    >  Si le point de terminaison de mise en miroir de base de données existe et est déjà utilisé, nous vous recommandons d'utiliser ce point de terminaison pour toute session établie sur l'instance du serveur. La suppression d'un point de terminaison en cours d'utilisation perturbe les connexions des sessions existantes. Si un témoin a été défini pour une session, la suppression du point de terminaison de la mise en miroir peut provoquer la perte du quorum par le serveur principal de cette session ; si cela se produit, la base de données est mise en mode hors connexion et ses utilisateurs sont déconnectés. Pour plus d’informations, consultez [Quorum : effets d’un témoin sur la disponibilité de la base de données &#40;Mise en miroir de bases de données&#41;](../../database-engine/database-mirroring/quorum-how-a-witness-affects-database-availability-database-mirroring.md).  
  
     Si le témoin ne dispose pas d’un point de terminaison, consultez [Créer un point de terminaison de mise en miroir de bases de données pour l’authentification Windows &#40;Transact-SQL&#41;](../../database-engine/database-mirroring/create-a-database-mirroring-endpoint-for-windows-authentication-transact-sql.md).  
  
2.  Si les instances partenaires s'exécutent sous différents comptes d'utilisateurs de domaine, créez une connexion pour ces différents comptes dans la base de données master de chaque instance. Pour plus d’informations, consultez [Autoriser l’accès sur le réseau à un point de terminaison de mise en miroir de bases de données au moyen de l’authentification Windows &#40;SQL Server&#41;](../../database-engine/database-mirroring/database-mirroring-allow-network-access-windows-authentication.md).  
  
3.  Connectez-vous au serveur principal et exécutez l'instruction suivante :  
  
     ALTER DATABASE *<nom_base_de_données>* SET WITNESS **=** _<adresse_réseau_serveur>_  
  
     où *<nom_base_de_données>* est le nom de la base de données à mettre en miroir (ce nom est identique sur les deux partenaires) et *<adresse_réseau_serveur>* est l’adresse réseau du serveur de l’instance de serveur témoin.  
  
     La syntaxe pour une adresse réseau de serveur est la suivante :  
  
     TCP <b>://</b> _\<system-address>_ <b>:</b> _\<port>_  
  
     où \<*system-address>* est une chaîne qui identifie de façon non ambiguë l’ordinateur de destination, et \<*port>* le numéro de port utilisé par le point de terminaison de mise en miroir de l’instance de serveur partenaire. Pour plus d’informations, consultez [Spécifier une adresse réseau de serveur &#40;mise en miroir de bases de données&#41;](../../database-engine/database-mirroring/specify-a-server-network-address-database-mirroring.md).  
  
     Ainsi, sur l'instance de serveur principal, l'instruction ALTER DATABASE suivante définit le témoin. Le nom de la base de données est **AdventureWorks**, l’adresse système est DBSERVER3 (nom du système témoin) et le port utilisé par le point de terminaison de mise en miroir de bases de données du témoin est `7022` :  
  
    ```  
    ALTER DATABASE AdventureWorks   
      SET WITNESS = 'TCP://DBSERVER3:7022'  
    ```  
  
## <a name="example"></a>Exemple  
 L'exemple suivant installe un témoin de mise en miroir de bases de données. Sur l'instance du serveur témoin (instance par défaut sur `WITNESSHOST4`) :  
  
1.  Créez un point de terminaison pour cette instance de serveur, afin que le rôle WITNESS utilise uniquement le port `7022`.  
  
    ```  
    CREATE ENDPOINT Endpoint_Mirroring  
        STATE=STARTED   
        AS TCP (LISTENER_PORT=7022)   
        FOR DATABASE_MIRRORING (ROLE=WITNESS)  
    GO  
    ```  
  
2.  Créez une connexion pour les comptes d'utilisateurs de domaine des instances partenaires s'ils sont différents. Supposez, par exemple, que le témoin s'exécute sous `SOMEDOMAIN\witnessuser`tandis que les partenaires s'exécutent sous `MYDOMAIN\dbousername`. Créez une connexion pour les partenaires comme suit :  
  
    ```  
    --Create a login for the partner server instances,  
    --which are both running as MYDOMAIN\dbousername:  
    USE master ;  
    GO  
    CREATE LOGIN [MYDOMAIN\dbousername] FROM WINDOWS ;  
    GO  
    --Grant connect permissions on endpoint to login account   
    --of partners  
    GRANT CONNECT ON ENDPOINT::Endpoint_Mirroring TO [MYDOMAIN\dbousername];  
    GO  
    ```  
  
3.  Sur chacune des instances partenaires, créez une connexion pour l'instance de serveur témoin :  
  
    ```  
    --Create a login for the witness server instance,  
    --which is running as SOMEDOMAIN\witnessuser:  
    USE master ;  
    GO  
    CREATE LOGIN [SOMEDOMAIN\witnessuser] FROM WINDOWS ;  
    GO  
    --Grant connect permissions on endpoint to login account   
    --of partners  
    GRANT CONNECT ON ENDPOINT::Endpoint_Mirroring TO [SOMEDOMAIN\witnessuser];  
    GO  
    ```  
  
4.  Sur le serveur principal, définissez le témoin (qui se trouve sur `WITNESSHOST4`) :  
  
    ```  
    ALTER DATABASE AdventureWorks   
        SET WITNESS =   
        'TCP://WITNESSHOST4:7022'  
    GO  
    ```  
  
> [!NOTE]  
>  L'adresse réseau du serveur signale l'instance de serveur cible par son numéro de port qui est mappé avec le point de terminaison mis en miroir de l'instance.  
  
 Pour voir un exemple illustrant la configuration de la sécurité, la préparation de la base de données miroir, la définition des serveurs partenaires et l’ajout d’un témoin, consultez [Configuration de la mise en miroir d’une base de données &#40;SQL Server&#41;](../../database-engine/database-mirroring/setting-up-database-mirroring-sql-server.md).  
  
## <a name="see-also"></a>Voir aussi  
 [ALTER DATABASE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql.md)   
 [Autoriser l’accès sur le réseau à un point de terminaison de mise en miroir de bases de données au moyen de l’authentification Windows &#40;SQL Server&#41;](../../database-engine/database-mirroring/database-mirroring-allow-network-access-windows-authentication.md)   
 [Créer un point de terminaison de mise en miroir de bases de données pour l’authentification Windows &#40;Transact-SQL&#41;](../../database-engine/database-mirroring/create-a-database-mirroring-endpoint-for-windows-authentication-transact-sql.md)   
 [Établir une session de mise en miroir de bases de données au moyen de l’authentification Windows &#40;Transact-SQL&#41;](../../database-engine/database-mirroring/database-mirroring-establish-session-windows-authentication.md)   
 [Supprimer le témoin d’une session de mise en miroir de bases de données &#40;SQL Server&#41;](../../database-engine/database-mirroring/remove-the-witness-from-a-database-mirroring-session-sql-server.md)   
 [Témoin de mise en miroir de base de données](../../database-engine/database-mirroring/database-mirroring-witness.md)  
  
  
