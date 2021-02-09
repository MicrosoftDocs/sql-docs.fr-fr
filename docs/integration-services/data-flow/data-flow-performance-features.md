---
description: Fonctionnalités de performances de flux de données
title: Fonctionnalités de performances de flux de données | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
helpviewer_keywords:
- Aggregate transformation [Integration Services]
- Integration Services packages, performance
- performance [Integration Services]
- data flow [Integration Services], troubleshooting
- SQL Server Integration Services packages, performance
- loading data
- control flow [Integration Services], troubleshooting
- SSIS packages, performance
- packages [Integration Services], performance
- queries [Integration Services], troubleshooting
- sorting data [Integration Services]
- aggregations [Integration Services]
ms.assetid: c4bbefa6-172b-4547-99a1-a0b38e3e2b05
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 600ab1bce27f4fa53c2e25ae7562a870f2ccf6a6
ms.sourcegitcommit: 5dcbe4abbe2339292961370c1d8ca3affa625f72
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/04/2021
ms.locfileid: "99536350"
---
# <a name="data-flow-performance-features"></a>Fonctionnalités de performances de flux de données

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  Cette rubrique offre des suggestions pour éviter les problèmes de performances les plus fréquents lors de la conception de packages [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] . Cette rubrique fournit également des informations sur les fonctionnalités et les outils que vous pouvez utiliser pour résoudre des problèmes liés aux performances des packages.  
  
## <a name="configuring-the-data-flow"></a>Configuration du flux de données  
 Pour configurer la tâche de flux de données afin d'obtenir de meilleures performances, vous pouvez configurer les propriétés de la tâche, régler la taille du tampon et configurer le package pour une exécution parallèle.  
  
### <a name="configure-the-properties-of-the-data-flow-task"></a>Configuration des propriétés de la tâche de flux de données  
  
> [!NOTE]  
>  Les propriétés abordées dans cette section doivent être définies séparément pour chaque tâche de flux de données d'un package.  
  
 Vous pouvez configurer les propriétés suivantes de la tâche de flux de données qui affectent toutes les performances :  
  
-   Spécifiez les emplacements de stockage provisoires des données de la mémoire tampon (propriété BufferTempStoragePath) et des colonnes contenant des données d’objets BLOB (propriété BLOBTempStoragePath). Par défaut, ces propriétés contiennent les valeurs des variables d'environnement TEMP et TMP. Vous pouvez préciser d'autres dossiers pour placer les fichiers temporaires sur un autre lecteur de disque dur ou un lecteur plus rapide, ou bien les répartir sur plusieurs lecteurs. Vous pouvez spécifier plusieurs répertoires en séparant leurs noms par un point-virgule.  
  
-   Définissez la taille par défaut de la mémoire tampon utilisée par la tâche en définissant la propriété DefaultBufferSize, puis le nombre maximal de lignes dans chaque mémoire tampon en définissant la propriété DefaultBufferMaxRows. Définissez la propriété AutoAdjustBufferSize pour indiquer si la taille par défaut de la mémoire tampon est calculée automatiquement à partir de la valeur de la propriété DefaultBufferMaxRows. La taille par défaut de la mémoire tampon est de 10 mégaoctets (Mo), avec une taille maximale de 2^31-1 octets. Le nombre maximal de lignes par défaut est 10 000.  
  
-   Définissez le nombre de threads que la tâche peut utiliser pendant l’exécution en définissant la propriété EngineThreads. Cette propriété donne une indication au moteur de flux de données sur le nombre de threads à utiliser. La valeur par défaut est 10 et la valeur minimale 3. Néanmoins, le moteur n'utilise pas plus de threads qu'il n'en faut, quelle que soit la valeur de cette propriété. Si besoin est, pour éviter des problèmes de concurrence, le moteur peut également utiliser plus de threads que le nombre spécifié dans cette propriété.  
  
