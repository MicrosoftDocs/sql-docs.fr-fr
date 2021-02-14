---
title: Vérification du code de la base de données à l'aide de tests unitaires SQL Server
description: En savoir plus sur les tests unitaires SQL Server. Découvrez les éditions de Visual Studio qui prennent en charge les tests et affichez les ressources sur l’utilisation de tests pour vérifier le code de la base de données.
ms.prod: sql
ms.technology: ssdt
ms.topic: conceptual
ms.assetid: 003713e2-de6b-4277-a0a8-7d1f2f4ffb39
author: markingmyname
ms.author: maghan
ms.reviewer: “”
ms.custom: seo-lt-2019
ms.date: 02/09/2017
ms.openlocfilehash: 4c05b9e3a3330b395ee3cd8c2cb3da5aa7d50508
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100066697"
---
# <a name="verifying-database-code-by-using-sql-server-unit-tests"></a>Vérification du code de la base de données à l'aide de tests unitaires SQL Server

Vous pouvez utiliser des tests unitaires SQL Server pour établir l’état de référence de votre base de données, puis pour vérifier les modifications apportées par la suite aux objets de base de données.  
  
Pour établir l’état de référence d’une base de données, vous allez créer un projet de test et écrire des ensembles de Transact\-SQL qui s’appliquent à vos objets de base de données. Grâce à ces tests, vous pouvez vérifier dans un environnement de développement isolé si ces objets fonctionnent comme prévu. Les tests unitaires SQL Server fonctionnent bien en association avec le développement de base de données hors connexion à l’aide de projets de base de données SQL Server (voir [Développement de base de données hors connexion orienté projet](../ssdt/project-oriented-offline-database-development.md) pour plus d’informations). Dès que vous aurez votre ensemble de référence de tests unitaires SQL Server, vous pourrez les utiliser pour vérifier que la base de données fonctionne avant d’archiver les modifications auprès de la gestion de version.  
  
Créez des tests qui vérifient les modifications apportées à un objet de base de données. De plus, vous pouvez générer automatiquement des stubs de code Transact\-SQL qui testent les fonctions de base de données, les déclencheurs et les procédures stockées.  
  
> [!NOTE]  
> Il est possible de créer et d’exécuter des tests unitaires SQL Server sans ouvrir de projet de base de données. Toutefois, si vous souhaitez générer automatiquement des scripts de test pour tester des objets de base de données spécifiques de votre projet, vous devez ouvrir le projet de base de données contenant les objets à tester.  
  
Lorsque vous ou les membres de l'équipe modifiez le schéma de la base de données, utilisez ces tests pour vérifier si des modifications empêchent le bon fonctionnement des fonctionnalités existantes. Vous créez des tests unitaires SQL Server afin de compléter les tests unitaires logiciels élaborés par les développeurs de logiciels. Vous devez effectuer les ensembles de tests pour vérifier le comportement global de votre application.  
  
Les tests unitaires vérifient que les procédures réussissent et échouent comme prévu. Le fait de tester que les échecs appropriés se produisent est connu sous le nom de test négatif.  
  
## <a name="visual-studio-editions-support-for-sql-server-unit-tests"></a>Prise en charge des tests unitaires SQL Server dans les éditions de Visual Studio  
La fonctionnalité de tests unitaires de SQL Server, ajoutée dans la mise à jour de décembre 2012 de SQL Server Data Tools, permet de créer, de modifier et d’exécuter des tests unitaires SQL Server dans Visual Studio 2010 Professional, Visual Studio 2012 Professional et les éditions ultérieures.  
  
Pour avoir la certitude d’installer la dernière mise à jour de SQL Server Data Tools, accédez à la [boîte de dialogue Rechercher les mises à jour](../ssdt/check-for-updates-dialog-box.md).  
  
L’interpréteur de commandes SQL Server Data Tools intégré à Visual Studio 2010 et Visual Studio 2012 ne prend pas en charge les tests unitaires SQL Server.  
  
## <a name="common-tasks"></a>Tâches courantes  
Dans le tableau suivant, vous pouvez trouver des descriptions de tâches courantes qui prennent en charge ce scénario et proposent des liens vers des informations complémentaires sur la manière de compléter correctement ces tâches.  
  
