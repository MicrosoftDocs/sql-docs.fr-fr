---
title: Créer un test unitaire SQL Server vide
description: Découvrez comment créer des tests unitaires SQL Server. Découvrez comment utiliser les mêmes scripts TestInitialize et TestCleanup que les autres tests et comment utiliser des scripts différents.
ms.prod: sql
ms.technology: ssdt
ms.topic: conceptual
f1_keywords:
- sql.data.tools.unittesting.createtest
ms.assetid: b6f3cd5a-3389-42d6-a93f-97b3ddf31b95
author: markingmyname
ms.author: maghan
ms.reviewer: “”
ms.custom: seo-lt-2019
ms.date: 02/09/2017
ms.openlocfilehash: 510d669d780a7f7b6f71f5a84eab87e9b70bf4b2
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100018319"
---
# <a name="how-to-create-an-empty-sql-server-unit-test"></a>Procédure : Créer un test unitaire SQL Server vide

Incluez des tests unitaires dans votre projet de base de données pour vérifier que les modifications que vous apportez aux objets de base de données n'empêchent pas les fonctionnalités existantes. Les procédures suivantes expliquent comment créer des tests unitaires SQL Server pour n'importe quel objet de base de données. SQL Server Data Tools offre une prise en charge supplémentaire pour des fonctions de base de données, des déclencheurs et des procédures stockées. Pour plus d’informations, consultez [Procédure : créer des tests unitaires SQL Server pour des fonctions, des déclencheurs ou des procédures stockées](../ssdt/how-to-create-unit-tests-for-functions-triggers-stored-procedures.md).  
  
Lorsque vous créez un test unitaire SQL Server à l'aide de la première procédure, un projet de test est automatiquement créé s'il n'en existe aucun. S'il existe déjà des projets de test, vous avez la possibilité d'ajouter le nouveau test dans un de ces projets ou de créer un projet de test. Pour plus d’informations sur les projets de test, consultez [Procédure : créer un projet de test pour un test unitaire de base de données SQL Server](../ssdt/how-to-create-a-test-project-for-sql-server-database-unit-testing.md).  
  
Vous avez deux options pour créer un test unitaire SQL Server :  
  
-   Créer un test unitaire SQL Server dans une nouvelle classe de test.  
  
    Tous les tests unitaires SQL Server dans une classe de test donnée utilisent les mêmes scripts TestInitialize et TestCleanup. Créez une classe de test si vous souhaitez que votre test unitaire utilise des scripts TestInitialize et TestCleanup différents de ceux des autres tests unitaires. Pour plus d'informations, consultez [Scripts des tests unitaires SQL Server](../ssdt/scripts-in-sql-server-unit-tests.md).  
  
-   Créer un test unitaire SQL Server dans une classe de test existante.  
  
    Sélectionnez cette option si votre test unitaire utilise les mêmes scripts TestInitialize et TestCleanup que les autres tests unitaires de la classe.  
  
### <a name="to-create-a-sql-server-unit-test-inside-a-new-test-class"></a>Pour créer un test unitaire SQL Server dans une nouvelle classe de test  
  
1.  Dans le menu **Test**, cliquez sur **Nouveau test**.  
  
    La boîte de dialogue **Ajouter un nouveau test** s'affiche.  
  
2.  Sous **Modèles**, cliquez sur T **est unitaire SQL Server**.  
  
3.  Sous **Nom du test**, entrez un nom pour le test.  
  
4.  Sous **Ajouter au projet de test**, sélectionnez un projet de test existant auquel ajouter ce test. S'il n'existe aucun projet de test ou si vous souhaitez créer un projet de test, sélectionnez **Créer un nouveau projet de test <language>** .  
  
5.  Cliquez sur **OK**.  
  
    Si votre projet de test est nouveau, la boîte de dialogue **Nouveau projet de test** s'affiche. Nommez le projet, puis cliquez sur **OK**.  
  
    Si votre projet de test est nouveau ou n'a pas été configuré, la boîte de dialogue **Configuration du test SQL Server<ProjectName>** s'affiche. Cette boîte de dialogue vous permet de configurer les informations suivantes du projet de test :  
  
    -   La connexion de base de données utilisée pour effectuer des tests.  
  
    -   La connexion de base de données utilisée pour valider des tests, pour déployer une base de données et générer des données.  
  
    -   Le déploiement automatique du projet de base de données et les modifications de schéma associées apportées à une configuration de projet donnée avant exécution des tests unitaires.  
  
    Pour plus d’informations, consultez [Procédure : configurer l’exécution de test unitaire SQL Server](../ssdt/how-to-configure-sql-server-unit-test-execution.md).  
  
