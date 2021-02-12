---
title: Exécuter des tests unitaires SQL Server
description: Découvrez comment exécuter des tests unitaires SQL Server. suivez les étapes pour exécuter des tests à partir de différentes fenêtres et outils dans différentes versions de Visual Studio.
ms.prod: sql
ms.technology: ssdt
ms.topic: conceptual
ms.assetid: 34fe2d1e-d47b-4808-af56-8cc0fdae6518
author: markingmyname
ms.author: maghan
ms.reviewer: “”
ms.custom: seo-lt-2019
ms.date: 02/09/2017
ms.openlocfilehash: 05ad77151fbe0f9e7c0a9c7667b1dea6826f397f
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100018119"
---
# <a name="how-to-run-sql-server-unit-tests"></a>Procédure : Exécuter des tests unitaires SQL Server

Il existe différents moyens d’exécuter un test unitaire SQL Server, notamment en utilisant plusieurs fenêtres et une fenêtre d’invite de commandes.  
  
> [!NOTE]  
> Vous ne pouvez pas exécuter des tests unitaires à distance.  
  
Les méthodes disponibles dépendent du logiciel installé – voir [Exécuter des tests unitaires SQL Server](../ssdt/running-sql-server-unit-tests.md).  
  
### <a name="to-run-sql-server-unit-tests-using-test-view-visual-studio-2010"></a>Exécuter des tests unitaires SQL Server avec l’Affichage des tests (Visual Studio 2010)  
  
1.  Dans le menu **Test**, pointez sur **Fenêtres**, puis cliquez sur **Affichage des tests**.  
  
    La fenêtre **Affichage des tests** s’ouvre.  
  
2.  Dans la fenêtre **Affichage des tests**, cliquez sur le ou les tests à exécuter. À l'aide de la touche Ctrl ou Maj, spécifiez des blocs discontinus ou continus de tests.  
  
3.  Effectuez l'une des opérations suivantes :  
  
    -   Cliquez avec le bouton droit sur la surface de la fenêtre **Affichage des tests**, puis cliquez sur **Exécuter la sélection**.  
  
    -   Dans la barre d’outils **Affichage des tests**, cliquez sur **Exécuter la sélection**.  
  
### <a name="to-run-sql-server-unit-tests-using-test-explorer-visual-studio-2012"></a>Exécuter des tests unitaires SQL Server avec l’Explorateur de tests (Visual Studio 2012)  
  
1.  Dans le menu **Test**, pointez sur **Fenêtres**, puis cliquez sur **Explorateur de tests**.  
  
    La fenêtre **Explorateur de tests** s’ouvre.  
  
2.  Dans **l’Explorateur de tests**, cliquez sur le ou les tests à exécuter. À l'aide de la touche Ctrl ou Maj, spécifiez des blocs discontinus ou continus de tests.  
  
3.  Cliquez avec le bouton droit sur un des tests sélectionnés, puis cliquez sur **Exécuter les tests sélectionnés**.  
  
### <a name="to-run-sql-server-unit-tests-from-the-sql-server-unit-test-designer-visual-studio-2010"></a>Exécuter des tests unitaires SQL Server avec le Concepteur de tests unitaires SQL Server (Visual Studio 2010)  
  
-   Dans la barre d’outils **Outils de test**, vous trouverez les boutons nécessaires pour commencer un projet avec ou sans débogueur.  
  
Cette étape exécute tous les tests d'une série de tests active. Dès que vous démarrez une série de tests, la fenêtre **Résultats des tests** apparaît et affiche la progression de la série de tests. Cet affichage comprend les tests en cours d'exécution et les tests qui ont été finalisés. Pour plus d’informations, voir [Interprétation des résultats des tests unitaires SQL Server](../ssdt/interpreting-sql-server-unit-test-results.md).  
  
## <a name="see-also"></a>Voir aussi  
[Exécuter des tests unitaires SQL Server](../ssdt/running-sql-server-unit-tests.md)  
[Procédure : exécuter des tests automatisés à partir de Microsoft Visual Studio 2010](/previous-versions/visualstudio/visual-studio-2010/ms182470(v=vs.100))  
[Exécuter des tests automatisés en ligne de commande (Visual Studio 2010)](/previous-versions/visualstudio/visual-studio-2010/ms182486(v=vs.100))  
[Tester l’application (Visual Studio 2012)](/azure/devops/test/overview)  
