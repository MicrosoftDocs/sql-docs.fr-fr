---
title: Utilisez l’extension mssql Visual Studio Code
description: Utilisez l’extension mssql pour Visual Studio Code pour modifier et exécuter des scripts Transact-SQL pour SQL Server sur Linux.
ms.topic: conceptual
ms.prod: sql
ms.technology: tools-other
ms.assetid: 9766ee75-32d3-4045-82a6-4c7968bdbaa6
author: markingmyname
ms.author: maghan
ms.date: 10/28/2019
ms.openlocfilehash: fdb3d5e695824117def89347da883a0a7a5d7a9c
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100340541"
---
# <a name="use-visual-studio-code-to-create-and-run-transact-sql-scripts"></a>Utiliser Visual Studio Code pour créer et exécuter des scripts Transact-SQL

[!INCLUDE [SQL Server - Linux](../../includes/applies-to-version/sql-linux.md)]

Cet article explique comment utiliser l'extension **mssql** pour Visual Studio Code pour développer des bases de données SQL Server. Étant donné que Visual Studio Code est multiplateforme, vous pouvez utiliser l’extension **mssql** sur Linux, macOS et Windows.

## <a name="install-and-start-visual-studio-code"></a>Installer et démarrer Visual Studio Code

Visual Studio Code est un éditeur de code graphique multiplateforme qui prend en charge les extensions.

