---
title: Développement de base de données hors connexion orienté projet
description: Affichez des ressources disponibles sur des tâches de développement de base de données hors ligne orientées projet, telle que l’importation d’objets dans une base de données et l’utilisation d’objets de séquence.
ms.prod: sql
ms.technology: ssdt
ms.topic: conceptual
f1_keywords:
- sql.data.tools.dbprojectwizard.general
- sql.data.tools.dbprojectwizard.summary
ms.assetid: e61e830d-9fcd-45e7-b7b4-93a42155dd56
author: markingmyname
ms.author: maghan
ms.reviewer: “”
ms.custom: seo-lt-2019
ms.date: 02/09/2017
ms.openlocfilehash: e36876d1b30b12dbae1dd4c7d3c72230070c9ef8
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100077053"
---
# <a name="project-oriented-offline-database-development"></a>Développement de base de données hors connexion orienté projet

Cette section décrit les fonctionnalités fournies par SQL Server Data Tools (SSDT) pour la création, la génération, le débogage et la publication d’un projet de base de données.  
  
À l'aide de SSDT, vous pouvez créer un projet de base de données en mode hors connexion et implémenter les modifications du schéma en ajoutant, modifiant ou supprimant les définitions des objets (représentées par des scripts) dans le projet, sans connexion à une instance de serveur. Toutes ces opérations peuvent être réalisées à l’aide du Concepteur de tables ou de l’Éditeur Transact\-SQL. Vous pouvez également écrire et déboguer des objets Transact\-SQL et CLR dans le même projet. Vous pouvez utilisez une comparaison de schémas pour vérifier que votre projet reste synchronisé avec la base de données de production, et créer des instantanés pour votre projet à chaque étape du cycle de développement à des fins de comparaison. Lorsque vous utilisez des projets de base de données dans un environnement de travail en équipe, vous pouvez employer le contrôle de version pour tous les fichiers. Après avoir développé, testé et débogué votre projet de base de données, vous pouvez le remettre au personnel autorisé en vue de le publier dans un environnement de production.  
  
> [!NOTE]  
> Les rubriques de procédure de cette section contiennent une série de tâches qui peuvent être effectuées dans l'ordre.  
  
## <a name="in-this-section"></a>Dans cette section  
  
|Rubrique|Description|  
|---------|---------------|  
|[Importer dans un projet de base de données](../ssdt/import-into-a-database-project.md)|Décrit l'importation d'objets à partir d'une base de données active, d'un fichier .dacpac ou d'un script.|  
|[Boîte de dialogue Ajouter une référence de la base de données](../ssdt/add-database-reference-dialog-box.md)|Décrit les différentes méthodes d'ajout d'une référence de base de données.|  
|[Boîte de dialogue Rechercher des mises à jour](../ssdt/check-for-updates-dialog-box.md)|Décrit la manière dont SQL Server Data Tools peut rechercher des mises à jour du produit.|  
|[Paramètres des projets de base de données](../ssdt/database-project-settings.md)|Décrit les paramètres de projet permettant de contrôler des aspects de vos configurations de build et de bases de données.|  
|[Procédure : parcourir les objets d’un projet de base de données SQL Server](../ssdt/how-to-browse-objects-in-a-sql-server-database-project.md)|L’Explorateur d’objets SQL Server dans Visual Studio contient à présent un nœud Projets dédié, sous lequel tous les projets de base de données SQL Server de la solution sont regroupés dans une hiérarchie de type SQL Server Management Studio.|  
|[Fenêtre Opérations de Data Tools](../ssdt/data-tools-operations-window.md)|Décrit la fenêtre **Opérations des outils de données** , qui affiche la progression de certaines opérations et vous avertit en cas d'erreurs.|  
|[Options de l’Éditeur Transact-SQL](../ssdt/transact-sql-editor-options.md)|Décrit les options de Transact\-SQL.|  
|[Procédure : créer un nouveau projet de base de données](../ssdt/how-to-create-a-new-database-project.md)|Créez un projet de base de données et importez un schéma de base de données existant.|  
|[Procédure : utiliser le schéma pour comparer différentes définitions de base de données](../ssdt/how-to-use-schema-compare-to-compare-different-database-definitions.md)|Comparez les schémas d'une base de données et d'un projet et synchronisez.|  
|[Procédure : générer et déployer dans une base de données locale](../ssdt/how-to-build-and-deploy-to-a-local-database.md)|Utilisez l’instance SQL Server locale à la demande, qui est activée lors du débogage d’un projet de base de données.|  
|[Procédure : modifier la plateforme cible et publier un projet de base de données](../ssdt/how-to-change-target-platform-and-publish-a-database-project.md)|Modifiez la plateforme SQL Server cible de votre projet en une instance SQL Server prise en charge et validez la syntaxe.|  
|[Procédure : créer une capture instantanée d’un projet](../ssdt/how-to-create-a-snapshot-of-a-project.md)|Créez un proxy en lecture seule du schéma de la base de données et revenez au projet source lorsque des modifications indésirables sont appliquées au projet.|  
|[Procédure : utiliser des objets Microsoft SQL Server 2012 dans votre projet](../ssdt/how-to-use-microsoft-sql-server-2012-objects-in-your-project.md)|Ajoutez un nouvel objet séquence à votre projet.|  
|[Procédure : utilisation des objets de base de données CLR](../ssdt/how-to-work-with-clr-database-objects.md)|Créez et publiez des objets CLR dans le projet de base de données SQL Server Data Tools.|  
|[Procédure : convertir des projets de base de données Visual Studio 2010 en projets de base de données SQL Server et recibler vers une autre plateforme](../ssdt/how-to-convert-visual-studio-2010-database-projects-to-ssql-server-projects.md)|Convertissez une base de données SQL Server, des objets CLR et des projets d’application de la couche Données existants créés dans Visual Studio 2010 en projet de base de données SQL Server Data Tools.|  
|[Procédure : spécifier les scripts de prédéploiement et de post-déploiement](../ssdt/how-to-specify-predeployment-or-postdeployment-scripts.md)|Explique comment utiliser les scripts que vous souhaitez exécuter avant ou après le déploiement de votre base de données.|  
  
## <a name="related-sections"></a>Sections connexes

[Gérer des tables et des relations, et résoudre les erreurs](../ssdt/manage-tables-relationships-and-fix-errors.md)
