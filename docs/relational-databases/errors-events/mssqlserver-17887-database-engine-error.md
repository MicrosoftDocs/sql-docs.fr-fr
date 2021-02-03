---
description: MSSQLSERVER_17887
title: MSSQLSERVER_17887 | Microsoft Docs
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 17887 (Database Engine error)
ms.assetid: ad0806e6-3296-4c32-b103-fccf0f8a8d3d
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 32ce0464266726de06784ae078c7321339208f01
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99196583"
---
# <a name="mssqlserver_17887"></a>MSSQLSERVER_17887
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>Détails  
  
| Attribut | Valeur |  
| :-------- | :---- |  
|Nom du produit|SQL Server|  
|ID de l’événement|17887|  
|Source de l’événement|MSSQLSERVER|  
|Composant|SQLEngine|  
|Nom symbolique|SRV_IO_COMP_LISTENER_NONYIELDING|  
|Texte du message|L’écoute d’achèvement d’E/S (0x%lx) travail 0x%p semble être improductive sur le nœud %ld. Utilisation approximative de l'UC : noyau %I64d ms, utilisateur %I64d ms, intervalle : %I64d.|  
  
## <a name="explanation"></a>Explication  
Indique qu'il existe un éventuel problème avec l'écouteur du port d'exécution d'E/S sur le nœud spécifié lors de l'exécution de la routine d'exécution d'E/S pour un événement de lecture/écriture sur le réseau. Cette erreur disparaîtra lorsque l'écouteur du port d'exécution d'E/S sortira de la routine d'exécution d'E/S.  
  
## <a name="user-action"></a>Action de l'utilisateur  
Contactez les services d'assistance Microsoft.  
  
