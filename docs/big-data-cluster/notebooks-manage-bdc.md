---
title: 'Gérer : Notebooks Azure Data Studio'
titleSuffix: SQL Server Big Data Clusters
description: Utilisez un notebook d’Azure Data Studio pour gérer et dépanner les clusters Big Data SQL Server.
author: yualan
ms.author: alanyu
ms.reviewer: mikeray
ms.metadata: seo-lt-2019
ms.date: 03/30/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: a526f4d1da472a8d5bae9c5bceb296978d24986b
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/01/2021
ms.locfileid: "99235707"
---
# <a name="manage-sql-server-big-data-clusters-with-azure-data-studio-notebooks"></a>Gérer des clusters Big Data SQL Server avec des notebooks Azure Data Studio

[!INCLUDE[SQL Server 2019](../includes/applies-to-version/sqlserver2019.md)]

[!INCLUDE[sql-server-2019](../includes/sssql19-md.md)] fournit une extension pour Azure Data Studio qui inclut des notebooks. Un notebook fournit de la documentation et du code que vous pouvez utiliser dans Azure Data Studio pour gérer des clusters Big Data SQL Server 2019.

Initialement implémentés en tant que projet open source, les [notebooks](../azure-data-studio/notebooks/notebooks-guidance.md) ont été incorporés dans [Azure Data Studio](../azure-data-studio/download-azure-data-studio.md). Vous pouvez utiliser un marquage Markdown pour le texte des cellules de texte et un des noyaux disponibles pour écrire du code dans les cellules de code.

Vous pouvez utiliser des notebooks pour déployer des clusters Big Data pour [!INCLUDE[sql-server-2019](../includes/sssql19-md.md)].

En plus des notebooks, vous pouvez voir une collection de notebooks, qui est appelée « book Jupyter ». Un book Jupyter fournit une table des matières pour vous aider à naviguer dans une collection de notebooks : vous pouvez ainsi trouver facilement le notebook dont vous avez besoin si vous voulez résoudre un problème SQL Server ou voir l’état du cluster.

## <a name="prerequisites"></a>Prérequis

Vous avez besoin de ces prérequis pour ouvrir un notebook :

* Version la plus récente d’[Azure Data Studio](../azure-data-studio/download-azure-data-studio.md)
* L’extension [!INCLUDE[sql-server-2019](../includes/sssql19-md.md)] installée dans Azure Data Studio

En plus de ces prérequis, pour déployer des clusters Big Data SQL Server 2019, vous avez aussi besoin de :

* [azdata](../azdata/install/deploy-install-azdata.md)
* [kubectl](https://kubernetes.io/docs/tasks/tools/install-kubectl/#install-kubectl-binary-using-native-package-management)
* [Azure CLI](/cli/azure/install-azure-cli)

## <a name="access-troubleshooting-notebooks"></a>Accéder aux notebooks de résolution des problèmes

Il existe trois façons d’accéder aux notebooks de résolution des problèmes.

### <a name="command-palette"></a>Palette de commandes

1. Sélectionnez **Afficher** > **Palette de commandes**.

2. Entrez **Books Jupyter : Guide SQL Server 2019**.

La viewlet Jupyter Books avec le book Jupyter qui contient les notebooks de résolution des problèmes liés à des clusters Big Data SQL Server s’ouvre.

### <a name="sql-master-dashboard"></a>Tableau de bord principal SQL

1. Après avoir installé Azure Data Studio Insiders, connectez-vous à une instance de cluster Big Data SQL Server.

2. Une fois que vous êtes connecté à l’instance, cliquez avec le bouton droit sur le nom de votre serveur, sous **CONNEXIONS** et sélectionnez **Gérer**.

3. Dans le tableau de bord, sélectionnez **Cluster Big Data SQL Server**. Sélectionnez **Guide SQL Server 2019** pour ouvrir le book Jupyter contenant les notebooks dont vous avez besoin.
    ![Notebooks Jupyter dans le tableau de bord](media/manage-notebooks/jupyter-book-button.png)

4. Sélectionnez le notebook pour la tâche que vous devez effectuer.

### <a name="controller-dashboard"></a>Tableau de bord du contrôleur

1. Dans la vue **Connexions**, développez **Clusters Big Data SQL Server**.

2. Ajoutez les informations du point de terminaison du contrôleur.

3. Une fois que vous êtes connecté au contrôleur, cliquez avec le bouton droit sur le point de terminaison, puis sélectionnez **Gérer**.

4. Une fois le tableau de bord chargé, sélectionnez **résoudre les problèmes** pour ouvrir les guides de résolution des problèmes du book Jupyter.

## <a name="use-troubleshooting-notebooks"></a>Utiliser les notebooks de résolution des problèmes

1. Recherchez le guide de résolution des problèmes dont vous avez besoin dans la table des matières du book Jupyter.

2. Les notebooks étant optimisés, vous devez simplement sélectionner **Run Cells** (Exécuter les cellules). Cette action exécute chaque cellule du notebook individuellement jusqu’à ce que le notebook soit terminé.

3. Si une erreur est détectée, le book Jupyter suggère un notebook que vous pouvez exécuter pour corriger l’erreur. Suivez les étapes recommandées, puis réexécutez le notebook.

## <a name="change-the-big-data-cluster"></a>Modifier le cluster Big Data

Pour modifier le cluster Big Data SQL Server d’un notebook :

1. Cliquez sur le menu **Attacher à** dans la barre d’outils du notebook.

   ![Cliquer sur le menu Attacher à dans la barre d’outils du notebook](./media/notebooks-how-to-manage/select-attach-to-1.png)

2. Cliquez sur un serveur dans le menu **Attacher à**.

   ![Sélectionner un serveur dans le menu Attacher à](./media/notebooks-how-to-manage/select-attach-to-2.png)

## <a name="next-steps"></a>Étapes suivantes

Pour plus d’informations sur les notebooks dans Azure Data Studio, consultez [Guide pratique pour utiliser des notebooks avec SQL Server](../azure-data-studio/notebooks/notebooks-guidance.md).