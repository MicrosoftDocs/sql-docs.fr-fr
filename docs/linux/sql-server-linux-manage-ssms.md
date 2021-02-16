---
title: Utiliser SSMS pour gérer SQL Server sur Linux
description: Cet article présente SQL Server Management Studio, un environnement intégré permettant d’accéder aux composants de SQL Server pour les configurer, les gérer, les administrer et les développer.
author: VanMSFT
ms.author: vanto
ms.date: 05/21/2018
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
ms.assetid: b2fcf858-21c3-462a-8d49-50c85647d092
ms.openlocfilehash: 758c4e8165efd9fd24a979bfd3a1390e88b765ce
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100345641"
---
# <a name="use-sql-server-management-studio-on-windows-to-manage-sql-server-on-linux"></a>Utiliser SQL Server Management Studio sur Windows pour gérer SQL Server sur Linux

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

Cet article présente [SQL Server Management Studio (SSMS)](../ssms/sql-server-management-studio-ssms.md) et vous guide à travers quelques tâches courantes. SSMS est une application Windows. Utilisez donc SSMS quand vous disposez d’une machine Windows capable de se connecter à une instance distante sur Linux.

> [!TIP]
> Si vous n’avez pas de machine Windows sur laquelle exécuter SSMS, envisagez le nouveau [Azure Data Studio](../azure-data-studio/index.yml). Il fournit un outil graphique pour la gestion de SQL Server et s’exécute sur Linux et Windows.

[SQL Server Management Studio (SSMS)](../ssms/sql-server-management-studio-ssms.md) fait partie d’une suite d’outils SQL que Microsoft offre gratuitement pour vos besoins de développement et de gestion. SSMS est un environnement intégré permettant d'accéder aux composants de SQL Server en vue de les configurer, de les gérer, de les administrer et de les développer. Il peut se connecter à SQL Server s’exécutant sur n’importe quelle plateforme locale, dans des conteneurs Docker et dans le cloud. Il se connecte également à Azure SQL Database et à Azure Synapse Analytics. SSMS associe un large groupe d’outils graphiques à des éditeurs de script performants pour permettre aux développeurs et aux administrateurs de tous niveaux d’avoir accès à SQL Server.

SSMS offre un large éventail de fonctionnalités de développement et de gestion pour SQL Server, y compris des outils permettant de :

- configurer, surveiller et administrer plusieurs instances de SQL Server
- déployer, surveiller et mettre à niveau des composants de la couche Données, tels que les bases de données et les entrepôts de données
- sauvegarder et restaurer des bases de données
- générer et exécuter des requêtes et des scripts T-SQL et afficher les résultats
- générer des scripts T-SQL pour les objets de base de données
- afficher et modifier des données dans des bases de données
- concevoir visuellement des requêtes T-SQL et des objets de base de données tels que des affichages, des tableaux et des procédures stockées

Pour plus d'informations sur SSMS, consultez [Qu'est-ce que SSMS ?](../ssms/sql-server-management-studio-ssms.md).

## <a name="install-the-newest-version-of-sql-server-management-studio-ssms"></a>Installer la dernière version de SQL Server Management Studio (SSMS)

Lorsque vous utilisez SQL Server, vous devez toujours utiliser la version la plus récente de SQL Server Management Studio (SSMS). La dernière version de SSMS est continuellement mise à jour et optimisée et fonctionne actuellement avec SQL Server sur Linux. Pour télécharger et installer la dernière version, consultez [Télécharger SQL Server Management Studio (SSMS)](../ssms/download-sql-server-management-studio-ssms.md). Pour rester à jour, la dernière version de SSMS vous invite quand une nouvelle version est disponible au téléchargement.

> [!NOTE]
> Avant d’utiliser SSMS pour gérer Linux, consultez les [problèmes connus](sql-server-linux-release-notes.md) de SSMS sur Linux.

## <a name="connect-to-sql-server-on-linux"></a>Se connecter à SQL Server sur Linux

Utilisez les étapes de base suivantes pour vous connecter :

1. Démarrez SSMS en tapant **Microsoft SQL Server Management Studio** dans la zone de recherche Windows, puis cliquez sur l’application de bureau.

    ![SQL Server Management Studio](./media/sql-server-linux-manage-ssms/ssms.png)

1. Dans la fenêtre **Se connecter au serveur**, entrez les informations suivantes (si SSMS est déjà en cours d’exécution, cliquez sur **Se connecter > Moteur de base de données** pour ouvrir la fenêtre **Se connecter au serveur**) :

   | Paramètre | Description |
   |-----|-----|
   | **Type de serveur** | La valeur par défaut est moteur de base de données ; ne modifiez pas cette valeur. |
   | **Nom du serveur** | Entrez le nom de l’ordinateur SQL Server Linux cible, ou son adresse IP et son port au format `IP,port`. |
   | **Authentification** | Pour SQL Server sur Linux, utilisez l’**authentification SQL Server**. |
   | **Connexion** | Entrez le nom d’un utilisateur ayant accès à une base de données sur le serveur (par exemple, le compte **SA** par défaut créé lors de l’installation). |
   | **Mot de passe** | Entrez le mot de passe de l’utilisateur spécifié (pour le compte **SA**, vous l’avez créé lors de l’installation). |

    ![SQL Server Management Studio : Se connecter au serveur SQL Database](./media/sql-server-linux-manage-ssms/connect.png)

