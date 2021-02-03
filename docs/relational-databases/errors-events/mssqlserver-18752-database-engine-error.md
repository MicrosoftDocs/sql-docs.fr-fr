---
description: MSSQLSERVER_18752
title: MSSQLSERVER_18752 | Microsoft Docs
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 18752 (Database Engine error)
ms.assetid: 234c58d8-7a1e-4b07-a64b-32a311527980
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 51d1c0e130e0bada6601a9ef10875108da2f233a
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99196484"
---
# <a name="mssqlserver_18752"></a>MSSQLSERVER_18752
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>Détails  
  
| Attribut | Valeur |  
| :-------- | :---- |  
|Nom du produit|SQL Server|  
|ID de l’événement|18752|  
|Source de l’événement|MSSQLSERVER|  
|Composant|SQLEngine|  
|Nom symbolique|REPL_INUSE|  
|Texte du message|Un seul Agent de lecture du journal ou une seule procédure liée au journal (sp_repldone, sp_replcmds et sp_replshowcmds) peut se connecter à une base de données à la fois. Si vous avez exécuté une procédure liée au journal, supprimez la connexion à travers laquelle fut exécutée la procédure ou exécutez sp_replflush sur cette connexion avant de démarrer l'Agent de lecture du journal ou d'exécuter toute autre procédure liée au journal.|  
  
## <a name="explanation"></a>Explication  
Un seul Agent de lecture du journal ou une seule procédure liée au journal peut se connecter à une base de données à la fois.  
  
## <a name="user-action"></a>Action de l'utilisateur  
Assurez-vous qu'aucun autre lecteur de journal ne s'exécute pour la même base de données de publication, et qu'aucune autre connexion active n'avait auparavant exécuté sp_replcmds/sp_repltrans/sp_repldone sans exécuter sp_replflush ensuite ou se déconnecter.  
  
