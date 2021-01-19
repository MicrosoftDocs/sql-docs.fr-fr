---
title: Assistant Paramétrage du moteur de base de données | Microsoft Docs
description: Découvrez comment utiliser l’Assistant Paramétrage du moteur de base de données pour résoudre les problèmes liés aux requêtes, paramétrer un grand nombre de requêtes, analyser les modifications de conception et gérer l’espace de stockage dans SQL Server.
ms.custom: ''
ms.date: 01/09/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: performance
ms.topic: conceptual
f1_keywords:
- sql13.dta.general.f1
ms.assetid: 50dd0a0b-a407-4aeb-bc8b-b02a793aa30a
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: f3b5176bb0b9ac123090f032e0908d57155ff592
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/13/2021
ms.locfileid: "98172181"
---
# <a name="database-engine-tuning-advisor"></a>Database Engine Tuning Advisor
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  L'Assistant Paramétrage du moteur de base de données [!INCLUDE[msCoName](../../includes/msconame-md.md)] analyse les bases de données et émet des recommandations que vous pouvez utiliser pour optimiser les performances des requêtes. L'Assistant Paramétrage du moteur de base de données vous permet de sélectionner et de créer un ensemble optimal d'index, de vues indexées ou de partitions de table sans devoir être un expert familiarisé avec la structure de la base de données ou les mécanismes internes de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Vous pouvez effectuer les tâches suivantes à l'aide de l'Assistant Paramétrage du moteur de base de données :  
  
-   Résoudre les problèmes liés aux performances d'une requête de problème spécifique  
  
-   Paramétrer un grand nombre de requêtes sur une ou plusieurs bases de données  
  
-   Exécuter une analyse de simulation exploratoire des modifications de conception physique potentielles  
  
-   Gérer l'espace de stockage  
  
## <a name="database-engine-tuning-advisor-benefits"></a>Avantages de l'Assistant Paramétrage du moteur de base de données  
 L'optimisation des performances des requêtes peut être difficile sans une compréhension complète de la structure de la base de données et des requêtes exécutées sur la base de données. L’Assistant **Paramétrage du moteur de base de données (DTA)** peut simplifier cette tâche en analysant le cache du plan de requête actuel ou une charge de travail des requêtes [!INCLUDE[tsql](../../includes/tsql-md.md)] que vous créez, puis en recommandant une conception physique appropriée. Pour les administrateurs de base de données plus avancés, l'Assistant Paramétrage du moteur de base de données expose un mécanisme puissant pour effectuer une analyse de simulation exploratoire des différentes alternatives de conception physique. L'Assistant Paramétrage du moteur de base de données peut fournir les informations suivantes :  
  
-   recommander la meilleure combinaison d’index [columnstore](../../relational-databases/performance/columnstore-index-recommendations-in-database-engine-tuning-advisor-dta.md) pour les bases de données, en utilisant l’optimiseur de requête pour analyser des requêtes dans une charge de travail ;  
  
-   recommander des partitions alignées ou non alignées pour les bases de données référencées dans une charge de travail ;  
  
-   recommander des vues indexées pour les bases de données référencées dans une charge de travail ;  
  
-   analyser les effets des modifications suggérées, notamment l'utilisation des index, la distribution des requêtes au sein des tables et les performances des requêtes dans la charge de travail ;  
  
-   recommander des solutions d'optimisation de la base de données pour un ensemble réduit de requêtes à problème ;  
  
-   vous permettre de personnaliser la recommandation en définissant des options avancées telles que les contraintes d'espace disque ;  
  
-   fournir des rapports qui résument les effets de la mise en œuvre des recommandations pour une charge de travail donnée.  

-   prendre en compte d'autres solutions dans lesquelles vous fournissez des choix de conception possibles sous forme de configurations hypothétiques que l'Assistant Paramétrage du moteur de base de données évalue.

-   paramétrer les charges de travail à partir de diverses sources, notamment le magasin de requêtes SQL Server, le cache du plan, une table ou un fichier de trace SQL Server Profiler, ou encore un fichier .SQL.

  
L'Assistant Paramétrage du moteur de base de données est conçu pour gérer les types de charges de travail de requêtes suivants :  
  
-   Requêtes de traitement transactionnel en ligne (OLTP) uniquement  
  
-   Requêtes de traitement analytique en ligne (OLAP) uniquement  
  
-   Requêtes mixtes OLTP et OLAP  
  
-   Charges de travail à nombre élevé de requêtes (plus de requêtes que de modifications de données)  
  
-   Charges de travail à nombre élevé de mises à jour (plus de modifications de données que de requêtes)  
  
