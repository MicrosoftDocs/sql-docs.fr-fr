---
description: Documentation du développeur Master Data Services
title: Documentation du développeur
ms.custom: ''
ms.date: 03/17/2017
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: reference
ms.assetid: 067b1f69-84eb-4a13-b220-120cd63704b4
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: 4062f365552fbbb2076c68f5bf6d3d912b81a490
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100350105"
---
# <a name="master-data-services-developer-documentation"></a>Documentation du développeur Master Data Services

[!INCLUDE [SQL Server Windows Only - ASDBMI ](../../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  Recherchez des informations sur l'écriture du code pour personnaliser la façon dont vous et les utilisateurs interagissez avec [!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)]. Découvrez comment :  
  
-   Ecrire un programme qui accède au service Web [!INCLUDE[ssMDSmdm](../../includes/ssmdsmdm-md.md)] . Le service Web [!INCLUDE[ssMDSmdm](../../includes/ssmdsmdm-md.md)] est un service Windows Communication Foundation (WCF) utilisé par les développeurs pour contrôler les fonctionnalités de [!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)] via du code.  
  
-   Incorporer les fonctionnalités [!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)] dans des applications existantes.  
  
-   Entrer du code pour traiter des actions répétitives ou complexes qu'il est impossible ou difficile d'effectuer avec l'interface utilisateur [!INCLUDE[ssMDSmdm](../../includes/ssmdsmdm-md.md)] .  
  
-   Créer un flux de travail personnalisé qui s'exécute en réponse à une règle d'entreprise que vous spécifiez. Un flux de travail personnalisé appelle du code que vous écrivez et qui peut exécuter n'importe quelle action requise pour traiter le flux de travail.  
  
## <a name="master-data-manager-web-service"></a>Service Web Master Data Manager  
 Le service Web [!INCLUDE[ssMDSmdm](../../includes/ssmdsmdm-md.md)] vous permet de programmer les fonctionnalités de [!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)] à partir de n'importe quel ordinateur pouvant accéder à votre site Web [!INCLUDE[ssMDSmdm](../../includes/ssmdsmdm-md.md)] . Avant de démarrer l'écriture du code pour accéder au service Web, vous devez générer les classes proxy, contenues dans un espace de noms que vous spécifiez. Cette documentation utilise <xref:Microsoft.MasterDataServices> comme espace de noms de proxy. La classe proxy principale que vous utilisez pour effectuer des opérations de service Web est la classe <xref:Microsoft.MasterDataServices.ServiceClient>, qui implémente l'interface <xref:Microsoft.MasterDataServices.IService>. Depuis votre code, appelez les méthodes de la classe <xref:Microsoft.MasterDataServices.ServiceClient> pour accéder au service Web [!INCLUDE[ssMDSmdm](../../includes/ssmdsmdm-md.md)]. Le reste des classes dans l'espace de noms sont utilisées par les opérations de service Web.  
  
### <a name="web-service-content"></a>Contenu du service Web  
 [Créer des classes proxy de service Web Master Data Manager](../../master-data-services/develop/create-master-data-manager-web-service-proxy-classes.md)  
 Décrit comment activer la publication des métadonnées à partir du site Web [!INCLUDE[ssMDSmdm](../../includes/ssmdsmdm-md.md)] et comment créer les classes proxy qui peuvent être utilisées pour accéder par programmation aux opérations du service Web.  
  
 [Opérations de service web par catégorie &#40;Master Data Services&#41;](../../master-data-services/develop/categorized-web-service-operations-master-data-services.md)  
 Liste par catégorie des opérations de service Web de la classe <xref:Microsoft.MasterDataServices.ServiceClient>.  
  
## <a name="custom-workflows"></a>Flux de travail personnalisés  
 [!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)] utilise des règles d'entreprise pour créer des solutions de flux de travail de base. Vous pouvez automatiquement mettre à jour et valider les données et recevoir des notifications par courrier électronique en fonction des conditions que vous spécifiez. Les règles d'entreprise dans [!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)] sont prévues pour gérer les scénarios de flux de travail les plus courants. Si votre flux de travail nécessite le traitement d'événements plus complexes, tels que les approbations multicouche ou les arbres décisionnels complexes, vous pouvez configurer [!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)] pour envoyer des données à un assembly personnalisé que vous créez. Pour gérer les flux de travail personnalisés, vous devez configurer et démarrer SQL Server Service d’intégration de flux de travail MDS sur l’ordinateur de l’application Web, et créer un assembly qui implémente l’interface [MasterDataServices. WorkflowTypeExtender. IWorkflowTypeExtender](/previous-versions/sql/sql-server-2016/hh758785(v=sql.130)) .  
  
### <a name="custom-workflow-content"></a>Contenu personnalisé de flux de travail  
 [Créer un flux de travail personnalisé &#40;Master Data Services&#41;](../../master-data-services/develop/create-a-custom-workflow-master-data-services.md)  
 Instructions sur la création d'un assembly de gestionnaire de flux de travail, sur la configuration et le démarrage du service d'intégration de flux de travail MDS SQL Server et sur la création d'une règle d'entreprise dans [!INCLUDE[ssMDSmdm](../../includes/ssmdsmdm-md.md)] qui démarre un flux de travail personnalisé.  
  
## <a name="web-server-namespaces"></a>Espaces de noms de serveur Web  
 [!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)] installe un ensemble d'assemblys sur le serveur Web. Ces assemblys contiennent des espaces de noms qui peuvent être utilisés pour des scénarios avancés qui personnalisent le comportement du serveur Web. Le tableau suivant décrit ces espaces de noms.  
  
|Espace de noms|Description|  
|---------------|-----------------|  
|[Microsoft. MasterDataServices. Deployment](/previous-versions/sql/sql-server-2016/ff487448(v=sql.130))|Contient les classes qui peuvent être utilisées pour créer un package de déploiement de modèle et pour déployer un package dans une base de données [!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)] .|  
|<xref:Microsoft.MasterDataServices.Services>|Contient une classe qui reçoit et traite des opérations de service Web effectuées sur le serveur Web par l'application Web de [!INCLUDE[ssMDSmdm](../../includes/ssmdsmdm-md.md)] .|  
|<xref:Microsoft.MasterDataServices.Services.DataContracts>|Contient les classes qui définissent la façon dont les données sont transmises à partir de l'ordinateur client par l'application Web de [!INCLUDE[ssMDSmdm](../../includes/ssmdsmdm-md.md)] au serveur Web.|  
|<xref:Microsoft.MasterDataServices.Services.MessageContracts>|Contient les classes qui définissent la façon dont les demandes et les réponses sont transmises à partir de l'ordinateur client par l'application Web de [!INCLUDE[ssMDSmdm](../../includes/ssmdsmdm-md.md)] au serveur Web.|  
|<xref:Microsoft.MasterDataServices.Services.ServiceContracts>|Contient l'interface qui définit les opérations qui peuvent être appelées par le service Web [!INCLUDE[ssMDSmdm](../../includes/ssmdsmdm-md.md)] .|  
  
  
