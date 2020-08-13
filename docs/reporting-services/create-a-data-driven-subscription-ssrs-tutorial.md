---
title: Créer un abonnement piloté par les données (didacticiel SSRS) | Microsoft Docs
description: Apprenez-en davantage sur les abonnements pilotés par les données avec un exemple simple qui crée un abonnement piloté par les données pour générer et enregistrer une sortie de rapport filtrée dans un partage de fichiers.
ms.date: 05/26/2016
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: reporting-services
ms.topic: conceptual
helpviewer_keywords:
- subscriptions [Reporting Services], tutorials
- walkthroughs [Reporting Services]
- data-driven subscriptions
ms.assetid: 79ab0572-43e9-4dc4-9b5a-cd8b627b8274
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 2f4843c02d12c08cac8efa8b453999f4484ab084
ms.sourcegitcommit: 216f377451e53874718ae1645a2611cdb198808a
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/28/2020
ms.locfileid: "87246331"
---
# <a name="create-a-data-driven-subscription-ssrs-tutorial"></a>Créer un abonnement piloté par les données (didacticiel SSRS)
Ce tutoriel [!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)] vous enseigne les concepts des abonnements pilotés par les données en décrivant un exemple simple qui crée un abonnement piloté par les données pour générer et enregistrer une sortie de rapport filtrée dans un partage de fichiers. 
[!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)] Les abonnements pilotés par les données permettent de personnaliser et d’automatiser la distribution d’un rapport basé sur des données d’abonnés dynamiques. Les abonnements pilotés par les données s'utilisent dans les types de scénarios suivants :  
  
-   Distribution de rapports à un large ensemble de destinataires dont les membres peuvent changer d'une distribution à l'autre. Par exemple, envoi par e-mail d’un rapport mensuel à l’ensemble des clients actuels.  
  
-   Distribution de rapports à un groupe spécifique de destinataires sur la base de critères prédéfinis. Par exemple, envoi d’un rapport sur les résultats des ventes à tous les directeurs commerciaux d’une organisation.
+ Automatiser la génération de rapports dans une grande variété de formats, par exemple .xlsx et .pdf.  
  
## <a name="what-you-will-learn"></a>Contenu du didacticiel  
Ce didacticiel est divisé en trois leçons :  

| Leçon | Commentaires |
| ------ | -------- |
| [Leçon 1 : Créer un exemple de base de données d’abonnés](../reporting-services/lesson-1-creating-a-sample-subscriber-database.md) | Au cours de cette leçon, vous allez créer une base de données [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] locale qui contient des informations sur les abonnés. les informations Order Numbers à utiliser pour le filtrage et les formats des fichiers de sortie. |
| [Leçon 2 : Configurer les propriétés d’une source de données de rapport](../reporting-services/lesson-2-modifying-the-report-data-source-properties.md) | Dans cette leçon, vous allez configurer une source de données de rapport pour que le rapport puisse s’exécuter de manière planifiée et sans assistance. Les informations d'identification stockées sont nécessaires pour le traitement autonome. Vous allez également modifier le dataset du rapport afin d'inclure un paramètre fourni par les données d'abonné. Ce paramètre sert à filtrer les données de rapport en fonction du numéro de commande. |
| [Leçon 3 : Définir un abonnement piloté par les données](../reporting-services/lesson-3-defining-a-data-driven-subscription.md) | Dans cette leçon, vous allez créer un abonnement piloté par les données. Cette leçon vous guide à travers chaque page de l'Assistant Abonnement piloté par les données. |

Le diagramme suivant illustre le flux de travail de base du tutoriel :

| Étape    | Description |
| --------|------------ |
| (1)     | La configuration d’un abonnement note le rapport source, le calendrier et le mappage des champs avec la base de données d’abonnés. |
| (2)     | La table OrderInfo contient quatre numéros de commande à utiliser pour le filtrage, un par fichier. Elle contient également les formats de fichiers pour les rapports générés. |
| (3)     | Les informations de la base de données Adventureworks sont filtrées et retournées dans le rapport. |
| (4)     | Les rapports sont créés dans les formats de fichiers spécifiés dans la table Orderinfo. |



   ![ssrs_tutorial_datadriven_flow](../reporting-services/media/ssrs-tutorial-datadriven-flow.png) 
  
## <a name="requirements"></a>Spécifications  
Les abonnements pilotés par les données sont généralement créés par un administrateur de serveur de rapports, qui en assure également la mise à jour. Pour créer des abonnements pilotés par les données, vous devez créer des requêtes, connaître les sources de données qui contiennent les données d’abonnés et disposer d’autorisations élevées sur le serveur de rapports.  
  
Ce didacticiel utilise le rapport *Sales order* créé dans le didacticiel [Créer un rapport de tableau de base &#40;didacticiel SSRS&#41;](../reporting-services/create-a-basic-table-report-ssrs-tutorial.md) et les données de l’exemple de base de données **AdventureWorks2014**.  
  
Pour utiliser ce didacticiel, vous devez avoir installé les éléments suivants sur votre ordinateur :  
  
-   Une édition de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] qui prend en charge les abonnements pilotés par les données. Pour plus d’informations, consultez [Éditions et fonctionnalités de SQL Server 2017](../sql-server/editions-and-components-of-sql-server-2017.md).  
  
-   Le serveur de rapports doit être exécuté en mode natif. L'interface utilisateur décrite dans ce didacticiel est basée sur un serveur de rapports en mode natif. Les abonnements sont pris en charge sur les serveurs de rapports en mode SharePoint, mais l'interface utilisateur sera différente de ce qui est décrit dans ce didacticiel.  
  
-   Le service Agent SQL Server doit être en cours d'exécution.  
  
-   Rapport contenant des paramètres. Ce didacticiel suppose l’utilisation de l’exemple de rapport `Sales Orders` que vous créez à l’aide du didacticiel [Créer un rapport de tableau de base &#40;didacticiel SSRS&#41;](../reporting-services/create-a-basic-table-report-ssrs-tutorial.md).  
  
-   Exemple de base de données **AdventureWorks2014** , qui fournit des données à l’exemple de rapport.  
  
-   Attribution de rôle [!INCLUDE[ssRSnoversion_md](../includes/ssrsnoversion-md.md)] incluant la tâche Gérer tous les abonnements sur l’exemple de rapport. Cette tâche est obligatoire dans la définition des abonnements pilotés par les données. Si vous êtes l'administrateur de l'ordinateur, l'attribution de rôle par défaut pour les administrateurs locaux fournit les autorisations nécessaires à la création d'abonnements pilotés par les données. Pour plus d’informations, consultez [Octroi d'autorisations sur un serveur de rapports en mode natif](../reporting-services/security/granting-permissions-on-a-native-mode-report-server.md).  
  
-   Dossier partagé pour lequel vous bénéficiez de droits d'accès en écriture. Le dossier partagé doit être accessible via une connexion réseau.  
  
**Durée estimée pour effectuer le tutoriel :** 30 minutes. Trente minutes supplémentaires si vous n'avez pas étudié le didacticiel de création d'un rapport de base.  
  
## <a name="see-also"></a>Voir aussi  
[Abonnements pilotés par les données](../reporting-services/subscriptions/data-driven-subscriptions.md)  
[Créer un rapport de tableau de base &#40;Didacticiel SSRS&#41;](../reporting-services/create-a-basic-table-report-ssrs-tutorial.md)
 

