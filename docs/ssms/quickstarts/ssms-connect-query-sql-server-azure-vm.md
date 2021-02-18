---
title: Se connecter à une instance SQL Server et l’interroger sur une machine virtuelle Azure avec SQL Server Management Studio (SSMS)
description: Connectez-vous à une instance SQL Server sur une machine virtuelle Azure à l’aide de SSMS. Créez et interrogez SQL Server sur une machine virtuelle Azure en exécutant des requêtes T-SQL de base dans SSMS.
ms.prod: sql
ms.technology: ssms
ms.topic: quickstart
author: markingmyname
ms.author: maghan
ms.reviewer: sstein, mikeray
ms.custom: contperf-fy21q2
ms.date: 12/15/2020
ms.openlocfilehash: b6442715c1482ed581302667bcdaf7985948c554
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100350602"
---
# <a name="quickstart-connect-and-query-a-sql-server-instance-on-an-azure-virtual-machine-using-sql-server-management-studio-ssms"></a>Démarrage rapide : Se connecter à une instance SQL Server et l’interroger sur une machine virtuelle Azure avec SQL Server Management Studio (SSMS)

[!INCLUDE [sqlserver](../../includes/applies-to-version/sqlserver.md)]

Commencez à utiliser SQL Server Management Studio (SSMS) pour vous connecter à votre instance SQL Server sur une machine virtuelle Azure et exécutez des commandes Transact-SQL (T-SQL).

> [!div class="checklist"]
> - Se connecter à une instance de SQL Server
> - Création d'une base de données
> - Créer une table dans votre nouvelle base de données
> - Insérer des lignes dans votre nouvelle table
> - Interroger la nouvelle table et afficher les résultats
> - Utiliser la table de fenêtre de requête pour vérifier les propriétés de votre connexion
## <a name="prerequisites"></a>Prérequis

Pour effectuer les étapes de cet article, vous avez besoin de SQL Server Management Studio et d’un accès à une source de données.

