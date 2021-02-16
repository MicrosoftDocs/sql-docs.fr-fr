---
title: Inclure un serveur témoin (Configurer l'Assistant Sécurité de mise en miroir de bases de données)
description: Décrit la page « Inclure un serveur témoin » de l’Assistant « Configurer la sécurité de la mise en miroir de bases de données » dans l’interface graphique utilisateur de SSMS (SQL Server Management Studio).
ms.custom: seo-lt-2019
ms.date: 03/07/2017
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: database-mirroring
ms.topic: conceptual
f1_keywords:
- sql13.swb.configdbmsecurwiz.inclwitness.f1
ms.assetid: f04b38a4-f4e2-4d4c-bdac-7cc70e5a5684
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: f337f9a4be80ac3819ab0f123c020dec84ea6027
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100342289"
---
# <a name="include-witness-server-configure-database-mirroring-security-wizard"></a>Inclure un serveur témoin (Configurer l'Assistant Sécurité de mise en miroir de bases de données)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  Utilisez cette page pour indiquer si vous voulez inclure un serveur témoin dans cette configuration de sécurité pour la mise en miroir de bases de données.  
  
 **Pour configurer la mise en miroir de bases de données à l'aide de SQL Server Management Studio**  
  
-   [Établir une session de mise en miroir de bases de données au moyen de l’authentification Windows &#40;SQL Server Management Studio&#41;](../../database-engine/database-mirroring/establish-database-mirroring-session-windows-authentication.md)  
  
-   [Démarrer l’Assistant Configuration de la sécurité de mise en miroir de bases de données &#40;SQL Server Management Studio&#41;](../../database-engine/database-mirroring/start-the-configuring-database-mirroring-security-wizard.md)  
  
## <a name="options"></a>Options  
 **Oui**  
 Cliquez sur Oui pour inclure une instance de serveur témoin dans la configuration de sécurité. Ce serveur témoin est nécessaire en mode haute sécurité avec basculement automatique, ce qui permet la prise en charge du basculement automatique vers l'instance de serveur miroir en cas de défaillance de l'instance de serveur principal.  
  
 **Non**  
 Cliquez sur Non pour ne pas inclure de serveur témoin dans la configuration de sécurité.  
  
## <a name="see-also"></a>Voir aussi  
 [Propriétés de la base de données &#40;page Mise en miroir&#41;](../../relational-databases/databases/database-properties-mirroring-page.md)   
 [Mise en miroir de bases de données &#40;SQL Server&#41;](../../database-engine/database-mirroring/database-mirroring-sql-server.md)   
 [Témoin de mise en miroir de base de données](../../database-engine/database-mirroring/database-mirroring-witness.md)  
  
  