## <a name="dta-components-and-concepts"></a>Composants et concepts liés à l'Assistant Paramétrage du moteur de base de données  
 **Interface graphique utilisateur de l'Assistant Paramétrage du moteur de base de données**  
 Interface simple d'utilisation dans laquelle vous pouvez spécifier la charge de travail et sélectionner plusieurs options de paramétrage.  
  
 Utilitaire **dta**  
 Version d'invite de commandes de l'Assistant Paramétrage du moteur de base de données. L'utilitaire **dta** est conçu pour permettre l'utilisation de l'Assistant Paramétrage du moteur de base de données dans des applications et des scripts.  
  
 **charge de travail**  
 Fichier de script Transact-SQL, fichier de trace ou table de trace qui contient une charge de travail représentative pour les bases de données à paramétrer. À compter de [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)], vous pouvez spécifier le cache du plan comme charge de travail.  À compter de [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)], vous pouvez [spécifier le magasin de requêtes comme charge de travail](../../relational-databases/performance/tuning-database-using-workload-from-query-store.md). 
  
 **Fichier d'entrée XML**  
 Fichier au format XML que l'Assistant Paramétrage du moteur de base de données peut utiliser pour paramétrer les charges de travail. Le fichier d’entrée XML prend en charge les options avancées de paramétrage disponibles dans l’interface utilisateur graphique ou l’utilitaire **dta** .  
  
## <a name="limitations-and-restrictions"></a>Limitations et restrictions  
 L'Assistant Paramétrage du moteur de base de données présente les limitations et restrictions suivantes :  
  
-   Il ne peut pas ajouter ni supprimer des index uniques ou des index qui imposent des contraintes `PRIMARY KEY` ou `UNIQUE`.  
  
-   Il ne peut pas analyser une base de données qui est définie en mode mono-utilisateur.  
  
-   Si, dans le cadre des recommandations pour le paramétrage, vous spécifiez un espace disque maximal supérieur à l'espace disponible réel, l'Assistant Paramétrage du moteur de base de données utilise la valeur que vous indiquez. Toutefois, lorsque vous exécutez le script des recommandations pour les implémenter, il peut échouer si vous n'ajoutez pas d'abord davantage d'espace disque. Vous pouvez spécifier l’espace disque maximal à l’aide de l’option **-B** de l’utilitaire **dta** ou en entrant une valeur dans la boîte de dialogue **Options de paramétrage avancées** .  
  
-   Pour des raisons de sécurité, l'Assistant Paramétrage du moteur de base de données ne peut pas paramétrer une charge de travail dans une table de trace qui se trouve sur un serveur distant. Pour contourner cette limitation, vous pouvez utiliser un fichier de trace au lieu d'une table de trace ou copier la table de trace sur le serveur distant.  
  
-   Quand vous appliquez des contraintes, telles que celles que vous imposez lors de la définition d’un espace disque maximal pour les recommandations pour le paramétrage (à l’aide de l’option **-B** ou de la boîte de dialogue **Options de paramétrage avancées** ), l’Assistant Paramétrage du moteur de base de données peut être amené à supprimer certains index existants. Dans ce cas, les recommandations obtenues de l'Assistant Paramétrage du moteur de base de données peuvent produire une détérioration au lieu de l'amélioration attendue.  
  
-   Quand vous spécifiez une contrainte de limitation de la durée du paramétrage (à l’aide de l’option **-A** de l’utilitaire **dta** ou en activant l’option **Limiter la durée du paramétrage** sous l’onglet **Options de paramétrage** ), l’Assistant Paramétrage du moteur de base de données peut aller au-delà de cette limite pour produire une amélioration particulière attendue ainsi que les rapports d’analyse sur la partie de la charge de travail consommée jusqu’alors, quelle qu’elle soit.  
  
-   L'Assistant Paramétrage du moteur de base de données peut ne pas faire de recommandations dans les circonstances suivantes :  
  
    1.  La table en cours de paramétrage contient moins de 10 pages de données.  
  
    2.  Les index recommandés n'apporteraient pas d'amélioration suffisante aux performances des requêtes par rapport à la conception actuelle de la base de données physique.  
  
    3.  L’utilisateur qui exécute l’Assistant Paramétrage du moteur de base de données n’est pas membre du rôle de base de données **db_owner** ou du rôle de serveur fixe **sysadmin** . Les requêtes présentes dans la charge de travail sont analysées dans le contexte de sécurité de l'utilisateur qui exécute l'Assistant Paramétrage du moteur de base de données. Cet utilisateur doit être membre du rôle de base de données **db_owner** .  
  
-   L’Assistant Paramétrage du moteur de base de données stocke les données de la session de paramétrage et les autres données dans la base de données **msdb** . Si des changements sont apportés à la base de données **msdb** , vous risquez de perdre les données de la session de paramétrage. Pour éliminer ce risque, mettez en œuvre une stratégie de sauvegarde appropriée pour la base de données **msdb** .  
  
