---
title: Propriétés de SQL Server (onglet Haute disponibilité AlwaysOn)
description: Pour utiliser des groupes de disponibilité comme une solution de haute disponibilité et de récupération d’urgence, activez la fonctionnalité Groupes de disponibilité AlwaysOn dans SQL Server.
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.technology: tools-other
ms.topic: conceptual
ms.assetid: d8630923-a600-4f1c-aca1-027453a3ec82
author: MikeRayMSFT
ms.author: mikeray
monikerRange: '>=sql-server-2016'
ms.openlocfilehash: 8890ede78dbc42a885e1d994fe3aee42100085dd
ms.sourcegitcommit: 108bc8e576a116b261c1cc8e4f55d0e0713d402c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/25/2021
ms.locfileid: "98766441"
---
# <a name="sql-server-properties-always-on-high-availability-tab"></a>Propriétés de SQL Server (onglet Haute disponibilité AlwaysOn)
[!INCLUDE [SQL Server Windows Only - ASDBMI ](../../includes/applies-to-version/sql-windows-only-asdbmi.md)]
  Utilisez l’onglet **Haute disponibilité AlwaysOn** de la boîte de dialogue **Propriétés de SQL Server** du Gestionnaire de configuration [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pour activer ou désactiver la fonctionnalité Groupes de disponibilité AlwaysOn de [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)]. L’activation de Groupes de disponibilité AlwaysOn est nécessaire pour qu’une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] utilise les groupes de disponibilité comme solution de haute disponibilité et de récupération d’urgence.  
  
##  <a name="prerequisites"></a><a name="Prerequisites"></a> Conditions préalables  
 Pour être activée pour Groupes de disponibilité AlwaysOn, une instance de serveur doit remplir les conditions suivantes :  
  
-   L'instance de serveur doit résider sur un nœud de Clustering de basculement Windows Server (WSFC).  
  
-   Pour appartenir au même groupe de disponibilité, les instances doivent être dans le même cluster WSFC. Un groupe de disponibilité ne peut pas s'étendre sur plusieurs clusters WSFC.  
  
-   L'instance du serveur doit exécuter une édition de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] qui prend en charge [!INCLUDE[ssHADR](../../includes/sshadr-md.md)].  
  
-   Activez Groupes de disponibilité AlwaysOn pour une seule instance de serveur à la fois. Après avoir activé Groupes de disponibilité AlwaysOn, attendez le redémarrage du service [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] avant d’activer l’instance de serveur suivante.  
  
> [!NOTE]  
>  Pour plus d’informations sur la prise en charge des fonctionnalités et sur les conditions préalables, restrictions et recommandations supplémentaires pour [!INCLUDE[ssHADR](../../includes/sshadr-md.md)], consultez la documentation en ligne de [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] .  
  
## <a name="dialog-options"></a>Options de boîte de dialogue  
 **Nom du cluster de basculement Windows**  
 Affiche le nom du cluster WSFC dans lequel l'ordinateur local est un nœud.  
  
 **Activer les groupes de disponibilité Always On**  
 Cochez cette case pour activer ou désactiver Groupes de disponibilité AlwaysOn sur cette instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], comme suit :  
  
-   Si cette case à cocher est vide, Groupes de disponibilité AlwaysOn est désactivé. Pour activer Groupes de disponibilité AlwaysOn, cochez cette case, cliquez sur **OK** et redémarrez manuellement le service [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
-   Si cette case est déjà cochée, Groupes de disponibilité AlwaysOn est actuellement activé. Pour désactiver Groupes de disponibilité AlwaysOn, décochez la case et cliquez sur **OK**. Cela entraîne le redémarrage de l'instance de serveur.  
  
    > [!TIP]  
    >  Après avoir désactivé Groupes de disponibilité AlwaysOn, supprimez les réplicas de disponibilité locaux de l’instance de serveur. Si vous supprimez la dernière réplica d'un groupe de disponibilité donné, supprimez également le groupe.  
  
## <a name="ui-element-list"></a>Liste d’éléments d’interface utilisateur  
  
> [!NOTE]  
>  Pour plus d’informations sur le suivi après la désactivation de Groupes de disponibilité AlwaysOn et la procédure à suivre pour créer et configurer des groupes de disponibilité, consultez la documentation en ligne de [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] .  
  
  
