---
title: Qu’est-ce qu’Azure Data Studio ?
description: Azure Data Studio est un outil gratuit et léger qui s’exécute sur Windows, macOS et Linux pour la gestion de SQL Server, Azure SQL Database et Azure Synapse Analytics.
ms.prod: azure-data-studio
ms.technology: azure-data-studio
ms.topic: overview
author: markingmyname
ms.author: maghan
ms.reviewer: alayu, sstein
ms.custom: seodec18, sqlfreshmay19
ms.date: 10/20/2020
ms.openlocfilehash: b9adac14bfd458b96fe4e46c478af2d2b5d6a181
ms.sourcegitcommit: 15c7cd187dcff9fc91f2daf0056b12ed3f0403f0
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/08/2021
ms.locfileid: "102464868"
---
# <a name="what-is-azure-data-studio"></a>Qu’est-ce qu’Azure Data Studio ?

Azure Data Studio est un outil de base de données multiplateforme pour les professionnels des données qui utilisent des plateformes de données locales et cloud sur Windows, macOS et Linux.

Azure Data Studio offre une expérience d’éditeur moderne avec des fonctionnalités IntelliSense, des extraits de code, l’intégration du contrôle de code source et un terminal intégré. Il est conçu en tenant compte de l’utilisateur de la plateforme de données, avec l’intégration de la représentation graphique des jeux de résultats de requête et des tableaux de bord personnalisables.

Le code source d’Azure Data Studio et de ses fournisseurs de données est disponible sur GitHub, dans le cadre d’un CLUF de code source qui fournit des droits pour modifier et utiliser le logiciel, mais pas pour le redistribuer ou l’héberger dans un service cloud. Pour plus d’informations, consultez les [Questions fréquentes (FAQ) Azure Data Studio](faq.yml).

**[Télécharger et installer Azure Data Studio](./download-azure-data-studio.md)**

## <a name="sql-code-editor-with-intellisense"></a>Éditeur de code SQL avec IntelliSense

Azure Data Studio offre une expérience de codage SQL moderne et centrée sur le clavier qui facilite et simplifie les expériences de base que vous utilisez chaque jour grâce à des fonctionnalités intégrées telles que la présence de plusieurs fenêtres à onglets, un éditeur SQL riche, IntelliSense, la saisie semi-automatique de mots clés, des extraits de code/la navigation dans le code et l’intégration du contrôle de code source (Git). Exécutez des requêtes SQL à la demande, affichez et enregistrez les résultats sous forme de texte, JSON ou Excel. Modifiez les données, organisez vos connexions de base de données favorites et parcourez les objets de base de données dans une expérience de navigation d’objets familière. Pour savoir comment utiliser l’éditeur SQL, consultez [Utiliser l’éditeur SQL pour créer des objets de base de données](tutorial-sql-editor.md).

## <a name="smart-sql-code-snippets"></a>Extraits de code Smart SQL

Les extraits de code SQL génèrent la syntaxe SQL appropriée pour créer des bases de données, des tables, des vues, des procédures stockées, des utilisateurs, des connexions et des rôles, ainsi que pour mettre à jour des objets de base de données existants. Utilisez les extraits intelligents pour créer rapidement des copies de votre base de données à des fins de développement ou de test, et pour générer et exécuter des scripts CREATE et INSERT.

Azure Data Studio fournit également des fonctionnalités permettant de créer des extraits de code SQL personnalisés. Pour plus d’informations, consultez [Créer et utiliser des extraits de code](code-snippets.md).

## <a name="customizable-server-and-database-dashboards"></a>Tableaux de bord de serveur et de base de données personnalisables

Créez des tableaux de bord enrichis personnalisables pour surveiller et résoudre rapidement les goulots d’étranglement des performances dans vos bases de données. Pour en savoir plus sur les widgets d’insight et les tableaux de bord de base de données (et de serveur), consultez [Gérer les serveurs et les bases de données avec les widgets d’insight](insight-widgets.md).

## <a name="connection-management-server-groups"></a>Gestion des connexions (groupes de serveurs)

Les groupes de serveurs offrent un moyen d’organiser vos informations de connexion aux serveurs et aux bases de données avec lesquels vous travaillez. Pour plus d’informations, consultez [Groupes de serveurs](server-groups.md).

## <a name="integrated-terminal"></a>Terminal intégré

Utilisez vos outils en ligne de commande préférés (par exemple Bash, PowerShell, sqlcmd, bcp et ssh) dans la fenêtre de terminal intégrée directement dans l’interface utilisateur d’Azure Data Studio. Pour en savoir plus sur le terminal intégré, consultez [Terminal intégré](integrated-terminal.md).

## <a name="extensibility-and-extension-authoring"></a>Extensibilité et création d’extensions

Améliorez l’expérience Azure Data Studio en étendant les fonctionnalités de l’installation de base. Azure Data Studio fournit des points d’extensibilité pour les activités de gestion des données et la prise en charge de la création d’extensions.

Pour en savoir plus sur l’extensibilité d’Azure Data Studio, consultez [Extensibilité](extensibility.md).
Pour en savoir plus sur la création d'extensions, consultez [Création d’une extension](extensions/extension-authoring.md).

