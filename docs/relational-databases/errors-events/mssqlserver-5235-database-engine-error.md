---
description: MSSQLSERVER_5235
title: MSSQLSERVER_5235 | Microsoft Docs
ms.custom: ''
ms.date: 09/05/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 5235 (Database Engine error)
ms.assetid: 1aa7e6a5-7ccb-43c8-a1fd-d50e92e0a798
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 1a81590c727a34dfe272a03ec6319f487c02e9f9
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99198137"
---
# <a name="mssqlserver_5235"></a>MSSQLSERVER_5235
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>Détails  
  
| Attribut | Valeur |  
| :-------- | :---- |  
|Nom du produit|SQL Server|  
|ID de l’événement|5235|  
|Source de l’événement|MSSQLSERVER|  
|Composant|SQLEngine|  
|Nom symbolique|DBCC4_ERRORLOG_SUMMARY_PREMATURE_TERMINATION|  
|Texte du message|[EMERGENCY] DBCC DBCC_COMMAND_DETAILS exécuté par USER_NAME s'est terminé anormalement à cause de l'état d'erreur ERROR_STATE. Temps écoulé : HOURS heures MINUTES minutes SECONDS secondes.|  
  
## <a name="explanation"></a>Explication  
Il s'agit du message de synthèse que DBCC inscrit dans le journal des erreurs [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] lorsqu'un arrêt inattendu se produit au cours de l'exécution de la commande. L'état d'erreur signalé dans le message définit le type d'arrêt inattendu.  
  
Le tableau ci-dessous liste et définit les états d'erreur.  
  
|État d'erreur|Définition|  
|---------------|--------------|  
|État 1|L'instruction a pris fin en raison d'une altération des métadonnées qui s'est avérée fatale. Ce message est accompagné d’une ou plusieurs instances de l’erreur 8930.|  
|État 2|L'instruction a pris fin en raison d'un échec de contrôle interne. Ce message est accompagné d’une ou plusieurs instances de l’erreur 8967.|  
|État 3|Les contrôles de table système basiques effectués par les principales tables système du moteur de stockage ont échoué. Ce message est accompagné d’une ou de plusieurs instances des erreurs [7984](../../relational-databases/errors-events/mssqlserver-7984-database-engine-error.md), 7985, [7986](~/relational-databases/errors-events/mssqlserver-7986-database-engine-error.md), [7987](~/relational-databases/errors-events/mssqlserver-7987-database-engine-error.md) ou [7988](~/relational-databases/errors-events/mssqlserver-7988-database-engine-error.md).|  
|État 4|La réparation en mode urgence DBCC a échoué parce que la base de données n'a pas pu être démarrée après la reconstruction du journal des transactions. Ce message est accompagné de l’erreur 7909.|  
|État 5|Un échec d'assertion ou une violation d'accès se sont produits au cours de l'exécution de la commande.|  
|État 6|Une panne inconnue s'est produite et a arrêté la commande DBCC de manière inattendue.|  
|État 7|Arrêt anormal en raison d’une erreur sur le réplica (Always On).|  
  
## <a name="user-action"></a>Action de l'utilisateur  
Le tableau ci-dessous décrit les actions que l'utilisateur doit effectuer pour l'état d'erreur spécifié.  
  
|État d'erreur|Action requise|  
|---------------|---------------|  
|État 1|Procédez à une restauration à partir d'une sauvegarde.|  
|État 2|Contactez le [!INCLUDE[msCoName](../../includes/msconame-md.md)] Service clientèle et le Support technique.|  
|État 3|Procédez à une restauration à partir d'une sauvegarde.|  
|État 4|Procédez à une restauration à partir d'une sauvegarde.|  
|État 4|Contactez le Service clientèle et le Support technique.|  
|État 6|Réexécutez la commande. Si le problème persiste, contactez le Service clientèle et le Support technique.|  
  
## <a name="see-also"></a>Voir aussi  
[DBCC &#40;Transact-SQL&#41;](~/t-sql/database-console-commands/dbcc-transact-sql.md)  
  
