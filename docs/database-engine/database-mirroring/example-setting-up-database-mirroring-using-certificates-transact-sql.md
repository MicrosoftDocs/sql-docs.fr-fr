---
title: 'Exemple : Mise en miroir de bases de données avec certificats (T-SQL)'
description: Cet article contient un exemple de configuration de la mise en miroir de bases de données SQL Server avec l’authentification basée sur les certificats à l’aide de Transact-SQL dans SQL Server.
ms.custom: seo-lt-2019
ms.date: 05/17/2016
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: database-mirroring
ms.topic: conceptual
helpviewer_keywords:
- database mirroring [SQL Server], deployment
- certificates [SQL Server], database mirroring
- authentication [SQL Server], database mirroring
- database mirroring [SQL Server], security
ms.assetid: df489ecd-deee-465c-a26a-6d1bef6d7b66
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: 12459a76f612ba2e9acc915acf5189792512b2ed
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100342345"
---
# <a name="example-setting-up-database-mirroring-using-certificates-transact-sql"></a>Exemple : Configuration de la mise en miroir de bases de données à l’aide de certificats (Transact-SQL)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  Cet exemple décrit toutes les étapes de création d'une session de mise en miroir de bases de données à l'aide de l'authentification basée sur les certificats. Les exemples de cette rubrique utilisent [!INCLUDE[tsql](../../includes/tsql-md.md)]. À moins que vous ne puissiez garantir la sécurité de votre réseau, il est recommandé d'utiliser le chiffrement pour les connexions de mise en miroir de bases de données.  
  
 Lors de la copie d'un certificat sur un autre système, utilisez une méthode de copie sécurisée. Veillez particulièrement à sécuriser tous vos certificats.  
  
##  <a name="example"></a><a name="ExampleH2"></a> Exemple  
 L'exemple suivant illustre ce qui doit être fait sur un serveur partenaire qui réside sur HOST_A. Dans cet exemple, les deux serveurs partenaires sont les instances de serveur par défaut réparties sur trois systèmes informatiques. Les deux instances de serveur sont exécutées dans des domaines Windows non approuvés, par conséquent l'authentification basée sur les certificats est nécessaire.  
  
 Le rôle principal initial est occupé par HOST_A, et le rôle miroir par HOST_B.  
  
 La configuration de la mise en miroir de bases de données à l’aide de certificats implique quatre étapes générales, dont les première, deuxième et quatrième sont illustrées par cet exemple. Ces étapes sont les suivantes :  
  
