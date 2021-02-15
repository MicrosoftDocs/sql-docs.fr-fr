---
description: Tâches et autorisations - Tâches au niveau élément
title: Tâches au niveau élément | Microsoft Docs
ms.date: 02/04/2021
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: security
ms.topic: conceptual
helpviewer_keywords:
- item-level tasks [Reporting Services]
ms.assetid: fdeb7bc3-167a-4342-84e3-32e3faa1fa39
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 82101577abe86914aff2ac1d5296d7dc176a60b0
ms.sourcegitcommit: 6f4fb9cfd0cad06127a6328adc745e2ba7c191d1
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/05/2021
ms.locfileid: "99570433"
---
# <a name="tasks-and-permissions---item-level-tasks"></a>Tâches et autorisations - Tâches au niveau élément
  
  [!INCLUDE[ssrs-appliesto](../../includes/ssrs-appliesto.md)] [!INCLUDE[ssrs-appliesto-2016-and-later](../../includes/ssrs-appliesto-2016-and-later.md)] [!INCLUDE[ssrs-appliesto-pbirsi](../../includes/ssrs-appliesto-pbirs.md)]
  
  Une tâche au niveau élément est une collection d'autorisations liées à un rapport, un dossier, un modèle de rapport, une ressource ou une source de données partagée. [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] inclut également des tâches de niveau système qui s’appliquent à l’ensemble du site de serveur de rapports. Pour plus d’informations, consultez [Tâches au niveau système](../../reporting-services/security/tasks-and-permissions-system-level-tasks.md). Pour plus d'informations sur les tâches et les autorisations en général, consultez [Tasks and Permissions](../../reporting-services/security/tasks-and-permissions.md).  
  
> [!NOTE]  
>  Si vous travaillez avec ces tâches par programmation, vous devez utiliser des méthodes qui prennent en charge les tâches au niveau élément. Pour plus d’informations, consultez <xref:ReportService2010.ReportingService2010.ListTasks%2A> et <xref:ReportService2010.ReportingService2010.ListRoles%2A>.  
  
## <a name="permissions-in-item-level-tasks"></a>Autorisations dans les tâches au niveau élément  
 Le tableau suivant répertorie les tâches au niveau élément, les autorisations comprises dans chaque tâche et les éléments auxquels s'appliquent les autorisations. Les autorisations sont répertoriées à titre d'informations uniquement, afin de fournir une description plus exacte de la fonctionnalité disponible via chaque tâche.  
  
 Les datasets partagés utilisent le même jeu d'autorisations que les rapports. Les parties de rapports utilisent le même jeu d'autorisations que les ressources.  
  
