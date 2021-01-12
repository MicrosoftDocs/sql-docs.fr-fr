---
title: Mettre à niveau des bases de données à l’aide de l’Assistant Paramétrage de requêtes
description: Découvrez comment l’Assistant Paramétrage de requêtes vous guide tout au long du workflow recommandé pour maintenir la stabilité des performances lors des mises à niveau vers des versions plus récentes de SQL Server.
ms.custom: seo-dt-2019
ms.date: 02/13/2019
ms.prod: sql
ms.reviewer: wiassaf
ms.technology: performance
ms.topic: conceptual
f1_keywords:
- sql13.swb.querytuning.f1
helpviewer_keywords:
- query statistics [SQL Server] live query stats
- live query statistics
- debugging [SQL Server], live query stats
- statistics [SQL Server], live query statistics
- query profiling
- lightweight query profiling
- lightweight profiling
ms.assetid: 07f8f594-75b4-4591-8c29-d63811e7753e
author: pmasl
ms.author: pelopes
manager: amitban
ms.openlocfilehash: 08e105eac49b8377e128aa1cc746de1eebdee389
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/11/2021
ms.locfileid: "98102214"
---
# <a name="upgrading-databases-by-using-the-query-tuning-assistant"></a>Mise à niveau des bases de données à l’aide de l’Assistant Paramétrage de requête

[!INCLUDE[tsql-appliesto-ss2016-xxxx-xxxx-xxx-md.md](../../includes/tsql-appliesto-ss2016-xxxx-xxxx-xxx-md.md)]

Quand vous migrez d’une ancienne version de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] vers [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] ou version ultérieure et que vous passez au tout dernier [niveau de compatibilité de la base de données](../../relational-databases/databases/view-or-change-the-compatibility-level-of-a-database.md), il est possible que les performances d’une charge de travail fassent l’objet d’une régression. Cela est également possible (à un degré moindre) lors de la mise à niveau entre [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] et les versions plus récentes.

À compter de [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)], et avec chaque nouvelle version, tous les changements de l’optimiseur de requête sont liés au niveau de compatibilité de base de données le plus récent, de sorte que les plans d’exécution ne sont pas changés au moment même de la mise à niveau, mais quand un utilisateur remplace l’option de base de données `COMPATIBILITY_LEVEL` par la plus récente disponible. Pour plus d’informations sur les modifications de l’optimiseur de requête introduites dans [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)], consultez [Estimateur de cardinalité](../../relational-databases/performance/cardinality-estimation-sql-server.md). Pour plus d’informations sur les niveaux de compatibilité et leur impact sur les mises à niveau, consultez [Niveaux de compatibilité et mises à niveau du moteur de base de données](../../t-sql/statements/alter-database-transact-sql-compatibility-level.md#compatibility-levels-and-database-engine-upgrades).

Cette fonctionnalité de liaison fournie par le niveau de compatibilité de base de données, en association avec le Magasin des requêtes, procure un niveau élevé de contrôle sur les performances des requêtes dans le processus de mise à niveau si celle-ci respecte le flux de travail recommandé ci-dessous. Pour plus d’informations sur le flux de travail recommandé pour la mise à niveau du niveau de compatibilité, consultez [Modifier le mode de compatibilité de base de données et utiliser le magasin des requêtes](../../database-engine/install-windows/change-the-database-compatibility-mode-and-use-the-query-store.md). 

![Workflow de mise à niveau de base de données recommandé à l’aide du Magasin des requêtes](../../relational-databases/performance/media/query-store-usage-5.png "Workflow de mise à niveau de base de données recommandé à l’aide du Magasin des requêtes") 

Ce contrôle sur les mises à niveau a encore été amélioré avec [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)], qui a introduit le [réglage automatique](../../relational-databases/automatic-tuning/automatic-tuning.md). Celui-ci autorise l’automatisation de la dernière étape du flux de travail recommandé ci-dessus.