-   Indiquez si la tâche de flux de données s’exécute en mode optimisé (propriété RunInOptimizedMode). Le mode optimisé améliore les performances en supprimant les colonnes, les sorties et les composants inutilisés du flux de données.  
  
    > [!NOTE]  
    >  Une propriété du même nom, RunInOptimizedMode, peut être définie au niveau du projet dans [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)] pour indiquer que la tâche de flux de données fonctionne en mode optimisé lors du débogage. La propriété du projet remplace la propriété RunInOptimizedMode des tâches de flux de données au moment de la conception.  
  
### <a name="adjust-the-sizing-of-buffers"></a>Régler la taille des tampons  
 Le moteur de flux de données entame le processus de redimensionnement de ses tampons en calculant la taille estimée pour une seule ligne de données. Il multiplie ensuite la taille estimée d’une ligne par la valeur de DefaultBufferMaxRows pour obtenir une valeur de travail préliminaire pour la taille de la mémoire tampon.  
  
-   Si AutoAdjustBufferSize est défini sur true, le moteur de flux de données utilise la valeur calculée comme taille de mémoire tampon, et la valeur de DefaultBufferSize est ignorée.  
  
-   Si AutoAdjustBufferSize est défini sur false, le moteur de flux de données utilise les règles suivantes pour déterminer la taille de la mémoire tampon.  
  
    -   Si le résultat est supérieur à la valeur de DefaultBufferSize, le moteur réduit le nombre de lignes.  
  
    -   Si le résultat est inférieur à la taille de tampon minimale calculée en interne, le moteur augmente le nombre de lignes.  
  
    -   Si le résultat obtenu se situe entre la taille de tampon minimale et la valeur de DefaultBufferSize, le moteur redimensionne la mémoire tampon le plus près possible de la taille de ligne estimée multipliée par la valeur de DefaultBufferMaxRows.  
  
 Quand vous commencez à tester les performances de vos tâches de flux de données, utilisez les valeurs par défaut de DefaultBufferSize et DefaultBufferMaxRows. Activez la journalisation dans la tâche de flux de données et sélectionnez l'événement BufferSizeTuning pour connaître le nombre de lignes figurant dans chaque tampon.  
  
 Avant de régler la taille des tampons, l'amélioration la plus importante à apporter est de réduire la taille de chaque ligne de données en supprimant les colonnes inutiles et en configurant comme il se doit les types de données.  
  
 Pour déterminer la quantité optimale de mémoires tampon et leur taille, faites un essai avec les valeurs de DefaultBufferSize et DefaultBufferMaxRows, tout en surveillant les performances et les informations signalées par l’événement BufferSizeTuning.  
  
 N'augmentez pas la taille du tampon au point de déclencher la pagination sur le disque. Cela aurait des effets plus néfastes sur les performances que la non-optimisation de la taille du tampon. Pour déterminer si la pagination est en cours, surveillez le compteur de performances « Mémoires tampon spoulées » dans le composant logiciel enfichable Performance de la console MMC ( [!INCLUDE[msCoName](../../includes/msconame-md.md)] Management Console).  
  
### <a name="configure-the-package-for-parallel-execution"></a>Configurer le package pour une exécution parallèle  
 L'exécution parallèle améliore les performances sur les ordinateurs dotés de plusieurs processeurs physiques ou logiques. Pour prendre en charge l’exécution parallèle de différentes tâches dans le package, [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] utilise deux propriétés : **MaxConcurrentExecutables** et **EngineThreads**.  
  
#### <a name="the-maxconcurrentexcecutables-property"></a>La propriété MaxConcurrentExcecutables  
 La propriété **MaxConcurrentExecutables** est une propriété du package lui-même. Cette propriété définit le nombre de tâches pouvant s'exécuter simultanément. La valeur par défaut est -1, ce qui correspond au nombre de processeurs physiques ou logiques plus 2.  
  
 Pour comprendre comment cette propriété fonctionne, imaginez un package composé de trois tâches de flux de données. Si vous attribuez à **MaxConcurrentExecutables** la valeur 3, les trois tâches de flux de données peuvent s'exécuter simultanément. Toutefois, supposez que chaque tâche de flux de données comporte 10 arborescences d'exécution de la source vers la destination. Le fait d'attribuer à **MaxConcurrentExecutables** la valeur 3 ne garantit pas que les arborescences d'exécution à l'intérieur de chaque tâche de flux de données s'exécuteront en parallèle.  
  
