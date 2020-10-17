---
title: FAQ sur Azure Data Studio
description: Obtenez des réponses aux questions fréquemment posées sur Azure Data Studio, comme ses fonctionnalités, son coût ou à qui il s’adresse.
ms.prod: azure-data-studio
ms.technology: azure-data-studio
ms.topic: conceptual
author: markingmyname
ms.author: maghan
ms.reviewer: alayu, maghan, sstein
ms.custom: seodec18
ms.date: 09/24/2018
ms.openlocfilehash: 0231111907d1f342aeda6aea5a9d824a56f40e8b
ms.sourcegitcommit: a41e1f4199785a2b8019a419a1f3dcdc15571044
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/13/2020
ms.locfileid: "91987568"
---
# <a name="azure-data-studio-faq"></a>FAQ sur Azure Data Studio

## <a name="what-is-azure-data-studio"></a>Qu’est-ce qu’Azure Data Studio ?

Azure Data Studio est un nouvel environnement de bureau multiplateforme open source pour les professionnels des données qui utilisent la famille Azure Data de plateformes de données locales et cloud sur Windows, macOS et Linux. Précédemment publié sous le nom de SQL Operations Studio (préversion), Azure Data Studio offre une expérience d’éditeur moderne avec des fonctionnalités IntelliSense ultra rapides, des extraits de code, l’intégration du contrôle de code source et un terminal intégré. Il est conçu en tenant compte de l’utilisateur de la plateforme de données, avec l’intégration de la représentation graphique des jeux de résultats de requête et des tableaux de bord personnalisables.

La recherche a montré que les utilisateurs passent significativement plus de temps à travailler sur la modification des requêtes que sur toute autre tâche avec SQL Server Management Studio. Pour cette raison, Azure Data Studio a été conçu pour se concentrer profondément sur les fonctionnalités utilisées le plus, avec des expériences supplémentaires disponibles sous forme d’extensions facultatives dans le produit. Cela permet à tous les utilisateurs de personnaliser leur environnement pour les flux de travail qu’ils utilisent le plus souvent.

## <a name="how-much-does-azure-data-studio-cost"></a>Combien coûte Azure Data Studio ?

Azure Data Studio est gratuit pour une utilisation privée ou commerciale.

## <a name="who-should-use-azure-data-studio"></a>Qui doit utiliser Azure Data Studio ?

Tout le monde peut utiliser Azure Data Studio. Toutefois, il est conçu pour simplifier les tâches effectuées par les développeurs de bases de données, les administrateurs de bases de données, les administrateurs système et les éditeurs de logiciels indépendants.

## <a name="what-can-i-do-with-azure-data-studio"></a>Que puis-je faire avec Azure Data Studio ?

Azure Data Studio s’appuie sur Visual Studio Code et offre une expérience de flux de travail de code moderne, centrée sur le clavier lorsque vous travaillez avec SQL Server, Azure SQL Database ou Azure SQL DW. Azure Data Studio facilite et simplifie les expériences de base que vous utilisez chaque jour grâce à des fonctionnalités intégrées telles que la présence de plusieurs fenêtres à onglets, un éditeur SQL riche, IntelliSense, la saisie semi-automatique de mots clés, des extraits de code/la navigation dans le code et l’intégration du contrôle de code source (Git et TFS). Vous pouvez exécuter des requêtes à la demande, afficher et enregistrer des résultats au format de texte, JSON ou Excel, modifier des données, organiser et gérer vos connexions de base de données favorites et parcourir les objets de base de données dans une expérience de navigation d’objets familière.

Utilisez vos outils en ligne de commande préférés (par exemple Bash, PowerShell, sqlcmd, bcp, psql et ssh) dans la fenêtre de terminal intégrée directement dans l’interface utilisateur d’Azure Data Studio. Générez et exécutez facilement des scripts CREATE et INSERT pour vos objets de base de données afin de créer des copies de votre base de données à des fins de développement ou de test. Améliorez votre productivité avec des extraits de code intelligents et des expériences graphiques riches qui créent de nouvelles bases de données et d’autres objets de base de données (tels que des tables, des vues, des procédures stockées, des utilisateurs, des connexions, des rôles, etc.) ou mettent à jour des objets de base de données existants. Utilisez des tableaux de bord enrichis personnalisables pour surveiller et résoudre rapidement les goulots d’étranglement des performances dans vos bases de données locales, dans Azure ou dans n’importe quel cloud.

