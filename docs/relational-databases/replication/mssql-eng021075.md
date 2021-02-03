---
description: MSSQL_ENG021075
title: MSSQL_ENG021075 | Microsoft Docs
ms.custom: ''
ms.date: 03/07/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
helpviewer_keywords:
- MSSQL_ENG021075 error
ms.assetid: c8c29543-d1f6-49d5-b6c8-e8c3aa373090
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-mi-current||>=sql-server-2016
ms.openlocfilehash: 046c21ab2758ec1e9147a04f839ddd0e2b4aaa1d
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99187053"
---
# <a name="mssql_eng021075"></a>MSSQL_ENG021075
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]
    
## <a name="message-details"></a>Détails du message  
  
|Attribut|Valeur|  
|-|-|  
|Nom du produit|SQL Server|  
|ID de l’événement|21075|  
|Source de l’événement|MSSQLSERVER|  
|Composant|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]|  
|Nom symbolique||  
|Texte du message|L'instantané initial de la publication '%1!' n'est pas encore disponible.|  
  
## <a name="explanation"></a>Explication  
 L'erreur MSSQL_ENG021075 est signalée si l'Agent de Distribution ou l'Agent de fusion est démarré avant que l'Agent d'instantané ait terminé la génération de l'instantané.  
  
## <a name="user-action"></a>Action de l'utilisateur  
 Si l'Agent d'instantané pour la publication n'a pas été démarré depuis que l'abonnement a été créé, ou s'il n'a pas été démarré depuis la dernière fois que vous avez choisi de réinitialiser l'abonnement, démarrez l'Agent d'instantané et laissez-le terminer avant de démarrer l'Agent de distribution ou l'Agent de fusion. Pour plus d’informations, consultez [Créer et appliquer un instantané](../../relational-databases/replication/create-and-apply-the-initial-snapshot.md).  
  
 Si l'Agent d'instantané ne se termine pas, recherchez les erreurs dans son historique et résolvez-les. Pour obtenir des informations sur l’affichage de l’état de l’agent et des détails de l’erreur dans le moniteur de réplication, consultez [Afficher des informations et effectuer des tâches à l’aide du moniteur de réplication](../../relational-databases/replication/monitor/view-information-and-perform-tasks-replication-monitor.md).  
  
 Si l'erreur continue de se produire, augmentez le facteur de journalisation de l'agent et spécifiez un fichier de sortie pour le journal. En fonction du contexte de l'erreur, cette action peut fournir des pistes conduisant à l'erreur et/ou à d'autres messages d'erreur.  
  
## <a name="see-also"></a>Voir aussi  
 [Guide de référence des erreurs et des événements &#40;réplication&#41;](../../relational-databases/replication/errors-and-events-reference-replication.md)  
  
  