#### <a name="the-enginethreads-property"></a>La propriété EngineThreads  
 La propriété **EngineThreads** est une propriété de chaque tâche de flux de données. Cette propriété définit le nombre de threads que le moteur de flux de données peut créer et exécuter en parallèle. La propriété **EngineThreads** s'applique aussi bien aux threads sources que le moteur de flux de données crée pour les sources qu'aux threads de travail que le moteur crée pour les transformations et les destinations. Par conséquent, si vous attribuez à **EngineThreads** la valeur 10, le moteur pourra créer jusqu'à dix threads sources et dix threads de travail.  
  
 Pour comprendre comment cette propriété fonctionne, reprenez l'exemple de package composé de trois tâches de flux de données. Chaque tâche de flux de données contient dix arborescences d'exécution de la source vers la destination. Si vous attribuez à EngineThreads la valeur 10 sur chaque tâche de flux de données, les 30 arborescences d'exécution pourront potentiellement s'exécuter simultanément.  
  
> [!NOTE]  
>  Les threads ne sont pas traités dans cette rubrique. Toutefois, la règle générale consiste à ne pas exécuter plus de threads en parallèle que le nombre de processeurs disponibles. Si vous exécutez plus de threads que le nombre de processeurs disponibles, le changement de contexte fréquent entre les threads peut nuire aux performances.  
  
## <a name="configuring-individual-data-flow-components"></a>Configuration de composants de flux de données individuels  
 Pour configurer des composants de flux de données individuels afin d'obtenir de meilleures performances, il y a plusieurs règles générales que vous pouvez suivre. D'autres règles spécifiques s'appliquent également à chaque type de composant de flux de données : source, transformation et destination.  
  
### <a name="general-guidelines"></a>Instructions générales  
 Indépendamment du composant de flux de données, deux règles générales sont à suivre pour améliorer les performances : optimiser les requêtes et éviter les chaînes inutiles.  
  
#### <a name="optimize-queries"></a>Optimisation des requêtes  
 De nombreux composants de flux de données utilisent des requêtes, soit au cours de l'extraction de données à partir de sources, soit au cours d'opérations de recherche dans le but de créer des tables de référence. La requête par défaut utilise la syntaxe SELECT * FROM \<tableName>. Ce type de requête retourne toutes les colonnes dans la table source. Le fait de disposer de toutes les colonnes au moment de la conception permet de choisir n'importe quelle colonne comme colonne de recherche, comme colonne SQL directe ou comme colonne source. Cependant, après avoir sélectionné les colonnes à utiliser, vous devez vérifier la requête et vous assurer qu'elle contient uniquement les colonnes utilisées. La suppression de colonnes superflues permet de créer une ligne plus petite et donc d'accroître l'efficacité du flux de données dans un package. Avec des lignes plus petites, vous pouvez faire tenir plus de lignes dans un tampon et, de ce fait, réduire la charge de travail nécessaire pour traiter toutes les lignes dans le dataset.  
  
 Pour construire une requête, vous pouvez taper la requête ou utiliser le générateur de requêtes.  
  
> [!NOTE]  
>  Lorsque vous exécutez un package dans [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)], l'onglet Progression du concepteur [!INCLUDE[ssIS](../../includes/ssis-md.md)] affiche une liste d'avertissements, y compris un avertissement pour toutes les colonnes de données qu'une source met à la disposition du flux de données mais qui ne sont pas utilisées ensuite par les composants de flux de données en aval. Vous pouvez faire appel à la propriété **RunInOptimizedMode** pour supprimer automatiquement ces colonnes.  
  
#### <a name="avoid-unnecessary-sorting"></a>Suppression des tris non nécessaires  
 Le tri est, par essence, une opération lente et la décision d'éviter un tri inutile peut améliorer les performances du flux de données du package.  
  
 Parfois, les données sources ont déjà été triées avant d'être utilisées par un composant en aval. Ce pré-triage peut avoir lieu soit parce que la requête SELECT utilise une clause ORDER BY, soit parce que les données ont été insérées dans la source par ordre de tri. Pour de telles données sources pré-triées, vous pouvez fournir un indicateur qui précise que les données sont triées afin d'éviter l'utilisation d'une transformation de tri pour satisfaire aux spécifications de tri de certaines transformations en aval. (Par exemple, les transformations de fusion et de jointure de fusion nécessitent des entrées triées). Pour fournir un indicateur qui précise que les données sont triées, vous devez effectuer les tâches suivantes :  
  
