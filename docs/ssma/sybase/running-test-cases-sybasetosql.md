---
description: Exécution de cas de test (SybaseToSQL)
title: Exécution des cas de test (SybaseToSQL) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
helpviewer_keywords:
- Tester Component,Execution Steps
ms.assetid: 195ffdef-cfde-4bf4-a3ae-e7402bb07972
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: 8cb7521c0f58526c6dbc9e0f5fa7bfae848f6d38
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100017628"
---
# <a name="running-test-cases-sybasetosql"></a>Exécution de cas de test (SybaseToSQL)
Lorsque SSMA tester exécute un cas de test, il exécute les objets sélectionnés pour le test et crée un rapport sur les résultats de la vérification. Si les résultats sont identiques sur les deux plateformes, le test a réussi. La correspondance entre les objets entre Sybase et [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] est déterminée en fonction des paramètres de mappage de schéma pour le projet SSMA actuel.  
  
La nécessité d’un test réussi est que tous les objets Sybase sont convertis et chargés dans la base de données cible. En outre, les données de la table doivent être migrées afin que le contenu des tables sur les deux plateformes soit synchronisé.  
  
## <a name="run-test-case"></a>Exécuter le cas de test  
Pour exécuter le cas de test préparé :  
  
1.  Cliquez sur le bouton **Exécuter**.  
  
2.  Dans la boîte de dialogue **se connecter à Sybase** , entrez les informations de connexion, puis cliquez sur **se connecter**.  
  
Une fois le test terminé, le rapport de cas de test est créé. Cliquez sur le bouton **rapport** pour afficher les [rapports d’affichage des rapports de cas de Test &#40;SybaseToSQL&#41;](../../ssma/sybase/viewing-test-case-reports-sybasetosql.md). Le résultat du test (rapport de cas de test) est automatiquement stocké dans le [à l’aide des dépôts de test &#40;&#41;SybaseToSQL ](../../ssma/sybase/using-test-repositories-sybasetosql.md) pour une utilisation ultérieure.  
  
## <a name="test-case-execution-steps"></a>Étapes d’exécution du cas de test  
  
### <a name="prerequisites"></a>Prérequis  
Le testeur SSMA vérifie si toutes les conditions préalables sont remplies pour l’exécution des tests avant le démarrage du test. Si certaines conditions ne sont pas satisfaites, un message d’erreur s’affiche.  
  
### <a name="initialization"></a>Initialisation  
À cette étape, SSMA tester crée des objets auxiliaires (tables, déclencheurs et vues) à la fois sur Sybase et [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Elles autorisent le suivi des modifications apportées aux tables affectées choisies pour la vérification si le mode Comparaison de tables est **modifié uniquement**.  
  
Supposons que la table vérifiée est nommée USER_TABLE. Pour une telle table, les objets auxiliaires suivants sont créés dans Sybase.  
  
Les objets suivants sont créés sur Sybase dans la base de données SSMATESTER2005db ou SSMATESTER2008db et [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] dans la base de données ssmatesterdb_syb.  
  
|Nom|Type|Description|  
|--------|--------|---------------|  
|USER_TABLE $ Trg|Déclencheur|Déclenchez l’audit des modifications dans la table vérifiée.|  
|USER_TABLE $ AUD|Table de charge de travail|Table dans laquelle les lignes supprimées et remplacées sont enregistrées.|  
|USER_TABLE $ AudI|Table de charge de travail|Table dans laquelle les lignes nouvelles et modifiées sont enregistrées.|  
|USER_TABLE|Affichage|Représentation simplifiée des modifications de la table.|  
|USER_TABLE $ nouveau|Affichage|Représentation simplifiée des lignes insérées et remplacées.|  
|USER_TABLE $ new_id|Affichage|Identification des lignes insérées et modifiées.|  
|USER_TABLE $ Old|Affichage|Représentation simplifiée des lignes supprimées et remplacées.|  
  
L’objet suivant est créé dans la base de données de la table vérifiée sur Sybase et [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
|Nom|Type|Description|  
|--------|--------|---------------|  
|USER_TABLE $ Trg|Déclencheur|Déclenchez l’audit des modifications dans la table vérifiée.|  
  
### <a name="test-object-calls"></a>Appels d’objet de test  
À cette étape, SSMA tester appelle chaque objet sélectionné pour le test, compare les résultats et affiche le rapport.  
  
### <a name="finalization"></a>Finalisation  
Au cours de la finalisation, le testeur SSMA nettoie les objets auxiliaires créés lors de l’étape **d’initialisation** .  
  
## <a name="next-step"></a>étape suivante  
[Affichage des rapports de cas de test &#40;SybaseToSQL&#41;](../../ssma/sybase/viewing-test-case-reports-sybasetosql.md)  
  
## <a name="see-also"></a>Voir aussi  
[Sélection et configuration des objets pour tester &#40;SybaseToSQL&#41;](../../ssma/sybase/selecting-and-configuring-objects-to-test-sybasetosql.md)  
[Sélection et configuration des objets affectés &#40;SybaseToSQL&#41;](../../ssma/sybase/selecting-and-configuring-affected-objects-sybasetosql.md)  
[Test des objets de base de données migrés &#40;SybaseToSQL&#41;](../../ssma/sybase/testing-migrated-database-objects-sybasetosql.md)  
  
