---
title: Ajout de fonctionnalités supplémentaires via l’extensibilité
description: Apprenez-en davantage sur le modèle d’extensibilité et les domaines d’extensibilité clés pour étendre les fonctionnalités d’Azure Data Studio
ms.prod: azure-data-studio
ms.technology: azure-data-studio
ms.topic: conceptual
author: markingmyname
ms.author: maghan
ms.reviewer: alayu, maghan, sstein
ms.custom: seodec18
ms.date: 09/24/2018
ms.openlocfilehash: 051e3afcd5f76624a1978c5ef3ef0f6a264fe07d
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/17/2020
ms.locfileid: "97637729"
---
# <a name="azure-data-studio-extensibility"></a>Extensibilité d’Azure Data Studio

Azure Data Studio dispose de plusieurs mécanismes d’extensibilité qui permettent de personnaliser l’expérience utilisateur et de rendre ces personnalisations accessibles à l’ensemble de la communauté d’utilisateurs. La plateforme Azure Data Studio principale repose sur Visual Studio Code, ainsi la plupart des API d’extensibilité de Visual Studio Code sont disponibles. En outre, nous avons fourni des points d’extensibilité supplémentaires pour des activités spécifiques à la gestion des données.

Voici quelques-uns des principaux points d’extensibilité :

- API d’extensibilité de Visual Studio Code
- Outils de création d’extensions Azure Data Studio
- Gestion des contributions du panneau des onglets du tableau de bord
- Insights avec les expériences Actions
- API d’extensibilité d’Azure Data Studio
- API Fournisseur de données personnalisées

## <a name="visual-studio-code-extensibility-apis"></a>API d’extensibilité de Visual Studio Code

