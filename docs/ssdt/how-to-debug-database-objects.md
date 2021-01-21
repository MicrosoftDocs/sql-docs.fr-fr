---
title: Déboguer des objets de base de données
description: Découvrez comment déboguer des procédures stockées, des fonctions et des déclencheurs. Consultez comment activer le débogage, définir des points d’arrêt et exécuter un test unitaire SQL Server en mode débogage.
ms.prod: sql
ms.technology: ssdt
ms.topic: conceptual
ms.assetid: f5d4584f-e85f-4558-b056-83681c365978
author: markingmyname
ms.author: maghan
ms.reviewer: “”
ms.custom: seo-lt-2019
ms.date: 02/09/2017
ms.openlocfilehash: b77373adb76921ee526aea70528399970bda9246
ms.sourcegitcommit: d8cdbb719916805037a9167ac4e964abb89c3909
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/20/2021
ms.locfileid: "98597024"
---
# <a name="how-to--debug-database-objects"></a>Procédure :  Déboguer des objets de base de données

Un test unitaire SQL Server inclut les éléments suivants :  
  
-   Code de test unitaire écrit en Visual C\# ou en Visual Basic. Ce code, généré par le Concepteur de test unitaire SQL Server, est chargé d'envoyer le script Transact\-SQL qui forme le corps du test.  
  
-   Une ou plusieurs conditions de test, écrites en Visual C\# ou en Visual Basic. Pour déboguer les conditions de test, suivez la procédure de débogage d'un test unitaire, tel que le décrit dans [Procédure : déboguer lors de l'exécution d'un test (Visual Studio 2010)](/previous-versions/visualstudio/visual-studio-2010/ms182484(v=vs.100)) ou [Procédure : déboguer lors de l'exécution d'un test (Visual Studio 2012)](/previous-versions/ms182484(v=vs.140)).  
  
-   Un ou plusieurs scripts Transact\-SQL qui s'exécutent sur des objets de la base de données que vous testez. Vous ne pouvez pas déboguer ces scripts Transact\-SQL.  
  
Les procédures de cette rubrique décrivent comment déboguer des objets de base de données spécifiques, tels que des procédures stockées, des fonctions et des déclencheurs dans la base de données testée. Pour déboguer un objet de base de données, suivez les procédures dans l'ordre suivant :  
  
1.  Activez le débogage SQL Server dans votre projet de test.  
  
2.  Activez le débogage d'application sur l'instance SQL Server qui héberge la base de données testée.  
  
3.  Définissez les points d'arrêt dans le script Transact\-SQL du ou des objets de base de données que vous déboguez.  
  
4.  Déboguez votre test unitaire. Dans cette procédure, vous exécutez le test en mode débogage.  
  
### <a name="to-enable-sql-debugging-on-your-test-project"></a>Pour activer le débogage SQL Server dans votre projet de test  
  
1.  Ouvrez l'**Explorateur de solutions**.  
  
2.  Dans l'**Explorateur de solutions**, cliquez avec le bouton droit sur le projet de test, puis cliquez sur **Propriétés**.  
  
    Une page de propriétés qui porte le même nom que le projet de test s'ouvre.  
  
3.  Dans la page de propriétés, cliquez sur **Déboguer**.  
  
4.  Sous **Activer les débogueurs**, cliquez sur **Activer le débogage SQL Server**.  
  
5.  Enregistrez vos modifications.  
  
### <a name="to-set-an-increased-execution-context-timeout-to-enable-debugging-for-your-test-project"></a>Pour définir un plus long délai d'attente du contexte d'exécution de façon à activer le débogage pour votre projet de test  
  
1.  Dans le menu **Fichier**, pointez sur **Ouvrir** et cliquez sur **Fichier**.  
  
2.  Accédez au dossier qui contient votre projet de test, puis double-cliquez sur le fichier app.config.  
  
    Le fichier app.config s'ouvre dans l'éditeur.  
  
3.  Modifiez le nœud ExecutionContext pour ajouter un délai d'attente de commande, comme dans l'exemple suivant :  
  
    ```  
    <ExecutionContext CommandTimeout ="300" Provider="System.Data.SqlClient" ConnectionString="Data Source=TargetServerName\TargetInstanceName;Initial Catalog=TargetDatabaseName;Integrated Security=True;Pooling=False" />  
    ```  
  
4.  Enregistrez vos modifications.  
  
5.  Régénérez votre projet de test unitaire.  
  
> [!IMPORTANT]  
> Si vous ne régénérez pas votre projet, les modifications apportées dans le fichier app.config ne seront pas appliquées lorsque vous effectuerez les tests unitaires, et le débogage échouera.  
  
### <a name="to-add-breakpoints-to-your-transact-sql-script"></a>Pour ajouter des points d'arrêt à votre script Transact\-SQL  
  
1.  Dans le menu **Affichage**, ouvrez l’**Explorateur d'objets SQL Server**.  
  
2.  Sous **Connexions de données**, développez le nœud de la base de données à tester.  
  
3.  Si un petit que « x » apparaît en regard de l'icône de base de données, la connexion à la base de données est fermée. Dans ce cas, cliquez avec le bouton droit sur la base de données, puis cliquez sur **Actualiser**. Vous devrez peut-être fournir les informations d'identification pour ouvrir la connexion à la base de données.  
  
4.  Développez le nœud **Affichages**, **Procédures stockées** ou **Fonctions** pour rechercher l'objet que vous souhaitez déboguer.  
  
5.  Double-cliquez sur l'objet à déboguer.  
  
6.  Cliquez sur la barre latérale grise pour définir un point d'arrêt.  
  
### <a name="to-debug-your-sql-server-unit-test"></a>Pour déboguer votre test unitaire de SQL Server  
  
1.  Dans Visual Studio 2010, ouvrez la fenêtre **Affichage des tests** (Test -> Fenêtres). Dans Visual Studio 2012, ouvrez la fenêtre **Explorateur de tests**.  
  
2.  Cliquez avec le bouton droit sur le test dont le script Transact\-SQL configure l'objet de base de données dans lequel vous définissez des points d'arrêt, puis sélectionnez **Déboguer la sélection**.  
  
    Le test s'exécute en mode débogage jusqu'à ce qu'un point d'arrêt soit rencontré dans l'objet de base de données.  
  
3.  (Facultatif) Pour ouvrir une autre fenêtre, ouvrez le menu **Déboguer**, pointez sur **Fenêtres**, puis cliquez sur **Points d'arrêt**, **Sortie** ou **Immédiat**.  
  
## <a name="see-also"></a>Voir aussi  
[Exécuter des tests unitaires SQL Server](../ssdt/running-sql-server-unit-tests.md)  
[Débogage de Transact-SQL (Visual Studio 2010)](/previous-versions/visualstudio/visual-studio-2010/zefbf0t6(v=vs.100))  
