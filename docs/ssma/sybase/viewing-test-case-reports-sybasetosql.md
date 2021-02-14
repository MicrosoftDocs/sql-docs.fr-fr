---
description: Affichage des rapports de cas de test (SybaseToSQL)
title: Affichage des rapports de cas de test (SybaseToSQL) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
helpviewer_keywords:
- Tester Component,Test Case Reports
ms.assetid: cb75d281-43ef-4f4a-b754-2c4ee3b62ae7
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: 833b1e862aa5629a9a6cbea5854e3a8db5a5cab2
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100080710"
---
# <a name="viewing-test-case-reports-sybasetosql"></a>Affichage des rapports de cas de test (SybaseToSQL)
Le rapport cas de test affiche les résultats de la vérification de test et les informations de test générales. En cas d’échec d’un test, des informations sur les données incompatibles dans les objets vérifiés s’affichent également.  
  
## <a name="report-structure"></a>Structure du rapport  
Le haut du rapport présente les statistiques suivantes :  
  
-   Le nombre total d’objets testés et le nombre d’objets pour lesquels le test a réussi.  
  
-   Le nombre total de tables vérifiées et de clés étrangères, ainsi que le nombre de tables et de clés étrangères correctement mis en correspondance.  
  
-   L’heure de début, l’heure de fin du cas de test et la durée totale d’exécution.  
  
Le reste du rapport affiche des informations dans quatre catégories :  
  
**Erreurs de configuration requise**  
Affiche toutes les erreurs qui se sont produites à l’étape **conditions préalables** . Normalement, elle est ignorée.  
  
**D’initialisation**  
Affiche l’état de **réussite** ou d' **échec** de l’exécution.  
  
**Résultat des objets de test**  
Une comparaison des résultats (réussite ou échec) et l’incompatibilité du testeur SSMA détecté en cas de défaillance.  
  
**Finalisation**  
Affiche l’état de **réussite** ou d' **échec** de l’exécution.  
  
## <a name="see-also"></a>Voir aussi  
[Exécution de cas de test &#40;SybaseToSQL&#41;](../../ssma/sybase/running-test-cases-sybasetosql.md)  
[Test des objets de base de données migrés &#40;SybaseToSQL&#41;](../../ssma/sybase/testing-migrated-database-objects-sybasetosql.md)  
  
