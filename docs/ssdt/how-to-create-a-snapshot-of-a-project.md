---
title: Créer une capture instantanée d’un projet
description: Familiarisez-vous avec les fichiers d’application de la couche Données ou les instantanés et découvrez comment les utiliser. Découvrez comment créer ou importer des instantanés et comment les comparer.
ms.prod: sql
ms.technology: ssdt
ms.topic: conceptual
f1_keywords:
- sql.data.tools.SqlProjectImportSnapshotSummaryDialog.dialog
- sql.data.tools.SqlProjectImportSnapshotDialog.dialog
ms.assetid: bed670a3-13bd-4d88-91a1-58d5b9524a97
author: markingmyname
ms.author: maghan
ms.reviewer: “”
ms.custom: seo-lt-2019
ms.date: 02/09/2017
ms.openlocfilehash: 0a3c615e98f16c5894924817fc2d4d5acf37b962
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100018340"
---
# <a name="how-to-create-a-snapshot-of-a-project"></a>Procédure : Créer une capture instantanée d’un projet

Chaque fichier d'**application de la couche Données** vous fournit une représentation en lecture seule du schéma de la base de données au moment de la création de l'instantané. Il est essentiellement traité comme un schéma de base de données à partir duquel vous pouvez réimporter les objets de schéma vers un projet. Vous pouvez aussi le comparer au schéma d'une base de données ou d'un projet, et mettre à jour la base de données ou le projet pour refléter le schéma défini dans l'instantané.  
  
Si une erreur utilisateur se produit dans un projet de base de données source, vous pouvez rétablir le projet source dans l'état qui était le sien au moment où l'instantané a été créé. Vous pouvez aussi établir des instantanés à différentes phases de votre développement à des fins de référence.  
  
> [!WARNING]  
> Les procédures suivantes utilisent les entités créées dans les procédures précédentes des sections [Développement de base de données connectée](../ssdt/connected-database-development.md) et [Développement de base de données hors connexion orienté projet](../ssdt/project-oriented-offline-database-development.md).  
  
### <a name="to-create-a-snapshot"></a>Pour créer un instantané  
  
1.  Dans l’**Explorateur de solutions**, cliquez avec le bouton droit sur le projet **TradeDev** et sélectionnez **Application de la couche Données (\*.dacpac)...** .  
  
2.  SSDT tentera de générer le projet en premier. Si aucune erreur de build n'est détectée, un dossier **Instantané** est créé dans l'**Explorateur de solutions**. Dans ce dossier, SSDT crée un fichier .dacpac au format de nom « <Project Name>_YYYYMMDD_HH-MM-SS.dacpac ».  
  
3.  Cliquez avec le bouton droit sur le fichier .dacpac et sélectionnez **Renommer**. Remplacez le nom du fichier par défaut par « TradeDev1.dacpac ».  
  
4.  Dans l'**Explorateur de solutions**, cliquez avec le bouton droit sur la fonction **GetProductsBySupplier**, puis sélectionnez **Supprimer** pour la supprimer du projet.  
  
5.  Suivez la procédure précédente pour créer un nouvel instantané nommé **TradeDev2.dacpac**.  
  
### <a name="to-import-a-snapshot"></a>Pour importer un instantané  
  
1.  Dans l’**Explorateur de solutions**, cliquez avec le bouton droit sur le projet **TradeDev**. Sélectionnez **Importer**, puis **Application de la couche Données (\*.dacpac)...** dans les menus contextuels.  
  
2.  Dans la boîte de dialogue **Importer l'application de la couche Données**, cliquez sur **Parcourir** pour sélectionner le fichier **TradeDev1.dacpac** à utiliser comme source de l'importation.  
  
    Notez que la section **Projet cible** a été désactivée, car le projet actuel est la cible par défaut. Cliquez sur **Démarrer** pour démarrer l'importation.  
  
3.  Cliquez sur **Terminer** sur la page **Résumé**. Dans l'**Explorateur de solutions**, notez que la table supprimée a été restaurée dans le projet.  
  
    > [!WARNING]  
    > L'instantané d'importation importera toutes les entités de base de données dans le schéma de l'instantané vers le projet. En conséquence, des entités en double peuvent être créées. Par exemple, chacune des tables et vues contient à présent une copie supplémentaire d’elle-même nommée <ObjectName_1>. Dans l'**Explorateur de solutions**, cliquez avec le bouton droit sur chacun de ces objets en double et sélectionnez **Supprimer** pour le supprimer du projet.  
  
### <a name="to-compare-snapshots"></a>Pour comparer les instantanés  
  
1.  Cliquez avec le bouton droit sur **TradeDev1.dacpac** dans l'Explorateur de solutions et sélectionnez **Comparaison de schémas**. La fenêtre **Comparaison de schémas** s'ouvre.  
  
2.  Utilisez les options **Fichier d'application de la couche Données** pour définir les schémas source et cible. Vérifiez que le **Schéma source** est défini à **TradeDev1.dacpac** sous **Fichier d'application de la couche Données** et que le **Schéma cible** est défini à **TradeDev2.dacpac**.  
  
3.  Cliquez sur **OK** pour démarrer la comparaison. Notez que la fonction supprimée est mise en surbrillance en tant que différence entre l'ancien et le nouvel instantané.  
  
    Vous pouvez facilement rechercher le delta d'autres instantanés à l'aide de Comparaison de schémas. Dans ce cas, vous pouvez découvrir comment votre projet évolue au cours du processus de développement.  
  
## <a name="see-also"></a>Voir aussi  
[Procédure : utiliser le schéma pour comparer différentes définitions de base de données](../ssdt/how-to-use-schema-compare-to-compare-different-database-definitions.md)  
  