1.  [Configuration des connexions sortantes](#ConfiguringOutboundConnections)  
  
     Cet exemple montre les étapes nécessaires à la :  
  
    1.  configuration d'Host_A pour les connexions sortantes ;  
  
    2.  configuration d'Host_B pour les connexions sortantes.  
  
     Pour plus d’informations sur cette étape de la configuration de la mise en miroir de bases de données, consultez [Autoriser un point de terminaison de mise en miroir de bases de données à utiliser des certificats pour les connexions sortantes &#40;Transact-SQL&#41;](../../database-engine/database-mirroring/database-mirroring-use-certificates-for-outbound-connections.md).  
  
2.  [Configuration des connexions entrantes](#ConfigureInboundConnections)  
  
     Cet exemple montre les étapes nécessaires à la :  
  
    1.  configuration d'Host_A pour les connexions entrantes ;  
  
    2.  configuration d'Host_B pour les connexions entrantes.  
  
     Pour plus d’informations sur cette étape de la configuration de la mise en miroir de bases de données, consultez [Autoriser un point de terminaison de mise en miroir de bases de données à utiliser des certificats pour les connexions entrantes &#40;Transact-SQL&#41;](../../database-engine/database-mirroring/database-mirroring-use-certificates-for-inbound-connections.md).  
  
3.  Création de la base de données miroir  
  
     Pour plus d’informations sur la création d’une base de données miroir, consultez [Préparer une base de données miroir pour la mise en miroir &#40;SQL Server&#41;](../../database-engine/database-mirroring/prepare-a-mirror-database-for-mirroring-sql-server.md).  
  
4.  [Configuration des serveurs partenaires de mise en miroir](#ConfigureMirroringPartners)  
  
###  <a name="configuring-outbound-connections"></a><a name="ConfiguringOutboundConnections"></a> Configuration des connexions sortantes  
 **Pour configurer Host_A pour les connexions sortantes**  
  
1.  Dans la base de données master, créez la clé principale de base de données, si nécessaire.  
  
    ```  
    USE master;  
    CREATE MASTER KEY ENCRYPTION BY PASSWORD = '<1_Strong_Password!>';  
    GO  
    ```  
  
2.  Activez un certificat pour cette instance de serveur.  
  
    ```  
    USE master;  
    CREATE CERTIFICATE HOST_A_cert   
       WITH SUBJECT = 'HOST_A certificate';  
    GO  
    ```  
  
3.  Créez un point de terminaison de mise en miroir pour l'instance de serveur à l'aide du certificat.  
  
    ```  
    CREATE ENDPOINT Endpoint_Mirroring  
       STATE = STARTED  
       AS TCP (  
          LISTENER_PORT=7024  
          , LISTENER_IP = ALL  
       )   
       FOR DATABASE_MIRRORING (   
          AUTHENTICATION = CERTIFICATE HOST_A_cert  
          , ENCRYPTION = REQUIRED ALGORITHM AES  
          , ROLE = ALL  
       );  
    GO  
    ```  
  
4.  Sauvegardez le certificat HOST_A, et copiez-le sur l'autre système, HOST_B.  
  
    ```  
    BACKUP CERTIFICATE HOST_A_cert TO FILE = 'C:\HOST_A_cert.cer';  
    GO  
    ```  
  
5.  Au moyen d'une méthode sécurisée de copie quelconque, copiez C:\HOST_A_cert.cer sur HOST_B.  
  
 **Pour configurer Host_B pour les connexions sortantes**  
  
1.  Dans la base de données master, créez la clé principale de base de données, si nécessaire.  
  
    ```  
    USE master;  
    CREATE MASTER KEY ENCRYPTION BY PASSWORD = '<Strong_Password_#2>';  
    GO  
    ```  
  
2.  Activez un certificat sur l'instance de serveur HOST_B.  
  
    ```  
    CREATE CERTIFICATE HOST_B_cert   
       WITH SUBJECT = 'HOST_B certificate for database mirroring';  
    GO  
    ```  
  
3.  Créez un point de terminaison de mise en miroir pour l'instance de serveur sur HOST_B.  
  
    ```  
    CREATE ENDPOINT Endpoint_Mirroring  
       STATE = STARTED  
       AS TCP (  
          LISTENER_PORT=7024  
          , LISTENER_IP = ALL  
       )   
       FOR DATABASE_MIRRORING (   
          AUTHENTICATION = CERTIFICATE HOST_B_cert  
          , ENCRYPTION = REQUIRED ALGORITHM AES  
          , ROLE = ALL  
       );  
    GO  
    ```  
  
4.  Sauvegardez le certificat HOST_B.  
  
    ```  
    BACKUP CERTIFICATE HOST_B_cert TO FILE = 'C:\HOST_B_cert.cer';  
    GO   
    ```  
  
5.  Au moyen d'une méthode sécurisée de copie quelconque, copiez C:\HOST_B_cert.cer sur HOST_A.  
  
 Pour plus d’informations, consultez [Autoriser un point de terminaison de mise en miroir de bases de données à utiliser des certificats pour les connexions sortantes &#40;Transact-SQL&#41;](../../database-engine/database-mirroring/database-mirroring-use-certificates-for-outbound-connections.md).  
  
 [&#91;Début de l’exemple&#93;](#ExampleH2)  
  
###  <a name="configuring-inbound-connections"></a><a name="ConfigureInboundConnections"></a> Configuration des connexions entrantes  
 **Pour configurer Host_A pour les connexions entrantes**  
  
1.  Créez une connexion sur HOST_A pour HOST_B.  
  
    ```  
    USE master;  
    CREATE LOGIN HOST_B_login WITH PASSWORD = '1Sample_Strong_Password!@#';  
    GO  
    ```  
  
2.  --Créez un utilisateur pour cette connexion.  
  
    ```  
    CREATE USER HOST_B_user FOR LOGIN HOST_B_login;  
    GO  
    ```  
  
3.  --Associez le certificat à l'utilisateur.  
  
    ```  
    CREATE CERTIFICATE HOST_B_cert  
       AUTHORIZATION HOST_B_user  
       FROM FILE = 'C:\HOST_B_cert.cer'  
    GO  
    ```  
  
4.  Accordez l'autorisation CONNECT à la connexion pour le point de terminaison de mise en miroir distant.  
  
    ```  
    GRANT CONNECT ON ENDPOINT::Endpoint_Mirroring TO [HOST_B_login];  
    GO  
    ```  
  
 **Pour configurer Host_B pour les connexions entrantes**  
  
1.  Créez une connexion sur HOST_B pour HOST_A.  
  
    ```  
    USE master;  
    CREATE LOGIN HOST_A_login WITH PASSWORD = '=Sample#2_Strong_Password2';  
    GO  
    ```  
  
2.  Créez un utilisateur pour cette connexion.  
  
    ```  
    CREATE USER HOST_A_user FOR LOGIN HOST_A_login;  
    GO  
    ```  
  
3.  Associez le certificat à l'utilisateur.  
  
    ```  
    CREATE CERTIFICATE HOST_A_cert  
       AUTHORIZATION HOST_A_user  
       FROM FILE = 'C:\HOST_A_cert.cer'  
    GO  
    ```  
  
4.  Accordez l'autorisation CONNECT à la connexion pour le point de terminaison de mise en miroir distant.  
  
    ```  
    GRANT CONNECT ON ENDPOINT::Endpoint_Mirroring TO [HOST_A_login];  
    GO  
    ```  
  
> [!IMPORTANT]  
>  Si vous envisagez d'utiliser le mode haute sécurité avec basculement automatique, vous devez répéter les mêmes étapes pour configurer le témoin pour les connexions sortantes et entrantes. La configuration des connexions entrantes lorsqu'un serveur témoin est impliqué suppose de configurer les connexions et les utilisateurs du serveur témoin sur les deux serveurs partenaires, ainsi que les connexions et les utilisateurs des deux serveurs partenaires sur le serveur témoin.  
  
 Pour plus d’informations, consultez [Autoriser un point de terminaison de mise en miroir de bases de données à utiliser des certificats pour les connexions entrantes &#40;Transact-SQL&#41;](../../database-engine/database-mirroring/database-mirroring-use-certificates-for-inbound-connections.md).  
  
 [&#91;Début de l’exemple&#93;](#ExampleH2)  
  
### <a name="creating-the-mirror-database"></a>Création de la base de données miroir  
 Pour plus d’informations sur la création d’une base de données miroir, consultez [Préparer une base de données miroir pour la mise en miroir &#40;SQL Server&#41;](../../database-engine/database-mirroring/prepare-a-mirror-database-for-mirroring-sql-server.md).  
  
###  <a name="configuring-the-mirroring-partners"></a><a name="ConfigureMirroringPartners"></a> Configuration des serveurs partenaires de mise en miroir  
  
1.  Sur l'instance de serveur miroir de HOST_B, définissez l'instance de serveur de HOST_A en tant que serveur partenaire (en faisant d'elle l'instance initiale de serveur principal). Remplacez une adresse réseau valide par `TCP://HOST_A.Mydomain.Corp.Adventure-Works``.com:7024`. Pour plus d’informations, consultez [Spécifier une adresse réseau de serveur &#40;mise en miroir de bases de données&#41;](../../database-engine/database-mirroring/specify-a-server-network-address-database-mirroring.md).  
  
    ```  
    --At HOST_B, set server instance on HOST_A as partner (principal server):  
    ALTER DATABASE AdventureWorks   
        SET PARTNER = 'TCP://HOST_A.Mydomain.Corp.Adventure-Works.com:7024';  
    GO  
    ```  
  
2.  Sur l'instance de serveur principal de HOST_A, définissez l'instance de serveur de HOST_B en tant que serveur partenaire (en faisant d'elle l'instance initiale de serveur miroir). Remplacez une adresse réseau valide par `TCP://HOST_B.Mydomain.Corp.Adventure-Works.com:7024`.  
  
    ```  
    --At HOST_A, set server instance on HOST_B as partner (mirror server).  
    ALTER DATABASE AdventureWorks   
        SET PARTNER = 'TCP://HOST_B.Mydomain.Corp.Adventure-Works.com:7024';  
    GO  
    ```  
  
3.  Cet exemple suppose que la session est exécutée en mode hautes performances. Pour configurer cette session au mode hautes performances, sur l'instance de serveur principal (sur HOST_A), désactivez la sécurité des transactions.  
  
    ```  
    --Change to high-performance mode by turning off transacton safety.  
    ALTER DATABASE AdventureWorks   
        SET PARTNER SAFETY OFF  
    GO  
    ```  
  
    > [!NOTE]  
    >  Si vous envisagez d’utiliser le mode haute sécurité avec basculement automatique, laissez la sécurité des transactions définie avec la valeur FULL (valeur par défaut) et ajoutez dès que possible le témoin après l’exécution de la deuxième instruction SET PARTNER **’** _serveur_partenaire_ **’** . Notez que le serveur témoin doit d'abord être configuré pour les connexions sortantes et entrantes.  
  
 [&#91;Début de l’exemple&#93;](#ExampleH2)  
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> Tâches associées  
  
-   [Préparer une base de données miroir pour la mise en miroir &#40;SQL Server&#41;](../../database-engine/database-mirroring/prepare-a-mirror-database-for-mirroring-sql-server.md)  
  
-   [Autoriser un point de terminaison de mise en miroir de bases de données à utiliser des certificats pour les connexions entrantes &#40;Transact-SQL&#41;](../../database-engine/database-mirroring/database-mirroring-use-certificates-for-inbound-connections.md)  
  
-   [Autoriser un point de terminaison de mise en miroir de bases de données à utiliser des certificats pour les connexions sortantes &#40;Transact-SQL&#41;](../../database-engine/database-mirroring/database-mirroring-use-certificates-for-outbound-connections.md)  
  
-   [Gestion des connexions et des travaux après un basculement de rôle &#40;SQL Server&#41;](../../sql-server/failover-clusters/management-of-logins-and-jobs-after-role-switching-sql-server.md)  
  
-   [Gérer les métadonnées durant la mise à disposition d’une base de données sur une autre instance de serveur &#40;SQL Server&#41;](../../relational-databases/databases/manage-metadata-when-making-a-database-available-on-another-server.md) (SQL Server)  
  
-   [Résoudre des problèmes de configuration de mise en miroir de bases de données &#40;SQL Server&#41;](../../database-engine/database-mirroring/troubleshoot-database-mirroring-configuration-sql-server.md)  
  
## <a name="see-also"></a>Voir aussi  
 [Sécurité du transport de la mise en miroir de bases de données et des groupes de disponibilité Always On &#40;SQL Server&#41;](../../database-engine/database-mirroring/transport-security-database-mirroring-always-on-availability.md)   
 [Spécifier une adresse réseau de serveur &#40;mise en miroir de bases de données&#41;](../../database-engine/database-mirroring/specify-a-server-network-address-database-mirroring.md)   
 [Point de terminaison de mise en miroir de bases de données &#40;SQL Server&#41;](../../database-engine/database-mirroring/the-database-mirroring-endpoint-sql-server.md)   
 [Utiliser des certificats pour un point de terminaison de mise en miroir de bases de données &#40;Transact-SQL&#41;](../../database-engine/database-mirroring/use-certificates-for-a-database-mirroring-endpoint-transact-sql.md)   
 [ALTER DATABASE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql.md)   
 [Centre de sécurité pour le moteur de base de données SQL Server et Azure SQL Database](../../relational-databases/security/security-center-for-sql-server-database-engine-and-azure-sql-database.md)  
  
  
