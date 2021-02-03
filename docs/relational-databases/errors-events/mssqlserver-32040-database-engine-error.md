---
description: MSSQLSERVER_32040
title: MSSQLSERVER_32040 | Microsoft Docs
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 32040 (Database Engine error)
ms.assetid: 709219b1-f8b2-4696-8923-dd2e91492eb8
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: a77169d0599f90819f6e737b876004d03e0f2f43
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99194148"
---
# <a name="mssqlserver_32040"></a>MSSQLSERVER_32040
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>Détails  
  
| Attribut | Valeur |  
| :-------- | :---- |  
|Nom du produit|SQL Server|  
|ID de l’événement|32040|  
|Source de l’événement|MSSQLSERVER|  
|Composant|SQLEngine|  
|Nom symbolique|SQLErrorNum32040|  
|Texte du message|L'alerte relative à la transaction non envoyée la plus ancienne s'est déclenchée. La valeur actuelle de '%d' dépasse le seuil '%d'.|  
  
## <a name="explanation"></a>Explication  
Cet événement de mise en miroir de bases de données est émis sur l'instance du serveur principal pour indiquer que la durée de vie de la plus ancienne transaction non envoyée a atteint la valeur de seuil définie par l'utilisateur. En règle générale, cet événement se produit car les performances du système ont changé. Soit la bande passante entre les deux systèmes a diminué, soit la charge a augmenté.  
  
La durée de vie de la plus ancienne transaction non envoyée est une mesure de performance qui peut vous aider à évaluer le risque de perte de données exprimé en minutes de transactions non envoyées. Cette mesure s'avère particulièrement appropriée pour les sessions en mode hautes performances. Toutefois, elle est également adaptée aux sessions en mode haute sécurité lorsque la mise en miroir est interrompue ou suspendue suite à une déconnexion des serveurs partenaires.  
  
## <a name="user-action"></a>Action de l'utilisateur  
Vérifiez les charges incombant aux instances du serveur principal et du serveur miroir et à leur connexion réseau pour déterminer la cause du problème.  
  
## <a name="see-also"></a>Voir aussi  
[Mise en miroir de bases de données &#40;SQL Server&#41;](~/database-engine/database-mirroring/database-mirroring-sql-server.md)  
[Utiliser des seuils d’avertissement et d’alertes sur des métriques de performances de mise en miroir &#40;SQL Server&#41;](~/database-engine/database-mirroring/use-warning-thresholds-and-alerts-on-mirroring-performance-metrics-sql-server.md)  
  
