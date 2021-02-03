---
description: MSSQLSERVER_4846
title: MSSQLSERVER_4846 | Microsoft Docs
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 4846 (Database Engine error)
ms.assetid: a455e809-1883-4c7d-b3e3-835ee5bfe258
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 789b6220c0b3b61fba1567c88ca5405d7b1e8895
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99185581"
---
# <a name="mssqlserver_4846"></a>MSSQLSERVER_4846
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>Détails  
  
| Attribut | Valeur |  
| :-------- | :---- |  
|Nom du produit|SQL Server|  
|ID de l’événement|4846|  
|Source de l’événement|MSSQLSERVER|  
|Composant|SQLEngine|  
|Nom symbolique|BULKPROV_MEMORY|  
|Texte du message|Le fournisseur de données en bloc n'est pas parvenu à allouer de la mémoire.|  
  
## <a name="explanation"></a>Explication  
L'allocation de mémoire a échoué.  
  
## <a name="user-action"></a>Action de l'utilisateur  
Pour corriger des erreurs de mémoire, procédez comme suit :  
  
1.  Vérifiez si d'autres applications ou services consomment de la mémoire sur ce serveur. Reconfigurez les applications ou les services moins importants pour consommer moins de mémoire.  
  
2.  Démarrez la collecte des compteurs de l’analyseur de performances pour **SQL Server : Buffer Manager**, **SQL Server : Memory Manager**.  
  
3.  Vérifiez les paramètres de configuration de la mémoire de SQL Server suivants :  
  
    -   **Mémoire maximum du serveur**  
  
    -   **Mémoire minimum du serveur**  
  
    -   **Mémoire minimum par requête**  
  
    Notez tous les paramètres inhabituels. Si besoin est, corrigez-les. Prenez en compte la mémoire requise pour [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)]. Les paramètres par défaut sont répertoriés dans la rubrique « Définition des options de configuration de serveur » de la documentation en ligne de SQL Server.  
  
4.  Observez la sortie de DBCC MEMORYSTATUS et la façon dont elle change lorsque vous voyez ces messages d'erreur.  
  
5.  Vérifiez la charge de travail (par exemple, le nombre de sessions simultanées, les requêtes en cours d'exécution).  
  
Les actions ci-dessous peuvent éventuellement augmenter la quantité de mémoire disponible pour [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] :  
  
-   Si des applications autres que SQL Server consomment des ressources, essayez d'arrêter l'exécution de ces applications ou envisagez de les exécuter sur un serveur distinct. Vous relâcherez ainsi la pression sur la mémoire externe.  
  
-   Si vous avez configuré le paramètre **Mémoire maximum du serveur**, augmentez sa valeur.  
  
Exécutez les commandes DBCC ci-dessous pour libérer plusieurs caches mémoire [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
-   DBCC FREESYSTEMCACHE  
  
-   DBCC FREESESSIONCACHE  
  
-   DBCC FREEPROCCACHE  
  
Si le problème persiste, vous devez poursuivre vos recherches et éventuellement, réduire la charge de travail.  
  
