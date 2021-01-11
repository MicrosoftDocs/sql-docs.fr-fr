---
title: Notes de publication pour SQL Server Data Tools (SSDT)
description: Consultez les notes de publication pour toutes les versions de SQL Server Data Tools (SSDT) fonctionnant avec Visual Studio 2017 et les versions antérieures de Visual Studio.
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssdt
ms.topic: conceptual
ms.assetid: b071f8b8-c8e5-44e0-bbb6-04804dd1863a
author: dzsquared
ms.author: drskwier
ms.reviewer: maghan
ms.custom: seo-lt-2019
ms.date: 12/15/2019
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||=azuresqldb-mi-current'
ms.openlocfilehash: a380eabba4a20a65fd291b938d270a440f66d754
ms.sourcegitcommit: 44eebb659f9b226c08aea6c31a909b22ad4fec60
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/04/2021
ms.locfileid: "97860607"
---
# <a name="release-notes-for-sql-server-data-tools-ssdt"></a>Notes de publication pour SQL Server Data Tools (SSDT)

[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Ces notes de publication concernent [SQL Server Data Tools (SSDT)](download-sql-server-data-tools-ssdt.md) pour Visual Studio (VS).

<!--
Hello.  We have switched to a newer standardized format for Release Notes articles.
Basically we have switched from bullet lists to a 2-column table.
And we have shortened the H2 titles, to reduce wrapping in the rightNav.
And we have renamed the .md file to the standard format, from 'changelog-for-sql-server-data-tools-ssdt.md' to 'release-notes-ssdt.md'.
The presently latest H2 section (## 15.9.0) has been converted to the new format.
But the older sections are too numerous and long to warrant conversion.

REQUEST_1:  Please use the newer 2-column table format from now onward, for each new release section.

REQUEST_2:  Please consider whether it is time to erase perhaps the oldest 25% of these H2 sections.
Or maybe move all but the latest 9 (of 25) H2 sections to a new file perhaps named 'release-notes-history-ssdt.md', and link to it from the bottom of this file.

For questions, contact CraigG or SStein or GeneMi.

GeneMi , 2019/03/22.

P.S.  there's no need to keep this large HTML comment indefinitely.
-->

## <a name="1597nbsp-ssdt-for-vs-2017"></a>15.9.7,&nbsp; SSDT pour VS 2017

_Mise en production :_ &nbsp; 5 janvier 2021  
_Numéro de build :_ &nbsp; 14.0.16228.0  
_SSDT pour Visual Studio 2017._

### <a name="whats-new"></a>Nouveautés

| Nouvel élément | Détails |
|-----------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Integration Services (SSIS) | Rendez la création d’une SSISDB facultative dans l’assistant Création d’Integration Services. |
| Integration Services (SSIS) | Correction d’un problème dans lequel les éléments de Zone de liste modifiable d’abonnement Azure sont dupliqués dans l’Assistant de création IR et l’Assistant Projet pour Azure lorsque des abonnements différents portent le même nom. |
| Integration Services (SSIS) | Correction d’un problème qui entraînait parfois l’impossibilité d’activer le bouton Connexion dans l’Assistant Création IR. |
| Integration Services (SSIS) | Correction d’un problème dans lequel les éléments de Zone de liste modifiable d’abonnement Azure sont dupliqués dans l’Assistant de création IR et l’Assistant Projet pour Azure lorsque des abonnements différents portent le même nom. |
| Integration Services (SSIS) | Correction d’un problème où le code généré automatiquement sous bufferwrapper.cs du composant de script ajoutait des guillemets doubles quand les paramètres régionaux actuels étaient réglés sur Allemagne. |
| Integration Services (SSIS) | Résolution d’un problème où le bouton Télécharger WSDL ne s’affichait pas lorsque la version du serveur cible était SQL Server 2012, 2014, 2016. |
| Integration Services (SSIS) | Résolution d’un problème à cause duquel la génération de projets volumineux pouvait échouer en raison d’une exception de mémoire insuffisante. |
| Integration Services (SSIS) | Correction d’un problème qui indiquait que le package n’était pas rétrogradé à la version actuelle du serveur cible lorsqu’il était enregistré en tant que copie dans le système de fichiers ou dans MSDB dans le modèle de déploiement de package. |
| Integration Services (SSIS) | Correction d’un problème dans lequel la destination de traitement de dimension ne fonctionnait pas en raison d’une erreur « Cette interface est introuvable ». |
| Integration Services (SSIS) | Correction de certains problèmes liés à l’accessibilité et aux résolutions élevées. |

### <a name="known-issues"></a>Problèmes connus

| Problème connu | Détails |
| :---------- | :------ |
| La tâche d’exécution de package SSIS ne prend pas en charge le débogage quand ExecuteOutOfProcess a la valeur True. | Ce problème s’applique uniquement au débogage. L’enregistrement, le déploiement et l’exécution via DTExec.exe ou le catalogue SSIS ne sont pas affectés. |
| &nbsp; | &nbsp; |


## <a name="1596nbsp-ssdt-for-vs-2017"></a>15.9.6,&nbsp; SSDT pour VS 2017

_Mise en production :_ &nbsp; 31 août 2020  
_Numéro de build :_ &nbsp; 14.0.16222.0  
_SSDT pour Visual Studio 2017._

### <a name="whats-new"></a>Nouveautés

| Nouvel élément | Détails |
|-----------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Integration Services (SSIS) | Correction d’un problème qui empêchait le bouton **Aperçu** de fonctionner sur la source OLE DB lors de la connexion à une source de données SQL Server Analysis Services (SSAS). |
| Integration Services (SSIS) | Correction d’un problème qui, en cas de suppression d’une entrée ou d’une sortie d’un composant de workflow avant la suppression du chemin associé, pouvait entraîner une erreur COMException. |
| Integration Services (SSIS) | Correction du problème à cause duquel la tâche de traitement SSAS ne pouvait pas se connecter à un espace de travail Power BI et actualiser ses modèles. |
| Integration Services (SSIS) | Correction d’un problème à cause duquel Visual Studio bloquait lors du débogage d’une tâche de script/d’un composant lors de l’utilisation du runtime x64 et du ciblage SQL Server 2017. |
| Integration Services (SSIS) | Correction d’un problème lié au plantage de l’assistant d’importation/exportation lors de la sélection d’un pilote MySQL dans certains environnements. |
| Integration Services (SSIS) | Correction de certains problèmes liés à l’accessibilité et aux résolutions élevées. |
| Integration Services (SSIS) | Les utilisateurs peuvent maintenant ignorer la validation lors de l’ouverture des packages, ce qui améliore les performances. Pour plus d’informations, consultez [Accélérer l’ouverture des packages SSIS dans SSDT](https://techcommunity.microsoft.com/t5/sql-server-integration-services/accelerate-the-opening-of-ssis-package-in-ssdt/ba-p/1607099). |
| Integration Services (SSIS) | Bloquer le déploiement sur Azure-SSIS lorsque la version du serveur cible n’est pas SQL Server 2017. |

### <a name="known-issues"></a>Problèmes connus

| Problème connu | Détails |
| :---------- | :------ |
| La tâche d’exécution de package SSIS ne prend pas en charge le débogage quand ExecuteOutOfProcess a la valeur True. | Ce problème s’applique uniquement au débogage. L’enregistrement, le déploiement et l’exécution via DTExec.exe ou le catalogue SSIS ne sont pas affectés. |
| La source Power Query peut ne pas prendre en charge OData v4 quand SSIS et SSAS sont installés sur la même instance de Visual Studio. | &nbsp; |
| La source Power Query peut ne pas prendre en charge l’utilisation d’ODBC pour se connecter à Oracle quand SSIS et SSAS sont installés sur la même instance de Visual Studio. | &nbsp; |
| La source Power Query n’est pas localisée. | &nbsp; |
| &nbsp; | &nbsp; |

## <a name="1595nbsp-ssdt-for-vs-2017"></a>15.9.5,&nbsp; SSDT pour VS 2017

_Mise en production :_ &nbsp; 27 mai 2020  
_Numéro de build :_ &nbsp; 14.0.16218.0  
_SSDT pour Visual Studio 2017._

### <a name="whats-new"></a>Nouveautés

| Nouvel élément | Détails |
|-----------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Integration Services (SSIS) | Ajout de la prise en charge de la recherche de tâches et des composants de pipeline en insérant une zone de recherche dans la boîte à outils SSIS. |
| Integration Services (SSIS) | Ajout de la barre de progression lors du basculement de la version du serveur cible. |
| Integration Services (SSIS) | Ajout d’une configuration cloud supplémentaire pour un projet compatible Azure et ajout de la prise en charge de l’authentification Windows pour l’exécution de packages dans Azure. |
| Integration Services (SSIS) | Ajout de l’évaluation des packages à exécuter dans Azure dans un projet compatible Azure. |
| Integration Services (SSIS) | Correction d’un problème dans lequel des instances VS ne peuvent parfois pas être répertoriées dans le programme d’installation |
| Integration Services (SSIS) | Résolution d’un problème dans lequel cette production ne peut pas être désinstallée si l’instance VS a été désinstallée. |
| Integration Services (SSIS) | Résolution d’un problème dans lequel un composant de script copié à partir d’un autre package dans le même package ne peut pas être chargé correctement pendant le débogage lorsque la version du serveur cible est antérieure à SQL Server 2019. |
| Integration Services (SSIS) | Correction d’un problème d’accessibilité dans lequel les ratios de luminosité pour les lignes du connecteur du composant sont inférieurs à 3:1 dans la fenêtre du concepteur de packages. |
| Integration Services (SSIS) | Correction d’un problème d’accessibilité dans lequel le ratio de luminosité est inférieur à 3:1 pour le contrôle « Fit View to window » présent dans la fenêtre du concepteur de packages. |
| Integration Services (SSIS) | Résolution d’un problème dans lequel la tâche de transfert de bases de données ne fonctionne pas quand une base de données contient des groupes de fichiers avec un flux de fichier. |
| Integration Services (SSIS) | Correction d’un problème dans lequel, lors de l’utilisation de composants ODBC dans le composant Boucle ForEach, le composant ODBC affichera le message « Erreur de séquence de fonction » dans la deuxième boucle au cours de l’exécution du package. |
| Integration Services (SSIS) | Correction d’un problème dans lequel l’interface utilisateur de la tâche Reconstruire l’index sera tronquée en mode basse résolution. |
| Integration Services (SSIS) | Résolution d’un problème dans lequel le bouton « Se connecter » ne s’affiche pas en mode haute résolution. |
| Integration Services (SSIS) | Résolution d’un problème dans lequel les éléments du gestionnaire de connexions étaient affichés trop grands en mode haute résolution. |
| Integration Services (SSIS) | Résolution d’un problème dans lequel les résultats de l’exécution sont empilés les uns sur les autres en mode haute résolution. |

### <a name="known-issues"></a>Problèmes connus

| Problème connu | Détails |
| :---------- | :------ |
| La tâche d’exécution de package SSIS ne prend pas en charge le débogage quand ExecuteOutOfProcess a la valeur True. | Ce problème s’applique uniquement au débogage. L’enregistrement, le déploiement et l’exécution via DTExec.exe ou le catalogue SSIS ne sont pas impactés. |
| La source Power Query peut ne pas prendre en charge OData v4 quand SSIS et SSAS sont installés sur la même instance de Visual Studio. | &nbsp; |
| La source Power Query peut ne pas prendre en charge l’utilisation d’ODBC pour se connecter à Oracle quand SSIS et SSAS sont installés sur la même instance de Visual Studio. | &nbsp; |
| La source Power Query n’est pas localisée | &nbsp; |
| Lorsque vous ciblez SQL Server 2017 et SxS avec SQL Server 2017 corrigé avec CU19 ou une version ultérieure de CU, les packages de débogage contenant la tâche de script/composant avec des points d’arrêt cessent de fonctionner si Run64BitRuntime est défini sur true. | &nbsp; |
| &nbsp; | &nbsp; |

## <a name="1594nbsp-ssdt-for-vs-2017"></a>15.9.4,&nbsp; SSDT pour VS 2017

_Mise en production :_ &nbsp; 26 mars 2020  
_Numéro de build :_ &nbsp; 14.0.16214.0  
_SSDT pour Visual Studio 2017._

### <a name="whats-new"></a>Nouveautés

| Nouvel élément | Détails |
|-----------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Integration Services (SSIS) | Correction d’un problème qui pouvait entraîner le plantage de VS quand des lignes de contrainte d’un flux de contrôle étaient déplacées au sein d’un conteneur. |
| Integration Services (SSIS) | Correction d’un problème qui empêchait l’interface utilisateur des tâches du plan de maintenance de lister les gestionnaires de connexions ADO.NET créés en dehors de l’interface utilisateur des tâches. |
| Integration Services (SSIS) | Correction d’un problème où la page de connexion interactive à Azure ne s’affichait pas lors du déploiement d’un projet SSAS appartenant à une solution dans laquelle étaient déjà chargés des projets SSIS. |
| Integration Services (SSIS) | Correction d’un problème qui entraînait le plantage de l’Assistant DTS quand vous cliquiez sur le bouton des propriétés du pilote MSOLAP et que SQL Server n’était pas installé. |
| Integration Services (SSIS) | Correction d’un problème où le pilote MSOLEDBSQL ne prenait pas en charge l’authentification AAD dans l’Assistant DTS. |
| Integration Services (SSIS) | Correction d’un problème où la source XML et la destination ADO.NET n’étaient pas conservés correctement lors du ciblage de SQL Server 2012. |
| Integration Services (SSIS) | Correction d’un problème qui pouvait empêcher le bouton « Télécharger WSDL » de s’afficher correctement dans l’éditeur de tâche de service web. |
| Integration Services (SSIS) | Correction d’un problème qui pouvait empêcher la sélection d’une table dans la page Gestionnaire de connexions de l’éditeur de transformation de recherche. |
| Integration Services (SSIS) | Correction d’un problème qui pouvait entraîner un affichage incorrect dans l’éditeur de transformation du cache. |
| Integration Services (SSIS) | Correction d’un problème qui pouvait empêcher la zone « Gestionnaires de connexions » de s’afficher correctement dans l’éditeur de package. |
| Integration Services (SSIS) | Correction d’un problème qui pouvait empêcher l’icône d’état de s’afficher correctement dans l’Assistant Convertir en modèle de déploiement de package. |
| Integration Services (SSIS) | Changement du programme d’installation en un programme d’installation complet qui ne nécessite pas de téléchargement de charge utile à partir d’Internet. |

### <a name="known-issues"></a>Problèmes connus

| Problème connu | Détails |
| :---------- | :------ |
| La tâche d’exécution de package SSIS ne prend pas en charge le débogage quand ExecuteOutOfProcess a la valeur True. | Ce problème s’applique uniquement au débogage. L’enregistrement, le déploiement et l’exécution via DTExec.exe ou le catalogue SSIS ne sont pas impactés. |
| La source Power Query peut ne pas prendre en charge OData v4 quand SSIS et SSAS sont installés sur la même instance de Visual Studio. | &nbsp; |
| La source Power Query peut ne pas prendre en charge l’utilisation d’ODBC pour se connecter à Oracle quand SSIS et SSAS sont installés sur la même instance de Visual Studio. | &nbsp; |
| La source Power Query n’est pas localisée | &nbsp; |
| &nbsp; | &nbsp; |

## <a name="1593nbsp-ssdt-for-vs-2017"></a>15.9.3,&nbsp; SSDT pour VS 2017

_Mise en production :_ &nbsp; 3 janvier 2020  
_Numéro de build :_ &nbsp; 14.0.16203.0  
_SSDT pour Visual Studio 2017._

### <a name="whats-new"></a>Nouveautés

| Nouvel élément | Détails |
|-----------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Integration Services (SSIS) | Suppression du composant de boîte de réception Source Power Query pour SQL Server 2017. Nous avons maintenant annoncé la source Power Query comme composant prêt à l’emploi pour SQL Server 2017 & 2019, et vous pouvez le télécharger [ici](https://www.microsoft.com/en-us/download/details.aspx?id=100619). |
| Integration Services (SSIS) | Suppression du composant de boîte de réception Microsoft Oracle Connector pour SQL Server 2019. Nous avons maintenant annoncé le connecteur Microsoft Oracle pour SQL Server 2019 en tant que composant prêt à l’emploi, et vous pouvez le télécharger [ici](https://www.microsoft.com/en-us/download/details.aspx?id=58228). |
| Integration Services (SSIS) | Correction d’un problème de lancement occasionnel du débogueur SSIS en raison de l’interface IDtsHost non inscrite lorsque la version du serveur cible est SQL Server 2017 ou 2019. |
| Integration Services (SSIS) | Correction des principaux problèmes de disposition de l’interface utilisateur en mode haute résolution. |
| Integration Services (SSIS) | .NET Framework mis à niveau vers la version 4.7 pour les tâches/composants de script lorsque la version du serveur cible est SQL Server 2019. |
| Integration Services (SSIS) | Ajout de la propriété ConnectByProxy dans le gestionnaire de connexions ODBC afin de prendre en charge l’activation du runtime d'intégration auto-hébergé en tant que proxy dans le gestionnaire de connexions ODBC. |
| Integration Services (SSIS) | Résolution d’un problème où les utilisateurs ne pouvaient pas ajouter de nouvelles sources de données en mode de déploiement de package. |
| Integration Services (SSIS) | Résolution d’un problème où les utilisateurs ne pouvaient pas déboguer les tâches/composants de script si le code utilisait les nouvelles syntaxes introduites après .NET 4.5. |
| Integration Services (SSIS) | Résolution d’un problème qui provoquait l’échec de la création du premier Data Factory dans un abonnement Azure via l’assistant de création de runtime d'intégration, en raison de l’absence d’inscription du fournisseur de ressources Data Factory. |
| Integration Services (SSIS) | Correction du problème à cause duquel l’assistant de connexion SSIS dans ADF ne pouvait pas afficher correctement la liste des comptes de stockage Azure lorsqu’un seul compte de stockage de fichiers était présent dans l’abonnement. |
| Integration Services (SSIS) | Résolution d’un problème où « Exécuter dans Azure » ne fonctionnait pas lorsque le package comprenait un conteneur. |
| Integration Services (SSIS) | Résolution d’un problème lié au fait que char(n char) et varchar2(n char) étaient mappés à des types DTS incorrects dans le connecteur Oracle. |

### <a name="known-issues"></a>Problèmes connus

| Problème connu | Détails |
| :---------- | :------ |
| La tâche d’exécution de package SSIS ne prend pas en charge le débogage quand ExecuteOutOfProcess a la valeur True. | Ce problème s’applique uniquement au débogage. L’enregistrement, le déploiement et l’exécution via DTExec.exe ou le catalogue SSIS ne sont pas impactés. |
| La source Power Query peut ne pas prendre en charge OData v4 quand SSIS et SSAS sont installés sur la même instance de Visual Studio. | &nbsp; |
| La source Power Query peut ne pas prendre en charge l’utilisation d’ODBC pour se connecter à Oracle quand SSIS et SSAS sont installés sur la même instance de Visual Studio. | &nbsp; |
| La source Power Query n’est pas localisée | &nbsp; |
| &nbsp; | &nbsp; |

## <a name="1592nbsp-ssdt-for-vs-2017"></a>15.9.2, SSDT pour VS&nbsp;2017

_Mise en production :_ &nbsp; 17 juillet 2019  
_Numéro de build :_ &nbsp; 14.0.16194.0  
_SSDT pour Visual Studio 2017._

### <a name="whats-new"></a>Nouveautés

| Nouvel élément | Détails |
|-----------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Integration Services (SSIS) | Ajouter la fonctionnalité AzureEnabled. Activer les packages du projet à exécuter sur la plateforme PaaS (Platform-as-a-Service) SSIS dans Azure Data Factory |
| Integration Services (SSIS) | Résolution d’un problème qui empêchait la définition des propriétés du connecteur Oracle à partir d’une expression de variable |
| Integration Services (SSIS) | Résolution d’un problème où le connecteur Oracle a une erreur VS_NEEDSNEWMETATDATA lors du débogage de packages ciblés pour des versions antérieures à SQL Server 2019 |
| Integration Services (SSIS) | Résolution d’un problème où le connecteur Oracle n’a pas pu mettre à niveau/passer à une version antérieure un package/projet si le package/projet utilise des expressions pour les propriétés du gestionnaire de connexions |
| Integration Services (SSIS) | Résolution d’un problème où le bouton Télécharger WSDL de l’Éditeur de tâche de service web ne prend pas en charge le protocole TLS 1.1 et 1.2 (la cible est SQL Server 2019) |
| Integration Services (SSIS) | Résolution d’un problème qui empêchait le rechargement des packages contenant le gestionnaire de connexions DQS après l’enregistrement |

### <a name="known-issues"></a>Problèmes connus

| Problème connu | Détails |
| :---------- | :------ |
| La tâche d’exécution de package SSIS ne prend pas en charge le débogage quand ExecuteOutOfProcess a la valeur True. | Ce problème s’applique uniquement au débogage. L’enregistrement, le déploiement et l’exécution via DTExec.exe ou le catalogue SSIS ne sont pas impactés. |
| Impossible de créer ou de modifier des sources de données dans le modèle de déploiement de packages. | Échec de l’ouverture de l’Assistant Source de données. |
| La source Power Query peut ne pas prendre en charge OData v4 quand SSIS et SSAS sont installés sur la même instance de Visual Studio. | &nbsp; |
| La source Power Query peut ne pas prendre en charge l’utilisation d’ODBC pour se connecter à Oracle quand SSIS et SSAS sont installés sur la même instance de Visual Studio. | &nbsp; |
| La source Power Query n’est pas localisée | &nbsp; |
| &nbsp; | &nbsp; |

## <a name="1591nbsp-ssdt-for-vs-2017"></a>15.9.1,&nbsp; SSDT pour VS 2017

_Mise en production :_ &nbsp; 27 avril 2019  
_Numéro de build :_ &nbsp; 14.0.16191.0  
_SSDT pour Visual Studio 2017._

### <a name="whats-new"></a>Nouveautés

| Nouvel élément | Détails |
|-----------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Integration Services (SSIS) | Résolution d’un problème où un composant de package ne peut pas être correctement rendu persistant lors du ciblage d’une version précédente de SQL Server. |
| Integration Services (SSIS) | Résolution d’un problème qui empêche l’ajout d’une expression à une contrainte de précédence lors de l’utilisation d’un composant de package. |
| Integration Services (SSIS) | Résolution d’un problème où le bouton « Help » du gestionnaire de sources et de connexions Power Query ne renvoie pas au bon document. |
| Integration Services (SSIS) | Résolution d’un problème d’affichage de la version de build SSIS dans la fenêtre d’aide de VS. |
| Integration Services (SSIS) | Ajout de la propriété « ConnectByProxy » pour le gestionnaire de connexions Ole DB et Fichier plat, qui permet d’accéder aux données locales avec l’IR auto-hébergé dans Azure-SSIS IR. |
| Integration Services (SSIS) | Résolution d’un problème concernant les composants ODBC qui sont mappés aux types de données DT_DBDATE de manière erronée. |
| Integration Services (SSIS) | Ajout de la propriété « ConnectUsingManagedIdentity » pour le gestionnaire de connexions ADO.NET et OLE DB, qui active l’authentification d’identité managée pour se connecter à la source de données dans Azure-SSIS IR. |

### <a name="known-issues"></a>Problèmes connus

| Problème connu | Détails |
| :---------- | :------ |
| La tâche d’exécution de package SSIS ne prend pas en charge le débogage quand ExecuteOutOfProcess a la valeur True. | Ce problème s’applique uniquement au débogage. L’enregistrement, le déploiement et l’exécution via DTExec.exe ou le catalogue SSIS ne sont pas impactés. |
| Impossible de créer ou de modifier des sources de données dans le modèle de déploiement de packages. | Échec de l’ouverture de l’Assistant Source de données. |
| La source Power Query peut ne pas prendre en charge OData v4 quand SSIS et SSAS sont installés sur la même instance de Visual Studio. | &nbsp; |
| La source Power Query peut ne pas prendre en charge l’utilisation d’ODBC pour se connecter à Oracle quand SSIS et SSAS sont installés sur la même instance de Visual Studio. | &nbsp; |
| La source Power Query n’est pas localisée. | &nbsp; |
| &nbsp; | &nbsp; |

## <a name="1590nbsp-ssdt-for-vs-2017"></a>15.9.0,&nbsp; SSDT pour VS 2017

_Mise en production :_ &nbsp;28 janvier 2019  
_Numéro de build :_ &nbsp; 14.0.16186.0  
_SSDT pour Visual Studio 2017._

### <a name="whats-new"></a>Nouveautés

| Nouvel élément | Détails |
|-----------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Integration Services (SSIS) | Ajout de la source Power Query (préversion) pour SSIS dans ADF 2017. |
| Integration Services (SSIS) | Ajouter à nouveau la prise en charge de SQL Server 2012. |
| Integration Services (SSIS) | Ajout de la source et de la destination Oracle pour SQL Server 2019. |
| Integration Services (SSIS) | Les sources et destinations Oracle ciblant SQL Server 2019 ont déjà été installées par SSDT. <br/></br> Pour concevoir le package ciblant la version de serveur 2017 ou une version inférieure, téléchargez la version de connecteur Oracle correspondante à partir du site de téléchargement Microsoft et installez-la sur l’ordinateur SSDT. <br/></br> [Connecteur Microsoft Version 5.0 pour Oracle par Attunity ciblant SQL Server 2017](https://www.microsoft.com/download/details.aspx?id=55179 ) <br/></br> [Connecteur Microsoft Version 4.0 pour Oracle par Attunity ciblant SQL Server 2016](https://www.microsoft.com/download/details.aspx?id=52950 )<br/></br> [Connecteur Microsoft Version 3.0 pour Oracle par Attunity ciblant SQL Server 2014](https://www.microsoft.com/download/details.aspx?id=44582 )<br/></br> [Connecteur Microsoft Version 2.0 pour Oracle par Attunity ciblant SQL Server 2012](https://www.microsoft.com/download/details.aspx?id=29283 ) |
| Integration Services (SSIS) | Correction d’un problème qui empêchait le chargement du composant/de la tâche de script lors de la migration à partir de versions antérieures de SSIS. |
| Integration Services (SSIS) | Correction d’un problème qui empêchait la visionneuse de données de fonctionner sur Windows 7 SP1 et Windows 8.1. |
| Integration Services (SSIS) | Correction d’un problème où, dans certains cas, l’enregistrement du package entraînait le plantage de Visual Studio. |
| Integration Services (SSIS) | Résolution d’un problème qui, dans certains cas, empêche l’exécution du package. |
| Integration Services (SSIS) | Ce problème se produit quand les deux conditions suivantes sont remplies :< br />< br /> &bull;   Le niveau de protection est EncryptSensitiveWithPassword.< br /> &bull;   La version du serveur cible est antérieure à SQL Server 2017.          |
| Integration Services (SSIS) | Correction d’un problème qui empêchait l’affichage des annotations avec une police par défaut dans SSDT. |
| Integration Services (SSIS) | ISDeploymentWizard prend en charge l’authentification SQL Server, l’authentification intégrée à Azure Active Directory et l’authentification par mot de passe Azure Active Directory en mode ligne de commande. |

### <a name="known-issues"></a>Problèmes connus

| Problème connu | Détails |
| :---------- | :------ |
| La tâche d’exécution de package SSIS ne prend pas en charge le débogage quand ExecuteOutOfProcess a la valeur True. | Ce problème s’applique uniquement au débogage. L’enregistrement, le déploiement et l’exécution via DTExec.exe ou le catalogue SSIS ne sont pas impactés. |
| La source Power Query peut ne pas prendre en charge OData v4 quand SSIS et SSAS sont installés sur la même instance de Visual Studio. | &nbsp; |
| La source Power Query peut ne pas prendre en charge l’utilisation d’ODBC pour se connecter à Oracle quand SSIS et SSAS sont installés sur la même instance de Visual Studio. | &nbsp; |
| La source Power Query n’est pas localisée. | &nbsp; |
| &nbsp; | &nbsp; |

## <a name="1582nbsp-ssdt-for-vs-2017"></a>15.8.2,&nbsp; SSDT pour VS 2017

_Mise en production :_ &nbsp;5 novembre 2018  
_Numéro de build :_ &nbsp; 14.0.16182.0  
_SSDT pour Visual Studio 2017._

### <a name="whats-new"></a>Nouveautés
**SSIS :**

Correction d’un problème faisant en sorte que le déploiement d’un projet SSIS qui contient des packages contenant la destination Fichier plat/Tâche de script vers Azure-SSIS entraîne l’échec de l’exécution des packages dans Azure-SSIS. 

### <a name="known-issues"></a>Problèmes connus :

- La tâche d’exécution de package SSIS ne prend pas en charge le débogage quand ExecuteOutOfProcess a la valeur True. Ce problème s’applique uniquement au débogage. L’enregistrement, le déploiement et l’exécution via DTExec.exe ou le catalogue SSIS ne sont pas impactés.

## <a name="1581nbsp-ssdt-for-vs-2017"></a>15.8.1,&nbsp; SSDT pour VS 2017

_Mise en production :_ &nbsp;27 septembre 2018  
_Numéro de build :_ &nbsp; 14.0.16179.0  
_SSDT pour Visual Studio 2017._

### <a name="whats-new"></a>Nouveautés

**SSIS :**

1. Ajout de la prise en charge de [!INCLUDE[sql-server-2019](../includes/sssqlv15-md.md)].
2. Suppression de la prise en charge de SQL Server 2012.

### <a name="known-issues"></a>Problèmes connus :

- La tâche d’exécution de package SSIS ne prend pas en charge le débogage quand ExecuteOutOfProcess a la valeur True. Ce problème s’applique uniquement au débogage. L’enregistrement, le déploiement et l’exécution via DTExec.exe ou le catalogue SSIS ne sont pas impactés.
- Le déploiement de projets SSIS qui ont des packages contenant la destination Fichier plat/Tâche de script vers Azure-SSIS entraîne l’échec de l’exécution des packages dans Azure-SSIS.

## <a name="158nbsp-ssdt-for-vs-2017"></a>15.8,&nbsp; SSDT pour VS 2017

_Mise en production :_ &nbsp;5 septembre 2018  
_Numéro de build :_ &nbsp; 14.0.16174.0  
_SSDT pour Visual Studio 2017._

### <a name="whats-new"></a>Nouveautés

**SSIS :**

1. Correction d’une régression dans VS 15.8 : l’enregistrement du composant/de la tâche de script provoquait une erreur de compilation.
1. Correctif dans VS 15.8 : l’Assistant Déploiement ne fonctionnait pas.
1. Correction d’un problème : le gestionnaire de connexions ADO.NET ne prenait pas en charge le fournisseur ADO.NET tiers.

**Programme d’installation :**

- Implémentation du redémarrage en cours de route lors de l’installation de SSDT sur Windows 10.


### <a name="known-issues"></a>Problèmes connus :

- La tâche d’exécution de package SSIS ne prend pas en charge le débogage quand ExecuteOutOfProcess a la valeur True. Ce problème s’applique uniquement au débogage. L’enregistrement, le déploiement et l’exécution via DTExec.exe ou le catalogue SSIS ne sont pas impactés.

## <a name="1571nbsp-ssdt-for-vs-2017"></a>15.7.1,&nbsp; SSDT pour VS 2017

_Mise en production :_ &nbsp;2 juillet 2018  
_Numéro de build :_ &nbsp; 14.0.16167.0  
_SSDT pour Visual Studio 2017._

### <a name="whats-new"></a>Nouveautés

**SSIS :**

- Prise en charge de la nouvelle autorité AAD Azure Government (login.microsoftonline.us), utilisée avec les tâches AS.
- Résolution du problème selon lequel l’interface utilisateur des tâches de traitement AS affichait « Méthode introuvable » lorsque la version du serveur cible était SQL Server 2016.
- Résolution du problème selon lequel certains composants de pipeline ne pouvaient pas s’exécuter lorsque la version du serveur cible était SQL Server 2012.

**Programme d’installation :**

- Filtrage de la liste des instances VS de façon à exclure celles qui ne peuvent pas installer SSDT.

### <a name="known-issues"></a>Problèmes connus :

- La tâche d’exécution de package SSIS ne prend pas en charge le débogage quand ExecuteOutOfProcess a la valeur True. Ce problème s’applique uniquement au débogage. L’enregistrement, le déploiement et l’exécution via DTExec.exe ou le catalogue SSIS ne sont pas impactés.
- L’installation de SSDT sur Windows 10 avec « Installer la nouvelle instance SQL Server Data Tools pour Visual Studio 2017 » échoue sur « L’opération de métafichier demandée n’est pas prise en charge ». Redémarrez l’ordinateur et relancez le programme d’installation SSDT pour continuer l’installation.

## <a name="1570nbsp-ssdt-for-vs-2017"></a>15.7.0,&nbsp; SSDT pour VS 2017

_Mise en production :_ &nbsp; 4 juin 2018  
_Numéro de build :_ &nbsp; 14.0.16165.0  
_SSDT pour Visual Studio 2017._

### <a name="whats-new"></a>Nouveautés

**SSIS :**

- Résolution du problème d’affichage de la page *Concepteurs Integration Services* dans la boîte de dialogue Options.  
- Résolution du problème lié au ratio de luminosité du texte dans *l’Éditeur de transformation de tri*.  
- Résolution du problème entraînant la disparition de la boîte de dialogue *Résoudre les références* quand vous tentiez de modifier une zone de liste modifiable.  
- Résolution du problème lié au non-fonctionnement du lien d’aide (F1) du *Gestionnaire de connexions Hadoop*.  
- Résolution du problème entraînant la perte du code d’une tâche de script dans un conteneur en cas de ciblage de SQL Server 2016.  

**Programme d’installation :**

- Résolution du problème empêchant l’installation de SSAS avant l’installation de SSRS et de SSIS dans Visual Studio 15.7.2.

### <a name="known-issues"></a>Problèmes connus :

- La tâche d’exécution de package SSIS ne prend pas en charge le débogage quand *ExecuteOutOfProcess* a la valeur *True*. Ce problème s’applique uniquement au débogage. L’enregistrement, le déploiement et l’exécution via DTExec.exe ou le catalogue SSIS ne sont pas impactés.

## <a name="1560nbsp-ssdt-for-vs-2017"></a>15.6.0,&nbsp; SSDT pour VS 2017

_Mise en production :_ &nbsp; 10 avril 2018  
_Numéro de build :_ &nbsp; 14.0.16162.0  
_SSDT pour Visual Studio 2017._

### <a name="whats-new"></a>Nouveautés

**SSIS :**

- Résolution du problème où la tâche de traitement AS ne journalisait aucune étape de traitement quand SQLServer2016 et SQLServer2017 étaient ciblés
- Résolution du problème où une violation d’accès se produisait à l’ouverture de dtsx avec des noms de tâches longs non anglais dans SSDT
- Résolution du problème où parfois la liste des variables de ScriptTask disparaissait de l’interface utilisateur de la tâche
- Résolution du problème où l’ajout d’une copie du package existant échouait quand l’emplacement du package était SQL Server
- Résolution du problème où le focus restait coincé lors de l’accès à la zone de liste déroulante dans une boîte de dialogue d’éditeur.
- Résolution du problème où l’arrière-plan ne changeait pas alors que le thème de Visual Studio changeait.
- Résolution du problème où l’étiquette d’annotation et de chargement était invisible dans le thème foncé.
- Résolution du problème où la propriété d’état n’était pas correctement définie pour les éléments désactivés de la boîte à outils SSIS.
- Résolution du problème où l’exécution de WebServiceTask échouait tout le temps.
- Résolution du problème où le déploiement du package échouait si la chaîne de connexion était définie sur une variable avec une expression dépendante des paramètres du projet.

**Programme d’installation :**

- Ajoutez le lien du « Programme d’amélioration du produit pour SQL Server Data Tools » dans la clause d’exclusion de confidentialité.
- Résolution du problème où la fenêtre du programme d’installation de Visual Studio apparaissait quand « Installer la nouvelle instance SQL Server Data Tools pour Visual Studio 2017 » était sélectionné

### <a name="known-issues"></a>Problèmes connus :
- La tâche d’exécution de package SSIS ne prend pas en charge le débogage quand ExecuteOutOfProcess a la valeur True. Ce problème s’applique uniquement au débogage. L’enregistrement, le déploiement et l’exécution via DTExec.exe ou le catalogue SSIS ne sont pas impactés.

## <a name="1552nbsp-ssdt-for-vs-2017"></a>15.5.2,&nbsp; SSDT pour VS 2017

_Numéro de build :_ &nbsp; 14.0.16156.0  
_SSDT pour Visual Studio 2017._

### <a name="whats-new"></a>Nouveautés

**SSIS**
- Correction d’un problème qui entraînait l’échec de la migration des projets SSIS 2008 quand SSAS et SSIS étaient tous deux installés sur la même instance de Visual Studio 2017.
- Correction d’un problème qui faisait que les projets Rdlc ne pouvaient pas être générés quand le Concepteur de rapports Rdlc et SSIS étaient installés sur la même instance de Visual Studio 2017.
- Correction d’un problème empêchant la mise à jour de la couleur d’annotation.
- Correction d’un problème qui faisait que certaines chaînes dans l’éditeur du gestionnaire de connexions Hadoop étaient tronquées dans d’autres langues.
- Correction d’un problème qui faisait que certaines chaînes étaient tronquées dans l’éditeur du gestionnaire de connexions OData.
- Correction d’un problème qui faisait que certaines chaînes étaient tronquées dans la fenêtre de l’Assistant Importation de projet d’Integration Services.
- Correction d’un problème de titre dans la fenêtre d’informations de la boîte à outils SSIS.
- Correction d’un problème qui faisait que certaines chaînes étaient tronquées dans la fenêtre de l’Assistant Déploiement d’Integration Services. 

**Programme d’installation**
- Correction d’un problème où parfois, le téléchargement de charge utile échouait avec l’erreur « Le système ne trouve pas le fichier spécifié (0x80070002) ».  

### <a name="known-issues"></a>Problèmes connus
- La tâche d’exécution de package SSIS ne prend pas en charge le débogage quand *ExecuteOutOfProcess* a la valeur *True*. Ce problème s’applique uniquement au débogage. L’enregistrement, le déploiement et l’exécution via DTExec.exe ou le catalogue SSIS ne sont pas impactés.

## <a name="1551nbsp-ssdt-for-vs-2017"></a>15.5.1,&nbsp; SSDT pour VS 2017

_Numéro de build :_ &nbsp; 14.0.16148.0  
_SSDT pour Visual Studio 2017._

### <a name="whats-new"></a>Nouveautés

Visual Studio 2017 (15.5.1) est la même version que la version 15.5.0 à l’exception des résolutions de bogues suivantes appliquées au programme d’installation :

1. Résolution d’un problème lié au fait que le programme d’installation cesse de répondre lors de la post-installation de SQL Server Integration Services.
2. Résolution d’un problème qui entraîne l’échec de l’installation avec le message d’erreur suivant : « L’opération de métafichier demandée n’est pas prise en charge (0x800707D3) ».

En plus de ces deux résolutions de bogues, les détails suivants pour 15.5.0 s’appliquent toujours à 15.5.1

## <a name="1550nbsp-ssdt-for-vs-2017"></a>15.5.0,&nbsp; SSDT pour VS 2017

_Numéro de build :_ &nbsp; 14.0.16146.0  
_SSDT pour Visual Studio 2017._

### <a name="whats-new"></a>Nouveautés

SSDT pour Visual Studio 2017 (15.5.0) qui était en préversion est désormais en disponibilité générale.

**Programme d’installation**
1. L’interface utilisateur du programme d’installation est localisée.
1. L’icône est remplacée par une version de qualité supérieure.

**Integration Services (IS)**
1. Ajout d’une étape de validation de package dans l’Assistant Déploiement lors du déploiement sur le runtime d’intégration Azure-SSIS dans ADF, ce qui permet de détecter les éventuels problèmes de compatibilité dans les packages SSIS à exécuter dans le runtime d’intégration Azure-SSIS. Pour plus d’informations, consultez [Valider des packages SSIS déployés sur Azure](../integration-services/lift-shift/ssis-azure-validate-packages.md).
1. L’extension SSIS est localisée.

### <a name="bug-fixes"></a>Résolution des bogues

**Integration Services (IS)**
1. Résolution d’un problème qui entraîne l’endommagement de la disposition du gestionnaire de connexions OLE DB et ADO.NET.
2. Résolution d’un problème qui entraîne l’affichage d’une erreur d’assembly introuvable lors de la tentative de modification d’une tâche de traitement de dimension.

### <a name="known-issues"></a>Problèmes connus

**IS (Integration Services)** La tâche d’exécution de package SSIS ne prend pas en charge le débogage quand ExecuteOutOfProcess a la valeur True. Ce problème s’applique uniquement au débogage. L’enregistrement, le déploiement et l’exécution via DTExec.exe ou le catalogue SSIS ne sont pas impactés.

## <a name="173nbsp-ssdt-for-vs-2015"></a>17.3,&nbsp; SSDT pour VS 2015

_Numéro de build :_ &nbsp; 14.0.61712.050  
_SSDT pour Visual Studio 2015._

### <a name="whats-new"></a>Nouveautés

**Projets AS (Analysis Services)**
- Ajout de trois nouvelles options aux projets tabulaires (sous Options > Analysis Services (tabulaire) > Importation des données) :
  - Activer les sources de données héritées : permet à l’utilisateur de créer des sources de données « mode de compatibilité 1200 » plus anciennes dans des modes de compatibilité plus récents.
  - Automatic type detection (Détection automatique de type) : quand cette option est activée, l’éditeur de requête pour les sources de données modernes tente de détecter les types de données pour les requêtes non structurées lors de leur chargement. Si la détection est réussie, une nouvelle étape peut être ajoutée à la requête.
  - Run background analysis (Exécuter une analyse en arrière-plan) : quand cette option est activée, l’éditeur de requête pour les sources de données modernes exécute des requêtes sur la source de données lors du chargement des requêtes afin d’analyser le schéma de sortie de la requête.

**Integration Services (IS)**
- Ajout d’une étape de validation de package dans l’Assistant Déploiement lors du déploiement sur le runtime d’intégration Azure-SSIS dans ADF, ce qui permet de détecter les éventuels problèmes de compatibilité dans les packages SSIS à exécuter dans le runtime d’intégration Azure-SSIS. Pour plus d’informations, consultez [Valider des packages SSIS déployés sur Azure](../integration-services/lift-shift/ssis-azure-validate-packages.md).

### <a name="bug-fixes"></a>Résolution des bogues

**Projets AS (Analysis Services) :**
- Résolution d’un problème qui pouvait entraîner une exception non gérée lors de l’archivage des modifications du modèle dans TFS.
- Résolution d’un problème qui pouvait entraîner une exception lors de l’ajout d’une table avec une expression M complexe à un modèle 1400.
- Résolution d’un problème qui pouvait entraîner un incident dans Visual Studio lors de la recherche de métadonnées dans la vue de diagramme de modèle.
- Résolution d’un problème avec les modèles 1400 qui pouvait entraîner la suppression des colonnes calculées de la définition de table lors de l’enregistrement des modifications apportées aux requêtes M de partition.
- Résolution d’un problème qui pouvait entraîner un blocage quand vous utilisiez Renommer la requête sur les modèles 1400 dans l’interface utilisateur de l’éditeur d’obtention de données/de table lors de la validation de la compatibilité avec le modèle de données actuel.
- Résolution d’un problème qui a provoqué l’absence d’une référence d’assembly Newtonsoft lors du déploiement de modèle 1400 sur Azure Analysis Service.
- Résolution d’un problème qui a provoqué une erreur d’importation de données via PQ dans un modèle 1400 dans certains cas.
- Résolution d’un problème de mise à l’échelle dans les boîtes de dialogue d’interface utilisateur PowerQuery qui s’affiche quand la mise à l’échelle Windows est définie.
- Résolution d’un problème de renommage des rôles.
- Résolution des problèmes avec les configurations de projet qui peuvent avoir entraîné une synchronisation ou un enregistrement incorrect des modifications dans certains cas.
- Résolution d’un problème dans l’éditeur PowerQuery qui ajoutait automatiquement des étapes du genre « Type de modification ».
- Résolution d’un problème qui a provoqué une erreur d’ouverture du fichier BIM après le passage au mode Espace de travail intégré ou la sortie de ce mode.
- La propriété MaxConnections est à présent visible pour les sources de données dans les modèles tabulaires.
- Augmentation de la taille initiale de la fenêtre de l’éditeur PowerQuery.
- Les mots clés de requêtes M tels que « Source » dans l’éditeur PowerQuery ne s’affichent pas comme étant localisés.
- Mise en cache des informations d’identification lors de l’utilisation des modèles 1400 et des sources de données structurées pour éviter d’avoir à entrer les mêmes informations d’identification pour chaque table modifiée.

**Projets RS :**
- Résolution d’un problème qui empêchait le déploiement d’un rapport unique dans un projet de plusieurs rapports
- Résolution d’un problème qui pouvait se poser avec les sources de données partagées lors du déploiement
- Résolution d’un problème qui pouvait entraîner un incident dans le gestionnaire d’annulation quand vous basculiez entre le mode Code, la vue design et la fenêtre de l’éditeur de requête
- Résolution d’un problème qui peut avoir provoqué la disparition du volet des paramètres après une erreur d’exécution
- Résolution d’un problème avec les projets de rapport qui peut leur avoir fait perdre les mappages de contrôle de code source

**Integration Services :**
- Résolution d’un problème qui s’est peut-être produit lors du remplacement d’une connexion sur une tâche de processus Analysis Services
- Résolution d’un problème où des composants/tâches ne sont pas correctement localisés.
- Résolution d’un problème où des composants CDC s’arrêtent après l’application d’un correctif SQL pour la capture de données modifiées qui ajoute la colonne \__$command\_id.

## <a name="1540-previewnbsp-ssdt-for-vs-2017"></a>15.4.0 (préversion),&nbsp; SSDT pour VS 2017

_Numéro de build :_ &nbsp; 14.0.16134.0  
_SSDT pour Visual Studio 2017._
  
### <a name="whats-new"></a>Nouveautés

Cette version fournit un programme d’installation web autonome pour les projets de base de données SQL Server, Analysis Services, Reporting Services et Integration Services dans Visual Studio 2017 15.4 ou ultérieur.

### <a name="installer"></a>Programme d’installation

- L’utilisateur peut donner un surnom quand il installe une nouvelle instance de SSDT pour VS2017.
- Les cases à cocher de sélection de fonctionnalité du programme d’installation sont masquées si aucune instance de Visual Studio n’est sélectionnée.
- Certains messages du programme d’installation ont été affinés en réponse aux commentaires des clients.
- Un problème empêchant le programme d’installation de prendre en charge la mise à niveau a été résolu.

### <a name="ssis"></a>SSIS

- Résolution d’un problème empêchant l’Assistant Importation/Exportation de répertorier la source de données quand le Feature Pack Azure est installé.
- Résolution d’un problème où la modification d’une tâche de processus SSIS Analysis Services levait une exception au changement de connexion.
- Résolution d’un problème qui entraînait la rupture de composants CDC après l’application du correctif SQL qui ajoute la colonne __$command_id.
- Résolution d’un problème empêchant la modification et l’exécution d’un package tiers si une ancienne version de SQL Server est ciblée.
- Résolution d’un problème empêchant d’afficher correctement la boîte de dialogue de configuration Source du fichier plat quand l’utilisateur double-cliquait sur DTSWizard.exe et sélectionnait Source du fichier plat.
- Résolution d’un problème empêchant l’exécution d’un package contenant un composant/une tâche Feature Pack Azure quand SQL Server 2017 était ciblé.

**Problèmes connus**

- Le programme d’installation n’est pas localisé.
- La tâche d’exécution de package SSIS ne prend pas en charge le débogage quand *ExecuteOutOfProcess* a la valeur True. Ce problème s’applique uniquement au débogage. L’enregistrement, le déploiement et l’exécution via DTExec.exe ou le catalogue SSIS ne sont pas impactés.

## <a name="1730nbsp-ssdt-for-vs-2015"></a>17.30,&nbsp; SSDT pour VS 2015

_Numéro de build :_ &nbsp; 14.0.61709.290  
_SSDT pour Visual Studio 2015._

### <a name="whats-new"></a>Nouveautés

**Analysis Services (AS)**

- Cosmos DB et HDI Spark sont activés dans les modèles 1400.
- Propriétés des sources de données tabulaires.
- L’option « Requête vide » est maintenant disponible quand vous créez une requête dans l’éditeur de requête pour les modèles de niveau de compatibilité 1400.
- L’éditeur de requête pour les modèles 1400 permet désormais d’enregistrer des requêtes sans nouvelles tables traitées automatiquement.

**Reporting Services (RS)**

- Maintenant, les projets affichent à l’ouverture une invite de mise à niveau du format pour prendre en charge les opérations de génération et de déploiement à l’aide de MSBuild.

### <a name="known-issues"></a>Problèmes connus

**Analysis Services (AS)**

- Les modèles de niveau de compatibilité 1400 en mode Direct Query qui ont des perspectives échouent lors de l’interrogation ou la découverte des métadonnées.

**Reporting Services (RS)**

- Le nouveau format de projet de rapport ne conserve pas la liaison du contrôle de code source et génère une erreur semblable à ce message :

   *Le fichier projet C:\path n’est pas lié au contrôle de code source, mais la solution contient des informations de liaison du contrôle de code source.*

   Pour contourner ce problème, cliquez sur **Utiliser la liaison de solution** à chaque ouverture de la solution.

- Après la mise à niveau de votre projet au nouveau format MSBuild, la sauvegarde peut échouer avec un message semblable à celui-ci :

   *« Le paramètre 'unevaluatedValue' ne peut pas être Null. »*

   Pour contourner ce problème, mettez à jour vos *Configurations de projet* et définissez la propriété *Plateforme*.

### <a name="bug-fixes"></a>Correctifs de bogues

**Analysis Services (AS)**

- Nette amélioration des performances lors du chargement de la vue de diagramme du modèle tabulaire.
- Correction de certains problèmes pour améliorer l’intégration et l’utilisation de Power Query dans les modèles de niveau de compatibilité 1400.
   - Correction d’un problème qui empêchait la modification des autorisations pour les sources de fichiers.
   - Correction d’un problème qui empêchait le changement de source de fichiers.
   - Correction d’un problème qui provoquait l’affichage d’une interface utilisateur incorrecte pour les sources de fichiers.
- Correction d’un problème qui provoquait la suppression de la propriété « JoinOnDate » quand une relation « Joindre sur la date » était rendue inactive.
- L’option Nouvelle requête dans le Générateur de requêtes permet maintenant de créer une requête vide.
- Correction d’un problème qui empêchait la mise à jour de la définition du modèle de la table de niveau de compatibilité 1400 quand une requête de source de données existante était modifiée.
- Correction de problèmes liés aux expressions de contexte personnalisé qui pouvaient déclencher des exceptions.
- Quand une nouvelle table est importée avec un nom dupliqué dans des modèles tabulaires 1400, l’utilisateur est maintenant averti qu’un conflit de noms a été détecté et que le nom a été modifié pour être unique.
- Le mode d’emprunt d’identité de l’utilisateur actuel a été supprimé dans les modèles en mode Importation, car ce scénario n’est pas pris en charge.
- L’intégration de Power Query prend maintenant en charge les options pour les sources de données supplémentaires (OData.Feed, Odbc.DataSource, Access.Database, SapBusinessWarehouse.Cubes).
- Les chaînes des options Power Query pour les sources de données affichent maintenant correctement le texte localisé selon les paramètres régionaux du client.
- La vue de diagramme affiche maintenant les colonnes venant d’être créées à partir de l’éditeur de requête M dans les modèles de niveau de compatibilité 1400.
- L’éditeur Power Query comporte à présent une option pour ne pas importer les données.
- Correction d’un problème avec l’installation d’une cartouche de données utilisée pour importer des tables Oracle dans des modèles multidimensionnels dans Visual Studio 2017.
- Correction d’un problème qui pouvait exceptionnellement provoquer un incident quand le curseur de la souris quittait la barre de formule tabulaire.
- Correction d’un problème dans la boîte de dialogue Modifier les propriétés de la table qui entraînait la modification incorrecte du nom de la table source à la suite de la modification du nom de table, ce qui générait une erreur inattendue.
- Correction d’un incident qui pouvait se produire dans Visual Studio 2017 lors d’une tentative d’appel de Tester la sécurité du cube dans le concepteur de l’onglet Données des cellules du concepteur de rôles dans les projets multidimensionnels.
- SSDT : Les propriétés ne sont pas modifiables pour les sources de données tabulaires.
- Correction d’un problème qui pouvait empêcher le bon fonctionnement des builds MSBuild et DevEnv dans certains cas avec des fichiers de solution.
- Nette amélioration des performances au moment de la validation des modifications de modèle (modifications DAX pour les mesures, les colonnes calculées) pour les modèles tabulaires contenant des métadonnées volumineuses.
- Correction de plusieurs problèmes avec l’utilisation de Power Query pour importer des données dans des modèles tabulaires de niveau de compatibilité 1400.
   - L’importation prend beaucoup de temps après que l’utilisateur a cliqué sur Importer, et l’interface utilisateur n’affiche pas d’état.
   - Affichage d’une grande liste de tables dans les vues du navigateur quand vous tentez de sélectionner des tables très longues à importer
   - Diminution des performances de l’éditeur de requête lors de l’utilisation d’une liste de 35 requêtes dans la vue de l’éditeur de requête (problème également rencontré dans Power BI Desktop).
   - L’importation de plusieurs tables entraînait la désactivation de la barre d’outils et ne finissait jamais dans certaines situations. 
   - Le concepteur de modèle apparaissait désactivé et n’affichait pas de données après l’importation d’une table à l’aide de Power Query.
   - Quand l’option « Créer une table » était désélectionnée dans l’interface utilisateur de Power Query, une nouvelle table était quand même créée.
   - La source de données du dossier n’affichait pas d’invite d’informations d’identification. 
   - Une exception « La référence d’objet n’est pas définie » pouvait être déclenchée lors d’une tentative d’obtention des informations d’identification mises à jour sur la source de données structurées.
   - L’ouverture du Gestionnaire de partition avec une expression M était très lente.
   - La sélection de l’option Propriétés de la table dans l’éditeur Power Query n’affichait pas les propriétés.
- Amélioration de la robustesse dans l’intégration de l’interface utilisateur Power Query pour intercepter les exceptions de premier niveau et les afficher dans la fenêtre Sortie.
- Correction d’un problème avec ChangeSource sur la source de données structurées qui ne conservait pas les changements avec l’expression de contexte.
- Correction d’un problème où les erreurs d’expression M pouvaient entraîner des échecs de mise à jour du modèle sans message d’erreur.
- Correction d’un problème à la fermeture de SSDT qui provoquait l’erreur « La génération doit être arrêtée avant la fermeture de la solution ».
- Correction d’un problème où Visual Studio peut sembler ne plus répondre lors de la définition d’un mode d’emprunt d’identité incorrect dans un modèle de niveau de compatibilité 1400. 
- La propriété de lignes de détails est désormais uniquement sérialisée dans JSON quand elle n’est pas vide (changement de la valeur par défaut)
- Le pilote Oracle OLEDB est maintenant proposé dans la liste pour le mode Direct Query tabulaire.
- Les expressions M ajoutées dans les modèles tabulaires de niveau de compatibilité 1400 sont maintenant affichées et actualisées dans l’Explorateur de modèles tabulaires.
- Correction d’un problème qui empêchait l’affichage du fournisseur MSOLAP dans Visual Studio 2017 lors d’une tentative d’importation avec une source de données « Autre » dans des modèles de niveau de compatibilité antérieur à 1400.
- Correction d’un problème où l’ajout d’une translation via l’Explorateur de modèles tabulaires pouvait provoquer des erreurs. 
- Correction d’un problème dans l’interface de la sécurité au niveau des objets qui provoquait l’affichage ou le masquage incorrect de l’onglet dans certains cas.
- Correction d’un problème qui pouvait provoquer l’échec des tentatives d’ouverture d’un modèle multidimensionnel précédemment chargé à l’aide de la boîte de dialogue Connexion à une base de données.
- Correction d’un problème qui provoquait une erreur lors de l’ajout d’assemblys personnalisés à un modèle multidimensionnel.

**Reporting Services (RS)**

- Correction d’un problème avec la compilation et la build de RDLC dans Visual Studio 2017.

## <a name="1530-previewnbsp-ssdt-for-vs-2017"></a>15.3.0 (préversion),&nbsp; SSDT pour VS 2017

_Numéro de build :_ &nbsp; 14.0.16121.0  
_SSDT pour Visual Studio 2017._
  
### <a name="whats-new"></a>Nouveautés

Cette préversion est la première version de SSDT pour Visual Studio 2017. Cette version présente une expérience d’installation web autonome pour les projets de base de données SQL Server, Analysis Services, Reporting Services et Integration Services dans Visual Studio 2017 15.3 ou ultérieur.

**Problèmes connus**

- Le programme d’installation n’est pas localisé.
- SSIS n’est pas localisé.
- La tâche d’exécution de package SSIS ne prend pas en charge le débogage quand *ExecuteOutofProcess* a la valeur *True*. Ce problème s’applique uniquement au débogage. L’enregistrement, le déploiement et l’exécution via DTExec.exe ou le catalogue SSIS ne sont pas impactés.
- Les packages SSIS qui contiennent des extensions tierces ne peuvent pas être intervertis pour cibler d’autres versions de serveur.


## <a name="172nbsp-ssdt-for-vs-2015"></a>17.2,&nbsp; SSDT pour VS 2015

_Numéro de build :_ &nbsp; 14.0.61707.300  
_SSDT pour Visual Studio 2015._

### <a name="whats-new"></a>Nouveautés

**Projets AS :**
- La sécurité au niveau des objets peut maintenant être configurée dans la boîte de dialogue *Rôles* pour la sécurité avancée dans les modèles tabulaires de niveau de compatibilité 1400.
- Une nouvelle sélection de membre du rôle AAD a été ajoutée pour les utilisateurs sans adresse e-mail dans les modèles Azure AS des projets AS SSDT pour VS2017.
- Une nouvelle propriété de projet Azure AS « Toujours demander » a été ajoutée dans les projets tabulaires AS SSDT pour personnaliser le comportement de mise en cache des informations d’identification ADAL.

### <a name="bug-fixes"></a>Correctifs de bogues

**Généralités**
- Mise à jour des références de personnalisation pour SQL Server 2017.

**Projets AS**
- Les performances ont été considérablement corrigées pour améliorer l’expérience durant la validation des changements de mesures DAX et d’autres modifications de modèle.
- Correction de plusieurs problèmes avec l’intégration de Power Query dans les projets Analysis Services utilisant des modèles tabulaires de niveau de compatibilité 1400.
- Correction d’un problème dans les projets multidimensionnels dans VS2017 uniquement, où le concepteur d’agrégation pouvait ne pas se charger.
- Correction d’un problème pendant le glissement d’un élément dans le diagramme DSV multidimensionnel d’Analysis Services qui pouvait bloquer VS 2017.
- Correction d’un problème dans les projets AS où la boîte de dialogue Déployer n’était pas toujours au premier plan dans Visual Studio.
- Suppression de l’importation Analysis Services à partir du marketplace de données comme source de données, car le service a été désactivé.
- Correction d’un problème où le concepteur de tables était désactivé après l’importation d’une nouvelle table d’une source de données existante à partir de l’Explorateur de modèles tabulaires.
- Correction d’un problème susceptible de ne pas permettre l’affichage des éléments du menu Modèle Importer à partir d’une source de données/Ajouter une source de données dans le contexte approprié.
- Amélioration de l’expérience de création d’une mesure dans l’Explorateur de modèles tabulaires pour éviter de ramener le focus sur la colonne utilisée afin de créer une mesure.
- Les anciens fichiers de base de données sont maintenant nettoyés quand vous passez d’un espace de travail intégré dans les projets tabulaires AS à un serveur d’espace de travail explicite.
- Correction d’un problème dans les projets de modèles tabulaires 1400 où l’état de l’interface utilisateur de la case à cocher Sécurité au niveau des lignes était initialement décoché, quel que soit l’état réel de l’objet sous-jacent.
- Correction d’un incident pouvant se produire pendant l’importation d’un fichier texte ou d’un fichier Excel dans un modèle tabulaire en mode de compatibilité 1400 à l’aide de PowerQuery, et où une exception non gérée était levée.
- Correction d’un problème pouvant se produire avec le curseur de défilement du contrôle de modification de la formule DAX dans le concepteur de modèles tabulaires AS.
- Correction d’un problème qui empêchait la modification d’une source de données mashup PowerQuery quand elle contenait une authentification par nom d’utilisateur/mot de passe.
- Correction d’un problème susceptible d’empêcher une source de données de se connecter quand des propriétés supplémentaires étaient définies dans la chaîne de connexion.
- Correction d’un problème qui pouvait bloquer VS lors du chargement de plusieurs projets de modèle tabulaire AS en fermant le second concepteur de modèles alors qu’aucune interaction préalable n’avait eu lieu.
- Correction d’un problème où les modifications apportées à la mise en forme de l’indicateur de performance clé n’étaient pas persistantes dans certains cas.
- Correction d’un problème de l’interface utilisateur de PowerQuery qui affichait un état activé incorrect pour le menu permettant de choisir d’afficher ou non la barre de formule.
- Correction d’un problème dans les projets de modèle tabulaire AS de niveau de compatibilité 1400 avec les sources de données PowerQuery qui pouvait bloquer Visual Studio lors de la sélection du menu Changer la source de données à partir de l’Explorateur de modèles tabulaires.
- Correction d’un problème intermittent où le chargement d’un modèle tabulaire 1400 pouvait afficher l’erreur *Impossible de charger le fichier ou l’assembly ’Microsoft.ProBI.MashupLibrary’* .

**Projets RS**
- Les préférences utilisateur concernant l’état de sélection des paramètres des zones Règle et Paramètre de RS sont correctement mémorisées entre les sessions.

**Projets IS**
- Correction d’un problème où le conteneur ForEachLoop ADO/ADO.NET ne s’affichait pas correctement
- Correction d’un problème où des Assistants/tâches/composants n’étaient pas traduits
- Changement de la dernière *TargetServerVersion* de « SQL Server vNext » en « SQL Server 2017"

## <a name="1710nbsp-ssdt-for-vs-2015"></a>17.10,&nbsp; SSDT pour VS 2015

_Numéro de build :_ &nbsp; 14.0.61705.170  
_SSDT pour Visual Studio 2015._

### <a name="whats-new"></a>Nouveautés
**Projets AS :**
- Les utilisateurs peuvent définir des indications sur le codage des colonnes dans l’interface utilisateur sur les modèles 1400
- IntelliSense non associé à un modèle est désormais disponible en mode hors connexion
- L’Explorateur de modèles tabulaires contient maintenant un nœud pour représenter des expressions M nommées disponibles dans le modèle (modèles tabulaires de niveau de compatibilité 1400)
- Un sélecteur de personnes Azure Active Directory similaire à la gestion des identités et des accès du portail Microsoft Azure est désormais disponible lors de la configuration des membres du rôle dans les modèles tabulaires

**Projets de base de données :**
- Mis à jour vers DacFx 17.1

### <a name="bug-fixes"></a>Correctifs de bogues
- Correction d’un problème où le nom du groupe des concepteurs Business Intelligence était affiché in correctement dans les options de Visual Studio dans VS2017
- Correction d’un problème où un blocage pouvait se produire lors de la génération d’une carte de code pour une solution avec un projet de rapport ou un projet AS
- Correction de plusieurs problèmes avec l’intégration de PowerQuery pour les modèles tabulaires Analysis Services de niveau de compatibilité 1400
- Correction d’un problème dans la fenêtre du nouvel éditeur DAX où l’opérateur d’assignation ne pouvait pas être sur une ligne distincte lors de la définition d’une mesure
- Correction d’un problème qui empêchait la mise à jour de l’affichage des mesures tabulaires lors du renommage des mesures dans une perspective
- Mise à jour du moteur de l’espace de travail intégré d’Analysis Services et du modèle d’objet tabulaire qui corrige une régression provoquant l’échec du déploiement de projets tabulaires 1200 contenant des traductions sur le serveur SQL Server 2016 Analysis Services
- Correction d’un problème de performances qui rendait la création et la suppression de nouvelles sources de données tabulaires 1400 très lentes
- Correction d’un problème où le diagramme de vue de source de données dans les modèles multidimensionnels pouvait cesser l’affichage en cas de changement rapide entre différentes vues de source de données

## <a name="dacfx-171"></a>DacFx 17.1

- Correction d’un problème lors du chiffrement d’une colonne avec des tables optimisées en mémoire avec d’autres colonnes d’identité
- Prise en charge de SQLDOM pour l’option CATALOG_COLLATION pour CREATE DATABASE

## <a name="dacfx-1701"></a>DacFx 17.0.1

- Correction d’un problème pour les bases de données avec une clé asymétrique par un module de sécurité matériel (HSM) avec un fournisseur EKM [Article de Microsoft Connect](https://connect.microsoft.com/SQLServer/feedback/details/3132749/sqlpackage-exe-fails-when-extracting-a-database-which-contains-an-asymmetric-key-using-an-ekm-provider)

## <a name="170nbsp-ssdt-for-vs-2015"></a>17.0,&nbsp; SSDT pour VS 2015

_Numéro de build :_ &nbsp; 14.0.61704.140  
_SSDT pour Visual Studio 2015._  
_Prend en charge jusqu’à SQL Server 2017._

### <a name="whats-new"></a>Nouveautés
**Projets de base de données :**
- La modification d’un index cluster sur une vue ne bloque plus le déploiement.
- Les chaînes de comparaison de schéma relatives au chiffrement des colonnes utilisent désormais le nom approprié au lieu du nom de l’instance.   
- Ajout d’une nouvelle option de ligne de commande à SqlPackage : ModelFilePath.  Ceci permet aux utilisateurs avancés de spécifier un fichier model.xml externe pour les opérations d’importation, de publication et de script.   
- L’API DacFx a été étendue pour prendre en charge l’authentification universelle Azure AD et l’authentification multifacteur

**Projets IS :**
- Le Gestionnaire de connexions OData et de sources OData de SSIS prend maintenant en charge la connexion aux flux OData de Microsoft Dynamics AX Online et Microsoft Dynamics CRM Online.
- Les projets SSIS prennent maintenant en charge la version de serveur cible « SQL Server 2017 ». 
- Prise en charge des tâches de contrôle CDC, du séparateur CDC et des sources CDC lors du ciblage de SQL Server 2017. 

**Projets AS :**
- Intégration de PowerQuery à Analysis Services (modèles tabulaires de niveau de compatibilité 1400) :
    - DirectQuery est disponible pour SQL Oracle et pour Teradata si l’utilisateur a installé les pilotes tiers
    - Ajouter des colonnes par exemple dans PowerQuery
    - Options d’accès aux données dans les modèles 1400 (propriétés de niveau modèle utilisées par le moteur M)
        - Activer la combinaison rapide (la valeur par défaut est false ; quand la valeur est true, le moteur de mashup ignore les niveaux de confidentialité des sources de données lors de la combinaison des données)
        - Activer les redirections héritées (la valeur par défaut est false ; quand la valeur est true, le moteur de mashup suit les redirections HTTP qui sont potentiellement non sécurisées.    Par exemple, une redirection d’un URI HTTPS vers un URI HTTP)  
        - Retourner les valeurs d’erreur comme Null (la valeur par défaut est false ; quand la valeur est true, les erreurs de niveau cellule sont retournées comme Null. Quand la valeur est false, une exception est levée lorsqu’une cellule contient une erreur)  
    - Sources de données supplémentaires (sources de données fichiers) avec PowerQuery
        - Excel 
        - Texte/CSV 
        - Xml 
        - Json 
        - Dossier 
        - Base de données Access 
        - Stockage Blob Azure 
    - Interface utilisateur de PowerQuery localisée
- Fenêtre de l’outil Éditeur DAX
    - Expérience améliorée de l’édition DAX pour les mesures, les colonnes calculées et les expressions de lignes de détails, disponibles via le menu Affichage, Autres fenêtres dans SSDT
    - Améliorations apportées à l’analyseur\Intellisense DAX


**Projets RS :**
- Les contrôles RVC intégrable sont désormais disponibles pour la prise en charge de SSRS 2016

### <a name="bug-fixes"></a>Résolution des bogues
**Projets AS :**
- Correction de la priorité du modèle pour les projets BI pour qu’ils ne s’affichent plus en haut des catégories Nouveaux projets dans VS
- Correction d’un incident VS susceptible de survenir rarement lors de l’ouverture de la solution SSIS, SSAS ou SSRS
- Tabulaire : Améliorations et correctifs de performances pour l’analyse et la barre de formules DAX.
- Tabulaire : L’Explorateur de modèles tabulaires ne sera plus visible si aucun projet tabulaire SSAS n’est ouvert.
- Multidimensionnel: Résolution d’un problème où la boîte de dialogue de traitement était inutilisable sur les machines avec écran haute résolution.
- Tabulaire : Résolution d’un problème d’erreur de SSDT à l’ouverture de projets BI quand SSMS est déjà ouvert. [Article de Microsoft Connect](https://connect.microsoft.com/SQLServer/feedback/details/3100900/ssdt-faults-when-opening-any-bi-project-when-ssms-is-already-open)
- Tabulaire : Résolution d’un problème où les hiérarchies n’étaient pas correctement enregistrées dans le fichier BIM dans un modèle 1103. [Article de Microsoft Connect](https://connect.microsoft.com/SQLServer/feedback/details/3105222/vs-2015-ssdt)
- Tabulaire : Résolution d’un problème où le mode Espace de travail intégré était autorisé sur les ordinateurs 32 bits alors qu’il n’est pas pris en charge.
- Tabulaire : Résolution d’un problème où le fait de cliquer sur quoi que ce soit en mode de semi-sélection (par exemple en tapant une expression DAX mais en cliquant sur une mesure) pouvait provoquer des blocages.
- Tabulaire : Résolution d’un problème où l’Assistant Déploiement réinitialisait la propriété .Name du modèle sur « Model ». [Article de Microsoft Connect](https://connect.microsoft.com/SQLServer/feedback/details/3107018/ssas-deployment-wizard-resets-modelname-to-model)
- Tabulaire : Correction d’un problème où la sélection d’une hiérarchie dans TME affichait des propriétés même si la vue Diagramme n’était pas sélectionnée.
- Tabulaire : Résolution d’un problème où l’opération Coller dans la barre de formule DAX collait les images ou d’autres contenus au lieu du texte lors d’une opération Coller à partir de certaines applications.
- Tabulaire : Résolution d’un problème où certains anciens modèles dans 1103 ne pouvaient pas être ouverts en raison de la présence de mesures avec une définition spécifique.
- Tabulaire : Résolution d’un problème où les sessions XEvent ne pouvaient pas être supprimées.
- Correction d’un problème lié à l’échec de la tentative de génération de fichiers « smproj » avec devenv.com
- Correction d’un problème lié à la finalisation trop fréquente des modifications de texte lors de l’utilisation de l’éditeur IME coréen dans les titres d’onglet de feuille de modèle tabulaire AS
- Correction d’un problème lié au dysfonctionnement de la fonction intellisense pour l’affichage de colonnes d’autres tables
- Amélioration de l’importation de projet tabulaire AS à partir de la boîte de dialogue de base de données par le tri de la liste des bases de données AS
- Correction d’un problème survenant lors de la création de tables calculées dans le modèle tabulaire AS où les tables ne sont pas listées en tant qu’objets suggérés dans l’expression
- Correction d’un problème lié aux modèles AS 1400 d’aperçu lors de leur tentative d’ouverture à l’aide du serveur d’espace de travail intégré après affichage du code
- Correction d’un problème qui empêchait, dans certains cas, le fonctionnement correct de certaines sources de données (sans prise en charge du catalogue initial) 
- L’Assistant de déploiement doit appliquer les modifications aux partitions de tables calculées même lorsque l’option de conservation des partitions est activée
- Correction d’un problème où la boîte de dialogue Propriétés avancées de la connexion AS existante n’affiche pas la liste complète tant qu’elle n’est pas resélectionnée
- Correction de quelques problèmes avec les chaînes tronquées de l’interface utilisateur qui apparaissaient dans certaines versions localisées
- Correction de plusieurs problèmes avec l’intégration de PowerQuery dans les modèles tabulaires Analysis Services de niveau de compatibilité 1400
- Correction d’un problème avec les modèles de style de l’Assistant Rapport qui ne s’affichaient pas correctement
- Correction d’un problème avec l’Assistant Rapport qui pouvait aboutir à des paramètres de source de données incorrects lors du passage de SQL à AS
- Correction d’un problème provoquant l’échec de la génération des projets Analysis Services (tabulaire) à partir de la ligne de commande (devenv.com\exe)
- Correction d’un problème avec l’analyseur de mesure DAX dans l’affichage de la couleur correcte du texte mis en surbrillance lors de la présence de lettres au commencement avant : =
- Correction d’un problème de déclenchement d’une ObjectRefException si les chemins étaient trop longs pour Afficher tous les fichiers pour Projet tabulaire en mode Espace de travail intégré
- Correction d’un problème avec le Concepteur de Source de données pour le fournisseur de données du client Compact 4.0 où il apparaissait indisponible
- Correction d’un problème qui provoquait une erreur lors de la navigation dans un modèle d’exploration de données AS dans VS2017
- Correction d’un problème dans un modèle multidimensionnel AS dans VS2017 où le diagramme de vue de source de données cesse l’affichage après la modification des vues puis lève une exception
- Correction d’un problème dans l’aperçu des rapports avec un échec de connexion à AS dans VS2017
 

**Projets RS :**
- Correction d’un problème lié à la réduction de l’affichage de l’arborescence des paramètres, des sources de données et des jeux de données quand la plupart des modifications ont été effectuées, survenant lors de la conception de rapports dans SSDT 
- Résolution d’un problème où l’opération Enregistrer enregistrait la version de RDL et non pas la version la plus récente.
- Résolution d’un problème où SSDT RS sauvegardait des fichiers alors que la sauvegarde était désactivée et de plusieurs autres problèmes.
- Résolution d’un problème dans le Générateur de rapports où une erreur était affichée quand l’utilisateur cliquait sur « Fractionner les cellules ». [Article de Microsoft Connect](https://connect.microsoft.com/SQLServer/feedback/details/3101818/ssdt-2015-ssrs-designer-error-by-matrix-cell-split)
- Résolution d’un problème où la mise en cache pouvait entraîner des données incorrectes dans un rapport. [Article de Microsoft Connect](https://connect.microsoft.com/SQLServer/feedback/details/3102158/ssdtbi-14-0-60812-report-preview-data-is-frequently-wrong-due-to-bad-caching)

**Projets IS :**
- Résolution d’un problème où le paramètre run64bitruntime n’était pas conservé.
- Résolution d’un problème où DataViewer n’enregistrait pas les colonnes affichées.
- Résolution d’un problème où Parties de package masque les annotations. [Article de Microsoft Connect](https://connect.microsoft.com/SQLServer/feedback/details/3106624/package-parts-hide-annotations)
- Résolution d’un problème où Parties de package ignore les dispositions et les annotations des flux de données. [Article de Microsoft Connect](https://connect.microsoft.com/SQLServer/feedback/details/3109241/package-parts-discard-data-flow-layouts-and-annotations)
- Résolution d’un problème où SSDT se bloque lors de l’importation d’un projet depuis SQL Server.
- Correction d’un problème où la valeur par défaut de TimeoutInMinutes pour les tâches du système de fichiers Hadoop était définie sur 10 après l’ouverture d’un package SSIS enregistré et à l’exécution.

**Projets de base de données :**
- SSDT DACPAC : réinsertion du paramètre de déploiement/ajout pour IgnoreColumnOrder [Article de Microsoft Connect](https://connect.microsoft.com/SQLServer/feedback/details/1221587/ssdt-dacpac-deploy-add-setting-back-in-for-ignorecolumnorder)
- Échec de compilation en cas d’utilisation de STRING_SPLIT [Article de Microsoft Connect](https://connect.microsoft.com/SQLServer/feedback/details/2906200/ssdt-failing-to-compile-if-string-split-is-used)
- Correction du problème lié à l’accès de DeploymentContributors au modèle public, sans que le schéma de sauvegarde ait été initialisé [Problème de GitHub](https://github.com/Microsoft/DACExtensions/issues/8)
- Correctif temporel DacFx pour le positionnement de FILEGROUP
- Correction de l’erreur « Référence non résolue » pour les synonymes externes. 
- Always Encrypted : Le chiffrement en ligne ne désactive pas le suivi des changements lors de l’annulation et ne fonctionne pas correctement si le suivi des changements n’a pas été désactivé avant le démarrage du chiffrement

## <a name="165nbsp-ssdt-for-vs-2015"></a>16.5,&nbsp; SSDT pour VS 2015

_Mise en production :_ &nbsp;20 octobre 2016  
_Numéro de build :_ &nbsp; 14.0.61021.0  
_SSDT pour Visual Studio 2015._  
_Prend en charge jusqu’à SQL Server 2016._

**Nouveautés**

### <a name="connection-improvements"></a>Améliorations des connexions

* La nouvelle zone de recherche de l’onglet **Parcourir** vous permet de filtrer les serveurs locaux, les serveurs réseau et les bases de données SQL Azure. Si ces listes contiennent un grand nombre de serveurs ou de bases de données, cela peut être utile.
* L’onglet **Historique** propose des options de menu contextuel permettant d’épingler/désépingler des favoris, ainsi qu’une nouvelle option pour supprimer des connexions de l’historique.

### <a name="sqlpackage-and-dacfx-api-improvements"></a>Améliorations des API SqlPackage et DacFx

Avec les API SqlPackage.exe et DacFx, vous pouvez maintenant générer un rapport de déploiement et un script de déploiement, puis les publier dans une base de données en une seule opération. Cela fait gagner du temps à toute personne souhaitant conserver un rapport des éléments qui ont été publiés pendant un déploiement. Autre avantage dans les scénarios Azure, des scripts distincts sont créés pour la base de données MASTER et la base de donné cible de déploiement. Jusqu’à maintenant, un seul script était créé ce qui était inutile pour les déploiements répétés.

Deux nouveaux arguments ont été ajoutés aux actions Publier et Script de SqlPackage.

* DeployScriptPath (nom court : dsp). Il s’agit d’un chemin facultatif où écrire le script de déploiement. Dans un déploiement Azure, s’il existe des commandes TSQL pour créer et modifier la base de données, un script principal est écrit dans le même chemin, mais le nom du fichier de sortie est « NomFichier_Master.sql ».
* DeployReportPath (nom court : drp). Il s’agit d’un chemin facultatif où écrire le rapport de déploiement.

Pour l’action Script, vous devez utiliser au choix les arguments de chemin de sortie existants ou les nouveaux arguments pour les scripts/rapports, mais pas les deux.

Exemple d’utilisation :

**Action Publier**

```Sqlpackage.exe /a:Publish /tsn:(localdb)\ProjectsV13 /tdn:MyDatabase /deployscriptpath:"My\DeployScript.sql" /deployreportpath:"My\DeployReport.xml"```

**Action Script**

```Sqlpackage.exe /a:Script /tsn:(localdb)\ProjectsV13 /tdn:MyDatabase /deployscriptpath:"My\DeployScript.sql" /deployreportpath:"My\DeployReport.xml"```

Dans DacFx, deux nouvelles API ont été ajoutées : DacServices.Publish() et DacServices.Script(). Celles-ci prennent également en charge les actions de publication + script + rapport dans une même opération. Exemple d’utilisation :

```
DacServices service = new DacServices(connectionString);
using(DacPackage package = DacPackage.Load(@"C:\My\db.dacpac")) {
var options = new PublishOptions() {
    GenerateDeploymentScript = true, // Should a deployment script be created?
    GenerateDeploymentReport = true, // Should an xml deploy report be created?
    DatabaseScriptPath = @"C:\My\OutputScript.sql", // optional path to save script to
    MasterDbScriptPath = @"C:\My\OutputScript_Master.sql", // optional path to save master script to
    DeployOptions = new DacDeployOptions()
};

// Call publish and receive deployment script & report in the results
PublishResult result = service.Publish(package, "TargetDb", options);
Console.WriteLine(result.DatabaseScript);
Console.WriteLine(result.MasterDbScript);
Console.WriteLine(result.DeploymentReport);

// Call script and receive deployment script & report in results
result = service.Script(package, "TargetDb", options);
Console.WriteLine(result.DatabaseScript);
Console.WriteLine(result.MasterDbScript);
Console.WriteLine(result.DeploymentReport);
```

**Analysis Services & Reporting Services**

Amélioration des performances de l’analyseur DAX du concepteur tabulaire SSAS lors de l’utilisation d’expressions DAX longues.
Pour plus d’informations, lisez le [billet du blog Analysis Services](/archive/blogs/analysisservices/introducing-integrated-workspace-mode-for-sql-server-data-tools-for-analysis-services-tabular-projects-ssdt-tabular).

### <a name="fixed--improved-this-month"></a>Corrections et améliorations du mois

**Outils de base de données**

* [Bogue Connect 3055711](https://connect.microsoft.com/SQLServer/feedback/details/3055711/columns-can't-be-selected-from-cross-apply-openjson-with-explicit-schema) : impossible de sélectionner des colonnes depuis CROSS APPLY OPENJSON avec un schéma explicite
* Correction : Problème avec les index de tables d’historique auto-générés, où DacFx supprimait l’index lors du redéploiement
* Correction : Problème avec l’analyseur de lot DacFx qui n’analysait pas le caractère crochet dans une séquence d’échappement ']', ce qui conduisait à l’échec de la publication
* Amélioration : SqlPackage comprend des descriptions de chaque action dans l’aide
* Correction : L’option « Mémoriser le mot de passe » de la boîte de dialogue de connexion n’était pas conservée lors de la modification des options avancées et d’une chaîne de connexion enregistrée dans Publier, Comparaison de schémas ou d’autres fichiers
* Correction : Pour les connexions affichées dans l’onglet Historique avec IntegratedAuthentication = true, le champ Authentification était vide dans les propriétés de la connexion. Maintenant, il indique « Authentification Windows » comme prévu
* Correction : Les modifications apportées aux paramètres Intellisense des outils SQL Server sous Outils-> Options-> Éditeur de texte n’étaient pas conservées
* Amélioration : Le bouton Épingler/Désépingler dans l’onglet Historique de la boîte de dialogue de connexion est désormais plus compact, ce qui rend moins probable l’affichage d’une barre de défilement
* Correction : Plusieurs problèmes d’accessibilité ont été résolus dans la boîte de dialogue de connexion.

**Analysis Services & Reporting Services**

* Correction d’un problème dans le concepteur tabulaire SSDT AS où le fait de cliquer sur le curseur de la barre de défilement dans la grille de données générait un blocage dans certaines situations
* Correction d’un problème où l’option permettant de se connecter en tant qu’utilisateur actuel dans le concepteur tabulaire SSDT AS n’était pas disponible
* Correction d’un problème dans le concepteur tabulaire SSDT AS où, si vous développiez trop la barre de formule, la réouverture du projet était impossible
* Correction d’un incident dans le concepteur tabulaire SSDT AS qui pouvait se produire quand vous appuyiez sur une touche alors que l’onglet de table était sélectionné
* Correction d’un problème dans les projets SSDT AS où Analyse dans Excel ne se connectait pas aux versions de bas niveau du serveur AS

**Integration Services**

* Correction du bogue Connect [1608896](https://connect.microsoft.com/SQLServer/feedback/details/1608896/move-multiple-integration-service-package-tasks) : déplacer plusieurs tâches du package Integration Services

## <a name="164-ssdt-for-vs-2015"></a>16.4, SSDT pour VS 2015

_Mise en production :_ &nbsp; 20 septembre 2016  
_Numéro de build :_ &nbsp; 14.0.60918  
_Pour SQL Server 2016._

**Nouveautés**

La comparaison de schémas est maintenant prise en charge dans SqlPackage.exe et l’API d’infrastructure d’application de couche Données (DacFx). Pour plus d’informations, consultez [Schema Compare in SqlPackage and the Data-Tier Application Framework](/archive/blogs/ssdt/schema-compare-in-sqlpackage-and-the-data-tier-application-framework-dacfx).

**Analysis Services - Mode Espace de travail intégré pour le modèle tabulaire SSDT (SSAS)**

Le modèle tabulaire SSDT comprend désormais une instance SSAS interne qu’il démarre automatiquement en arrière-plan si le mode Espace de travail intégré est activé afin que vous puissiez ajouter et afficher des tables, des colonnes et des données dans le générateur de modèles sans avoir à fournir une instance de serveur d’espace de travail externe. Le mode d’espace de travail intégré ne modifie pas le fonctionnement du modèle tabulaire SSDT avec une base de données et un serveur d’espace de travail. Le changement réside dans l’emplacement où le modèle tabulaire SSDT héberge la base de données d’espace de travail. Pour activer le mode Espace de travail intégré, sélectionnez l’option Espace de travail intégré dans la boîte de dialogue Générateur de modèles tabulaires qui s’affiche lors de la création d’un projet tabulaire. Pour les projets tabulaires existants qui utilisent un serveur d’espace de travail explicite, vous pouvez basculer en mode Espace de travail intégré en définissant le paramètre Mode Espace de travail intégré avec la valeur True dans la fenêtre Propriétés qui s’affiche lorsque vous sélectionnez le fichier Model.bim dans l’Explorateur de solutions. Pour plus d’informations, consultez le [billet de blog Analysis Services](/archive/blogs/analysisservices/introducing-integrated-workspace-mode-for-sql-server-data-tools-for-analysis-services-tabular-projects-ssdt-tabular).

**Mises à jour et corrections**
**Outils de base de données :**

- [Problème Connect 3087775](https://connect.microsoft.com/SQLServer/feedback/details/3087775) : Tables temporelles rompues dans Visual Studio Data Tools (mise à jour 14.0.60629.0 de juillet) : « La valeur ne peut pas être Null. Nom du paramètre : reportedElement »
- [Problème Connect 1026648](https://connect.microsoft.com/SQLServer/feedback/details/1026648) : IsPersistedNullable est différent dans la comparaison de SSDT
- [Problème Connect 2054735](https://connect.microsoft.com/SQLServer/feedback/details/2054735) : L’identité est réinitialisée lors de l’importation d’un fichier BACPAC
- [Problème Connect 2900167](https://connect.microsoft.com/SQLServer/feedback/details/2900167) : L’exécution de tests unitaires SSDT laisse des fichiers temporaires
- [Problème Connect 1807712](https://connect.microsoft.com/SQLServer/feedback/details/1807712) : Rupture de compatibilité descendante - AppLocal et Nugetization

**Analysis Services & Reporting Services**

* Correction d’un problème dans SSDT où des fenêtres contextuelles de conseil sur les erreurs s’affichaient lors de la modification des colonnes calculées DAX pour DirectQuery.
* Correction d’un problème dans la grille tabulaire SSDT AS où l’icône de KPI ne s’affichait pas dans la grille de mesure quand le facteur d’échelle Windows était défini avec une haute résolution de plus de 200 %.
* Correction d’un problème dans SSDT AS où le collage des données d’une grande table pouvait entraîner le blocage du projet tabulaire.
* Correction d’un problème dans l’éditeur de modèle tabulaire SSDT AS afin de marquer le modèle comme devant enregistrer les modifications lors du renommage d’une connexion.
* Correction d’un problème dans les projets tabulaires SSDT AS où il était impossible de redimensionner la largeur des colonnes dans la boîte de dialogue de gestion des relations.
* Correction d’un problème dans les modèles tabulaires SSDT AS de niveau 1200 où le collage des données depuis Excel avec des paramètres régionaux comme Allemand ne traitait pas correctement la virgule comme séparateur décimal.
* Correction d’un problème dans les projets SSDT AS où des jeux d’icônes de KPI pouvaient générer une erreur « Impossible de récupérer les données de cet élément visuel ».
* Correction d’un problème pour ancrer correctement la boîte de dialogue des propriétés de projet SSDT AS lors du redimensionnement à une résolution élevée.
* Correction d’un problème dans les projets SSDT AS qui peut avoir provoqué une erreur de mise à niveau de certains modèles avec les tables collées.
* Correction d’un problème dans SSDT AS où le collage de lignes de feuille entière à partir d’Excel était lent et créait de nombreuses colonnes inutiles.
* Correction d’un problème dans SSDT AS où l’analyse et la sélection de grandes expressions DataTable statiques étaient lentes ou semblaient ne plus répondre.
* Correction d’un problème dans SSDT AS afin d’ajouter des mesures et des valeurs de KPI à la perspective actuelle sélectionnée dans l’éditeur.
* Correction d’un problème dans SSDT où l’importation de données dans un projet AS depuis SQL Azure ne prenait pas en charge les types de schémas autres que « dbo ».

## <a name="163nbsp-ssdt-for-vs-2015"></a>16.3,&nbsp; SSDT pour VS 2015

_Mise en production :_ &nbsp;15 août 2016  
_Numéro de build :_ &nbsp; 14.0.60812.0  
_Pour SQL Server 2016._

**Nouveautés**

- **Gestion et numérotation des versions :** Les versions sont maintenant nommées avec des chiffres, et non plus avec des mois. Cette initiative s’inscrit dans la nouvelle politique SSMS et simplifie les cas où plusieurs versions ou correctifs sont publiés au cours d’un même mois. Cette version est la 16.3, autrement dit la troisième mise à jour après la publication de la version RTM. Les correctifs seront numérotés 16.3.1 et ainsi de suite jusqu’à notre prochaine mise à jour (prévue le mois prochain) qui sera la version 16.4.
- **Analysis Services - Explorateur de modèles tabulaires :** L’Explorateur de modèles tabulaires vous permet de naviguer facilement dans les différents objets de métadonnées d’un modèle, tels que les sources de données, les tables, les mesures et les relations. Il est implémenté comme fenêtre d’outils distincte que vous pouvez afficher en ouvrant le menu Affichage dans Visual Studio, en pointant sur Autres fenêtres, puis en cliquant sur Explorateur de modèles tabulaires. L’Explorateur de modèles tabulaires s’affiche par défaut dans la zone Explorateur de solutions sous un onglet distinct. L’Explorateur de modèles tabulaires organise les objets de métadonnées dans une arborescence qui ressemble beaucoup au schéma d’un modèle tabulaire 1200 et propose beaucoup d’autres nouvelles fonctionnalités.
- **Outils de base de données - Always Encrypted** :  Cette version propose de nouvelles boîtes de dialogue de [gestion des clés Always Encrypted](../relational-databases/security/encryption/overview-of-key-management-for-always-encrypted.md) permettant de facilement ajouter des clés principales de colonne ou des clés de chiffrement de colonne à votre projet de base de données ou une base de données active dans l’Explorateur d’objets SQL Server. Cette version prend en charge les certificats dans le magasin de certificats Windows. Dans les versions à venir, Azure Key Vault et les fournisseurs CNG seront pris en charge.
    - Quand vous créez une clé principale de colonne ou une clé de chiffrement de colonne, vous pouvez constater que les modifications ne sont pas immédiatement répercutées dans l’Explorateur d’objets SQL Server après avoir cliqué sur Mettre à jour la base de données. Pour contourner ce problème, actualisez le nœud de base de données dans l’Explorateur d’objets SQL Server.
    - Si vous essayez de chiffrer une colonne dans une table contenant des données à partir de l’Explorateur d’objets SQL Server, vous risquez d’échouer. Actuellement, cette fonctionnalité est prise en charge uniquement dans les projets de base de données SSDT et SSMS. La prise en charge de l’Explorateur d’objets SQL Server est prévue dans une version ultérieure.

**Mises à jour et corrections**
* **Outils de base de données :**
    - **SSDT :**
        - Bogue Connect 1898001 [Correction d’un problème de description de colonne avec une limitation à 128 caractères](https://connect.microsoft.com/SQLServer/feedback/details/1898001/column-description-limited-to-128-characters).
        - Correction d’un problème où la publication d’une base de données à partir de Visual Studio n’appliquait pas la propriété DatabaseServiceObjective dans le profil de publication xml.
        - Bogue Connect 2900167 [Correction d’un problème de test unitaire qui laissait des fichiers temporaires](https://connect.microsoft.com/SQLServer/feedback/details/2900167/running-ssdt-unit-tests-leaves-temp-files-behind).
        - Correction d’un problème où la zone de liste modifiable Période de rétention dans les paramètres de base de données était tronquée.
        - Correction d’un problème où un ancien mot de passe vide dans les propriétés de projet SQL CLR n’était pas vérifié lors de la modification du mot de passe.
    - **DACFx :**
        - Correction d’un problème où le niveau de compatibilité de DACFx n’était pas mis à jour pour l’erreur SqlAzureV12.
        - Correction d’un problème où la propriété IsAutoGeneratedHistoryTable était exclue sans raison de la comparaison du modèle.

- **Analysis Services & Reporting Services**
    - **SSDT :**
        - Correction d’un problème où il était impossible d’enregistrer le modèle tabulaire lorsque la connexion au serveur était perdue.
        - Correction d’un problème où SSDT cessait de répondre en raison d’un possible problème de boucle infinie dans AS.
        - Correction d’un problème d’expression DAX qui conduisait à des comportements incohérents selon la façon dont vous validiez l’expression.
        - Correction d’un problème de blocage de VS lors de la création de KPI.
        - Correction d’un problème qui générait des rapports non valides pour SQL Server 2008 R2, 2012 et 2014.
        - Correction d’un problème d’ordre hiérarchique qui générait une erreur de boucle infinie pour un projet .dwpro.
        - Correction d’un problème RDL de RS où le passage à une version antérieure de RDL nécessitait une regénération complète, ce qui entraînait une certaine confusion chez l’utilisateur.
        - Correction d’un problème de KPI où Masquer dans les outils clients n’avait aucun effet.

## <a name="july-2016nbsp-ssdt-for-vs-2015"></a>Juillet 2016,&nbsp; SSDT pour VS 2015

_Mise en production :_ &nbsp; 30 juin 2016  
_Numéro de build :_ &nbsp;14.0.60629.0  
_Pour SQL Server 2016._

**Nouveautés**  
- **Prise en charge d’Always Encrypted :** Pour les bases de données qui contiennent des colonnes Always Encrypted, cette version prend en charge Always Encrypted par le biais de nos API principales et de notre outil en ligne de commande (SqlPackage.exe). Vous pouvez créer et publier des projets de base de données avec prise en charge complète de toutes les fonctionnalités Always Encrypted.  
- **Prise en charge améliorée des tables temporelles :** Expérience simplifiée avec suppression de la liaison des tables temporelles avant les changements, puis rétablissement de la liaison une fois les changements apportés. Cela signifie que les tables temporelles ont une parité avec d’autres types de table (standard, en mémoire) en terme d’opérations prises en charge. 
- **Changements de SqlPackage.exe et de l’installation :** Changements en vue d’isoler SSDT du moteur SQL Server et des mises à jour SSMS. Pour plus d’informations, consultez [Changes to SSDT and SqlPackage.exe installation and updates](/archive/blogs/ssdt/changes-to-ssdt-and-sqlpackage-exe-installation-and-updates).


**Mises à jour et corrections**
* **Outils de base de données :**
    * À partir de maintenant, SSDT ne désactivera jamais TDE (Transparent Data Encryption) sur une base de données. Avant, si l’option de chiffrement par défaut dans les paramètres de base de données d’un projet était désactivée, le chiffrement l’était aussi. Avec ce correctif, le chiffrement peut être activé mais jamais désactivé au cours de la publication. 
    * Augmentation du nombre de tentatives et de la résilience pour les connexions Azure SQL Database au cours de la connexion initiale.
    * Si le groupe de fichiers par défaut n’est pas PRIMARY, Importer/Publier dans Azure V12 échouait. Maintenant ce paramètre est ignoré lors de la publication.
    * Correction d’un problème où lors de l’exportation d’une base de données contenant un objet avec Identificateur entre guillemets activé, la validation de l’exportation pouvait dans certains cas échouer.
    * Correction d’un problème où l’option TEXTIMAGE_ON était ajoutée lors de la création de tables Hekaton alors qu’elle n’était pas autorisée.
    * Correction d’un problème où l’exportation d’une grande quantité de données prenait beaucoup de temps en raison de l’écriture dans le fichier model.xml une fois la phase de données terminée, ce qui provoquait le réécriture du fichier .bacpac.
    * Correction d’un problème où les utilisateurs ne figuraient pas dans le dossier Sécurité des connexions APS et DW Azure SQL.

* **Analysis Services & Reporting Services :**
    * Correction d’un problème SxS avec le fournisseur OLEDB MSOLAP où seul le fournisseur 32 bits était installé, ce qui impactait Excel 2016 64 bits lors de la connexion à SQL Server 2014 (ne se reproduisait pas avec les installations ClickOnce à partir d’Office365, seulement avec l’installation du MSI Excel).
    * Correction d’un problème par le renforcement un « corner case » qui pouvait générer l’erreur « La relation utilise un ID de colonne non valide » pendant le passage du niveau de compatibilité 1103 à 1200 pour un modèle AS avec des tables collées.
    * Correction d’un problème SxS où SSDT-BI 2013, installé sur le même ordinateur, ne pouvait plus importer des données dans le modèle Analysis Services après la désinstallation de SSDT 2015 (paramètre du Registre partagé de cartouches).
    * Amélioration de la robustesse pour résoudre les problèmes/blocages lorsque la connexion au moteur AS est perdue (par exemple, SSDT laissé ouvert pendant la nuit et serveur AS recyclé ou d’autres cas où la connexion est perdue temporairement). 
    * Correction de problèmes liés à l’ouverture de boîtes de dialogue sur des écrans autres que Visual Studio dans le cas d’écrans multiples. 
    * Prise en charge corrigée/activée du collage à partir de tables HTML (données de grille) dans des tables collées du modèle AS. 
    * Correction d’un problème d’échec de mise à niveau d’une table collée vide vers 1200 (utilisée uniquement comme table de conteneur pour les mesures). 
    * Correction d’un problème de mise à niveau d’un modèle tabulaire AS avec des tables collées vers 1200 pour contourner un problème de moteur AS avec des CalcTables (utilisés pour les tables collées dans 1200) et effectuer un processus complet sur les nouvelles tables calculées après la mise à niveau. 
    * Correction d’un problème où l’annulation de la création d’une nouvelle table calculée de modèle AS 1200 avec une expression DAX incomplète pouvait causer un blocage. 
    * Correction d’un problème lors de l’importation du modèle 1200 du serveur AS dans le projet SSDT AS lorsque le nom de base de données et le nom de table étaient identiques. 
    * Correction d’un problème concernant la modification d’une mesure de KPI dans le modèle tabulaire 1103. 
    * Résolution d’une exception Référence d’objet non définie lorsqu’une mesure de KPI est collée dans la grille d’un modèle 1200 Analysis Services. 
    * Correction d’un problème où une colonne d’une table calculée ne pouvait pas être supprimée de la vue de diagramme dans les modèles 1200. 
    * Résolution d’une exception Référence d’objet non définie lors de l’affichage des propriétés du fichier projet model.bim en mode Code. 
    * Correction d’un problème où le collage de données dans la grille de modèle Analysis Services pour créer une table collée générait des valeurs incorrectes dans les paramètres régionaux internationaux utilisant la virgule comme séparateur décimal. 
    * Correction d’un problème lors de l’ouverture d’un projet Reporting Services 2008 dans SSDT en choisissant de ne pas le mettre à niveau. 
    * Correction d’un problème dans l’IU des tables calculées dans les modèles de niveau de compatibilité 1200 en cas d’utilisation de la mise en forme par défaut pour le type de colonne afin d’autoriser le changement du type de mise en forme à partir de l’IU.

## <a name="june-2016nbsp-ssdt-for-vs-2015"></a>Juin 2016,&nbsp; SSDT pour VS 2015

_Mise en production :_ &nbsp; 1er juin 2016  
_Numéro de build :_ &nbsp; 14.0.60525.0  
_Pour SQL Server 2016._

La version SSDT GA (General Availability) a été publiée. La mise à jour SSDT GA de juin 2016 prend en charge les dernières mises à jour de SQL Server 2016 RTM et diverses résolutions de bogues. Pour plus d’informations, consultez [SQL Server Data Tools GA update for June 2016](/archive/blogs/ssdt/sql-server-data-tools-ga-update-for-june-2016).

## <a name="additional-resources"></a>Ressources supplémentaires

- [Télécharger SQL Server Data Tools &#40;SSDT&#41;](../ssdt/download-sql-server-data-tools-ssdt.md)
- [Versions antérieures de SQL Server Data Tools &#40;SSDT et SSDT-BI&#41;](../ssdt/previous-releases-of-sql-server-data-tools-ssdt-and-ssdt-bi.md)
- [Nouveautés du moteur de base de données](../sql-server/what-s-new-in-sql-server-2016.md)
- [Nouveautés d’Analysis Services](/analysis-services/what-s-new-in-analysis-services)
- [Nouveautés d’Integration Services](../integration-services/what-s-new-in-integration-services-in-sql-server-2016.md)
