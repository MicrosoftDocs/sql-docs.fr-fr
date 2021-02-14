---
title: Générer et déployer dans une base de données locale
description: En savoir plus sur l’instance de serveur local fournie par SQL Server 2012. Découvrez comment utiliser cette instance pour générer, tester et déboguer des projets de développement.
ms.prod: sql
ms.technology: ssdt
ms.topic: conceptual
ms.assetid: ebca8ff8-9a09-4207-8979-9d577af7c1d5
author: markingmyname
ms.author: maghan
ms.reviewer: “”
ms.custom: seo-lt-2019
ms.date: 02/09/2017
ms.openlocfilehash: 598c75f5ef502d1679bd8ed7f17cb44c0f536abc
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100063534"
---
# <a name="how-to-build-and-deploy-to-a-local-database"></a>Procédure : Générer et déployer dans une base de données locale

Microsoft SQL Server 2012 fournit une instance de serveur local à la demande, appelée SQL Server Express Local Database Runtime, qui est activée lorsque vous déboguez un projet de base de données SQL Server. Cette instance de serveur local peut être utilisée en tant que sandbox pour la création, le test et le débogage de votre projet. Elle est indépendante des instances SQL Server installées et n'est pas accessible en dehors de SQL Server Data Tools (SSDT). Cette organisation convient pour les développeurs qui ont un accès limité ou aucun accès aux bases de données de production, mais souhaiteraient tester les projets localement avant de les remettre au personnel autorisé qui les déploiera en production. En outre, lorsque vous développez une solution de base de données pour SQL Azure, vous pouvez utiliser les fonctionnalités fournies par ce serveur local pour développer et tester votre projet de base de données localement, avant de le déployer dans le nuage.  
  
> [!WARNING]  
> Une base de données sous le nœud de la base de données locale dans l'Explorateur d'objets SQL Server est une réflexion du projet de base de données correspondant et n'est pas associée à la base de données du même nom dans une instance de serveur connecté.  
  
> [!WARNING]  
> Les procédures suivantes utilisent les entités créées dans les procédures précédentes des sections [Développement de base de données connectée](../ssdt/connected-database-development.md) et [Développement de base de données hors connexion orienté projet](../ssdt/project-oriented-offline-database-development.md).  
  
### <a name="to-use-the-local-database"></a>Pour utiliser la base de données locale  
  
1.  Notez que dans l’**Explorateur d’objets SQL Server**, sous le nœud **SQL Server**, un nouveau nœud nommé **Local** s’affiche. Il s'agit de l'instance de base de données locale.  
  
2.  Développez les nœuds **Local** et **Bases de données**. Notez l'apparence d'une base de données portant le même nom que le projet TradeDev. Développez les nœuds sous cette base de données. La fenêtre **Opérations des outils de données** affiche l'état des opérations d'expansion/d’importation en cours sur une base de données dans le nœud **Local**. Notez qu'ils ne contiennent aucune des tables et entités créées au cours des procédures précédentes.  
  
3.  Appuyez sur F5 pour déboguer le projet de base de données TradeDev.  
  
    Par défaut, SSDT utilise l'instance du serveur de base de données local pour déboguer les projets de base de données. Dans ce cas, SSDT tente dans un premier temps de générer le projet, et si aucune erreur n'est détectée, le projet (et ses entités) sera déployé dans la base de données locale. Si vous déboguez le même projet ultérieurement, SSDT détectera les modifications apportées depuis votre dernière session de débogage, et déploiera uniquement les modifications dans la base de données locale.  
  
4.  Dans le serveur de base de données **Local**, redéveloppez les nœuds sous **TradeDev**. Cette fois-ci, notez que les tables, les affichages et les fonctions ont été déployés sur le serveur de base de données local.  
  
5.  Cliquez avec le bouton droit sur le nœud **TradeDev** et sélectionnez **Nouvelle requête**.  
  
6.  Dans le volet de script, collez le code suivant et cliquez sur le bouton **Exécuter la requête** pour exécuter la requête.  
  
    ```  
    select * from dbo.GetProductsBySupplier(1)  
    ```  
  
7.  Le volet de **message** affiche (0 ligne(s) affectée(s)), et le volet de **résultats** ne retourne aucune ligne. En effet, vous exécutez une requête par rapport à la base de données locale au lieu d'utiliser la base de données connectée qui contient des données réelles.  
  
    Pour vérifier ce point, cliquez avec le bouton droit sur la table **Products** sous cette base de données **TradeDev** locale, puis sélectionnez **Afficher les données**. Notez que la table est vide.  
  
### <a name="to-replicate-real-data-to-the-local-database"></a>Pour répliquer des données réelles dans la base de données locale  
  
1.  Dans l’**Explorateur d’objets SQL Server**, développez votre instance SQL Server connectée et recherchez la base de données **TradeDev**.  
  
    Cliquez avec le bouton droit sur la table **Suppliers** et sélectionnez **Afficher les données**.  
  
2.  Cliquez sur le bouton **Script** (le deuxième à partir de la droite) dans la partie supérieure de l'Éditeur de données. Copiez les instructions `INSERT` du script.  
  
3.  Développez l'instance de serveur **Local** et cliquez avec le bouton droit sur le nœud **TradeDev**, puis sélectionnez **Nouvelle requête**.  
  
4.  Collez les instructions `INSERT` dans cette fenêtre de code et exécutez la requête.  
  
5.  Répétez les étapes ci-dessus pour répliquer les données des tables **Products** et **Fruits** de la base de données **TradeDev** connectée dans la base de données **TradeDev** locale.  
  
6.  Cliquez avec le bouton droit sur l'instance de serveur **Local** et sélectionnez **Actualiser**. Examinez les tables à l'aide d’**Afficher les données** pour vérifier que la base de données locale a été remplie.  
  
7.  Cliquez avec le bouton droit sur le nœud **TradeDe** de l'instance de serveur Local et sélectionnez **Nouvelle requête**.  
  
8.  Dans le volet de script, collez le code suivant et cliquez sur le bouton **Exécuter la requête** pour exécuter la requête.  
  
    ```  
    select * from dbo.GetProductsBySupplier(1)  
    ```  
  
9. Dans le volet **Résultats** sous le volet de l'Éditeur Transact\-SQL, vous verrez que les lignes Apples et Potato Chips de la table `Products` sont retournées.  
  
