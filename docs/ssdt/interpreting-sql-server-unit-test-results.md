---
title: Interprétation des résultats des tests unitaires SQL Server
description: Découvrez comment utiliser la fenêtre Résultats des tests ou les fichiers .trx pour afficher des résultats de tests unitaires SQL Server. Consultez comment obtenir des informations détaillées sur les résultats.
ms.prod: sql
ms.technology: ssdt
ms.topic: conceptual
ms.assetid: fde3c95b-2f68-483d-a197-0f7161b72fa3
author: markingmyname
ms.author: maghan
ms.reviewer: “”
ms.custom: seo-lt-2019
ms.date: 02/09/2017
ms.openlocfilehash: 88fc08dcc00e64c4b107a99608b891a43f236d66
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100017909"
---
# <a name="interpreting-sql-server-unit-test-results"></a>Interprétation des résultats des tests unitaires SQL Server

Lorsque vous exécutez un test unitaire SQL Server, le résultat du test est généré automatiquement, enregistré sur le disque et résumé dans la fenêtre **Résultats des tests**. Dès que vous démarrez une série de tests, la fenêtre **Résultats des tests** apparaît et affiche la progression de la série de tests. Cet affichage comprend les tests en cours d'exécution et les tests qui ont été finalisés.  
  
Pour afficher des informations détaillées sur un résultat de test, double-cliquez dessus dans la fenêtre **Résultats des tests** pour afficher la page **Détails des résultats des tests**. Pour plus d'informations sur un résultat de test, double-cliquez dessus.  
  
Pour plus d'informations sur la modification de l'affichage de la fenêtre **Résultats des tests**, consultez [Procédure : ajouter ou supprimer des colonnes dans les fenêtres de test (Visual Studio 2010)](/previous-versions/visualstudio/visual-studio-2010/ms182508(v=vs.100))ou [Procédure : ajouter ou supprimer des colonnes dans les fenêtres de test (Visual Studio 2012)](/previous-versions/visualstudio/visual-studio-2012/ms182508(v=vs.110)).  
  
## <a name="storing-test-results"></a>Stockage des résultats des tests  
Les résultats des tests unitaires sont automatiquement stockés sur votre disque dur dans des fichiers portant l'extension .trx. Un fichier .trx est un fichier XML qui contient les détails d'une série de tests. Chargez les fichiers .trx de séries de tests précédentes pour examiner les résultats de ces séries de tests ou réexécuter des tests antérieurs. Pour plus d’informations, consultez [Procédure : Réexécuter un Test (Visual Studio 2010)](/previous-versions/visualstudio/visual-studio-2010/ms182472(v=vs.100)).  
  
> [!NOTE]  
> Vous ne pouvez pas exécuter des tests unitaires à distance.  
  
Si votre équipe utilise un projet d'équipe Visual Studio Team Foundation Server pour l'aider à gérer son travail, vous pouvez également publier vos données de test dans une base de données SQL Server appelée magasin opérationnel.  
  
Pour plus d'informations sur l'enregistrement des résultats des tests, leur réutilisation et leur partage avec votre équipe, consultez [Procédure : enregistrer et ouvrir les résultats des tests dans Visual Studio 2010](/previous-versions/visualstudio/visual-studio-2010/ms404662(v=vs.100)) ou [Procédure : enregistrer et ouvrir les résultats des tests dans Visual Studio 2012](/previous-versions/ms404662(v=vs.140)).  
  
## <a name="see-also"></a>Voir aussi  
[Exécuter des tests unitaires SQL Server](../ssdt/running-sql-server-unit-tests.md)  
