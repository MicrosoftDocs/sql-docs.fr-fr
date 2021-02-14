---
title: Ouvrir un test unitaire SQL Server à modifier
description: Découvrez comment modifier un test unitaire SQL Server pour pouvoir ajouter des fonctionnalités ou personnaliser des conditions. Découvrez les différentes façons d’ouvrir le fichier de code source du test.
ms.prod: sql
ms.technology: ssdt
ms.topic: conceptual
ms.assetid: c6af1b12-54cd-42f9-b2ef-7164f8078323
author: markingmyname
ms.author: maghan
ms.reviewer: “”
ms.custom: seo-lt-2019
ms.date: 02/09/2017
ms.openlocfilehash: 0cd06acbd8817c62e728ee054024f570682450fc
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100018169"
---
# <a name="how-to-open-a-sql-server-unit-test-to-edit"></a>Procédure : Ouvrir un test unitaire SQL Server à modifier

Après avoir créé un test unitaire SQL Server, utilisez le **Concepteur de test unitaire SQL Server** pour ajouter des instructions Transact\-SQL et des conditions de test. Les tests créés à l'aide du concepteur génèrent du code Visual C# ou Visual Basic. Ce code s'exécute lors de l'exécution du test.  
  
Si le test vous convient, exécutez-le en l'état. Si vous souhaitez ajouter d'autres fonctionnalités à ce test unitaire, modifiez son code. Ce code réside dans un fichier .cs ou .vb dans votre projet de test. Pour plus d’informations, consultez [Fichiers de tests unitaires SQL Server](../ssdt/sql-server-unit-test-files.md). Vous pouvez également personnaliser vos tests lors de la création de nouvelles conditions de test. Pour plus d’informations, consultez [Procédure : créer des conditions de test pour le Concepteur de test unitaire de base de données (Visual Studio 2010)](/previous-versions/visualstudio/visual-studio-2010/aa833409(v=vs.100)).  
  
> [!NOTE]  
> Si vous supprimez une méthode de test en modifiant le fichier .cs ou .vb, la méthode de test continue d'apparaître dans le **Concepteur de test unitaire SQL Server**. Cette situation se produit parce que la méthode InitializeComponent de la classe de test contient toujours les variables des membres de ce test. Bien que le test s'affiche dans le concepteur, vous ne pouvez pas l'exécuter, car son code n'est plus présent. Pour régénérer la méthode de test pour ce test, modifiez les instructions Transact\-SQL dans l'éditeur, puis enregistrez le fichier de test .cs ou .vb ou reconstruisez le projet de test.  
  
### <a name="to-open-the-source-code-file-of-a-sql-server-unit-test-from-solution-explorer"></a>Pour ouvrir le fichier de code source d'un test unitaire SQL Server dans l'Explorateur de solutions  
  
-   Dans l'**Explorateur de solutions**, cliquez avec le bouton droit sur le fichier de code source qui contient le test unitaire SQL Server, puis cliquez sur **Afficher le code**.  
  
    La méthode de test du test unitaire s'affiche dans la fenêtre d'édition principale de Visual Studio lorsque le fichier s'ouvre.  
  
### <a name="to-open-the-source-code-file-of-a-sql-server-unit-test-from-the-test-view-window-visual-studio-2010"></a>Pour ouvrir le fichier de code source d'un test unitaire SQL Server dans la fenêtre Affichage des tests (Visual Studio 2010)  
  
1.  Exécutez un test unitaire. Pour plus d’informations, consultez la section « Exécution de tests unitaires SQL Server » dans [Procédure pas à pas : création et exécution d’un test unitaire SQL Server](../ssdt/walkthrough-creating-and-running-a-sql-server-unit-test.md).  
  
2.  Dans la fenêtre Affichage des tests, cliquez avec le bouton droit sur le test, puis cliquez sur **Ouvrir un test**.  
  
    La méthode de test du test unitaire s'affiche dans la fenêtre d'édition principale de Visual Studio lorsque le fichier s'ouvre.  
  
### <a name="to-open-the-source-code-file-of-a-sql-server-unit-test-from-the-test-view-window-visual-studio-2012"></a>Pour ouvrir le fichier de code source d'un test unitaire SQL Server dans la fenêtre Affichage des tests (Visual Studio 2012)  
  
1.  Exécutez un test unitaire. Pour plus d’informations, consultez la section « Exécution de tests unitaires SQL Server » dans [Procédure pas à pas : création et exécution d’un test unitaire SQL Server](../ssdt/walkthrough-creating-and-running-a-sql-server-unit-test.md).  
  
2.  Dans la fenêtre Explorateur de tests, cliquez sur le nom du fichier de code source d'un test unitaire.  
  
    La méthode de test du test unitaire s'affiche dans la fenêtre d'édition principale de Visual Studio lorsque le fichier s'ouvre.  
  
## <a name="see-also"></a>Voir aussi  
[Création et définition de tests unitaires SQL Server](../ssdt/creating-and-defining-sql-server-unit-tests.md)  
[Vérifier le code de la base de données à l’aide de tests unitaires SQL Server](../ssdt/verifying-database-code-by-using-sql-server-unit-tests.md)  
