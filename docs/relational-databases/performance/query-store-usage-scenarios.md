---
title: Scénarios d’utilisation du Magasin des requêtes | Microsoft Docs
description: Découvrez comment utiliser le Magasin des requêtes pour suivre et garantir les performances de charge de travail prévisibles. Prenons plusieurs exemples dans SQL Server.
ms.custom: ''
ms.date: 11/29/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: performance
ms.topic: conceptual
helpviewer_keywords:
- Query Store, usage scenarios
ms.assetid: f5309285-ce93-472c-944b-9014dc8f001d
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-current||>=sql-server-2016||= azure-sqldw-latest||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: c65196b3195b8963b6d7c0cf97720b2c4f0d3212
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100347701"
---
# <a name="query-store-usage-scenarios"></a>Scénarios d’utilisation du Magasin des requêtes
[!INCLUDE [SQL Server ASDB, ASDBMI, ASDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa.md)]

  Le magasin de requêtes peut être utilisé dans un vaste ensemble de scénarios quand il est essentiel de suivre et de garantir les performances de charges de travail prévisibles. Voici quelques exemples que vous pouvez examiner :  
  
-   Repérer et résoudre des requêtes avec des régressions de choix de plan  
-   Identifier et paramétrer les principales requêtes consommatrices de ressources  
-   Test A/B  
-   Maintenir la stabilité des performances lors de la mise à niveau vers une version plus récente de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]  
-   Identifier et améliorer les charges de travail ad hoc  
  
