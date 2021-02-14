---
title: Notebooks avec noyau SQL dans Azure Data Studio
description: Ce tutoriel vous montre comment créer et exécuter un notebook SQL Server.
ms.topic: how-to
ms.prod: azure-data-studio
ms.technology: azure-data-studio
author: markingmyname
ms.author: maghan
ms.reviewer: mikeray, alayu
ms.custom: ''
ms.date: 07/01/2020
ms.openlocfilehash: d2d04be4d571b14445abfb638f070b3666ce9984
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100040279"
---
# <a name="create-and-run-a-sql-server-notebook"></a>Créer et exécuter un notebook SQL Server

[!INCLUDE[SQL Server 2019](../../includes/applies-to-version/sqlserver2019.md)]

Ce tutoriel montre comment créer et exécuter un notebook dans Azure Data Studio à l’aide de SQL Server.

## <a name="prerequisites"></a>Prérequis

- [Azure Data Studio installé](../download-azure-data-studio.md)
- SQL Server installé
  - [Windows](../../database-engine/install-windows/install-sql-server.md)
  - [Linux](../../linux/sql-server-linux-setup.md)

## <a name="create-a--notebook"></a>Créer un notebook

Les étapes suivantes montrent comment créer un fichier de notebook dans Azure Data Studio :

1. Dans Azure Data Studio, connectez-vous à votre serveur SQL Server.

2. Effectuez la sélection sous **Connexions** dans la fenêtre **Serveurs**. Sélectionnez ensuite **Nouveau notebook**.

3. Attendez que **Noyau** et le contexte cible (**Attacher à**) soient remplis. Vérifiez que **Noyau** indique **SQL**, puis définissez **Attacher à** avec votre serveur SQL Server (dans cet exemple, il s’agit de *localhost*).

   ![Définir Noyau et Attacher à](media/notebooks-sql-kernel/set-kernel-and-attach-to.png)

Vous pouvez enregistrer le notebook à l’aide de la commande **Enregistrer** ou **Enregistrer sous** du menu **Fichier**.

Pour ouvrir un notebook, vous pouvez utiliser la commande **Ouvrir le fichier** du menu **Fichier**, sélectionner **Ouvrir le fichier** dans la page **Bienvenue**, ou utiliser la commande **Fichier : Ouvrir** dans la palette de commandes.

## <a name="change-the-sql-connection"></a>Changer la connexion SQL

Pour changer la connexion SQL pour un notebook :

1. Sélectionnez le menu **Attacher à** dans la barre d’outils du notebook, puis sélectionnez **Changer la connexion**.

   ![Sélectionnez le menu Attacher à dans la barre d’outils du notebook](./media/notebooks-sql-kernel/select-attach-to-1.png)

2. Vous pouvez maintenant sélectionner un serveur de connexion récent ou entrer de nouveaux détails de connexion pour vous connecter.

## <a name="run-a-code-cell"></a>Exécuter une cellule de code

Vous pouvez créer des cellules contenant du code SQL que vous pouvez exécuter sur place en cliquant sur le bouton **Exécuter la cellule** (flèche noire ronde) à gauche de la cellule. Les résultats sont affichés dans le notebook après la fin de l’exécution de la cellule.

Par exemple :

1. Ajoutez une nouvelle cellule de code en sélectionnant la commande **+ Code** dans la barre d’outils.

   ![Barre d’outils du notebook](media/notebooks-guidance/notebook-toolbar.png)

2. Copiez et collez l’exemple suivant dans la cellule, puis cliquez sur **Exécuter la cellule**. Cet exemple crée une base de données.

   ```sql
   USE master
   GO

   -- Drop the database if it already exists
   IF  EXISTS (
           SELECT name
           FROM sys.databases
           WHERE name = N'TestNotebookDB'
      )
   DROP DATABASE TestNotebookDB
   GO

   -- Create the database
   CREATE DATABASE TestNotebookDB
   GO
   ```

   ![Exécuter la cellule](media/notebooks-sql-kernel/run-notebook-cell.png)

## <a name="save-the-result"></a>Enregistrer le résultat

Si vous exécutez un script qui retourne un résultat, vous pouvez enregistrer ce résultat dans différents formats à l’aide de la barre d’outils affichée au-dessus du résultat.

- Enregistrer au format CSV
- Enregistrer au format Excel
- Enregistrer au format JSON
- Enregistrer au format XML

Par exemple, le code suivant retourne le résultat de [PI](../../t-sql/functions/pi-transact-sql.md).

```sql
SELECT PI() AS PI;
GO
```

![Enregistrer le résultat](media/notebooks-sql-kernel/run-notebook-cell-2.png)

## <a name="next-steps"></a>Étapes suivantes

Découvrez-en plus sur les notebooks :

- [Guide pratique pour utiliser les notebooks dans Azure Data Studio](./notebooks-guidance.md)
- [Créer et exécuter un notebook Python](././notebooks-python-kernel.md)
- [Exécuter un exemple de notebook avec Spark](../../big-data-cluster/notebooks-tutorial-spark.md)