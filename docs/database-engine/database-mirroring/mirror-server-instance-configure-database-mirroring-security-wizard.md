---
title: 'Assistant Configurer la sécurité : Instance de serveur miroir'
description: Décrit la page « Instance de serveur miroir » de l’« Assistant Configurer la sécurité de la mise en miroir de bases de données » dans SQL Server Management Studio.
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: database-mirroring
ms.topic: conceptual
f1_keywords:
- sql13.swb.configdbmsecurwiz.mirrorsrvr.f1
ms.assetid: 53223432-615e-440f-904d-925d33ec2144
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: f2215721da618b3b6714b9c92d6d2da94f54e332
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/17/2020
ms.locfileid: "97641611"
---
# <a name="configure-database-mirrroing-security-wizard-mirror-server-instance"></a>Assistant Configurer la sécurité de la mise en miroir de bases de données : Instance de serveur miroir
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  Cette page vous permet de spécifier des informations sur l'instance de serveur avec la base de données miroir.  
  
> [!IMPORTANT]  
>  L'instance de serveur miroir doit exécuter la même édition de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)](Standard ou Entreprise) que l'instance de serveur principal. En outre, nous vous conseillons vivement d'exécuter les instances sur des systèmes comparables pouvant gérer des charges de travail identiques.  
  
 **Pour configurer la mise en miroir de bases de données à l'aide de SQL Server Management Studio**  
  
-   [Établir une session de mise en miroir de bases de données au moyen de l’authentification Windows &#40;SQL Server Management Studio&#41;](../../database-engine/database-mirroring/establish-database-mirroring-session-windows-authentication.md)  
  
-   [Démarrer l’Assistant Configuration de la sécurité de mise en miroir de bases de données &#40;SQL Server Management Studio&#41;](../../database-engine/database-mirroring/start-the-configuring-database-mirroring-security-wizard.md)  
  
## <a name="options"></a>Options  
 **Instance de serveur miroir**  
 Si une instance de serveur miroir est déjà spécifiée (dans la page **Mise en miroir** de la boîte de dialogue **Propriétés de la base de données** ), cette instance est affichée. Pour plus d’informations, consultez [Propriétés de la base de données &#40;page Mise en miroir&#41;](../../relational-databases/databases/database-properties-mirroring-page.md).  
  
 Dans le cas contraire, entrez le nom de l'instance de serveur miroir. Notez que l'instance de serveur miroir ne peut pas être identique à l'instance de serveur principal.  
  
 **Connexion**  
 Si aucune instance de serveur miroir n’a été spécifiée, cliquez sur **Se connecter** pour afficher la boîte de dialogue **Se connecter au serveur** , dans laquelle vous pouvez spécifier l’instance de serveur et établir une connexion.  
  
 Si l’instance a été spécifiée, mais que l’Assistant ne propose pas de connexion doté d’autorisations suffisantes pour vérifier l’existence du point de terminaison, cliquez sur **Se connecter**. Dans la boîte de dialogue **Se connecter au serveur** qui s’affiche, l’instance de serveur est présélectionnée et ne peut pas être modifiée. Spécifiez un compte de domaine qui possède une autorisation suffisante et connectez-vous à l'instance du serveur.  
  
> [!NOTE]  
>  Au moment de se connecter à l’instance du serveur, l’Assistant de configuration de la sécurité de mise en miroir de bases de données utilise les informations d’identification fournies dans la boîte de dialogue **Se connecter au serveur** . Celles-ci diffèrent des informations d'identification d'une session de mise en miroir, qui utilise les informations d'identification du compte de démarrage où l'instance de serveur est en cours d'exécution en tant que service.  
  
 **Port de l’écouteur**  
 Le comportement de cette option varie comme suit selon qu'il existe ou non un point de terminaison de mise en miroir pour cette instance du serveur :  
  
-   Si aucun port d’écoute n’existe pour l’instance de serveur, le numéro de port 5022 est affiché dans la zone de texte **Port** . Vous pouvez utiliser n'importe quel numéro de port disponible, tel que 7022.  
  
-   Lorsque le point de terminaison de mise en miroir existe déjà, le numéro de port issu de ce point de terminaison est affiché. Si vous devez modifier le port, utilisez une commande ALTER ENDPOINT. Pour plus d’informations, consultez [ALTER ENDPOINT &#40;Transact-SQL&#41;](../../t-sql/statements/alter-endpoint-transact-sql.md).  
  
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
  
  
