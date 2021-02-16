---
title: Activer la propriété Trustworthy pour une base de données miroir
description: Découvrez comment activer la propriété de base de données TRUSTWORTHY sur une base de données récemment mise en miroir à l’aide de Transact-SQL dans SQL Server.
ms.custom: seo-lt-2019
ms.date: 03/09/2017
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: database-mirroring
ms.topic: conceptual
helpviewer_keywords:
- TRUSTWORTHY database option
- mirror database [SQL Server]
- database mirroring [SQL Server], security
ms.assetid: 6993b076-78ef-453e-b0ea-e341b8e5ee3e
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: e8431286a4b19d8c2c3390bfd604cd6556d984d0
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100344478"
---
# <a name="set-up-a-mirror-database-to-use-the-trustworthy-property-transact-sql"></a>Configurer une base de données miroir pour utiliser la propriété Trustworthy (Transact-SQL)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  Lorsqu'une base de données est sauvegardée, la valeur OFF est attribuée à la propriété TRUSTWORTHY de la base de données. Par conséquent, la propriété TRUSTWORTHY d'une nouvelle base de données miroir a toujours la valeur OFF. Si la base de données doit être fiable après un basculement, des opérations de configuration supplémentaires sont requises après le début de la mise en miroir.  
  
> [!NOTE]  
>  Pour plus d’informations sur cette propriété de base de données, consultez [Propriété de base de données TRUSTWORTHY](../../relational-databases/security/trustworthy-database-property.md).  
  
## <a name="procedure"></a>Procédure  
  
#### <a name="to-setup-a-mirror-database-to-use-the-trustworthy-property"></a>Pour configurer une base de données miroir pour qu'elle utilise la propriété Trustworthy  
  
1.  Sur l'instance du serveur principal, vérifiez que la propriété Trustworthy de la base de données principale est activée.  
  
    ```  
    SELECT name, database_id, is_trustworthy_on FROM sys.databases   
    ```  
  
     Pour plus d’informations, consultez [sys.databases &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md).  
  
2.  Après avoir démarré la mise en miroir, vérifiez que la base de données est actuellement la base de données principale, que la session utilise un mode de fonctionnement synchrone et que la session est toujours synchronisée.  
  
    ```  
    SELECT database_id, mirroring_role, mirroring_safety_level_desc, mirroring_state_desc FROM sys.database_mirroring  
    ```  
  
     Pour plus d’informations, consultez [sys.database_mirroring &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-database-mirroring-transact-sql.md).  
  
3.  Une fois la session de mise en miroir synchronisée, basculez manuellement vers la base de données miroir.  
  
     Cette opération est possible dans SQL Server Management Studio ou en utilisant Transact-SQL :  
  
    -   [Basculer manuellement une session de mise en miroir de bases de données &#40;SQL Server Management Studio&#41;](../../database-engine/database-mirroring/manually-fail-over-a-database-mirroring-session-sql-server-management-studio.md)  
  
    -   [Basculer manuellement une session de mise en miroir de bases de données &#40;Transact-SQL&#41;](../../database-engine/database-mirroring/manually-fail-over-a-database-mirroring-session-transact-sql.md)  
  
4.  Activez la propriété Trustworthy de la base de données en utilisant la commande ALTER DATABASE suivante :  
  
    ```  
    ALTER DATABASE <database_name> SET TRUSTWORTHY ON  
    ```  
  
     Pour plus d’informations, consultez [ALTER DATABASE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql.md).  
  
5.  Vous pouvez également basculer manuellement de nouveau vers la base de données principale d'origine.  
  
6.  Vous pouvez également passer en mode hautes performances asynchrone en attribuant la valeur OFF à SAFETY et en garantissant que WITNESS a aussi la valeur OFF.  
  
     Dans Transact-SQL :  
  
    -   [Modifier la sécurité des transactions dans une session de mise en miroir de bases de données &#40;Transact-SQL&#41;](../../database-engine/database-mirroring/change-transaction-safety-in-a-database-mirroring-session-transact-sql.md)  
  
    -   [Supprimer le témoin d’une session de mise en miroir de bases de données &#40;SQL Server&#41;](../../database-engine/database-mirroring/remove-the-witness-from-a-database-mirroring-session-sql-server.md)  
  
     Dans SQL Server Management Studio :  
  
    -   [Établir une session de mise en miroir de bases de données au moyen de l’authentification Windows &#40;SQL Server Management Studio&#41;](../../database-engine/database-mirroring/establish-database-mirroring-session-windows-authentication.md)  
  
## <a name="see-also"></a>Voir aussi  
 [Propriété de base de données TRUSTWORTHY](../../relational-databases/security/trustworthy-database-property.md)   
 [Configurer une base de données miroir chiffrée](../../database-engine/database-mirroring/set-up-an-encrypted-mirror-database.md)  
  
  
