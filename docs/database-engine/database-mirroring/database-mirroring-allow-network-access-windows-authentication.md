---
title: Mise en miroir de bases de données - Autoriser l’accès réseau - Authentification Windows | Microsoft Docs
description: Découvrez comment utiliser l’authentification Windows pour connecter les points de terminaison de mise en miroir de bases de données de deux instances de SQL Server, ce qui peut nécessiter une configuration manuelle.
ms.custom: ''
ms.date: 05/17/2016
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: database-mirroring
ms.topic: conceptual
helpviewer_keywords:
- Windows authentication [SQL Server]
- database mirroring [SQL Server], security
ms.assetid: 28c8fec5-5feb-4c84-8d72-f2bd1ae3b40d
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: dfe10e2d812d133571d4df64d05ea7705d076464
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100353196"
---
# <a name="database-mirroring---allow-network-access---windows-authentication"></a>Mise en miroir de bases de données - Autoriser l’accès réseau - Authentification Windows
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  L'utilisation de l'authentification Windows pour connecter les points de terminaison de mise en miroir de bases de données de deux instances de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] requiert une configuration manuelle des comptes de connexion dans les conditions suivantes :  
  
-   Si les instances de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] fonctionnent comme des services sous des comptes de domaine différents (dans les mêmes domaines ou dans des domaines approuvés), la connexion de chaque compte doit être créée en **maître** sur chacune des instances de serveur distant, et cette connexion doit disposer d’autorisations CONNECT sur le point de terminaison.  
  
-   Si les instances de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] fonctionnent comme un compte de service réseau, la connexion de chaque compte d’ordinateur hôte (*DomainName* **\\** _ComputerName$_ ) doit être créée en **maître** sur chacune des instances de serveur distantes, et cette connexion doit avoir des autorisations CONNECT sur le point de terminaison. En effet, une instance de serveur s'exécutant sous le compte de service réseau s'authentifie à l'aide du compte de domaine de l'ordinateur hôte.  
  
> [!NOTE]  
>  Vérifiez qu'il existe un point de terminaison pour chaque instance du serveur. Pour plus d’informations, consultez [Créer un point de terminaison de mise en miroir de bases de données pour l’authentification Windows &#40;Transact-SQL&#41;](../../database-engine/database-mirroring/create-a-database-mirroring-endpoint-for-windows-authentication-transact-sql.md).  
  
### <a name="to-configure-logins-for-windows-authentication"></a>Pour configurer des connexions pour l'authentification Windows  
  
1.  Pour le compte d'utilisateur de chaque instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], créez une connexion sur les autres instances de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Utilisez une instruction [CREATE LOGIN](../../t-sql/statements/create-login-transact-sql.md) avec la clause FROM WINDOWS.  
  
     Pour plus d’informations, consultez [Créer un compte de connexion](../../relational-databases/security/authentication-access/create-a-login.md).  
  
2.  En outre, pour vérifier que l'utilisateur de connexion a accès au point de terminaison, utilisez l'instruction [GRANT](../../t-sql/statements/grant-transact-sql.md) pour accorder des autorisations de connexion sur le point de terminaison à la connexion. Notez que l'attribution d'autorisations de connexion n'est pas nécessaire si l'utilisateur est un administrateur.  
  
     Pour en savoir plus, voir [Grant a Permission to a Principal](../../relational-databases/security/authentication-access/grant-a-permission-to-a-principal.md).  
  
## <a name="example"></a>Exemple  
 L'exemple [!INCLUDE[tsql](../../includes/tsql-md.md)] suivant crée une connexion [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pour un compte d'utilisateur nommé `Otheruser` qui appartient à un domaine du nom de `Adomain`. Des autorisations de connexion sont ensuite octroyées à cet utilisateur pour le point de terminaison de la mise en miroir de bases de données préexistantes appelé `Mirroring_Endpoint`.  
  
```  
USE master;  
GO  
CREATE LOGIN [Adomain\Otheruser] FROM WINDOWS;  
GO  
GRANT CONNECT on ENDPOINT::Mirroring_Endpoint TO [Adomain\Otheruser];  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Vue d’ensemble des groupes de disponibilité Always On &#40;SQL Server&#41;](../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)   
 [Mise en miroir de bases de données &#40;SQL Server&#41;](../../database-engine/database-mirroring/database-mirroring-sql-server.md)   
 [Sécurité du transport de la mise en miroir de bases de données et des groupes de disponibilité Always On &#40;SQL Server&#41;](../../database-engine/database-mirroring/transport-security-database-mirroring-always-on-availability.md)   
 [Point de terminaison de mise en miroir de bases de données &#40;SQL Server&#41;](../../database-engine/database-mirroring/the-database-mirroring-endpoint-sql-server.md)  
  
  
