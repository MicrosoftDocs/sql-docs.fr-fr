---
title: Régler la compression pour un groupe de disponibilité | Microsoft Docs
description: Découvrez comment SQL Server compresse les flux de données pour les groupes de disponibilité, ce qui réduit le trafic réseau, augmente la charge du processeur et peut induire une latence.
ms.custom: ''
ms.date: 06/22/2016
ms.prod: sql
ms.technology: availability-groups
ms.topic: conceptual
ms.assetid: 7632769c-b246-4766-886f-7c60ec540be8
author: cawrites
ms.author: chadam
ms.openlocfilehash: 1066b1e519f757207b06f1deb94031f2f054bc5f
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/17/2020
ms.locfileid: "97641849"
---
# <a name="tune-compression-for-availability-group"></a>Régler la compression pour un groupe de disponibilité
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
Par défaut, SQL Server compresse les flux de données pour les groupes de disponibilité, si besoin. La compression réduit le trafic réseau, augmente la charge processeur et peut entraîner une latence. Vous devez être membre du rôle de serveur fixe pour activer la compression. Le tableau suivant indique à quel moment SQL Server utilise la compression pour les flux de journaux des groupes de disponibilité :

| Scénario | Paramètre de compression
| ---- | ----
| Réplica à validation synchrone | Non compressé
| Réplicas à validation asynchrone | Compressé
| Pendant l’amorçage automatique | Non compressé
| TDE activée dans la base de données  | Non compressé

## <a name="trace-flags-for-availability-group-compression"></a>Indicateurs de trace pour la compression du groupe de disponibilité 

Dans la majorité des cas, Microsoft recommande de ne pas modifier ces paramètres. Vous pouvez utiliser des indicateurs de trace globaux pour tester la modification de ces paramètres. SQL Server applique des indicateurs de trace globaux à l’intégralité de l’instance. Tous les groupes de disponibilité inclus dans l’instance sont affectés par ces paramètres.  

Le tableau suivant présente les indicateurs de trace qui modifient le comportement de compression par défaut pour SQL Server. 

Indicateur de trace | Description
------------- | -------------
1462          | Désactive la compression du flux de journal pour les groupes de disponibilité avec des réplicas asynchrones. Cette fonctionnalité est activée par défaut sur les réplicas asynchrones pour optimiser la bande passante réseau.
9567          | Active la compression du flux de données pour les groupes de disponibilité au cours de l’amorçage automatique. Pendant l’amorçage automatique, la compression peut réduire considérablement le temps de transfert et augmente la charge sur le processeur.
9592          | Active la compression du flux de journal pour les groupes de disponibilité avec des réplicas synchrones. Cette fonctionnalité est désactivée par défaut sur les réplicas synchrones car la compression ajoute une latence. La compression du flux de journal est activée par défaut pour les réplicas asynchrones.


## <a name="resources"></a>Ressources


[Options de démarrage du moteur de base de données](../../../database-engine/configure-windows/database-engine-service-startup-options.md)

[Amorçage automatique](./automatically-initialize-always-on-availability-group.md)

[Conditions préalables relatives à Always On](prereqs-restrictions-recommendations-always-on-availability.md)