Azure Data Studio offre une expérience cohérente pour la sauvegarde et la restauration de vos bases de données. Avec la prise en charge planifiée des groupes de disponibilité Always On SQL Server, vous pouvez facilement configurer, surveiller et dépanner les groupes pour vos bases de données SQL Server critiques et basculer rapidement vers une base de données secondaire en cas de sinistre. Azure Data Studio a été conçu pour vous rendre plus productif dans le cycle de vie DevOps des bases de données de votre choix sur les systèmes d’exploitation de votre choix. Par conséquent, vous avez toujours le contrôle et vous pouvez réduire les risques, résoudre les problèmes plus rapidement et fournir en continu une valeur qui dépasse les attentes des clients.

## <a name="is-azure-data-studio-open-source"></a>Est-ce qu’Azure Data Studio est open source ?

Le code source d’Azure Data Studio et de ses fournisseurs de données est disponible sur GitHub. Le code source pour le frontal Azure Data Studio (basé sur Visual Studio Code) est disponible dans le cadre d’un CLUF de code source qui fournit des droits pour modifier et utiliser le logiciel, mais pas pour le redistribuer ou l’héberger dans un service cloud. Le code source des fournisseurs de données est disponible sous licence MIT sur [https://github.com/Microsoft/sqltoolsservice](https://github.com/Microsoft/sqltoolsservice).

## <a name="do-we-plan-to-open-source-ssms"></a>Envisagez-vous de rendre SSMS open source ?

Non. Toutefois, les outils de l’interface de ligne de commande multi-OS et de l’interface graphique utilisateur de nouvelle génération sont open source. Par exemple, l’extension mssql pour VS Code, mssql-scripter, et msql-CLI sont tous open source sur GitHub. Le code source d’Azure Data Studio est disponible sur GitHub.  

## <a name="now-that-there-is-azure-data-studio-does-microsoft-plan-to-deprecate-ssms-and-ssdt"></a>Maintenant qu’il y a Azure Data Studio, Microsoft prévoit-il de déprécier SSMS et SSDT ? 

Non. Les investissements dans les outils phares Windows (SSMS, SSDT, PowerShell) continueront en plus de la prochaine génération d’outils d’interface de ligne de commande multi-OS et multi-BDD. L’objectif est d’offrir aux clients le choix d’utiliser les outils qu’ils souhaitent sur les plateformes de leur choix pour leurs scénarios. Azure Data Studio est plus étroitement axé sur les expériences en matière d’édition de requêtes et de développement de données, qui selon nos recherches est de loin la fonctionnalité la plus utilisée dans SQL Server Management Studio. D’autres fonctionnalités d’administration de haute valeur, telles que la sauvegarde, la restauration, la gestion des travaux d’agent et le profilage de serveur, sont également disponibles en tant qu’extensions dans Azure Data Studio. Azure Data Studio est également multiplateforme, ce qui permet aux utilisateurs de travailler sur la plateforme de leur choix. Toutefois, SQL Server Management Studio offre toujours le plus grand nombre de fonctions d’administration et reste l’outil phare pour les tâches de gestion de plateforme. 

## <a name="when-should-i-use-azure-data-studio-vs-sql-server-management-studio"></a>Quand dois-je utiliser Azure Data Studio et SQL Server Management Studio ?

*Utilisez Azure Data Studio si vous :*

- Passez la majeure partie de votre temps à modifier ou exécuter des requêtes.
- Avez besoin de visualiser et représenter graphiquement des jeux de résultats rapidement.
- Pouvez exécuter la plupart des tâches d’administration via le terminal intégré à l’aide de sqlcmd ou de PowerShell.
- Avez des besoins minimaux en assistants.
- N’avez pas besoin d’effectuer une configuration d’administration ou de plateforme complète.
- Devez exécuter la solution sur macOS ou Linux.

*Utilisez SQL Server Management Studio si vous :*

- Consacrez la majeure partie de votre temps aux tâches d’administration de base de données.
- Effectuez une configuration administrative ou de plateforme complexe.
- Effectuez la gestion de la sécurité, notamment la gestion des utilisateurs, l’évaluation des vulnérabilités et la configuration des fonctionnalités de sécurité.
- Devez utiliser les conseillers et tableaux de bord d’optimisation des performances.
- Utilisez des diagrammes de base de données et des concepteurs de tables.
- Effectuez l’importation/exportation de DACPAC.
- Devez accéder aux serveurs inscrits.
- Utilisez le mode sqlcmd, les statistiques des requêtes actives ou les statistiques du client.

## <a name="feature-comparison"></a>Comparaison des fonctionnalités

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
|Thèmes|Oui||
|Mode sombre|Oui||
|Azure Resource Explorer|PRÉVERSION||
|Assistant Générer des scripts||Oui
|Importation/exportation de DACPAC||Oui|
|Propriétés des objets||Oui|
|Concepteur de tables||Oui|

### <a name="query-editor"></a>Éditeur de requête

|Fonctionnalité|Azure Data Studio|SSMS|
|:---|:---|:---|
|Visionneuse de graphiques|Oui||
|Exportation des résultats au format CSV, JSON, XLSX|Oui||
|IntelliSense|Oui|Oui|
|Extraits de code|Oui|Oui|
|Plan d’affichage|PRÉVERSION|Oui|
|Statistiques du client||Oui|
|Statistiques des requêtes dynamiques||Oui|
|Options de requête||Oui|
|Résultats dans un fichier||Oui|
|Résultats dans du texte||Oui|
|Visionneuse spatiale||Oui|
|SQLCMD||Oui|
|Débogueur T-SQL||Oui|

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
|Importation de fichiers plats|PRÉVERSION|Oui|
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
|SQL Mail||Oui|
|Explorateur de modèles||Oui|
|Évaluation des vulnérabilités||Oui|
|Gestion de XEvent||Oui|


## <a name="azure-data-studio-is-missing-a-feature-that-is-in-ssmsssdt-will-you-add-it"></a>Azure Data Studio ne propose pas une fonctionnalité qui se trouve dans SSMS/SSDT. Allez-vous l’ajouter ?

Cela dépend du scénario et des besoins des clients et entreprises. Pour faciliter la hiérarchisation des priorités, envoyez une suggestion sur [GitHub](https://github.com/microsoft/azuredatastudio/issues).

## <a name="i-understand-azure-data-studio-and-the-mssql-extension-for-vs-code-are-powered-by-a-new-tools-service-that-uses-smo-apis-under-the-covers-is-smo-available-on-linux-and-macos"></a>Je comprends qu’Azure Data Studio et l’extension mssql pour VS Code reposent sur un nouveau service d’outils qui utilise les API SMO. SMO est-il disponible sur Linux et macOS ?

Les API SMO ne sont pas encore disponibles sur Linux ou macOS de manière consommable. Nous avons porté sur un sous-ensemble des API SMO dont nous avions besoin pour Azure Data Studio sur .NET Core et nous prévoyons de développer cela dans le cadre de notre feuille de route. Le service Outils SQL se trouve sur GitHub : [https://github.com/Microsoft/sqltoolsservice](https://github.com/Microsoft/sqltoolsservice).

## <a name="do-you-plan-to-port-the-dacfx-apis-andor-sqlpackageexe-andor-ssdt-to-linux-and-macos"></a>Prévoyez-vous de porter les API DACFx et/ou sqlpackage.exe et/ou SSDT sur Linux et macOS ?

Cela fait partie de notre feuille de route à long terme. Pour faciliter la hiérarchisation des priorités, envoyez une suggestion sur [GitHub](https://github.com/microsoft/azuredatastudio/issues).

## <a name="will-sql-powershell-cmdlets-be-available-on-linux-and-macos"></a>Les applets de commande SQL PowerShell seront-elles disponibles sur Linux et macOS ?

SQL PowerShell est disponible dès aujourd’hui dans PowerShell Gallery et vous pouvez l’utiliser sur Windows pour travailler avec des instances SQL Server qui s’exécutent n’importe où, y compris SQL sur Linux. La mise à disposition des applets de commande SQL PowerShell sur Linux & macOS figure dans la feuille de route. Pour faciliter la hiérarchisation des priorités, envoyez une suggestion sur [GitHub](https://github.com/microsoft/azuredatastudio/issues).

## <a name="who-usually-uses-azure-data-studio"></a>Qui utilise généralement Azure Data Studio ?

Les développeurs et les administrateurs de bases de données sont les utilisateurs typiques d’Azure Data Studio.

## <a name="does-azure-data-studio-integrate-with-azure-synapse-analytics"></a>Azure Data Studio s’intègre-t-il à Azure Synapse Analytics ?

Oui. La prise en charge Azure Data Studio d’Azure Synapse Analytics est en préversion, avec Azure SQL Managed Instance et Big Data SQL Server 2019.

## <a name="why-is-azure-data-studio-important-for-the-new-version-of-sql-server"></a>Pourquoi Azure Data Studio est-il important pour la nouvelle version de SQL Server ?

Alors que SQL Server étend ses fonctionnalités dans l’espace du Big Data, il a besoin de nouveaux outils pour prendre en charge ces nouveaux cas d’utilisation. C’est la raison pour laquelle Azure Data Studio offre une nouvelle expérience (en préversion) de prise en charge de SQL Server Big Data, y compris la première expérience de notebook dans l’ensemble d’outils SQL Server et un nouvel assistant de création de table externe qui rend l’accès aux données à partir d’instances SQL Server et Oracle facile et rapide.
