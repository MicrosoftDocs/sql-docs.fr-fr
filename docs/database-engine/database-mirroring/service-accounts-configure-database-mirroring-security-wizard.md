---
title: 'Assistant Configurer la sécurité : Comptes de service'
description: Décrit la page « Comptes de service » de l’« Assistant Configurer la sécurité de la mise en miroir de bases de données » dans SQL Server Management Studio.
ms.custom: seo-lt-2019
ms.date: 05/17/2016
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: database-mirroring
ms.topic: conceptual
f1_keywords:
- sql13.swb.configdbmsecurwiz.serviceaccounts.f1
ms.assetid: d58d8f93-7888-4d66-af4d-969ef6a2dbee
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: e3493525e67edcaf75ca6bbe7e3f3ef087ea71c0
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/17/2020
ms.locfileid: "97641285"
---
# <a name="configure-database-mirroring-security-wizard-service-accounts"></a>Assistant Configurer la sécurité de la mise en miroir de bases de données : Comptes de service
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  Lors de l'utilisation de l'authentification Windows, si les instances de serveur utilisent des comptes différents, spécifiez les comptes de service pour [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Ces comptes de service doivent tous être des comptes de domaine (dans les mêmes domaines ou des domaines approuvés).  
  
 Si toutes les instances de serveur utilisent le même compte de domaine ou une authentification basée sur les certificats, laissez les champs vides. Cliquez simplement sur **Terminer**; l'Assistant configure alors automatiquement les comptes en fonction du compte de l'Assistant actuel.  
  
> [!IMPORTANT]  
>  Si les points de terminaison de mise en miroir de base de données des instances de serveur sont configurés pour utiliser des certificats, vous devez laisser les champs de compte de service vides.  
  
 **Pour configurer la mise en miroir de bases de données à l'aide de SQL Server Management Studio**  
  
-   [Établir une session de mise en miroir de bases de données au moyen de l’authentification Windows &#40;SQL Server Management Studio&#41;](../../database-engine/database-mirroring/establish-database-mirroring-session-windows-authentication.md)  
  
-   [Démarrer l’Assistant Configuration de la sécurité de mise en miroir de bases de données &#40;SQL Server Management Studio&#41;](../../database-engine/database-mirroring/start-the-configuring-database-mirroring-security-wizard.md)  
  
## <a name="options"></a>Options  
 **Principal**  
 Permet de spécifier le compte de service de l'instance de serveur principal. Entrez le nom du domaine en majuscules :  
  
 *NOM_DOMAINE*\\*nom_utilisateur*  
  
 **Miroir**  
 Permet de spécifier le compte de service de l'instance de serveur miroir. Entrez le nom du domaine en majuscules :  
  
 *NOM_DOMAINE*\\*nom_utilisateur*  
  
 **Témoin**  
 Permet de spécifier le compte de service de l'instance de serveur témoin. Entrez le nom du domaine en majuscules :  
  
 *NOM_DOMAINE*\\*nom_utilisateur*  
  
## <a name="see-also"></a>Voir aussi  
 [Propriétés de la base de données &#40;page Mise en miroir&#41;](../../relational-databases/databases/database-properties-mirroring-page.md)   
 [Démarrer le moniteur de mise en miroir de bases de données &#40;SQL Server Management Studio&#41;](../../database-engine/database-mirroring/start-database-mirroring-monitor-sql-server-management-studio.md)   
 [Mise en miroir de bases de données &#40;SQL Server&#41;](../../database-engine/database-mirroring/database-mirroring-sql-server.md)   
 [Configurer des comptes de connexion pour la mise en miroir de bases de données ou les groupes de disponibilité Always On &#40;SQL Server&#41;](../../database-engine/database-mirroring/set-up-login-accounts-database-mirroring-always-on-availability.md)  
  
  
