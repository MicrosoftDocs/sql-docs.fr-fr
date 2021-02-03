---
description: MSSQLSERVER_3619
title: MSSQLSERVER_3619 | Microsoft Docs
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 3619 (Database Engine error)
ms.assetid: 7d94f8d9-65ca-4fde-9c17-7b83e94a3779
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: a5f805abd590470884f86bed70358be73447563c
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99185217"
---
# <a name="mssqlserver_3619"></a>MSSQLSERVER_3619
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>Détails  
  
| Attribut | Valeur |  
| :-------- | :---- |  
|Nom du produit|SQL Server|  
|ID de l’événement|3619|  
|Source de l’événement|MSSQLSERVER|  
|Composant|SQLEngine|  
|Nom symbolique|SYS_NOLOG|  
|Texte du message|Impossible d'écrire un enregistrement de point de contrôle dans la base de données ID %d car le journal est saturé. Demandez à l'administrateur de la base de données de tronquer ce journal ou d'allouer davantage d'espace aux fichiers journaux de la base de données.|  
  
## <a name="explanation"></a>Explication  
Le journal des transactions est saturé.  
  
## <a name="user-action"></a>Action de l'utilisateur  
Tronquez le journal pour libérer de l'espace ou allouez plus d'espace au journal. Pour plus d'informations, consultez « Résolution des problèmes en cas de journal des transactions saturé (erreur 9002) » dans la documentation en ligne de SQL Server.  
  
