---
description: Utilisation de référentiels de tests (OracleToSQL)
title: Utilisation de référentiels de test (OracleToSQL) | Microsoft Docs
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
helpviewer_keywords:
- Test Cases Repository
- Test Results Repository
ms.assetid: f941cce4-d3e3-4aeb-a88a-4f101a97a9f4
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: f73dd1aabecee8625ace5cc4af0d342124306815
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100063454"
---
# <a name="using-test-repositories-oracletosql"></a>Utilisation de référentiels de tests (OracleToSQL)
Le référentiel de test SSMA stocke les cas de test et les résultats des tests SSMA tester pour une utilisation ultérieure. Les données de référentiel sont enregistrées dans les tables SQL Server **TestCaseRepository** et **RunTestCaseResultRepository** dans le schéma **ssma_oracle_utilities** de la base de données **ssmatesterdb** .  
  
Les boutons suivants sont disponibles dans la boîte de dialogue référentiel des cas de test :  
  
-   Cliquez sur le bouton **Actualiser** pour actualiser la liste cas de Test ou résultats des tests.  
  
-   Cliquez sur le bouton **Fermer** pour fermer la boîte de dialogue espace de stockage des cas de test.  
  
## <a name="test-cases-repository"></a>Référentiel des cas de test  
Vous pouvez afficher le référentiel des cas de test en cliquant sur **cas de test...** dans le menu **testeur** . SSMA affiche ensuite la fenêtre de boîte **de dialogue référentiel des cas de test** avec une liste de cas de test enregistrés sur la page **cas de test** .  
  
La grille affiche les informations suivantes sur chaque cas de test :  
  
-   Nom : nom du cas de test.  
  
-   Créé : date de création du cas de test.  
  
-   Modifié : la date de la dernière modification du cas de test.  
  
-   Description : les descriptions de cas de test.  
  
Les boutons suivants sont disponibles sur la page cas de test :  
  
-   Cliquez sur le bouton **Ajouter** pour exécuter l’Assistant cas de test et créer un nouveau test.  
  
-   Cliquez sur le bouton **supprimer** pour supprimer le test sélectionné du référentiel. Lorsqu’un cas de test est supprimé, tous les Résultats des tests connexes sont également supprimés.  
  
-   Cliquez sur le bouton **modifier** pour exécuter l’Assistant cas de test et modifier le test sélectionné.  
  
-   Cliquez sur le bouton **exécuter** pour ouvrir la boîte de dialogue [exécution des cas de test (OracleToSQL)](./running-test-cases-oracletosql.md) et exécuter le test sélectionné.  
  
## <a name="test-results-repository"></a>Référentiel Résultats des tests  
Vous pouvez afficher le référentiel Résultats des tests sur la page **résultats des tests** de la fenêtre **référentiel des cas de test** . Ouvrez-le en cliquant sur **résultats des tests...** dans le menu **testeur** .  
  
Vous pouvez utiliser deux filtres sur **résultats des tests** page :  
  
-   Filtre de nom de cas de test : permet de choisir les résultats des tests par nom de cas de test. La valeur de **tous les cas de test** de ce filtre permet d’afficher les résultats des tests pour tous les cas de test.  
  
-   Filtre de date d’exécution de cas de test : filtre les résultats des tests par date d’enregistrement. La valeur de **toutes les périodes** de ce filtre permet d’afficher les résultats des tests pour toute date d’enregistrement.  
  
Les informations suivantes sur les résultats des tests s’affichent dans la grille.  
  
-   Nom : nom du cas de test.  
  
-   Enregistré : date de l’enregistrement du cas de test.  
  
-   Résultats : bref résumé de l’exécution des tests (l’info-bulle de cette cellule affiche un résumé complet de l’exécution des tests).  
  
Les boutons suivants sont disponibles sur la page résultat de test :  
  
-   Cliquez sur le bouton **Afficher** pour ouvrir [affichage des rapports de cas de test &#40;OracleToSQL&#41;](../../ssma/oracle/viewing-test-case-reports-oracletosql.md) du résultat de cas de test actuel.  
  
-   Cliquez sur le bouton **supprimer** pour supprimer le résultat de test sélectionné  
  
## <a name="see-also"></a>Voir aussi  
[Exécution de cas de test &#40;OracleToSQL&#41;](../../ssma/oracle/running-test-cases-oracletosql.md)  
[Test des objets de base de données migrés &#40;OracleToSQL&#41;](../../ssma/oracle/testing-migrated-database-objects-oracletosql.md)  
