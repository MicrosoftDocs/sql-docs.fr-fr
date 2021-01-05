---
title: Regroupement de connexions
description: En savoir plus sur la mise en pool des connexions, une technique d’optimisation utilisée par ADO.NET pour réduire le coût de l’ouverture des connexions aux sources de données.
ms.date: 11/13/2020
ms.assetid: 955c057f-aea8-4ba8-aa6d-e3dfa18ba8d5
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
ms.reviewer: v-chmalh
ms.openlocfilehash: 783fad79522c52685349defca93360c4ea8c80c9
ms.sourcegitcommit: c938c12cf157962a5541347fcfae57588b90d929
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/25/2020
ms.locfileid: "97771641"
---
# <a name="connection-pooling"></a>Regroupement de connexions

[!INCLUDE[appliesto-netfx-netcore-netst-md](../../includes/appliesto-netfx-netcore-netst-md.md)]

[!INCLUDE[Driver_ADONET_Download](../../includes/driver_adonet_download.md)]

Se connecter à une source de données peut prendre beaucoup de temps. Pour minimiser le coût de l'ouverture de connexions, ADO.NET utilise une technique d'optimisation nommée *mise en pool de connexions*, qui limite le coût des ouvertures et fermetures des connexions à répétition.

## <a name="in-this-section"></a>Contenu de cette section  

[Regroupement de connexions SQL Server (ADO.NET)](sql-server-connection-pooling.md)  
Fournit une vue d’ensemble du regroupement de connexions et décrit son fonctionnement dans SQL Server.

## <a name="see-also"></a>Voir aussi

- [Récupération et modification de données dans ADO.NET](retrieving-modifying-data.md)
- [Microsoft ADO.NET pour SQL Server](microsoft-ado-net-sql-server.md)