|Tâches courantes|Contenu de prise en charge|  
|----------------|----------------------|  
|**Effectuer des exercices pratiques :** suivez une procédure pas à pas pour vous familiariser avec la création et l’exécution d’un test unitaire SQL Server simple. Elle comporte un exemple de test unitaire SQL Server négatif.|[Procédure pas à pas : création et exécution d’un test unitaire SQL Server](../ssdt/walkthrough-creating-and-running-a-sql-server-unit-test.md)|  
|**Définir des tests unitaires SQL Server :** vous devez créer des tests unitaires SQL Server dans leur propre projet. Configurez les paramètres de ce projet et définissez une ou plusieurs conditions de test pour chaque test.|[Création et définition de tests unitaires SQL Server](../ssdt/creating-and-defining-sql-server-unit-tests.md)<br /><br />[Utilisation de conditions de test dans les tests unitaires SQL Server](../ssdt/using-test-conditions-in-sql-server-unit-tests.md)|  
|**Exécuter des tests unitaires SQL Server :** après avoir défini un ou plusieurs tests unitaires, exécutez-les, déboguez les problèmes, puis examinez vos résultats de test.|[Exécuter des tests unitaires SQL Server](../ssdt/running-sql-server-unit-tests.md)|  
|**Gérer des groupes de tests (Visual Studio 2010) :** organisez les tests en groupes, s’ils doivent être exécutés simultanément. Les listes de tests sont encore prises en charge, mais pour les nouveaux groupes de tests, envisagez plutôt des catégories de tests. Par exemple, vous pourriez créer une catégorie de test pour vos déclencheurs ou tous les objets d’un *schéma* en particulier.|[Définir des catégories de test pour regrouper des tests](/previous-versions/visualstudio/visual-studio-2010/dd286595(v=vs.100))<br /><br />[Définir des listes de tests pour regrouper des tests](/previous-versions/visualstudio/visual-studio-2010/dd286584(v=vs.100))|  
|**Archiver les projets de test et les tests dans la gestion de version :** après avoir exécuté vos tests et vérifié qu’ils fonctionnent correctement, vous devez archiver votre projet de test et tous les fichiers associés dans la gestion de version afin que tous les membres de votre équipe puissent les exécuter. Si vous archivez votre projet de test dans la gestion de version avec votre projet de base de données SQL Server, vous pourrez facilement restaurer les versions compatibles de la base de données et des tests de base de données.|[Ajouter des fichiers à la gestion de version](/previous-versions/visualstudio/visual-studio-2010/ms181374(v=vs.100))<br /><br />[Utiliser les fenêtres Archiver et Modifications en attente](/previous-versions/visualstudio/visual-studio-2010/ms245462(v=vs.100))|  
|**Définir des conditions de test personnalisées :** vous pouvez créer des conditions de test personnalisées si vous devez tester le comportement non couvert par l’ensemble de conditions de test par défaut. Vous devez distribuer ces conditions à tous les membres de l'équipe souhaitant exécuter les tests qui utilisent les nouvelles conditions.|[Scénario : définir des conditions de test personnalisées pour les tests unitaires SQL Server](/previous-versions/visualstudio/visual-studio-2010/dd193282(v=vs.100))|  
|**Mettre à jour les tests unitaires existants :** si vos tests unitaires de base de données ont été créés dans une version précédente de Visual Studio, vous devez les mettre à niveau pour qu’ils puissent se générer et s’exécuter avec cette version.<br /><br />**REMARQUE :** Si vous ouvrez une solution contenant à la fois un projet de base de données et un projet de test unitaire de base de données provenant d’une version antérieure de Visual Studio, vous devrez mettre à niveau le projet de base de données. Vous ne serez pas invité à mettre à niveau les projets de test unitaire de base de données, qui doivent être mis à niveau manuellement.|[Mettre à niveau un projet de test antérieur contenant des tests unitaires de base de données](../ssdt/upgrade-an-older-test-project-containing-database-unit-tests.md)|  
|**Extensibilité :** vous pouvez étendre SQL Server Data Tools en créant des extensions de fonctionnalités.|[Conditions de test personnalisées pour les tests unitaires SQL Server](../ssdt/custom-test-conditions-for-sql-server-unit-tests.md)|  
|**Résoudre les problèmes :** pour en savoir plus sur la résolution des problèmes courants rencontrés avec les tests unitaires SQL Server.|[Résoudre les problèmes liés aux tests unitaires de base de données SQL Server](../ssdt/troubleshooting-sql-server-database-unit-testing-issues.md)|  
  
## <a name="related-scenarios"></a>Scénarios connexes  
[Développement de base de données hors connexion orienté projet](../ssdt/project-oriented-offline-database-development.md)  
Les tests unitaires de base de données sont particulièrement efficaces s’ils sont utilisés conjointement avec le développement de projets hors connexion à l’aide de projets de base de données SQL Server.  
  
## <a name="see-also"></a>Voir aussi  
[SQL Server Data Tools](../ssdt/sql-server-data-tools.md)  