1. [Téléchargez et installez Visual Studio Code](https://code.visualstudio.com/) sur votre ordinateur.

2. Démarrez Visual Studio Code.

    >[!NOTE]
    >Si Visual Studio Code ne démarre pas lorsque vous êtes connecté par le biais d’une session Bureau xrdp à distance, consultez [VS Code ne fonctionne pas sur Ubuntu quand vous êtes connecté à l’aide de XRDP](https://github.com/Microsoft/vscode/issues/3451).

## <a name="install-the-mssql-extension"></a>Installer l’extension mssql

L’[extension mssql pour Visual Studio Code](https://aka.ms/mssql-marketplace) vous permet de vous connecter à une instance SQL Server, d’effectuer une requête avec Transact-SQL (T-SQL) et d’afficher les résultats.

1. Dans Visual Studio Code, sélectionnez **Afficher** > **Palette de commandes**, ou appuyez sur **Ctrl**+**Maj**+**P**, ou bien sur **F1** pour ouvrir la **Palette de commandes**.

2. Dans la **palette de commandes**, sélectionnez **Extensions : Installer des extensions** dans la liste déroulante.

3. Dans le volet **Extensions**, tapez *mssql*.

4. Sélectionnez l'extension **SQL Server (mssql)**, puis sélectionnez **Installer**.

   ![Installer l’extension mssql](./media/sql-server-develop-use-vscode/vscode-extension.png)

5. Une fois l’installation terminée, sélectionnez **Recharger** pour activer l’extension.

## <a name="create-or-open-a-sql-file"></a>Créer ou ouvrir un fichier SQL

L’extension mssql active les commandes mssql et T-SQL IntelliSense dans l’éditeur de code lorsque le mode de langage est défini sur **SQL**.

1. Sélectionnez **Fichier** > **Nouveau fichier**, ou appuyez sur **Ctrl**+**N**. Visual Studio Code ouvre par défaut un nouveau fichier texte brut. 

2. Sélectionnez **Texte brut** dans la barre d’état inférieure, ou appuyez sur **Ctrl**+**K** > **M** et sélectionnez **SQL** à partir du menu déroulant de langages. 

   ![Mode de langage SQL](./media/sql-server-develop-use-vscode/vscode-language-mode.png)

   > [!NOTE]
   > Si vous avez utilisé l’extension pour la première fois, elle installe la prise en charge des outils SQL Server.

Si vous ouvrez un fichier existant qui a une extension de fichier *.sql*, le mode de langage est automatiquement défini sur SQL.  

## <a name="connect-to-sql-server"></a>Se connecter à SQL Server

Procédez comme suit pour créer un profil de connexion et vous connecter à une instance SQL Server.

1. Appuyez sur **Ctrl**+**MAJ**+**P** ou sur **F1** pour ouvrir la **palette de commandes**. 

2. Tapez *sql* pour afficher les commandes mssql ou tapez *sqlcon*, puis sélectionnez **MS SQL : Se connecter** dans le menu déroulant.

   ![Commandes mssql](./media/sql-server-develop-use-vscode/vscode-commands.png)

   >[!NOTE]
   >Un fichier SQL tel que le fichier SQL vide que vous avez créé doit avoir son focus dans l’éditeur de code avant que vous puissiez exécuter les commandes mssql.

3. Sélectionnez la commande **MS SQL : Gérer les profils de connexion**.

4. Sélectionnez ensuite **Créer** afin de créer un profil de connexion pour votre instance SQL Server.

5. Suivez les invites afin de spécifier les propriétés du nouveau profil de connexion. Après avoir spécifié chaque valeur, appuyez sur **Entrée** pour continuer.

   | Propriété de connexion | Description |
   |---|---|
   | **Nom du serveur ou chaîne de connexion ADO** | Spécifiez le nom de l'instance SQL Server. Utilisez *localhost* pour vous connecter à une instance SQL Server sur votre ordinateur local. Pour vous connecter à une instance SQL Server distante, entrez le nom de l’instance SQL Server cible ou son adresse IP. Pour vous connecter à un conteneur SQL Server, spécifiez l’adresse IP de l’ordinateur hôte du conteneur. Si vous devez spécifier un port, utilisez une virgule pour le séparer du nom. Par exemple, pour un serveur qui écoute sur le port 1401, entrez `<servername or IP>,1401`.<br/><br/>Vous pouvez également entrer ici la chaîne de connexion ADO pour votre base de données. |
   | **Nom de base de données** (facultatif) | La base de données à utiliser. Pour vous connecter à la base de données par défaut, ne spécifiez pas de nom de base de données ici. |
   | **Type d'authentification** | Choisissez **Intégrée** ou **SQL**. |
   | **Nom d'utilisateur** | Si vous avez sélectionné **Connexion SQL**, entrez le nom d’un utilisateur ayant accès à une base de données sur le serveur. |
   | **Mot de passe** | Entrez le mot de passe de l'utilisateur spécifié. |
   | **Enregistrer le mot de passe** | Appuyez sur **Entrée** pour sélectionner **Oui** et enregistrer le mot de passe. Sélectionnez **Non** pour être invité à entrer le mot de passe chaque fois que le profil de connexion est utilisé. |
   | **Nom du profil** (facultatif) | Tapez un nom pour le profil de connexion, par exemple *profil localhost*. |

   Une fois que vous avez entré toutes les valeurs et sélectionné **Entrée**, Visual Studio Code crée le profil de connexion et se connecte à l’instance SQL Server.

   > [!TIP]
   > Si la connexion échoue, essayez de diagnostiquer le problème à partir du message d’erreur dans le panneau **Sortie** de Visual Studio Code. Pour ouvrir le panneau **Sortie**, sélectionnez **Afficher** > **Sortie**. Examinez également les [recommandations en matière de résolution des problèmes de connexion](../../linux/sql-server-linux-troubleshooting-guide.md).

6. Vérifiez votre connexion dans la barre d’état inférieure.

   ![État de la connexion](./media/sql-server-develop-use-vscode/vscode-connection-status.png)

Comme alternative aux étapes précédentes, vous pouvez également créer et modifier des profils de connexion dans le fichier de paramètres utilisateur(*settings.json*). Pour ouvrir le fichier de paramètres, sélectionnez **Fichier** > **Préférences** > **Paramètres**. Pour plus d’informations, consultez [Gérer les profils de connexion](https://github.com/Microsoft/vscode-mssql/wiki/manage-connection-profiles).

## <a name="create-a-sql-database"></a>Créer une base de données SQL

1. Dans le nouveau fichier SQL que vous avez démarré précédemment, tapez *sql* pour afficher une liste d’extraits de code modifiables.

   ![Extraits de code SQL](./media/sql-server-develop-use-vscode/vscode-sql-snippets.png)

2. Sélectionnez **sqlCreateDatabase**.

3. Dans l’extrait de code, tapez `TutorialDB` pour remplacer « DatabaseName » :

   ```sql
   -- Create a new database called 'TutorialDB'
   -- Connect to the 'master' database to run this snippet
   USE master
   GO
   IF NOT EXISTS (
      SELECT name
      FROM sys.databases
      WHERE name = N'TutorialDB'
   )
   CREATE DATABASE [TutorialDB]
   GO
   ```

4. Appuyez sur **Ctrl**+**MAJ**+**E** pour exécuter les commandes Transact-SQL. Affichez les résultats dans la fenêtre de requête.

    ![Créer des messages de base de données](./media/sql-server-develop-use-vscode/vscode-create-database-messages.png)

    > [!TIP]
    > Vous pouvez personnaliser les touches de raccourci pour les commandes mssql. Consultez [Personnaliser les raccourcis](https://github.com/Microsoft/vscode-mssql/wiki/customize-shortcuts).

## <a name="create-a-table"></a>Créer une table

1. Supprimez les contenus de la fenêtre de l’éditeur de code.

2. Appuyez sur **Ctrl**+**MAJ**+**P** ou sur **F1** pour ouvrir la **palette de commandes**.

3. Tapez *sql* pour afficher les commandes mssql ou tapez sur *sqluse*, puis sélectionnez la commande **MS SQL : Utiliser une base de données**.

4. Sélectionnez la nouvelle base de données **TutorialDB**.

   ![Utiliser une base de données](./media/sql-server-develop-use-vscode/vscode-use-database.png)

5. Dans l’éditeur de code, tapez *sql* pour afficher les extraits de code, sélectionnez **sqlCreateTable**, puis appuyez sur **Entrée**.

6. Dans l’extrait de code, tapez `Employees` pour le nom de la table.

7. Appuyez sur **Onglet** pour accéder au champ suivant, puis tapez `dbo` pour le nom du schéma.

8. Remplacez les définitions de colonne par les colonnes suivantes :

   ```sql
   EmployeesId INT NOT NULL PRIMARY KEY,
   Name [NVARCHAR](50)  NOT NULL,
   Location [NVARCHAR](50)  NOT NULL
   ```

9. Appuyez sur **Ctrl**+**MAJ**+**E** pour créer la table.

## <a name="insert-and-query"></a>Insérer et interroger

1. Ajoutez les instructions suivantes pour insérer quatre lignes dans la table **Employees**.

   ```sql
   -- Insert rows into table 'Employees'
   INSERT INTO Employees
      ([EmployeesId],[Name],[Location])
   VALUES
      ( 1, N'Jared', N'Australia'),
      ( 2, N'Nikita', N'India'),
      ( 3, N'Tom', N'Germany'),
      ( 4, N'Jake', N'United States')
   GO
   -- Query the total count of employees
   SELECT COUNT(*) as EmployeeCount FROM dbo.Employees;
   -- Query all employee information
   SELECT e.EmployeesId, e.Name, e.Location
   FROM dbo.Employees as e
   GO
   ```

   Pendant que vous tapez, T-SQL IntelliSense vous aide à compléter les instructions :

   ![T-SQL IntelliSense](./media/sql-server-develop-use-vscode/vscode-intellisense.png)

   > [!TIP]
   > L’extension mssql contient également des commandes pour vous aider à créer des instructions INSERT et SELECT. Elles n’étaient pas utilisées dans l’exemple précédent.

2. Appuyez sur **Ctrl**+**MAJ**+**E** pour exécuter les commandes. Les deux jeux de résultats s’affichent dans la fenêtre **Résultats**.

   ![Résultats](./media/sql-server-develop-use-vscode/vscode-result-grid.png)

## <a name="view-and-save-the-result"></a>Affichez et enregistrez le résultat

1. Sélectionnez **Afficher** > **Disposition de l’éditeur** > **Disposition de basculement** pour passer à une disposition de division verticale ou horizontale.

2. Sélectionnez les en-têtes **Résultats** et **Messages** du panneau pour réduire et développer les panneaux.

   ![Activer/désactiver les en-têtes](./media/sql-server-develop-use-vscode/vscode-toggle-messages-pannel.png)

   > [!TIP]
   > Vous pouvez personnaliser le comportement par défaut de l’extension mssql. Consultez [Options de personnalisation d’extension](https://github.com/Microsoft/vscode-mssql/wiki/customize-options).

3. Sélectionnez l’icône d’agrandissement de la grille sur la deuxième grille de résultats pour effectuer un zoom avant sur ces résultats.

   ![Agrandir la grille](./media/sql-server-develop-use-vscode/vscode-maximize-grid.png)

   > [!NOTE]
   > L’icône d’agrandissement s’affiche lorsque votre script T-SQL produit au moins deux grilles de résultats.

4. Ouvrez le menu contextuel de la grille en cliquant avec le bouton droit sur la grille.

   ![Menu contextuel](./media/sql-server-develop-use-vscode/vscode-grid-context-menu.png)

5. Sélectionnez **Sélectionner tout**.

6. Rouvrez le menu contextuel de la grille et sélectionnez **Enregistrer au format JSON** pour enregistrer le résultat dans un fichier *.json*.

7. Spécifiez un nom de fichier pour le fichier JSON.

8. Vérifiez que le fichier JSON est enregistré et s’ouvre dans Visual Studio Code.

   ![Enregistrer au format JSON](./media/sql-server-develop-use-vscode/vscode-save-as-json.png)

Si vous avez besoin d’enregistrer et d’exécuter ultérieurement des scripts SQL, pour l’administration ou pour un projet de développement plus important, enregistrez les scripts avec une extension *.sql*.

## <a name="next-steps"></a>Étapes suivantes

Si vous débutez avec T-SQL, consultez [Tutoriel : Écrire des instructions Transact-SQL](../../t-sql/tutorial-writing-transact-sql-statements.md) et [Informations de référence sur Transact-SQL (moteur de base de données)](../../t-sql/language-reference.md).

Pour plus d’informations sur l’utilisation de l’extension mssql ou sur la contribution à cette dernière, consultez le [wiki du projet d’extension mssql](https://github.com/Microsoft/vscode-mssql/wiki).

Pour plus d’informations sur l’utilisation de Visual Studio Code, consultez la [documentation Visual Studio Code](https://code.visualstudio.com/docs).