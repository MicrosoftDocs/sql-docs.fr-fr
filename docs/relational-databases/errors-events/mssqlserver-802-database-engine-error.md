---
description: MSSQLSERVER_802 - Erreur du moteur de base de données
title: MSSQLSERVER_802 - Erreur du moteur de base de données | Microsoft Docs
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: language-reference
helpviewer_keywords:
- 802 (Database Engine error)
ms.assetid: 5892ed24-4dcb-4bf9-a8a4-a7ca898832d5
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: c6beb0d2b1930f03955918e1464d2a7eb966db66
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/17/2020
ms.locfileid: "88482717"
---
# <a name="mssqlserver_802---database-engine-error"></a>MSSQLSERVER_802 - Erreur du moteur de base de données
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>Détails  
  
| Attribut | Valeur |  
| :-------- | :---- |  
|Nom du produit|SQL Server|  
|ID de l’événement|802|  
|Source de l’événement|MSSQLSERVER|  
|Composant|SQLEngine|  
|Nom symbolique|NO_BUFS|  
|Texte du message|Mémoire disponible insuffisante dans le pool de mémoires tampons.|  
  
## <a name="explanation"></a>Explication  
Ceci survient lorsque le pool de mémoires tampons est plein et a atteint sa taille maximale.  
  
## <a name="user-action"></a>Action de l'utilisateur  
La liste suivante présente les procédures générales à suivre pour résoudre les erreurs de mémoire.  
  
1.  Vérifiez si d'autres applications ou services consomment de la mémoire sur ce serveur. Reconfigurez les applications ou les services moins importants pour consommer moins de mémoire.  
  
2.  Commencez la collecte des compteurs de l’analyseur de performances pour [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]  **: Buffer Manager**, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]  **: Memory Manager**.  
  
3.  Vérifiez les paramètres de configuration de la mémoire de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] suivants :  
  
    -   **Mémoire maximum du serveur**  
  
    -   **Mémoire minimum du serveur**  
  
    -   **Mémoire minimum par requête**  
  
    Identifiez tout paramètre inhabituel et corrigez-le si nécessaire. Prenez en compte l'augmentation de la mémoire requise pour [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Les paramètres par défaut sont répertoriés dans la rubrique « Définition des options de configuration de serveur » de la documentation en ligne de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
4.  Observez la sortie de DBCC MEMORYSTATUS et la façon dont elle change lorsque vous voyez ces messages d'erreur.  
  
5.  Vérifiez la charge de travail (le nombre de sessions simultanées, les requêtes en cours d'exécution).  
  
Les actions ci-dessous peuvent éventuellement augmenter la quantité de mémoire disponible pour [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] :  
  
-   Si des applications voisines de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] consomment des ressources, essayez d'en interrompre l'exécution ou pensez à les exécuter sur un serveur séparé.  
  
-   Si vous avez configuré le paramètre **max server memory**, augmentez sa valeur.  
  
Exécutez les commandes DBCC ci-dessous pour libérer plusieurs caches mémoire [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
-   DBCC FREESYSTEMCACHE  
  
-   DBCC FREESESSIONCACHE  
  
-   DBCC FREEPROCCACHE  
  
Si le problème persiste, vous devez poursuivre vos recherches et éventuellement, réduire la charge de travail.  
  
