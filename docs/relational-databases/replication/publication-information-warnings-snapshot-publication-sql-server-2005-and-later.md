---
description: Informations de publication, Avertissements (Publication d'instantané, SQL Server 2005 et versions ultérieures)
title: Avertissements (Instantané - Moniteur de réplication)
decription: Describes the 'Warnings' tab for a Snapshot Publication in the Replication Monitor found in SQL Server Management Studio (SSMS).
ms.custom: seo-lt-2019
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
f1_keywords:
- sql13.rep.monitor.publicationinfo.warningsandagents.snapshot.f1
ms.assetid: 7aa2eb52-b6b7-4dd3-8483-8ef00d9f0435
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-mi-current||>=sql-server-2016
ms.openlocfilehash: 30520fe81c7de54a154ed8b287b9006b1bd6a8fe
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/14/2020
ms.locfileid: "97469020"
---
# <a name="publication-information-warnings-snapshot-publication-sql-server-2005-and-later"></a>Informations de publication, Avertissements (Publication d'instantané, SQL Server 2005 et versions ultérieures)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]
  L'onglet **Avertissements** est disponible pour les serveurs de distribution qui exécutent [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] et versions ultérieures. L'onglet **Avertissements** vous permet d'effectuer les tâches suivantes selon la publication sélectionnée :  
  
-   activer les avertissements ;  
  
-   définir des seuils associés aux avertissements ;  
  
-   définir des alertes associées aux avertissements ;  
  
## <a name="warnings-thresholds-and-alerts"></a>Avertissements, seuils et alertes  
 Le moniteur de réplication affiche par défaut des avertissements relatifs aux abonnements non initialisés : l’état **Abonnement non initialisé** s’affiche en tant qu’avertissement dans la colonne **État** des pages incluant des informations propres aux abonnements. Pour les publications de instantanés, vous pouvez également indiquer que l'expiration d'abonnement imminente génère un avertissement en définissant l'option **Avertir si un abonnement expire avant le seuil défini**. Si le seuil précisé est atteint ou dépassé, l'état de l'abonnement se change alors en **Expire bientôt/Expiré** (à moins qu'un problème dont la priorité est supérieure doit être affiché avant l'état de l'abonnement).  
  
 L'atteinte d'un seuil déclenche un avertissement dans le Moniteur de réplication, mais également une alerte. Les alertes sont définies en cliquant sur **Configurer des alertes** et en fournissant les informations nécessaires dans la boîte de dialogue **Configurer les alertes de réplication** .  
  
## <a name="options"></a>Options  
 **Activé**  
 Permet d'activer un avertissement et de définir un seuil.  
  
 **Avertissement**  
 Description de l'avertissement associé à un seuil.  
  
 **Seuil**  
 Permet de spécifier une valeur pour le seuil.  
  
 **Configurer des alertes**  
 Sélectionnez une ligne dans la grille **Avertissements** , puis cliquez sur **Configurer des alertes** pour ouvrir la boîte de dialogue **Configurer des alertes de réplication** . Cette boîte de dialogue permet de définir une alerte associée au seuil et à l'avertissement sélectionnés.  
  
 **Ignorer les modifications**  
 Permet d'annuler les modifications apportées aux avertissements et aux seuils.  
  
> [!NOTE]  
>  Cliquer sur **Ignorer les modifications** n'affecte en rien les alertes définies dans la boîte de dialogue **Configurer les alertes de réplication** .  
  
 **Save Changes**  
 Permet d'enregistrer toute modification apportée aux avertissements et aux seuils.  
  
## <a name="see-also"></a>Voir aussi  
 [Démarrer le Moniteur de réplication](../../relational-databases/replication/monitor/start-the-replication-monitor.md)   
 [Afficher des informations et effectuer des tâches à l’aide du moniteur de réplication](../../relational-databases/replication/monitor/view-information-and-perform-tasks-replication-monitor.md)   
 [Surveillance de la réplication](../../relational-databases/replication/monitor/monitoring-replication.md)  
  
  
