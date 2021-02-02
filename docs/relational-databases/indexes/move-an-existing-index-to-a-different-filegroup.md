---
description: Déplacer un index existant dans un autre groupe de fichiers
title: Déplacer un index existant dans un autre groupe de fichiers | Microsoft Docs
ms.custom: ''
ms.date: 02/17/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: table-view-index
ms.topic: conceptual
helpviewer_keywords:
- moving tables
- switching filegroups for index
- moving indexes
- indexes [SQL Server], moving
- filegroups [SQL Server], switching
ms.assetid: 167ebe77-487d-4ca8-9452-4b2c7d5cb96e
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: 4ccacb2b46b609b61485821f250644596f0e3955
ms.sourcegitcommit: 04d101fa6a85618b8bc56c68b9c006b12147dbb5
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/29/2021
ms.locfileid: "99048882"
---
# <a name="move-an-existing-index-to-a-different-filegroup"></a>Déplacer un index existant dans un autre groupe de fichiers
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Cette rubrique explique comment déplacer un index existant d'un groupe de fichiers à un autre à l'aide de [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] en utilisant [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] ou [!INCLUDE[tsql](../../includes/tsql-md.md)].  
  
 **Dans cette rubrique**  
  
-   **Avant de commencer :**  
  
     [Limitations et restrictions](#Restrictions)  
  
     [Sécurité](#Security)  
  
-   **Pour placer un index existant dans un autre groupe de fichiers à l'aide de :**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Avant de commencer  
  
###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> Limitations et restrictions  
  
-   Si une table possède un index cluster, le déplacement de celui-ci vers un nouveau groupe de fichiers entraîne le déplacement de la table vers ce groupe de fichiers.  
  
-   Vous ne pouvez pas déplacer des index créés à l'aide de la contrainte UNIQUE ou PRIMARY KEY en utilisant [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)]. Pour déplacer ces index, utilisez l’instruction [CREATE INDEX](../../t-sql/statements/create-index-transact-sql.md) avec l’option (DROP_EXISTING=ON) dans [!INCLUDE[tsql](../../includes/tsql-md.md)].  
  
###  <a name="security"></a><a name="Security"></a> Sécurité  
  
####  <a name="permissions"></a><a name="Permissions"></a> Autorisations  
 Nécessite une autorisation ALTER sur la table ou la vue. L’utilisateur doit être membre du rôle serveur fixe **sysadmin** ou des rôles de base de données fixes **db_ddladmin** et **db_owner** .  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Utilisation de SQL Server Management Studio  
  
#### <a name="to-move-an-existing-index-to-a-different-filegroup-using-table-designer"></a>Pour placer un index existant dans un autre groupe de fichiers à l'aide du Concepteur de tables  
  
1.  Dans l'Explorateur d'objets, cliquez sur le signe plus (+) pour développer la base de données qui contient la table avec l'index que vous souhaitez déplacer.  
  
2.  Cliquez sur le signe plus (+) pour développer le dossier **Tables** .  
  
3.  Cliquez avec le bouton droit sur la table avec l’index que vous souhaitez déplacer et sélectionnez **Conception**.  
  
4.  Dans le menu **Concepteur de tables** , cliquez sur **Index/Clés**.  
  
5.  Sélectionnez l'index à déplacer.  
  
6.  Dans la grille principale, développez **Spécification de l'espace de données**.  
  
7.  Sélectionnez **Nom du schéma de partition ou du groupe de fichiers** , puis sélectionnez dans la liste le groupe de fichiers ou le schéma de partition où vous souhaitez déplacer l'index.  
  
8.  Cliquez sur **Fermer**.  
  
9. Dans le menu **Fichier** , sélectionnez **Enregistrer**_nom_table_.  

#### <a name="to-move-an-existing-index-to-a-different-filegroup-in-object-explorer"></a>Pour placer un index existant dans un autre groupe de fichiers dans l'Explorateur d'objets  
  
1.  Dans l'Explorateur d'objets, cliquez sur le signe plus (+) pour développer la base de données qui contient la table avec l'index que vous souhaitez déplacer.  
  
2.  Cliquez sur le signe plus (+) pour développer le dossier **Tables** .  
  
3.  Cliquez sur le signe plus (+) pour développer la table contenant l'index à déplacer.  
  
4.  Cliquez sur le signe plus (+) pour développer le dossier **Index** .  
  
5.  Cliquez avec le bouton droit sur l’index à déplacer, puis sélectionnez **Propriétés**.  
  
6.  Sous **Sélectionner une page**, sélectionnez **Stockage**.  
  
7.  Sélectionnez le groupe de fichiers vers lequel vous souhaitez déplacer l'index.  
  
     Si la table ou l'index est partitionné(e), sélectionnez le schéma de partition vers lequel déplacer l'index. Pour plus d'informations sur les index partitionnés, consultez [Partitioned Tables and Indexes](../../relational-databases/partitions/partitioned-tables-and-indexes.md).  
  
     Si vous déplacez un index cluster, vous pouvez procéder en ligne. Le traitement en ligne autorise l'accès des utilisateurs aux données sous-jacentes et aux index non cluster pendant l'opération sur l'index. Pour plus d'informations, consultez [Perform Index Operations Online](../../relational-databases/indexes/perform-index-operations-online.md).  
  
     Sur les ordinateurs multiprocesseurs qui utilisent [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)], vous pouvez configurer le nombre de processeurs utilisés pour exécuter l'instruction sur l'index en spécifiant un degré maximal de parallélisme. La fonctionnalité permettant les opérations d'index parallèles ne sont pas disponibles dans toutes les édition de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Pour obtenir la liste des fonctionnalités prises en charge par les éditions de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], consultez les fonctionnalités prises en charge par les éditions de SQL Server 2016. Pour plus d’informations sur les opérations d’index parallèles, consultez [Configurer des opérations d’index parallèles](../../relational-databases/indexes/configure-parallel-index-operations.md).  
  
8.  Cliquez sur **OK**.  
  
 Les informations suivantes sont disponibles dans la page **Stockage** de la boîte de dialogue **Propriétés de l’index -** _nom_index_ :  
  
 **Groupe de fichiers**  
 Stocke l'index dans le groupe de fichiers spécifié. La liste répertorie uniquement les groupes de fichiers standard (row). Le groupe de fichiers PRIMARY de la base de données est sélectionné par défaut dans la liste.  
  
 **Groupe de fichiers Filestream**  
 Spécifie le groupe de fichiers pour les données FILESTREAM. Cette liste affiche uniquement des groupes de fichiers FILESTREAM. Le groupe de fichiers sélectionné par défaut dans la liste est le groupe PRIMARY FILESTREAM.  
  
 **Schéma de partition**  
 Stocke l'index dans un schéma de partition. En cliquant sur **Schéma de partition** , vous activez la grille ci-dessous. Le schéma de partition sélectionné par défaut dans la liste est celui qui est utilisé pour stocker les données de la table. Si vous sélectionnez un autre schéma de partition dans la liste, les informations affichées dans la grille sont actualisées.  
  
 L'option Schéma de partition n'est pas disponible s'il n'y a pas de schémas de partition dans la base de données.  
  
 **Schéma de partition Filestream**  
 Spécifie le schéma de partition utilisé pour les données FILESTREAM. Ce schéma de partition doit être symétrique avec celui spécifié dans l'option **Schéma de partition** .  
  
 Si la table n'est pas partitionnée, le champ est vide.  
  
 **Paramètre du schéma de partition**  
 Affiche le nom de la colonne qui participe au schéma de partition.  
  
 **Colonne de table**  
 Sélectionnez la table ou vue à mapper avec le schéma de partition.  
  
 **Type de données de la colonne**  
 Affiche des informations sur les types de données figurant dans la colonne.  
  
> [!NOTE]  
>  Si la colonne de table est une colonne calculée, **Type de données de la colonne** contient la mention « colonne calculée ».  
  
 **Autoriser le traitement en ligne des instructions DML lors du déplacement de l'index**  
 Permet aux utilisateurs d'accéder à la table sous-jacente ou aux données des index cluster et à tous les index non-cluster associés pendant l'opération d'index.  
  
> [!NOTE]  
>  Cette option n'est pas disponible pour les index XML ou si l'index est un index cluster désactivé.  
  
 **Définir le degré maximal de parallélisme**  
 Limite le nombre de processeurs à utiliser pendant l'exécution des plans parallèles. La valeur par défaut est 0, indiquant que le nombre réel de processeurs disponibles est utilisé. Spécifier la valeur 1 supprime la génération d'un plan parallèle ; spécifier une valeur supérieure à 1 limite le nombre maximal de processeurs utilisés au cours de l'exécution d'une requête simple. Cette option est disponible seulement si la boîte de dialogue est dans l'état **Reconstruire** ou **Recréer** .  
  
> [!NOTE]  
>  Si une valeur supérieure au nombre d'UC disponibles est spécifiée, le nombre réel d'UC est utilisé.  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Utilisation de Transact-SQL  
  
#### <a name="to-move-an-existing-index-to-a-different-filegroup"></a>Pour placer un index existant dans un autre groupe de fichiers  
  
1.  Dans l' **Explorateur d'objets**, connectez-vous à une instance du [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
2.  Dans la barre d'outils standard, cliquez sur **Nouvelle requête**.  
  
3.  Copiez et collez l'exemple suivant dans la fenêtre de requête, puis cliquez sur **Exécuter**.  
  
    ```  
    USE AdventureWorks2012;  
    GO  
    -- Creates the TransactionsFG1 filegroup on the AdventureWorks2012 database  
    ALTER DATABASE AdventureWorks2012  
    ADD FILEGROUP TransactionsFG1;  
    GO  
    /* Adds the TransactionsFG1dat3 file to the TransactionsFG1 filegroup. Please note that you will have to change the filename parameter in this statement to execute it without errors.  
    */  
    ALTER DATABASE AdventureWorks2012   
    ADD FILE   
    (  
        NAME = TransactionsFG1dat3,  
        FILENAME = 'C:\Program Files\Microsoft SQL Server\MSSQL13\MSSQL\DATA\TransactionsFG1dat3.ndf',  
        SIZE = 5MB,  
        MAXSIZE = 100MB,  
        FILEGROWTH = 5MB  
    )  
    TO FILEGROUP TransactionsFG1;  
    GO  
    /*Creates the IX_Employee_OrganizationLevel_OrganizationNode index  
      on the TransactionsPS1 filegroup and drops the original IX_Employee_OrganizationLevel_OrganizationNode index.  
    */  
    CREATE NONCLUSTERED INDEX IX_Employee_OrganizationLevel_OrganizationNode  
        ON HumanResources.Employee (OrganizationLevel, OrganizationNode)  
        WITH (DROP_EXISTING = ON)  
        ON TransactionsFG1;  
    GO  
    ```  
  
 Pour plus d’informations, consultez [CREATE INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/create-index-transact-sql.md).  
  
  