## <a name="performance-considerations"></a>Considérations relatives aux performances  
 L'Assistant Paramétrage du moteur de base de données peut consommer des ressources processeur et mémoire significatives au cours de l'analyse. Pour éviter de ralentir le serveur de production, adoptez l'une des stratégies suivantes :  
  
-   Paramétrez les bases de données lorsque le serveur est disponible. L'Assistant Paramétrage du moteur de base de données peut affecter les performances des tâches de maintenance.  
  
-   Utilisez la fonctionnalité serveur de test/serveur de production. Pour plus d’informations, consultez  [Réduire la charge de paramétrage du serveur de production](../../relational-databases/performance/reduce-the-production-server-tuning-load.md).  
  
-   Spécifiez seulement les structures de conception de base de données physique qui doivent être analysées par l'Assistant Paramétrage du moteur de base de données. L'Assistant Paramétrage du moteur de base de données offre de nombreuses options, mais il ne spécifie que celles qui sont nécessaires.  
  
## <a name="dependency-on-xp_msver-extended-stored-procedure"></a>Dépendance vis-à-vis de la procédure stockée étendue xp_msver  
 Pour bénéficier de toutes ses fonctionnalités, l’Assistant Paramétrage du moteur de base de données fait appel à la procédure stockée étendue **xp_msver** . Cette procédure stockée étendue est activée par défaut. L'Assistant Paramétrage du moteur de base de données utilise cette procédure stockée étendue pour déterminer le nombre de processeurs et la quantité de mémoire disponibles sur l'ordinateur où réside la base de données que vous paramétrez. Si **xp_msver** n’est pas disponible, l’Assistant Paramétrage du moteur de base de données définit de façon arbitraire les caractéristiques matérielles de l’ordinateur sur lequel s’exécute l’Assistant Paramétrage du moteur de base de données. Si les caractéristiques matérielles de l'ordinateur sur lequel l'Assistant Paramétrage du moteur de base de données s'exécute ne sont pas disponibles, l'ordinateur est supposé être équipé d'un seul processeur et de 1024 mégaoctets (Mo) de mémoire.  
  
 Cette dépendance présente un impact sur les recommandations en matière de partitionnement, car le nombre de partitions recommandées dépend de ces deux valeurs (nombre de processeurs et quantité de mémoire disponible). Les résultats de votre paramétrage s'en trouvent également affectées lorsque vous paramétrez votre serveur de production par le biais d'un serveur de test. Dans ce scénario, l’Assistant Paramétrage du moteur de base de données utilise **xp_msver** pour déterminer les propriétés matérielles du serveur de production. Après avoir paramétré la charge de travail sur le serveur de test, l'Assistant Paramétrage du moteur de base de données se base sur ces propriétés matérielles pour générer une recommandation. Pour plus d’informations, consultez [xp_msver &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/xp-msver-transact-sql.md).  
  
## <a name="database-engine-tuning-advisor-tasks"></a>Tâches de l'Assistant Paramétrage du moteur de base de données  
 Le tableau suivant répertorie les tâches courantes de l'Assistant Paramétrage du moteur de base de données et les rubriques qui décrivent comment les exécuter.  
  
|Tâche de l'Assistant Paramétrage du moteur de base de données|Rubrique|  
|-----------------------------------------|-----------|  
|Initialiser et démarrer l'Assistant Paramétrage du moteur de base de données.<br /><br /> Créer une charge de travail en spécifiant le cache du plan, en créant un script ou en générant un fichier de trace ou une table de trace.<br /><br /> Paramétrer une base de données à l'aide de l'outil d'interface utilisateur graphique de l'Assistant Paramétrage du moteur de base de données.<br /><br /> Créer des fichiers d'entrée XML pour paramétrer les charges de travail.<br /><br /> Afficher les descriptions des options d'interface utilisateur de l'Assistant Paramétrage du moteur de base de données.|[Démarrer et utiliser l’Assistant Paramétrage du moteur de base de données](../../relational-databases/performance/start-and-use-the-database-engine-tuning-advisor.md)|  
|Afficher les résultats de l'opération de paramétrage de base de données.<br /><br /> Sélectionner et mettre en œuvre des recommandations de paramétrage.<br /><br /> Réaliser une analyse de simulation exploratoire sur la charge de travail.<br /><br /> Examiner les sessions de paramétrage et les sessions de clonage en fonction des sessions existantes <br />ou modifier les recommandations de paramétrage existantes pour une évaluation ou une implémentation plus poussée.<br /><br /> Afficher les descriptions des options d'interface utilisateur de l'Assistant Paramétrage du moteur de base de données.|[Afficher et utiliser la sortie de l’Assistant Paramétrage du moteur de base de données](../../relational-databases/performance/view-and-work-with-the-output-from-the-database-engine-tuning-advisor.md)|  
  
  
