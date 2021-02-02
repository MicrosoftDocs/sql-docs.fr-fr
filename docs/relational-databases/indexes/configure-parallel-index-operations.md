---
description: Configurer des opérations d'index parallèles
title: Configurer des opérations d’index parallèles | Microsoft Docs
ms.custom: ''
ms.date: 02/17/2017
ms.prod: sql
ms.prod_service: table-view-index, sql-database
ms.reviewer: ''
ms.technology: table-view-index
ms.topic: conceptual
helpviewer_keywords:
- index parallel operations [SQL Server]
- processors [SQL Server], parallel index operations
- max degree of parallelism option
- MAXDOP index option, parallel index operations
- parallel index operations [SQL Server]
ms.assetid: 8ec8c71e-5fc1-443a-92da-136ee3fc7f88
author: MikeRayMSFT
ms.author: mikeray
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 41f1cb29f7bd1671166c7eaa0504a7c22c796a30
ms.sourcegitcommit: 04d101fa6a85618b8bc56c68b9c006b12147dbb5
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/29/2021
ms.locfileid: "99048973"
---
# <a name="configure-parallel-index-operations"></a>Configurer des opérations d'index parallèles
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Cette rubrique définit le degré maximal de parallélisme et explique comment modifier ce paramètre dans [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] à l'aide de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] ou de [!INCLUDE[tsql](../../includes/tsql-md.md)]. 

