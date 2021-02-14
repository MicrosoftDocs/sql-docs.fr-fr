---
title: Création et définition de tests unitaires SQL Server
description: En savoir plus sur les tests unitaires SQL Server. Affichez des sources d’informations sur la façon de créer et d’exécuter des tests unitaires, de résoudre des problèmes et d’effectuer d’autres tâches connexes.
ms.prod: sql
ms.technology: ssdt
ms.topic: conceptual
ms.assetid: 3c082177-a2b1-4fde-8833-b49b2a351815
author: markingmyname
ms.author: maghan
ms.reviewer: “”
ms.custom: seo-lt-2019
ms.date: 02/09/2017
ms.openlocfilehash: a6aab4b3ce10b59633d13ccadf3fb8ead67e375a
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100079610"
---
# <a name="creating-and-defining-sql-server-unit-tests"></a>Création et définition de tests unitaires SQL Server

Exécutez des tests unitaires SQL Server pour vérifier si les modifications apportées à un ou plusieurs objets de base de données d'un schéma empêchent le fonctionnement de certaines fonctionnalités existantes dans une application de base de données. Ces tests complètent les tests unitaires créés par les développeurs de logiciels. Vous devez exécuter les deux types de tests pour vérifier le comportement de votre application.  
  
Pour vérifier le comportement d'un objet dans votre schéma, ajoutez un test unitaire SQL Server et ajoutez un script Transact\-SQL pour tester cet objet. Ou bien, générez automatiquement le stub d'un script Transact\-SQL pour vérifier le comportement d'une fonction, d'un déclencheur ou d'une procédure stockée spécifique. Après avoir généré le stub, personnalisez-le pour obtenir des résultats significatifs.  
  
> [!NOTE]  
> Créez un test vide, ajoutez-lui du code et exécutez-le sans avoir à ouvrir un projet de base de données SQL Server. Toutefois, vous ne pouvez pas générer automatiquement un stub Transact\-SQL qui teste une fonction, un déclencheur ou une procédure stockée sans ouvrir le projet qui contient l'objet à tester.  
  
## <a name="common-tasks"></a>Tâches courantes  
Dans le tableau suivant, vous pouvez trouver des descriptions de tâches courantes qui prennent en charge ce scénario et proposent des liens vers des informations complémentaires sur la manière de compléter correctement ces tâches.  
  
