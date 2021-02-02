---
title: Déterminer si une table ou une procédure stockée doit être portée vers l’OLTP en mémoire
description: Utilisez le rapport d’analyse des performances de transaction dans SQL Server Management Studio pour déterminer si l’OLTP en mémoire peut améliorer les performances des applications de base de données.
ms.custom: seo-dt-2019
ms.date: 08/02/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: in-memory-oltp
ms.topic: conceptual
helpviewer_keywords:
- Analyze, Migrate, Report
- AMR
ms.assetid: c1ef96f1-290d-4952-8369-2f49f27afee2
author: MightyPen
ms.author: genemi
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 48d4d4b9602049c0c09e9e60ac8991c5c9a74154
ms.sourcegitcommit: 04d101fa6a85618b8bc56c68b9c006b12147dbb5
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/29/2021
ms.locfileid: "99049237"
---
# <a name="determining-if-a-table-or-stored-procedure-should-be-ported-to-in-memory-oltp"></a>Déterminer si un tableau ou une procédure stockée doit être déplacée vers l'OLTP en mémoire
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Le rapport d’analyse des performances de transaction dans [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] vous aide à évaluer si l’OLTP en mémoire améliore les performances de votre application de base de données. Il vous indique également le volume de travail nécessaire pour activer l'OLTP en mémoire dans votre application. Après avoir identifié une table sur disque pour la fonctionnalité OLTP en mémoire, utilisez le [Conseiller d’optimisation de la mémoire](../../relational-databases/in-memory-oltp/memory-optimization-advisor.md)pour migrer la table. De même, le [Conseiller de compilation native](../../relational-databases/in-memory-oltp/native-compilation-advisor.md) vous aide à déplacer une procédure stockée vers une procédure stockée compilée en mode natif. Pour plus d’informations sur les méthodologies de migration, consultez [OLTP en mémoire - Modèles de charge de travail courants et considérations relatives à la migration](/previous-versions/dn673538(v=msdn.10)).  
  
 Le rapport d’analyse des performances de transaction est exécuté directement sur la base de données de production ou sur une base de données test avec une charge de travail active similaire à la charge de travail de production.  
  
 Le rapport et les conseillers migration vous aident à effectuer les tâches suivantes :  
  
-   Analyser votre charge de travail pour déterminer les points sensibles où l’OLTP en mémoire peut potentiellement améliorer les performances. Le rapport d’analyse des performances de transaction recommande les tables et les procédures stockées qui tireront le plus parti de la migration vers l'OLTP en mémoire.  
  
-   Aide pour la planification et l'exécution de votre migration vers l'OLTP en mémoire. Le chemin de migration d'une table sur disque vers une table mémoire optimisée peut prendre beaucoup de temps. Le Conseiller d'optimisation de la mémoire vous aide à identifier les incompatibilités dans votre table que vous devez supprimer avant de déplacer la table vers l'OLTP en mémoire. Le gestionnaire d'optimisation de la mémoire vous aide également à comprendre l'impact que la migration d'une table vers une table mémoire optimisée aura sur votre application.  
  
     Déterminez si votre application va tirer parti de l'OLTP en mémoire, lorsque vous souhaitez planifier votre migration vers l'OLTP en mémoire et lorsque vous travaillez pour migrer certaines de vos tables et procédures stockées vers l'OLTP en mémoire.  
  
    > [!IMPORTANT]  
    >  Les performances d'un système de base de données dépendent de différents facteurs, tous ne pouvant pas être observés et mesurés par le collecteur de performances de transaction. Par conséquent, le rapport d'analyse des performances de transaction ne garantit pas que les gains de performances réels correspondront aux prédictions, si des prédictions sont faites.  
  
 Le rapport d’analyse des performances de transaction et les conseillers de migration sont installés en même temps que SQL Server Management Studio (SSMS) quand vous sélectionnez **Outils de gestion — De base** ou **Outils de gestion — Avancés** au moment d’installer [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)], ou quand vous [téléchargez SQL Server Management Studio](../../ssms/download-sql-server-management-studio-ssms.md).    
  
## <a name="transaction-performance-analysis-reports"></a>Rapports d’analyse des performances de transaction  
 Vous pouvez générer des rapports d’analyse des performances de transaction dans l’ **Explorateur d’objets** en cliquant avec le bouton droit sur la base de données, en sélectionnant **Rapports**, **Rapports standard**, puis **Présentation de l’analyse des performances des transactions**. La base de données doit avoir une charge de travail active ou une exécution récente d’une charge de travail pour pouvoir générer un rapport d'analyse explicite.  
  
### <a name="tables"></a>Tables
  
 Le rapport détaillé d'une table comprend trois sections :  
  
