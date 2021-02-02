---
description: Désactiver les index et contraintes
title: Désactiver les index et contraintes | Microsoft Docs
ms.custom: ''
ms.date: 02/17/2017
ms.prod: sql
ms.prod_service: table-view-index, sql-database
ms.reviewer: ''
ms.technology: table-view-index
ms.topic: conceptual
f1_keywords:
- sql13.swb.disableindexes.f1
helpviewer_keywords:
- disabled indexes [SQL Server], index operations
- nonclustered indexes [SQL Server], disabling
- disabled indexes [SQL Server], guidelines
- clustered indexes, disabling
- constraints [SQL Server], disabling
- disabled indexes [SQL Server], viewing
- FOREIGN KEY constraints, disabling
- statistical information [SQL Server], indexes
- index disabling [SQL Server]
- indexed views [SQL Server], disabled indexes
ms.assetid: 2198f1af-fa44-47e9-92df-f4fde322ba18
author: MikeRayMSFT
ms.author: mikeray
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 97718ff757e09cd33116c47f31155f6ae0edb08b
ms.sourcegitcommit: 04d101fa6a85618b8bc56c68b9c006b12147dbb5
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/29/2021
ms.locfileid: "99048892"
---
# <a name="disable-indexes-and-constraints"></a>Désactiver les index et contraintes
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Cette rubrique explique comment désactiver un index ou des contraintes dans [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] à l'aide de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] ou de [!INCLUDE[tsql](../../includes/tsql-md.md)]. La désactivation d'un index empêche l'accès des utilisateurs à celui-ci et, s'il s'agit d'un index cluster, aux données de la table sous-jacente. La définition de l'index reste présente dans les métadonnées et les statistiques sont conservées sur les index non cluster. La désactivation d'un index, qu'il soit non cluster ou cluster, sur une vue supprime physiquement les données de l'index. La désactivation d'un index cluster sur une table empêche l'accès aux données de celle-ci ; ces dernières existent toujours dans la table, mais les opérations de langage de manipulation de données (DML) ne peuvent pas les utiliser tant que l'index n'est pas supprimé ou reconstruit.  
  
 **Dans cette rubrique**  
  
