---
title: Éditeur de requête SSMS
description: Éditeur de requêtes SQL Server Management Studio (SSMS)
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
f1_keywords:
- sql13.swb.query.advanced.f1
- sql13.swb.query.ansi.f1
- sql13.swb.query.general.f1
- sql13.swb.query.grid.f1
- sql13.swb.sqleditors.multiserverresultssettings
- sql13.swb.tsqlquery.f1
- sql13.swb.tsqlresults.f1
dev_langs:
- TSQL
helpviewer_keywords:
- SQL Server Management Studio [SQL Server], editor
- SQL Server Management Studio [SQL Server], Database Engine Query Editor
- SQL Server Management Studio [SQL Server], templates
- SQL Server Management Studio [SQL Server], query editor
- Query Editor [SQL Server Management Studio]
- Query Editor [Database Engine]
- Query Editor [Database Engine], Features
- Query Editor [Database Engine], Toolbar
- Query Editor [SQL Server Management Studio], full screen mode
- Query Editor [Database Engine], templates
- Query Editor [SQL Server Management Studio], about Query Editor
- Transact-SQL Editor See Query Editor [Database Engine]
- Database Engine Query Editor See Query Editor [Database Engine]
- Code Editor [SQL Server Management Studio], about Query Editor
- editors [SQL Server Management Studio], Database Engine Query Editor
- full screen mode [SQL Server Management Studio]
- writing scripts
- modifying scripts
- writing queries
- scripts [SQL Server], SQL Server Management Studio
- queries [SQL Server], SQL Server Management Studio
ms.assetid: 05cfae9b-96d5-4a35-a098-0bc3a548bcfc
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019, contperf-fy21q1
ms.date: 08/28/2020
ms.openlocfilehash: 6ce879af451abb20e108e20b36fbe129024f4806
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100355164"
---
# <a name="sql-server-management-studio-ssms-query-editor"></a>Éditeur de requêtes SQL Server Management Studio (SSMS)

[!INCLUDE [SQL Server ASDB, ASDBMI, ASDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa.md)]

Cet article explique les fonctionnalités et fonctions de l’éditeur de requête dans SQL Server Management Studio (SSMS).

