---
description: Agent d'instantané (Assistant Nouvelle publication)
title: Agent d’instantané (Assistant Nouvelle publication) | Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
f1_keywords:
- sql13.rep.newpubwizard.configuresnapshotagent.f1
ms.assetid: 0257d4ee-1f7b-49fd-b4ef-65bfc1ef6951
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-mi-current||>=sql-server-2016
ms.openlocfilehash: 6dab75938bc65b741bd556447d140c57d9557184
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/14/2020
ms.locfileid: "97479700"
---
# <a name="snapshot-agent-new-publication-wizard"></a>Agent d'instantané (Assistant Nouvelle publication)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]
  L'Agent d'instantané crée des fichiers qui contiennent le schéma de publication et les données utilisées pour initialiser de nouveaux abonnements. Par défaut, il s'exécute immédiatement après la création de la publication dans l'Assistant Nouvelle publication. Par la suite, il s'exécute selon une planification spécifiée. La création de nouveaux fichiers d'instantané par l'agent à chaque exécution dépend du type de réplication et des options choisies. Pour plus d’informations, consultez [Créer et appliquer un instantané](../../relational-databases/replication/create-and-apply-the-initial-snapshot.md).  
  
 Pour les publications de fusion utilisant les filtres paramétrés, vous devez créer un instantané pour chaque partition de données après la fin de l'instantané pour la publication. Pour plus d'informations, voir [Snapshots for Merge Publications with Parameterized Filters](../../relational-databases/replication/create-a-snapshot-for-a-merge-publication-with-parameterized-filters.md).  
  
## <a name="options"></a>Options  
 **Créer un instantané immédiatement** (réplication de fusion) ou **Créer un instantané immédiatement et garder cette dernière disponible pour l'initialisation des abonnements** (réplication transactionnelle)  
 Activez cette case à cocher pour créer un instantané immédiatement après la fin de l'Assistant Nouvelle publication. Désactivez cette case à cocher si vous comptez modifier les propriétés d'instantané dans la boîte de dialogue **Propriétés de la publication** avant la génération d'un instantané ou si vous lancez l'abonné sans instantané. Pour plus d’informations, consultez [Initialiser un abonnement transactionnel sans instantané](../../relational-databases/replication/initialize-a-transactional-subscription-without-a-snapshot.md).  
  
> [!NOTE]  
>  Il se peut que l'Assistant demande une connexion au serveur de distribution pour pouvoir démarrer le travail approprié pour l'Agent de distribution ou l'Agent de fusion.  
  
 **Planifier l'exécution de l'Agent d'instantané aux heures suivantes**  
 Acceptez la planification par défaut pour l'exécution de l'Agent d'instantané ou cliquez sur **Modifier** pour en spécifier une.  
  
## <a name="see-also"></a>Voir aussi  
 [Create a Publication](../../relational-databases/replication/publish/create-a-publication.md)   
 [Créer et appliquer l’instantané initial](../../relational-databases/replication/create-and-apply-the-initial-snapshot.md)   
 [Afficher et modifier les propriétés d’une publication](../../relational-databases/replication/publish/view-and-modify-publication-properties.md)   
 [Initialiser un abonnement avec un instantané](../../relational-databases/replication/initialize-a-subscription-with-a-snapshot.md)   
 [Publier des données et des objets de base de données](../../relational-databases/replication/publish/publish-data-and-database-objects.md)   
 [Présentation des Agents de réplication](../../relational-databases/replication/agents/replication-agents-overview.md)  
  
  
