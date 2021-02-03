---
description: MSSQLSERVER_3181
title: MSSQLSERVER_3181 | Microsoft Docs
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 3181 (Database Engine error)
ms.assetid: e6d2e716-5263-477c-ad0e-7bcbfef4c1f3
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 9d64c35a396e6a011ec96463d14cff1eb74e8184
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99194155"
---
# <a name="mssqlserver_3181"></a>MSSQLSERVER_3181
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>Détails  
  
| Attribut | Valeur |  
| :-------- | :---- |  
|Nom du produit|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]|  
|ID de l’événement|3181|  
|Source de l’événement|MSSQLSERVER|  
|Composant|SQLEngine|  
|Nom symbolique|LDDB_STORAGE_VERIFY|  
|Texte du message|La restauration de cette sauvegarde risque de rencontrer des problèmes d'espace de stockage. D'autres messages fourniront des détails.|  
  
## <a name="explanation"></a>Explication  
L'instruction RESTORE VERIFYONLY vérifie l'espace de stockage disponible sur le disque sur lequel la base de données doit être restaurée.  
  
### <a name="possible-causes"></a>Causes possibles  
L'espace disque disponible est peut-être insuffisant pour restaurer la sauvegarde en cours de vérification.  
  
## <a name="user-action"></a>Action de l'utilisateur  
Restaurez la sauvegarde à un emplacement doté de suffisamment d'espace disque ou prévoyez plus d'espace sur le disque.  
  
## <a name="see-also"></a>Voir aussi  
[Restaurer une base de données à un nouvel emplacement &#40;SQL Server&#41;](~/relational-databases/backup-restore/restore-a-database-to-a-new-location-sql-server.md)  
[Restaurer des fichiers à un nouvel emplacement &#40;SQL Server&#41;](~/relational-databases/backup-restore/restore-files-to-a-new-location-sql-server.md)  
[RESTORE &#40;Transact-SQL&#41;](~/t-sql/statements/restore-statements-transact-sql.md)  
[RESTORE VERIFYONLY &#40;Transact-SQL&#41;](~/t-sql/statements/restore-statements-verifyonly-transact-sql.md)  
  
