---
description: MSSQLSERVER_1406
title: MSSQLSERVER_1406 | Microsoft Docs
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: language-reference
helpviewer_keywords:
- 1406 (Database Engine error)
ms.assetid: 915f97de-9b74-41f8-8bd5-b2d061416718
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 45c5889a0f79757ccd2ae4de74cccc1eed8846f4
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/17/2020
ms.locfileid: "88334775"
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
  
