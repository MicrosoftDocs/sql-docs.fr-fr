---
title: Extension Kusto (KQL) pour Azure Data Studio
description: Cet article explique comment vous pouvez connecter et interroger les clusters Azure Data Explorer avec Azure Data Studio.
ms.prod: azure-data-studio
ms.technology: azure-data-studio
ms.topic: conceptual
author: markingmyname
ms.author: maghan
ms.reviewer: jukoesma
ms.custom: ''
ms.date: 10/29/2020
ms.openlocfilehash: 15b2dc331a1387039c79cbab164682f00981c591
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100052030"
---
# <a name="kusto-kql-extension-for-azure-data-studio-preview"></a>Extension Kusto (KQL) pour Azure Data Studio (préversion)

L’extension Kusto (KQL) pour [Azure Data Studio](../what-is-azure-data-studio.md) vous permet de connecter et d’interroger les [clusters Azure Data Explorer](/azure/data-explorer/data-explorer-overview).

Les utilisateurs peuvent écrire et exécuter des requêtes KQL et créer des notebooks avec le [noyau Kusto](../notebooks/notebooks-kusto-kernel.md) terminé avec IntelliSense.

En activant l’expérience Kusto (KQL) native dans Azure Data Studio, les ingénieurs de données, les scientifiques des données et les analystes de données peuvent rapidement observer des tendances et des anomalies par rapport aux grandes quantités de données stockées dans Azure Data Explorer.

Cette extension est actuellement en préversion.

## <a name="prerequisites"></a>Prérequis

