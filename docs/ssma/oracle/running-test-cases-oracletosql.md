---
description: Exécution de cas de test (OracleToSQL)
title: Exécution des cas de test (OracleToSQL) | Microsoft Docs
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: fc208cdb-7373-4f6b-8f6c-cdff9d3dcd02
author: nahk-ivanov
ms.author: alexiva
manager: alexiva
ms.openlocfilehash: bc811575dd845be43aef7a9398d8b4fb3458db1b
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100067714"
---
# <a name="running-test-cases-oracletosql"></a>Exécution de cas de test (OracleToSQL)
Lorsque SSMA tester exécute un cas de test, il exécute les objets sélectionnés pour le test et crée un rapport sur les résultats de la vérification. Si les résultats sont identiques sur les deux plateformes, le test a réussi. La correspondance entre les objets entre Oracle et [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] est déterminée en fonction des paramètres de mappage de schéma pour le projet SSMA actuel.  
  
La nécessité d’un test réussi est que tous les objets Oracle sont convertis et chargés dans la base de données cible. En outre, les données de la table doivent être migrées afin que le contenu des tables sur les deux plateformes soit synchronisé.  
  
## <a name="run-test-case"></a>Exécuter le cas de test  
Pour exécuter le cas de test préparé :  
  
1.  Cliquez sur le bouton **Exécuter**.  
  
2.  Dans la boîte de dialogue **se connecter à Oracle** , entrez les informations de connexion, puis cliquez sur **se connecter**.  
  
Une fois le test terminé, le rapport de cas de test est créé. Cliquez sur le bouton **rapport** pour afficher le [rapport de cas de test](viewing-test-case-reports-oracletosql.md). Le résultat du test (rapport de cas de test) est automatiquement stocké dans le [référentiel résultats des tests](using-test-repositories-oracletosql.md) pour une utilisation ultérieure.  
  
## <a name="test-case-execution-steps"></a>Étapes d’exécution du cas de test  
  
### <a name="prerequisites"></a>Prérequis  
Le testeur SSMA vérifie si toutes les conditions préalables sont remplies pour l’exécution des tests avant le démarrage du test. Si certaines conditions ne sont pas satisfaites, un message d’erreur s’affiche.  
  
### <a name="initialization"></a>Initialisation  
À cette étape, SSMA tester crée des objets auxiliaires (tables, déclencheurs et vues) dans le schéma de SSMATESTER_ORACLE du serveur Oracle. Elles autorisent le suivi des modifications effectuées dans les objets affectés choisis pour la vérification.  
  
Supposons que la table vérifiée est nommée USER_TABLE. Pour une telle table, les objets auxiliaires suivants sont créés dans Oracle.  
  
|Nom|Type|Description|  
|-|-|-|  
|USER_TABLE $ Trg|déclencheur|Déclenchez l’audit des modifications dans la table vérifiée.|  
|USER_TABLE $ AUD|table|Table dans laquelle les lignes supprimées et remplacées sont enregistrées.|  
|USER_TABLE $ AUDI|table|Table dans laquelle les lignes nouvelles et modifiées sont enregistrées.|  
|USER_TABLE|vue|Représentation simplifiée des modifications de la table.|  
|USER_TABLE $ NOUVEAU|vue|Représentation simplifiée des lignes insérées et remplacées.|  
|USER_TABLE $ NEW_ID|vue|Identification des lignes insérées et modifiées.|  
|USER_TABLE $ OLD|vue|Représentation simplifiée des lignes supprimées et remplacées.|  
  
L’objet suivant est créé dans le schéma de la table vérifiée à l’adresse [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
|Nom|Type|Description|  
|-|-|-|  
|USER_TABLE $ Trg|déclencheur|Déclenchez l’audit des modifications dans la table vérifiée.|  
  
Et les objets suivants sont créés [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] dans la base de données ssmatesterdb.  
  
|Nom|Type|Description|  
|-|-|-|  
|USER_TABLE $ AUD|table|Table dans laquelle les lignes supprimées et remplacées sont enregistrées.|  
|USER_TABLE $ AudI|table|Table dans laquelle les lignes nouvelles et modifiées sont enregistrées.|  
|USER_TABLE|vue|Représentation simplifiée des modifications de la table.|  
|USER_TABLE $ nouveau|vue|Représentation simplifiée des lignes insérées et remplacées.|  
|USER_TABLE $ new_id|vue|Identification des lignes insérées et modifiées.|  
|USER_TABLE $ Old|vue|Représentation simplifiée des lignes supprimées et remplacées.|  
  
### <a name="test-object-calls"></a>Appels d’objet de test  
À cette étape, SSMA tester appelle chaque objet sélectionné pour le test, compare les résultats et affiche le rapport.  
  
### <a name="finalization"></a>Finalisation  
Au cours de la finalisation, le testeur SSMA nettoie les objets auxiliaires créés lors de l’étape **d’initialisation** .  
  
## <a name="next-step"></a>étape suivante  
[Affichage des rapports de cas de test &#40;OracleToSQL&#41;](../../ssma/oracle/viewing-test-case-reports-oracletosql.md)  
  
## <a name="see-also"></a>Voir aussi  
[Sélection et configuration des objets pour tester &#40;OracleToSQL&#41;](../../ssma/oracle/selecting-and-configuring-objects-to-test-oracletosql.md)  
[Sélection et configuration des objets affectés &#40;OracleToSQL&#41;](../../ssma/oracle/selecting-and-configuring-affected-objects-oracletosql.md)  
[Test des objets de base de données migrés &#40;OracleToSQL&#41;](../../ssma/oracle/testing-migrated-database-objects-oracletosql.md)  
  
