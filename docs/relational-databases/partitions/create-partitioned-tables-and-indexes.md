---
description: Créer des tables et des index partitionnés
title: Créer des tables et des index partitionnés | Microsoft Docs
ms.custom: ''
ms.date: 1/5/2021
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: ''
ms.topic: conceptual
f1_keywords:
- sql13.swb.createpartition.progress.f1
- sql13.swb.createpartition.partitioncolumn.f1
- sql13.swb.createpartition.createjob.f1
- sql13.swb.createpartition.finish.f1
- sql13.swb.createpartition.selectoutput.f1
- sql13.swb.createpartition.partitionfunction.f1
- sql13.swb.createpartition.partitionscheme.f1
- sql13.swb.createpartition.getstart.f1
- sql13.swb.createpartition.mappartition.f1
- sql13.swb.createpartition.summary.f1
helpviewer_keywords:
- partitioned indexes [SQL Server], creating
- partition schemes [SQL Server], creating
- partition functions [SQL Server], creating
- partitioned tables [SQL Server], creating
- partition functions [SQL Server]
- partition schemes [SQL Server]
ms.assetid: 7641df10-1921-42a7-ba6e-4cb03b3ba9c8
author: julieMSFT
ms.author: jrasnick
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 273e6f625b0a86be179da529e8b52e0d7ce991cc
ms.sourcegitcommit: 04d101fa6a85618b8bc56c68b9c006b12147dbb5
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/29/2021
ms.locfileid: "99049147"
---
# <a name="create-partitioned-tables-and-indexes"></a>Créer des tables et des index partitionnés
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
  Vous pouvez créer une table ou un index partitionné(e) dans [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] à l'aide de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] ou de [!INCLUDE[tsql](../../includes/tsql-md.md)]. Les données contenues dans des tables et des index partitionnés sont divisées horizontalement en unités qui peuvent être réparties sur plusieurs groupes de fichiers d'une base de données. Le partitionnement permet de rendre des tables et des index volumineux plus gérables et plus évolutifs.  
  
 La création d'une table ou d'un index partitionné(e) se produit généralement en quatre étapes :  
  
1.  Créez un ou plusieurs groupes de fichiers et les fichiers correspondants qui contiendront les partitions spécifiées par le schéma de partition.  
  
2.  Créez une fonction de partition qui mappe les lignes d'une table ou d'un index avec des partitions basées sur les valeurs d'une colonne spécifiée.  
  
3.  Créez un schéma de partition qui mappe les partitions d'une table ou d'un index partitionné(e) avec les nouveaux groupes de fichiers.  
  
4.  Créez ou modifiez une table ou un index et spécifiez le schéma de partition comme emplacement de stockage.  
 
> [!NOTE]
> Dans Azure [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)], seuls les groupes de fichiers primaires sont pris en charge.  
  
 **Dans cette rubrique**  
  