Si vous n’avez pas d’abonnement Azure, créez un [compte Azure gratuit](https://azure.microsoft.com/free/) avant de commencer.

Les prérequis suivants sont également obligatoires :

- [Azure Data Studio installé](../download-azure-data-studio.md).
- [Un cluster et une base de données Azure Data Explorer](/azure/data-explorer/create-cluster-database-portal).

## <a name="install-the-kusto-kql-extension"></a>Installer l’extension Kusto (KQL)

Pour installer l’extension Kusto (KQL) dans Azure Data Studio, procédez comme suit.

1. Ouvrez le gestionnaire d’extensions dans Azure Data Studio. Vous pouvez sélectionner l’icône des extensions ou l’option **Extensions** dans le menu Affichage.

2. Entrez *Kusto* dans la barre de recherche.

3. Sélectionnez l’extension **Kusto (KQL)** , puis affichez les détails associés.

4. Sélectionnez **Installer**.

:::image type="content" source="media/kusto-extension/kusto-extension-icon.png" alt-text="Extension Kusto":::

## <a name="how-to-connect-to-an-azure-data-explorer-cluster"></a>Comment se connecter à un cluster Azure Data Explorer

### <a name="find-your-azure-data-explorer-cluster"></a>Localiser votre cluster Azure Data Explorer

Localisez votre cluster Azure Data Explorer dans le [portail Azure](https://ms.portal.azure.com/#home), puis recherchez l’URI du cluster.

:::image type="content" source="media/kusto-extension/kusto-extension-adx-cluster-uri.png" alt-text="URI":::

Toutefois, vous pouvez immédiatement utiliser le cluster *help.kusto.windows.net*.

Pour cet article, nous utilisons les données du cluster help.kusto.windows.net pour obtenir des exemples.

### <a name="connection-details"></a>Détails de la connexion

Pour configurer un cluster Azure Data Explorer auquel se connecter, procédez comme suit.

1. Sélectionnez **Nouvelle connexion** dans le volet **Connexions**.

2. Remplissez les informations de la section **Détails de la connexion**.
    1. Pour **Type de connexion**, sélectionnez *Kusto*.
    2. Pour **Cluster**, entrez le nom de votre cluster Azure Data Explorer.

        > [!Note]
        > Lorsque vous entrez le nom du cluster, n’incluez pas le préfixe `https://` ou un `/`de fin.

    3. Pour **Type d’authentification**, utilisez le type *Azure Active Directory - Universel avec un compte MFA* par défaut.
    4. Pour **Compte**, utilisez les informations de votre compte.
    5. Pour **Base de données**, utilisez *Par défaut*.
    6. Pour **Groupe de serveurs**, utilisez *Par défaut*.
        1. Vous pouvez utiliser ce champ pour organiser vos serveurs dans un groupe spécifique.
    7. Laissez le champ **Nom (facultatif)** vide.
        1. Vous pouvez utiliser ce champ pour attribuer un alias à votre serveur.

    :::image type="content" source="media/kusto-extension/kusto-extension-connection-details.png" alt-text="Détails de la connexion":::

## <a name="how-to-query-an-azure-data-explorer-database-in-azure-data-studio"></a>Comment interroger une base de données Azure Data Explorer dans Azure Data Studio

Maintenant que vous avez configuré une connexion à votre cluster Azure Data Explorer, vous pouvez interroger vos bases de données à l’aide de Kusto (KQL).

Pour créer un onglet de requête, vous pouvez sélectionner **Fichier > Nouvelle requête**, utiliser les touches *Ctrl + N*, ou cliquer avec le bouton droit de la souris sur la base de données et sélectionner **Nouvelle requête**.

Une fois l’onglet de création d’une nouvelle requête ouvert, entrez votre requête Kusto.

Voici quelques exemples de requêtes KQL :

```kusto
StormEvents
| limit 1000
```

```kusto
StormEvents
| where EventType == "Waterspout"
```

Pour plus d’informations sur l’écriture de requêtes KQL, consultez [Écrire des requêtes pour Azure Data Explorer](/azure/data-explorer/write-queries#overview-of-the-query-language)

## <a name="view-extension-settings"></a>Consulter les paramètres de l’extension

Pour modifier les paramètres de l’extension Kusto, procédez comme suit.

1. Ouvrez le gestionnaire d’extensions dans Azure Data Studio. Vous pouvez sélectionner l’icône des extensions ou l’option **Extensions** dans le menu Affichage.

2. Localisez l’extension **Kusto (KQL)** .

3. Sélectionnez l’icône **Gérer**.

4. Sélectionnez l’icône **Paramètres de l’extension**.

Vos paramètres d’extension ressemblent à ceci :

:::image type="content" source="media/kusto-extension/kusto-extension-settings.png" alt-text="Paramètres de l’extension Kusto (KQL)":::

## <a name="sanddance-visualization"></a>Visualisation SandDance

L’extension [SandDance](sanddance-extension.md) et l’extension Kusto (KQL) dans Azure Data Studio offrent une visualisation interactive riche. Dans le jeu de résultats de la requête KQL, sélectionnez le bouton **Visualiseur** pour lancer [SandDance](https://sanddance.js.org/).

:::image type="content" source="media/kusto-extension/kusto-extension-sanddance-demo.gif" alt-text="Visualisation SandDance":::

## <a name="known-issues"></a>Problèmes connus

| Détails | Solution de contournement |
|---------|------------|
| [Dans un notebook Kusto, le changement d’une connexion de base de données sur une connexion d’alias enregistrée est bloqué après une erreur d’exécution d’une cellule de code](https://github.com/microsoft/azuredatastudio/issues/12384) | Fermez et rouvrez le notebook, puis connectez-vous au cluster approprié avec la base de données. |
| [Dans un notebook Kusto, le changement d’une connexion de base de données sur une connexion d’alias non enregistrée ne fonctionne pas](https://github.com/microsoft/azuredatastudio/issues/12843) |Créez une connexion à partir du viewlet de connexion et enregistrez-la avec un alias. Créez ensuite un notebook et connectez-vous à la connexion nouvellement enregistrée. | 
| [Dans le notebook Kusto, la liste déroulante de bases de données n’est pas remplie lors de la création d’une connexion ADX](https://github.com/microsoft/azuredatastudio/issues/12666) | Créez une connexion à partir du viewlet de connexion et enregistrez-la avec un alias. Créez ensuite un notebook et connectez-vous à la connexion nouvellement enregistrée. |

Vous pouvez envoyer une [requête de fonctionnalités](https://github.com/microsoft/azuredatastudio/issues/new?assignees=&labels=&template=feature_request.md&title=) pour fournir des commentaires à l’équipe produit.  
Vous pouvez signaler un [bogue](https://github.com/microsoft/azuredatastudio/issues/new?assignees=&labels=&template=bug_report.md&title=) pour fournir des commentaires à l’équipe produit.

## <a name="next-steps"></a>Étapes suivantes

- [Créer et exécuter un notebook Kusto](../notebooks/notebooks-kusto-kernel.md)
- [Notebook Kqlmagic dans Azure Data Studio](../notebooks/notebooks-kqlmagic.md)
- [Aide-mémoire SQL vers Kusto](/azure/data-explorer/kusto/query/sqlcheatsheet)
- [Qu’est-ce que l’Explorateur de données Azure ?](/azure/data-explorer/data-explorer-overview)
- [Utilisation des visualisations SandDance](https://sanddance.js.org/)
