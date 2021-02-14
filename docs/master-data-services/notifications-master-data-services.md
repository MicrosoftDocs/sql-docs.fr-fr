---
description: Notifications (Master Data Services)
title: Notifications
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
helpviewer_keywords:
- notifications [Master Data Services]
- notifications [Master Data Services], about notifications
- e-mail [Master Data Services]
- e-mail [Master Data Services], about e-mail notifications
ms.assetid: d7ad32d5-9fe5-48fd-8c61-0b00c0aff082
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: 60bc5e525d858413ddd3637f5977b9a61fd7789c
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100340293"
---
# <a name="notifications-master-data-services"></a>Notifications (Master Data Services)

[!INCLUDE [SQL Server - Windows only ASDBMI  ](../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] peut être configuré pour envoyer une notification par courrier électronique lors d’un échec de validation de règle d’entreprise, d’une modification de l’état d’une version de modèle ou d’une modification de l’état d’un ensemble de modifications.  
  
## <a name="how-notifications-are-sent"></a>Mode d'envoi des notifications  
 Vous pouvez configurer les notifications dans [!INCLUDE[ssMDScfgmgr](../includes/ssmdscfgmgr-md.md)]. Les notifications envoient des messages électroniques à l’aide de Database Mail sur l’instance de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] [!INCLUDE[ssDE](../includes/ssde-md.md)] qui héberge la [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] base de données. Pour plus d’informations sur la messagerie de base de données, consultez [Objets de configuration de la messagerie de base de données](../relational-databases/database-mail/database-mail-configuration-objects.md) dans la documentation en ligne de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] .  
  
## <a name="when-notifications-are-sent"></a>Critères d'envoi des notifications  
 Une fois les notifications configurées, elles peuvent être envoyées automatiquement par courrier électronique dans les cas suivants.  
  
|Instance|Description|  
|--------------|-----------------|  
|Les données ne remplissent pas les conditions de validation des règles d'entreprise.|Les règles d'entreprise doivent être configurées individuellement pour envoyer des messages électroniques lorsqu'une valeur d'attribut fait échouer la validation de la règle d'entreprise. La notification contient les informations ci-après.<br /><br /> Modèle<br /><br /> Version<br /><br /> Entité<br /><br /> Code de membre<br /><br /> Règle d’entreprise ayant échoué<br /><br /> Lien vers le membre dont la valeur d’attribut fait échouer la règle d’entreprise<br /><br /> Heure d’émission de la notification<br /><br /> Pour plus d’informations, consultez [Configurer des règles d’entreprise pour envoyer des notifications &#40;Master Data Services&#41;](../master-data-services/configure-business-rules-to-send-notifications-master-data-services.md).|  
|L'état d'une version de modèle change|Chaque fois que l'état d'une version de modèle change, les utilisateurs qui sont des administrateurs de modèle reçoivent automatiquement des notifications. La notification contient les informations ci-après.<br /><br /> Modèle<br /><br /> Version<br /><br /> État précédent et nouvel état de la version<br /><br /> Heure d’émission de la notification<br /><br /> Pour plus d’informations, consultez [administrateurs &#40;Master Data Services&#41;](../master-data-services/administrators-master-data-services.md).|  
|Modifications d’état de l’ensemble de modifications|Chaque fois que l’état d’un ensemble de modifications change pour une entité qui nécessite une approbation, les administrateurs d’entité et/ou les propriétaires d’ensemble de modifications reçoivent automatiquement des notifications. La notification contient les informations ci-après.<br /><br /> Modèle<br /><br /> Version<br /><br /> Nom de l’ensemble de modifications<br /><br /> État précédent<br /><br /> Nouvel état<br /><br /> Lien pour appliquer l’ensemble de modifications afin d’afficher et de modifier les modifications en attente.<br /><br /> Pour plus d’informations, consultez [Ensembles de modifications &#40;Master Data Services&#41;](../master-data-services/changesets-master-data-services.md)|  
  
## <a name="system-settings"></a>Paramètres système  
 Il existe des paramètres dans le [!INCLUDE[ssMDScfgmgr](../includes/ssmdscfgmgr-md.md)] qui affectent les notifications. Vous pouvez ajuster ces paramètres dans [!INCLUDE[ssMDScfgmgr](../includes/ssmdscfgmgr-md.md)] ou directement dans la table Paramètres système dans la base de données [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] . Pour plus d’informations, consultez [Paramètres système &#40;Master Data Services&#41;](../master-data-services/system-settings-master-data-services.md).  
  
## <a name="related-tasks"></a>Tâches associées  
  
|Description de la tâche|Rubrique|  
|----------------------|-----------|  
|Configurer [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] pour envoyer des notifications par courrier électronique.|[Configurer des notifications par courrier électronique &#40;Master Data Services&#41;](../master-data-services/configure-email-notifications-master-data-services.md)|  
|Configurer [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)] pour envoyer des notifications lorsque les valeurs d'attribut changent.|[Configurer des règles d’entreprise pour envoyer des notifications &#40;Master Data Services&#41;](../master-data-services/configure-business-rules-to-send-notifications-master-data-services.md)|  
  
## <a name="related-content"></a>Contenu associé  
  
-   [Règles d’entreprise &#40;Master Data Services&#41;](../master-data-services/business-rules-master-data-services.md)  
  
-   [Versions &#40;Master Data Services&#41;](../master-data-services/versions-master-data-services.md)  
  
-   [Résolution des problèmes liés aux notifications par courrier électronique (Master Data Services)](https://social.technet.microsoft.com/wiki/contents/articles/troubleshooting-email-notifications-master-data-services.aspx)  
  
  
