---
title: Afficher les propriétés des statistiques | Microsoft Docs
description: Découvrez comment afficher des statistiques d’optimisation des requêtes actuelles pour une table ou une vue indexée dans SQL Server à l’aide de SQL Server Management Studio ou de Transact-SQL.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: performance
ms.topic: conceptual
f1_keywords:
- sql13.swb.statistics.details.f1
helpviewer_keywords:
- viewing statistics properties
- statistics [SQL Server], viewing properties
ms.assetid: 0eaa2101-006e-4015-9979-3468b50e0aaa
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 440b2e3dd7bcf7d1c7934a32ff95f9cd2299602d
ms.sourcegitcommit: 38e055eda82d293bf5fe9db14549666cf0d0f3c0
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/02/2021
ms.locfileid: "99250218"
---
# <a name="view-statistics-properties"></a>Afficher les propriétés des statistiques
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
  Vous pouvez afficher les statistiques d'optimisation de la requête actuelle pour une table ou une vue indexée dans [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] à l'aide de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] ou de [!INCLUDE[tsql](../../includes/tsql-md.md)]. Les objets de statistiques incluent un en-tête contenant des métadonnées sur les statistiques, un histogramme indiquant la distribution des valeurs dans la première colonne clé de l'objet des statistiques, et un vecteur de densité destiné à mesurer la corrélation entre les colonnes. Pour plus d’informations sur les histogrammes et les vecteurs de densité, consultez [DBCC SHOW_STATISTICS &#40;Transact-SQL&#41;](../../t-sql/database-console-commands/dbcc-show-statistics-transact-sql.md).  
  
 **Dans cette rubrique**  
  
