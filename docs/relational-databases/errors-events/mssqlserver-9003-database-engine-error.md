---
description: MSSQLSERVER_9003
title: MSSQLSERVER_9003 | Microsoft Docs
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: language-reference
helpviewer_keywords:
- 9003 (Database Engine error)
ms.assetid: 7fdfb391-5c6f-428b-b434-6c3d0b30fd7b
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 0e49ed3ea5b9be9475a5db57716c7f49e5673200
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/17/2020
ms.locfileid: "88486983"
---
# <a name="mssqlserver_9003"></a>MSSQLSERVER_9003
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>Détails  
  
| Attribut | Valeur |  
| :-------- | :---- |  
|Nom du produit|SQL Server|  
|ID de l’événement|9003|  
|Source de l’événement|MSSQLSERVER|  
|Composant|SQLEngine|  
|Nom symbolique|LOG_INVALID_LSN|  
|Texte du message|Le numéro d'analyse du journal % S_LSN transmis à l'analyse du journal dans la base de données '%.*ls' n'est pas valide. Cette erreur peut indiquer les données sont endommagées ou que le fichier journal (.ldf) ne correspond pas au fichier de données (.mdf). Si cette erreur s'est produite pendant la réplication, recréez la publication. Sinon, restaurez les données à partir d'une sauvegarde si le problème se traduit par une défaillance lors du démarrage.|  
  
## <a name="explanation"></a>Explication  
Un composant a passé un numéro séquentiel dans le journal non valide au gestionnaire de fichiers journaux de la base de données. Il peut s'agir d'une réplication, d'une altération ou d'une incohérence entre le fichier de données primaires et le journal.  
  
## <a name="user-action"></a>Action de l'utilisateur  
Si cela s’est produit pendant la réplication, recréez la publication. Sinon, effectuez une restauration à partir de la sauvegarde.  
  