|Tâche|S'applique à l'élément|Autorisations|  
|----------|---------------------|-----------------|  
|Effectuer des commentaires sur les rapports<br />(SSRS 2017 et ultérieur, Power BI Report Server)|Rapports|Lire les propriétés<br /><br /> Créer des commentaires<br /><br /> Supprimer des commentaires<br /><br /> Lire les commentaires<br /><br /> Mettre à jour les commentaires|  
|Lire les rapports|Rapports|Lire le contenu<br /><br /> Lire les définitions de rapport<br /><br /> Lire les propriétés|  
|Lire les rapports|Datasets partagés|Lire le contenu<br /><br /> Lire les définitions de rapport<br /><br /> Lire les propriétés|  
|Créer des rapports liés|Rapports|Créer un lien<br /><br /> Lire les propriétés|  
|Gérer tous les abonnements|Rapports|Lire les propriétés<br /><br /> Lire n'importe quel abonnement<br /><br /> Créer n'importe quel abonnement<br /><br /> Supprimer n'importe quel abonnement<br /><br /> Mettre à jour n'importe quel abonnement|  
|Gérer les commentaires<br />(SSRS 2017 et ultérieur, Power BI Report Server)|Rapports|Lire les propriétés<br /><br />Supprimer n’importe quel commentaire|  
|Gérer les sources de données|Dossiers|Créer une source de données|  
|Gérer les sources de données|Data Sources|Mettre à jour les propriétés<br /><br /> Supprimer le contenu de mise à jour<br /><br /> Lire les propriétés|  
|Gérer les dossiers|Dossiers|Créer un dossier<br /><br /> Supprimer les propriétés de mise à jour<br /><br /> Lire les propriétés|  
|Gérer les abonnements individuels|Rapports|Lire les propriétés<br /><br /> Créer un abonnement<br /><br /> Supprimer un abonnement<br /><br /> Lire un abonnement<br /><br /> Mettre à jour un abonnement|  
|Gérer les modèles|Dossiers|Créer le modèle|  
|Gérer les modèles|Modèles|Lire les propriétés<br /><br /> Lire le contenu<br /><br /> Supprimer le contenu de mise à jour<br /><br /> Lire les sources de données<br /><br /> Mettre à jour les sources de données<br /><br /> Lire les stratégies d'autorisation des éléments de modèles<br /><br /> Mettre à jour les stratégies d'autorisation des éléments de modèles<br /><br /> Supprimer les propriétés de mise à jour|  
|Gérer l'historique de rapport|Rapports|Lire les propriétés<br /><br /> Créer un historique de rapport<br /><br /> Supprimer un historique de rapport<br /><br /> Exécuter la stratégie de lecture<br /><br /> Mettre à jour la stratégie<br /><br /> Répertorier l'historique de rapport|  
|Gérer les rapports|Dossiers|Créer un rapport<br /><br /> s'applique également à la création de datasets partagés|  
|Gérer les rapports|Rapports|Lire les propriétés<br /><br /> Supprimer les propriétés de mise à jour<br /><br /> Mettre à jour les paramètres<br /><br /> Lire les sources de données<br /><br /> Mettre à jour les sources de données<br /><br /> Lire la définition de rapport<br /><br /> Mettre à jour à la définition de rapport<br /><br /> Exécuter la stratégie de lecture<br /><br /> Mettre à jour la stratégie|  
|Gérer les rapports|Datasets partagés|Lire les propriétés<br /><br /> Supprimer les propriétés de mise à jour<br /><br /> Mettre à jour les paramètres<br /><br /> Lire les sources de données<br /><br /> Mettre à jour les sources de données<br /><br /> Lire la définition de rapport<br /><br /> Mettre à jour à la définition de rapport<br /><br /> Exécuter la stratégie de lecture<br /><br /> Mettre à jour la stratégie|  
|Gestion des ressources|Dossiers|Créer une ressource|  
|Gestion des ressources|Ressources|Mettre à jour les propriétés<br /><br /> Supprimer le contenu de mise à jour<br /><br /> Lire les propriétés|  
|Gestion des ressources|Parties de rapports|Mettre à jour les propriétés<br /><br /> Supprimer le contenu de mise à jour<br /><br /> Lire les propriétés|  
|Définir la sécurité pour des éléments individuels|Rapports, ressources, sources de données, datasets Partagés, dossiers|Lire les stratégies de sécurité Mettre à jour les stratégies de sécurité|  
|Afficher les sources de données|Sources de données|Lire le contenu<br /><br /> Lire les propriétés|  
|Afficher les dossiers|Dossiers|Lire les propriétés<br /><br /> Exécuter et afficher<br /><br /> Répertorier l'historique de rapport|  
|Afficher les modèles|Modèles de rapport|Lire les propriétés<br /><br /> Lire le contenu<br /><br /> Lire les sources de données|  
|Affichage des rapports|Rapports|Lire le contenu<br /><br /> Lire les propriétés|  
|Affichage des rapports|Datasets partagés|Lire le contenu<br /><br /> Lire les propriétés|  
|Afficher les ressources|Ressources|Lire le contenu<br /><br /> Lire les propriétés|  
|Afficher les ressources|Parties de rapports|Lire le contenu<br /><br /> Lire les propriétés|  
  
## <a name="see-also"></a>Voir aussi  
 [Octroi d'autorisations sur un serveur de rapports en mode natif](../../reporting-services/security/granting-permissions-on-a-native-mode-report-server.md)  
  
  
