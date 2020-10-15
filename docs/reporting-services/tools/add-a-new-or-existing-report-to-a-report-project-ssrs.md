---
title: Ajouter un nouveau rapport ou un rapport existant à un projet de rapport | Microsoft Docs
description: Découvrez comment ajouter un nouveau rapport ou un rapport existant à un projet de rapport à l’aide de l’Assistant Rapport dans SQL Server Reporting Services.
ms.date: 03/17/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: tools
ms.topic: conceptual
helpviewer_keywords:
- reports [Reporting Services], creating
ms.assetid: 8bc0bb53-ad8a-464d-bb6a-7fea5fa62c5c
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 07d1f6d9f31fa300b2e9e43e80eda9c03111cc6c
ms.sourcegitcommit: a41e1f4199785a2b8019a419a1f3dcdc15571044
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/13/2020
ms.locfileid: "91986505"
---
# <a name="add-a-new-or-existing-report-to-a-report-project-ssrs"></a>Ajouter un nouveau rapport ou un rapport existant à un projet de rapport (SSRS)
  Dans [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)], vous pouvez ajouter un nouveau rapport paginé [!INCLUDE[ssRSnoversion_md](../../includes/ssrsnoversion-md.md)] en utilisant l’Assistant Rapport ou en ajoutant un nouveau rapport vide à votre projet. Vous pouvez aussi ajouter un rapport existant. Après avoir ajouté un rapport, vous pouvez voir son nom sous le dossier **Rapports** de votre projet.  
  
> [!NOTE]  
>  Pour afficher l'aperçu d'un rapport avec les sources de données existantes, vous devez disposer d'autorisations sur la source de données à partir du client de création de votre rapport. Pour plus d’informations, consultez [Connexions de données, sources de données et chaînes de connexion](../../reporting-services/report-data/data-connections-data-sources-and-connection-strings-report-builder-and-ssrs.md).  
  
 Après avoir ajouté un rapport, vous pouvez définir des sources de données et des datasets, ainsi que concevoir la mise en page du rapport. Pour commencer, consultez [Créer un rapport de table de base &#40;didacticiel SSRS&#41;](../../reporting-services/create-a-basic-table-report-ssrs-tutorial.md) ou [Tables &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/tables-report-builder-and-ssrs.md).  
  
## <a name="to-add-a-new-report-using-the-report-wizard"></a>Pour ajouter un nouveau rapport en utilisant l'Assistant Rapport  
  
1.  Dans l’Explorateur de solutions, cliquez avec le bouton droit sur le dossier Rapports, puis sélectionnez **Ajouter un nouveau rapport**. La boîte de dialogue **Assistant Rapport** s’ouvre.  
  
     Les étapes de l’Assistant vous guident dans le processus de création d’une source de données, de création d’un dataset avec une requête, de définition de groupes, de spécification d’une mise en page et de création du rapport. Procédez comme suit :  
  
    -   **Sélectionnez une source de données.** La première étape de création d'un rapport consiste à définir une source de données. L'Assistant Rapport fournit la liste de toutes les sources de données partagées du projet de rapport et offre la possibilité de créer une nouvelle source de données.  
  
    -   **Créez une requête.** L'étape suivante consiste à créer une requête. Vous pouvez taper la chaîne de requête, la générer à l'aide du concepteur de requêtes ou importer une requête à partir d'un autre rapport. Pour plus d’informations sur les concepteurs de requêtes, consultez [Concepteurs de requêtes Reporting Services](/previous-versions/sql/).  
  
    -   **Choisissez un type de rapport.** L'étape suivante consiste à sélectionner le type de rapport souhaité. Vous pouvez choisir un rapport tabulaire ou de matrice. Un rapport tabulaire comporte un nombre fixe de colonnes. Un rapport de matrice (ou tableau croisé dynamique) comporte un nombre variable de colonnes qui dépend des résultats de la requête. Un rapport cartographique affiche des informations analytiques sur un arrière-plan géographique.  
  
    -   **Nommez le rapport.**  La dernière étape consiste à donner un nom au rapport et à vérifier les champs qui y seront inclus. Lorsque la procédure est terminée, le Concepteur de rapports crée le rapport et l'ajoute au projet du serveur de rapports.  
  
## <a name="to-add-a-new-blank-report"></a>Pour ajouter un rapport vide  
  
1.  Dans le menu **Projet** , cliquez sur **Ajouter un nouvel élément**.  
  
2.  Dans **Modèles**, cliquez sur **Rapport**.  
  
3.  Cliquez sur **Add**.  
  
     Un nouveau rapport vide est ajouté au projet et affiché sur l'aire de conception.  
  
## <a name="to-add-an-existing-report"></a>Pour ajouter un rapport existant  
  
1.  Dans le menu **Projet** , cliquez sur **Ajouter**, puis sur  **Élément existant**.  
  
2.  Naviguez jusqu’à l’emplacement du fichier .rdl, sélectionnez-le, puis cliquez sur **Ajouter**.  
  
     Le rapport est ajouté au projet sous le dossier **Rapports** . Lorsque vous fermez et rouvrez le projet, les rapports sont triés alphabétiquement.  
  
## <a name="see-also"></a>Voir aussi  
 [Didacticiels sur Reporting Services &#40;SSRS&#41;](../../reporting-services/reporting-services-tutorials-ssrs.md)  
 D’autres questions ? [Essayez le forum Reporting Services](https://go.microsoft.com/fwlink/?LinkId=620231)
  