Étant donné que la plateforme principale Azure Data Studio repose sur Visual Studio Code, vous trouverez des détails sur les API d’extensibilité de Visual Studio Code dans les documentations [Création d’extensions](https://code.visualstudio.com/docs/extensions/overview) et [API d’extension](https://code.visualstudio.com/docs/extensionAPI/overview) sur le site web Visual Studio Code.

> [!NOTE]
>  Les versions d’Azure Data Studio sont alignées avec une version récente de VS Code. Cependant, le moteur VS Code inclus ne correspond pas forcément à la version actuelle de VS Code. Par exemple, en novembre 2020, le moteur VS Code dans Azure Data Studio est sous la version 1.48 alors que la version actuelle de VS Code est 1.51.  Le message d’erreur « Impossible d’installer l’extension "<name>", car elle n’est pas compatible avec VS Code <version> », qui apparaît lors de l’installation d’une extension, est provoqué par une extension pour laquelle la version du moteur VS Code définie dans le manifeste du package est une version ultérieure (`package.json`). Vous pouvez vérifier la version du moteur VS Code dans Azure Data Studio par le biais du menu **Aide** sous **À propos de**.

## <a name="manage-dashboard-tab-panel-contributions"></a>Gestion des contributions du panneau des onglets du tableau de bord

Pour plus d’informations, consultez [Points de contribution](#contribution-points) et [Variables de contexte](#context-variables).

## <a name="azure-data-studio-extensibility-apis"></a>API d’extensibilité d’Azure Data Studio

Pour plus d’informations, consultez [API d’extensibilité](extensibility-apis.md).


## <a name="contribution-points"></a>Points de contribution

Cette section décrit les différents points de contribution définis dans le manifeste de l’extension package.json.

IntelliSense est pris en charge dans Azure Data Studio.

### <a name="dashboard-contribution-points"></a>Points de contribution du tableau de bord

Contribuez à un onglet, un conteneur et/ou un widget insight dans le tableau de bord.

![tableau de bord](media/extensibility/dashboard-page.png)

`dashboard.tabs`

Dashboard.tabs crée les sections d’onglets sur la page de tableau de bord. Il attend un objet ou un tableau d’objets,  

```json
"dashboard.tabs": [
{
    "id": "test-tab1",
    "title": "Test 1",
    "description": "The test 1 displays a list of widgets.",
    "when": "connectionProvider == 'MSSQL' && !mssql:iscloud",
    "alwaysShow": true,
    "container": {
        ...
    }
}
]
```

`dashboard.containers`

au lieu de spécifier le conteneur du tableau de bord inclus (dans le dashboard.tab). Vous pouvez inscrire des conteneurs à l’aide de dashboard.containers. Il accepte un objet ou un tableau d’objets.

```json
"dashboard.containers": [
{
    "id": "innerTab1",
    "container": {
        ...
    }
},
{
    "id": "innerTab2",
    "container": {
       ...
    }
}
]
```

Pour faire référence au conteneur inscrit, spécifiez l’ID du conteneur

```json
"dashboard.tabs": [
{
    ...
    "container": {
        "innerTab1": {             
        }
    }
}
]

```

`dashboard.insights`

Vous pouvez enregistrer des insights à l’aide de dashboard.insights. Cela est similaire au [Didacticiel : Créer un widget insight personnalisé](./tutorial-build-custom-insight-sql-server.md)

```json
"dashboard.insights": {
"id": "my-widget"
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
```


### <a name="dashboard-container-types"></a>Types de conteneurs de tableau de bord

Quatre types de conteneur sont actuellement pris en charge :

1. `widgets-container`

    ![Conteneur de widgets](media/extensibility/widgets-container.png)

    Liste des widgets qui seront affichés dans le conteneur. Il s’agit d’une mise en page fluide. Il accepte la liste des widgets.

    ```json
    "container": {
        "widgets-container": [
        {
            "widget": {
                "query-data-store-db-insight": {
                }
            }
        },
        {
            "widget": {
                "explorer-widget": {
                }
            }
        }
      ]
    }
    ```

2. `webview-container`

    ![Conteneur d’affichage web](media/extensibility/webview-container.png)

    La vue d’affichage web s’affichera dans l’intégralité du conteneur. Elle s’attend à ce que l’ID d’affichage web soit le même que l’ID de l’onglet

    ```json
    "container": {
        "webview-container": null
    }
    ```

3. `grid-container`

   ![Conteneur de grille](media/extensibility/grid-container.png)

   Liste des widgets ou des affichages web qui seront affichés dans la disposition de la grille

    ```json
    "container": {
        "grid-container": [
        {
            "name": "widget 1",
            "widget": {
                "explorer-widget": {
                }
            },
            "row":0,
            "col":0
        },
        {
            "name": "widget 2",
            "widget": {
                "tasks-widget": {
                    "backup", 
                    "restore",
                    "configureDashboard",
                    "newQuery"
                }
            },
            "row":0,
            "col":1
        },
        {
            "name": "Webview 1",
            "webview": {
                "id": "google"
            },
            "row":1,
            "col":0,
            "colspan":2
        },
        {
            "name": "widget 3",
            "widget": {
                "explorer-widget": {}
            },
            "row":0,
            "col":3,
            "rowspan":2
        },
    ]
    ```

4.  `nav-section`

    ![Section de navigation](media/extensibility/nav-section.png)

    La section de navigation s’affiche dans le conteneur

    ```json
    "container": {
        "nav-section": [
            {
                "id": "innerTab1",
                "title": "inner-tab1",
                "icon": {
                    "light": "./icons/tab1Icon.svg",
                    "dark": "./icons/tab1Icon_dark.svg"
                },
                "container": {
                    ...
                }
            },
            {
                "id": "innerTab2",
                "title": "inner-tab2",
                "icon": {
                    "light": "./icons/tab2Icon.svg",
                    "dark": "./icons/tab2Icon_dark.svg"
                },
                "container": {
                    ...
                }
            }
        ]
    }
    ```



## <a name="context-variables"></a>Variables de contexte

Pour obtenir des informations générales sur le contexte dans Visual Studio Code et Azure Data Studio, consultez [Extensibilité](https://code.visualstudio.com/docs/extensionAPI/extension-points#_example).

Dans Azure Data Studio, nous avons un contexte spécifique pour les connexions de base de données disponibles pour les extensions.

### <a name="dashboard"></a>tableau de bord

Dans le tableau de bord, nous fournissons les variables de contexte suivantes :

|Variable de contexte| description|
|:---|:---|
|`connectionProvider` | Chaîne de l’identificateur pour le fournisseur de la connexion actuelle. Ex. `connectionProvider == 'MSSQL'`.|
|`serverName`|Chaîne de nom du serveur de la connexion actuelle. Ex. `serverName == 'localhost'`.|
|`databaseName` | Chaîne de nom de la base de données de la connexion actuelle. Ex. `databaseName == 'master'`.|
|`connection` | Objet de profil de connexion complet pour la connexion actuelle (IConnectionProfile)|
|`dashboardContext` | Une chaîne de contexte de la page du tableau de bord est actuellement activée. Il s’agit de « Base de données » ou de « Serveur ». Ex. `dashboardContext == 'database'`|