---
title: Instance de serveur principal (Configurer l'Assistant Sécurité de mise en miroir de bases de données)
description: Description de la page « Instance de serveur principal » de l’Assistant « Configurer la sécurité de la mise en miroir de bases de données » dans SQL Server Management Studio.
ms.custom: seo-lt-2019
ms.date: 03/07/2017
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: database-mirroring
ms.topic: conceptual
f1_keywords:
- sql13.swb.configdbmsecurwiz.principalsrvr.f1
ms.assetid: 58af27d7-c5dd-4669-be6b-b472bc2c8ef4
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: 258014c74dac92e85d3efa5d4078f5bc3b1bbe47
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100337878"
---
# <a name="principal-server-instance-configure-database-mirroring-security-wizard"></a>Instance de serveur principal (Configurer l'Assistant Sécurité de mise en miroir de bases de données)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  Cette page vous permet de spécifier des informations sur l'instance de serveur de la base de données principal. La base de données principale est la copie de la base de données qui commence la session de mise en miroir. Une fois que la session a commencé, la base de données principale est la copie de la base de données qui accepte les modifications d'utilisateur. (Lorsqu'un basculement a lieu, les rôles principal et de mise en miroir sont échangés ; par conséquent, la base de données principale initiale peut ne pas demeurer la base de données principale.)  
  
 **Pour configurer la mise en miroir de bases de données à l'aide de SQL Server Management Studio**  
  
-   [Établir une session de mise en miroir de bases de données au moyen de l’authentification Windows &#40;SQL Server Management Studio&#41;](../../database-engine/database-mirroring/establish-database-mirroring-session-windows-authentication.md)  
  
-   [Démarrer l’Assistant Configuration de la sécurité de mise en miroir de bases de données &#40;SQL Server Management Studio&#41;](../../database-engine/database-mirroring/start-the-configuring-database-mirroring-security-wizard.md)  
  
## <a name="options"></a>Options  
 **Instance de serveur principal**  
 Dans [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] , la mise en miroir de base de données est toujours configurée à partir du serveur principal ; c'est pourquoi l'instance de serveur active est toujours l'instance de serveur principal.  
  
 **Port de l’écouteur**  
 Le comportement de cette option varie comme suit selon qu'il existe ou non un point de terminaison de mise en miroir pour cette instance du serveur :  
  
-   Si le port d’écoute n’existe pas pour cette instance de serveur, le numéro de port 5022 est affiché dans la zone de texte **Port** . Vous pouvez utiliser n'importe quel numéro de port disponible, tel que 7022.  
  
-   Si le point de terminaison de mise en miroir existe déjà, son numéro de port est affiché. Si vous devez modifier le port, utilisez une commande ALTER ENDPOINT. Pour plus d’informations, consultez [ALTER ENDPOINT &#40;Transact-SQL&#41;](../../t-sql/statements/alter-endpoint-transact-sql.md).  
  
> [!NOTE]  
>  Un numéro de port est obligatoire.  
  
 **Nom du point de terminaison**  
 Si le point de terminaison de mise en miroir existe pour cette instance du serveur, le nom du point de terminaison est affiché ici. Si le point de terminaison n'existe pas, vous pouvez spécifier son nom.  
  
 **Chiffrer les données envoyées via ce point de terminaison**  
 Par défaut, le chiffrement est activé. Lorsqu'il est activé, il devient obligatoire (et non simplement pris en charge) et il utilise les valeurs par défaut pour toutes les options de chiffrement. Pour plus d’informations, consultez [CREATE ENDPOINT &#40;Transact-SQL&#41;](../../t-sql/statements/create-endpoint-transact-sql.md).  
  
 Pour désactiver le chiffrement, désactivez la case à cocher. Pour réactiver le chiffrement, activez la case à cocher.  
  
## <a name="see-also"></a>Voir aussi  
 [Point de terminaison de mise en miroir de bases de données &#40;SQL Server&#41;](../../database-engine/database-mirroring/the-database-mirroring-endpoint-sql-server.md)   
 [Propriétés de la base de données &#40;page Mise en miroir&#41;](../../relational-databases/databases/database-properties-mirroring-page.md)   
 [Créer un point de terminaison de mise en miroir de bases de données pour l’authentification Windows &#40;Transact-SQL&#41;](../../database-engine/database-mirroring/create-a-database-mirroring-endpoint-for-windows-authentication-transact-sql.md)   
 [Démarrer le moniteur de mise en miroir de bases de données &#40;SQL Server Management Studio&#41;](../../database-engine/database-mirroring/start-database-mirroring-monitor-sql-server-management-studio.md)   
 [Mise en miroir de bases de données &#40;SQL Server&#41;](../../database-engine/database-mirroring/database-mirroring-sql-server.md)  
  
  
