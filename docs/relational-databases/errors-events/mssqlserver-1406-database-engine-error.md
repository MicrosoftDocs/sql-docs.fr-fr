---
description: MSSQLSERVER_1406
title: MSSQLSERVER_1406 | Microsoft Docs
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 1406 (Database Engine error)
ms.assetid: 915f97de-9b74-41f8-8bd5-b2d061416718
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: c5d7fe3f66ea581305f985e25cc1a2c30c1809e4
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99197014"
---
# <a name="mssqlserver_1406"></a>MSSQLSERVER_1406
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>Détails  
  
| Attribut | Valeur |  
| :-------- | :---- |  
|Nom du produit|SQL Server|  
|ID de l’événement|1406|  
|Source de l’événement|MSSQLSERVER|  
|Composant|SQLEngine|  
|Nom symbolique|DBM_BADSTATEFORFORCESERVICE|  
|Texte du message|Impossible de forcer le service en toute sécurité. Supprimez la mise en miroir de bases de données et récupérez la base de données "%.*ls" pour y accéder.|  
  
## <a name="explanation"></a>Explication  
Le [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] ne peut pas forcer le service car l'état de la mise en miroir ne peut pas garantir le bon fonctionnement du processus impliqué pour forcer le service.  
  
## <a name="user-action"></a>Action de l'utilisateur  
Supprimez la mise en miroir de bases de données. Vous pouvez ensuite récupérer la base de données miroir pour y accéder.  
  
## <a name="see-also"></a>Voir aussi  
[Forcer le service dans une session de mise en miroir de bases de données &#40;Transact-SQL&#41;](~/database-engine/database-mirroring/force-service-in-a-database-mirroring-session-transact-sql.md)  
[Suppression d’une mise en miroir des bases de données &#40;SQL Server&#41;](~/database-engine/database-mirroring/removing-database-mirroring-sql-server.md)  
  