## <a name="feature-comparison-with-sql-server-management-studio-ssms"></a>Comparaison des fonctionnalités avec SQL Server Management Studio (SSMS)

**Utilisez Azure Data Studio si vous :**

- Modifie ou exécute principalement des requêtes.
- Avez besoin de visualiser et représenter graphiquement des jeux de résultats rapidement.
- Pouvez exécuter la plupart des tâches d’administration via le terminal intégré à l’aide de sqlcmd ou de PowerShell.
- Avez des besoins minimaux en assistants.
- N’avez pas besoin d’effectuer une configuration d’administration ou de plateforme complète.
- Devez exécuter la solution sur macOS ou Linux.

**Utilisez SQL Server Management Studio si vous :**

- Effectuez une configuration administrative ou de plateforme complexe.
- Effectuez la gestion de la sécurité, notamment la gestion des utilisateurs, l’évaluation des vulnérabilités et la configuration des fonctionnalités de sécurité.
- Devez utiliser les conseillers et tableaux de bord d’optimisation des performances.
- Utilisez des diagrammes de base de données et des concepteurs de tables.
- Devez accéder aux serveurs inscrits.
- Utilisez les statistiques des requêtes actives ou les statistiques du client.

### <a name="shell-features"></a>Fonctionnalités de l’interpréteur de commandes

|Fonctionnalité|Azure Data Studio|SSMS|
|:---|:---|:---|
|Connexion à Azure|Oui|Oui|
|tableau de bord|Oui| |
|Extensions|Oui| |
|Terminal intégré|Oui||
|Explorateur d’objets|Oui|Oui|
|Scripts d’objets|Oui|Oui|
|Système de projet|Oui||
|Select from Table|Oui|Oui|
|Contrôle de code source|Oui||
|Volet des tâches|Oui||
|Thèmes, y compris le mode Sombre|Oui||
|Azure Resource Explorer|PRÉVERSION||
|Assistant Générer des scripts||Oui|
|Propriétés des objets||Oui|
|Concepteur de tables||Oui|

### <a name="query-editor"></a>Éditeur de requête

|Fonctionnalité|Azure Data Studio|SSMS|
|:---|:---|:---|
|Visionneuse de graphiques|Oui||
|Exportation des résultats au format CSV, JSON, XLSX|Oui||
|Résultats dans un fichier||Oui|
|Résultats dans du texte||Oui|
|IntelliSense|Oui|Oui|
|Extraits de code|Oui|Oui|
|Plan d’affichage|PRÉVERSION|Oui|
|Statistiques du client||Oui|
|Statistiques des requêtes dynamiques||Oui|
|Options de requête||Oui|
|Visionneuse spatiale||Oui|
|SQLCMD|Oui|Oui|

### <a name="operating-system-support"></a>Prise en charge du système d'exploitation

|Fonctionnalité|Azure Data Studio|SSMS|
|:---|:---|:---|
|Windows|Oui|Oui|
|macOS|Oui||
|Linux|Oui||

### <a name="data-engineering"></a>Engineering données

|Fonctionnalité|Azure Data Studio|SSMS|
|:---|:---|:---|
|Assistant de données externes|PRÉVERSION||
|Intégration HDFS|PRÉVERSION||
|Notebooks|PRÉVERSION||

### <a name="database-administration"></a>Administration de bases de données

|Fonctionnalité|Azure Data Studio|SSMS|
|:---|:---|:---|
|Sauvegarder/restaurer|Oui|Oui|
|Importation de fichiers plats|Oui|Oui|
|SQL Agent|PRÉVERSION|Oui|
|SQL Profiler|PRÉVERSION|Oui|
|Always On||Oui|
|Always Encrypted||Oui|
|Assistant Copier des données||Oui|
|Assistant Paramétrage des données||Oui|
|Diagrammes de base de données||Oui|
|Visionneuse de journal des erreurs||Oui|
|Plans de maintenance||Oui|
|Requêtes sur plusieurs serveurs||Oui|
|Gestion basée sur des stratégies||Oui|
|PolyBase||Oui|
|Magasin des requêtes||Oui|
|Serveurs inscrits||Oui|
|Réplication||Oui|
|Gestion de la sécurité||Oui|
|Service Broker||Oui|
|SQL Assessment|PRÉVERSION|Oui|
|SQL Mail||Oui|
|Explorateur de modèles||Oui|
|Évaluation des vulnérabilités||Oui|
|Gestion de XEvent||Oui|

### <a name="database-development"></a>Développement de bases de données
|Fonctionnalité|Azure Data Studio|SSMS|
|:---|:---|:---|
|Importation/exportation de DACPAC|Oui|Oui|
|Projets SQL|PRÉVERSION||
|Comparaison de schémas|Oui||

## <a name="next-steps"></a>Étapes suivantes

- [Télécharger et installer Azure Data Studio](./download-azure-data-studio.md)
- [FAQ sur Azure Data Studio](faq.yml)
- [Se connecter à et interroger SQL Server](quickstart-sql-server.md)
- [Se connecter à et interroger Azure SQL Database](quickstart-sql-database.md)

[!INCLUDE[get-help-sql-tools](../includes/paragraph-content/get-help-sql-tools.md)]

[!INCLUDE[contribute-to-content](../includes/paragraph-content/contribute-to-content.md)]
