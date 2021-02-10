---
description: Sélection et configuration des objets à tester (SybaseToSQL)
title: Sélection et configuration des objets à tester (SybaseToSQL) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
helpviewer_keywords:
- Tester Component,Parameter Comparision Setting
- Tester Component,Selecting Objects
ms.assetid: 89c23aad-bfee-4917-bc16-175288390ac0
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: a43c4d8b1448949725820d69c6489aff26770d98
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100060564"
---
# <a name="selecting-and-configuring-objects-to-test-sybasetosql"></a>Sélection et configuration des objets à tester (SybaseToSQL)
À cette étape, vous sélectionnez les objets à tester, puis vous configurez les paramètres de comparaison des paramètres de sortie des procédures et des fonctions, ainsi que des valeurs de retour des fonctions.  
  
## <a name="selection-of-objects-to-test"></a>Sélection des objets à tester  
Dans l’arborescence d’objets Sybase située sur le côté gauche de la fenêtre, cochez les objets que vous souhaitez appeler pendant le processus de test. Consultez la liste complète des objets testable dans la rubrique [test des objets de base de données migrés &#40;SybaseToSQL&#41;](../../ssma/sybase/testing-migrated-database-objects-sybasetosql.md) .  
  
Si SSMA tester ne prend pas en charge les objets sélectionnés pour le test, le lien étiqueté **certains objets sélectionnés contient des erreurs** sous l’arborescence objets. Cliquez sur ce lien pour afficher les raisons pour lesquelles ces objets ne peuvent pas être testés et pour effacer la sélection d’objets incorrects.  
  
Sur le côté droit, vous pouvez afficher plusieurs pages la page **SQL** affiche la définition de l’objet actuel. Dans les pages **pré SQL** et **après SQL** , vous pouvez spécifier des scripts qui s’exécutent avant et après le démarrage de l’appel de l’objet de test. Cela peut être utile lorsque l’objet requiert des objets supplémentaires, tels que des tables temporaires ou des curseurs. La page **paramètres** répertorie les paramètres si l’objet est une procédure stockée ou une fonction. La page **Propriétés** affiche des caractéristiques supplémentaires de l’objet. Consultez les pages Description des **paramètres comanalysis** et **valeurs d’appel** ci-dessous.  
  
## <a name="parameter-comparison-settings"></a>Paramètres de comparaison des paramètres  
Établissez les règles de comparaison pour les paramètres de sortie et les valeurs de retour dans la page **Comparaison de paramètres** . Vous pouvez définir les paramètres suivants.  
  
### <a name="use-during-comparisons"></a>Utiliser pendant les comparaisons  
Active l’utilisation du paramètre sélectionné dans la comparaison des résultats des tests.  
  
-   Si vous choisissez **true**, SSMA compare la valeur de sortie de ce paramètre après l’exécution de la procédure sur Sybase avec la valeur correspondante sur [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]  
  
-   Si vous choisissez la **valeur false**, le paramètre sera exclu de la vérification des résultats.  
  
### <a name="use-custom-scale"></a>Utiliser l’échelle personnalisée  
Pour les paramètres de type de données numérique de longueur approximative et fixe, vous pouvez définir une échelle personnalisée pour la comparaison.  
  
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
Vous pouvez spécifier des valeurs de paramètre d’entrée sur la page **valeurs d’appel** . Le bouton **Ajouter un appel** ajoute un nouvel appel avec des valeurs de paramètre vides. Le bouton **Supprimer l’appel** supprime l’appel en cours.  
  
## <a name="next-step"></a>étape suivante  
[Sélection et configuration des objets affectés &#40;SybaseToSQL&#41;](../../ssma/sybase/selecting-and-configuring-affected-objects-sybasetosql.md)  
  
## <a name="see-also"></a>Voir aussi  
[Test des objets de base de données migrés &#40;SybaseToSQL&#41;](../../ssma/sybase/testing-migrated-database-objects-sybasetosql.md)  
  
