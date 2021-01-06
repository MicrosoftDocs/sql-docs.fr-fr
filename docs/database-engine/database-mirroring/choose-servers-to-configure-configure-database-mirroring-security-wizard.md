---
title: 'Assistant Configurer la sécurité : Choisir les serveurs'
description: Décrit les propriétés trouvées à la page « Choisir les serveurs » de l’« Assistant Configurer la sécurité de la mise en miroir de bases de données ».
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: database-mirroring
ms.topic: conceptual
f1_keywords:
- sql13.swb.configdbmsecurwiz.choosesrvrs.f1
ms.assetid: 59e23ff3-d7ee-4e32-9629-0b54d3a258f7
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: 52df789b46ac1caf5609a3342cc2f9e7131b239c
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/17/2020
ms.locfileid: "97643655"
---
# <a name="configure-database-mirroring-wizard-choose-servers-to-configure"></a>Assistant Configurer la mise en miroir de bases de données : Choisir les serveurs à configurer 
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  Cette page vous permet de spécifier les instances de serveurs que vous voulez configurer. Vous devez sélectionner au moins une instance de serveur avant de poursuivre l'exécution de l'Assistant.  
  
 Si vous désactivez la case à cocher pour une instance de serveur, l'Assistant ne lui apportera aucune modification. Toutefois, l'Assistant vous demandera d'entrer des informations sur cette instance et d'enregistrer ces informations au sein de la configuration des autres instances de serveurs. Par exemple, si vous désactivez la case à cocher pour l'instance de serveur témoin, l'Assistant vous demandera d'entrer le compte de service [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] du témoin car une connexion doit être créée pour ce compte dans le cadre de la configuration de sécurité enregistrée au niveau des instances du principal et du serveur miroir.  
  
 **Pour configurer la mise en miroir de bases de données à l'aide de SQL Server Management Studio**  
  
-   [Établir une session de mise en miroir de bases de données au moyen de l’authentification Windows &#40;SQL Server Management Studio&#41;](../../database-engine/database-mirroring/establish-database-mirroring-session-windows-authentication.md)  
  
-   [Démarrer l’Assistant Configuration de la sécurité de mise en miroir de bases de données &#40;SQL Server Management Studio&#41;](../../database-engine/database-mirroring/start-the-configuring-database-mirroring-security-wizard.md)  
  
## <a name="options"></a>Options  
 **Instance de serveur principal**  
 Sélectionnez cette option pour configurer la sécurité pour le serveur principal.  
  
 **Instance de serveur miroir**  
 Sélectionnez cette option pour configurer la sécurité pour le serveur miroir.  
  
 **Instance de serveur témoin**  
 Sélectionnez cette option pour configurer la sécurité pour le serveur témoin (s'il est présent).  
  
## <a name="see-also"></a>Voir aussi  
 [Propriétés de la base de données &#40;page Mise en miroir&#41;](../../relational-databases/databases/database-properties-mirroring-page.md)   
 [Mise en miroir de bases de données &#40;SQL Server&#41;](../../database-engine/database-mirroring/database-mirroring-sql-server.md)  
  
  
