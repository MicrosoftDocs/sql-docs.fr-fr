---
description: Outil de résolution des conflits de réplication Microsoft interactif
title: Outil de résolution des conflits interactif (fusionner)
describes: Describes the Interactive Conflict Resolver that can be used for merge subscriptions that are synchronized using the Windows Synchronization Manager.
ms.custom: seo-lt-2019
ms.date: 03/07/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
f1_keywords:
- sql13.rep.replconflictviewer.interactiveresolver.f1
ms.assetid: d3d4a480-782b-4b1d-b839-565c8cf6cb24
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-current||>=sql-server-2016||=sqlallproducts-allversions
ms.openlocfilehash: c0ecc10fd8155a0f18e216458d2dcd47132d35d4
ms.sourcegitcommit: 4d370399f6f142e25075b3714e5c2ce056b1bfd0
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/09/2020
ms.locfileid: "91868087"
---
# <a name="microsoft-replication-interactive-conflict-resolver"></a>Outil de résolution des conflits de réplication Microsoft interactif
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
  L'Outil de résolution des conflits de réplication Microsoft interactif peut s'utiliser pour les abonnements de fusion synchronisés au moyen du Gestionnaire de synchronisation Windows. Il permet d'afficher, comparer, modifier et sélectionner les conflits dans les données des résultats. La réplication comporte également l'Outil de résolution des conflits qui permet d'afficher et de modifier les résultats des conflits après leur validation. L'Outil de résolution des conflits interactif permet de sélectionner le résultat pendant la synchronisation.  
  
> [!NOTE]  
>  Les conflits qui concernent des enregistrements logiques ne sont pas affichés dans le résolveur interactif. Pour afficher des informations sur ces conflits, utilisez des procédures stockées de réplication. Pour plus d’informations, consultez [Afficher les informations relatives aux conflits pour les publications de fusion &#40;programmation Transact-SQL de la réplication&#41;](./view-and-resolve-data-conflicts-for-merge-publications.md).  
  
## <a name="options"></a>Options  
 **Nom de la colonne**  
 Noms de toutes les colonnes de la table. Une ou plusieurs colonnes peuvent comporter des données en conflit. Indépendamment des colonnes en conflit, la ligne gagnante complète remplace la totalité de la ligne perdante.  
  
 **Résolution suggérée**  
 Résolution du conflit suggérée par l'outil de résolution des conflits pour l'article.  
  
 **Publisher**  
 Valeur des données du serveur de publication.  
  
 **Abonné**  
 Valeur des données de l'abonné.  
  
 **Accepter la suggestion**, **Accepter le serveur de publication**et **Accepter l'Abonné**  
 Cliquez sur l'option correspondante pour accepter la ligne qui sera appliquée par le serveur de publication ou l'abonné, en fonction du perdant du conflit. Si le serveur de publication perd le conflit, tous les autres abonnés reçoivent la ligne gagnante lors de leur prochaine synchronisation avec le serveur de publication.  
  
 **Résoudre automatiquement tous les conflits restants**  
 Résout tous les conflits restants en utilisant la résolution suggérée par l'outil de résolution des conflits pour l'article.  
  
 **Journaliser les détails de ce conflit pour future référence**  
 Enregistre les détails du conflit dans des tables système.  
  
## <a name="see-also"></a>Voir aussi  
 [Résolution interactive des conflits](../../relational-databases/replication/merge/advanced-merge-replication-conflict-interactive-resolution.md)   
 [Afficher et résoudre les conflits de données pour les publications de fusion &#40;SQL Server Management Studio&#41;](../../relational-databases/replication/view-and-resolve-data-conflicts-for-merge-publications.md)   
 [Synchroniser un abonnement à l’aide du Gestionnaire de synchronisation Windows &#40;Gestionnaire de synchronisation Windows&#41;](../../relational-databases/replication/synchronize-a-subscription-using-windows-synchronization-manager.md)   
 [Advanced Merge Replication Conflict Detection and Resolution](../../relational-databases/replication/merge/advanced-merge-replication-conflict-detection-and-resolution.md)  
  