-   attribuer à la propriété **IsSorted** sur la sortie d'un composant de flux de données en amont la valeur **True**;  
  
-   spécifier les colonnes clés de tri sur lesquelles les données sont triées.  
  
 Pour plus d’informations, consultez [Trier des données pour les transformations de fusion et de jointure de fusion](../../integration-services/data-flow/transformations/sort-data-for-the-merge-and-merge-join-transformations.md).  
  
 Si vous devez trier les données dans le flux de données, vous pouvez améliorer les performances en concevant le flux de données de façon à utiliser aussi peu d'opérations de tri que possible. Par exemple, le flux de données utilise une transformation de multidiffusion pour copier le dataset. Triez le dataset une fois avant que la transformation de multidiffusion ne s'exécute au lieu de trier plusieurs sorties après la transformation.  
  
 Pour plus d'informations, consultez [Sort Transformation](../../integration-services/data-flow/transformations/sort-transformation.md), [Merge Transformation](../../integration-services/data-flow/transformations/merge-transformation.md), [Merge Join Transformation](../../integration-services/data-flow/transformations/merge-join-transformation.md)et [Multicast Transformation](../../integration-services/data-flow/transformations/multicast-transformation.md).  
  
### <a name="sources"></a>Sources  
  
#### <a name="ole-db-source"></a>Source OLE DB  
 Lorsque vous utilisez une source OLE DB pour extraire les données d'une vue, sélectionnez « Commande SQL » comme mode d'accès aux données et entrez une instruction SELECT. L'utilisation d'une instruction SELECT pour accéder aux données offre de meilleures performances que le mode d'accès aux données « Table ou vue ».  
  
### <a name="transformations"></a>Transformations  
 Utilisez les suggestions de cette section pour améliorer les performances des transformations d'agrégation, de recherche floue, de regroupement probable, de recherche, de jointure de fusion et de dimension à variation lente.  
  
#### <a name="aggregate-transformation"></a>Transformation d'agrégation  
 La transformation d'agrégation inclut les propriétés **Keys**, **KeysScale**, **CountDistinctKeys** et **CountDistinctScale** . Ces propriétés améliorent les performances en permettant à la transformation de préallouer la quantité de mémoire dont la transformation a besoin pour les données que la transformation met en cache. Si vous savez le nombre exact ou approximatif des groupes attendus d'une opération **Group by** , définissez les propriétés **Keys** et **KeysScale** , respectivement. Si vous savez le nombre exact ou approximatif des valeurs distinctes attendues d'une opération **Comptage de valeurs** , définissez les propriétés **CountDistinctKeys** et **CountDistinctScale** , respectivement.  
  
 Si vous devez créer plusieurs agrégations dans un flux de données, songez à créer plusieurs agrégations qui utilisent une transformation d'agrégation au lieu de créer plusieurs transformations. Cette approche améliore les performances lorsqu'une agrégation est un sous-ensemble d'une autre agrégation, car la transformation peut optimiser le stockage interne et analyser une seule fois les données entrantes. Par exemple, si une agrégation utilise une clause GROUP BY et une agrégation AVG, le fait de les combiner en une seule transformation peut améliorer les performances. Toutefois, du fait que la réalisation de plusieurs agrégations au sein d'une transformation d'agrégation sérialise les opérations d'agrégation, il est possible que les performances ne s'améliorent pas lorsque plusieurs agrégations doivent être calculées indépendamment.  
  