-   **Avant de commencer :**  
  
     [Limitations et restrictions](#Restrictions)  
  
     [Sécurité](#Security)  
  
-   **Pour désactiver un index, utilisez :**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Avant de commencer  
  
###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> Limitations et restrictions  
  
-   L'index n'est pas géré pendant qu'il est désactivé.  
  
-   L'optimiseur de requête ne tient pas compte de l'index désactivé lors de la création de plans d'exécution de requête. En outre, les requêtes qui référencent l'index désactivé avec un indicateur de table échouent.  
  
-   Vous ne pouvez pas créer un index qui utilise le même nom qu'un index désactivé existant.  
  
-   Un index désactivé peut être supprimé.  
  
-   Lorsque vous désactivez un index unique, la contrainte PRIMARY KEY ou UNIQUE et toutes les contraintes FOREIGN KEY qui référencent les colonnes indexées des autres tables sont également désactivées. Lorsque vous désactivez un index cluster, toutes les contraintes FOREIGN KEY entrantes et sortantes sur la table sous-jacente sont également désactivées. Les noms des contraintes sont répertoriés dans un message d'avertissement lorsque l'index est désactivé. Une fois l'index reconstruit, toutes les contraintes doivent être activées manuellement à l'aide de l'instruction ALTER TABLE CHECK CONSTRAINT.  
  
-   Les index non-cluster sont automatiquement désactivés lorsque l'index cluster associé est désactivé. Ils demeurent désactivés tant que l'index cluster de la table ou de la vue n'est pas activé ou que l'index cluster de la table n'est pas supprimé. Les index non-cluster doivent être explicitement activés, sauf si l'index cluster a été activé à l'aide de l'instruction ALTER INDEX ALL REBUILD.  
  
-   L'instruction ALTER INDEX ALL REBUILD recrée et active tous les index désactivés sur la table, sauf les index désactivés sur des vues. Les index sur des vues doivent être activés dans une instruction ALTER INDEX ALL REBUILD distincte.  
  
-   La désactivation d'un index cluster sur une table désactive également tous les index cluster et non cluster sur les vues qui font référence à cette table. Ces index doivent être recréés comme ceux appartenant à la table référencée.  
  
-   Les lignes de données de l'index cluster désactivé ne sont accessibles que pour supprimer ou reconstruire cet index.  
  
-   Vous pouvez reconstruire un index non cluster désactivé en ligne lorsque la table ne possède pas d'index cluster désactivé. Toutefois, vous devez toujours reconstruire un index cluster désactivé hors ligne si vous utilisez l'instruction ALTER INDEX REBUILD ou CREATE INDEX WITH DROP_EXISTING. Pour plus d’informations sur les opérations en ligne sur les index, consultez [Exécuter des opérations en ligne sur les index](../../relational-databases/indexes/perform-index-operations-online.md).  
  
-   L'instruction CREATE STATISTICS ne peut pas être correctement exécutée sur une table qui possède un index cluster désactivé.  
  
-   L'option de base de données AUTO_CREATE_STATISTICS crée de nouvelles statistiques sur une colonne lorsque l'index est désactivé et que les conditions suivantes sont réunies :  
  
    -   AUTO_CREATE_STATISTICS a pour valeur ON.  
  
    -   Il n'existe pas de statistiques sur la colonne.  
  
    -   Des statistiques sont requises pendant l'optimisation de la requête.  
  
-   Si un index cluster est désactivé, [DBCC CHECKDB](../../t-sql/database-console-commands/dbcc-checkdb-transact-sql.md) ne peut pas retourner d'informations sur la table sous-jacente. À la place, l'instruction signale que l'index cluster est désactivé. [DBCC INDEXDEFRAG](../../t-sql/database-console-commands/dbcc-indexdefrag-transact-sql.md) ne peut pas être utilisé pour défragmenter un index désactivé ; l'instruction échoue avec un message d'erreur. Utilisez l'instruction [DBCC DBREINDEX](../../t-sql/database-console-commands/dbcc-dbreindex-transact-sql.md) pour recréer un index désactivé.  
  
-   La création d'un nouvel index cluster active les index non cluster précédemment désactivés. Pour plus d’informations, consultez [Enable Indexes and Constraints](../../relational-databases/indexes/enable-indexes-and-constraints.md).  
  
###  <a name="security"></a><a name="Security"></a> Sécurité  
  
####  <a name="permissions"></a><a name="Permissions"></a> Autorisations  
 Pour pouvoir exécuter l'instruction ALTER INDEX, vous devez obligatoirement bénéficier au minimum d'autorisations nécessaires pour exécuter les instructions ALTER sur la table ou la vue.  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Utilisation de SQL Server Management Studio  
  
#### <a name="to-disable-an-index"></a>Pour désactiver un index  
  
1.  Dans l'Explorateur d'objets, cliquez sur le signe plus (+) pour développer la base de données qui contient la table sur laquelle vous souhaitez désactiver un index.  
  
2.  Cliquez sur le signe plus (+) pour développer le dossier **Tables** .  
  
3.  Cliquez sur le signe plus (+) pour développer la table sur laquelle vous souhaitez désactiver un index.  
  
4.  Cliquez sur le signe plus (+) pour développer le dossier **Index** .  
  
5.  Cliquez avec le bouton droit sur l’index que vous souhaitez désactiver et sélectionnez **Désactiver**.  
  
6.  Dans la boîte de dialogue **Désactiver des index** , vérifiez que l'index correct figure dans la grille **Index à désactiver** et cliquez sur **OK**.  
  
#### <a name="to-disable-all-indexes-on-a-table"></a>Pour désactiver tous les index d'une table  
  
1.  Dans l'Explorateur d'objets, cliquez sur le signe plus (+) pour développer la base de données qui contient la table sur laquelle vous souhaitez désactiver les index.  
  
2.  Cliquez sur le signe plus (+) pour développer le dossier **Tables** .  
  
3.  Cliquez sur le signe plus (+) pour développer la table sur laquelle vous souhaitez désactiver les index.  
  
4.  Cliquez avec le bouton droit sur le dossier **Index** et sélectionnez **Désactiver tout**.  
  
5.  Dans la boîte de dialogue **Désactiver des index** , vérifiez que les index corrects figurent dans la grille **Index à désactiver** et cliquez sur **OK**. Pour supprimer un index de la grille **Index à désactiver** , sélectionnez-le et appuyez sur la touche SUPPR.  
  
 Les informations suivantes sont disponibles dans la boîte de dialogue **Désactiver des index** :  
  
 **Nom de l'index**  
 Affiche le nom de l'index. Durant l'exécution, cette colonne comporte également une icône pour indiquer l'état.  
  
 **Nom du tableau**  
 Affiche le nom de la table ou de la vue sur laquelle l'index a été créé.  
  
 **Type d'index**  
 Affiche le type d’index : **Cluster**, **Non-cluster**, **Spatial** ou **XML**.  
  
 **État**  
 Affiche l'état de l'opération de désactivation. Les valeurs possibles après l'exécution sont les suivantes :  
  
-   Vide  
  
     Avant l'exécution, la valeur d' **État** est vide.  
  
-   **En cours**  
  
     La désactivation d'index a débuté mais elle n'est pas terminée.  
  
-   **Success**  
  
     L'opération de désactivation est achevée.  
  
-   **Error**  
  
     Une erreur est survenue pendant la désactivation d'index et celle-ci n'a pas pu être accomplie.  
  
-   **Arrêté**  
  
     La désactivation d'index n'a pas été accomplie parce que l'utilisateur a interrompu l'opération.  
  
 **Message**  
 Test des messages d'erreur survenant durant la désactivation. Pendant l'exécution, les erreurs sont affichées comme des liens hypertexte. Le corps du message d'erreur est indiqué dans le lien hypertexte. La colonne **Message** est souvent trop étroite pour que la totalité du message soit visible. Deux solutions sont possibles pour afficher l'intégralité du texte :  
  
-   Déplacez le pointeur de la souris sur la cellule du message pour afficher une info-bulle contenant le texte de l'erreur.  
  
-   Cliquez sur le lien hypertexte pour afficher une boîte de dialogue indiquant le message d'erreur.  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Utilisation de Transact-SQL  
  
#### <a name="to-disable-an-index"></a>Pour désactiver un index  
  
1.  Dans l' **Explorateur d'objets**, connectez-vous à une instance du [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
2.  Dans la barre d'outils standard, cliquez sur **Nouvelle requête**.  
  
3.  Copiez et collez l'exemple suivant dans la fenêtre de requête, puis cliquez sur **Exécuter**.  
  
    ```  
    USE AdventureWorks2012;  
    GO  
    -- disables the IX_Employee_OrganizationLevel_OrganizationNode index  
    -- on the HumanResources.Employee table  
    ALTER INDEX IX_Employee_OrganizationLevel_OrganizationNode ON HumanResources.Employee  
    DISABLE;  
    ```  
  
#### <a name="to-disable-all-indexes-on-a-table"></a>Pour désactiver tous les index d'une table  
  
1.  Dans l' **Explorateur d'objets**, connectez-vous à une instance du [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
2.  Dans la barre d'outils standard, cliquez sur **Nouvelle requête**.  
  
3.  Copiez et collez l'exemple suivant dans la fenêtre de requête, puis cliquez sur **Exécuter**.  
  
    ```  
    USE AdventureWorks2012;  
    GO  
    -- Disables all indexes on the HumanResources.Employee table.  
    ALTER INDEX ALL ON HumanResources.Employee  
    DISABLE;  
    ```  
  
 Pour plus d’informations, consultez [ALTER INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/alter-index-transact-sql.md).  
  
  
