---
title: OLTP en mémoire (optimisation en mémoire) | Microsoft Docs
description: Utilisez ces exemples et ressources pour OLTP en mémoire, ce qui peut améliorer considérablement les performances de SQL Server.
ms.custom: ''
ms.date: 11/21/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: in-memory-oltp
ms.topic: conceptual
helpviewer_keywords:
- In-Memory OLTP
- memory-optimized tables
ms.assetid: e1d03d74-2572-4a55-afd6-7edf0bc28bdb
author: MightyPen
ms.author: genemi
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 8d1dfd8b3d09adb4c3fdb0ad0d2bd02b1044f69a
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/01/2021
ms.locfileid: "100351249"
---
# <a name="in-memory-oltp-and-memory-optimization"></a>OLTP en mémoire et optimisation de la mémoire

[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]

 [!INCLUDE[hek_2](../../includes/hek-2-md.md)] permet d’améliorer considérablement les performances du traitement transactionnel, de l’intégration et du chargement des données, et des scénarios de données temporaires.  Pour accéder au code et aux connaissances de base indispensables pour tester rapidement votre propre table optimisée en mémoire et votre procédure stockée compilée en mode natif, consultez
 -  [Démarrage rapide 1 : technologies OLTP en mémoire pour accélérer les performances Transact-SQL](../../relational-databases/in-memory-oltp/survey-of-initial-areas-in-in-memory-oltp.md).  
 
Nous avons chargé sur YouTube une [**vidéo de 17 minutes**](#anchorname-17minute-video) expliquant l’OLTP en mémoire sur SQL Server et ses avantages en matière de performances.

Pour une présentation plus détaillée de l’OLTP en mémoire et un examen des scénarios où l’utilisation de cette technologie offre des avantages en matière de performances :

- [Vue d’ensemble et scénarios d’utilisation](../../relational-databases/in-memory-oltp/overview-and-usage-scenarios.md)
 
 Notez que [!INCLUDE[hek_2](../../includes/hek-2-md.md)] est la technologie [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] permettant d’améliorer les performances du traitement des transactions. Pour découvrir la technologie [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] qui améliore les performances des requêtes de création de rapports et analytiques, consultez le [Guide des index columnstore](../../relational-databases/indexes/columnstore-indexes-overview.md).
  
 Plusieurs améliorations ont récemment été apportées à la fonction OLTP en mémoire dans [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] et [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)], ainsi que dans [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]. La surface d’exposition Transact-SQL a été augmentée pour simplifier la migration des applications de base de données. Les opérations ALTER sur les tables optimisées en mémoire et les procédures stockées compilées en mode natif sont désormais prises en charge afin de simplifier la gestion des applications.
  
> [!NOTE]  
>  **Faites un essai**  
>   
>  L’OLTP en mémoire est disponible dans les pools élastiques et les bases de données SQL Azure des niveaux Premium et Critique pour l’entreprise. Pour prendre en main l’OLTP en mémoire et Columnstore dans Azure SQL Database, consultez [Optimize Performance using In-Memory Technologies in SQL Database](/azure/azure-sql/in-memory-oltp-overview)(Optimiser les performances à l’aide des technologies en mémoire dans SQL Database).  
  

## <a name="in-this-section"></a>Contenu de cette section  
 Cette section contient les rubriques suivantes :  
  
