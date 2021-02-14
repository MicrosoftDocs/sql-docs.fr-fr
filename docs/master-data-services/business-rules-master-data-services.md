---
title: Règles d'entreprise
description: En savoir plus sur les règles d’entreprise dans Master Data Services, qui peuvent mettre à jour des données, envoyer des messages électroniques ou démarrer un processus ou un flux de travail d’entreprise.
ms.custom: ''
ms.date: 03/18/2017
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
helpviewer_keywords:
- business rules [Master Data Services], about business rules
- business rules [Master Data Services]
ms.assetid: a9f9e41a-2461-4845-b947-58b3a205543f
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: 4869875867f7b1cad99780b1ec82664c9d569201
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100341487"
---
# <a name="business-rules-master-data-services"></a>Règles d'entreprise (Master Data Services)

[!INCLUDE [SQL Server - Windows only ASDBMI  ](../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  Dans [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)], une règle d'entreprise est une règle qui vous permet de vérifier la qualité et l'exactitude de vos données de référence. Vous pouvez utiliser une règle d'entreprise pour mettre à jour automatiquement vos données, envoyer un message électronique ou démarrer un processus ou un flux de travail d'entreprise.  
  
 Pour afficher des exemples de règles d’entreprise, consultez [Exemples de règles d’entreprise &#40;Master Data Services&#41;](../master-data-services/business-rule-examples-master-data-services.md).  
  
## <a name="create-and-publish-business-rules"></a>Créer et publier des règles d'entreprise  
 Les règles d'entreprise sont des instructions **If/Then/Else** que vous créez dans [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)]. Si une valeur d’attribut répond à une condition spécifiée, une action est alors entreprise. Dans le cas contraire, une action Else est effectuée. Les actions possibles incluent la définition d'une valeur par défaut ou la modification d'une valeur. Ces actions peuvent être associées à l'envoi d'une notification par courrier électronique.  
  
 Les règles d'entreprise peuvent être basées sur des valeurs d'attribut spécifiques (par exemple, effectuer une action si Color=Blue), ou lorsque les valeurs d'attribut changent (par exemple, effectuer une action si la valeur de l'attribut Color change). Pour plus d’informations sur le suivi des modifications non spécifiques, consultez [Suivi des modifications &#40;Master Data Services&#41;](../master-data-services/change-tracking-master-data-services.md).  
  
 Pour utiliser des règles d'entreprise vous devez d'abord les créer et les publier, puis appliquer les règles publiées aux données. Vous pouvez appliquer des règles à des sous-ensembles de données ou à toutes les données d'une version en validant la version. Une version ne peut pas être validée tant que tous les attributs n'ont pas passé la validation de la règle d'entreprise.  
  
 Si un utilisateur tente d’ajouter une valeur d’attribut qui n’a pas passé la validation de la règle d’entreprise, la valeur peut toujours être enregistrée. Vous pouvez examiner et corriger les problèmes de validation qui s'affichent dans [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)].  
  
 Lorsque vous créez un package de déploiement de modèle, si vous souhaitez inclure des règles d'entreprise, vous devez inclure les données de la version dans le package.  
  
 Si vous créez une règle d'entreprise qui utilise l'opérateur **OR** , vous devez créer une règle distincte pour chaque instruction conditionnelle qui peut être évaluée indépendamment. Vous pouvez alors exclure des règles si nécessaire, ce qui offre plus de souplesse et facilite la résolution des problèmes.  
  
## <a name="how-business-rules-are-applied"></a>Comment appliquer les règles d'entreprise  
 Vous pouvez définir un ordre de priorité pour l’exécution des règles d’entreprise en déplaçant ces dernières vers le haut ou vers le bas. Toutefois, avant que la priorité soit prise en compte, les règles d'entreprise sont appliquées selon le type d'action qu'elles exécutent. L'ordre est le suivant :  
  
1.  **Valeur par défaut**  
  
2.  **Modifier la valeur**  
  
3.  **Validation**  
  
4.  **Action externe**  
  
5.  **Script d’action défini par l’utilisateur**  
  
 Dans ces groupes, les actions sont appliquées selon un ordre de priorité croissant. Par exemple, quatre règles distinctes peuvent comporter des actions ayant une **Valeur par défaut** . L'action avec la **Valeur par défaut** qui se produit en premier est déterminée par l'ordre de priorité spécifié dans l'interface utilisateur Web.  
  
 Voici d'autres remarques importantes concernant l'application des règles :  
  
-   Si une règle d'entreprise est exclue ou n'est pas publiée avec un état **Actif**, elle reste disponible mais n'est pas prise en compte lorsque les règles d'entreprise sont appliquées.  
  
-   Les règles d'entreprise s'appliquent aux valeurs d'attribut de tous les membres feuille ou de tous les membres consolidés, mais pas les deux à la fois.  
  
-   Les règles d'entreprise peuvent être appliquées sur n'importe quelle version d'un modèle **Ouvert** ou **Verrouillé**.  
  
-   Les modifications apportées aux données lorsque les règles d'entreprise sont appliquées ne sont pas enregistrées en tant que transactions.  
  
-   Une règle d'entreprise ne peut pas contenir plus d'une action **démarrer le flux de travail** .  
  
## <a name="system-settings"></a>Paramètres système  
 Il existe deux paramètres dans [!INCLUDE[ssMDScfgmgr](../includes/ssmdscfgmgr-md.md)] qui affectent les règles d'entreprise. Vous pouvez ajuster ces paramètres dans le [!INCLUDE[ssMDScfgmgr](../includes/ssmdscfgmgr-md.md)] ou directement dans la table Paramètres système. Pour plus d’informations, consultez [Paramètres système &#40;Master Data Services&#41;](../master-data-services/system-settings-master-data-services.md).  
  
## <a name="related-tasks"></a>Tâches associées  
  
|Description de la tâche|Rubrique|  
|----------------------|-----------|  
|Créer et publier une règle d'entreprise.|[Créer et publier une règle d’entreprise &#40;Master Data Services&#41;](../master-data-services/create-and-publish-a-business-rule-master-data-services.md)|  
|Ajouter plusieurs conditions à une règle d'entreprise.|[Ajouter plusieurs conditions à une règle d’entreprise &#40;Master Data Services&#41;](../master-data-services/add-multiple-conditions-to-a-business-rule-master-data-services.md)|  
|Créer une règle d'entreprise pour exiger que des valeurs soient affectées aux attributs.|[Requérir des valeurs d’attribut &#40;Master Data Services&#41;](../master-data-services/require-attribute-values-master-data-services.md)|  
|Créer une règle d'entreprise pour entreprendre une action en fonction des modifications apportées aux valeurs d'attribut.|[Initier des actions en fonction de modifications de valeurs d’attribut &#40;Master Data Services&#41;](../master-data-services/initiate-actions-based-on-attribute-value-changes-master-data-services.md)|  
|Créer une règle d’entreprise pour traiter un script défini par l’utilisateur comme une condition|[Extension de règles d’entreprise &#40;Master Data Services&#41;](../master-data-services/business-rules-extension-master-data-services.md)|  
|Créer une règle d’entreprise pour traiter un script défini par l’utilisateur comme une action|[Extension de règles d’entreprise &#40;Master Data Services&#41;](../master-data-services/business-rules-extension-master-data-services.md)|  
|Modifier le nom d'une règle d'entreprise existante.|[Renommer une règle d’entreprise &#40;Master Data Services&#41;](../master-data-services/change-a-business-rule-name-master-data-services.md)|  
|Configurer [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)] pour envoyer des notifications lorsque les règles d'entreprise sont appliquées.|[Configurer des règles d’entreprise pour envoyer des notifications &#40;Master Data Services&#41;](../master-data-services/configure-business-rules-to-send-notifications-master-data-services.md)|  
|Appliquer des règles d'entreprise à des membres spécifiques.|[Valider des membres spécifiques par rapport aux règles d’entreprise &#40;Master Data Services&#41;](../master-data-services/validate-specific-members-against-business-rules-master-data-services.md)|  
|Exclure une règle d'entreprise de façon à ne pas l'utiliser.|[Exclure une règle d’entreprise &#40;Master Data Services&#41;](../master-data-services/exclude-a-business-rule-master-data-services.md)|  
|Supprimer une règle d'entreprise existante.|[Supprimer une règle d’entreprise &#40;Master Data Services&#41;](../master-data-services/delete-a-business-rule-master-data-services.md)|  
  
## <a name="related-content"></a>Contenu associé  
  
-   [Vue d’ensemble de Master Data Services &#40;MDS&#41;](../master-data-services/master-data-services-overview-mds.md)  
  
-   [Versions &#40;Master Data Services&#41;](../master-data-services/versions-master-data-services.md)  
  
-   [Validation &#40;Master Data Services&#41;](../master-data-services/validation-master-data-services.md)  
  
-   [Suivi des modifications &#40;Master Data Services&#41;](../master-data-services/change-tracking-master-data-services.md)  
  
  