-   Section des statistiques d'analyse  
  
     Cette section comprend une seule table contenant les statistiques collectées à propos des analyses sur la table de base de données. Les colonnes sont les suivantes :  
  
    -   Pourcentage du total des accès. Pourcentage des analyses et des recherches sur cette table, par rapport à l'activité de la base de données totale. Plus ce pourcentage est élevé, plus la table est sollicitée par rapport aux autres tables de la base de données.  
  
    -   Statistiques de recherche/Statistiques d'analyse de plage. Cette colonne indique le nombre de recherches de point et d'analyses de plage (analyses d'index et de table) effectuées sur la table pendant le profilage. La moyenne par transaction est une estimation.  
    
-   Section des statistiques de contention  
  
     Cette section comprend un tableau indiquant la contention sur la table de base de données. Pour plus d'informations sur les verrous internes et les verrous de base de données, consultez Architecture du verrouillage Les colonnes sont les suivantes :  
  
    -   Pourcentage du total des attentes. Pourcentage des attentes attribuables à un verrou interne et un verrou sur cette table de base de données par rapport à l'activité de la base de données. Plus ce pourcentage est élevé, plus la table est sollicitée par rapport aux autres tables de la base de données.  
  
    -   Statistiques des verrous internes. Ces colonnes indiquent le nombre d'attentes attribuables à un verrou interne pour les requêtes impliquant cette table. Pour plus d'informations sur les verrous internes, consultez Verrouillage interne. Plus ce nombre est élevé, plus il y a de contention de verrous internes sur la table.  
  
    -   Statistiques des verrous. Ce groupe de colonnes indique le nombre d'acquisitions et d'attentes attribuables à des verrous de page pour les requêtes de cette table. Pour plus d'informations sur les verrous, consultez Comprendre le verrouillage dans SQL Server. Plus le nombre d'attentes est élevé, plus il y a de contention de verrous sur la table.  
  
-   Section des difficultés de migration  
  
     Cette section comprend un tableau des difficultés rencontrées pour convertir cette table de base de données en une table mémoire optimisée. Plus le taux de difficultés est élevé, plus il est difficile de convertir la table. Pour afficher les détails de la conversion de cette table de base de données, utilisez le Conseiller d'optimisation de la mémoire.  
  
Les statistiques d'analyse et de contention dans le rapport détaillé de la table sont collectées et agrégées depuis sys.dm_db_index_operational_stats (Transact-SQL).  

### <a name="stored-procedures"></a>Procédures stockées

 Une procédure stockée avec un rapport élevé temps UC/temps écoulé est un candidat pour la migration. Le rapport affiche toutes les références de table, car les procédures stockées compilées en mode natif ne peuvent référencer que les tables mémoire optimisées, ce qui augmente le coût de migration.  
  
 Le rapport détaillé d'une procédure stockée comprend deux sections :  
  
-   Section des statistiques d'exécution  
  
     Cette section comprend un tableau contenant les statistiques collectées à propos des exécutions de la procédure stockée. Les colonnes sont les suivantes :  
  
    -   Heure de mise en cache. Heure à laquelle ce plan d'exécution est mis en cache. Si la procédure stockée supprime le cache du plan et recommence, il y aura des heures pour chaque cache.  
  
    -   Temps processeur total. Temps processeur total consommé par la procédure stockée pendant le profilage. Plus ce nombre est élevé, plus la procédure stockée utilise de temps de processeur.  
  
    -   Durée totale d'exécution. Durée d'exécution totale de la procédure stockée pendant le profilage. Plus la différence entre ce chiffre et le temps processeur est élevée, moins le temps processeur est utilisé efficacement par la procédure stockée.  
  
    -   Total des absences dans le cache. Nombre d’absences dans le cache (lectures depuis un stockage physique) causées par les exécutions de la procédure stockée pendant le profilage.  
  
    -   Nombre d'exécutions. Nombre de fois que la procédure stockée est exécutée pendant le profilage.  
  
-   Section des références de table  
  
     Cette section comprend un tableau répertoriant les tables auxquelles cette procédure stockée se réfère. Avant de convertir la procédure stockée en une procédure stockée compilée en mode natif, toutes ces tables doivent être converties en tables mémoire optimisées, et résider sur le même serveur et la même base de données.  
  
 Les statistiques d'exécution dans le rapport détaillé de la procédure stockée sont collectées et agrégées depuis sys.dm_exec_procedure_stats (Transact-SQL). Les références sont obtenues à partir de sys.sql_expression_dependencies (Transact-SQL).  
  
 Pour afficher les détails sur la conversion d'une procédure stockée en procédure stockée compilée en mode natif, utilisez le Conseiller de compilation native.  
  
## <a name="generating-in-memory-oltp-migration-checklists"></a>Génération des listes de contrôle de migration OLTP en mémoire  
 Les listes de contrôle de migration identifient les fonctionnalités de table ou de procédure stockée non prises en charge avec les tables optimisées en mémoire ou les procédures stockées et compilées en mode natif. L'optimisation de la mémoire et les conseillers de compilation native peuvent générer une liste de contrôle pour une unique table sur disque ou une procédure stockée T-SQL interprétée. Il est également possible de générer des listes de contrôle de migration pour plusieurs tables et procédures stockées dans une base de données.  
  
 Vous pouvez générer une liste de contrôle de migration dans [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] à l’aide de la commande **Générer des listes de contrôle de migration OLTP en mémoire** ou de PowerShell.  
  
**Pour générer une liste de contrôle de migration à l'aide de la commande de l'interface utilisateur**  
  
1.  Dans l’ **Explorateur d’objets**, cliquez avec le bouton droit sur une base de données autre que la base de données système, cliquez sur **Tâches**, puis sur **Générer des listes de contrôle de migration OLTP en mémoire**.  
  
2.  Dans la boîte de dialogue Générer des listes de contrôle de migration OLTP en mémoire, cliquez sur Suivant pour accéder à la page **Configurer les options de génération de listes de contrôle** . Sur cette page, procédez comme suit.  
  
    1.  Entrez un chemin d'accès au dossier dans la zone **Enregistrer la liste de contrôle sous** .  
  
    2.  Vérifiez que l’option **Générer des listes de contrôle pour des tables et des procédures stockées spécifiques** est sélectionnée.  
  
    3.  Développez les nœuds **Table** et **Procédure stockée** .  
  
    4.  Sélectionnez quelques objets dans la zone de sélection.  
  
3.  Cliquez sur **Suivant** et vérifiez que la liste des tâches correspond à vos paramètres sur la page de **configuration des options de génération de liste de contrôle** .  
  
4.  Cliquez sur **Terminer**, puis vérifiez que les rapports de liste de contrôle de migration ont été générés uniquement pour les objets sélectionnés.  

 Vous pouvez vérifier l'exactitude des rapports en les comparant aux rapports générés par l'outil Conseiller d'optimisation de la mémoire et l'outil Conseiller de compilation native. Pour plus d'informations, consultez [Memory Optimization Advisor](../../relational-databases/in-memory-oltp/memory-optimization-advisor.md) et [Native Compilation Advisor](../../relational-databases/in-memory-oltp/native-compilation-advisor.md).  
  
**Pour générer une liste de contrôle de migration à l'aide de SQL Server PowerShell**  
  
1.  Dans l' **Explorateur d'objets**, cliquez sur une base de données, puis sur **Démarrer PowerShell**. Vérifiez que les l’invite suivante apparaît.  
  
    ```  
    PS SQLSERVER: \SQL\{Instance Name}\DEFAULT\Databases\{two-part DB Name}>  
    ```  
  
2.  Entrez la commande ci-après.  
  
    ```  
    Save-SqlMigrationReport -FolderPath "<folder_path>"  
    ```  
  
3.  Vérifiez les éléments suivants.  
  
    -   Le chemin du dossier est créé s’il n’existe pas déjà.  
  
    -   Le rapport de liste de contrôle de migration est généré pour toutes les tables et procédures stockées dans la base de données, et se trouve dans l'emplacement spécifié par folder_path.  
  
**Pour générer une liste de contrôle de migration à l'aide de Windows PowerShell**  
  
1.  Démarrez une session Windows PowerShell avec élévation de privilèges.  
  
2.  Entrez les commandes suivantes. L'objet peut être une table ou une procédure stockée.  
  
    ```  
    [System.Reflection.Assembly]::LoadWithPartialName('Microsoft.SqlServer.SMO')  
  
    ```  
  
    ```  
    Save-SqlMigrationReport -Server "<instance_name>" -Database "<db_name>" -FolderPath "<folder_path1>"  
  
    ```  
  
    ```  
    Save-SqlMigrationReport -Server "<instance_name>" -Database "<db_name>" -Object <object_name> -FolderPath "<folder_path2>"  
  
    ```  
  
3.  Vérifiez les éléments suivants.  
  
    -   Un rapport de liste de contrôle de migration est généré pour toutes les tables et procédures stockées dans la base de données, et se trouve dans l'emplacement spécifié par folder_path.  
  
    -   Un rapport de liste de contrôle de migration pour <object_name> est le seul rapport figurant dans l'emplacement spécifié par folder_path2.  
  
## <a name="see-also"></a>Voir aussi  
 [Migration vers OLTP en mémoire](./plan-your-adoption-of-in-memory-oltp-features-in-sql-server.md)  
  
