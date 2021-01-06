---
title: 'Sécurité du transport : Groupes de disponibilité et mise en miroir de bases de données'
description: Découvrez comment sécuriser le transport des messages échangés entre les bases de données participant à un groupe de disponibilité Always On ou à une session de mise en miroir de bases de données hébergée sur SQL Server.
ms.custom: seo-lt-2019
ms.date: 05/17/2016
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: database-mirroring
ms.topic: conceptual
helpviewer_keywords:
- sessions [SQL Server], database mirroring
- cryptography [SQL Server], database mirroring
- certificates [SQL Server], database mirroring
- encryption [SQL Server], database mirroring
- Windows authentication [SQL Server]
- authentication [SQL Server], database mirroring
- transport security
- database mirroring [SQL Server], security
ms.assetid: 49239d02-964e-47c0-9b7f-2b539151ee1b
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: 8ff60136994d809b78cf09ebd98e4cc36ceb1a02
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/17/2020
ms.locfileid: "97641633"
---
# <a name="transport-security---database-mirroring---always-on-availability"></a>Sécurité du transport - Mise en miroir de bases de données - Groupes de disponibilité Always On
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  La sécurité du transport implique l'authentification et, éventuellement, le chiffrement des messages échangés entre les bases de données. Pour la mise en miroir de bases de données et [!INCLUDE[ssHADR](../../includes/sshadr-md.md)], l'authentification et le chiffrement sont configurés sur le point de terminaison de la mise en miroir. Pour une présentation des points de terminaison de mise en miroir de bases de données, consultez [Point de terminaison de mise en miroir de bases de données &#40;SQL Server&#41;](../../database-engine/database-mirroring/the-database-mirroring-endpoint-sql-server.md).  
  
 **Dans cette rubrique :**  
  
-   [Authentification](#Authentication)  
  
-   [Chiffrement des données](#DataEncryption)  
  
-   [Tâches associées](#RelatedTasks)  
  
##  <a name="authentication"></a><a name="Authentication"></a> Authentification  
 L'authentification est le processus qui permet de vérifier qu'un utilisateur est la personne qu'il prétend être. Les connexions entre les points de terminaison de mise en miroir de bases de données requièrent une authentification. Les demandes de connexion éventuellement formulées par un partenaire ou un témoin doivent être authentifiées.  
  
 Le type d'authentification utilisé pour la mise en miroir de bases de données ou [!INCLUDE[ssHADR](../../includes/sshadr-md.md)] par une instance de serveur est une propriété du point de terminaison de la mise en miroir. Deux types de sécurité de transport sont disponibles pour les points de terminaison de mise en miroir de bases de données : l’authentification Windows (SSPI, Security Support Provider Interface) et l’authentification basée sur les certificats.  
  
### <a name="windows-authentication"></a>Authentification Windows  
 Avec l'authentification Windows, chaque instance de serveur se connecte à l'autre partie à l'aide des informations d'identification Windows du compte d'utilisateur Windows sous lequel le processus est en cours d'exécution. L'authentification Windows peut nécessiter une configuration manuelle des comptes de connexion, comme suit :  
  
-   Si les instances de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] fonctionnent comme des services sous le même compte de domaine, aucune configuration supplémentaire n'est nécessaire.  
  
-   Si les instances de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] fonctionnent comme des services sous des comptes de domaine différents (dans les mêmes domaines ou dans des domaines approuvés), la connexion de chaque compte doit être créée en **maître** sur chacune des autres instances de serveur, et cette connexion doit disposer d’autorisations CONNECT sur le point de terminaison.  
  
-   Si les instances de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] fonctionnent comme le compte de service réseau, la connexion de chaque compte d’ordinateur hôte (_DomainName_ **\\** _ComputerName$_ ) doit être créée en **maître** sur chacun des autres serveurs, et cette connexion doit disposer d’autorisations CONNECT sur le point de terminaison. En effet, une instance de serveur s'exécutant sous le compte de service réseau s'authentifie à l'aide du compte de domaine de l'ordinateur hôte.  
  
> [!NOTE]  
>  Pour obtenir un exemple de configuration d’une session de mise en miroir de bases de données utilisant l’authentification Windows, consultez [Exemple : Configuration de la mise en miroir de bases de données à l’aide de l’authentification Windows &#40;Transact-SQL&#41;](../../database-engine/database-mirroring/example-setting-up-database-mirroring-using-windows-authentication-transact-sql.md).  
  
### <a name="certificates"></a>Certificats  
 Dans certaines situations, par exemple lorsque les instances de serveur ne se trouvent pas dans des domaines approuvés ou que [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] est en cours d'exécution en tant que service local, l'authentification Windows n'est pas disponible. Dans ces cas, ce ne sont pas des informations d'identification utilisateur, mais des certificats, qui sont nécessaires pour authentifier les demandes de connexion. Le point de terminaison de mise en miroir de chaque instance de serveur doit être configuré avec son propre certificat créé localement.  
  
 La méthode de chiffrement est établie lorsque le certificat est créé. Pour plus d’informations, consultez [Autoriser un point de terminaison de mise en miroir de bases de données à utiliser des certificats pour les connexions sortantes &#40;Transact-SQL&#41;](../../database-engine/database-mirroring/database-mirroring-use-certificates-for-outbound-connections.md). Gérez avec précaution les certificats que vous utilisez.  
  
 Une instance de serveur utilise la clé privée de son propre certificat pour établir son identité lors de la configuration d'une connexion. L'instance de serveur qui reçoit la demande de connexion utilise la clé publique du certificat de l'émetteur pour authentifier l'identité de celui-ci. Par exemple, considérons deux instances de serveur, en l'occurrence Serveur_A et Serveur_B. Serveur_A utilise sa clé privée pour chiffrer l'en-tête de connexion avant d'envoyer une demande de connexion à Serveur_B. Serveur_B utilise la clé publique du certificat de Serveur_A pour déchiffrer l'en-tête de connexion. Si l'en-tête déchiffré est correct, Serveur_B sait que l'en-tête a été chiffré par Serveur_A et la connexion est authentifiée. Si l'en-tête déchiffré est incorrect, Serveur_B sait que la demande de connexion n'est pas authentique et refuse la connexion.  
  
##  <a name="data-encryption"></a><a name="DataEncryption"></a> Chiffrement des données  
 Par défaut, un point de terminaison de mise en miroir de bases de données requiert le chiffrement des données envoyées via les connexions de mise en miroir. Dans ce cas, le point de terminaison ne peut se connecter qu'aux points de terminaison utilisant également le chiffrement. Sauf si vous pouvez garantir que votre réseau est sécurisé, il est recommandé d'imposer le chiffrement des connexions de mise en miroir de bases de données. Toutefois, vous pouvez désactiver le chiffrement ou le rendre disponible sans qu'il soit nécessaire. Si le chiffrement est désactivé, les données ne sont jamais chiffrées et le point de terminaison ne peut pas se connecter à un point de terminaison qui requiert le chiffrement. Si le chiffrement est pris en charge, les données ne sont chiffrées que si le point de terminaison opposé prend en charge ou requiert le chiffrement.  
  
> [!NOTE]  
>  Sur les points de terminaison de mise en miroir créés par [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] , le chiffrement est requis ou désactivé. Pour attribuer au paramètre de chiffrement la valeur SUPPORTED, utilisez l'instruction [!INCLUDE[tsql](../../includes/tsql-md.md)] ALTER ENDPOINT. Pour plus d’informations, consultez [ALTER ENDPOINT &#40;Transact-SQL&#41;](../../t-sql/statements/alter-endpoint-transact-sql.md).  
  
 Vous pouvez également contrôler les algorithmes de chiffrement qui peuvent être utilisés par un point de terminaison, en spécifiant une des valeurs ci-dessous pour l'option ALGORITHM dans une instruction CREATE ENDPOINT ou ALTER ENDPOINT :  
  
|valeur ALGORITHM|Description|  
|---------------------|-----------------|  
|RC4|Indique que le point de terminaison doit utiliser l'algorithme RC4. Il s’agit de la valeur par défaut.<br /><br /> <strong>\*\* Avertissement \*\*</strong> L’algorithme RC4 est déconseillé. [!INCLUDE[ssNoteDepFutureDontUse](../../includes/ssnotedepfuturedontuse-md.md)] Nous vous recommandons d'utiliser AES.|  
|AES|Indique que le point de terminaison doit utiliser l'algorithme AES.|  
|AES RC4|Indique que les deux points de terminaison négocieront un algorithme de chiffrement avec ce point de terminaison, en donnant la préférence à l'algorithme AES.|  
|RC4 AES|Indique que les deux points de terminaison négocieront un algorithme de chiffrement avec ce point de terminaison, en donnant la préférence à l'algorithme RC4.|  
  
 Si les points de terminaison se connectant spécifient les deux algorithmes mais dans des ordres différents, le point de terminaison acceptant la connexion a le dernier mot.  
  
> [!NOTE]  
>  L'algorithme RC4 est uniquement pris en charge pour des raisons de compatibilité descendante. Le nouveau matériel ne peut être chiffré à l'aide de RC4 ou de RC4_128 que lorsque la base de données se trouve dans le niveau de compatibilité 90 ou 100. (Non recommandé.) Utilisez à la place un algorithme plus récent, tel qu'un des algorithmes AES. Dans [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] et versions ultérieures, le matériel chiffré à l’aide de RC4 ou de RC4_128 peut être déchiffré dans n’importe quel niveau de compatibilité.  
>   
>  Bien que sensiblement plus rapide que AES, RC4 est un algorithme relativement faible, tandis que AES est relativement fort. Par conséquent, nous vous recommandons d'utiliser l'algorithme AES.  
  
 Pour plus d’informations sur la syntaxe [!INCLUDE[tsql](../../includes/tsql-md.md)] permettant de définir le chiffrement, consultez [CREATE ENDPOINT &#40;Transact-SQL&#41;](../../t-sql/statements/create-endpoint-transact-sql.md).  
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> Tâches associées  
 **Pour configurer la sécurité du transport pour un point de terminaison de mise en miroir de bases de données**  
  
-   [Créer un point de terminaison de mise en miroir de bases de données pour l’authentification Windows &#40;Transact-SQL&#41;](../../database-engine/database-mirroring/create-a-database-mirroring-endpoint-for-windows-authentication-transact-sql.md)  
  
-   [Établir une session de mise en miroir de bases de données au moyen de l’authentification Windows &#40;SQL Server Management Studio&#41;](../../database-engine/database-mirroring/establish-database-mirroring-session-windows-authentication.md)  
  
-   [Créer un point de terminaison de mise en miroir de bases de données pour l’authentification Windows &#40;Transact-SQL&#41;](../../database-engine/database-mirroring/create-a-database-mirroring-endpoint-for-windows-authentication-transact-sql.md)  
  
-   [Autoriser un point de terminaison de mise en miroir de bases de données à utiliser des certificats pour les connexions sortantes &#40;Transact-SQL&#41;](../../database-engine/database-mirroring/database-mirroring-use-certificates-for-outbound-connections.md)  
  
## <a name="see-also"></a>Voir aussi  
 [Choisir un algorithme de chiffrement](../../relational-databases/security/encryption/choose-an-encryption-algorithm.md)   
 [ALTER ENDPOINT &#40;Transact-SQL&#41;](../../t-sql/statements/alter-endpoint-transact-sql.md)   
 [DROP ENDPOINT &#40;Transact-SQL&#41;](../../t-sql/statements/drop-endpoint-transact-sql.md)   
 [Centre de sécurité pour le moteur de base de données SQL Server et Azure SQL Database](../../relational-databases/security/security-center-for-sql-server-database-engine-and-azure-sql-database.md)   
 [Gérer les métadonnées durant la mise à disposition d’une base de données sur une autre instance de serveur &#40;SQL Server&#41;](../../relational-databases/databases/manage-metadata-when-making-a-database-available-on-another-server.md)   
 [Point de terminaison de mise en miroir de bases de données &#40;SQL Server&#41;](../../database-engine/database-mirroring/the-database-mirroring-endpoint-sql-server.md)   
 [sys.database_mirroring_endpoints &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-database-mirroring-endpoints-transact-sql.md)   
 [sys.dm_db_mirroring_connections &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/database-mirroring-sys-dm-db-mirroring-connections.md)   
 [Résolution des problèmes de configuration de mise en miroir de bases de données &#40;SQL Server&#41;](../../database-engine/database-mirroring/troubleshoot-database-mirroring-configuration-sql-server.md)   
 [Résoudre des problèmes de configuration des groupes de disponibilité Always On &#40;SQL Server&#41;](../../database-engine/availability-groups/windows/troubleshoot-always-on-availability-groups-configuration-sql-server.md)  
  
  
