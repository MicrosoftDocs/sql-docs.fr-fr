---
title: Créer une évaluation de la migration SSIS avec la Assistant Migration de données
description: Découvrez comment utiliser Assistant Migration de données pour évaluer un service SSIS (SQL Server Integration Service) local avant de migrer vers Azure SQL Database ou Azure SQL Managed Instance
ms.date: 08/23/2019
ms.prod: sql
ms.prod_service: dma
ms.reviewer: ''
ms.technology: dma
ms.topic: conceptual
keywords: ''
helpviewer_keywords:
- Data Migration Assistant, Assess
ms.assetid: ''
author: chugugrace
ms.author: chugu
ms.custom: seo-lt-2019
ms.openlocfilehash: a893ad7e086abfdf0cc61c785ad6d93c3eeed184
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100061524"
---
# <a name="perform-a-sql-server-integration-service-migration-assessment-with-data-migration-assistant"></a>Effectuer une évaluation de la migration du service d’intégration SQL Server avec Assistant Migration de données

## <a name="prerequisites"></a>Prérequis

Pour évaluer les packages SQL Server Integration Service (SSIS), les composants ci-dessous doivent être installés avec Assistant Migration de données :

- SQL Server Service d’intégration avec la même version que les packages SSIS à évaluer.
- Le Feature Pack Azure ou d’autres composants tiers si les packages SSIS à évaluer comportent ces composants.  

DMA doit s’exécuter avec un accès **administrateur** pour évaluer les packages SSIS dans le magasin de packages.

## <a name="performance-assessments"></a>Évaluations des performances

Les instructions pas à pas suivantes vous aident à effectuer la première évaluation de la migration de packages SQL Server Integration Service (SSIS) vers Azure SQL Database ou Azure SQL Managed Instance, à l’aide de Assistant Migration de données.

## <a name="create-an-assessment"></a>Créer une évaluation

1. Sélectionnez l’icône **nouveau** (+), puis sélectionnez le type de projet **évaluation** en tant que **service d’intégration**.

1. Définissez le type de serveur source et cible.

    Sélectionnez la source en tant que **SQL Server** et définissez le type de serveur cible sur **Azure SQL Database** ou sur **Azure SQL Managed instance**.

1. Cliquez sur **Créer**.

    ![créer une évaluation](media/dma-assess-ssis/dma-assess-ssis-create.png)

## <a name="connect-to-a-server"></a>Se connecter à un serveur

1. Suivez l’option par défaut, puis cliquez sur **suivant** pour **Sélectionner les sources**.
1. Entrez le nom de l’instance SQL Server, choisissez le type d’authentification, définissez les propriétés de connexion appropriées.
1. Facultatif Entrez un chemin d’accès au dossier qui contient les packages SSIS.
1. Facultatif Entrez le mot de passe du chiffrement du package, le cas échéant.
1. Cliquez sur **se connecter** au serveur SQL Server source.
  ![Capture d’écran montrant le volet connexion à un serveur avec l’option entrer un chemin d’accès au dossier qui contient les packages SSIS et entrez le mot de passe du chiffrement du package, le cas échéant.](media/dma-assess-ssis/dma-assess-ssis-addsource.png)

## <a name="add-sources-to-assess"></a>Ajouter des sources à évaluer

1. Sélectionnez les types de stockage de package SSIS à évaluer, puis sélectionnez **Ajouter**.
![Capture d’écran montrant le volet ajouter des sources.](media/dma-assess-ssis/dma-assess-ssis-addsource-type.png)
1. Sélectionnez **Ajouter des sources** pour ouvrir le menu contextuel connexion, si vous avez besoin d’évaluer plusieurs dossiers.
1. Cliquez sur **Start Assessment** (Démarrer l’évaluation).
  ![Démarrer l’évaluation](media/dma-assess-ssis/dma-assess-ssis-assess.png)

## <a name="view-results"></a>Afficher les résultats

La catégorie de problèmes de compatibilité fournit des fonctionnalités partiellement prises en charge ou non prises en charge qui bloquent la migration de packages SSIS locaux vers Azure-SSIS Integration Runtime. Il fournit ensuite des recommandations pour vous aider à résoudre ces problèmes.

![Afficher les résultats](media/dma-assess-ssis/dma-assess-ssis-result.png)

## <a name="next-steps"></a>Étapes suivantes

- [Vue d’ensemble de la migration des charges de travail SSIS locales vers SSIS dans ADF](/azure/data-factory/scenario-ssis-migration-overview)
- [Migrer les packages SQL Server Integration Services vers une instance managée Azure SQL Managed Instance](/azure/dms/how-to-migrate-ssis-packages-managed-instance)
- [Redéployer des packages SQL Server Integration Services vers Azure SQL Database](/azure/dms/how-to-migrate-ssis-packages)