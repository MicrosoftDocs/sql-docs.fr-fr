---
description: Ajouter un rapport personnalisé à Management Studio
title: Ajouter un rapport personnalisé à Management Studio
ms.custom: seo-lt-2019
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- SQL Server Management Studio [SQL Server], custom reports
ms.assetid: 3cf8d726-0a90-4f80-98d0-352a2a59be0f
author: markingmyname
ms.author: maghan
ms.openlocfilehash: aa920b620cfa0045228fcdb5675c1e88bc7ab035
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/17/2020
ms.locfileid: "88480159"
---
# <a name="add-a-custom-report-to-management-studio"></a>Ajouter un rapport personnalisé à Management Studio
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
Cette rubrique explique comment créer un simple rapport [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] enregistré comme fichier .rdl, puis ajouter ce fichier à [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] en tant que rapport personnalisé. [!INCLUDE[ssRS](../../includes/ssrs.md)] permet de créer un large choix de rapports élaborés. Pour que vous puissiez créer un rapport en suivant cette rubrique, [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull_md.md)] doit être installé sur votre ordinateur. Vous n'avez pas besoin d'installer [!INCLUDE[ssRS](../../includes/ssrs.md)] sur [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pour exécuter un rapport personnalisé à l'aide de [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)].  
  
 
### <a name="to-create-a-simple-report-saved-as-an-rdl-file"></a>Pour créer un rapport simple enregistré en tant que fichier .rdl  
  
1.  Cliquez sur **Démarrer**, pointez sur **Programmes**, puis sur **Microsoft SQL Server**, puis cliquez sur **Outils de données SQL Server**.  
  
2.  Dans le menu **Fichier** , pointez sur **Nouveau**, puis cliquez sur **Projet**.  
  
3.  Dans la liste **Types de projets** , cliquez sur **Projets Business Intelligence**.  
  
4.  Dans la liste **Modèles** , cliquez sur **Assistant Projet Report Server**.  
  
5.  Dans la zone **Nom**, tapez **ConnectionsReport**, puis cliquez sur **OK**.  
  
6.  Dans la page **Assistant Rapport** , cliquez sur **Suivant**.  
  
7.  Dans la page **Sélectionner la source de données** , dans la zone Nom, entrez un nom de connexion à votre [!INCLUDE[ssDE](../../includes/ssde_md.md)], puis cliquez sur **Modifier**.  
  
8.  Dans la boîte de dialogue **Propriétés de connexion** , dans la zone de texte **Nom du serveur** , tapez le nom de votre instance de [!INCLUDE[ssDE](../../includes/ssde_md.md)].  
  
9. Dans la zone **Sélectionner ou entrer un nom de base de données** , tapez le nom de l’une de vos bases de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], comme [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)], puis cliquez sur **OK**.  
  
10. Dans la page **Sélectionner la source de données** , cliquez sur **Suivant**.  
  
11. Dans la page **Concevoir la requête** , dans la zone **Chaîne de requête** , tapez l’instruction [!INCLUDE[tsql](../../includes/tsql-md.md)] suivante qui répertorie les connexions en cours à votre [!INCLUDE[ssDE](../../includes/ssde_md.md)], puis cliquez sur **Suivant**. La zone Chaîne de requête de l'Assistant Rapport n'accepte pas les paramètres de rapport. Les rapports personnalisés plus complexes doivent être créés manuellement.  
  
    **SELECT session_id, net_transport FROM sys.dm_exec_connections;**  
  
12. Dans la page **Sélectionner le type de rapport** , sélectionnez **Tabulaire**, puis cliquez sur **Terminer**.  
  
13. Dans la page **Fin de l’Assistant** , dans la zone **Nom du rapport** , tapez **ConnectionsReport**, puis cliquez sur **Terminer** pour créer et enregistrer le rapport.  
  
14. Fermez [!INCLUDE[ssBIDevStudio](../../includes/ssbidevstudio-md.md)].  
  
15. Copiez **ConnectionsReport.rdl** dans un dossier que vous avez créé sur votre serveur de bases de données pour les rapports personnalisés.  
  
### <a name="to-add-a-report-to-management-studio"></a>Pour ajouter un rapport à Management Studio  
  
-   Dans [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)], cliquez avec le bouton droit sur un nœud dans l’Explorateur d’objets, pointez sur **Rapports**, puis cliquez sur **Rapports personnalisés**. Dans la boîte de dialogue **Ouvrir un fichier** , recherchez le dossier des rapports personnalisés, sélectionnez le fichier **ConnectionsReport.rdl** , puis cliquez sur **Ouvrir**.  
  
    Quand un nouveau rapport personnalisé est ouvert pour la première fois à partir d’un nœud de l’Explorateur d’objets, il vient s’ajouter à la liste des derniers fichiers utilisés sous **Rapports personnalisés** dans le menu contextuel de ce nœud. Un rapport standard s’affiche aussi dans la liste des derniers rapports utilisés sous **Rapports personnalisés**quand vous l’ouvrez pour la première fois. En cas de suppression d'un fichier de rapport personnalisé, lorsque l'élément est à nouveau sélectionné, une invite apparaît et propose de supprimer l'élément de la liste des derniers fichiers utilisés.  
  
    1.  Pour modifier le nombre de fichiers dévoilés dans la liste des derniers fichiers utilisés, cliquez sur **Options** dans le menu **Outils** , développez le dossier **Environnement** , puis cliquez sur **Général**.  
  
    2.  Ajustez le nombre de fichiers dans la **liste des derniers fichiers utilisés**.  
  
## <a name="see-also"></a>Voir aussi  
[Rapports personnalisés dans Management Studio](../../ssms/object/custom-reports-in-management-studio.md)  
[Utiliser des rapports personnalisés avec les propriétés de nœud de l’Explorateur d’objets](../../ssms/object/use-custom-reports-with-object-explorer-node-properties.md)  
[Annuler la suppression des avertissements d'exécution de rapports personnalisés](../../ssms/object/unsuppress-run-custom-report-warnings.md)  
[SQL Server Reporting Services](../../reporting-services/create-deploy-and-manage-mobile-and-paginated-reports.md)  
  