-   **Avant de commencer :**  
  
     [Sécurité](#Security)  
  
-   **Pour afficher les propriétés des statistiques, utilisez :**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Avant de commencer  
  
###  <a name="security"></a><a name="Security"></a> Sécurité  
  
####  <a name="permissions"></a><a name="Permissions"></a> Autorisations  
 Pour afficher l’objet de statistiques, l’utilisateur doit être propriétaire de la table ou être membre du rôle serveur fixe **sysadmin** , du rôle de base de données fixe **db_owner** ou du rôle de base de données fixe **db_ddladmin** .  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Utilisation de SQL Server Management Studio  
  
#### <a name="to-view-statistics-properties"></a>Pour afficher les propriétés des statistiques  
  
1.  Dans l' **Explorateur d'objets**, cliquez sur le signe plus (+) pour développer la base de données dans laquelle vous souhaitez créer une nouvelle statistique.  
  
2.  Cliquez sur le signe plus (+) pour développer le dossier **Tables** .  
  
3.  Cliquez sur le signe plus (+) pour développer la table dans laquelle vous souhaitez afficher les propriétés de la statistique.  
  
4.  Cliquez sur le signe plus (+) pour développer le dossier **Statistiques** .  
  
5.  Cliquez avec le bouton droit sur l’objet Statistiques dont vous voulez afficher les propriétés, puis sélectionnez **Propriétés**.  
  
6.  Dans la boîte de dialogue **Propriétés des statistiques -** _nom_statistiques_, dans le volet **Sélectionner une page** , sélectionnez **Détails**.  
  
     Les propriétés suivantes s’affichent dans la page **Détails** de la boîte de dialogue **Propriétés des statistiques -** _nom_statistiques_.  
  
     **Nom de la table**  
     Affiche le nom de la table décrite par les statistiques.  
  
     **Nom des statistiques**  
     Spécifie le nom de l'objet de base de données dans lequel les statistiques sont stockées.  
  
     **Statistiques de l’INDEX statistics_name**  
     Cette zone de texte affiche les propriétés retournées par l'objet de statistiques. Ces propriétés sont divisées en trois sections : En-tête des statistiques, vecteur de densité et histogramme.  
  
     Les informations suivantes décrivent les colonnes retournées dans le jeu de résultats de l'en-tête de statistiques.  
  
     **Nom**  
     Nom de l'objet de statistiques.  
  
     **Updated**  
     Date et heure de la dernière mise à jour des statistiques.  
  
     **Lignes**  
     Nombre total de lignes dans la table ou la vue indexée au moment de la dernière mise à jour des statistiques. Si les statistiques sont filtrées ou correspondent à un index filtré, le nombre de lignes peut être inférieur à celui de la table.  
  
     **Lignes échantillonnées**  
     Nombre total de lignes échantillonnées pour le calcul des statistiques. Si Rows Sampled < Rows, l'histogramme et les résultats de densité affichés sont des estimations basées sur les lignes échantillonnées.  
  
     **Étapes**  
     Nombre d'étapes dans l'histogramme. Chaque étape couvre une plage de valeurs de colonnes suivie d'une valeur de colonne de limite supérieure. Les étapes d'histogramme sont définies sur la première colonne clé des statistiques. Le nombre maximal d'étapes est 200.  
  
     **Densité**  
     La formule 1 / *valeurs distinctes* est utilisée pour toutes les valeurs de la première colonne clé de l’objet de statistiques, à l’exception des valeurs limites de l’histogramme. Cette valeur de densité n'est pas utilisée par l'optimiseur de requête ; elle est affichée pour la compatibilité descendante avec les versions antérieures à SQL Server 2008.  
  
     **Longueur moyenne d'une clé**  
     Nombre moyen d'octets par valeur pour toutes les colonnes clés de l'objet de statistiques.  
  
     **String Index**  
     La valeur Yes indique que l'objet de statistiques contient des statistiques de résumé de chaîne pour améliorer les estimations de cardinalité des prédicats de requête qui utilisent l'opérateur LIKE ; c'est le cas par exemple de `WHERE ProductName LIKE '%Bike'`. Les statistiques de résumé de chaîne sont stockées à l’écart de l’histogramme et créées sur la première colonne clé de l’objet de statistiques quand il est de type **char**, **varchar**, **nchar**, **nvarchar**, **varchar(max)** , **nvarchar(max)** , **text** ou **ntext**.  
  
     **Expression de filtre**  
     Prédicat pour le sous-ensemble des lignes de table incluses dans l'objet de statistiques. NULL = statistiques non filtrées.  
  
     **Lignes non filtrées**  
     Nombre total de lignes dans la table avant l'application de l'expression de filtre. Si Expression de filtre a la valeur NULL, Lignes non filtrées est égal à Lignes.  
  
     Les informations suivantes décrivent les colonnes retournées dans le jeu de résultats du vecteur de densité.  
  
     **Toutes les densités**  
     La densité est calculée selon la formule 1 / *valeurs distinctes*. Les résultats affichent la densité pour chaque préfixe des colonnes de l'objet de statistiques, à raison d'une ligne par densité. Une valeur distincte est une liste distincte des valeurs de colonnes par ligne et par préfixe de colonne. Par exemple, si l'objet de statistiques contient des colonnes clés (A, B, C), les résultats rapportent la densité des listes distinctes de valeurs dans chacun des préfixes de colonnes suivants : (A), (A,B) et (A, B, C). Avec le préfixe (A, B, C), chacune des listes suivantes est une liste de valeurs distincte : (3, 5, 6), (4, 4, 6), (4, 5, 6), (4, 5, 7). Avec le préfixe (A, B) les listes de valeurs distinctes suivantes sont associées aux mêmes valeurs de colonnes : (3, 5), (4, 4) et (4, 5).  
  
     **Longueur moyenne**  
     Longueur moyenne, en octets, pour le stockage d'une liste des valeurs de colonnes pour le préfixe de colonne. Par exemple, si les valeurs dans la liste (3, 5, 6) nécessitent 4 octets chacune, la longueur est égale à 12 octets.  
  
     **Colonnes**  
     Noms des colonnes dans le préfixe dont les valeurs Toutes les densités et Longueur moyenne sont affichées.  
  
     Les informations suivantes décrivent les colonnes retournées dans le jeu de résultats de l'histogramme.  
  
     **RANGE_HI_KEY**  
     Valeur de colonne de limite supérieure pour une étape d'histogramme. La valeur de colonne est également appelée « valeur de clé ».  
  
     **RANGE_ROWS**  
     Nombre estimé de lignes dont la valeur de colonne est comprise dans une étape d'histogramme, à l'exception de la limite supérieure.  
  
     **EQ_ROWS**  
     Nombre estimé de lignes dont la valeur de colonne est égale à la limite supérieure de l'étape d'histogramme.  
  
     **DISTINCT_RANGE_ROWS**  
     Nombre estimé de lignes ayant une valeur de colonne distincte dans une étape d'histogramme, à l'exception de la limite supérieure.  
  
     **AVG_RANGE_ROWS**  
     Nombre moyen de lignes ayant des valeurs de colonnes dupliquées dans une étape d'histogramme, à l'exception de la limite supérieure (RANGE_ROWS / DISTINCT_RANGE_ROWS pour DISTINCT_RANGE_ROWS > 0).  
  
7.  Cliquez sur **OK**.  

##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Utilisation de Transact-SQL  
  
#### <a name="to-view-statistics-properties"></a>Pour afficher les propriétés des statistiques  
  
1.  Dans l' **Explorateur d'objets**, connectez-vous à une instance du [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
2.  Dans la barre d'outils standard, cliquez sur **Nouvelle requête**.  
  
3.  Copiez et collez l'exemple suivant dans la fenêtre de requête, puis cliquez sur **Exécuter**.  
  
    ```  
    USE AdventureWorks2012;  
    GO  
    -- The following example displays all statistics information for the AK_Address_rowguid index of the Person.Address table.   
    DBCC SHOW_STATISTICS ("Person.Address", AK_Address_rowguid);   
    GO  
    ```  
  
 Pour plus d’informations, consultez [DBCC SHOW_STATISTICS &#40;Transact-SQL&#41;](../../t-sql/database-console-commands/dbcc-show-statistics-transact-sql.md).  
  
#### <a name="to-find-all-of-the-statistics-on-a-table-or-view"></a>Pour rechercher toutes les statistiques sur une table ou une vue  
  
1.  Dans l' **Explorateur d'objets**, connectez-vous à une instance du [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
2.  Dans la barre d'outils standard, cliquez sur **Nouvelle requête**.  
  
3.  Copiez et collez l'exemple suivant dans la fenêtre de requête, puis cliquez sur **Exécuter**.  
  
    ```  
    USE AdventureWorks2012;   
    GO  
    /*Gets the following information: name and ID of the statistics, whether the statistics were created automatically or by the user, whether the statistics were created with the NORECOMPUTE option, and whether the statistics have a filter and, if so, what that filter is.  
    */  
    SELECT name AS statistics_name  
        ,stats_id  
        ,auto_created  
        ,user_created  
        ,no_recompute  
        ,has_filter  
        ,filter_definition  
    -- using the sys.stats catalog view  
    FROM sys.stats  
    -- for the Sales.SpecialOffer table  
    WHERE object_id = OBJECT_ID('Sales.SpecialOffer');  
    GO  
    ```  
  
 Pour plus d’informations, consultez [sys.stats &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-stats-transact-sql.md).  
  
  