#### <a name="fuzzy-lookup-and-fuzzy-grouping-transformations"></a>Transformations de recherche floue et de regroupement probable  
 Pour plus d'informations sur l'optimisation des performances des transformations de recherche floue et de regroupement probable, consultez le livre blanc [Présentation des transformations Fuzzy Lookup (recherche approximative) et Fuzzy Grouping (regroupement approximatif) dans les services DTS (Data Transformation Services) de SQL Server 2005](/previous-versions/sql/sql-server-2005/administrator/ms345128(v=sql.90)).  
  
#### <a name="lookup-transformation"></a>Transformation de recherche  
 Réduisez la taille des données de référence en mémoire en entrant une instruction SELECT qui recherche uniquement les colonnes dont vous avez besoin. Cette approche est plus performante que la sélection d'une table ou d'une vue entière qui retourne une quantité importante de données inutiles.  
  
#### <a name="merge-join-transformation"></a>transformation de jointure de fusion  
 Vous n'avez plus à configurer la valeur de la propriété **MaxBuffersPerInput** car Microsoft a apporté des modifications qui réduisent le risque que la transformation de jointure de fusion consomme de la mémoire en excès. Ce problème s'est quelquefois produit lorsque plusieurs entrées de jointure de fusion produisaient des données à des taux irréguliers.  
  
