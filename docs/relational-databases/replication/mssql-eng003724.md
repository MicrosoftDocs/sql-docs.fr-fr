---
description: MSSQL_ENG003724
title: MSSQL_ENG003724 | Microsoft Docs
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
helpviewer_keywords:
- MSSQL_ENG003724 error
ms.assetid: 10cb119d-92df-4124-b85d-cd2f2666c99c
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-mi-current||>=sql-server-2016
ms.openlocfilehash: ba60ce4e15ce700a08b417e513638dcb47785282
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99198951"
---
# <a name="mssql_eng003724"></a>MSSQL_ENG003724
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]
    
## <a name="message-details"></a>Détails du message  
  
|Attribut|Valeur|  
|-|-|  
|Nom du produit|SQL Server|  
|ID de l’événement|3724|  
|Source de l’événement|MSSQLSERVER|  
|Composant|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]|  
|Nom symbolique||  
|Texte du message|Impossible de% S_MSG% S_MSG'%. * ls', car il est utilisé pour la réplication.|  
  
## <a name="explanation"></a>Explication  
 Lorsque des objets d'une base de données sont répliqués, ils sont marqués comme étant répliqués dans la table système **sysarticles** (publications transactionnelles ou d'instantanés) ou dans la table système **sysmergearticles** (publications de fusion). Si vous tentez de supprimer un objet répliqué, cette erreur est générée.  
  
## <a name="user-action"></a>Action de l'utilisateur  
 Vérifiez que l'objet de base de données n'est pas répliqué avant d'essayer de le supprimer. Par exemple :  
  
-   Si l'erreur se produit dans la base de données de publication, supprimez l'article de la publication avant de supprimer l'objet. Pour plus d’informations, consultez [Ajouter et supprimer des articles de publications existantes](../../relational-databases/replication/publish/add-articles-to-and-drop-articles-from-existing-publications.md).  
  
-   Si l'erreur se produit dans la base de données d'abonnement, supprimez l'abonnement avant de supprimer l'objet. Pour plus d’informations, consultez [S’abonner à des publications](../../relational-databases/replication/subscribe-to-publications.md). Dans le cas d'abonnements aux publications transactionnelles, il est possible de supprimer l'abonnement à un article individuel plutôt qu'à la publication complète. Pour plus d’informations, consultez [sp_dropsubscription &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-dropsubscription-transact-sql.md).  
  
 Si cette erreur se produit dans une base de données qui n’est pas répliquée, exécutez [sp_removedbreplication &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-removedbreplication-transact-sql.md) pour vérifier que les objets de la base de données ne sont pas marqués comme étant répliqués.  
  
## <a name="see-also"></a>Voir aussi  
 [Guide de référence des erreurs et des événements &#40;réplication&#41;](../../relational-databases/replication/errors-and-events-reference-replication.md)  
  
  