|Rubrique|Description|  
|-----------|-----------------|  
|[Démarrage rapide 1 : technologies OLTP en mémoire pour accélérer les performances Transact-SQL](../../relational-databases/in-memory-oltp/survey-of-initial-areas-in-in-memory-oltp.md)|Plongez directement au cœur de l’OLTP en mémoire.|
|[Vue d’ensemble et scénarios d’utilisation](../../relational-databases/in-memory-oltp/overview-and-usage-scenarios.md)|Vue d’ensemble de l’OLTP en mémoire et des scénarios où l’utilisation de cette technologie offre des avantages en matière de performances.|
|[Conditions requises pour l’utilisation des tables optimisées en mémoire](../../relational-databases/in-memory-oltp/requirements-for-using-memory-optimized-tables.md)|Décrit les configurations matérielle et logicielle requises et fournit des instructions pour l'utilisation des tables optimisées en mémoire.|  
|[Exemples de code OLTP en mémoire](./sample-database-for-in-memory-oltp.md)|Contient des exemples de code qui montrent comment créer et utiliser une table optimisée en mémoire.|  
|[Tables à mémoire optimisée](./sample-database-for-in-memory-oltp.md)|Présente les tables optimisées en mémoire.|  
|[Variables de table optimisée en mémoire](./faster-temp-table-and-table-variable-by-using-memory-optimization.md)|L'exemple de code illustre comment utiliser une variable de table optimisée en mémoire plutôt qu'une variable de table traditionnelle pour réduire l'utilisation de tempdb.|  
|[Index sur des tables optimisées en mémoire](./indexes-for-memory-optimized-tables.md)|Présente les index optimisés en mémoire.|  
|[Procédures stockées compilées en mode natif](./a-guide-to-query-processing-for-memory-optimized-tables.md)|Présente les procédures stockées compilées en mode natif.|  
|[Gestion de la mémoire pour l’OLTP en mémoire](/previous-versions/sql/sql-server-2016/dn465872(v=sql.130))|Comprendre et gérer l'utilisation de la mémoire sur votre système.|  
|[Création et gestion du stockage des objets à mémoire optimisée](../../relational-databases/in-memory-oltp/creating-and-managing-storage-for-memory-optimized-objects.md)|Traite des fichiers de données et delta, qui stockent les informations sur les transactions dans les tables optimisées en mémoire.|  
|[Sauvegarder, restaurer et récupérer des tables optimisées en mémoire](/previous-versions/sql/sql-server-2016/dn624160(v=sql.130))|Décrit la sauvegarde, la restauration et la récupération des tables optimisées en mémoire.|  
|[Prise en charge de Transact-SQL pour OLTP en mémoire](../../relational-databases/in-memory-oltp/transact-sql-support-for-in-memory-oltp.md)|Décrit la prise en charge [!INCLUDE[tsql](../../includes/tsql-md.md)] pour l' [!INCLUDE[hek_2](../../includes/hek-2-md.md)].|  
|[Prise en charge de la haute disponibilité pour les bases de données OLTP en mémoire](../../relational-databases/in-memory-oltp/high-availability-support-for-in-memory-oltp-databases.md)|Décrit les groupes de disponibilité et le clustering de basculement dans l' [!INCLUDE[hek_2](../../includes/hek-2-md.md)].|  
|[Prise en charge d'OLTP en mémoire par SQL Server](./transact-sql-support-for-in-memory-oltp.md)|Répertorie les nouveautés et les mises à jour en matière de syntaxe et de fonctionnalités prenant en charge les tables optimisées en mémoire.|  
|[Migration vers OLTP en mémoire](./plan-your-adoption-of-in-memory-oltp-features-in-sql-server.md)|Explique comment migrer les tables sur disque vers des tables optimisées en mémoire.|  
| &nbsp; | &nbsp; |

## <a name="links-to-other-websites"></a>Liens vers d’autres sites web

Cette section fournit des liens vers d’autres sites web qui contiennent des informations sur l’OLTP en mémoire dans SQL Server.

- [**Vidéo** expliquant l’OLTP en mémoire et ses avantages en termes de performances](#anchorname-17minute-video)

- [In-Memory OLTP Performance Demo v1.0](https://github.com/Microsoft/sql-server-samples/releases/tag/in-memory-oltp-demo-v1.0)

-   [Livre blanc technique sur les mécanismes internes de la fonction OLTP en mémoire SQL Server](./sql-server-in-memory-oltp-internals-for-sql-server-2016.md)  

-   [Comparaison des fonctionnalités de Columnstore et d’OLTP en mémoire SQL Server](https://download.microsoft.com/download/D/0/0/D0075580-6D72-403D-8B4D-C3BD88D58CE4/SQL_Server_2016_In_Memory_OLTP_and_Columnstore_Comparison_White_Paper.pdf)

-   Nouveautés d’OLTP en mémoire dans SQL Server 2016, [partie 1](/archive/blogs/sqlserverstorageengine/in-memory-oltp-whats-new-in-sql2016-ctp3) et [partie 2](/archive/blogs/sqlserverstorageengine/whats-new-for-in-memory-oltp-in-sql-server-2016-since-ctp3)
  
-   [OLTP en mémoire - Modèles de charge de travail courants et considérations relatives à la migration](/previous-versions/dn673538(v=msdn.10))  
  
-   [Blog OLTP en mémoire](https://cloudblogs.microsoft.com/sqlserver/2013/06/26/sql-server-2014-in-memory-technologies-blog-series-introduction/)  

## <a name="17-minute-video-indexed"></a><a name="anchorname-17minute-video"></a>Vidéo de 17 minutes, indexée

- _Titre de la vidéo :_ &nbsp; **OLTP en mémoire dans SQL Server 2016**
- _Date de publication :_ &nbsp; 2019-03-10, sur `YouTube.com`.
- _Durée :_ &nbsp; 17:32 &nbsp; &nbsp; (Consultez la section [**Index**](#anchorname-index-17minute-video) suivante afin d’obtenir des liens vers la vidéo.)
- _Hébergée par :_ &nbsp; Jos de Bruijn, Chef de programme SQL Server

### <a name="demo-can-be-downloaded"></a>Possibilité de télécharger la démonstration

Au marqueur de temps 08:09, la vidéo exécute deux fois une démonstration. Vous pouvez télécharger le code source de la démonstration exécutable des performances utilisée dans la vidéo à partir du lien suivant :

- [In-Memory OLTP Performance Demo v1.0, code source](https://github.com/Microsoft/sql-server-samples/releases/tag/in-memory-oltp-demo-v1.0)

Les étapes générales présentées dans la vidéo sont les suivantes :

1. Tout d’abord, la démonstration est exécutée avec une table normale.
2. Une édition optimisée en mémoire de la table est ensuite créée et remplie en quelques clics dans SQL Server Management Studio (SSMS.exe).
3. La démonstration est alors réexécutée avec la table optimisée en mémoire. La mesure obtenue indique une amélioration considérable de la vitesse.

### <a name="index-to-each-section-in-the-video"></a><a name="anchorname-index-17minute-video"></a>Index de chaque section de la vidéo

| Marqueur de temps (lien) | Titre de la section |
| :------------- | :------------ |
| A.&nbsp; [00:00](https://www.youtube.com/watch?v=l5l5eophmK4&t=0) | Introduction. |
| <br/>B.&nbsp; [00:56](https://www.youtube.com/watch?v=l5l5eophmK4&t=56) | <br/>Pourquoi les clients doivent s’intéresser à l’OLTP en mémoire. |
| &nbsp; &nbsp; [01:03](https://www.youtube.com/watch?v=l5l5eophmK4&t=63) | Le matériel moderne nécessite une architecture moderne du système de base de données. |
| &nbsp; &nbsp; [02:10](https://www.youtube.com/watch?v=l5l5eophmK4&t=130) | Explosion des données générées : les opérations doivent être instantanées (faible latence). |
| &nbsp; &nbsp; [03:19](https://www.youtube.com/watch?v=l5l5eophmK4&t=199) | Réduction du coût total de possession : en faire plus avec les ressources disponibles. |
| <br/>C.&nbsp; [03:33](https://www.youtube.com/watch?v=l5l5eophmK4&t=213) | <br/>Présentation de l’OLTP en mémoire.<br/>Optimisation des performances au moyen d’une technologie à mémoire optimisée. |
| &nbsp; &nbsp; [05:03](https://www.youtube.com/watch?v=l5l5eophmK4&t=303) | Traitement transactionnel jusqu’à 30 fois plus rapide. |
| &nbsp; &nbsp; [05:22](https://www.youtube.com/watch?v=l5l5eophmK4&t=322) | Durabilité complète : les données résistent aux défaillances du serveur. |
| &nbsp; &nbsp; [06:15](https://www.youtube.com/watch?v=l5l5eophmK4&t=375) | Entièrement intégré à SQL Server. Inutile donc d’apprendre de nouveaux langages ou de nouveaux outils. |
| &nbsp; &nbsp; [07:22](https://www.youtube.com/watch?v=l5l5eophmK4&t=442) | Fonction publiée pour la première fois dans SQL Server 2014, mais qui a fait l’objet d’améliorations majeures en 2016. |
| &nbsp; &nbsp; [07:58](https://www.youtube.com/watch?v=l5l5eophmK4&t=558) | Disponible dans Azure SQL Database également (dans le cloud). |
| <br/>D.&nbsp; [08:09](https://www.youtube.com/watch?v=l5l5eophmK4&t=489) | <br/>Démonstration des performances.<br/> Exécutez la démonstration avec une table normale. |
| &nbsp; &nbsp; [09:11](https://www.youtube.com/watch?v=l5l5eophmK4&t=551) | Menu contextuel de SSMS : **Rapports** &gt; **Analyse des performances de transaction** |
| &nbsp; &nbsp; [10:38](https://www.youtube.com/watch?v=l5l5eophmK4&t=638) | Menu contextuel de SSMS : **Conseil d’optimisation par mémoire**<br/> &nbsp; &nbsp; Créez en fait une table à mémoire optimisée à partir d’une table normale, et migrez les données. |
| &nbsp; &nbsp; [11:28](https://www.youtube.com/watch?v=l5l5eophmK4&t=688) | Réexécutez la démonstration avec cette fois-ci des performances 45 fois supérieures. |
| <br/>E.&nbsp; [12:17](https://www.youtube.com/watch?v=l5l5eophmK4&t=737) | <br/>OLTP en mémoire plus facile à utiliser dans SQL Server 2016 (par rapport à 2014). |
| &nbsp; &nbsp; [12:43](https://www.youtube.com/watch?v=l5l5eophmK4&t=763) | Analyse simplifiée pour faciliter la migration d’applications. |
| &nbsp; &nbsp; [13:03](https://www.youtube.com/watch?v=l5l5eophmK4&t=783) | Réduction de la complexité de la migration des applications grâce à la prise en charge améliorée du langage Transact-SQL (notamment avec des clés étrangères et des déclencheurs). |
| &nbsp; &nbsp; [13:56](https://www.youtube.com/watch?v=l5l5eophmK4&t=836) | Facilité de gestion accrue.<br/> &nbsp; &nbsp; Par exemple, modification du schéma et des index, mise à jour automatique des statistiques. |
| <br/>F.&nbsp; [14:46](https://www.youtube.com/watch?v=l5l5eophmK4&t=886) | <br/>Scalabilité améliorée. |
| &nbsp; &nbsp; [15:12](https://www.youtube.com/watch?v=l5l5eophmK4&t=912) | Tables à mémoire optimisée volumineuses (jusqu’à 2 To par base de données). |
| &nbsp; &nbsp; [15:34](https://www.youtube.com/watch?v=l5l5eophmK4&t=934) | Mise à l’échelle encore plus performante. |
| &nbsp; &nbsp; [16:41](https://www.youtube.com/watch?v=l5l5eophmK4&t=1001) | En faire plus avec les ressources disponibles ! |
| <br/>G.&nbsp; [16:53](https://www.youtube.com/watch?v=l5l5eophmK4&t=1013) | <br/>Commentaires finaux. (Se termine à 17:32.) |
| &nbsp; | &nbsp; |

## <a name="see-also"></a>Voir aussi  
 [Fonctionnalités de base de données](../databases/databases.md)  
