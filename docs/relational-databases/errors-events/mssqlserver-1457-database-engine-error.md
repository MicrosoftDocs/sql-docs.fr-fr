---
description: MSSQLSERVER_1457
title: MSSQLSERVER_1457 | Microsoft Docs
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: language-reference
helpviewer_keywords:
- 1457 (Database Engine error)
ms.assetid: 28434ba1-b033-4866-ab41-111fccef45a2
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: b9d9c797f1c7071518b79a7f90d4a99d312eda55
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/17/2020
ms.locfileid: "88334085"
---
# <a name="mssqlserver_1457"></a>MSSQLSERVER_1457
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>Détails  
  
| Attribut | Valeur |  
| :-------- | :---- |  
|Nom du produit|SQL Server|  
|ID de l’événement|1457|  
|Source de l’événement|MSSQLSERVER|  
|Composant|SQLEngine|  
|Nom symbolique|DBM_PAGE_UNDO_PENDING|  
|Texte du message|La synchronisation de la base de données miroir, '%.*ls', a été interrompue : la base de données est donc dans un état incohérent. La commande ALTER DATABASE a échoué. Vérifiez que la base de données miroir est sauvegardée et en ligne, reconnectez l'instance de serveur miroir, puis autorisez la base de données miroir à terminer la synchronisation.|  
  
## <a name="explanation"></a>Explication  
Ce message indique que l’instruction ALTER DATABASE *nom_base_de_données* SET PARTNER OFF a échoué. L'échec de la tentative de suppression de la mise en miroir de bases de données a interrompu la synchronisation de la base de données miroir. La base de données est incohérente.  
  
## <a name="user-action"></a>Action de l'utilisateur  
Pour résoudre cette erreur, vous pouvez effectuer l'une des actions suivantes :  
  
-   Rétablissez le contact entre le serveur miroir et le serveur principal pour permettre la synchronisation de la base de données miroir.  
  
-   Supprimez la base de données miroir.  
  
    Après avoir supprimé la base de données miroir, vous pouvez en créer une nouvelle à partir de vos sauvegardes.  
  
    > [!NOTE]  
    > Vous pouvez supprimer la base de données miroir lorsque la mise en miroir de bases de données est toujours activée uniquement après l'échec d'une instruction SET PARTNER OFF.  
  
## <a name="see-also"></a>Voir aussi  
[Mise en miroir de bases de données &#40;SQL Server&#41;](~/database-engine/database-mirroring/database-mirroring-sql-server.md)  
[ALTER DATABASE &#40;Transact-SQL&#41;](~/t-sql/statements/alter-database-transact-sql-set-options.md)  
[Configuration de la mise en miroir d’une base de données &#40;SQL Server&#41;](~/database-engine/database-mirroring/setting-up-database-mirroring-sql-server.md)  
[Suppression d’une mise en miroir des bases de données &#40;SQL Server&#41;](~/database-engine/database-mirroring/removing-database-mirroring-sql-server.md)  
[Préparer une base de données miroir pour la mise en miroir &#40;SQL Server&#41;](~/database-engine/database-mirroring/prepare-a-mirror-database-for-mirroring-sql-server.md)  
  
