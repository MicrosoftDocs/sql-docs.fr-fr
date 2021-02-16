---
title: Mise en miroir de bases de données - Utiliser des certificats pour les connexions sortrantes | Microsoft Docs
description: Découvrez comment configurer les instances de serveur afin qu’elles utilisent des certificats pour authentifier les connexions entrantes dans le cadre de la mise en miroir de bases de donnée, après avoir configuré les connexions sortantes.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: database-mirroring
ms.topic: conceptual
helpviewer_keywords:
- certificates [SQL Server], database mirroring
- outbound connections [SQL Server]
- database mirroring [SQL Server], security
ms.assetid: 464c9096-10d6-4c5e-8bb1-19acba27ad9e
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: a53b3c3c871b62ac8824d170f71035d4359fa09f
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100349194"
---
# <a name="database-mirroring---use-certificates-for-outbound-connections"></a>Mise en miroir de bases de données - Utiliser des certificats pour les connexions sortrantes
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  Cette rubrique explique comment configurer les instances de serveur de sorte qu'elles utilisent des certificats pour authentifier les connexions sortantes de la mise en miroir de bases de données. La configuration des connexions sortantes doit être effectuée avant celle des connexions entrantes.  
  
> [!NOTE]  
>  Toutes les connexions de mise en miroir situées sur une instance du serveur utilisent un point de terminaison de mise en miroir de bases de données unique, et vous devez spécifier la méthode d'authentification de l'instance du serveur au moment de la création de ce point de terminaison.  
  
 Le processus de configuration des connexions sortantes comprend les étapes générales suivantes :  
  
1.  Dans la base de données **MASTER** , créez une clé principale de base de données.  
  
2.  Dans la base de données **master** , créez un certificat chiffré sur l’instance du serveur.  
  
3.  Créez un point de terminaison pour l'instance du serveur à l'aide de son certificat.  
  
4.  Sauvegardez le certificat dans un fichier et copiez-le par sécurité sur un ou plusieurs autres systèmes.  
  
 Vous devez exécuter cette procédure pour chaque partenaire et pour le témoin éventuel.  
  
 La procédure suivante décrit ces étapes en détail. Pour chaque étape, la procédure fournit un exemple de configuration d'une instance du serveur sur un système nommé HOST_A. La section Exemple qui l'accompagne explique les mêmes étapes pour une autre instance du serveur sur un système nommé HOST_B.  
  
## <a name="procedure"></a>Procédure  
  
#### <a name="to-configure-server-instances-for-outbound-mirroring-connections-on-host_a"></a>Pour configurer les instances du serveur pour les connexions de mise en miroir sortantes (sur HOST_A)  
  
