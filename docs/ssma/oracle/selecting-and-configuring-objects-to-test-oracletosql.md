---
description: Sélection et configuration des objets à tester (OracleToSQL)
title: Sélection et configuration des objets à tester (OracleToSQL) | Microsoft Docs
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
helpviewer_keywords:
- Selection of Objects to Test,Parameter Comparison Settings
ms.assetid: 29fb6542-5c1f-4b14-a822-87700beb7623
author: nahk-ivanov
ms.author: alexiva
manager: alexiva
ms.openlocfilehash: 2cf0aab82f56740dfe73476af89899fd9fd135f6
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100067611"
---
# <a name="selecting-and-configuring-objects-to-test-oracletosql"></a>Sélection et configuration des objets à tester (OracleToSQL)
À cette étape, vous sélectionnez les objets à tester, puis vous configurez les paramètres de comparaison des paramètres de sortie des procédures et des fonctions, ainsi que des valeurs de retour des fonctions.  
  
## <a name="selection-of-objects-to-test"></a>Sélection des objets à tester  
Dans l’arborescence d’objets Oracle située sur le côté gauche de la fenêtre, cochez les objets que vous souhaitez appeler pendant le processus de test. Consultez la liste complète des objets testable dans la rubrique [test des objets de base de données migrés &#40;OracleToSQL&#41;](../../ssma/oracle/testing-migrated-database-objects-oracletosql.md) .  
  
Si SSMA tester ne prend pas en charge les objets sélectionnés pour le test, le lien étiqueté **certains objets sélectionnés contient des erreurs** sous l’arborescence objets. Cliquez sur ce lien pour afficher les raisons pour lesquelles ces objets ne peuvent pas être testés et pour effacer la sélection d’objets incorrects.  
  
Sur le côté droit, vous pouvez afficher plusieurs pages la page **SQL** affiche la définition de l’objet actuel. La page **paramètres** répertorie les paramètres si l’objet est une procédure stockée ou une fonction. La page **Propriétés** affiche des caractéristiques supplémentaires de l’objet. Consultez la description des pages **comparaisons de paramètres** et **valeurs d’appel** ci-dessous.  
  
## <a name="parameter-comparison-settings"></a>Paramètres de comparaison des paramètres  
Établissez les règles de comparaison pour les paramètres de sortie et les valeurs de retour dans la page **comparaisons de paramètres** . Vous pouvez définir les paramètres suivants.  
  
### <a name="use-during-test-comparisons"></a>Utiliser pendant les comparaisons de tests  
Active l’utilisation du paramètre sélectionné dans la comparaison des résultats des tests.  
  
-   Si vous choisissez **true**, SSMA compare la valeur de sortie de ce paramètre après l’exécution de la procédure sur Oracle avec la valeur correspondante sur SQL Server.
  
-   Si vous choisissez la **valeur false**, le paramètre sera exclu de la vérification des résultats.  
  
### <a name="use-custom-scale"></a>Utiliser l’échelle personnalisée  
Pour les paramètres de type de données numérique, vous pouvez définir une échelle personnalisée pour la comparaison.  
  
-   Si vous choisissez **true**, les valeurs numériques sont arrondies en fonction de la valeur d' **échelle de comparaison** avant d’être comparées.  
  
-   Si vous choisissez la **valeur false**, la comparaison numérique sera exacte.  
  
### <a name="comparing-scale"></a>Comparaison de l’échelle  
Disponible uniquement si l’option utiliser la mise à l' **échelle personnalisée** a la valeur **true**. Il s’agit de la précision pour la comparaison numérique.  
  
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
  
-   Si vous choisissez la **valeur false**, la comparaison respecte la casse.  
  
### <a name="ignore-trailing-spaces"></a>Ignorez les espaces de fin  
Contrôle la manière dont les espaces de fin sont traités pendant la comparaison.  
  
-   Si vous choisissez **true**, les chaînes comparées sont tronquées à droite avant d’être comparées.  
  
-   Si vous choisissez la **valeur false**, les chaînes comparées conservent les espaces blancs de fin.  
  
## <a name="specify-input-values-for-procedures-and-functions-call-values"></a>Spécifier des valeurs d’entrée pour des procédures et des fonctions (valeurs d’appel)  
Vous pouvez spécifier des valeurs de paramètre d’entrée sur la page **valeurs d’appel** . Le bouton **Ajouter un appel** ajoute un nouvel appel avec des valeurs de paramètre vides. Le bouton **Supprimer les appels** supprime l’appel en cours.  
  
## <a name="next-step"></a>étape suivante  
[Sélection et configuration des objets affectés &#40;OracleToSQL&#41;](../../ssma/oracle/selecting-and-configuring-affected-objects-oracletosql.md)  
  
## <a name="see-also"></a>Voir aussi  
[Test des objets de base de données migrés &#40;OracleToSQL&#41;](../../ssma/oracle/testing-migrated-database-objects-oracletosql.md)  
  