1. Cliquez sur **Connecter**.

    > [!TIP]
    > Si un échec de connexion s’affiche, tentez tout d’abord de diagnostiquer le problème à partir du message d’erreur. Examinez ensuite les [recommandations en matière de résolution des problèmes de connexion](sql-server-linux-troubleshooting-guide.md#connection).
 
1. Une fois connecté à votre SQL Server, l’**Explorateur d’objets** s’ouvre et vous pouvez maintenant accéder à votre base de données pour effectuer des tâches administratives ou interroger les données.

## <a name="run-transact-sql-queries"></a>Exécuter les requêtes Transact-SQL

Après vous être connecté à votre serveur, vous pouvez vous connecter à une base de données et exécuter des requêtes Transact-SQL. Les requêtes Transact-SQL peuvent être utilisées pour la plupart des tâches de base de données.

1. Dans l’**Explorateur d’objets**, naviguez jusqu’à la base de données cible sur le serveur. Par exemple, développez **Bases de données système** pour utiliser la base de données **MASTER**.

1. Cliquez avec le bouton de droite sur la base de données et sélectionnez **Nouvelle requête**.

1. Dans la fenêtre de requête, écrivez une requête Transact-SQL pour sélectionner le retour des noms de toutes les bases de données sur votre serveur.

   ```sql
   SELECT [Name]
   FROM sys.Databases
   ```

   Si vous ne connaissez pas l’écriture des requêtes, consultez [Écriture d’instructions Transact-SQL](../t-sql/tutorial-writing-transact-sql-statements.md).

1. Cliquez sur le bouton **Exécuter** pour exécuter la requête et consulter les résultats.

   ![Réussite. Se connecter au serveur SQL Database : SQL Server Management Studio](./media/sql-server-linux-manage-ssms/execute-query.png)

Bien qu’il soit possible d’effectuer quasiment n’importe quelle tâche de gestion avec des requêtes Transact-SQL, SSMS est un outil graphique qui facilite la gestion de SQL Server. Les sections suivantes fournissent des exemples d’utilisation de l’interface utilisateur graphique.

## <a name="create-and-manage-databases"></a>Créer et gérer des bases de données

Lorsque vous êtes connecté à la base de données *MASTER*, vous pouvez créer des bases de données sur le serveur et modifier ou déposer des bases de données existantes. Les étapes suivantes décrivent comment accomplir plusieurs tâches de gestion de bases de données courantes par le biais de Management Studio. Pour effectuer ces tâches, assurez-vous que vous êtes connecté à la base de données *MASTER* avec la connexion du principal au niveau du serveur que vous avez créée lors de la configuration de SQL Server sur Linux.

### <a name="create-a-new-database"></a>Créer une base de données

1. Démarrer SSMS et se connecter à votre serveur dans SQL Server sur Linux

2. Dans l’Explorateur d’objets, cliquez avec le bouton de droite sur le dossier *Bases de données* puis cliquez sur *Nouvelle base de données... »

3. Dans la boîte de dialogue *Nouvelle base de données*, entrez un nom pour votre nouvelle base de données, puis cliquez sur *OK*

La nouvelle base de données a été créée avec succès sur votre serveur. Si vous préférez créer une nouvelle base de données à l’aide de T-SQL, consultez [CREATE DATABASE (SQL Server Transact-SQL)](../t-sql/statements/create-database-transact-sql.md).

### <a name="drop-a-database"></a>Déposer une base de données

1. Démarrer SSMS et se connecter à votre serveur dans SQL Server sur Linux

2. Dans l’Explorateur d’objets, développez le dossier *Bases de données* pour afficher une liste de toutes les bases de données sur le serveur.

3. Dans l’Explorateur d’objets, cliquez avec le bouton de droite sur la base de données à déposer, puis cliquez sur *Supprimer*

4. Dans la boîte de dialogue *Supprimer l'objet*, cochez la case *Fermer les connexions existantes*, puis cliquez sur *OK*

La base de données a été déposée avec succès depuis votre serveur. Si vous préférez déposer une base de données à l’aide de T-SQL, consultez [DROP DATABASE (SQL Server Transact-SQL)](../t-sql/statements/drop-database-transact-sql.md).

## <a name="use-activity-monitor-to-see-information-about-sql-server-activity"></a>Utiliser le moniteur d’activité pour consulter des informations sur l’activité SQL Server

L'outil [Moniteur d’activité](../relational-databases/performance-monitor/activity-monitor.md) est intégré à SQL Server Management Studio (SSMS) et affiche des informations sur les processus de SQL Server et sur la façon dont ces processus affectent l’instance actuelle de SQL Server.

1. Démarrer SSMS et se connecter à votre serveur dans SQL Server sur Linux

1. Dans l’Explorateur d’objets, cliquez avec le bouton de droite sur le nœud *serveur*, puis cliquez sur *Moniteur d’activité*

Le moniteur d’activité affiche des volets extensibles et réductibles avec les informations suivantes :

- Vue d’ensemble
- Processus
- Attentes de ressources
- E/S du fichier de données
- Requêtes coûteuses récentes
- Requêtes coûteuses actives

Lorsqu'un volet est développé, le Moniteur d'activité interroge l'instance pour obtenir des informations. Lorsqu'un volet est réduit, toutes les activités d'interrogation cessent pour ce volet. Vous pouvez développer un ou plusieurs volets en même temps pour afficher différents types d’activité sur l’instance.

## <a name="see-also"></a>Voir aussi
- [Qu’est-ce que SSMS ?](../ssms/sql-server-management-studio-ssms.md)
- [Exporter et importer une base de données avec SSMS](sql-server-linux-migrate-ssms.md)
- [Tutoriel : SQL Server Management Studio](../ssms/quickstarts/ssms-connect-query-sql-server.md)
- [Tutoriel : Écriture d’instructions Transact-SQL](../t-sql/tutorial-writing-transact-sql-statements.md)
- [Analyse des performances et surveillance de l'activité du serveur](../relational-databases/performance/server-performance-and-activity-monitoring.md)