|Tâches courantes|Contenu de prise en charge|  
|----------------|----------------------|  
|**Effectuer des exercices pratique** : suivez une procédure d’introduction pour vous familiariser avec la création et l’exécution d’un test unitaire SQL Server simple.|-   [Procédure pas à pas : création et exécution d’un test unitaire SQL Server](../ssdt/walkthrough-creating-and-running-a-sql-server-unit-test.md)|  
|**En savoir plus sur les tests unitaires SQL Server** : pour en savoir plus sur les fichiers et les scripts qui composent un test unitaire SQL Server. Vous apprendrez également comment utiliser des conditions de test et des assertions Transact\-SQL dans vos tests unitaires.|-   [Scripts des tests unitaires SQL Server](../ssdt/scripts-in-sql-server-unit-tests.md)<br />-   [Fichiers de tests unitaires SQL Server](../ssdt/sql-server-unit-test-files.md)<br />-   [Utilisation de conditions de test dans les tests unitaires SQL Server](../ssdt/using-test-conditions-in-sql-server-unit-tests.md)<br />-   [Utilisation d'assertions Tansact-SQL dans les tests unitaires SQL Server](../ssdt/using-transact-sql-assertions-in-sql-server-unit-tests.md)|  
|**Créer un ou plusieurs projets de test** : vous devez créer des tests unitaires SQL Server dans un projet de test. Si vous créez un test unitaire SQL Server à l'aide de l'Explorateur d'objets SQL Server avant de créer un projet de test, un projet de test est créé automatiquement. Créez plusieurs projets de test si, par exemple, vous souhaitez utiliser différents plans de génération de données ou configurations de déploiement dans les différents ensembles de tests. Lorsque vous créez le projet de test, configurez les paramètres de test (tels que la chaîne de connexion), les paramètres de déploiement et un plan de génération de données à utiliser pour ce projet.|-   [Procédure : créer un projet de test pour un test unitaire de base de données SQL Server](../ssdt/how-to-create-a-test-project-for-sql-server-database-unit-testing.md)<br />-|  
|**Configurer le mode d'exécution du test unitaire** : spécifiez la chaîne de connexion à la base de données dans laquelle vous exécutez les tests, le plan de génération de données et les paramètres de déploiement. Vous configurez ces paramètres lorsque vous ajoutez pour la première fois un test unitaire SQL Server au projet, mais vous pouvez aussi les modifier ultérieurement.|-   [Procédure : configurer l’exécution de test unitaire SQL Server](../ssdt/how-to-configure-sql-server-unit-test-execution.md)<br />-   [Vue d'ensemble des chaînes de connexion et des autorisations](../ssdt/overview-of-connection-strings-and-permissions.md)|  
|**Créer un test unitaire SQL Server** : créez automatiquement les stubs de code Transact\-SQL pour les tests unitaires SQL Server qui vérifient le comportement d'une fonction, d'un déclencheur ou d'une procédure stockée. Vous pouvez également créer un test unitaire SQL Server vide et ajouter du code Transact\-SQL pour tester les autres types d'objets de base de données.|-   [Procédure : créer des tests unitaires SQL Server pour des fonctions, des déclencheurs ou des procédures stockées](../ssdt/how-to-create-unit-tests-for-functions-triggers-stored-procedures.md)<br />-   [Procédure : créer un test unitaire SQL Server vide](../ssdt/how-to-create-an-empty-sql-server-unit-test.md)|  
|**Écrire le code d'un test unitaire** : après avoir créé un test unitaire, modifiez ou écrivez le code Transact\-SQL pour tester un objet de base de données. Pour chaque test, vous définissez une ou plusieurs conditions de test qui déterminent si le test réussit ou échoue. Pour les tests plus complexes, modifiez le code Visual Basic ou Visual C\# dans le projet de base de données. Par exemple, écrivez un test unitaire qui s'exécute dans l'étendue d'une seule transaction.|-   [Procédure : Ouvrir un test unitaire SQL Server à modifier](../ssdt/how-to-open-a-sql-server-unit-test-to-edit.md)<br />-   [Procédure : ajouter des conditions de test à des tests unitaires SQL Server](../ssdt/how-to-add-test-conditions-to-sql-server-unit-tests.md)<br />-   [Procédure : écrire un test unitaire SQL Server qui s’exécute dans l’étendue d’une seule transaction](../ssdt/how-to-write-sql-server-unit-test-that-runs-in-single-transaction-scope.md)<br />-   [Raccourcis clavier du Concepteur de test unitaire SQL Server](../ssdt/keyboard-shortcuts-for-sql-server-unit-test-designer.md)|  
|**Résoudre les problèmes** : pour en savoir plus sur la résolution des problèmes courants rencontrés avec SQL Server.|-   [Résoudre les problèmes liés aux tests unitaires de base de données SQL Server](../ssdt/troubleshooting-sql-server-database-unit-testing-issues.md)|  
  
## <a name="related-scenarios"></a>Scénarios connexes  
[Exécuter des tests unitaires SQL Server](../ssdt/running-sql-server-unit-tests.md)  
Après avoir créé les tests unitaires SQL Server, exécutez-les dans la fenêtre Affichage des tests, le Concepteur de test unitaire SQL Server ou à l'aide de Team Foundation Build.  
  
[Scénario : définir des conditions de test personnalisées pour les tests unitaires de base de données](/previous-versions/visualstudio/visual-studio-2010/dd193282(v=vs.100))  
Créez une condition de test personnalisée afin de tester un comportement que les conditions de test par défaut ne peuvent pas vérifier.  
  
## <a name="see-also"></a>Voir aussi  
[Vérifier le code de la base de données à l’aide de tests unitaires SQL Server](../ssdt/verifying-database-code-by-using-sql-server-unit-tests.md)  