## <a name="pinpoint-and-fix-queries-with-plan-choice-regressions"></a>Repérer et résoudre des requêtes avec des régressions de choix de plan  
 Quand il exécute des requêtes classiques, l’optimiseur de requête peut décider de choisir un autre plan, car des entrées importantes ont changé : la cardinalité des données a changé, des index ont été créés, changés ou supprimés, les statistiques ont été mises à jour, etc. Dans l’ensemble, le nouveau plan est mieux ou sensiblement le même que le plan précédemment utilisé. Toutefois, dans certains cas, quand le nouveau plan est nettement plus mauvais, cette situation est qualifiée de « régression due à un changement de plan ». Avant le Magasin des requêtes, il était difficile d’identifier et de résoudre ce problème, car [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ne fournissait pas de magasin de données intégré permettant aux utilisateurs de rechercher les plans d’exécution qui étaient utilisés dans le temps.  
  
 Avec le Magasin des requêtes, vous pouvez rapidement effectuer les opérations suivantes :  
  
-   Identifier toutes les requêtes dont les métriques d’exécution ont été dégradées, au cours de la période digne d’intérêt (dernière heure, journée, semaine, etc.). Utilisez **Requêtes régressées** dans [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] pour accélérer votre analyse.  
  
-   Parmi les requêtes régressées, il est facile de trouver celles qui avaient plusieurs plans et qui ont subi une dégradation en raison du choix d’un plan incorrect. Utilisez le volet **Résumé du plan** dans **Requêtes régressées** pour visualiser tous les plans d’une requête régressée et leurs performances de requête dans le temps.  
  
-   Forcer l’application du plan précédent de l’historique, s’il a été identifié comme étant meilleur. Utilisez le bouton **Forcer le plan** dans **Requêtes régressées** pour forcer l’application du plan sélectionné pour la requête.  
  
 ![Capture d’écran du Magasin des requêtes montrant un résumé du plan](../../relational-databases/performance/media/query-store-usage-1.png "requête-magasin-utilisation-1")  
  
 Pour une description détaillée du scénario, reportez-vous au blog [Query Store: A flight data recorder for your database](https://azure.microsoft.com/blog/query-store-a-flight-data-recorder-for-your-database/).  
  
## <a name="identify-and-tune-top-resource-consuming-queries"></a>Identifier et paramétrer les principales requêtes consommatrices de ressources  
 Même si votre charge de travail peut générer des milliers de requêtes, seules quelques-unes d’entre elles utilisent généralement la plupart des ressources système et, par conséquent, nécessitent une attention particulière. Parmi les principales requêtes consommatrices de ressources, vous trouvez généralement les requêtes qui ont fait l’objet d’une régression ou celles qui peuvent être améliorées avec un paramétrage supplémentaire.  
  
 La façon la plus simple de commencer l’exploration consiste à ouvrir **Principales requêtes consommatrices de ressources** dans [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)]. L’interface utilisateur est divisée en trois volets : un histogramme représentant les principales requêtes consommatrices de ressources (à gauche), un résumé du plan pour la requête sélectionnée (à droite) et un plan de requête visuel pour le plan sélectionné (en bas). Cliquez sur le bouton **Configurer** pour contrôler le nombre de requêtes que vous voulez analyser et l’intervalle de temps digne d’intérêt. Par ailleurs, vous pouvez choisir entre différentes dimensions de consommation de ressources (durée, processeur, mémoire, E/S, nombre d’exécutions) et la ligne de base (Moyenne, Min, Max, Total, Écart type).  
  
 ![Capture d’écran du Magasin des requêtes montrant comment identifier et paramétrer les requêtes les plus consommatrices de ressources](../../relational-databases/performance/media/query-store-usage-2.png "requête-magasin-utilisation-2")  
  
 Examinez le résumé du plan situé à droite pour analyser l’historique d’exécution et en savoir plus sur les différents plans et leurs statistiques d’exécution. Utilisez le volet inférieur pour examiner les différents plans ou les comparer visuellement, en les affichant côte à côte (à l’aide du bouton Comparer).  
  
Quand vous identifiez une requête dont les performances ne sont pas optimales, votre action dépend de la nature du problème :  
  
1.  Si la requête a été exécutée avec plusieurs plans et que le dernier est nettement plus mauvais que le précédent, vous pouvez utiliser le mécanisme de forçage d’application du plan pour garantir que [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] utilisera le plan optimal pour les exécutions futures.  
  
2.  Vérifiez si l’optimiseur suggère des index absents dans le plan XML. Si tel est le cas, créez les index absents et utilisez le magasin de requêtes pour évaluer les performances des requêtes après la création de l’index.  
  
3.  Vérifiez que les statistiques sont à jour pour les tables sous-jacentes utilisées par la requête.  
  
4.  Vérifiez que les index utilisés par la requête sont défragmentés.  
  
5.  Envisagez de récrire la requête coûteuse. Par exemple, profitez du paramétrage des requêtes et réduisez l’utilisation d’instructions SQL dynamiques. Implémentez une logique optimale lors de la lecture de données (appliquez le filtrage des données côté base de données, et non pas côté application).  

## <a name="ab-testing"></a>Test A/B  
 Utilisez le magasin de requêtes pour comparer les performances de la charge de travail avant et après la modification d’application que vous prévoyez d’introduire. La liste suivante contient plusieurs exemples où vous pouvez utiliser le magasin de requêtes pour évaluer l’impact de la modification de l’environnement ou de l’application sur les performances de la charge de travail :  
  
-   Déploiement d’une nouvelle version de l’application.  
  
-   Ajout de nouveau matériel pour le serveur.  
  
-   Création d’index manquants sur des tables référencées par des requêtes coûteuses.  
  
-   Application d’une stratégie de filtrage pour la sécurité au niveau des lignes. Pour plus d’informations, consultez [Optimizing Row Level Security with Query Store](/archive/blogs/sqlsecurity/optimizing-rls-performance-with-the-query-store) (Optimisation de la sécurité au niveau des lignes avec le Magasin des requêtes).  
  
-   L’ajout d’un contrôle de version du système temporel aux tables qui sont fréquemment modifiées par vos applications OLTP.  
  
Dans chacun de ces scénarios, appliquez le flux de travail suivant :  
  
1.  Exécutez votre charge de travail avec le magasin de requêtes avant la modification planifiée pour générer une ligne de base des performances.  
  
2.  Appliquez la modification d’application au moment contrôlé.  
  
3.  Poursuivez l’exécution de la charge de travail suffisamment longtemps pour générer l’image de performances du système après la modification.  
  
4.  Comparez les résultats obtenus aux étapes 1 et 3.  
  
    1.  Ouvrez **Consommation globale de la base de données** pour déterminer l’impact sur l’ensemble de la base de données.  
  
    2.  Ouvrez **Principales requêtes consommatrices de ressources** (ou exécutez votre propre analyse à l’aide de [!INCLUDE[tsql](../../includes/tsql-md.md)]) pour analyser l’impact de la modification sur les requêtes les plus importantes.  
  
5.  Décidez s’il faut conserver la modification ou effectuer une restauration dans le cas où les nouvelles performances ne seraient pas acceptables.  
  
L’illustration suivante montre l’analyse du magasin de requêtes (étape 4) en cas de création d’index absents. Ouvrez **Principales requêtes consommatrices de ressources** / Volet Résumé du plan pour obtenir l’affichage suivant pour la requête qui doit être affectée par la création d’index :  
  
![Capture d’écran de l’analyse du Magasin des requêtes (étape 4) en cas de création d’index manquant](../../relational-databases/performance/media/query-store-usage-3.png "requête-magasin-utilisation-3")  
  
De plus, vous pouvez comparer les plans avant et après la création d’index en les affichant côte à côte. (Utilisez l’option « Comparez les plans pour la requête sélectionnée dans une fenêtre distincte » de la barre d’outils, signalée par un carré rouge.)  
  
![Capture d’écran du Magasin des requêtes et de l’option Comparer les plans pour la requête sélectionnée dans une fenêtre distincte de la barre d’outils](../../relational-databases/performance/media/query-store-usage-4.png "requête-magasin-utilisation-4")  
  
Le plan avant la création d’index (plan_id = 1, au-dessus) a un indicateur d’index absents et vous pouvez vérifier que l’option Analyse d’index cluster était l’opérateur le plus coûteux dans la requête (rectangle rouge).  
  
Le plan après la création d’index absents (plan_id = 15, en dessous) a maintenant une option Recherche d’index (non cluster) qui réduit le coût global de la requête et améliore ses performances (rectangle vert).  
  
En fonction de l’analyse, vous conservez généralement les index, car les performances de la requête ont été améliorées.  
  
## <a name="keep-performance-stability-during-the-upgrade-to-newer-ssnoversion"></a><a name="CEUpgrade"></a> Maintenir la stabilité des performances lors de la mise à niveau vers une version plus récente de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]  
Avant [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)], les utilisateurs étaient exposés au risque d’une régression des performances lors de la mise à niveau vers la dernière version de la plateforme. Cela était dû au fait que la dernière version de l’optimiseur de requête devenait immédiatement actif une fois les nouveaux éléments installés.  
  