#### <a name="slowly-changing-dimension-transformation"></a>Transformation de dimension à variation lente  
 L'Assistant Dimension à variation lente et la transformation de dimension à variation lente sont des outils à caractère général qui répondent aux besoins de la plupart des utilisateurs. Toutefois, le flux de données généré par l'Assistant n'est pas optimisé en termes de performances.  
  
 En général, les composants les plus lents de la transformation de dimension à variation lente sont les transformations de commande OLE DB qui effectuent des mises à jour sur une ligne à la fois. Par conséquent, le moyen le plus efficace pour améliorer les performances de la transformation de dimension à variation lente consiste à remplacer les transformations de commande OLE DB. Vous pouvez remplacer ces transformations par des composants de destination qui enregistrent toutes les lignes à mettre à jour dans une table de transit. Ensuite, vous pouvez ajouter une tâche d'exécution SQL qui effectue une opération UPDATE Transact-SQL basée sur un jeu unique sur toutes les lignes en même temps.  
  
 Les utilisateurs expérimentés peuvent concevoir un flux de données personnalisé pour le traitement des dimensions à variation lente qui est optimisé pour les grandes dimensions. Pour en savoir plus et obtenir un exemple de cette approche, consultez la section « Scénario de dimension Unique » dans le livre blanc [Projet REAL: Pratiques de conception ETL Business Intelligence](https://www.microsoft.com/download/details.aspx?id=14582).  
  
### <a name="destinations"></a>Destinations  
 Pour obtenir de meilleures performances avec les destinations, songez à utiliser une destination [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et à tester les performances de la destination.  
  
#### <a name="sql-server-destination"></a>Destination SQL Server  
 Lorsqu'un package charge des données dans une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sur le même ordinateur, utilisez une destination [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Cette destination est optimisée pour les chargements en masse à haute vitesse.  
  
#### <a name="testing-the-performance-of-destinations"></a>Test des performances des destinations  
 L'enregistrement des données sur les destinations peut être plus long que prévu. Pour déterminer si la lenteur provient de l'incapacité de la destination à traiter rapidement des données, vous pouvez provisoirement remplacer la destination par une transformation de calcul du nombre de lignes. Si le débit en sortie s'améliore significativement, il est probable que la destination chargeant les données est la cause du ralentissement.  
  
### <a name="review-the-information-on-the-progress-tab"></a>Vérification des informations de l'onglet Progression  
 [!INCLUDE[ssIS](../../includes/ssis-md.md)] fournit des informations sur le flux de contrôle et le flux de données lorsque vous exécutez un package dans [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)]. L'onglet **Progression** énumère les tâches et les conteneurs par ordre d'exécution et indique les heures de début et de fin, les avertissements et les messages d'erreur pour chaque tâche et chaque conteneur, y compris le package lui-même. Il répertorie également les composants de flux de données par ordre d'exécution et dévoile des informations sur la progression (sous forme de pourcentage) et le nombre de lignes traitées.  
  
 Pour activer ou désactiver l'affichage de messages sous l'onglet **Progression** , basculez l'option **Création de rapports de progression de débogage** dans le menu **SSIS** . La désactivation du rapport de progression peut aider à améliorer les performances lors de l'exécution d'un package complexe dans [!INCLUDE[ssBIDevStudio](../../includes/ssbidevstudio-md.md)].  
  
## <a name="related-tasks"></a>Tâches associées  
  
-   [Trier des données pour les transformations de fusion et de jointure de fusion](../../integration-services/data-flow/transformations/sort-data-for-the-merge-and-merge-join-transformations.md)  
  
## <a name="related-content"></a>Contenu associé  
 **Articles et publications de blog**  
  
-   Article technique, [SQL Server 2005 Integration Services : une stratégie pour de meilleures performances](/previous-versions/sql/sql-server-2005/administrator/cc966530(v=technet.10)), sur le site technet.microsoft.com  
  
-   Article technique, [Integration Services : techniques de réglage des performances](/previous-versions/sql/sql-server-2005/administrator/cc966529(v=technet.10)), sur le site technet.microsoft.com  
  
-   Article technique [Increasing Throughput of Pipelines by Splitting Synchronous Transformations into Multiple Tasks](https://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/SQLCAT's%20Guide%20to%20BI%20and%20Analytics.pdf) dans _SQLCAT's Guide to BI and Analytics_
  
-   Article technique, [Guide des performances de chargement des données](/previous-versions/sql/sql-server-2008/dd425070(v=sql.100)), sur le site msdn.microsoft.com.  
  
-   Article technique, [Nous avons chargé 1 To en 30 minutes avec SSIS, vous le pouvez aussi](/previous-versions/sql/sql-server-2008/dd537533(v=sql.100)), sur le site msdn.microsoft.com.  
  
-   Article technique, [Les 10 meilleures pratiques pour SQL Server Integration Services](https://techcommunity.microsoft.com/t5/datacat/top-10-sql-server-integration-services-best-practices/ba-p/305163), sur le site sqlcat.com.  
  
-   Article technique et exemple, [« Distributeur de données équilibrées » pour SSIS](https://www.sqlshack.com/ssis-balanced-data-distributor-overview/), sur le site sqlcat.com.  
  
-   Publication de blog [Résolution des problèmes de performances des packages SSIS](https://techcommunity.microsoft.com/t5/sql-server-integration-services/api-sample-oledb-source-and-oledb-destination/ba-p/387553)sur blogs.msdn.com.  
  
 **Vidéos**  
  
-   Série de vidéos, [Conception et optimisation des performances de vos packages SSIS en entreprise (Série de vidéos SQL)](https://go.microsoft.com/fwlink/?LinkId=400878)  
  
-   Vidéo, [Optimisation du flux de données de votre package SSIS en entreprise (Vidéo SQL Server)](/previous-versions/ff686901(v=msdn.10)), sur technet.microsoft.com  
  
-   Vidéo, [Présentation des mémoires tampon de flux de données SSIS (Vidéo SQL Server)](/previous-versions/ff686905(v=msdn.10)), sur technet.microsoft.com  
  
-   Vidéo [Modèles de conception des performances de Microsoft SQL Server Integration Services](https://go.microsoft.com/fwlink/?LinkID=233698&clcid=0x409), sur channel9.msdn.com.  
  
-   Présentation, [Exploitation par Microsoft IT des améliorations apportées au moteur de flux de données SQL Server 2008 SSIS](https://channel9.msdn.com/Shows/TechNet+Radio/TechNet-Radio-How-Microsoft-IT-Leverages-SQL-Server-2008-SSIS-Dataflow-Engine-Enhancements), sur le site sqlcat.com.  
  
-   Vidéo, [Distributeur de données équilibrées](/previous-versions/dn912438(v=msdn.10)), sur technet.microsoft.com.  
  
## <a name="see-also"></a>Voir aussi  
 [Outils de dépannage pour le développement des packages](../../integration-services/troubleshooting/troubleshooting-tools-for-package-development.md)   
 [Outils de dépannage pour l’exécution des packages](../../integration-services/troubleshooting/troubleshooting-tools-for-package-execution.md)  
  
