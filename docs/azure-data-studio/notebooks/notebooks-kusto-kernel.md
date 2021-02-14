---
title: Notebooks avec noyau Kusto dans Azure Data Studio
description: Ce tutoriel vous montre comment créer et exécuter un notebook Kusto.
ms.topic: how-to
ms.prod: azure-data-studio
ms.technology: azure-data-studio
author: markingmyname
ms.author: maghan
ms.reviewer: jukoesma
ms.custom: ''
ms.date: 09/22/2020
ms.openlocfilehash: 1362e6a5ccd6152a1cb2597076d25c2da4149fd0
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100048229"
---
# <a name="create-and-run-a-kusto-kql-notebook-preview"></a>Créer et exécuter un notebook Kusto (KQL) (Préversion)

Cet article vous montre comment créer et exécuter un [notebook Azure Data Studio](./notebooks-guidance.md) à l’aide de l’[extension Kusto (KQL)](../extensions/kusto-extension.md), en vous connectant à un cluster Azure Data Explorer.

Avec l’extension Kusto (KQL), vous pouvez choisir l’option de noyau **Kusto**.

Actuellement, cette fonctionnalité est uniquement disponible en tant que version préliminaire.

## <a name="prerequisites"></a>Prérequis

Si vous n’avez pas d’abonnement Azure, créez un [compte Azure gratuit](https://azure.microsoft.com/free/) avant de commencer.

- [Un cluster Azure Data Explorer avec une base de données à laquelle vous pouvez vous connecter](/azure/data-explorer/create-cluster-database-portal).
- [Azure Data Studio](../download-azure-data-studio.md)
- [Extension Kusto (KQL) pour Azure Data Studio](../extensions/kusto-extension.md).

## <a name="create-a-kusto-kql-notebook"></a>Créer un notebook Kusto (KQL)

Les étapes suivantes montrent comment créer un fichier de notebook dans Azure Data Studio :

1. Dans Azure Data Studio, connectez-vous à votre cluster Azure Data Explorer.

2. Accédez au volet **Connexions** et, dans la fenêtre **Serveurs**, cliquez avec le bouton droit de la souris sur la base de données Kusto et sélectionnez *Nouveau notebook*. Vous pouvez également accéder à **Fichier** > **Nouveau Notebook**.

   :::image type="content" source="media/notebooks-kusto-kernel/kusto-new-notebook.png" alt-text="Ouvrir un notebook":::

3. Pour **Noyau**, sélectionnez *Kusto*. Vérifiez que le menu **Attacher à** est défini sur le nom et la base de données du cluster. Pour cet article, nous utilisons le cluster help.kusto.windows.net avec des exemples de données de la base de données.

   :::image type="content" source="media/notebooks-kusto-kernel/set-kusto-kernel.png" alt-text="Définir Noyau et Attacher à":::

Vous pouvez enregistrer le notebook à l’aide de la commande **Enregistrer** ou **Enregistrer sous** du menu **Fichier**.

Pour ouvrir un notebook, vous pouvez utiliser la commande **Ouvrir le fichier** du menu **Fichier**, sélectionner **Ouvrir le fichier** dans la page **Bienvenue**, ou utiliser la commande **Fichier : Ouvrir** dans la palette de commandes.

## <a name="change-the-connection"></a>Changer la connexion

Pour changer la connexion Kusto pour un notebook :

1. Sélectionnez le menu **Attacher à** dans la barre d’outils du notebook, puis sélectionnez **Changer la connexion**.

   :::image type="content" source="media/notebooks-kusto-kernel/kusto-select-attach-to-change-connections.png" alt-text="modifier les connexions":::

   > [!Note]
   > Assurez-vous que la valeur de la base de données est remplie. La base de données doit être spécifiée pour les notebooks Kusto.

2. Vous pouvez maintenant sélectionner un serveur de connexion récent ou entrer de nouveaux détails de connexion pour vous connecter.

   :::image type="content" source="media/notebooks-kusto-kernel/kusto-change-connection-cluster.png" alt-text="Sélectionnez un autre cluster":::

   > [!Note]
   > Spécifiez le nom du cluster sans `https://`.

## <a name="run-a-code-cell"></a>Exécuter une cellule de code

Vous pouvez créer des cellules contenant des requêtes KQL que vous pouvez exécuter en cliquant sur le bouton **Exécuter la cellule** situé à gauche de la cellule. Les résultats sont affichés dans le notebook après l’exécution de la cellule.

Par exemple :

1. Ajoutez une nouvelle cellule de code en sélectionnant la commande **+ Code** dans la barre d’outils.

   :::image type="content" source="media/notebooks-kusto-kernel/kusto-kernel-code.png" alt-text="Bloc de code du noyau Kusto":::

2. Copiez et collez l’exemple suivant dans la cellule, puis cliquez sur **Exécuter la cellule**. Cet exemple interroge les données StormEvents pour un type d’événement spécifique.

   ```kusto
    StormEvents
    | where EventType == "Waterspout"
   ```

   :::image type="content" source="media/notebooks-kusto-kernel/run-kusto-notebook-cell.png" alt-text="Exécuter la cellule":::

## <a name="save-the-result-or-show-chart"></a>Enregistrer le résultat ou afficher le graphique

Si vous exécutez un script qui retourne un résultat, vous pouvez enregistrer ce résultat dans différents formats à l’aide de la barre d’outils affichée au-dessus du résultat.

- Enregistrer au format CSV
- Enregistrer au format Excel
- Enregistrer au format JSON
- Enregistrer au format XML
- Afficher le graphique

```kusto
    StormEvents
    | limit 10
```

:::image type="content" source="media/notebooks-kusto-kernel/run-notebook-save-results.png" alt-text="Enregistrer le résultat":::

## <a name="known-issues"></a>Problèmes connus

| Détails | Solution de contournement |
|---------|------------|
| [Le résultat de la requête affiche uniquement les en-têtes de colonnes](https://github.com/microsoft/azuredatastudio/issues/12565). | N/A |

Vous pouvez envoyer une [requête de fonctionnalités](https://github.com/microsoft/azuredatastudio/issues/new?assignees=&labels=&template=feature_request.md&title=) pour fournir des commentaires à l’équipe produit.  
Vous pouvez signaler un [bogue](https://github.com/microsoft/azuredatastudio/issues/new?assignees=&labels=&template=bug_report.md&title=) pour fournir des commentaires à l’équipe produit.

## <a name="next-steps"></a>Étapes suivantes

Découvrez-en plus sur les notebooks :

- [Extension Kusto (KQL) pour Azure Data Studio](../extensions/kusto-extension.md)
- [Guide pratique pour utiliser les notebooks dans Azure Data Studio](./notebooks-guidance.md)
- [Créer et exécuter un notebook Python](./notebooks-python-kernel.md)
- [Créer et exécuter un notebook SQL Server](./notebooks-sql-kernel.md)