1.  Dans la base de données **MASTER** , créez la clé principale de base de données, s’il n’en existe aucune. Pour afficher les clés existantes d’une base de données, utilisez l’affichage catalogue [sys.symmetric_keys](../../relational-databases/system-catalog-views/sys-symmetric-keys-transact-sql.md) .  
  
     Pour créer la clé principale de base de données, utilisez la commande [!INCLUDE[tsql](../../includes/tsql-md.md)] suivante :  
  
    ```  
    CREATE MASTER KEY ENCRYPTION BY PASSWORD = '<1_Strong_Password!>';  
    GO  
    ```  
  
     Utilisez un mot de passe fort et unique, puis enregistrez-le dans un lieu sûr.  
  
     Pour plus d’informations, consultez [CREATE MASTER KEY &#40;Transact-SQL&#41;](../../t-sql/statements/create-master-key-transact-sql.md) et [Créer une clé principale de base de données](../../relational-databases/security/encryption/create-a-database-master-key.md).  
  
2.  Dans la base de données **MASTER**, créez un certificat chiffré sur l’instance de serveur à utiliser pour ses connexions sortantes de la mise en miroir de bases de données.  
  
     Par exemple, pour créer un certificat pour le système HOST_A :  
  
    > [!IMPORTANT]  
    >  Si vous envisagez d'utiliser le certificat pendant plusieurs années, spécifiez la date d'expiration en heure UTC à l'aide de l'option EXPIRY_DATE dans votre instruction CREATE CERTIFICATE. Nous vous recommandons également d'utiliser SQL Server Management Studio pour créer une règle de gestion basée sur des stratégies pour vous alerter lorsque vos certificats expirent. À l’aide de la boîte de dialogue de gestion de la stratégie **Créer une nouvelle condition**, créez cette règle sur le champ **\@ExpirationDate** de la facette **Certificat**. Pour plus d’informations, consultez [Administrer des serveurs à l’aide de la gestion basée sur des stratégies](../../relational-databases/policy-based-management/administer-servers-by-using-policy-based-management.md) et [Sécurisation de SQL Server](../../relational-databases/security/securing-sql-server.md).  
  
    ```  
    USE master;  
    CREATE CERTIFICATE HOST_A_cert   
       WITH SUBJECT = 'HOST_A certificate for database mirroring',   
       EXPIRY_DATE = '11/30/2013';  
    GO  
    ```  
  
     Pour plus d’informations, consultez [CREATE CERTIFICATE &#40;Transact-SQL&#41;](../../t-sql/statements/create-certificate-transact-sql.md).  
  
     Pour afficher les certificats stockés dans la base de données **MASTER**, vous pouvez utiliser les instructions [!INCLUDE[tsql](../../includes/tsql-md.md)] suivantes :  
  
    ```  
    USE master;  
    SELECT * FROM sys.certificates;  
    ```  
  
     Pour plus d’informations, consultez [sys.certificates &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-certificates-transact-sql.md).  
  
3.  Vérifiez que le point de terminaison de mise en miroir de bases de données existe sur chacune des instances de serveur.  
  
     Si un point de terminaison de mise en miroir de bases de données existe déjà pour l'instance de serveur, vous devez le réutiliser pour toutes les autres sessions que vous établissez sur cette instance. Pour déterminer si un point de terminaison de mise en miroir de bases de données existe sur une instance de serveur et pour afficher sa configuration, utilisez l'instruction suivante :  
  
    ```  
    SELECT name, role_desc, state_desc, connection_auth_desc, encryption_algorithm_desc   
       FROM sys.database_mirroring_endpoints;  
    ```  
  
     Si aucun point de terminaison n'existe, créez un point de terminaison qui utilise ce certificat pour les connexions sortantes et qui a recours à ses informations d'identification à des fins de vérification sur l'autre système. Il s'agit d'un point de terminaison de niveau serveur utilisé par toutes les sessions de mise en miroir auxquelles participe l'instance de serveur.  
  
     L'instruction suivante permet de créer un point de terminaison de mise en miroir pour l'exemple d'instance de serveur de HOST_A :  
  
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
  
     Pour plus d’informations, consultez [CREATE ENDPOINT &#40;Transact-SQL&#41;](../../t-sql/statements/create-endpoint-transact-sql.md).  
  
4.  Sauvegardez le certificat et copiez-le sur le ou les autres systèmes. Cette opération est nécessaire pour configurer les connexions entrantes sur l'autre système.  
  
    ```  
    BACKUP CERTIFICATE HOST_A_cert TO FILE = 'C:\HOST_A_cert.cer';  
    GO  
    ```  
  
     Pour plus d’informations, consultez [BACKUP CERTIFICATE &#40;Transact-SQL&#41;](../../t-sql/statements/backup-certificate-transact-sql.md).  
  
     Copiez ce certificat en utilisant la méthode sécurisée de votre choix. Veillez particulièrement à sécuriser tous vos certificats.  
  
 L'exemple de code des étapes précédentes configure des connexions sortantes sur HOST_A.  
  
 Vous devez maintenant effectuer les étapes équivalentes pour configurer des connexions sortantes sur HOST_B. Ces étapes sont illustrées dans la section « Exemple » ci-après.  
  
## <a name="example"></a>Exemple  
 L'exemple ci-dessous illustre la configuration de HOST_B pour la prise en charge des connexions sortantes.  
  
```  
USE master;  
--Create the database Master Key, if needed.  
CREATE MASTER KEY ENCRYPTION BY PASSWORD = '<Strong_Password_#2>';  
GO  
-- Make a certifcate on HOST_B server instance.  
CREATE CERTIFICATE HOST_B_cert   
   WITH SUBJECT = 'HOST_B certificate for database mirroring',   
   EXPIRY_DATE = '11/30/2013';  
GO  
--Create a mirroring endpoint for the server instance on HOST_B.  
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
--Backup HOST_B certificate.  
BACKUP CERTIFICATE HOST_B_cert TO FILE = 'C:\HOST_B_cert.cer';  
GO   
--Using any secure copy method, copy C:\HOST_B_cert.cer to HOST_A.  
```  
  
 Copiez le certificat sur l'autre système en utilisant la méthode sécurisée de votre choix. Veillez particulièrement à sécuriser tous vos certificats.  
  
> [!IMPORTANT]  
>  Une fois que vous avez défini les connexions sortantes, vous devez configurer les connexions entrantes sur chacune des instances du serveur pour l'instance ou les instances de l'autre serveur. Pour plus d’informations, consultez [Autoriser un point de terminaison de mise en miroir de bases de données à utiliser des certificats pour les connexions entrantes &#40;Transact-SQL&#41;](../../database-engine/database-mirroring/database-mirroring-use-certificates-for-inbound-connections.md).  
  
 Pour plus d’informations sur la création d’une base de données miroir, et obtenir un exemple Transact-SQL, consultez [Préparer une base de données miroir pour la mise en miroir &#40;SQL Server&#41;](../../database-engine/database-mirroring/prepare-a-mirror-database-for-mirroring-sql-server.md).  
  
 Pour obtenir un exemple Transact-SQL d’établissement d’une session en mode hautes performances, consultez [Exemple : Configuration de la mise en miroir de bases de données à l’aide de certificats &#40;Transact-SQL&#41;](../../database-engine/database-mirroring/example-setting-up-database-mirroring-using-certificates-transact-sql.md).  
  
## <a name="net-framework-security"></a>Sécurité du .NET Framework  
 À moins que vous ne puissiez garantir la sécurité de votre réseau, il est recommandé d'utiliser le chiffrement pour les connexions de mise en miroir de bases de données.  
  
 Lors de la copie d'un certificat sur un autre système, utilisez une méthode de copie sécurisée.  
  
## <a name="see-also"></a>Voir aussi  
 [Choisir un algorithme de chiffrement](../../relational-databases/security/encryption/choose-an-encryption-algorithm.md)   
 [Préparer une base de données miroir pour la mise en miroir &#40;SQL Server&#41;](../../database-engine/database-mirroring/prepare-a-mirror-database-for-mirroring-sql-server.md)   
 [ALTER ENDPOINT &#40;Transact-SQL&#41;](../../t-sql/statements/alter-endpoint-transact-sql.md)   
 [Exemple : Configuration de la mise en miroir de bases de données à l’aide de certificats &#40;Transact-SQL&#41;](../../database-engine/database-mirroring/example-setting-up-database-mirroring-using-certificates-transact-sql.md)   
 [Point de terminaison de mise en miroir de bases de données &#40;SQL Server&#41;](../../database-engine/database-mirroring/the-database-mirroring-endpoint-sql-server.md)   
 [Résolution des problèmes de configuration de mise en miroir de bases de données &#40;SQL Server&#41;](../../database-engine/database-mirroring/troubleshoot-database-mirroring-configuration-sql-server.md)   
 [Configurer une base de données miroir chiffrée](../../database-engine/database-mirroring/set-up-an-encrypted-mirror-database.md)  
  
  
