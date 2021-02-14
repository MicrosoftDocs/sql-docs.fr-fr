---
title: Configurer l’exécution de test unitaire SQL Server
description: Découvrez comment configurer des tests unitaires SQL Server. Consultez comment spécifier des chaînes de connexion et comment déployer un schéma de la base de données.
ms.prod: sql
ms.technology: ssdt
ms.topic: conceptual
ms.assetid: e0179429-13ce-4d23-ae27-e6419de0a575
author: markingmyname
ms.author: maghan
ms.reviewer: “”
ms.custom: seo-lt-2019
ms.date: 02/09/2017
ms.openlocfilehash: 569262aece2d8b1212704c0b2ba443c6c63f2b07
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100018409"
---
# <a name="how-to-configure-sql-server-unit-test-execution"></a>Procédure : Configurer l’exécution de test unitaire SQL Server

Lorsque vous configurez votre projet de test, vous spécifiez plusieurs paramètres qui contrôlent la façon dont vos tests unitaires SQL Server sont exécutés. Ces paramètres de configuration sont stockés dans le fichier app.config de votre projet de test. Si vous modifiez ce fichier directement, les nouvelles valeurs apparaissent dans la boîte de dialogue de configuration du test.  
  
Votre solution peut contenir plusieurs projets de test. Chaque projet de test contient un fichier app.config (autrement dit, un ensemble de paramètres de configuration). Par conséquent, votre solution peut contenir différents ensembles de tests unitaires (un ensemble pour chaque projet de test) qui sont configurés pour s'exécuter différemment.  
  
Ces paramètres contrôlent la façon dont votre test se connecte à la base de données que vous allez tester, le mode de déploiement d'un schéma de projet de base de données dans cette base de données :  
  
-   **Connexions de base de données**. Utilisez ce paramètre pour spécifier les chaînes de connexion utilisées pour la connexion à la base de données que vous testez. Pour plus d'informations, consultez [Spécifier des chaînes de connexion](#SpecifyConnectionStrings)  
  
-   **Déploiement du schéma**. Un projet de base de données est une représentation hors connexion de votre base de données. Le projet de base de données représente la structure de vos objets de base de données, mais ne contient pas de données. Après avoir apporté des modifications au schéma dans un projet de base de données, vous pouvez le tester dans une base de données réelle. Dans l'étape de déploiement du schéma, les objets de base de données que vous souhaitez tester sont copiés de votre projet de base de données dans la base de données sur laquelle vous exécutez des tests. Pour plus d'informations sur le déploiement du schéma, consultez [Déployer un schéma de base de données](#DeployingDBSchema).  
  
    > [!NOTE]  
    > Les tests ne s'exécutent pas dans le dossier de la solution, mais dans un dossier distinct sur le disque dur. Bien que vous puissiez configurer tous les aspects du déploiement de test, en général vous n'avez pas besoin de les configurer pour les tests unitaires. Pour plus d'informations sur le déploiement de test, consultez [Exécution de tests](/previous-versions/visualstudio/visual-studio-2010/dd286680(v=vs.100)).  
  
## <a name="specify-connection-strings"></a><a name="SpecifyConnectionStrings"></a>Spécifier des chaînes de connexion  
  
#### <a name="to-specify-database-connection-strings"></a>Pour spécifier les chaînes de connexion de base de données  
  
1.  Dans l'**Explorateur de solutions**, cliquez avec le bouton droit sur le projet de test unitaire, puis cliquez sur **Configuration de test SQL Server**.  
  
    La boîte de dialogue **Configuration de test SQL Server - «<projectname> »** 'affiche.  
  
2.  Sous **Connexions de base de données**, vous pouvez effectuer les opérations suivantes :  
  
    -   Cliquez sur la connexion de base de données dans laquelle vous souhaitez effectuer des tests unitaires.  
  
    -   Activez la case à cocher **Utiliser une connexion de données secondaire pour valider des tests unitaires**, puis cliquezsur une connexion de base de données dans la liste si vous souhaitez que l'exécution des tests soit validée par rapport à une autre connexion de base de données.  
  
    -   Cliquez sur **Nouvelle connexion** pour ajouter une connexion à une des listes. Vous pouvez également cliquer sur **Modifier la connexion** pour modifier les paramètres sur une connexion existante.  
  
    Cette étape crée la chaîne de connexion `ExecutionContext`, qui permet d'exécuter le script de test dans le test unitaire. Si vous spécifiez également une connexion secondaire, la chaîne de connexion `PrivilegedContext` est également créée. Cette connexion permet de tester les interactions avec la base de données à l'extérieur du script de test dans le test unitaire. Pour plus d’informations, consultez [Vue d’ensemble des chaînes de connexion et des autorisations](../ssdt/overview-of-connection-strings-and-permissions.md).  
  
3.  Cliquez sur **OK** pour fermer la boîte de dialogue **Configuration de test SQL Server -'« <projectname> »** .  
  
4.  Régénérez le projet de test pour appliquer les modifications de configuration.  
  
## <a name="deploy-a-database-schema"></a><a name="DeployingDBSchema"></a>Déployer un schéma de base de données  
  
#### <a name="to-deploy-to-a-database-the-schema-of-a-database-project"></a>Pour déployer dans une base de données le schéma d'un projet de base de données  
  
1.  Dans l'**Explorateur de solutions**, cliquez avec le bouton droit sur le projet de base de données, puis cliquez sur **Générer**.  
  
    Lorsque vous générez votre projet de base de données, vous créez un script Transact\-SQL. Ce script, lorsqu'il est exécuté sur une base de données, recrée la structure de votre projet de base de données dans cette base de données.  
  
2.  Sélectionnez le projet de test à configurer.  
  
3.  Dans l'**Explorateur de solutions**, cliquez avec le bouton droit sur le projet de test unitaire, puis cliquez sur **Configuration de test SQL Server**.  
  
    La boîte de dialogue **Configuration de test SQL Server - «<projectname> »** 'affiche.  
  
4.  Sous **Déploiement**, vous pouvez effectuer les opérations suivantes :  
  
    -   Activez la case à cocher **Déployer automatiquement les projets de base de données avant l'exécution des tests** pour vérifier que toutes les modifications de schéma apportées à votre projet de base de données sont validées avant exécution des tests.  
  
    -   Sous **Projet de base de données**, cliquez sur le projet de base de données que vous souhaitez déployer, ou cliquez sur les points de suspension pour rechercher un autre projet. Les fichiers projet de base de données portent une extension .dbproj.  
  
    -   Sous **Configuration du déploiement**, cliquez sur la configuration du projet dans lequel vous souhaitez effectuer le déploiement. Les options sont les suivantes : **Débogage**, **Par défaut** ou **Version finale**. Toutefois, si vous créez une configuration pour le test unitaire, cette configuration s'affiche également en tant qu'option.  
  
5.  Cliquez sur **OK** pour fermer la boîte de dialogue **Configuration de test SQL Server -'« <projectname> »** .  
  
    Au début d'une série de tests, le script Transact\-SQL généré à l'étape 1 s'exécute. Cette action déploie le schéma dans la base de données cible.  
  
6.  Régénérez le projet de test unitaire pour appliquer les modifications de configuration.  
  
## <a name="see-also"></a>Voir aussi  
[Création et définition de tests unitaires SQL Server](../ssdt/creating-and-defining-sql-server-unit-tests.md)  
[Vérifier le code de la base de données à l’aide de tests unitaires SQL Server](../ssdt/verifying-database-code-by-using-sql-server-unit-tests.md)  
