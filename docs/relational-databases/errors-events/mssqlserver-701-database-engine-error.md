---
description: MSSQLSERVER_701
title: MSSQLSERVER_701 | Microsoft Docs
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 701 (Database Engine error)
ms.assetid: 3b975000-63a1-43c2-a40f-89d0a8a36bef
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 5df51550a44455b728222aa02ea2f507a0b7d4fd
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99194131"
---
# <a name="mssqlserver_701"></a>MSSQLSERVER_701
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>Détails  
  
| Attribut | Valeur |  
| :-------- | :---- |  
|Nom du produit|SQL Server|  
|ID de l’événement|701|  
|Source de l’événement|MSSQLSERVER|  
|Composant|SQLEngine|  
|Nom symbolique|NOSYSMEM|  
|Texte du message|Mémoire système insuffisante pour exécuter cette requête.|  
  
## <a name="explanation"></a>Explication  
[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] n'a pas réussi à allouer suffisamment de mémoire pour exécuter la requête. Cela peut être dû à diverses raisons, notamment des paramètres de système d’exploitation, une disponibilité de mémoire physique ou des limites de mémoire sur la charge de travail en cours. Dans la plupart des cas, la transaction qui a échoué n’est pas la cause de cette erreur.  
  
Les requêtes de diagnostic, telles que les instructions DBCC, peuvent échouer parce que la mémoire du serveur est insuffisante.  
  
Le délai a été dépassé pendant l'attente de ressources mémoire pour exécuter la requête dans le pool de ressources 'default'.  
  
## <a name="user-action"></a>Action de l'utilisateur  
Si vous n'utilisez pas le gouverneur de ressources, nous vous recommandons de vérifier l'état général et la charge du serveur, ou les paramètres du pool de ressources ou du groupe de charges de travail.  
  
La liste suivante présente les procédures générales à suivre pour résoudre les erreurs de mémoire.  
  
1.  Vérifiez si d'autres applications ou services consomment de la mémoire sur ce serveur. Reconfigurez les applications ou les services moins importants pour consommer moins de mémoire.  
  
2.  Commencez la collecte des compteurs de l’analyseur de performances pour [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]  **: Buffer Manager**, **SQL Server : Memory Manager**.  
  
3.  Vérifiez les paramètres de configuration de la mémoire de SQL Server suivants :  
  
    -   **Mémoire maximum du serveur**  
  
    -   **Mémoire minimum du serveur**  
  
    -   **Mémoire minimum par requête**  
  
    Identifiez les paramètres inhabituels. Si besoin est, corrigez-les. Prenez en compte l'augmentation de la mémoire requise. Les paramètres par défaut sont répertoriés dans la rubrique « Définition des options de configuration de serveur » de la documentation en ligne de SQL Server.  
  
4.  Observez la sortie de DBCC MEMORYSTATUS et la façon dont elle change lorsque vous voyez ces messages d'erreur.  
  
5.  Vérifiez la charge de travail (par exemple, le nombre de sessions simultanées, les requêtes en cours d'exécution).  

Les actions ci-dessous peuvent éventuellement augmenter la quantité de mémoire disponible pour [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] :  
  
-   Si des applications autres que [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] consomment des ressources, essayez d'arrêter l'exécution de ces applications ou envisagez de les exécuter sur un serveur distinct. Vous relâcherez ainsi la pression sur la mémoire externe.  
  
-   Si vous avez configuré le paramètre **Mémoire maximum du serveur**, augmentez sa valeur.  
  
Exécutez les commandes DBCC ci-dessous pour libérer plusieurs caches mémoire [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
-   DBCC FREESYSTEMCACHE  
  
-   DBCC FREESESSIONCACHE  
  
-   DBCC FREEPROCCACHE  
  
Si le problème persiste, vous devez poursuivre vos recherches et éventuellement, réduire la charge de travail.  
  
