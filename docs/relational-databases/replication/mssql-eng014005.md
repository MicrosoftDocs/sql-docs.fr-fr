---
description: MSSQL_ENG014005
title: MSSQL_ENG014005 | Microsoft Docs
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
helpviewer_keywords:
- MSSQL_ENG014005 error
ms.assetid: f168f0d6-cb11-45d4-9781-c374d7f388ee
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-mi-current||>=sql-server-2016
ms.openlocfilehash: 741ba12abae8aefc58e5dc9f8a8c3c11320f0891
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99198933"
---
# <a name="mssql_eng014005"></a>MSSQL_ENG014005
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]
    
## <a name="message-details"></a>Détails du message  
  
|Attribut|Valeur|  
|-|-|  
|Nom du produit|SQL Server|  
|ID de l’événement|14005|  
|Source de l’événement|MSSQLSERVER|  
|Composant|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]|  
|Nom symbolique||  
|Texte du message|Impossible de supprimer la publication. Il existe un abonnement.|  
  
## <a name="explanation"></a>Explication  
 Vous avez tenté de supprimer une publication qui a un ou plusieurs abonnements associés. Une publication peut être supprimée seulement s'il n'y a pas d'abonnements associés.  
  
## <a name="user-action"></a>Action de l'utilisateur  
 Supprimez les abonnements avant de supprimer la publication. Si vous utilisez [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] pour supprimer la publication, vous pouvez choisir de supprimer automatiquement tous les abonnements associés avant de supprimer la publication. Si vous utilisez des procédures stockées, vous devez d'abord explicitement supprimer les abonnements. Pour plus d'informations, consultez [Delete a Push Subscription](../../relational-databases/replication/delete-a-push-subscription.md) et [Delete a Pull Subscription](../../relational-databases/replication/delete-a-pull-subscription.md).  
  
 S'il semble ne pas exister d'abonnements pour la publication ou si vous voyez cette erreur quand vous créez une publication, il est possible qu'un abonnement antérieur n'ait pas été complètement nettoyé quand il a été supprimé. Exécutez [sp_removedbreplication &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-removedbreplication-transact-sql.md) sur la base de données pour supprimer tous les objets et paramètres liés à la réplication.  
  
## <a name="see-also"></a>Voir aussi  
 [Guide de référence des erreurs et des événements &#40;réplication&#41;](../../relational-databases/replication/errors-and-events-reference-replication.md)  
  
  