À compter de [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)], tous les changements de l’optimiseur de requête sont liés au [niveau de compatibilité de base de données](../../relational-databases/databases/view-or-change-the-compatibility-level-of-a-database.md) le plus récent, de sorte que les plans ne sont pas changés au moment même de la mise à niveau, mais quand un utilisateur remplace le `COMPATIBILITY_LEVEL` par le plus récent. Cette fonctionnalité, en association avec le magasin de requêtes, vous offre un niveau de contrôle élevé sur les performances des requêtes dans le processus de mise à niveau. Le flux de travail de mise à niveau recommandé est illustré dans l’image suivante :  
  
![Diagramme montrant le workflow de mise à niveau recommandé.](../../relational-databases/performance/media/query-store-usage-5.png "requête-magasin-utilisation-5")  
  
1.  Mettez à niveau [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sans changer le niveau de compatibilité de base de données. Cela n’expose pas les derniers changements de l’optimiseur de requête, mais fournit quand même les fonctionnalités [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] les plus récentes, notamment le Magasin des requêtes.  
  
2.  Activez le Magasin des requêtes. Pour plus d’informations sur ce sujet, consultez [Ajuster le Magasin des requêtes à votre charge de travail](../../relational-databases/performance/best-practice-with-the-query-store.md#Configure).

3.  Autorisez le Magasin des requêtes à capturer des requêtes et des plans, puis établissez une base de référence des performances avec le niveau de compatibilité de base de données source/précédent. Restez à cette étape suffisamment longtemps pour capturer tous les plans et obtenir une base de référence stable. Cela peut être la durée d’un cycle d’activité habituel pour une charge de travail de production.  
  
4.  Passez au niveau de compatibilité de base de données le plus récent : exposez votre charge de travail aux derniers changements de l’optimiseur de requête et laissez-le éventuellement créer des plans.  
  
5.  Utilisez le Magasin des requêtes pour l’analyse et la correction des régressions : dans l’ensemble, les changements de l’optimiseur de requête doivent produire de meilleurs plans. Toutefois, le Magasin des requêtes est un moyen facile d’identifier les régressions de choix de plan et de les corriger à l’aide du mécanisme de forçage d’application du plan. Avec [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)], cette étape est désormais automatique grâce à la fonctionnalité [Correction du plan automatique](../../relational-databases/automatic-tuning/automatic-tuning.md#automatic-plan-correction).  

    a.  En cas de régression, forcez le précédent plan connu adéquat dans le Magasin des requêtes.  
  
    b.  Si des plans de requête ne peuvent pas être forcés ou si les performances restent insuffisantes, envisagez de revenir au [niveau de compatibilité de la base de données](../../relational-databases/databases/view-or-change-the-compatibility-level-of-a-database.md) précédent et de faire appel au support technique Microsoft.  
    
> [!TIP]
> Utilisez la tâche *Mettre à niveau la base de données* de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] pour mettre à niveau le [niveau de compatibilité de la base de données](../../t-sql/statements/alter-database-transact-sql-compatibility-level.md#compatibility-levels-and-database-engine-upgrades). Consultez [Mise à niveau des bases de données à l’aide de l’Assistant Paramétrage de requête](../../relational-databases/performance/upgrade-dbcompat-using-qta.md) pour plus d’informations.
  
## <a name="identify-and-improve-ad-hoc-workloads"></a>Identifier et améliorer les charges de travail ad hoc  
Certaines charges de travail n’ont pas de requêtes dominantes que vous pouvez ajuster pour améliorer les performances globales de l’application. Ces charges de travail se caractérisent généralement par un nombre relativement important de requêtes différentes, chacune consommant une partie des ressources système. Chacune étant unique, ces requêtes sont exécutées très rarement (généralement une seule fois, d’où leur nom de « requêtes ad hoc »). Leur consommation d’exécution n’est donc pas critique. Par ailleurs, étant donné que l’application génère en permanence de nouvelles requêtes, une part importante des ressources système sont consacrées à la compilation des requêtes, ce qui n’est pas optimal. Cette situation n’est pas non plus idéale pour le magasin de requêtes car un grand nombre de requêtes et de plans inondent l’espace que vous avez réservé. De ce fait, il est probable que le magasin de requêtes se retrouvera très rapidement en mode lecture seule. Si vous avez activé **Stratégie de nettoyage basée sur la taille** ([fortement recommandé](best-practice-with-the-query-store.md) pour toujours maintenir le magasin de requêtes activé et en cours d’exécution), le processus en arrière-plan nettoie les structures du magasin de requêtes qui, la plupart du temps, utilisent également d’importantes ressources système.  
  
 L’affichage **Principales requêtes consommatrices de ressources** vous donne une première indication de la nature ad hoc de votre charge de travail :  
  
![Capture d’écran de l’affichage Requêtes les plus consommatrices de ressources montrant que la majorité d’entre elles ne sont exécutées qu’une seule fois](../../relational-databases/performance/media/query-store-usage-6.png "requête-magasin-utilisation-6")  
  
Utilisez la métrique **Nombre d’exécutions** pour analyser si vos requêtes principales sont ad hoc (vous devez, pour cela, exécuter le Magasin des requêtes avec `QUERY_CAPTURE_MODE = ALL`). Dans le diagramme ci-dessus, vous pouvez voir que 90 % de vos **principales requêtes consommatrices de ressources** sont exécutées une seule fois.  
  
Vous pouvez également exécuter un script [!INCLUDE[tsql](../../includes/tsql-md.md)] pour obtenir le nombre total de textes de requêtes, de requêtes et de plans dans le système, et déterminer dans quelle mesure elles sont différentes en comparant les valeurs query_hash et plan_hash :  
  
```sql  
--Do cardinality analysis when suspect on ad hoc workloads
SELECT COUNT(*) AS CountQueryTextRows FROM sys.query_store_query_text;  
SELECT COUNT(*) AS CountQueryRows FROM sys.query_store_query;  
SELECT COUNT(DISTINCT query_hash) AS CountDifferentQueryRows FROM  sys.query_store_query;  
SELECT COUNT(*) AS CountPlanRows FROM sys.query_store_plan;  
SELECT COUNT(DISTINCT query_plan_hash) AS  CountDifferentPlanRows FROM  sys.query_store_plan;  
```  
  
Voici un résultat potentiel que vous pouvez obtenir en cas de charge de travail avec des requêtes ad hoc :  
  
![Capture d’écran du résultat potentiel en cas de charge de travail comportant des requêtes ad hoc](../../relational-databases/performance/media/query-store-usage-7.png "requête-magasin-utilisation-7")  
  
Le résultat des requêtes montre que, malgré le grand nombre de requêtes et de plans dans le Magasin des requêtes, leurs valeurs query_hash et query_plan_hash ne sont, en fait, pas différentes. Un rapport entre les textes de requêtes uniques et les hachages de requêtes uniques nettement supérieur à 1 indique que la charge de travail est un candidat approprié pour le paramétrage, car la seule différence entre les requêtes est une constante littérale (paramètre) fournie en tant que partie du texte de la requête.  
  
En général, cette situation se produit si votre application génère des requêtes (au lieu d’appeler des procédures stockées ou des requêtes paramétrables), ou si elle s’appuie sur des infrastructures de mappage relationnel objet qui génèrent des requêtes par défaut.  
  
Si vous contrôlez le code d’application, vous pouvez envisager de récrire la couche d’accès aux données pour utiliser des procédures stockées ou des requêtes paramétrables. Toutefois, il est possible d’améliorer considérablement cette situation sans apporter de modifications à l’application en forçant le paramétrage des requêtes pour l’ensemble de la base de données (toutes les requêtes) ou pour les modèles de requête individuels avec la même valeur query_hash.  
  
L’approche avec des modèles de requête individuels requiert la création d’un repère de plan :  
  
```sql  
--Apply plan guide for the selected query template 
DECLARE @stmt nvarchar(max);  
DECLARE @params nvarchar(max);  
EXEC sp_get_query_template   
    N'<your query text goes here>',  
    @stmt OUTPUT,   
    @params OUTPUT;  
  
EXEC sp_create_plan_guide   
    N'TemplateGuide1',   
    @stmt,   
    N'TEMPLATE',   
    NULL,   
    @params,   
    N'OPTION (PARAMETERIZATION FORCED)';  
```  
  
La solution avec les repères de plan est plus précise, mais elle nécessite plus de travail.  
  
Si toutes vos requêtes (ou la plupart d’entre elles) sont idéales pour un autoparamétrage, il peut être préférable d’opter pour la modification de `FORCED PARAMETERIZATION` pour l’ensemble de la base de données :  
  
```sql  
--Apply forced parameterization for entire database  
ALTER DATABASE <database name> SET PARAMETERIZATION FORCED;  
```  

> [!NOTE]
> Pour plus d’informations à ce sujet, consultez [Principes d’utilisation du paramétrage forcé](../../relational-databases/query-processing-architecture-guide.md#ForcedParamGuide).

Après avoir appliqué l’une de ces étapes, l’option **Principales requêtes consommatrices de ressources** présente une image différente de votre charge de travail.  
  
![Capture d’écran de l’affichage Requêtes les plus consommatrices de ressources montrant une image différente de la charge de travail](../../relational-databases/performance/media/query-store-usage-8.png "requête-magasin-utilisation-8")  
  
Dans certains cas, votre application peut générer beaucoup de requêtes qui ne sont pas idéales pour un autoparamétrage. Un grand nombre de requêtes s’affichent alors dans le système, mais le rapport entre les requêtes uniques et les valeurs `query_hash` uniques est probablement proche de 1.  
  
Dans ce cas, vous pouvez activer l’option de serveur [**Optimiser pour les charges de travail ad hoc**](../../database-engine/configure-windows/optimize-for-ad-hoc-workloads-server-configuration-option.md) afin d’éviter de gaspiller la mémoire cache sur des requêtes qui ne seront probablement pas réexécutées. Pour empêcher la capture de ces requêtes dans le magasin de requêtes, définissez `QUERY_CAPTURE_MODE` sur `AUTO`.  
  
```sql  
EXEC sys.sp_configure N'show advanced options', N'1' RECONFIGURE WITH OVERRIDE
GO
EXEC sys.sp_configure N'optimize for ad hoc workloads', N'1'
GO
RECONFIGURE WITH OVERRIDE
GO 
  
ALTER DATABASE [QueryStoreTest] SET QUERY_STORE CLEAR;  
ALTER DATABASE [QueryStoreTest] SET QUERY_STORE = ON   
    (OPERATION_MODE = READ_WRITE, QUERY_CAPTURE_MODE = AUTO);  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Analyse des performances à l'aide du magasin de requêtes](../../relational-databases/performance/monitoring-performance-by-using-the-query-store.md)   
 [Bonnes pratiques relatives au magasin de requêtes](../../relational-databases/performance/best-practice-with-the-query-store.md)         
 [Mise à niveau des bases de données à l’aide de l’Assistant Paramétrage de requête](../../relational-databases/performance/upgrade-dbcompat-using-qta.md)           