Sur les systèmes multiprocesseurs qui exécutent [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Enterprise ou une version ultérieure, les instructions d'index peuvent, à l'instar d'autres requêtes, utiliser des processeurs multiples (UC) pour réaliser les opérations d'analyse, de tri et d'indexation associées à l'instruction d'index. Le nombre d’UC utilisées pour exécuter une instruction d’index est déterminé par l’option de configuration de serveur [Degré maximal de parallélisme](../../database-engine/configure-windows/configure-the-max-degree-of-parallelism-server-configuration-option.md) , par la charge de travail actuelle et par les statistiques d’index. L'option max degree of parallelism détermine le nombre maximal de processeurs à utiliser au cours de l'exécution d'un plan parallèle. Si le [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] détecte que le système est occupé, le degré de parallélisme de l'opération d'index est automatiquement réduit avant l'exécution de l'instruction. Le [!INCLUDE[ssDE](../../includes/ssde-md.md)] peut également réduire le degré de parallélisme si la colonne clé principale d’un index non partitionné a un nombre limité de valeurs distinctes ou si la fréquence de chaque valeur distincte varie considérablement. Pour plus d’informations, consultez le [Guide d’architecture de traitement des requêtes](../../relational-databases/query-processing-architecture-guide.md#parallel-query-processing). 
  
> [!NOTE]  
> Les opérations d'index parallèles ne sont pas disponibles dans toutes les édition de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Pour plus d’informations, consultez [Fonctionnalités prises en charge par les éditions de SQL Server 2016](../../sql-server/editions-and-components-of-sql-server-2016.md).  
  
 **Dans cette rubrique**  
  
-   **Avant de commencer :**  
  
     [Limitations et restrictions](#Restrictions)  
  
     [Sécurité](#Security)  
  
-   **Pour configurer l'option Degré maximal de parallélisme à l'aide de :**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Avant de commencer  
  
###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> Limitations et restrictions  
  
-   Le nombre de processeurs utilisés par l'optimiseur de requête garantit généralement des performances optimales. Toutefois, des opérations comme la création, la reconstruction ou la suppression d'index volumineux exigent beaucoup de ressources et peuvent, pendant leur exécution, entraîner un manque de ressources pour d'autres opérations d'applications ou de base de données. Lorsque cette situation se produit, vous pouvez configurer manuellement le nombre maximal de processeurs utilisés pour exécuter l'instruction d'index en limitant le nombre de processeurs qui peuvent être utilisés par l'opération d'index.  
  
-   L'option d'index MAXDOP remplace l'option de configuration max degree of parallelism uniquement pour la requête qui la spécifie. Le tableau suivant répertorie les valeurs entières valides qui peuvent être spécifiées au moyen de l'option de configuration max degree of parallelism et de l'option d'index MAXDOP.  
  
    |Valeur|Description|  
    |-----------|-----------------|  
    |0|Spécifie que le serveur détermine le nombre de processeurs utilisés, selon la charge système actuelle. Il s'agit de la valeur par défaut et recommandée.|  
    |1|Supprime la création de plans parallèles. L'opération est exécutée en série.|  
    |2-64|Limite le nombre de processeurs à la valeur spécifiée. Un nombre moins élevé de processeurs peuvent être utilisés en fonction de la charge de travail actuelle. Si une valeur supérieure au nombre d'UC disponibles est spécifiée, c'est le nombre d'UC disponibles qui est utilisé.|  
  
-   L'exécution parallèle d'index et l'option d'index MAXDOP s'appliquent aux instructions [!INCLUDE[tsql](../../includes/tsql-md.md)] suivantes :  
  
    -   [CREATE INDEX](../../t-sql/statements/create-index-transact-sql.md)  
  
    -   [ALTER INDEX (...) REBUILD](../../t-sql/statements/alter-index-transact-sql.md)  
  
    -   [DROP INDEX](../../t-sql/statements/drop-index-transact-sql.md) (index cluster uniquement.)  
  
    -   [ALTER TABLE ADD (index) CONSTRAINT](../../t-sql/statements/alter-table-table-constraint-transact-sql.md) 
  
    -   [ALTER TABLE DROP (index cluster) CONSTRAINT](../../t-sql/statements/alter-table-table-constraint-transact-sql.md)   
  
-   L'option d'index MAXDOP ne peut pas être spécifiée dans l'instruction `ALTER INDEX (...) REORGANIZE`.  
  
-   Les besoins en mémoire des opérations d'index partitionné avec tri peuvent augmenter si l'optimiseur de requête applique des degrés de parallélisme à l'opération de construction. Plus le degré de parallélisme est élevé, plus les besoins en mémoire sont importants. Pour plus d’informations, consultez [Tables et index partitionnés](../../relational-databases/partitions/partitioned-tables-and-indexes.md).  
  
###  <a name="permissions"></a><a name="Security"></a> <a name="Permissions"></a> Autorisations  
 Nécessite l’autorisation `ALTER` sur la table ou la vue.  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Utilisation de SQL Server Management Studio  
  
#### <a name="to-set-max-degree-of-parallelism-on-an-index"></a>Pour définir le degré maximal de parallélisme sur un index  
  
1.  Dans l'Explorateur d'objets, cliquez sur le signe plus (+) pour développer la base de données qui contient la table dans laquelle vous souhaitez spécifier un degré maximal de parallélisme pour un index.  
  
2.  Développez le dossier **Tables** .  
  
3.  Cliquez sur le signe plus (+) pour développer la table sur laquelle vous souhaitez définir le degré maximal de parallélisme pour un index.  
  
4.  Développez le dossier **Index** .  
  
5.  Cliquez avec le bouton droit sur l’index où vous souhaitez définir le degré maximal de parallélisme et sélectionnez **Propriétés**.  
  
6.  Sous **Sélectionner une page**, sélectionnez **Options**.  
  
7.  Sélectionnez **Degré maximal de parallélisme**, puis entrez une valeur comprise entre 1 et 64.  
  
8.  Cliquez sur **OK**.  

##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Utilisation de Transact-SQL  
  
#### <a name="to-set-max-degree-of-parallelism-on-an-existing-index"></a>Pour définir le degré maximal de parallélisme sur un index existant  
  
1.  Dans l' **Explorateur d'objets**, connectez-vous à une instance du [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
2.  Dans la barre d'outils standard, cliquez sur **Nouvelle requête**.  
  
3.  Copiez et collez l'exemple suivant dans la fenêtre de requête, puis cliquez sur **Exécuter**.  
  
    ```sql  
    USE AdventureWorks2012;   
    GO  
    /*Alters the IX_ProductVendor_VendorID index on the Purchasing.ProductVendor table so that, if the server has eight or more processors, the Database Engine will limit the execution of the index operation to eight or fewer processors.  
    */  
    ALTER INDEX IX_ProductVendor_VendorID ON Purchasing.ProductVendor  
    REBUILD WITH (MAXDOP=8);   
    GO  
    ```  
  
 Pour plus d’informations, consultez [ALTER INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/alter-index-transact-sql.md).  
  
#### <a name="set-max-degree-of-parallelism-on-a-new-index"></a>Définir le degré maximal de parallélisme sur un nouvel index  
  
1.  Dans l' **Explorateur d'objets**, connectez-vous à une instance du [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
2.  Dans la barre d'outils standard, cliquez sur **Nouvelle requête**.  
  
3.  Copiez et collez l'exemple suivant dans la fenêtre de requête, puis cliquez sur **Exécuter**.  
  
    ```sql  
    USE AdventureWorks2012;  
    GO  
    CREATE INDEX IX_ProductVendor_NewVendorID   
    ON Purchasing.ProductVendor (BusinessEntityID)  
    WITH (MAXDOP=8);  
    GO  
    ```  
 
## <a name="see-also"></a>Voir aussi
[Guide d’architecture de traitement des requêtes](../../relational-databases/query-processing-architecture-guide.md#parallel-query-processing)    
[CREATE INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/create-index-transact-sql.md)     
[ALTER INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/alter-index-transact-sql.md)     
[DROP INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/drop-index-transact-sql.md)      
[ALTER TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-table-transact-sql.md)      
[ALTER TABLE table_constraint &#40;Transact-SQL&#41;](../../t-sql/statements/alter-table-table-constraint-transact-sql.md)       
[ALTER TABLE index_option &#40;Transact-SQL&#41;](../../t-sql/statements/alter-table-index-option-transact-sql.md)    
