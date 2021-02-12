---
title: Exécution de tests unitaires SQL Server
description: Familiarisez-vous avec les tests unitaires SQL Server. Affichez des ressources sur la création de tests, la création de conditions de test personnalisées, l’exécution de tests et l’interprétation des résultats.
ms.prod: sql
ms.technology: ssdt
ms.topic: conceptual
f1_keywords:
- sql.data.tools.unittesting.testconfig
ms.assetid: febcc87f-eb18-4c12-ba30-82ef0d49aaa3
author: markingmyname
ms.author: maghan
ms.reviewer: “”
ms.custom: seo-lt-2019
ms.date: 02/09/2017
ms.openlocfilehash: 5983d00806f895d8112ac111312309cf93aa48ee
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100063494"
---
# <a name="running-sql-server-unit-tests"></a>Exécution de tests unitaires SQL Server

Pour améliorer et gérer la qualité de votre code, créez et exécutez des tests unitaires SQL Server qui vérifient le comportement d'un objet de base de données, puis archivez ces tests dans le contrôle de version. Lorsque vous ou un membre de l'équipe modifiez le schéma de la base de données, vous exécutez des tests unitaires SQL Server et des tests unitaires de logiciel pour vérifier que les modifications n'empêchent pas l'exécution des fonctionnalités existantes. Exécutez des tests individuels ou des groupes de tests, appelés listes de tests. Pour plus d'informations, consultez [Utilisation de listes de tests (Visual Studio 2010)](/previous-versions/visualstudio/visual-studio-2010/ms182461(v=vs.100)).  
  
## <a name="ways-to-run-sql-server-unit-tests"></a>Manières d’exécuter des tests unitaires SQL Server  
Vous pouvez effectuer des tests unitaires SQL Server de plusieurs façons, en fonction du logiciel que vous avez installé, comme suit :  
  
-   Exécutez des tests à l’aide de la fenêtre Visual Studio 2010 **Affichage des tests**. Pour plus d’informations, consultez [Procédure : exécuter des tests unitaires SQL Server](../ssdt/how-to-run-sql-server-unit-tests.md) et [Procédure : exécuter des tests automatisés à partir de Microsoft Visual Studio 2010](/previous-versions/visualstudio/visual-studio-2010/ms182470(v=vs.100)). Pour Visual Studio 2012, consultez [Procédure : exécuter des tests automatisés à partir de Microsoft Visual Studio 2012](/previous-versions/ms182470(v=vs.140)).  
  
-   Exécutez des tests à l'aide de la commande MSTest.exe à l'invite de commandes. Pour plus d'informations, consultez [Procédure : exécuter des tests automatisés à partir de la ligne de commande à l'aide de MSTest (Visual Studio 2010)](/previous-versions/visualstudio/visual-studio-2010/ms182487(v=vs.100)) or [Procédure : exécuter des tests automatisés à partir de la ligne de commande à l'aide de MSTest (Visual Studio 2012)](/previous-versions/ms182487(v=vs.140)).  
  
-   Exécutez des tests dans l'**Explorateur de solutions** en exécutant un projet de test. Pour plus d'informations, consultez [Procédure : exécuter des tests automatisés à partir de Microsoft Visual Studio 2010](/previous-versions/visualstudio/visual-studio-2010/ms182470(v=vs.100)) ou [Procédure : exécuter des tests automatisés à partir de Microsoft Visual Studio 2012](/previous-versions/ms182470(v=vs.140)).  
  
-   Réexécutez des tests dans la fenêtre **Résultats des tests**. Pour plus d’informations, consultez [Procédure : Réexécuter un Test (Visual Studio 2010)](/previous-versions/visualstudio/visual-studio-2010/ms182472(v=vs.100)).  
  
-   Exécutez des tests individuels ou des listes de tests (Visual Studio 2010) dans la fenêtre **Explorateur de tests**. Pour plus d'informations, consultez [Procédure : exécuter des tests automatisés à partir de Microsoft Visual Studio 2010](/previous-versions/visualstudio/visual-studio-2010/ms182470(v=vs.100)) ou [Procédure : exécuter des tests automatisés à partir de Microsoft Visual Studio 2012](/previous-versions/ms182470(v=vs.140)).  
  
-   Exécutez des tests à mesure que vous créez un projet dans Team Foundation Build. Pour plus d'informations, consultez [Procédure : configurer et exécuter des tests planifiés après avoir généré votre application (Visual Studio 2010)](/previous-versions/visualstudio/visual-studio-2010/ms182465(v=vs.100)) or [Procédure : configurer et exécuter des tests planifiés après avoir généré votre application (Visual Studio 2012)](/previous-versions/visualstudio/visual-studio-2012/ms182465(v=vs.110)).  
  
Exécutez vos tests unitaires SQL Server dans un ordre particulier à l'aide d'un test ordonné. Pour plus d’informations, consultez [Procédure : Créer un Test ordonné (Visual Studio 2010)](/previous-versions/visualstudio/visual-studio-2010/ms182631(v=vs.100)) ou [Procédure : Créer un Test ordonné (Visual Studio 2012)](/previous-versions/ms182631(v=vs.140)).  
  
## <a name="interpreting-tests-results"></a>Interprétation des résultats de tests  
Après avoir exécuté vos tests, la fenêtre **Résultats des tests** indique les tests ayant réussi ou échoué. Pour plus d’informations, voir [Interprétation des résultats des tests unitaires SQL Server](../ssdt/interpreting-sql-server-unit-test-results.md). Pour plus d’informations sur la façon de diagnostiquer une erreur inattendue, consultez [Procédure : Déboguer des objets de base de données](../ssdt/how-to-debug-database-objects.md).  
  
## <a name="topics-in-this-section"></a>Rubriques de cette section  
Cette section contient les rubriques suivantes :  
  
-   [Procédure : Déboguer des objets de base de données](../ssdt/how-to-debug-database-objects.md)  
  
-   [Procédure : exécuter des tests unitaires SQL Server en utilisant Team Foundation Build](../ssdt/how-to-run-sql-server-unit-tests-from-team-foundation-build.md)  
  
-   [Procédure : exécuter des tests unitaires SQL Server](../ssdt/how-to-run-sql-server-unit-tests.md)  
  
-   [Interprétation des résultats des tests unitaires SQL Server](../ssdt/interpreting-sql-server-unit-test-results.md)  
  
## <a name="related-scenarios"></a>Scénarios connexes  
[Création et définition de tests unitaires SQL Server](../ssdt/creating-and-defining-sql-server-unit-tests.md)  
Définissez des tests unitaires pour vérifier le comportement des objets de base de données et pour associer chaque projet de test à un plan de génération de données, une configuration de déploiement et une chaîne de connexion différents.  
  
[Conditions de test personnalisées pour les tests unitaires SQL Server](../ssdt/custom-test-conditions-for-sql-server-unit-tests.md)  
Créez une condition de test personnalisée afin déterminer toute condition que vous ne pouvez pas vérifier en utilisant des conditions de test par défaut.  
  
## <a name="see-also"></a>Voir aussi  
[Vérifier le code de la base de données à l’aide de tests unitaires SQL Server](../ssdt/verifying-database-code-by-using-sql-server-unit-tests.md)  
