---
description: Terminer la préparation du cas de test (SybaseToSQL)
title: Fin de la préparation du cas de test (SybaseToSQL) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
helpviewer_keywords:
- Tester Component,Test Case Settings
ms.assetid: 8b2a49b0-4296-4f3f-9e56-323aa6a6fa8e
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: 0c1f34f19034042d542cba2d81a55bf32288058e
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100067517"
---
# <a name="finishing-test-case-preparation-sybasetosql"></a>Terminer la préparation du cas de test (SybaseToSQL)
La dernière page de l’Assistant affiche la description du cas de test et les informations relatives aux objets impliqués dans le test. En outre, sur cette page, vous pouvez définir les options d’exécution des tests.  
  
La section **informations sur les cas de test** affiche le nom et la description du cas de test.  
  
La section **objets de test** contient la liste nommée d’objets testés regroupés par type d’objet.  
  
La section **objets affectés à analyser** affiche la liste nommée des objets dont les modifications de données doivent être comparées après l’exécution des objets testés.  
  
## <a name="test-case-settings"></a>Paramètres de cas de test  
Dans la section **paramètres de cas de test** , vous pouvez définir les options de test d’exécution suivantes :  
  
### <a name="stop-test-execution-after-first-failure"></a>Arrêter l’exécution des tests après le premier échec  
Spécifie de rompre le test si une erreur se produit pendant l’exécution du test.  
  
-   Si vous choisissez **Oui**, l’exécution des tests s’arrête si une erreur se produit.  
  
-   Si vous choisissez **non**, l’exécution des tests se poursuit après une erreur.  
  
### <a name="perform-data-rollback"></a>Effectuer une restauration des données  
Activez la restauration automatique des données après l’exécution du test.  
  
-   Si vous choisissez **Oui**, les modifications apportées aux données seront perdues après l’exécution du test.  
  
-   Si vous choisissez **non**, toutes les modifications apportées aux données d’exécution des tests seront enregistrées.  
  
### <a name="auxiliary-tables-saving-mode"></a>Mode d’enregistrement des tables auxiliaires  
Définit le mode d’enregistrement pour les tables auxiliaires créées pendant l’exécution du test. Consultez la description des tables auxiliaires dans la rubrique [exécution des cas de Test &#40;SybaseToSQL&#41;](../../ssma/sybase/running-test-cases-sybasetosql.md) .  
  
-   Si vous sélectionnez **toujours enregistrer**, les données de la table auxiliaire sont toujours stockées pour une utilisation ultérieure.  
  
-   Si vous sélectionnez **enregistrer en cas d’échec** de la comparaison de tables, les données de la table auxiliaire sont stockées uniquement si une erreur se produit.  
  
-   Si vous sélectionnez **toujours supprimer**, les tables auxiliaires sont toujours supprimées après l’exécution du test.  
  
-   Si vous sélectionnez **demander** à l’utilisateur en cas d’échec de la comparaison de tables, l’utilisateur peut sélectionner l’action nécessaire si une erreur se produit.  
  
Cliquez sur le bouton **Terminer** pour enregistrer le cas de test préparé dans [à l’aide des dépôts de test &#40;&#41;SybaseToSQL](../../ssma/sybase/using-test-repositories-sybasetosql.md).  
  
## <a name="see-also"></a>Voir aussi  
[Utilisation de référentiels de test &#40;SybaseToSQL&#41;](../../ssma/sybase/using-test-repositories-sybasetosql.md)  
[Exécution de cas de test &#40;SybaseToSQL&#41;](../../ssma/sybase/running-test-cases-sybasetosql.md)  
[Test des objets de base de données migrés &#40;SybaseToSQL&#41;](../../ssma/sybase/testing-migrated-database-objects-sybasetosql.md)  
  