-   **Avant de commencer :**  
  
     [Limitations et restrictions](#Restrictions)  
  
     [Sécurité](#Security)  
  
-   **Pour créer une table ou un index partitionné(e), utilisez :**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Avant de commencer  
  
###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> Limitations et restrictions  
  
-   L'étendue d'une fonction de partition et d'un schéma est limitée à la base de données dans laquelle ils ont été créés. Dans la base de données, les fonctions de partition résident dans un espace de noms indépendant des autres fonctions.  
  
-   Si des lignes dans une fonction de partition ont des colonnes de partitionnement avec des valeurs Null, ces lignes sont allouées à la partition la plus à gauche. Toutefois, si NULL est spécifié comme valeur limite et que RIGHT est indiqué, la partition la plus à gauche reste vide et les valeurs NULL sont placées dans la deuxième partition.  
  
###  <a name="security"></a><a name="Security"></a> Sécurité  
  
####  <a name="permissions"></a><a name="Permissions"></a> Autorisations  
 La création d'une table partitionnée nécessite une autorisation CREATE TABLE dans la base de données et une autorisation ALTER pour le schéma dans lequel la table est créée. La création d'un index partitionné nécessite l'autorisation ALTER sur la table ou la vue dans laquelle l'index est créé. La création d'une table ou d'un index partitionné(e) nécessite l'une des autorisations supplémentaires suivantes :  
  
-   Autorisation ALTER ANY DATASPACE. Cette autorisation est attribuée par défaut aux membres du rôle de serveur fixe **sysadmin** et des rôles de base de données fixes **db_owner** et **db_ddladmin** .  
  
-   Autorisation CONTROL ou ALTER sur la base de données dans laquelle la fonction de partition et le schéma de partition sont créés.  
  
-   Autorisation CONTROL SERVER ou ALTER ANY DATABASE sur le serveur de la base de données dans laquelle la fonction de partition et le schéma de partition sont créés.  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Utilisation de SQL Server Management Studio  
 Suivez les étapes de cette procédure pour créer un ou plusieurs groupes de fichiers, les fichiers correspondants et une table. Vous référencerez ces objets dans la procédure suivante lorsque vous créerez la table partitionnée.  
  
#### <a name="to-create-new-filegroups-for-a-partitioned-table"></a>Pour créer de nouveaux groupes de fichiers pour une table partitionnée  
  
1.  Dans l’Explorateur d’objets, cliquez avec le bouton droit sur la base de données dans laquelle vous souhaitez créer une table partitionnée et sélectionnez **Propriétés**.  
  
2.  Dans la boîte de dialogue **Propriétés de la base de données -** *nom_base_de_données*, sous **Sélectionner une page**, sélectionnez **Groupes de fichiers**.  
  
3.  Sous **Lignes**, cliquez sur **Ajouter**. Dans la nouvelle ligne, entrez le nom du groupe de fichiers.  
  
    > [!WARNING]  
    >  Quand vous spécifiez plusieurs groupes de fichiers, vous devez toujours avoir un groupe de fichiers supplémentaire en plus du nombre de groupes de fichiers spécifié pour les valeurs limites lorsque vous créez des partitions.  
  
4.  Continuez à ajouter des lignes jusqu'à ce que vous ayez créé tous les groupes de fichiers pour la table partitionnée.  
  
5.  Cliquez sur **OK**.  
  
6.  Sous **Sélectionner une page**, sélectionnez **Fichiers**.  
  
7.  Sous **Lignes**, cliquez sur **Ajouter**. Dans la nouvelle ligne, entrez un nom de fichier et sélectionnez un groupe de fichiers.  
  
8.  Continuez à ajouter des lignes jusqu'à ce que vous ayez créé au moins un fichier pour chaque groupe de fichiers.  
  
9. Développez le dossier **Tables** et créez une table selon la procédure habituelle. Pour plus d’informations, consultez [Créer des tables &#40;moteur de base de données&#41;](../../relational-databases/tables/create-tables-database-engine.md). Vous pouvez éventuellement spécifier une table existante dans la procédure suivante.  
  
#### <a name="to-create-a-partitioned-table"></a>Pour créer une table partitionnée  
  
1.  Cliquez avec le bouton droit sur la table à partitionner, pointez sur **Stockage**, puis cliquez sur **Créer une partition...**.  
  
2.  Dans l' **Assistant Création de partition**, sur la page **Assistant Création de partition** , cliquez sur **Suivant**.  
  
3.  Sur la page **Sélectionner une colonne de partitionnement** , dans la grille **Colonnes de partitionnement disponibles** , sélectionnez la colonne sur laquelle vous souhaitez partitionner votre table. Seules les colonnes dont le type de données peut être utilisé pour partitionner des données seront affichées dans la grille **Colonnes de partitionnement disponibles** . Si vous sélectionnez une colonne calculée comme colonne de partitionnement, celle-ci doit être désignée en tant que colonne persistante.  
  
     Le degré avec lequel vous pouvez regrouper les données de façon logique déterminent les options dont vous disposez pour définir la colonne de partitionnement et la plage de valeurs. Par exemple, vous pouvez choisir de diviser vos données en regroupements logiques par mois ou trimestres d'une année. Les requêtes que vous projetez d'exécuter sur vos données détermineront si ce regroupement logique est adéquat pour gérer vos partitions de table. Tous les types de données sont utilisables comme colonnes de partitionnement, à l’exception de **text**, **ntext**, **image**, **xml**, **timestamp**, **varchar(max)**, **nvarchar(max)**, **varbinary(max)**, des types de données d’alias ou des types de données CLR définis par l’utilisateur.  
  
     Les options supplémentaires suivantes sont disponibles sur cette page :  
  
     **Colocaliser cette table avec la table partitionnée sélectionnée**  
     Permet de sélectionner une table partitionnée qui contient les données connexes à joindre à cette table sur la colonne de partitionnement. Les requêtes portant sur des tables présentant des partitions jointes sur les colonnes de partitionnement sont généralement plus efficaces.  
  
     **Aligner les index non uniques et uniques avec une colonne de partition indexée lors du stockage**  
     Aligne tous les index de la table qui sont partitionnés avec le même schéma de partition. Lorsqu'une table et ses index sont alignés, vous pouvez plus efficacement déplacer des partitions dans et hors de tables partitionnées, car vos données sont partitionnées avec le même algorithme.  
  
     Après avoir sélectionné la colonne de partitionnement et toute autre option, cliquez sur **Suivant**.  
  
4.  Sur la page **Sélectionner une fonction de partition** , sous **Sélectionner une fonction de partition**, cliquez sur **Nouvelle fonction de partition** ou sur **Fonction de partition existante**. Si vous choisissez **Nouvelle fonction de partition**, entrez le nom de la fonction. Si vous choisissez **Fonction de partition existante**, sélectionnez le nom de la fonction que vous souhaitez utiliser dans la liste. L'option **Fonction de partition existante** ne sera pas disponible s'il n'existe aucune autre fonction de partition sur la base de données.  
  
     Après avoir complété cette page, cliquez sur **Suivant**.  
  
5.  Sur la page **Sélectionner un schéma de partition** , sous **Sélectionner un schéma de partition**, cliquez sur **Nouveau schéma de partition** ou sur **Schéma de partition existant**. Si vous choisissez **Nouveau schéma de partition**, entrez le nom du schéma. Si vous choisissez **Schéma de partition existant**, sélectionnez le nom du schéma que vous souhaitez utiliser dans la liste. L'option **Schéma de partition existant** ne sera pas disponible s'il n'existe aucun autre schéma de partition sur la base de données.  
  
     Après avoir complété cette page, cliquez sur **Suivant**.  
  
6.  Sur la page **Associer les partitions** , sous **Plage**, sélectionnez **Limite gauche** ou **Limite droite** pour spécifier s'il faut inclure la valeur limite la plus élevée ou la plus basse dans chaque groupe de fichiers que vous créez. Quand vous spécifiez plusieurs groupes de fichiers, vous devez toujours entrer un groupe de fichiers supplémentaire en plus du nombre de groupes de fichiers spécifié pour les valeurs limites lorsque vous créez des partitions.  
  
     Dans la grille **Sélectionnez les groupes de fichiers et spécifiez les valeurs limites** , sous **Groupe de fichiers**, sélectionnez le groupe de fichiers dans lequel vous souhaitez partitionner vos données. Sous **Limite**, entrez la valeur limite pour chaque groupe de fichiers. Si la valeur limite reste vide, la fonction de partition mappe la totalité de la table ou de l'index en une seule partition en utilisant le nom de la fonction de partition.  
  
     Les options supplémentaires suivantes sont disponibles sur cette page :  
  
     **Définir les limites...**  
     Ouvre la boîte de dialogue **Définir les valeurs limites** pour sélectionner les valeurs limites et les plages de dates voulues pour vos partitions. Cette option est disponible uniquement quand vous avez sélectionné une colonne de partitionnement qui contient l’un des types de données suivants : **date**, **datetime**, **smalldatetime**, **datetime2** ou **datetimeoffset**.  
  
     **Estimer le stockage**  
     Estime le nombre de lignes, l'espace requis et l'espace disponible pour le stockage de chaque groupe de fichiers spécifié pour les partitions. Ces valeurs s'affichent dans la grille en tant que valeurs en lecture seule.  
  
     La boîte de dialogue **Définir les valeurs limites** autorise les options supplémentaires suivantes :  
  
     **Date de début**  
     Sélectionne la date de début pour les valeurs de plages de vos partitions.  
  
     **Date de fin**  
     Sélectionne la date de fin pour les valeurs de plages de vos partitions. Si vous avez sélectionné l’option **Limite gauche** dans la page **Associer les partitions** , cette date est la dernière valeur de chaque groupe de fichiers/partition. Si vous avez sélectionné l’option **Limite droite** dans la page **Associer les partitions** , cette date est la première valeur du prochain groupe de fichiers.  
  
     **Plage de dates**  
     Sélectionne la granularité de date ou l'incrément de valeur de plage qui vous intéresse pour chaque partition.  
  
     Après avoir complété cette page, cliquez sur **Suivant**.  
  
7.  Sur la page **Sélectionner une option de sortie** , spécifiez de quelle manière vous souhaitez remplir votre table partitionnée. Sélectionnez **Créer un script** pour créer un script SQL basé sur les pages précédentes de l'Assistant. Sélectionnez **Exécuter immédiatement** pour créer la nouvelle table partitionnée après avoir complété toutes les pages restantes de l'Assistant. Sélectionnez **Planification** pour créer la nouvelle table partitionnée à un moment prédéterminé dans le futur.  
  
     Si vous sélectionnez **Créer un script**, les options suivantes sont disponibles sous **Options de script**:  
  
     **Générer un script sur fichier**  
     Génère le script sous la forme d'un fichier .sql. Entrez un nom de fichier et un emplacement dans la boîte de dialogue **Nom de fichier** ou cliquez sur **Parcourir** pour ouvrir la boîte de dialogue **Emplacement du fichier de script** . Pour **Enregistrer sous**, sélectionnez **Texte Unicode** ou **Texte ANSI**.  
  
     **Générer un script sur le Presse-papiers**  
     Enregistre le script dans le Presse-papiers.  
  
     **Générer un script dans une nouvelle fenêtre de requête**  
     Génère le script dans une nouvelle fenêtre de l'éditeur de requêtes. Il s'agit de la sélection par défaut.  
  
     Si vous sélectionnez **Planification**, cliquez sur **Modifier la planification**.  
  
    1.  Dans la boîte de dialogue **Nouvelle planification du travail**, dans la zone **Nom**, entrez le nom de la planification du travail.  
  
    2.  Dans la liste **Type de planification** , sélectionnez le type de la planification :  
  
        -   **Lancer automatiquement au démarrage de SQL Server Agent**  
  
        -   **Démarrer dès que les processeurs sont inactifs**  
  
        -   **Périodique**. Sélectionnez cette option si votre nouvelle table partitionnée est mise à jour régulièrement avec de nouvelles informations.  
  
        -   **Une fois**. Il s'agit de la sélection par défaut.  
  
    3.  Activez ou désactivez la case à cocher **Activé** pour activer ou désactiver la planification.  
  
    4.  Si vous sélectionnez **Périodique**:  
  
        1.  Sous **Fréquence**, dans la liste **Périodicité** , spécifiez la fréquence d'occurrence :  
  
            -   Si vous sélectionnez **Quotidienne**, dans la zone **Répéter toutes les** , entrez la fréquence de répétition du travail de planification en jours.  
  
            -   Si vous sélectionnez **Hebdomadaire**, dans la zone **Répéter toutes les** , entrez la fréquence de répétition du travail de planification en semaines. Sélectionnez le jour ou les jours de la semaine pendant lesquels la planification du travail est exécutée.  
  
            -   Si vous sélectionnez **Mensuelle**, sélectionnez **Jour** ou **Le**.  
  
                -   Si vous sélectionnez **Jour**, entrez la date du mois à laquelle vous souhaitez que la planification du travail s'exécute, ainsi que la fréquence de répétition de la planification du travail en mois. Par exemple, si vous souhaitez que la planification du travail s’exécute le 15 du mois un mois sur deux, sélectionnez **Jour**, puis entrez « 15 » dans la première zone et « 2 » dans la deuxième zone. Notez que le nombre maximum autorisé dans la deuxième zone est « 99 ».  
  
                -   Si vous sélectionnez **Le**, sélectionnez le jour spécifique de la semaine et du mois pendant lequel vous voulez que la planification du travail s'exécute et la fréquence à laquelle la planification du travail doit se répéter en mois. Par exemple, si vous souhaitez que la planification du travail s’exécute le dernier jour de la semaine un mois sur deux, sélectionnez **Jour**, puis **dernier** dans la première liste, **jour ouvrable** dans la deuxième liste et « 2 » dans la dernière zone. Vous pouvez également sélectionner **premier**, **deuxième**, **troisième** ou **quatrième**, ainsi que des jours de la semaine spécifiques (par exemple, dimanche ou mercredi) dans les deux premières listes. Notez que le nombre maximum autorisé dans la dernière zone est « 99 ».  
  
        2.  Sous **Fréquence quotidienne**, spécifiez la fréquence à laquelle la planification du travail se répète le jour de son exécution :  
  
            -   Si vous sélectionnez **Une fois à**, entrez l'heure spécifique à laquelle la planification du travail doit s'exécuter dans la zone **Une fois à** . Entrez l'heure, les minutes et les secondes du jour, ainsi que AM ou PM.  
  
            -   Si vous sélectionnez **Toutes les**, spécifiez la fréquence à laquelle la planification du travail s'exécute pendant la journée choisie sous **Fréquence**. Par exemple, si vous souhaitez que la planification du travail se répète toutes les 2 heures le jour d’exécution de la planification du travail, sélectionnez **Toutes les**, entrez « 2 » dans la première zone, puis sélectionnez **heure(s)** dans la liste. Dans cette liste, vous pouvez également sélectionner **minute(s)** et **seconde(s)** . Notez que le nombre maximum autorisé dans la première zone est « 100 ».  
  
                 Dans la zone **Début** , entrez l'heure à laquelle l'exécution de la planification du travail doit démarrer. Dans la zone **Fin** , entrez l'heure à laquelle la planification du travail doit s'arrêter. Entrez l'heure, les minutes et les secondes du jour, ainsi que AM ou PM.  
  
        3.  Sous **Durée**, dans la zone **Date de début**, entrez la date à laquelle vous souhaitez que l'exécution de la planification du travail commence. Sélectionnez **Date de fin** ou **Aucune date de fin** pour indiquer à quel moment l'exécution de la planification du travail doit s'arrêter. Si vous sélectionnez **Date de fin**, entrez la date à laquelle l'exécution de la planification du travail doit s'arrêter.  
  
    5.  Si vous sélectionnez **Une fois** sous **Une seule occurrence**, dans la zone **Date** , entrez la date à laquelle la planification du travail est exécutée. Dans la zone **Heure** , entrez l'heure à laquelle la planification du travail sera exécutée. Entrez l'heure, les minutes et les secondes du jour, ainsi que AM ou PM.  
  
    6.  Sous **Résumé**, dans **Description**, vérifiez que tous les paramètres de planification du travail sont corrects.  
  
    7.  Cliquez sur **OK**.  
  
     Après avoir complété cette page, cliquez sur **Suivant**.  
  
8.  Dans la page **Vérifier le résumé** , sous **Vérifier vos sélections**, développez toutes les options disponibles pour vérifier que tous les paramètres de partition sont corrects. Si tout est conforme à vos attentes, cliquez sur **Terminer**.  
  
9. Sur la page **Progression de l'Assistant Création de partition** , surveillez les informations d'état relatives aux actions de l'Assistant Création de partition. Selon les options sélectionnées dans l'Assistant, la page de progression peut contenir une ou plusieurs actions. La zone supérieure affiche l'état global de l'Assistant et le nombre des messages d'état, d'erreur et d'avertissement qu'il a reçus.  
  
     Les options suivantes sont disponibles sur la page **Progression de l'Assistant Création de partition** :  
  
     **Détails**  
     Indique l'action, l'état et tous les messages retournés suite à l'action entreprise par l'Assistant.  
  
     **Action**  
     Indique le type et le nom de chaque action.  
  
     **État**  
     Indique si l’action de l’Assistant dans son ensemble a retourné la valeur **Réussite** ou **Échec**.  
  
     **Message**  
     Indique les messages d'erreur ou d'avertissement retournés par le processus.  
  
     **Report**  
     Crée un rapport qui contient les résultats de l'Assistant Création de partition. Les options sont **Afficher le rapport**, **Enregistrer le rapport dans un fichier**, **Copier le rapport dans le Presse-papiers** et **Envoyer le rapport sous forme de courrier électronique**.  
  
     **Afficher le rapport**  
     Ouvre la boîte de dialogue **Afficher le rapport** , qui contient un rapport au format texte de la progression de l’Assistant Création de partition.  
  
     **Enregistrer le rapport dans un fichier**  
     Ouvre la boîte de dialogue **Enregistrer le rapport sous** .  
  
     **Copier le rapport dans le Presse-papiers**  
     Copie les résultats du rapport de progression de l’Assistant dans le Presse-papiers.  
  
     **Envoyer le rapport sous forme de courrier électronique**  
     Copie les résultats du rapport de progression de l’Assistant dans un e-mail.  
  
     Une fois terminé, cliquez sur **Fermer**.  
  
 L'Assistant Création de partition crée la fonction de partition et le schéma, puis applique le partitionnement à la table spécifiée. Pour vérifier le partitionnement de table, dans l’Explorateur d’objets, cliquez avec le bouton droit sur la table et sélectionnez **Propriétés**. Cliquez sur la page **Stockage** . La page affiche des informations telles que le nom de la fonction de partition et du schéma, ainsi que le nombre de partitions.  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Utilisation de Transact-SQL  
  
#### <a name="to-create-a-partitioned-table"></a>Pour créer une table partitionnée  
  
1.  Dans l' **Explorateur d'objets**, connectez-vous à une instance du [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
2.  Dans la barre d'outils standard, cliquez sur **Nouvelle requête**.  
  
3.  Copiez et collez l'exemple suivant dans la fenêtre de requête, puis cliquez sur **Exécuter**. L'exemple crée des groupes de fichiers, une fonction de partition et un schéma de partition. Une nouvelle table est créée avec le schéma de partition spécifié comme emplacement de stockage.  
  
    ```  
    USE AdventureWorks2012;  
    GO  
    -- Adds four new filegroups to the AdventureWorks2012 database  
    ALTER DATABASE AdventureWorks2012  
    ADD FILEGROUP test1fg;  
    GO  
    ALTER DATABASE AdventureWorks2012  
    ADD FILEGROUP test2fg;  
    GO  
    ALTER DATABASE AdventureWorks2012  
    ADD FILEGROUP test3fg;  
    GO  
    ALTER DATABASE AdventureWorks2012  
    ADD FILEGROUP test4fg;   
  
    -- Adds one file for each filegroup.  
    ALTER DATABASE AdventureWorks2012   
    ADD FILE   
    (  
        NAME = test1dat1,  
        FILENAME = 'C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\DATA\t1dat1.ndf',  
        SIZE = 5MB,  
        MAXSIZE = 100MB,  
        FILEGROWTH = 5MB  
    )  
    TO FILEGROUP test1fg;  
    ALTER DATABASE AdventureWorks2012   
    ADD FILE   
    (  
        NAME = test2dat2,  
        FILENAME = 'C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\DATA\t2dat2.ndf',  
        SIZE = 5MB,  
        MAXSIZE = 100MB,  
        FILEGROWTH = 5MB  
    )  
    TO FILEGROUP test2fg;  
    GO  
    ALTER DATABASE AdventureWorks2012   
    ADD FILE   
    (  
        NAME = test3dat3,  
        FILENAME = 'C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\DATA\t3dat3.ndf',  
        SIZE = 5MB,  
        MAXSIZE = 100MB,  
        FILEGROWTH = 5MB  
    )  
    TO FILEGROUP test3fg;  
    GO  
    ALTER DATABASE AdventureWorks2012   
    ADD FILE   
    (  
        NAME = test4dat4,  
        FILENAME = 'C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\DATA\t4dat4.ndf',  
        SIZE = 5MB,  
        MAXSIZE = 100MB,  
        FILEGROWTH = 5MB  
    )  
    TO FILEGROUP test4fg;  
    GO  
    -- Creates a partition function called myRangePF1 that will partition a table into four partitions  
    CREATE PARTITION FUNCTION myRangePF1 (int)  
        AS RANGE LEFT FOR VALUES (1, 100, 1000) ;  
    GO  
    -- Creates a partition scheme called myRangePS1 that applies myRangePF1 to the four filegroups created above  
    CREATE PARTITION SCHEME myRangePS1  
        AS PARTITION myRangePF1  
        TO (test1fg, test2fg, test3fg, test4fg) ;  
    GO  
    -- Creates a partitioned table called PartitionTable that uses myRangePS1 to partition col1  
    CREATE TABLE PartitionTable (col1 int PRIMARY KEY, col2 char(10))  
        ON myRangePS1 (col1) ;  
    GO  
    ```  


#### <a name="to-create-a-partitioned-table-in-azure-sqldbesa"></a>Pour créer une table partitionnée dans Azure [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)]

Dans Azure [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)], l’ajout de fichiers et de groupes de fichiers n’est pas pris en charge, mais le partitionnement de table est pris en charge en partitionnant dans le groupe de fichiers PRIMARY uniquement.
  
1.  Dans l' **Explorateur d'objets**, connectez-vous à une instance du [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
1.  Dans la barre d'outils standard, cliquez sur **Nouvelle requête**.  
  
1.  Copiez et collez l'exemple suivant dans la fenêtre de requête, puis cliquez sur **Exécuter**. Cet exemple crée une fonction de partition et un schéma de partition. Une nouvelle table est créée avec le schéma de partition spécifié comme emplacement de stockage. 

    ```
    -- Creates a partition function called myRangePF1 that will partition a table into four partitions  
    CREATE PARTITION FUNCTION myRangePF1 (int)  
        AS RANGE LEFT FOR VALUES (1, 100, 1000) ;  
    GO  
    -- Creates a partition scheme called myRangePS1 that applies myRangePF1 to the PRIMARY filegroup 
    CREATE PARTITION SCHEME myRangePS1  
        AS PARTITION myRangePF1  
        ALL TO ('PRIMARY') ;  
    GO  
    -- Creates a partitioned table called PartitionTable that uses myRangePS1 to partition col1  
    CREATE TABLE PartitionTable (col1 int PRIMARY KEY, col2 char(10))  
        ON myRangePS1 (col1) ;  
    GO
    ```  

  
#### <a name="to-determine-if-a-table-is-partitioned"></a>Pour déterminer si une table est partitionnée  
  
1.  La requête suivante renvoie une ou plusieurs lignes si la table `PartitionTable` est partitionnée. Si la table n'est pas partitionnée, aucune ligne n'est retournée.  
  
    ```  
    SELECT *   
    FROM sys.tables AS t   
    JOIN sys.indexes AS i   
        ON t.[object_id] = i.[object_id]   
        AND i.[type] IN (0,1)   
    JOIN sys.partition_schemes ps   
        ON i.data_space_id = ps.data_space_id   
    WHERE t.name = 'PartitionTable';   
    GO  
    ```  
  
#### <a name="to-determine-the-boundary-values-for-a-partitioned-table"></a>Pour déterminer les valeurs limites pour une table partitionnée  
  
1.  La requête suivante renvoie les valeurs limites pour chaque partition de la table `PartitionTable` .  
  
    ```  
    SELECT t.name AS TableName, i.name AS IndexName, p.partition_number, p.partition_id, i.data_space_id, f.function_id, f.type_desc, r.boundary_id, r.value AS BoundaryValue   
    FROM sys.tables AS t  
    JOIN sys.indexes AS i  
        ON t.object_id = i.object_id  
    JOIN sys.partitions AS p  
        ON i.object_id = p.object_id AND i.index_id = p.index_id   
    JOIN  sys.partition_schemes AS s   
        ON i.data_space_id = s.data_space_id  
    JOIN sys.partition_functions AS f   
        ON s.function_id = f.function_id  
    LEFT JOIN sys.partition_range_values AS r   
        ON f.function_id = r.function_id and r.boundary_id = p.partition_number  
    WHERE t.name = 'PartitionTable' AND i.type <= 1  
    ORDER BY p.partition_number;  
    ```  
  
#### <a name="to-determine-the-partition-column-for-a-partitioned-table"></a>Pour déterminer la colonne de partition pour une table partitionnée  
  
1.  La requête suivante renvoie le nom de la colonne de partitionnement pour une table. `PartitionTable`.  
  
    ```  
    SELECT   
        t.[object_id] AS ObjectID   
        , t.name AS TableName   
        , ic.column_id AS PartitioningColumnID   
        , c.name AS PartitioningColumnName   
    FROM sys.tables AS t   
    JOIN sys.indexes AS i   
        ON t.[object_id] = i.[object_id]   
        AND i.[type] <= 1 -- clustered index or a heap   
    JOIN sys.partition_schemes AS ps   
        ON ps.data_space_id = i.data_space_id   
    JOIN sys.index_columns AS ic   
        ON ic.[object_id] = i.[object_id]   
        AND ic.index_id = i.index_id   
        AND ic.partition_ordinal >= 1 -- because 0 = non-partitioning column   
    JOIN sys.columns AS c   
        ON t.[object_id] = c.[object_id]   
        AND ic.column_id = c.column_id   
    WHERE t.name = 'PartitionTable' ;   
    GO  
    ```  
  
 Pour plus d'informations, consultez les pages suivantes :  
  
-   [Options de fichiers et de groupes de fichiers ALTER DATABASE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql-file-and-filegroup-options.md)  
  
-   [CREATE PARTITION FUNCTION &#40;Transact-SQL&#41;](../../t-sql/statements/create-partition-function-transact-sql.md)  
  
-   [CREATE PARTITION SCHEME &#40;Transact-SQL&#41;](../../t-sql/statements/create-partition-scheme-transact-sql.md)  
  
-   [CREATE TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/create-table-transact-sql.md)  
  
  