> [!Note]
> Si vous souhaitez savoir comment utiliser l’aide de Transact-SQL (T-SQL) avec F1, consultez la section [Aide sur Transact-SQL via la touche F1](#transact-sql-f1-help).
>
> Si vous souhaitez en savoir plus sur les tâches que vous pouvez effectuer avec l’éditeur, consultez la section [Tâches de l'éditeur](#editor-tasks).

Les éditeurs dans SSMS partagent une architecture classique. L'éditeur de texte implémente le niveau de base des fonctionnalités, et peut être utilisé comme éditeur de base pour les fichiers texte. Les autres éditeurs, ou éditeurs de requête, étendent cette base de fonctionnalité en incluant un service de langage qui définit la syntaxe d'un des langages pris en charge dans SQL Server. Les éditeurs de requête implémentent également différents niveaux de prise en charge des fonctionnalités de l'éditeur comme IntelliSense et le débogage. Les éditeurs de requête incluent l'éditeur de requête du moteur de base de données à utiliser dans l'élaboration de scripts contenant des instructions T-SQL et XQuery, l'éditeur MDX pour le langage MDX, l'éditeur DMX pour le langage DMX et l'éditeur XML/A pour le langage XML for Analysis.
Vous pouvez utiliser l’Éditeur de requête pour créer et exécuter des scripts contenant des instructions Transact-SQL.

![Nouvelle requête](media/database-engine-query-editor-sql-server-management-studio/new-query.png)

## <a name="sql-editor-toolbar"></a>Barre d’outils Éditeur SQL

Quand vous ouvrez l’Éditeur de requête, la barre d’outils Éditeur SQL, qui comporte les boutons suivants, s’affiche.

Vous pouvez également ajouter la barre d’outils Éditeur SQL en sélectionnant le menu **Affichage** , **Barres d’outils**, puis **Éditeur SQL**. Si vous ajoutez la barre d’outils Éditeur SQL alors qu’aucune fenêtre de l’Éditeur de requête n’est ouverte, tous les boutons sont indisponibles.

![Barre d’outils Éditeur](media/database-engine-query-editor-sql-server-management-studio/editor-toolbar.png)

### <a name="connect-using-the-editor-toolbar"></a>Connexion à l’aide de la barre d’outils Éditeur

Ouvre la boîte de dialogue **[Se connecter au serveur](connect-to-server-database-engine.md)** . Utilisez cette boîte de dialogue pour établir une connexion à un serveur.

Vous pouvez également vous connecter à votre base de données à l’aide du [menu contextuel](#connection-using-the-context-menu).

### <a name="change-connection-using-the-editor-toolbar"></a>Changement de connexion à l’aide de la barre d’outils Éditeur

Ouvre la boîte de dialogue **Se connecter au serveur** . Utilisez cette boîte de dialogue pour établir une [connexion à un autre serveur](f1-help-for-server-connections-sql-server-management-studio.md).

Vous pouvez également changer de connexion à l’aide du [menu contextuel](#connection-using-the-context-menu).

### <a name="available-databases-using-the-editor-toolbar"></a>Bases de données disponibles à l’aide de la barre d’outils Éditeur

Permet de se connecter à une autre base de données sur le même serveur.

### <a name="execute-using-the-editor-toolbar"></a>Exécution à l’aide de la barre d’outils Éditeur

Exécute le code sélectionné ou, si aucun code n'est sélectionné, exécute la totalité du code figurant de l'éditeur de requête.

Vous pouvez également **exécuter** une requête en sélectionnant F5 ou dans le [menu contextuel](#execute-using-the-context-menu).

### <a name="cancel-executing-query-using-the-editor-toolbar"></a>Annulation de l’exécution de la requête à l’aide de la barre d’outils Éditeur

Envoie une demande d'annulation au serveur. Certaines requêtes ne peuvent pas être annulées immédiatement et doivent attendre une condition d’annulation appropriée. Les opérations de restauration de transactions peuvent prendre un certain temps lorsque celles-ci sont annulées.

Pour annuler une requête en cours d’exécution, vous pouvez également sélectionner Alt+Pause.

### <a name="parse-using-the-editor-toolbar"></a>Analyse à l’aide de la barre d’outils Éditeur

Contrôle la syntaxe du code sélectionné. Si aucun code n’est sélectionné, vérifie la syntaxe de l’ensemble du code de la fenêtre de l’Éditeur de requête.

Pour vérifier le code dans l’Éditeur de requête, vous pouvez également sélectionner Ctrl+F5.

### <a name="display-estimated-execution-plan-using-the-editor-toolbar"></a>Affichage du plan d’exécution estimé à l’aide de la barre d’outils Éditeur

Demande un plan d’exécution de la requête au processeur de requêtes sans exécuter la requête et affiche le plan dans la fenêtre **Plan d’exécution**. Ce plan utilise les statistiques d'index pour estimer le nombre de lignes qui seront renvoyées pendant chaque partie de l'exécution de la requête. Le plan de requête utilisé dans la pratique peut être différent du plan d'exécution estimé. Si le nombre de lignes renvoyées diffère sensiblement de l'estimation et que le processeur de requêtes modifie le plan pour qu'il soit plus efficace, cela peut se produire.

Vous pouvez également afficher un plan d’exécution estimé en sélectionnant Ctrl+L ou dans le [menu contextuel](#display-estimated-execution-plan-using-the-context-menu).

### <a name="query-options-using-the-editor-toolbar"></a>Options de requête à l’aide de la barre d’outils Éditeur

Ouvre la boîte de dialogue **Options de requête** . Utilisez cette boîte de dialogue pour configurer les options par défaut d'exécution des requêtes et d'affichage de leurs résultats.

Vous pouvez également sélectionner **Options de requête** dans le [menu contextuel](#query-options-using-the-context-menu).

### <a name="intellisense-enabled-using-the-editor-toolbar"></a>IntelliSense activé à l’aide de la barre d’outils Éditeur

Indique si les fonctionnalités [IntelliSense](../scripting/configure-intellisense-sql-server-management-studio.md) sont disponibles dans l'éditeur de requête du moteur de base de données. Cette option est définie par défaut.

Vous pouvez également sélectionner **IntelliSense activé** en sélectionnant Ctrl+B, puis Ctrl+I, ou dans le [menu contextuel](#intellisense-enabled-using-the-context-menu).

### <a name="include-actual-execution-plan-using-the-editor-toolbar"></a>Ajout du plan d’exécution réel à l’aide de la barre d’outils Éditeur

Exécute la requête, puis renvoie les résultats de la requête et utilise le plan d'exécution pour la requête. Ces requêtes s’affichent sous forme de plan de requête graphique dans la fenêtre **Plan d’exécution**.

Vous pouvez également choisir **Ajout du plan d’exécution réel** en sélectionnant Ctrl+M ou dans le [menu contextuel](#include-actual-execution-plan-using-the-context-menu).

### <a name="include-live-query-statistics-using-the-editor-toolbar"></a>Ajout des statistiques des requêtes actives à l’aide de la barre d’outils Éditeur

Fournit des insights en temps réel sur le processus d’exécution des requêtes à mesure que les contrôles passent d’un opérateur de plan de requête à un autre.

Vous pouvez également sélectionner **Ajout des statistiques des requêtes actives** dans le [menu contextuel](#include-live-query-statistics-using-the-context-menu).

### <a name="include-client-statistics-using-the-editor-toolbar"></a>Ajout des statistiques du client à l’aide de la barre d’outils Éditeur

Insère une fenêtre **Statistiques du client** qui contient des statistiques sur la requête et les paquets réseau, ainsi que le temps écoulé depuis le début de la requête.

Vous pouvez également choisir **Ajout des statistiques du client** en sélectionnant Maj+Alt+S ou dans le [menu contextuel](#include-client-statistics-using-the-context-menu).

### <a name="results-to-text-using-the-editor-toolbar"></a>Résultats dans du texte à l’aide de la barre d’outils Éditeur

Retourne les résultats de la requête dans la fenêtre **Résultats** sous forme de texte.

Vous pouvez également retourner les résultats dans du texte en sélectionnant Ctrl+T ou dans le [menu contextuel](#results-using-the-context-menu).

### <a name="results-to-grid-using-the-editor-toolbar"></a>Résultats dans une grille à l’aide de la barre d’outils Éditeur

Retourne les résultats de la requête dans la fenêtre **Résultats** sous forme d’une ou plusieurs grilles. Cette option est activée par défaut.

Vous pouvez également retourner les résultats dans une grille en sélectionnant Ctrl+D ou dans le [menu contextuel](#results-using-the-context-menu).

### <a name="results-to-file-using-the-editor-toolbar"></a>Résultats dans un fichier à l’aide de la barre d’outils Éditeur

Lors de l’exécution de la requête, la boîte de dialogue **Enregistrer les résultats** s’ouvre. Dans **Enregistrer dans**, sélectionnez le dossier dans lequel vous souhaitez enregistrer le fichier. Dans **Nom de fichier**, tapez le nom du fichier, puis sélectionnez **Enregistrer** pour enregistrer les résultats de la requête dans un fichier **Rapport** portant l’extension .rpt. Pour accéder aux options avancées, sélectionnez la flèche déroulante vers le bas qui se trouve sur le bouton **Enregistrer**, puis sélectionnez **Enregistrer avec l’encodage**.

Vous pouvez également retourner les résultats dans un fichier en sélectionnant Ctrl+Maj+F ou dans le [menu contextuel](#results-using-the-context-menu).

### <a name="comment-out-the-selected-lines-using-the-editor-toolbar"></a>Mise en commentaire des lignes sélectionnées à l’aide de la barre d’outils Éditeur

Transforme la ligne active en commentaire en ajoutant un opérateur de commentaire (--) en début de ligne.

Pour mettre une ligne en commentaire, vous pouvez également sélectionner Ctrl+K, puis Ctrl+C.

### <a name="uncomment-the-selected-lines-using-the-editor-toolbar"></a>Suppression de la marque de commentaire sur les lignes sélectionnées à l’aide de la barre d’outils Éditeur

Transforme la ligne active en instruction source active en supprimant l'opérateur de commentaire (--) en début de ligne.

Pour supprimer la marque de commentaire sur une ligne, vous pouvez également sélectionner Ctrl+K, puis Ctrl+U.

### <a name="decrease-indent-using-the-editor-toolbar"></a>Réduction du retrait à l’aide de la barre d’outils Éditeur

Déplace le texte de la ligne à gauche en supprimant les espaces en début de ligne.

### <a name="increase-line-indent-using-the-editor-toolbar"></a>Augmentation du retrait de ligne à l’aide de la barre d’outils Éditeur

Déplace le texte de la ligne vers la droite en ajoutant des espaces en début de ligne.

### <a name="specify-values-for-template-parameters-using-the-editor-toolbar"></a>Spécification des valeurs des paramètres du modèle à l’aide de la barre d’outils Éditeur

Ouvre une boîte de dialogue qui vous permet de spécifier les valeurs des paramètres des procédures stockées et des fonctions.

## <a name="context-menu"></a>Menu contextuel

Pour accéder au menu contextuel, *cliquez avec le bouton droit* n’importe où dans l’Éditeur de requête. Les options du menu contextuel sont similaires à celles de la barre d’outils Éditeur SQL. Le menu contextuel comporte les mêmes options que **Connexion** et **Exécution**, ainsi que d’autres options, comme **Insertion d’un extrait de code** et **Entourage avec**.

![Options](media/database-engine-query-editor-sql-server-management-studio/context-menu.png)

### <a name="insert-snippet-using-the-context-menu"></a>Insertion d’un extrait de code à l’aide du menu contextuel

Un [extrait de code T-SQL](../scripting/add-transact-sql-snippets.md) est un modèle pouvant servir de point de départ à l’écriture de nouvelles instructions Transact-SQL dans l’Éditeur de requête.

### <a name="surround-with-using-the-context-menu"></a>Entourage avec à l’aide du menu contextuel

Un extrait de code d’entourage est un modèle pouvant servir de point de départ à l’insertion d’un ensemble d’instructions Transact-SQL dans un bloc BEGIN, IF ou WHILE.

### <a name="connection-using-the-context-menu"></a>Connexion à l’aide du menu contextuel

![Connexions](media/database-engine-query-editor-sql-server-management-studio/context-menu-connections.png)

Il existe plus d’options **Connexion** dans le menu contextuel que dans la barre d’outils de SSMS.

- **Connexion** : ouvre la boîte de dialogue Se connecter au serveur. Utilisez cette boîte de dialogue pour établir une connexion à un serveur.

- **Déconnexion** : déconnecte l’Éditeur de requête actif du serveur.

- **Déconnexion de toutes les requêtes** : déconnecte toutes les connexions de requête.

- **Changement de connexion** : ouvre la boîte de dialogue Se connecter au serveur. Utilisez cette boîte de dialogue pour établir une connexion à un serveur différent.

### <a name="open-server-in-object-explorer-using-the-context-menu"></a>Ouverture du serveur dans l’Explorateur d’objets à l’aide du menu contextuel

L'Explorateur d'objets fournit une interface utilisateur hiérarchique pour afficher et gérer les objets dans chaque instance de SQL Server. Le volet Détails de l'Explorateur d'objets présente une vue tabulaire des objets d'instance et la fonction de recherche d'objets spécifiques. Les fonctionnalités de l'Explorateur d'objets varient légèrement selon le type de serveur. Il contient toutefois des fonctionnalités de développement pour les bases de données et des fonctionnalités de gestion pour tous les types de serveurs.

### <a name="execute-using-the-context-menu"></a>Exécution à l’aide du menu contextuel

Exécute le code sélectionné ou, si aucun code n'est sélectionné, exécute la totalité du code figurant dans l'éditeur de requête.

### <a name="display-estimated-execution-plan-using-the-context-menu"></a>Affichage du plan d’exécution estimé à l’aide du menu contextuel

Demande un plan d’exécution de la requête au processeur de requêtes sans exécuter la requête et affiche le plan dans la fenêtre **Plan d’exécution** . Ce plan utilise les statistiques d'index pour estimer le nombre de lignes qui seront renvoyées pendant chaque partie de l'exécution de la requête. Le plan de requête utilisé dans la pratique peut être différent du plan d'exécution estimé. Cela peut se produire si le nombre de lignes renvoyées diffère de l'estimation. À ce moment-là, le processeur de requêtes modifie le plan pour qu'il soit plus efficace

### <a name="intellisense-enabled-using-the-context-menu"></a>IntelliSense activé à l’aide du menu contextuel

Indique si les fonctionnalités IntelliSense sont disponibles dans l'éditeur de requête du moteur de base de données. Cette option est définie par défaut.

### <a name="trace-query-in-sql-server-profiler-using-the-context-menu"></a>Suivi de la requête dans SQL Server Profiler à l’aide du menu contextuel

SQL Server Profiler est une interface permettant de créer et de gérer les traces, ainsi que d’analyser et de relire les résultats de trace. Les événements sont enregistrés dans un fichier de trace, qui peut être analysé ou utilisé ultérieurement pour relire une série d’étapes spécifique lors de la tentative de diagnostic d’un problème.

### <a name="analyze-query-in-database-engine-tuning-advisor-using-the-context-menu"></a>Analyse de la requête dans l’Assistant Paramétrage du moteur de base de données à l’aide du menu contextuel

L’Assistant Paramétrage du moteur de base de données Microsoft analyse les bases de données et émet des recommandations pour optimiser les performances des requêtes. Il permet de sélectionner et de créer un ensemble optimal d’index, de vues indexées ou de partitions de table sans avoir une compréhension approfondie de la structure de la base de données ou des mécanismes internes de SQL Server. Vous pouvez effectuer les tâches suivantes à l'aide de l'Assistant Paramétrage du moteur de base de données :

### <a name="design-query-in-editor-using-the-context-menu"></a>Conception des requêtes dans l’Éditeur à l’aide du menu contextuel

Le Concepteur de requêtes et de vues s'affiche lorsque vous ouvrez la définition d'une vue, lorsque vous affichez les résultats d'une requête ou d'une vue, ou encore lorsque vous créez ou ouvrez une requête.

### <a name="include-actual-execution-plan-using-the-context-menu"></a>Ajout du plan d’exécution réel à l’aide du menu contextuel

Exécute la requête, puis renvoie les résultats de la requête et utilise le plan d'exécution pour la requête. Ces requêtes s’affichent sous forme de plan de requête graphique dans la fenêtre **Plan d’exécution**.

### <a name="include-live-query-statistics-using-the-context-menu"></a>Ajout des statistiques des requêtes actives à l’aide du menu contextuel

Fournit des insights en temps réel sur le processus d’exécution des requêtes à mesure que les contrôles passent d’un opérateur de plan de requête à un autre.

### <a name="include-client-statistics-using-the-context-menu"></a>Ajout des statistiques du client à l’aide du menu contextuel

Insère une fenêtre **Statistiques du client** qui contient des statistiques sur la requête et les paquets réseau, ainsi que le temps écoulé depuis le début de la requête.

### <a name="results-using-the-context-menu"></a>Résultats à l’aide du menu contextuel

![Options des résultats](media/database-engine-query-editor-sql-server-management-studio/context-menu-results.png)

Vous pouvez sélectionner l’option *Résultats* de votre choix dans le menu contextuel.

- **Résultats dans du texte** : retourne les résultats de la requête sous forme de texte dans la fenêtre **Résultats**.

- **Résultats dans une grille** : retourne les résultats de la requête sous forme d’une ou plusieurs grilles dans la fenêtre **Résultats**.

- **Résultats dans un fichier** : lors de l’exécution de la requête, la boîte de dialogue **Enregistrer les résultats** s’ouvre. Dans **Enregistrer dans**, sélectionnez le dossier dans lequel vous souhaitez enregistrer le fichier. Dans **Nom de fichier**, tapez le nom du fichier, puis sélectionnez **Enregistrer** pour enregistrer les résultats de la requête dans un fichier **Rapport** avec l’extension .rpt. Pour accéder aux options avancées, sélectionnez la flèche déroulante vers le bas qui se trouve sur le bouton **Enregistrer**, puis sélectionnez **Enregistrer avec l’encodage**.

### <a name="properties-window-using-the-context-menu"></a>Fenêtre Propriétés à l’aide du menu contextuel

La [Fenêtre Propriétés](../menu-help/properties-window-f1-help-management-studio.md) décrit l’état d’un élément de SQL Server Management Studio, comme une connexion ou un opérateur de plan d’exécution de requêtes, et donne des informations sur les objets de base de données, comme les tables, les vues et les concepteurs.

Vous utilisez également la fenêtre Propriétés pour afficher les propriétés de la connexion active. La plupart des propriétés sont en lecture seule dans la Fenêtre Propriétés, mais sont modifiables à d’autres endroits de Management Studio. Par exemple, la propriété Database d'une requête est en lecture seule dans la fenêtre Propriétés mais elle peut être modifiée dans la barre d'outils.

### <a name="query-options-using-the-context-menu"></a>Options de requête à l’aide du menu contextuel

Ouvre la boîte de dialogue **Options de requête** . Utilisez cette boîte de dialogue pour configurer les options par défaut d'exécution des requêtes et d'affichage de leurs résultats.

## <a name="transact-sql-f1-help"></a>Aide sur Transact-SQL via la touche F1

L’Éditeur de requête vous communique un lien vers la rubrique de référence d’une instruction Transact-SQL donnée lorsque vous sélectionnez F1. Pour ce faire, mettez en surbrillance le nom d'une instruction Transact-SQL, puis sélectionnez F1. Le moteur de recherche d’aide recherche alors une rubrique comportant un attribut d’aide F1 qui corresponde à la chaîne en surbrillance.

Si le moteur de recherche d’aide ne trouve pas de rubrique comportant un mot clé d’aide F1 qui correspond exactement à la chaîne en surbrillance, cette rubrique s’affiche. Dans ce cas, il existe deux approches pour obtenir l’aide recherchée :

- Copier et coller la chaîne de l'éditeur que vous avez sélectionnée dans l'onglet de recherche de la documentation en ligne de SQL Server et effectuer une recherche.

- Ne sélectionner que la partie de l'instruction Transact-SQL susceptible de correspondre à un mot clé d'aide F1 appliqué à une rubrique et sélectionner à nouveau F1. Le moteur de recherche nécessite une correspondance exacte entre la chaîne que vous avez mise en surbrillance et un mot clé d'aide F1 affecté à une rubrique. Si la chaîne en surbrillance contient des éléments propres à votre environnement, comme des noms de colonnes ou de paramètres, le moteur de recherche n’obtient pas de correspondance. Voici quelques exemples de chaînes à sélectionner :

  - Nom d'une instruction Transact-SQL, comme SELECT, CREATE DATABASE or BEGIN TRANSACTION.

  - Nom d’une fonction intégrée, comme SERVERPROPERTY ou @@VERSION.

  - Nom d'une table, ou d'une vue, de procédure stockée système, comme sys.data_spaces ou sp_tableoption.

## <a name="editor-tasks"></a>Tâches de l'éditeur

| Description de la tâche | Rubrique |
|------------------|-------|
| Décrit les différentes façons d'ouvrir les éditeurs dans SSMS.| [Ouvrir un éditeur](../scripting/open-an-editor-sql-server-management-studio.md) |
| Configurez les options des différents éditeurs, tels que la numérotation des lignes et les options IntelliSense. | [Configurer les éditeurs](../scripting/configure-editors-sql-server-management-studio.md) |
| Comment gérer le mode d'affichage, tel que le retour automatique à la ligne, le fractionnement d'une fenêtre, ou les onglets.| [Gérer l'Éditeur et le mode d'affichage](../scripting/manage-the-editor-and-view-mode.md) |
| Définir les options de mise en forme, telles que le texte masqué ou la mise en retrait. | [Gérer la mise en forme du code](../scripting/manage-code-formatting.md) |
| Parcourir le texte dans une fenêtre d'éditeur à l'aide de fonctionnalités telles que la recherche incrémentielle ou Atteindre. | [Naviguer dans le code et le texte](../scripting/navigate-code-and-text.md) |
| Définir les options de codage en couleurs pour différentes classes de syntaxe, ce qui facilite la lecture des instructions complexes. | [Codage en couleurs dans les éditeurs de requête](../scripting/color-coding-in-query-editors.md) |
| Faire glisser le texte d'un emplacement dans un script et le placer à un nouvel emplacement.| [Glisser et déplacer du texte](../scripting/drag-and-drop-text.md) |
| Définir des signets afin de rechercher plus facilement les segments de code importants. | [Gérer les signets](../scripting/manage-bookmarks.md) |
| Imprimer des scripts ou les résultats dans une fenêtre ou une grille.| [Imprimer le code et les résultats](../scripting/print-code-and-results.md) |
| Afficher et utiliser les fonctionnalités de base dans l'éditeur de requête MDX. | [Créer des scripts Analysis Services](/analysis-services/instances/create-analysis-services-scripts-in-management-studio) |
| Afficher et utiliser les fonctionnalités de base dans l'éditeur de requête DMX. | [Créer une requête DMX](/analysis-services/data-mining/create-a-dmx-query-in-sql-server-management-studio) |
| Afficher et utiliser les fonctionnalités de base dans l'éditeur de requête XML/A. | [Éditeur XML](../scripting/xml-editor-sql-server-management-studio.md) |
| Utiliser les fonctionnalités sqlcmd dans l'éditeur de requête du moteur de base de données.| [Modifier des scripts SQLCMD](../scripting/edit-sqlcmd-scripts-with-query-editor.md) |
| Comment utiliser des extraits de code dans l’éditeur de requête du moteur de base de données. Les extraits de code sont des modèles pour des instructions ou des blocs couramment utilisés, et peuvent être personnalisés ou étendus pour inclure les extraits de code spécifiques au site.| [Extraits de code T-SQL](../scripting/add-transact-sql-snippets.md) |
| Comment utiliser le débogueur Transact\-SQL pour parcourir le code et afficher les informations de débogage telles que les valeurs des variables et des paramètres.| [Débogueur T-SQL](../scripting/transact-sql-debugger.md) |

## <a name="see-also"></a>Voir aussi

- [Personnalisation des menus et des touches de raccourci](../customize-menus-and-shortcut-keys.md)
- [Raccourcis clavier dans SQL Server Management Studio](../../ssms/sql-server-management-studio-keyboard-shortcuts.md)