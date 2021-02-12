---
description: Sélection et configuration des objets affectés (OracleToSQL)
title: Sélection et configuration des objets affectés (OracleToSQL) | Microsoft Docs
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
helpviewer_keywords:
- Columns Comparison Settings
- Selection of Affected Objects
ms.assetid: 545eeda2-9829-4187-a858-619a96b4b71d
author: nahk-ivanov
ms.author: alexiva
manager: alexiva
ms.openlocfilehash: b8256909a2ebc460da2dd2db826e809dd6dfb50c
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100067624"
---
# <a name="selecting-and-configuring-affected-objects-oracletosql"></a>Sélection et configuration des objets affectés (OracleToSQL)
Sur cette page, vous pouvez sélectionner des tables et des clés étrangères, les modifications dans lesquelles doivent être comparées lorsque SSMA vérifie les résultats de l’exécution pour les objets choisis à l’étape précédente. En outre, vous pouvez personnaliser les paramètres de vérification.  
  
## <a name="selection-of-affected-objects"></a>Sélection des objets affectés  
Dans l’arborescence d’objets Oracle située sur le côté gauche de la fenêtre, vérifiez les tables et les clés étrangères, les modifications dans lesquelles doit être comparée pour être identiques.  
  
Si SSMA tester ne peut pas vérifier l’un de ces objets, le lien étiqueté **certains objets sélectionnés contient des erreurs** sous l’arborescence objets. Cliquez sur ce lien pour afficher les raisons pour lesquelles ces objets ne peuvent pas être comparés et pour effacer la sélection d’objets incorrects.  
  
## <a name="table"></a>Table de charge de travail  
L’onglet table contient la vue de grille de la table sélectionnée. La grille contient les informations suivantes sur la table sélectionnée :  
  
-   Nom de la colonne  
  
-   Type de données  
  
-   Precision  
  
-   Scale  
  
-   Règle  
  
-   Default  
  
-   Identité  
  
-   Nullable  
  
## <a name="sql"></a>SQL  
L’onglet SQL contient le SQL « créer une table » de la table sélectionnée.  
  
## <a name="data"></a>Données  
L’onglet données affiche les données présentes dans la table sélectionnée.  
  
## <a name="properties"></a>Propriétés  
L’onglet Propriétés affiche les propriétés de la table sélectionnée. Les champs suivants sont présents sous l’onglet Propriétés :  
  
-   Créé ou modifié pour la dernière fois  
  
-   Nom d’objet  
  
## <a name="columns-comparison-settings"></a>Paramètres de comparaison des colonnes  
Établissez les règles de comparaison pour les colonnes de table sur la page de **comparaison des colonnes** . Vous pouvez définir les paramètres suivants.  
  
### <a name="use-during-test-comparisons"></a>Utiliser pendant les comparaisons de tests  
Détermine si cette colonne participera à la vérification des résultats des tests.  
  
-   Si vous choisissez **true**, SSMA compare le contenu de cette colonne après l’exécution du test sur Oracle avec le contenu de la colonne dans SQL Server. 
  
-   Si vous choisissez la **valeur false**, la colonne sera exclue de la vérification des résultats.  
  
### <a name="use-custom-scale"></a>Utiliser l’échelle personnalisée  
Pour les colonnes de type de données numérique, vous pouvez définir une échelle personnalisée pour la comparaison.  
  
-   Si vous choisissez **true**, les valeurs numériques sont arrondies en fonction de la valeur d' **échelle de comparaison** avant d’être comparées.  
  
-   Si vous choisissez la **valeur false**, la comparaison numérique sera exacte.  
  
### <a name="comparing-scale"></a>Comparaison de l’échelle  
  
-   Disponible uniquement si l’option utiliser la mise à l' **échelle personnalisée** a la valeur **true**. Il s’agit de la précision pour la comparaison numérique.  
  
### <a name="date-time-comparing"></a>Date et heure de comparaison  
Définit comment les valeurs de date/d’heure sont comparées.  
  
-   Si vous sélectionnez **comparer la date entière**, la comparaison complète des valeurs des deux plateformes sera effectuée.  
  
-   Si vous sélectionnez **comparer uniquement la date**, la partie heure sera ignorée.  
  
-   Si vous sélectionnez **comparer uniquement l’heure**, la partie date sera ignorée.  
  
-   Si vous sélectionnez **Ignorer les millisecondes**, les résultats seront comparés à quelques secondes.  
  
-   Si vous sélectionnez **ignorer la date et les millisecondes**, le résultat est comparé uniquement par la partie heure et ignore les parties fractionnaires d’une seconde.  
  
### <a name="ignore-strings-case"></a>Ignorer la casse des chaînes  
Contrôle le respect de la casse de la comparaison.  
  
-   Si vous choisissez **true**, la comparaison ne respecte pas la casse.  
  
-   Si vous choisissez la **valeur false**, la comparaison prendra en compte la casse.  
  
## <a name="comparing-sql"></a>Comparaison de SQL  
Vous pouvez afficher les instructions SELECT générées par SSMA tester sur la page **comparaison SQL** . Le testeur compare les jeux de résultats de ces instructions ligne par ligne. Chaque ligne suivante d’un jeu de résultats Oracle doit être égale à la ligne suivante du jeu de résultats produit dans SQL Server.
  
Vous pouvez modifier ces instructions SELECT pour fournir une vérification personnalisée. Pour enregistrer les modifications dans les instructions Oracle et dans SQL Server, utilisez les boutons **appliquer** sous le SQL source et cible, en conséquence.  
  
## <a name="next-step"></a>étape suivante  
[Personnalisation de l’ordre des appels &#40;OracleToSQL&#41;](../../ssma/oracle/customizing-calls-order-oracletosql.md)  
  
## <a name="see-also"></a>Voir aussi  
[Fin de la préparation du cas de test &#40;OracleToSQL&#41;](../../ssma/oracle/finishing-test-case-preparation-oracletosql.md)  
[Exécution de cas de test &#40;OracleToSQL&#41;](../../ssma/oracle/running-test-cases-oracletosql.md)  
[Test des objets de base de données migrés &#40;OracleToSQL&#41;](../../ssma/oracle/testing-migrated-database-objects-oracletosql.md)  
  
