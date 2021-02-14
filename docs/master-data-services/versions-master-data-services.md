---
title: Versions
description: Vous pouvez créer plusieurs versions des données de référence dans un modèle dans Master Data Services. En savoir plus sur les versions et leur utilisation.
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
helpviewer_keywords:
- version flags [Master Data Services], about version flags
- versions [Master Data Services]
- version flags [Master Data Services]
- versions [Master Data Services], version flags
ms.assetid: 752ec96d-53d7-4160-8ed2-92e0324645f3
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: 54c250aa3b6a6e803f90522218b1eef854af7903
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100348844"
---
# <a name="versions-master-data-services"></a>Versions (Master Data Services)

[!INCLUDE [SQL Server - Windows only ASDBMI  ](../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  Dans [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)], vous pouvez créer plusieurs versions des données de référence dans un modèle. Les versions peuvent être verrouillées pendant que vous validez vos données et activées une fois les données validées. Les versions activées constituent un enregistrement des modifications pouvant être audité. Chaque version que vous créez contient l'ensemble des membres, valeurs d'attribut, membres de hiérarchie, relations de hiérarchie et collections pour le modèle.  
  
## <a name="when-to-use-versions"></a>Cas d'utilisation des versions  
 Utilisez les versions pour :  
  
-   gérer un enregistrement vérifiable de vos données de référence à mesure de leur modification ;  
  
-   empêcher des utilisateurs d'apporter des modifications pendant que vous vous assurez de la validation réussie de toutes les données par rapport aux règles d'entreprise ;  
  
-   verrouiller un modèle qui doit être utilisé par des systèmes d'abonnement ;  
  
-   tester différentes hiérarchies sans les implémenter immédiatement.  
  
> [!NOTE]  
>  Lorsque vous modifiez la structure de votre modèle, par exemple en créant une entité ou un attribut basé sur un domaine, la modification s'applique à toutes les versions. Si vous affichez une version antérieure du modèle, l'entité ou l'attribut est affiché, mais sans aucune donnée.  
  
## <a name="version-flags"></a>Indicateurs de version  
 Lorsqu'une version est prête pour les utilisateurs ou pour un système d'abonnement, vous pouvez définir un indicateur pour identifier la version. Vous pouvez déplacer autant que nécessaire cet indicateur d'une version à une autre. Les indicateurs aident les utilisateurs et systèmes d'abonnement à identifier la version d'un modèle à utiliser.  
  
## <a name="workflow-for-version-management"></a>Flux de travail pour la gestion des versions  
 Utilisez le flux de travail suivant pour la gestion des versions :  
  
1.  Une version initiale est créée automatiquement lorsque vous créez un modèle et remplissez la base de données [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] avec les données de référence de votre société. Selon les autorisations, les utilisateurs peuvent apporter autant que nécessaire des modifications à cette version.  
  
2.  Lorsque vous voulez valider une version d'un modèle, verrouillez la version afin que seuls les administrateurs de modèle puissent mettre à jour les données. Pour plus d’informations, consultez [administrateurs &#40;Master Data Services&#41;](../master-data-services/administrators-master-data-services.md). Si les notifications sont configurées, une notification par e-mail est envoyée aux administrateurs de modèle à chaque modification de l’état de la version. Pour plus d’informations, consultez [Configurer des notifications par e-mail &#40;Master Data Services&#41;](../master-data-services/configure-email-notifications-master-data-services.md).  
  
3.  Appliquez les règles d'entreprise aux données de la version verrouillée et examinez tous les problèmes de validation. Si nécessaire, vous pouvez renseigner les informations manquantes ou rétablir la transaction à l'origine du problème. Vous pouvez également déverrouiller la version pour que les utilisateurs apportent des modifications.  
  
4.  Lorsque toutes les données ont passé la validation, validez la version et affectez-lui un indicateur pour qu'elle soit utilisée par des systèmes d'abonnement. Aucune modification ne peut être apportée à une version validée.  
  
5.  Copiez la version activée et indiquez aux utilisateurs qu'ils peuvent commencer à travailler dans une nouvelle version du modèle.  
  
## <a name="sequential-or-simultaneous-versions"></a>Versions séquentielles ou simultanées  
 Vous pouvez créer des versions séquentielles ou simultanées de votre modèle.  
  
-   **Versions séquentielles.** Chaque fois que vous activez une version, créez une copie et attribuez à la version le numéro séquentiel suivant. Par exemple, vous pouvez copier la **Version 7** de votre modèle et nommer la copie **Version 8**.  
  
-   **Versions simultanées.** Créez des versions simultanées de votre modèle lorsque vous souhaitez travailler sur plusieurs versions de vos données à la fois. Cela est utile lorsque votre société a des réorganisations ou fusions qui coïncident avec son activité habituelle et que vous souhaitez déterminer la façon dont les nouvelles données de référence peuvent s'ajuster à vos structures existantes.  
  
    > [!NOTE]  
    >  Un paramètre dans le [!INCLUDE[ssMDScfgmgr](../includes/ssmdscfgmgr-md.md)] détermine si vous pouvez ou non copier toutes les versions ou uniquement celles qui sont activées. Pour créer des versions simultanées vous devez configurer [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] pour vous permettre de copier toutes les versions. Ce paramètre est également disponible dans la table Paramètres système. Pour plus d’informations, consultez [Paramètres système &#40;Master Data Services&#41;](../master-data-services/system-settings-master-data-services.md).  
  
## <a name="related-tasks"></a>Tâches associées  
  
|Description de la tâche|Rubrique|  
|----------------------|-----------|  
|Modifier le nom d'une version existante.|[Modifier le nom d’une version &#40;Master Data Services&#41;](../master-data-services/change-a-version-name-master-data-services.md)|  
|Verrouiller une version pour que seuls les administrateurs puissent en modifier les données.|[Verrouiller une version &#40;Master Data Services&#41;](../master-data-services/lock-a-version-master-data-services.md)|  
|Déverrouiller une version afin que les utilisateurs puissent en modifier les données.|[Déverrouiller une version &#40;Master Data Services&#41;](../master-data-services/unlock-a-version-master-data-services.md)|  
|Activer une version une fois que toutes les données ont été validées.|[Activer une version &#40;Master Data Services&#41;](../master-data-services/commit-a-version-master-data-services.md)|  
|Créer un indicateur pour marquer une version.|[Créer un indicateur de version &#40;Master Data Services&#41;](../master-data-services/create-a-version-flag-master-data-services.md)|  
|Modifier le nom d'un indicateur de version existant.|[Modifier le nom d’un indicateur de version &#40;Master Data Services&#41;](../master-data-services/change-a-version-flag-name-master-data-services.md)|  
|Affecter un indicateur existant à une version.|[Affecter un indicateur à une version &#40;Master Data Services&#41;](../master-data-services/assign-a-flag-to-a-version-master-data-services.md)|  
|Créer une copie d'une version existante|[Copier une version &#40;Master Data Services&#41;](../master-data-services/copy-a-version-master-data-services.md)|  
|Supprimer une version existante.|[Supprimer une version &#40;Master Data Services&#41;](../master-data-services/delete-a-version-master-data-services.md)|  
|Vider les membres supprimés (récupérables) d’une version|[Purger les membres de version &#40;Master Data Services&#41;](../master-data-services/purge-version-members-master-data-services.md)|  
  
## <a name="related-content"></a>Contenu associé  
  
-   [Inverser une transaction &#40;Master Data Services&#41;](../master-data-services/reverse-a-transaction-master-data-services.md)  
  
-   [Notifications &#40;Master Data Services&#41;](../master-data-services/notifications-master-data-services.md)  
  
-   [Règles d’entreprise &#40;Master Data Services&#41;](../master-data-services/business-rules-master-data-services.md)  
  
  
