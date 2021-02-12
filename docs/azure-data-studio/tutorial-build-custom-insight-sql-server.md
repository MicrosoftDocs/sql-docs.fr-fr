---
title: Créer un widget insight personnalisé
description: Ce didacticiel montre comment créer des widgets d’insight personnalisés et les ajouter aux tableaux de bord de base de données et de serveur dans Azure Data Studio.
ms.prod: azure-data-studio
ms.technology: azure-data-studio
ms.topic: tutorial
author: markingmyname
ms.author: maghan
ms.reviewer: alayu
ms.custom: seodec18
ms.date: 08/26/2020
ms.openlocfilehash: bd4d3c12ea7a56ceddec0ad23f7276a4be4b9f40
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100039959"
---
# <a name="tutorial-build-a-custom-insight-widget"></a>Tutoriel : Créer un widget insight personnalisé

Ce didacticiel montre comment utiliser vos propres requêtes d’insight pour créer des widgets d’insight personnalisés.

Dans ce didacticiel, vous apprendrez à :
> [!div class="checklist"]
> * Exécuter votre propre requête et l’afficher dans un graphique
> * Créer un widget d’insight personnalisé à partir du graphique
> * Ajouter le graphique à un serveur ou un tableau de bord de base de données
> * Ajouter des détails à votre widget d’insight personnalisé

## <a name="prerequisites"></a>Prérequis

Ce didacticiel nécessite la base de données *TutorialDB* de SQL Server ou Azure SQL Database. Pour créer la base de données *TutorialDB*, suivez un des démarrages rapides suivants :

- [Se connecter à et interroger SQL Server avec [!INCLUDE[name-sos-short](../includes/name-sos-short.md)]](quickstart-sql-server.md)
- [Se connecter à et interroger Azure SQL Database avec [!INCLUDE[name-sos-short](../includes/name-sos-short.md)]](quickstart-sql-database.md)


## <a name="run-your-own-query-and-view-the-result-in-a-chart-view"></a>Exécuter votre propre requête et afficher le résultat dans une vue de graphique
À cette étape, exécutez un script SQL pour interroger les sessions actives actuelles.

1. Pour ouvrir un nouvel éditeur, appuyez sur **Ctrl+N**. 

2. Remplacez le contexte de connexion par **TutorialDB**.

3. Collez la requête ci-après dans l’éditeur de requête :

   ```sql
   SELECT count(session_id) as [Active Sessions]
   FROM sys.dm_exec_sessions
   WHERE status = 'running'
   ```

4. Enregistrez la requête dans l’éditeur dans un fichier \*.sql. Pour ce didacticiel, enregistrez le script sous le nom *activeSession.sql*.

5. Pour exécuter la requête, appuyez sur **F5**.

6. Une fois les résultats de la requête affichés, cliquez sur **Afficher en tant que graphique**, puis sur l’onglet **Visionneuse de graphique**.

7. Modifiez le **Type de graphique** en **nombre**. Ces paramètres affichent un graphique de nombres.

## <a name="add-the-custom-insight-to-the-database-dashboard"></a>Ajouter l’insight personnalisé au tableau de bord de la base de données

1. Pour ouvrir la configuration du widget d’insight, cliquez sur **Créer un aperçu** sur la *Visionneuse de graphique* :

   ![configuration](./media/tutorial-build-custom-insight-sql-server/create-insight.png)
   
2. Copiez la configuration d’insight (les données JSON). 

3. Appuyez sur **Ctrl+ virgule** pour ouvrir les *Paramètres utilisateur*.

4. Saisissez *tableau de bord* dans *Rechercher des paramètres*.

5. Cliquez sur **Modifier** pour *dashboard.database.widgets*.

   ![paramètres du tableau de bord](./media/tutorial-build-custom-insight-sql-server/dashboard-settings.png)

6. Collez le code JSON de la configuration Insight dans *dashboard.database.widgets*. Les paramètres du tableau de bord de base de données se présentent comme suit :

   ```json
    "dashboard.database.widgets": [
        {
            "name": "My-Widget",
            "gridItemConfig": {
                "sizex": 2,
                "sizey": 1
            },
            "widget": {
                "insights-widget": {
                    "type": {
                        "count": {
                            "dataDirection": "vertical",
                            "dataType": "number",
                            "legendPosition": "none",
                            "labelFirstColumn": false,
                            "columnsAsLabels": false
                        }
                    },
                    "queryFile": "{your file folder}/activeSession.sql"
                }
            }
        }
    ]
   ```

7. Enregistrez le fichier *Paramètres utilisateur* et ouvrez le tableau de bord de la base de données *TutorialDB* pour afficher le widget de sessions actives:

   ![session active insight tableau de bord](./media/tutorial-build-custom-insight-sql-server/insight-activesession-dashboard.png)

## <a name="add-details-to-custom-insight"></a>Ajouter des détails à un insight personnalisé

1. Pour ouvrir un nouvel éditeur, appuyez sur **Ctrl+N**.

2. Remplacez le contexte de connexion par **TutorialDB**.

3. Collez la requête ci-après dans l’éditeur de requête :

   ```sql
    SELECT session_id AS [SID], login_time AS [Login Time], host_name AS [Host Name], program_name AS [Program Name], login_name AS [Login Name]
    FROM sys.dm_exec_sessions
    WHERE status = 'running'
   ```

4. Enregistrez la requête dans l’éditeur dans un fichier \*.sql. Pour ce didacticiel, enregistrez le script sous le nom *activeSessionDetail.sql*.

5. Appuyez sur **Ctrl+ virgule** pour ouvrir les *Paramètres utilisateur*.

6. Modifiez le nœud *dashboard.database.widgets* existant dans votre fichier de paramètres :

   ```json
    "dashboard.database.widgets": [
        {
            "name": "My-Widget",
            "gridItemConfig": {
                "sizex": 2,
                "sizey": 1
            },
            "widget": {
                "insights-widget": {
                    "type": {
                        "count": {
                            "dataDirection": "vertical",
                            "dataType": "number",
                            "legendPosition": "none",
                            "labelFirstColumn": false,
                            "columnsAsLabels": false
                        }
                    },
                    "queryFile": "{your file folder}/activeSession.sql",
                    "details": {
                        "queryFile": "{your file folder}/activeSessionDetail.sql",
                        "label": "SID",
                        "value": "Login Name"
                    }
                }
            }
        }
    ]
   ```

7. Enregistrez le fichier *Paramètres utilisateur* et ouvrez le tableau de bord de la base de données *TutorialDB*. Cliquez sur le bouton de sélection (...) à côté de *My-Widget* pour afficher les détails :

    ![session active insight détail](./media/tutorial-build-custom-insight-sql-server/insight-activesession-detail.png)

## <a name="next-steps"></a>Étapes suivantes
Dans ce didacticiel, vous avez appris à :
> [!div class="checklist"]
> * Exécuter votre propre requête et l’afficher dans un graphique
> * Créer un widget d’insight personnalisé à partir du graphique
> * Ajouter le graphique à un serveur ou un tableau de bord de base de données
> * Ajouter des détails à votre widget d’insight personnalisé

Pour savoir comment sauvegarder et restaurer des bases de données, suivez le didacticiel suivant :

> [!div class="nextstepaction"]
> [Sauvegarder et restaurer des bases de données](tutorial-backup-restore-sql-server.md).
