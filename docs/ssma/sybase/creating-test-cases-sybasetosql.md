---
description: Création de cas de test (SybaseToSQL)
title: Création de cas de test (SybaseToSQL) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
helpviewer_keywords:
- Tester component,Test Case Wizard
ms.assetid: b52dfd93-95af-4299-bc8f-83f2a7a6a518
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: 460ea84af51b872841eb82cd405cc29ebfe59e65
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100056684"
---
# <a name="creating-test-cases-sybasetosql"></a>Création de cas de test (SybaseToSQL)
Utilisez l’Assistant cas de test pour créer un test. Cet Assistant vous permet de créer des cas de test en choisissant des objets testés et vérifiés et en spécifiant les paramètres de test.  
  
## <a name="starting-the-test-case-wizard"></a>Démarrage de l’Assistant cas de test  
Pour démarrer l’Assistant cas de test **, cliquez sur nouveau cas de test...** dans le menu **testeur** .  
  
Au démarrage, l’Assistant recherche la base de données ssmatester2005db ou ssmatester2008db (selon le type de projet) sur le serveur Sybase source. Il s’agit du schéma d’extension du testeur utilisé pour le stockage des objets auxiliaires. Si l’Assistant cas de test ne trouve pas ssmatester2005db ou ssmatester2008db, il affiche une fenêtre de boîte de dialogue qui propose de créer la base de données d’extension du testeur. (Cette situation se produit généralement lors de la première exécution du testeur SSMA.)  
  
Si vous accédez à la boîte de dialogue, cliquez sur **Oui** pour créer la base de données de testeur Sybase sur le serveur source. Une nouvelle fenêtre de dialogue s’affiche alors à l’emplacement où vous devez ajouter un ou plusieurs appareils sur lesquels rechercher la nouvelle base de données de testeur. Cliquez sur **Ajouter** pour ajouter un appareil. Dans la boîte **de dialogue espace d’allocation pour la base de données du testeur** , choisissez l’appareil et spécifiez la taille utilisée par la base de données du testeur. En outre, vous pouvez définir l’appareil distinct pour les journaux de base de données. Notez que vous devez disposer des privilèges Sybase pour créer des bases de données.  
  
## <a name="overview-of-creating-test-cases-using-the-wizard"></a>Vue d’ensemble de la création de cas de test à l’aide de l’Assistant  
Le processus de création d’un cas de test consiste en cinq étapes :  
  
1.  [Initialisation des cas de Test &#40;&#41;SybaseToSQL ](../../ssma/sybase/initializing-test-cases-sybasetosql.md).  
  
2.  [Sélection et configuration des objets pour tester &#40;&#41;SybaseToSQL ](../../ssma/sybase/selecting-and-configuring-objects-to-test-sybasetosql.md).  
  
3.  [Sélection et configuration des objets affectés &#40;&#41;SybaseToSQL ](../../ssma/sybase/selecting-and-configuring-affected-objects-sybasetosql.md).  
  
4.  [Personnalisation de l’ordre des appels &#40;la&#41;SybaseToSQL ](../../ssma/sybase/customizing-calls-order-sybasetosql.md).  
  
5.  [Fin de la préparation du cas de Test &#40;&#41;SybaseToSQL ](../../ssma/sybase/finishing-test-case-preparation-sybasetosql.md).  
  
## <a name="see-also"></a>Voir aussi  
[Test des objets de base de données migrés &#40;SybaseToSQL&#41;](../../ssma/sybase/testing-migrated-database-objects-sybasetosql.md)  
  
