---
description: Utiliser des certificats pour un point de terminaison de mise en miroir de bases de données (Transact-SQL)
title: Utiliser des certificats pour un point de terminaison de mise en miroir de bases de données
descriptoin: Steps to configure the use of a certificate on both inbound and outbound connections for a SQL Server database mirroring endpoint.
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
ms.assetid: f7c23cc2-48dc-4b78-b441-89ca29a0bd9e
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: 93d25bca0427698a56b77c8e6024d621ba7114b4
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100352171"
---
# <a name="use-certificates-for-a-database-mirroring-endpoint-transact-sql"></a>Utiliser des certificats pour un point de terminaison de mise en miroir de bases de données (Transact-SQL)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  Pour activer l'authentification des certificats en vue de la mise en miroir de bases de données sur une instance déterminée du serveur, l'administrateur système doit configurer chaque instance du serveur afin d'utiliser les certificats à la fois sur les connexions sortantes et entrantes. Les connexions sortantes doivent être configurées en premier.  
  
> [!NOTE]  
>  Toutes les connexions de mise en miroir situées sur une instance du serveur utilisent un point de terminaison de mise en miroir de bases de données unique, et vous devez spécifier la méthode d'authentification de l'instance du serveur au moment de la création de ce point de terminaison. Par conséquent, vous ne pouvez utiliser qu'une forme d'authentification par instance du serveur pour la mise en miroir de bases de données.  
  
## <a name="configuring-outbound-connections"></a>Configuration des connexions sortantes  
 Procédez comme suit sur chaque instance du serveur que vous configurez pour la mise en miroir de bases de données :  
  
1.  Dans la base de données **master** , créez une clé principale de base de données.  
  
2.  Dans la base de données **master** , créez un certificat chiffré sur l’instance du serveur.  
  
3.  Créez un point de terminaison pour l'instance du serveur à l'aide de son certificat.  
  
4.  Sauvegardez le certificat dans un fichier et copiez-le par sécurité sur un ou plusieurs autres systèmes.  
  
 Vous devez exécuter cette procédure pour chaque partenaire et pour le témoin éventuel.  
  
 Pour plus d’informations, consultez [Autoriser un point de terminaison de mise en miroir de bases de données à utiliser des certificats pour les connexions sortantes &#40;Transact-SQL&#41;](../../database-engine/database-mirroring/database-mirroring-use-certificates-for-outbound-connections.md).  
  
## <a name="configuring-inbound-connections"></a>Configuration des connexions entrantes  
 Ensuite, procédez comme suit pour chaque partenaire que vous configurez pour la mise en miroir de bases de données. Dans la base de données **master** :  
  
1.  Créez une connexion pour l'autre système.  
  
2.  Créez un utilisateur pour cette connexion.  
  
3.  Obtenez le certificat pour la mise en miroir du point de terminaison de l'autre instance du serveur.  
  
4.  Associez le certificat à l'utilisateur créé à l'étape 2.  
  
5.  Accordez à ce point de terminaison de mise en miroir l'autorisation CONNECT sur la connexion.  
  
 S'il existe un témoin, vous devez également configurer les connexions entrantes pour celui-ci. Cela exige la définition des noms de connexion, des utilisateurs et des certificats pour le témoin sur les deux partenaires, et inversement.  
  
 Pour plus d’informations, consultez [Autoriser un point de terminaison de mise en miroir de bases de données à utiliser des certificats pour les connexions entrantes &#40;Transact-SQL&#41;](../../database-engine/database-mirroring/database-mirroring-use-certificates-for-inbound-connections.md).  
  
## <a name="security"></a>Sécurité  
 À moins que vous ne puissiez garantir la sécurité de votre réseau, il est recommandé d'utiliser le chiffrement pour les connexions de mise en miroir de bases de données. Pour plus d’informations, consultez [Point de terminaison de mise en miroir de bases de données &#40;SQL Server&#41;](../../database-engine/database-mirroring/the-database-mirroring-endpoint-sql-server.md).  
  
 Lors de la copie d'un certificat sur un autre système, utilisez une méthode de copie sécurisée. Veillez particulièrement à sécuriser tous vos certificats.  
  
## <a name="see-also"></a>Voir aussi  
 [Créer une clé principale de base de données](../../relational-databases/security/encryption/create-a-database-master-key.md)   
 [CREATE MASTER KEY &#40;Transact-SQL&#41;](../../t-sql/statements/create-master-key-transact-sql.md)   
 [Sécurité du transport de la mise en miroir de bases de données et des groupes de disponibilité Always On &#40;SQL Server&#41;](../../database-engine/database-mirroring/transport-security-database-mirroring-always-on-availability.md)   
 [Centre de sécurité pour le moteur de base de données SQL Server et Azure SQL Database](../../relational-databases/security/security-center-for-sql-server-database-engine-and-azure-sql-database.md)   
 [Point de terminaison de mise en miroir de bases de données &#40;SQL Server&#41;](../../database-engine/database-mirroring/the-database-mirroring-endpoint-sql-server.md)  
  
  