À compter de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] v18, la nouvelle fonctionnalité **Assistant Paramétrage de requêtes** guide les utilisateurs tout le long du flux de travail recommandé pour maintenir la stabilité des performances pendant les mises à niveau vers des versions plus récentes de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], comme décrit dans la section *Maintenir la stabilité des performances lors de la mise à niveau vers une version plus récente de SQL Server* de [Scénarios d’utilisation du Magasin des requêtes](../../relational-databases/performance/query-store-usage-scenarios.md#CEUpgrade). Cependant, l’Assistant Paramétrage de requêtes ne revient pas à un bon plan antérieur comme dans la dernière étape du workflow recommandé. Il repère les régressions trouvées dans l’affichage [**Requêtes en régression** Magasin des requêtes](../../relational-databases/performance/monitoring-performance-by-using-the-query-store.md#Regressed) et parcourt les permutations possibles des variations du modèle d’optimiseur applicable dans le but de produire un meilleur plan.

> [!IMPORTANT]
> L’Assistant Paramétrage de requêtes ne génère pas de charge de travail utilisateur. Si vous exécutez l’Assistant Paramétrage de requêtes dans un environnement qui n’est pas utilisé par vos applications, vérifiez que vous pouvez néanmoins exécuter des charges de travail de test représentatives sur le [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] ciblé par d’autres moyens.

## <a name="the-query-tuning-assistant-workflow"></a>Workflow de l’Assistant Paramétrage de requêtes

Le point de départ de l’Assistant Paramétrage de requêtes est la supposition qu’une base de données d’une version précédente de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] est déplacée (à l’aide de [CREATE DATABASE... FOR ATTACH](../..//relational-databases/databases/attach-a-database.md) ou [RESTORE](../../t-sql/statements/restore-statements-transact-sql.md)) vers une version plus récente du [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)], et que le niveau de compatibilité de base de données avant la mise à niveau n’est pas changé immédiatement. L’Assistant Paramétrage de requêtes vous aide à effectuer les étapes suivantes :

1. Configurer le Magasin des requêtes conformément aux paramètres recommandés pour la durée de la charge de travail (en jours) définie par l’utilisateur Réfléchir à la durée de la charge de travail qui correspond à votre cycle d’entreprise classique
2. Demander le démarrage de la charge de travail requise, afin que le Magasin des requêtes puisse collecter une base de référence de données de charge de travail (si aucune n’est encore disponible)
3. Procéder à la mise à niveau vers le niveau de compatibilité de base de données cible choisi par l’utilisateur
4. Demander à ce qu’une deuxième série de données de charge de travail soit collectée à des fins de comparaison et de détection de régression
5. Itérer les régressions détectées d’après la vue [Magasin des requêtes **Requêtes régressées**](../../relational-databases/performance/monitoring-performance-by-using-the-query-store.md#Regressed), effectuer des expérimentations en recueillant des statistiques d’exécution concernant les permutations possibles des variations de modèle d’optimiseur applicables, et mesurer le résultat 
6. Obtenir des rapports sur les améliorations mesurées, et éventuellement autoriser la persistance de ces modifications à l’aide de [repères de plan](../../relational-databases/performance/plan-guides.md)

Pour plus d’informations sur l’attachement d’une base de données, consultez [Attacher et détacher une base de données](../../relational-databases/databases/database-detach-and-attach-sql-server.md#AttachDb).

Voir ci-dessous comment l’Assistant Paramétrage de requêtes ne fait que changer les dernières étapes du workflow recommandé pour la mise à niveau du niveau de compatibilité à l’aide du Magasin des requêtes mentionné ci-dessus. Au lieu d’offrir la possibilité de choisir entre le plan d’exécution inefficace actuel et le dernier bon plan d’exécution connu, l’Assistant Paramétrage de requêtes présente des options de paramétrage propres aux requêtes en régression sélectionnées, afin de créer un nouvel état amélioré avec des plans d’exécution paramétrés.

![Workflow de mise à niveau de base de données recommandé à l’aide de l’Assistant Paramétrage de requêtes](../../relational-databases/performance/media/qta-usage.png "Workflow de mise à niveau de base de données recommandé à l’aide de l’Assistant Paramétrage de requêtes")

### <a name="qta-tuning-internal-search-space"></a>Espace de recherche interne de paramétrage de l’Assistant Paramétrage de requêtes

L’Assistant Paramétrage de requêtes cible uniquement les requêtes `SELECT` qui peuvent être exécutés à partir du Magasin des requêtes. Les requêtes paramétrables sont éligibles si le paramètre compilé est connu. Les requêtes qui dépendent des constructions au moment de l’exécution, telles que les tables temporaires ou les variables de table, ne sont pas éligibles à l’heure actuelle.

L’Assistant Paramétrage de requêtes cible les modèles possibles connus de régressions de requêtes dues aux changements de version de l’[estimateur de cardinalité](../../relational-databases/performance/cardinality-estimation-sql-server.md). Par exemple, lors de la mise à niveau d’une base de données à partir de [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] et du niveau de compatibilité de base de données 110 vers [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] et le niveau de compatibilité de base de données 140, certaines requêtes peuvent régresser car elles ont été conçues spécifiquement pour fonctionner avec la version de l’estimateur de cardinalité qui existait dans [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] (estimateur de cardinalité 70). Cela ne signifie pas que repasser de l’estimateur de cardinalité 140 à l’estimateur de cardinalité 70 est l’unique option. Si seul un changement spécifique dans la nouvelle version introduit la régression, il est possible de faire en sorte (à l’aide d’un indicateur) que cette requête utilise uniquement la partie pertinente de la version précédente de l’estimateur de cardinalité qui fonctionnait mieux pour la requête spécifique, tout en tirant parti de toutes les autres améliorations offertes par les versions plus récentes de l’estimateur de cardinalité. Vous pouvez aussi faire en sorte que d’autres requêtes dans la charge de travail qui n’ont pas subi de régression tirent parti des améliorations du nouvel estimateur de cardinalité.

Les modèles d’estimateur de cardinalité recherchés par l’Assistant Paramétrage de requêtes sont les suivants :

- **Indépendance ou corrélation** : si l’hypothèse de l’indépendance fournit de meilleures estimations pour la requête en question, l’indicateur de requête `USE HINT ('ASSUME_MIN_SELECTIVITY_FOR_FILTER_ESTIMATES')` fait en sorte que [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] génère un plan d’exécution à l’aide d’une sélectivité minimale lors de l’estimation des prédicats `AND` pour les filtres afin de prendre en compte la corrélation. Pour plus d’informations, consultez [Indicateurs de requête USE HINT](../../t-sql/queries/hints-transact-sql-query.md#use_hint) et [Versions de l’estimateur de cardinalité](../../relational-databases/performance/cardinality-estimation-sql-server.md#versions-of-the-ce).
- **Autonomie simple ou autonomie de base** : si une autonomie de jointure différente fournit de meilleures estimations pour la requête en question, l’indicateur de requête `USE HINT ('ASSUME_JOIN_PREDICATE_DEPENDS_ON_FILTERS')` fait en sorte que [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] génère un plan d’exécution à l’aide de l’hypothèse Autonomie simple plutôt que l’hypothèse Autonomie de base par défaut. Pour plus d’informations, consultez [Indicateurs de requête USE HINT](../../t-sql/queries/hints-transact-sql-query.md#use_hint) et [Versions de l’estimateur de cardinalité](../../relational-databases/performance/cardinality-estimation-sql-server.md#versions-of-the-ce).
- **Estimation de cardinalité fixe des fonctions table à instructions multiples (MSTVF)** de 100 lignes ou 1 ligne : si l’estimation fixe par défaut pour les TVF de 100 lignes ne donne pas un plan plus efficace que l’utilisation de l’estimation fixe d’1 ligne pour les TVF (correspondant à la valeur par défaut sous le modèle d’estimateur de cardinalité de l’optimiseur de requête de [!INCLUDE[ssKilimanjaro](../../includes/ssKilimanjaro-md.md)] et versions antérieures), l’indicateur de requête `QUERYTRACEON 9488` est utilisé pour générer un plan d’exécution. Pour plus d’informations sur les MSTVF, consultez [Créer des fonctions définies par l’utilisateur &#40;moteur de base de données&#41;](../../relational-databases/user-defined-functions/create-user-defined-functions-database-engine.md#TVF).

> [!NOTE]
> En dernier recours, si les indicateurs à étendue étroite ne génèrent pas de résultats suffisamment bons pour les modèles de requête éligibles, l’utilisation complète de l’estimateur de cardinalité 70 est également considérée, en utilisant l’indicateur de requête `USE HINT ('FORCE_LEGACY_CARDINALITY_ESTIMATION')` pour générer un plan d’exécution.

> [!IMPORTANT]
> Tout indicateur force certains comportements qui pourront être traités dans les mises à jour ultérieures de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Nous vous recommandons d’appliquer des indicateurs uniquement quand il n’existe aucune autre option, et de revoir le code avec indicateur lors de chaque nouvelle mise à niveau. En forçant des comportements, vous risquez d’empêcher votre charge de travail de profiter des améliorations introduites dans les dernières versions de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].

## <a name="starting-query-tuning-assistant-for-database-upgrades"></a>Démarrage de l’Assistant Paramétrage de requêtes pour les mises à niveau de base de données

L’Assistant Paramétrage de requêtes est une fonctionnalité basée sur la session qui stocke l’état de session dans le schéma `msqta` de la base de données utilisateur où une session est créée pour la première fois. Vous pouvez créer plusieurs sessions de paramétrage sur une même base de données au fil du temps, mais il ne peut exister qu’une seule session active pour chaque base de données.

### <a name="creating-a-database-upgrade-session"></a>Création d’une session de mise à niveau de base de données

1. Dans [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], ouvrez l’Explorateur d’objets et connectez-vous à [!INCLUDE[ssDE](../../includes/ssde-md.md)].

2. Cliquez avec le bouton droit sur le nom de la base de données dont vous prévoyez de mettre à niveau le niveau de compatibilité, sélectionnez **Tâches** avec le bouton droit, **Mise à niveau de la base de données**, puis sélectionnez **Nouvelle session de mise à niveau de base de données**.

3. Dans la fenêtre de l’Assistant Paramétrage de requêtes, les deux étapes sont requises pour configurer une session :

    1. Dans la fenêtre **Configuration**, configurez le Magasin des requêtes afin de capturer l’équivalent d’un cycle complet de données de charge de travail à analyser et à paramétrer.
        - Entrez la durée attendue de la charge de travail en jours (le minimum est 1). Cette valeur servira à proposer des paramètres recommandés dans le Magasin des requêtes pour autoriser provisoirement la collecte de la base de référence entière. La capture d’une bonne base de référence est importante pour s’assurer que toutes les requêtes régressées détectées après le changement du niveau de compatibilité de base de données puissent être analysées. 
        - Définissez le niveau de compatibilité de base de données cible auquel doit être la base de données utilisateur une fois le workflow de l’Assistant Paramétrage de requêtes terminé.
        Quand vous avez terminé, sélectionnez **Suivant**.

       ![Fenêtre de configuration de nouvelle session de mise à niveau de base de données](../../relational-databases/performance/media/qta-new-session-setup.png "Fenêtre de configuration de nouvelle mise à niveau de base de données")  
  
    2. Dans la fenêtre **Paramètres**, deux colonnes indiquent l’état **Actuel** du Magasin des requêtes dans la base de données ciblée ainsi que les paramètres **Recommandés**. 
        - Les paramètres recommandés sont sélectionnés par défaut, mais sélectionner la case d’option présente sur la colonne Actuel permet d’accepter les paramètres actuels et également d’affiner la configuration actuelle du Magasin des requêtes.
        - Le paramètre *Durée de validité de la requête* proposé est le double de la durée attendue de la charge de travail (en jours). En effet, le Magasin des requêtes devra contenir des informations sur la charge de travail de base de référence et la charge de travail de post-mise à niveau de base de données.
        Quand vous avez terminé, sélectionnez **Suivant**.

       ![Fenêtre de nouveaux paramètres de mise à niveau de base de données](../../relational-databases/performance/media/qta-new-session-settings.png "Fenêtre de nouveaux paramètres de mise à niveau de base de données")

       > [!IMPORTANT]
       > La proposition *Taille maximale* est une valeur arbitraire qui peut convenir à une charge de travail de courte durée.
       > Toutefois, gardez à l’esprit qu’elle risque d’être insuffisante pour contenir des informations sur les charges de travail de base de référence et de post-mise à niveau de base de données très intensives, c’est-à-dire quand de nombreux plans différents peuvent être générés.
       > Si vous prévoyez que ce sera le cas, entrez une valeur plus élevée qui convient.

4. La fenêtre **Réglage** conclut la configuration de session et indique les étapes suivantes à effectuer pour ouvrir et poursuivre la session. Lorsque vous avez terminé, sélectionnez **Terminer**.

    ![Fenêtre de nouveau paramétrage de mise à niveau de base de données](../../relational-databases/performance/media/qta-new-session-tuning.png "Fenêtre de nouveau paramétrage de mise à niveau de base de données")

> [!NOTE]
> Un autre scénario possible commence par la restauration d’une sauvegarde de base de données à partir du serveur de production, où une base de données a déjà suivi le workflow de mise à niveau de compatibilité de base de données recommandé, vers un serveur de test.

### <a name="executing-the-database-upgrade-workflow"></a>Exécution du workflow de mise à niveau de base de données

1. Sélectionnez avec le bouton droit le nom de la base de données dont vous prévoyez de mettre à niveau le niveau de compatibilité, sélectionnez **Tâches**, **Mise à niveau de la base de données**, puis **Superviser les sessions**.

2. La page **Gestion des sessions** liste les sessions actives et passées pour la base de données dans la portée. Sélectionnez la session souhaitée, puis sélectionnez **Détails**.

    > [!NOTE]
    > Si la session active n’est pas présente, sélectionnez le bouton **Actualiser**.

    La liste contient les informations suivantes :
    - **ID de session**
    - **Nom de la session** : nom généré par le système, composé du nom de la base de données, de la date et de l’heure de création de la session.
    - **État** : état de la session (active ou fermée).
    - **Description** : description générée par le système composée du niveau de compatibilité de la base de données cible sélectionné par l’utilisateur et du nombre de jours de la charge de travail du cycle des opérations.
    - **Heure de début** : date et heure de création de la session.

    ![Page de gestion des sessions de l’Assistant Paramétrage de requêtes](../../relational-databases/performance/media/qta-session-management.png "Page de gestion des sessions de l’Assistant Paramétrage de requêtes")

    > [!NOTE]
    > L’option **Supprimer la session** permet de supprimer toutes les données stockées pour la session sélectionnée.
    > Cependant, la suppression d’une session fermée ne supprime **pas** les repères de plan déployés précédemment.
    > Si vous supprimez une session qui avait des repères de plan déployé, vous ne pouvez pas utiliser l’Assistant Paramétrage de requêtes pour revenir en arrière.
    > Au lieu de cela, recherchez les repères de plan à l’aide de la table système [sys.plan_guides](../../relational-databases/system-catalog-views/sys-plan-guides-transact-sql.md) et supprimez-les manuellement à l’aide de [sp_control_plan_guide](../../relational-databases/system-stored-procedures/sp-control-plan-guide-transact-sql.md).
  
3. Le point d’entrée pour une nouvelle session est l’étape **Collecte des données**.

    > [!NOTE]
    > Le bouton **Sessions** permet de revenir à la page **Gestion des sessions** en laissant la session active telle quelle.

    Cette étape comporte trois sous-étapes :

    1. La **Collecte des données de référence** invite l’utilisateur à exécuter le cycle de charge de travail représentatif, afin que le Magasin des requêtes puisse collecter une base de référence. Une fois cette charge de travail terminée, cochez la case **Exécution terminée de la charge de travail** et sélectionnez **Suivant**.

        > [!NOTE]
        > La fenêtre de l’Assistant Paramétrage de requêtes peut être fermée pendant l’exécution de la charge de travail. Si vous revenez ultérieurement à la session qui reste dans un état actif, vous reprendrez à partir de l’étape où vous vous êtes arrêté. 

        ![Étape 2 de l’Assistant Paramétrage de requêtes : Sous-étape 1](../../relational-databases/performance/media/qta-step2-substep1.png "Étape 2 de l’Assistant Paramétrage de requêtes : Sous-étape 1")

    2. **Mettre à niveau la base de données** vous invitera à autoriser la mise à niveau du niveau de compatibilité de base de données vers le niveau cible souhaité. Pour continuer à l’étape suivante, sélectionnez **Oui**.

        ![Étape 2 de l’Assistant Paramétrage de requêtes : Sous-étape 2 - Mettre à niveau le niveau de compatibilité de la base de données](../../relational-databases/performance/media/qta-step2-substep2-prompt.png "Étape 2 de l’Assistant Paramétrage de requêtes : Sous-étape 2 - Mettre à niveau le niveau de compatibilité de la base de données")

        La page suivante confirme que le niveau de compatibilité de base de données a été correctement mis à niveau.

        ![Étape 2 de l’Assistant Paramétrage de requêtes : Sous-étape 2](../../relational-databases/performance/media/qta-step2-substep2.png "Étape 2 de l’Assistant Paramétrage de requêtes : Sous-étape 2")

    3. La **Collecte des données observées** invite l’utilisateur à réexécuter le cycle de la charge de travail représentative, afin que le Magasin des requêtes puisse collecter une base de référence comparative qui servira à rechercher des pistes d’optimisation. Pendant l’exécution de la charge de travail, utilisez le bouton **Actualiser** pour continuer à mettre à jour la liste des requêtes régressées, si certaines ont été détectées. Modifier la valeur de **Requêtes à afficher** pour limiter le nombre de requêtes affichées. L’ordre de la liste est affecté par la **Métrique** (durée ou temps processeur) et l’**Agrégation** (Moyenne est la valeur par défaut). Sélectionnez également le nombre de **Requêtes à afficher**. Une fois cette charge de travail terminée, cochez la case **Exécution terminée de la charge de travail** et sélectionnez **Suivant**.

        ![Étape 2 de l’Assistant Paramétrage de requêtes : Sous-étape 3](../../relational-databases/performance/media/qta-step2-substep3.png "Étape 2 de l’Assistant Paramétrage de requêtes : Sous-étape 3")

        La liste contient les informations suivantes :
        - **ID de requête** 
        - **Texte de la requête** : instruction [!INCLUDE[tsql](../../includes/tsql-md.md)] que vous pouvez développer en sélectionnant le bouton **...** .
        - **Exécutions** : affiche le nombre d’exécutions de cette requête pour toute la collection de charges de travail.
        - **Métrique de référence** : métrique sélectionnée (durée ou temps processeur), en ms, pour la collecte de données de référence avant la mise à niveau de la compatibilité de base de données.
        - **Métrique observée** : métrique sélectionnée (durée ou temps processeur), en ms, pour la collecte de données après la mise à niveau de la compatibilité de base de données.
        - **% de changement** : pourcentage de changement pour la métrique sélectionnée, entre les états antérieur et postérieur à la mise à niveau de la compatibilité de base de données. Un nombre négatif représente la quantité de régression mesurée pour la requête.
        - **Paramétrable** : *True* ou *False* selon que la requête est éligible pour l’expérimentation.

4. **Afficher l’analyse** permet de sélectionner les requêtes sur lesquelles expérimenter et trouver des opportunités d’optimisation. La valeur de **Requêtes à afficher** devient l’étendue des requêtes éligibles sur lesquelles expérimenter. Une fois que vous avez coché les requêtes souhaitées, sélectionnez **Suivant** pour démarrer l’expérimentation.

    > [!NOTE]
    > Les requêtes avec Paramétrable = False ne peuvent pas être sélectionnées pour l’expérimentation.

    > [!IMPORTANT]
    > Un message signale qu’une fois que l’Assistant Paramétrage de requêtes sera passé à la phase d’expérimentation, il ne sera plus possible de revenir à la page Afficher l’analyse.   
    > Si vous ne sélectionnez pas toutes les requêtes éligibles avant de passer à la phase d’expérimentation, vous devez créer ultérieurement une nouvelle session et répéter le workflow. Cela nécessite la réinitialisation du niveau de compatibilité de base de données à la valeur précédente.

    ![Étape 3 de l’Assistant Paramétrage de requêtes](../../relational-databases/performance/media/qta-step3.png "Étape 3 de l’Assistant Paramétrage de requêtes")

5. **Afficher les résultats** vous permet de sélectionner les requêtes pour lesquelles déployer l’optimisation proposée en tant que repère de plan. 

    La liste contient les informations suivantes :
    - **ID de requête**
    - **Texte de la requête** : instruction [!INCLUDE[tsql](../../includes/tsql-md.md)] que vous pouvez développer en sélectionnant le bouton **...** .
    - **État** : affiche l’état actuel de l’expérimentation pour la requête.
    - **Métrique de référence** : métrique sélectionnée (durée ou temps processeur), en ms, pour la requête telle qu’exécutée à l’**Étape 2 Sous-étape 3**, représentant la requête régressée après la mise à niveau de la compatibilité de base de données.
    - **Métrique observée** : métrique sélectionnée (durée ou temps processeur), en ms, pour la requête après l’expérimentation, pour une optimisation proposée suffisamment bonne.
    - **% de changement** : pourcentage de changement pour la métrique sélectionnée, entre les états antérieur et postérieur à l’expérimentation, représentant le taux d’amélioration mesuré pour la requête avec l’optimisation proposée.
    - **Option de requête** : lien vers l’indicateur proposé qui améliore la métrique d’exécution de requête.
    - **Peut déployer** : *True* ou *False* selon que l’optimisation de requête proposée peut être déployée ou non en tant que repère de plan.

    ![Étape 4 de l’Assistant Paramétrage de requêtes](../../relational-databases/performance/media/qta-step4.png "Étape 4 de l’Assistant Paramétrage de requêtes")

6. **Vérification** affiche l’état du déploiement des requêtes sélectionnées précédemment pour cette session. La liste dans cette page diffère de la page précédente, dans le sens où **Peut restaurer** remplace la colonne **Peut déployer**. Cette colonne peut être *True* ou *False* selon que l’optimisation de requête déployée peut être annulée et son repère de plan supprimé.

    ![Étape 5 de l’Assistant Paramétrage de requêtes](../../relational-databases/performance/media/qta-step5.png "Étape 5 de l’Assistant Paramétrage de requêtes")

    S’il se révèle par la suite nécessaire d’annuler une optimisation proposée, sélectionnez la requête en question et sélectionnez **Restaurer**. Ce repère de plan de requête est supprimé et la liste est mise à jour de façon à supprimer la requête restaurée. Dans l’image ci-dessous, notez que la requête 8 a été supprimée.

    ![Étape 5 de l’Assistant Paramétrage de requêtes - Restauration](../../relational-databases/performance/media/qta-step5-rollback.png "Étape 5 de l’Assistant Paramétrage de requêtes - Restauration")

    > [!NOTE]
    > La suppression d’une session fermée ne supprime **pas** les repères de plan déployés précédemment.
    > Si vous supprimez une session qui avait des repères de plan déployé, vous ne pouvez pas utiliser l’Assistant Paramétrage de requêtes pour revenir en arrière.
    > Au lieu de cela, recherchez les repères de plan à l’aide de la table système [sys.plan_guides](../../relational-databases/system-catalog-views/sys-plan-guides-transact-sql.md) et supprimez-les manuellement à l’aide de [sp_control_plan_guide](../../relational-databases/system-stored-procedures/sp-control-plan-guide-transact-sql.md).
  
## <a name="permissions"></a>Autorisations

Nécessite l’appartenance au rôle **db_owner**.
  
## <a name="see-also"></a>Voir aussi

- [Niveaux de compatibilité et mises à niveau du moteur de base de données](../../t-sql/statements/alter-database-transact-sql-compatibility-level.md#compatibility-levels-and-database-engine-upgrades)
- [Outils de surveillance et de réglage des performances](../../relational-databases/performance/performance-monitoring-and-tuning-tools.md)
- [Analyse des performances à l’aide du magasin de requêtes](../../relational-databases/performance/monitoring-performance-by-using-the-query-store.md)
- [Modifier le mode de compatibilité de base de données et utiliser le magasin des requêtes](../../database-engine/install-windows/change-the-database-compatibility-mode-and-use-the-query-store.md)
- [Indicateurs de trace](../../t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql.md)
- [USE HINT query hints](../../t-sql/queries/hints-transact-sql-query.md#use_hint)
- [Estimateur de cardinalité](../../relational-databases/performance/cardinality-estimation-sql-server.md)
- [Paramétrage automatique](../../relational-databases/automatic-tuning/automatic-tuning.md)   
- [Utiliser l’Assistant Paramétrage de requêtes SQL Server](/learn/modules/use-sql-server-query-tuning-assistant/)