6.  Spécifiez les informations de configuration du projet et cliquez sur **OK**.  
  
    \- ou -  
  
    Cliquez sur **Annuler** pour créer le test unitaire sans configurer le projet de test.  
  
    Le test vide apparaît dans le **Concepteur de test unitaire SQL Server**. Selon le langage spécifié pour créer le projet de test, un fichier de code source Visual Basic ou Visual C\# est ajouté au projet de test. Ce fichier contient la classe de test unitaire SQL Server générée par SQL Server Data Tools pour le test unitaire que vous venez de créer. Cette classe de test peut contenir un ou plusieurs tests unitaires que vous pouvez ajouter par le biais du Concepteur de test unitaire SQL Server ou du code en tant que nouvelles méthodes de test dans la classe de test.  
  
    Vous pouvez également ajouter des tests supplémentaires comme suit :  
  
    -   Cliquez avec le bouton droit sur un projet de test dans l'**Explorateur de solutions**, cliquez sur **Ajouter**, sur **Nouveau test**, puis sur **Test unitaire SQL Server**.  
  
    -   Dans l'Explorateur d'objets SQL Server, sélectionnez Créer des tests unitaires.  
  
    Lorsque vous sélectionnez ce fichier dans l'**Explorateur de solutions** il s'affiche dans le Concepteur de test unitaire SQL Server, par défaut. Pour afficher le code ou le personnaliser pour ajouter des fonctionnalités à vos tests unitaires, sélectionnez le fichier, cliquez dessus avec le bouton droit, puis choisissez **Afficher le code**.  
  
### <a name="to-create-a-sql-server-unit-test-inside-an-existing-test-class"></a>Pour créer un test unitaire SQL Server dans une classe de test existante  
  
1.  Ouvrez une classe de test unitaire SQL Server existante dans le **Concepteur de test unitaire SQL Server**. Pour accéder au **Concepteur de test unitaire SQL Server**, double-cliquez sur le fichier de code source d'un test unitaire dans l'**Explorateur de solutions**.  
  
2.  Dans la barre de navigation, cliquez sur le signe plus ( **+** ) pour afficher la boîte de dialogue **Spécifier un nom de test unitaire**.  
  
3.  Tapez un nom et cliquez sur **OK**.  
  
    Le nouveau test unitaire SQL Server est disponible dans la liste déroulante de la barre de navigation. Il est également ajouté en tant que nouvelle méthode de test dans la classe de test. Pour afficher la méthode de test dans le code, sélectionnez le fichier de classe, cliquez dessus avec le bouton droit, puis choisissez **Afficher le code**. Le nom du fichier de classe de test actif s'affiche dans l'onglet en haut du **Concepteur de test unitaire SQL Server**.  
  
Après avoir configuré le projet de test et créé le test unitaire, voici les étapes suivantes :  
  
-   Ajouter un script de test Transact\-SQL.  
  
-   Définissez les actions avant test et après test.  
  
-   Ajoutez des conditions de test ou une autre instruction d'assertion pour vérifier les résultats du script.  
  
> [!NOTE]  
> La condition de test Non concluant est la condition par défaut ajoutée à chaque test. Cette condition de test est incluse pour indiquer que la vérification du test n'a pas été implémentée. Supprimez cette condition de test de votre test après avoir ajouté d'autres conditions de test. Pour plus d’informations, consultez [Procédure : ajouter des conditions de test à des tests unitaires SQL Server](/previous-versions/visualstudio/visual-studio-2010/aa833242(v=vs.100)).  
  
## <a name="see-also"></a>Voir aussi  
[Procédure : exécuter des tests unitaires SQL Server](../ssdt/how-to-run-sql-server-unit-tests.md)  
[Création et définition de tests unitaires SQL Server](../ssdt/creating-and-defining-sql-server-unit-tests.md)  
[Création de tests unitaires](/previous-versions/visualstudio/visual-studio-2008/ms182523(v=vs.90))  