- Installez [SQL Server Management Studio](../download-sql-server-management-studio-ssms.md).
- [SQL Server sur une machine virtuelle Azure](https://azure.microsoft.com/services/virtual-machines/sql-server/?&ef_id=CjwKCAiA17P9BRB2EiwAMvwNyP3g3mY45X8dbqEtIr8HASwSLUYRBrwwciZptYt0vUU4K7TuWnXTbxoCoPQQAvD_BwE:G:s&OCID=AID2100131_SEM_CjwKCAiA17P9BRB2EiwAMvwNyP3g3mY45X8dbqEtIr8HASwSLUYRBrwwciZptYt0vUU4K7TuWnXTbxoCoPQQAvD_BwE:G:s&gclid=CjwKCAiA17P9BRB2EiwAMvwNyP3g3mY45X8dbqEtIr8HASwSLUYRBrwwciZptYt0vUU4K7TuWnXTbxoCoPQQAvD_BwE).

## <a name="connect-to-sql-virtual-machines"></a>Se connecter à des machines virtuelles SQL

Les étapes suivantes décrivent comment créer une étiquette DNS pour votre machine virtuelle Azure et vous connecter à SQL Server Management Studio (SSMS).

### <a name="configure-a-dns-label-for-the-public-ip-address"></a>Configurer un nom DNS pour l’adresse IP publique

Pour vous connecter au moteur de base de données SQL Server à partir d’Internet, il est recommandé de créer une étiquette DNS pour votre adresse IP publique. Vous pouvez vous joindre par adresse IP, mais l’étiquette DNS crée un enregistrement A qui est plus facile à identifier et extrait l’adresse IP publique sous-jacente.

> [!NOTE]
> Les noms DNS ne sont pas nécessaires si vous prévoyez uniquement de vous connecter à l’instance SQL Server dans le même réseau virtuel ou localement.

1. Créez une étiquette DNS en sélectionnant **Machines virtuelles** sur le portail. Sélectionnez votre machine virtuelle SQL Server pour afficher ses propriétés.

2. Dans l’aperçu de la machine virtuelle, sélectionnez votre **adresse IP publique**.

    :::image type="content" source="media/ssms-connect-query-sql-server-azure-vm/azure-sql-vm-ip-address-.png" alt-text="adresse ip publique":::

3. Dans les propriétés de votre adresse IP publique, développez **Configuration**.

4. Entrez un nom DNS. Il s’agit d’un enregistrement A qui peut être utilisé pour la connexion directe à votre machine virtuelle SQL Server par nom plutôt que directement par adresse IP.

5. Sélectionnez le bouton **Enregistrer**.

    :::image type="content" source="media/ssms-connect-query-sql-server-azure-vm/azure-sql-vm-dns-label.png" alt-text="étiquette dns":::

### <a name="connect"></a>Se connecter

1. Exécutez SQL Server Management Studio. La première fois que vous exécutez SSMS, la fenêtre **Se connecter au serveur** s’ouvre. Si elle ne s’ouvre pas, vous pouvez l’ouvrir manuellement en sélectionnant **Explorateur d’objets** > **Se connecter** > **Moteur de base de données**.

    :::image type="content" source="media/ssms-connect-query-sql-server-azure-vm/connect-object-explorer.png" alt-text="Lien de connexion dans l’Explorateur d’objets":::

2. La boîte de dialogue **Se connecter au serveur** s’affiche. Entrez les informations suivantes :

    |   Paramètre   |   Valeur(s) suggérée(s)   |   Description   |
    |--------------|-----------------------|-----------------|
    | **Type de serveur** | Moteur de base de données | Pour **Type de serveur**, sélectionnez **Moteur de base de données** (généralement l’option par défaut). |
    | **Nom du serveur** | Nom complet du serveur | Pour **Nom du serveur**, entrez le nom de votre machine virtuelle Azure SQL. Vous pouvez aussi utiliser l’adresse IP de la machine virtuelle Azure SQL pour vous connecter. | 
    | **Authentification** | l’authentification SQL Server | Utilisez l’**Authentification SQL Server** pour la connexion de la machine virtuelle Azure SQL. De même, si vous avez une configuration d’environnement Azure Active Directory, vous pouvez utiliser une des options Azure Active Directory. </br> </br> La méthode d’**authentification Windows** n’est pas prise en charge pour la machine virtuelle Azure SQL. Pour plus d’informations, consultez [Authentification Azure SQL](/azure/sql-database/sql-database-security-overview#access-management).|
    | **Connexion** | ID d’utilisateur du compte serveur | ID d’utilisateur du compte serveur utilisé pour créer le serveur. Une connexion est nécessaire quand l’**authentification SQL Server** est utilisée. |
    | **Mot de passe** | Mot de passe du compte serveur | Mot de passe du compte serveur utilisé pour créer le serveur. Un mot de passe est nécessaire quand l’**authentification SQL Server** est utilisée. |

    :::image type="content" source="media/ssms-connect-query-sql-server-azure-vm/connect-to-azure-sql-vm-object-explorer.png" alt-text="Champ Nom du serveur pour les machines virtuelles SQL":::

3. Une fois que vous avez renseigné tous les champs, sélectionnez **Se connecter**.

    Vous pouvez également modifier d’autres options de connexion en sélectionnant **Options**. Exemples d’options de connexion : la base de données à laquelle vous êtes connecté, la valeur du délai d’expiration de la connexion et le protocole réseau. Cet article utilise les valeurs par défaut pour toutes les options.

4. Pour vérifier que votre connexion SQL Server sur la machine virtuelle Azure a abouti, développez et explorez les objets dans l’**Explorateur d’objets** où figurent le nom du serveur, la version de SQL Server et le nom d’utilisateur. Ces objets sont différents selon le type de serveur.

    :::image type="content" source="media/ssms-connect-query-sql-server-azure-vm/connect-azure-sql-vm.png" alt-text="connexion de la machine virtuelle Azure sql":::

## <a name="troubleshoot-connectivity-issues"></a>Résoudre les problèmes de connectivité

Bien que le portail offre des options de configuration automatique de la connectivité, il est utile de savoir comment configurer manuellement la connectivité. Comprendre la configuration requise peut également faciliter la résolution des problèmes.

Le tableau suivant répertorie la configuration requise pour se connecter à SQL Server sur une machine virtuelle Azure.

| Condition requise | Description |
|---|---|
| [Activer le mode d’authentification SQL Server](../../database-engine/configure-windows/change-server-authentication-mode.md#use-ssms) | L’authentification SQL Server est nécessaire pour se connecter à distance à la machine virtuelle, sauf si vous avez configuré Active Directory sur un réseau virtuel. |
| [Créer une connexion SQL](../../relational-databases/security/authentication-access/create-a-login.md) | Si vous utilisez l’authentification SQL, vous avez besoin d’un identifiant SQL avec un nom d’utilisateur et un mot de passe et qui dispose d’autorisations sur votre base de données cible. |
| Activer le protocole TCP/IP | SQL Server doit autoriser les connexions sur TCP. |
| [Activer la règle de pare-feu pour le port SQL Server](../../database-engine/configure-windows/configure-a-windows-firewall-for-database-engine-access.md) | Le pare-feu sur la machine virtuelle doit autoriser le trafic entrant sur le port SQL Server (1433 par défaut). |
| [Créer une règle de groupe de sécurité pour TCP 1433](/azure/virtual-network/manage-network-security-group#create-a-security-rule) | Autorisez la machine virtuelle à recevoir le trafic sur le port SQL Server (1433 par défaut) si vous souhaitez vous connecter par Internet. Ce n’est pas nécessaire pour les connexions locales et réseau virtuel uniquement. Cette étape est nécessaire sur le portail Azure uniquement. |

> [!TIP]
> Les étapes décrites dans le tableau précédent sont effectuées pour vous lorsque vous configurez la connectivité dans le portail. Utilisez ces étapes uniquement pour vérifier votre configuration ou pour configurer manuellement la connectivité pour SQL Server.

## <a name="create-a-database"></a>Création d'une base de données

Créez une base de données appelée TutorialDB en effectuant les étapes suivantes :

1. Dans l’Explorateur d’objets, cliquez avec le bouton droit sur votre instance de serveur et sélectionnez **Nouvelle requête** :

   :::image type="content" source="media/ssms-connect-query-sql-server-azure-vm/new-query.png" alt-text="Lien Nouvelle requête":::

2. Collez l’extrait de code T-SQL suivant dans la fenêtre de requête :

    ```sql
    IF NOT EXISTS (
    SELECT name
    FROM sys.databases
    WHERE name = N'TutorialDB'
    )
    CREATE DATABASE [TutorialDB]
    GO
    
    ALTER DATABASE [TutorialDB] SET QUERY_STORE=ON
    GO
    ```

3. Exécutez la requête en sélectionnant **Exécuter** ou en sélectionnant F5 sur votre clavier.

   :::image type="content" source="media/ssms-connect-query-sql-server-azure-vm/execute.png" alt-text="Commande Exécuter":::
  
    Une fois que la requête est terminée, la nouvelle base de données TutorialDB s’affiche dans la liste des bases de données de l’Explorateur d’objets. Si elle n’apparaît pas, cliquez avec le bouton droit sur le nœud **Bases de données** et sélectionnez **Actualiser**.

## <a name="create-a-table-in-the-new-database"></a>Créer une table dans la nouvelle base de données

Dans cette section, vous allez créer une table dans la base de données TutorialDB que vous venez de créer. Étant donné que l’éditeur de requête est toujours dans le contexte de la base de données *master*, changez le contexte de connexion avec la base de données *TutorialDB* en effectuant les étapes suivantes :

1. Dans la liste déroulante des bases de données, sélectionnez la base de données que vous voulez, comme ici :

   :::image type="content" source="media/ssms-connect-query-sql-server-azure-vm/change-db.png" alt-text="Changer la base de données":::

2. Collez l’extrait de code T-SQL suivant dans la fenêtre de requête :

    ```sql
    USE [TutorialDB]
    -- Create a new table called 'Customers' in schema 'dbo'
    -- Drop the table if it already exists
    IF OBJECT_ID('dbo.Customers', 'U') IS NOT NULL
    DROP TABLE dbo.Customers
    GO
    -- Create the table in the specified schema
    CREATE TABLE dbo.Customers
    (
       CustomerId        INT    NOT NULL   PRIMARY KEY, -- primary key column
       Name      [NVARCHAR](50)  NOT NULL,
       Location  [NVARCHAR](50)  NOT NULL,
       Email     [NVARCHAR](50)  NOT NULL
    );
    GO
    ```

3. Exécutez la requête en sélectionnant **Exécuter** ou en sélectionnant F5 sur votre clavier.

Une fois que la requête est terminée, la nouvelle table Customers s’affiche dans la liste des tables de l’Explorateur d’objets. Si la table ne s’affiche pas, cliquez avec le bouton droit sur le nœud **TutorialDB** > **Tables** dans l’Explorateur d’objets et sélectionnez **Actualiser**.

   :::image type="content" source="media/ssms-connect-query-sql-server-azure-vm/new-table.png" alt-text="Nouvelle table":::

## <a name="insert-rows-into-the-new-table"></a>Insérer des lignes dans la nouvelle table

À présent, insérons quelques lignes dans la table Customers que vous avez créée. Collez l’extrait de code T-SQL suivant dans la fenêtre de requête, puis sélectionnez **Exécuter** :

   ```sql
   -- Insert rows into table 'Customers'
   INSERT INTO dbo.Customers
      ([CustomerId],[Name],[Location],[Email])
   VALUES
      ( 1, N'Orlando', N'Australia', N''),
      ( 2, N'Keith', N'India', N'keith0@adventure-works.com'),
      ( 3, N'Donna', N'Germany', N'donna0@adventure-works.com'),
      ( 4, N'Janet', N'United States', N'janet1@adventure-works.com')
   GO
   ```

## <a name="query-the-table-and-view-the-results"></a>Interroger la table et afficher les résultats

Les résultats d’une requête sont visibles sous la fenêtre du texte de requête. Pour interroger la table Customers et voir les lignes qui ont été insérées, suivez les étapes ci-dessous :

1. Collez l’extrait de code T-SQL suivant dans la fenêtre de requête, puis sélectionnez **Exécuter** :

   ```sql
   -- Select rows from table 'Customers'
   SELECT * FROM dbo.Customers;
   ```

    Les résultats de la requête s’affichent en dessous de la zone où le texte a été entré.

   :::image type="content" source="media/ssms-connect-query-sql-server-azure-vm/query-results.png" alt-text="Liste des résultats":::

    Vous pouvez aussi modifier la façon dont les résultats sont présentés en sélectionnant une des options suivantes :

   ![Trois options d’affichage des résultats de requête](media/ssms-connect-query-sql-server-azure-vm/results.png)

   - Le premier bouton affiche les résultats en **Mode texte**, comme illustré dans l’image de la section suivante.
   - Le bouton du milieu affiche les résultats en **Mode grille**, qui est l’option par défaut.
       - Il s’agit du paramètre par défaut
   - Le troisième bouton vous permet d’enregistrer les résultats dans un fichier dont l’extension est .rpt par défaut.

## <a name="verify-your-connection-properties-by-using-the-query-window-table"></a>Vérifier les propriétés de votre connexion en utilisant la table de la fenêtre de requête

Des informations sur les propriétés de connexion figurent sous les résultats de votre requête. Après avoir exécuté la requête citée plus haut dans l’étape précédente, vérifiez les propriétés de connexion en bas de la fenêtre de requête.

- Vous pouvez identifier le serveur et la base de données auxquels vous êtes connecté, ainsi que le nom d’utilisateur employé.
- Vous pouvez également voir la durée de la requête et le nombre de lignes qui sont retournées par la requête exécutée précédemment.

   :::image type="content" source="media/ssms-connect-query-sql-server-azure-vm/connection-properties.png" alt-text="Propriétés de connexion":::

## <a name="additional-tools"></a>Outils supplémentaires

Vous pouvez aussi utiliser [Azure Data Studio](../../azure-data-studio/download-azure-data-studio.md) pour vous connecter à [SQL Server](../../azure-data-studio/quickstart-sql-server.md), [Azure SQL Database](../../azure-data-studio/quickstart-sql-database.md) et [Azure Synapse Analytics](../../azure-data-studio/quickstart-sql-dw.md) et les interroger.

## <a name="next-steps"></a>Étapes suivantes

La meilleure façon de se familiariser avec SSMS est d’effectuer des exercices pratiques. Ces articles vous aident à vous familiariser avec les différentes fonctionnalités disponibles dans SSMS.

- [Éditeur de requêtes SQL Server Management Studio (SSMS)](../f1-help/database-engine-query-editor-sql-server-management-studio.md)
- [Création de scripts](../tutorials/scripting-ssms.md)
- [Utilisation de modèles dans SSMS](../template/templates-ssms.md)
- [Configuration de SSMS](../tutorials/ssms-configuration.md)
- [Conseils et astuces supplémentaires pour utiliser SSMS](../tutorials/ssms-tricks.md)