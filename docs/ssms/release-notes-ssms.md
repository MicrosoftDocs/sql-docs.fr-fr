---
title: Notes de publication de (SSMS)
description: Notes de publication de SQL Server Management Studio (SSMS).
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
ms.assetid: 3dc76cc1-3b4c-4719-8296-f69ec1b476f9
author: dzsquared
ms.author: drskwier
ms.reviewer: maghan
ms.custom: seo-lt-2019
ms.date: 12/17/2020
ms.openlocfilehash: 346e964b30f76bf79c20d6f348636a626a8beee2
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/17/2020
ms.locfileid: "97642371"
---
# <a name="release-notes-for-sql-server-management-studio-ssms"></a>Notes de publication de SQL Server Management Studio (SSMS)

[!INCLUDE [SQL Server ASDB, ASDBMI, ASDW ](../includes/applies-to-version/sql-asdb-asdbmi-asa.md)]

Cet article fournit des détails sur les mises à jour, les améliorations et les correctifs de bogues des versions actuelles et précédentes de SSMS.

## <a name="current-ssms-release"></a>Version actuelle de SSMS

### <a name="188"></a>18,8

![télécharger](media/download-icon.png) [Télécharger SSMS 18.8](download-sql-server-management-studio-ssms.md)

- Numéro de version : 18,8
- Numéro de build : 15.0.18369.0
- Date de publication : 17 décembre 2020

[Chinois (simplifié)](https://go.microsoft.com/fwlink/?linkid=2151644&clcid=0x804) | [Chinois (traditionnel)](https://go.microsoft.com/fwlink/?linkid=2151644&clcid=0x404) | [Anglais (États-Unis)](https://go.microsoft.com/fwlink/?linkid=2151644&clcid=0x409) | [Français](https://go.microsoft.com/fwlink/?linkid=2151644&clcid=0x40c) | [Allemand](https://go.microsoft.com/fwlink/?linkid=2151644&clcid=0x407) | [Italien](https://go.microsoft.com/fwlink/?linkid=2151644&clcid=0x410) | [Japonais](https://go.microsoft.com/fwlink/?linkid=2151644&clcid=0x411) | [Coréen](https://go.microsoft.com/fwlink/?linkid=2151644&clcid=0x412) | [Portugais (Brésil)](https://go.microsoft.com/fwlink/?linkid=2151644&clcid=0x416) | [Russe](https://go.microsoft.com/fwlink/?linkid=2151644&clcid=0x419) | [Espagnol](https://go.microsoft.com/fwlink/?linkid=2151644&clcid=0x40a)

SSMS 18.8 est la dernière version en disponibilité générale de SSMS. Si vous avez besoin d’une version précédente de SSMS, consultez les [versions précédentes de SSMS](release-notes-ssms.md#previous-ssms-releases).

#### <a name="whats-new-in-188"></a>Nouveautés de la version 18.8

[!INCLUDE [ssms-ads-install](../includes/ssms-azure-data-studio-install.md)]

| Nouvel élément | Détails |
|----------|---------|
| Intégration de l’installation Azure Data Studio | L’installation de SSMS installe Azure Data Studio 1.25.1. |
| Analysis Services | Ajout de la prise en charge des espaces de travail Power BI Premium Gen2. |
| Audit | Ajout de la prise en charge d’EXTERNAL_MONITOR et de l’« audit d’opérateur ».  Pour plus d’informations, consultez la [documentation](https://docs.microsoft.com/azure/azure-sql/managed-instance/auditing-configure) relative à la configuration. |
| Integration Services | Amélioration de l’Assistant Création de runtime d’intégration. Désormais, la création de SSISDB est facultative lors de la création du runtime d’intégration SSIS |

#### <a name="bug-fixes-in-188"></a>Correctifs de bogues dans la version 18.8

| Nouvel élément | Détails |
|----------|---------|
| Accessibilité | Résolution des problèmes affectant Narrator dans la boîte de dialogue ProgressReport |
| Moniteur d'activité | Résolution d’un problème où le menu contextuel « Modifier le texte de la requête » dans la grille « Requêtes coûteuses actives » ne fonctionnait pas |
| Always Encrypted | Correction d’un problème où SSMS pouvait générer une erreur d’exécution (« Classe introuvable ») |
| Résolutions élevées/mise à l’échelle | Amélioration de l’affichage 4k de la boîte de dialogue ProgressReport |
| Résolutions élevées/mise à l’échelle | Résolution du problème dans le formulaire « Nouveau calendrier - Package » dans Integration Services |
| SSMS général |  Correction d’un problème où SSMS pouvait générer une erreur lors de la tentative d’affichage des propriétés d’une colonne qui a une valeur initiale d’identité ou un incrément d’identité qui dépasse la taille d’un bigint.
| SSMS général | Correction d’un incident dans SSMS provoqué par un élément de menu « Modèles d’exportation de projets... » qui n’était pas conçu pour être affiché |
| SSMS général | Résolution d’un problème qui pouvait provoquer le blocage de SSMS lors de l’énumération des sauvegardes dans un dossier |
| SSMS général | Résolution d’un problème qui provoquait des fuites des descripteurs et de la mémoire par SSMS lors de l’affichage des résultats des requêtes |
| SSMS général | Résolution d’un problème où les utilisateurs n’étaient pas en mesure de créer un index ColumnStore en cluster pour des colonnes de types particuliers (UniqueIdentifier sur SQL 2014 et versions ultérieures, VarCharMax/NVarCharMax/VarBinaryMax sur SQL 2017 et versions ultérieures) |
| SSMS général | Correction d’un problème longtemps resté en suspens où la réorganisation des colonnes XML dans la « Grille de résultats » ne fonctionnait pas. Voir [Commentaires des utilisateurs sur SQL Server](https://feedback.azure.com/forums/908035/suggestions/32890930) |
| SSMS général | Résolution d’un problème où SSMS affichait de manière incorrecte un avertissement indiquant que « le suivi des modifications n’est pas pris en charge pour une base de données avec des objets mémoire optimisés » |
| Assistant Générer des scripts | Correction d’un bogue qui empêchait l’énumération des objets de base de données sur SQL Server 2014 et versions antérieures. Voir [Commentaires des utilisateurs sur SQL Server](https://feedback.azure.com/forums/908035-sql-server/suggestions/41885587). |
| Outils de réplication | Correction d’un problème où le moniteur de réplication SQL ne respectait pas les paramètres de connexion automatique, forçant ainsi l’utilisateur à se reconnecter manuellement à chaque fois. Voir [Commentaires des utilisateurs sur SQL Server](https://feedback.azure.com/forums/908035/suggestions/40394743) |
| SMO/Création de scripts | Résolution d’un problème lors de la tentative d’utilisation de « Générer un script... » sur une vue chiffrée dans SQL Azure DB |
| Interface utilisateur XEvent | Correction d’un problème qui pouvait provoquer le blocage de SSMS lors du passage d’autres applications à SSMS. |
| Interface utilisateur XEvent | Retrait de l’amélioration de la version 18.7 qui provoquait un bogue dans la création de XEvent en raison de guillemets supplémentaires. *Cette modification est temporaire et l’amélioration reviendra dans une version ultérieure sans le bogue qui l’accompagne.* |

#### <a name="known-issues-188"></a>Problèmes connus (18.8)

| Nouvel élément | Détails | Solution de contournement |
|----------|---------|------------|
| Analysis Services | Erreur lors de la connexion à SSAS via msmdpump.dll. Voir [Commentaires des utilisateurs sur SQL Server](https://feedback.azure.com/forums/908035-sql-server/suggestions/40144696). | N/A |
| Analysis Services | Dans de rares cas, lors de l’utilisation de la mise à niveau, il peut y avoir un message d’erreur « L’objet n’est pas défini sur l’instance d’un objet » lors d’une tentative d’ouverture de l’éditeur DAX après la mise à niveau de SSMS. | Désinstallation et réinstallation de SQL Server Management Studio.  Si le problème n’est pas résolu par la réinstallation, fermez toutes les instances de SSMS, sauvegardez, puis supprimez `%AppData%\Microsoft\SQL Server Management Studio` et `%LocalAppData%\Microsoft\SQL Server Management Studio`. |
| SSMS général | La boîte de dialogue Nouvelle spécification de l’audit du serveur peut provoquer le blocage de SSMS avec une erreur de violation d’accès. | N/A |
| SSMS général | Les extensions SSMS utilisant SMO doivent être recompilées pour cibler le nouveau package SMO v161 spécifique à SSMS. Une préversion est disponible à l’adresse https://www.nuget.org/packages/Microsoft.SqlServer.SqlManagementObjects.SSMS/ </br></br> Les extensions compilées avec des versions 160 antérieures du package Microsoft.SqlServer.SqlManagementObjects continueront de fonctionner. | N/A |
| Integration Services | Lorsque vous importez ou exportez des packages dans Integration Services ou exportez des packages dans Azure-SSIS Integration Runtime, des scripts sont perdus pour les packages contenant des tâches/composants de script. | Supprimer le dossier « C:\Program Files (x86)\Microsoft SQL Server Management Studio 18\Common7\IDE\CommonExtensions\MSBuild ». |
| Integration Services | Les connexions à distance à Integration Services peuvent échouer avec le message « Le service spécifié n’existe pas en tant que service installé. » sur les systèmes d’exploitation récents. | Identifiez l’emplacement du Registre associé aux services d'intégration sous Computer\HKEY_CLASSES_ROOT\AppID et Computer\HKEY_CLASSES_ROOT\ WOW6432Node\AppID et dans ces ruches, renommez la clé de Registre « LocalService » en « LocalService_A » pour la version spécifique des services d’intégration que vous essayez de connecter |


Pour connaître les autres problèmes connus et pour envoyer vos commentaires à l’équipe produit, accédez à [Commentaires des utilisateurs sur SQL Server](https://feedback.azure.com/forums/908035-sql-server).

## <a name="previous-ssms-releases"></a>Versions précédentes de SSMS

[!INCLUDE[ssms-connect-aazure-ad](../includes/ssms-connect-azure-ad.md)]

Téléchargez les versions précédentes de SSMS en sélectionnant le lien de téléchargement de la section associée.

| Version de SSMS | Numéro de build | Date de publication |
|--------------|--------------|--------------|
| [18.7.1](#1871) | 15.0.18358.0 | 27 octobre 2020 |
| [18,7](#187) | 15.0.18357.0 | 20 octobre 2020 |
| [18.6](#186) | 15.0.18338.0 | 22 juillet 2020 |
| [18.5.1](#1851) | 15.0.18333.0 | 9 juin 2020 |
| [18.5](#185) | 15.0.18330.0 | 7 avril 2020 |
| [18.4](#184) | 15.0.18206.0 | 4 novembre 2019 |
| [18.3.1](#1831) | 15.0.18183.0 | 2 octobre 2019 |
| [18.2](#182) | 15.0.18142.0 | 25 juillet 2019 |
| [18.1](#181) | 15.0.18131.0 | 11 juin 2019 |
| [18.0](#180) | 15.0.18118.0 | 24 avril 2019 |
| [17.9.1](#1791) | 14.0.17289.0 | 21 novembre 2018 |
| [16.5.3](#1653) | 13.0.16106.4 | 30 janvier 2017 |

### <a name="1871"></a>18.7.1

![télécharger](media/download-icon.png) [Télécharger SSMS 18.7](https://go.microsoft.com/fwlink/?linkid=2147207)

- Numéro de version : 18.7.1
- Numéro de build : 15.0.18358.0
- Date de publication : 27 octobre 2020

[Chinois (simplifié)](https://go.microsoft.com/fwlink/?linkid=2147207&clcid=0x804) | [Chinois (traditionnel)](https://go.microsoft.com/fwlink/?linkid=2147207&clcid=0x404) | [Anglais (États-Unis)](https://go.microsoft.com/fwlink/?linkid=2147207&clcid=0x409) | [Français](https://go.microsoft.com/fwlink/?linkid=2147207&clcid=0x40c) | [Allemand](https://go.microsoft.com/fwlink/?linkid=2147207&clcid=0x407) | [Italien](https://go.microsoft.com/fwlink/?linkid=2147207&clcid=0x410) | [Japonais](https://go.microsoft.com/fwlink/?linkid=2147207&clcid=0x411) | [Coréen](https://go.microsoft.com/fwlink/?linkid=2147207&clcid=0x412) | [Portugais (Brésil)](https://go.microsoft.com/fwlink/?linkid=2147207&clcid=0x416) | [Russe](https://go.microsoft.com/fwlink/?linkid=2147207&clcid=0x419) | [Espagnol](https://go.microsoft.com/fwlink/?linkid=2147207&clcid=0x40a)

SSMS 18.7 est la dernière version en disponibilité générale de SSMS. Si vous avez besoin d’une version précédente de SSMS, consultez les [versions précédentes de SSMS](release-notes-ssms.md#previous-ssms-releases).

#### <a name="whats-new-in-1871"></a>Nouveautés de la version 18.7.1

[!INCLUDE [ssms-ads-install](../includes/ssms-azure-data-studio-install.md)]


#### <a name="bug-fixes-in-1871"></a>Correctifs de bogues dans la version 18.7.1

| Nouvel élément | Détails |
|----------|---------|
| Magasin des requêtes | Correction de l’erreur qui était levée lorsque vous cliquiez avec le bouton droit sur le nœud Explorateur d’objets dans Magasin des requêtes. |


#### <a name="known-issues-1871"></a>Problèmes connus (18.7.1)

| Nouvel élément | Détails | Solution de contournement |
|----------|---------|------------|
| Analysis Services | Erreur lors de la connexion à SSAS via msmdpump.dll. Voir [Commentaires des utilisateurs sur SQL Server](https://feedback.azure.com/forums/908035-sql-server/suggestions/40144696). | N/A |
| Analysis Services | Dans de rares cas, lors de l’utilisation de la mise à niveau, il peut y avoir un message d’erreur « L’objet n’est pas défini sur l’instance d’un objet » lors d’une tentative d’ouverture de l’éditeur DAX après la mise à niveau de SSMS. | Pour résoudre ce problème, désinstallez, puis réinstallez SSMS. |
| Éditeur de requête MDX | L’ouverture de l’éditeur de requête DAX provoque l’erreur « l’objet n’est pas défini sur l’instance d’un objet » | Désinstallation et réinstallation de SQL Server Management Studio.  Si le problème n’est pas résolu par la réinstallation, fermez toutes les instances de SSMS, sauvegardez, puis supprimez `%AppData%\Microsoft\SQL Server Management Studio` et `%LocalAppData%\Microsoft\SQL Server Management Studio`. |
| SSMS général | La boîte de dialogue Nouvelle spécification de l’audit du serveur peut provoquer le blocage de SSMS avec une erreur de violation d’accès. | N/A |
| SSMS général | Les extensions SSMS utilisant SMO doivent être recompilées pour cibler le nouveau package SMO v161 spécifique à SSMS. Une préversion est disponible à l’adresse https://www.nuget.org/packages/Microsoft.SqlServer.SqlManagementObjects.SSMS/ </br></br> Les extensions compilées avec des versions 160 antérieures du package Microsoft.SqlServer.SqlManagementObjects continueront de fonctionner. | N/A |
| Assistant Générer des scripts | L’Assistant échoue lors de la tentative d’énumération des objets de base de données sur SQL Server 2014 et versions antérieures. Voir [Commentaires des utilisateurs sur SQL Server](https://feedback.azure.com/forums/908035-sql-server/suggestions/41885587). | Utilisez SSMS 18.6 pour sélectionner des objets dans l’Assistant Génération de scripts pour SQL 2014 et versions antérieures. |
| Integration Services | Lorsque vous importez ou exportez des packages dans Integration Services ou exportez des packages dans Azure-SSIS Integration Runtime, des scripts sont perdus pour les packages contenant des tâches/composants de script. | Supprimer le dossier « C:\Program Files (x86)\Microsoft SQL Server Management Studio 18\Common7\IDE\CommonExtensions\MSBuild ». |
| Integration Services | Les connexions à distance à Integration Services peuvent échouer avec le message « Le service spécifié n’existe pas en tant que service installé. » sur les systèmes d’exploitation récents. | Identifiez l’emplacement du Registre associé aux services d'intégration sous Computer\HKEY_CLASSES_ROOT\AppID et Computer\HKEY_CLASSES_ROOT\ WOW6432Node\AppID et dans ces ruches, renommez la clé de Registre « LocalService » en « LocalService_A » pour la version spécifique des services d’intégration que vous essayez de connecter |
| Explorateur d’objets | Les versions de SSMS antérieures à 18.7 ont un changement cassant dans l’Explorateur d’objets en raison des modifications du moteur relatives au [pool SQL serverless Azure Synapse Analytics](https://docs.microsoft.com/azure/synapse-analytics/sql/on-demand-workspace-overview). | Pour continuer à utiliser l’Explorateur d’objets dans SSMS avec le pool SQL serverless Azure Synapse Analytics, vous avez besoin de SSMS 18.7 ou version ultérieure. |

### <a name="187"></a>18,7

![télécharger](media/download-icon.png) [Télécharger SSMS 18.7](https://go.microsoft.com/fwlink/?linkid=2146265)

- Numéro de version : 18.7
- Numéro de build : 15.0.18357.0
- Date de publication : 20 octobre 2020

[Chinois (simplifié)](https://go.microsoft.com/fwlink/?linkid=2146265&clcid=0x804) | [Chinois (traditionnel)](https://go.microsoft.com/fwlink/?linkid=2146265&clcid=0x404) | [Anglais (États-Unis)](https://go.microsoft.com/fwlink/?linkid=2146265&clcid=0x409) | [Français](https://go.microsoft.com/fwlink/?linkid=2146265&clcid=0x40c) | [Allemand](https://go.microsoft.com/fwlink/?linkid=2146265&clcid=0x407) | [Italien](https://go.microsoft.com/fwlink/?linkid=2146265&clcid=0x410) | [Japonais](https://go.microsoft.com/fwlink/?linkid=2146265&clcid=0x411) | [Coréen](https://go.microsoft.com/fwlink/?linkid=2146265&clcid=0x412) | [Portugais (Brésil)](https://go.microsoft.com/fwlink/?linkid=2146265&clcid=0x416) | [Russe](https://go.microsoft.com/fwlink/?linkid=2146265&clcid=0x419) | [Espagnol](https://go.microsoft.com/fwlink/?linkid=2146265&clcid=0x40a)

SSMS 18.7 est la dernière version en disponibilité générale de SSMS. Si vous avez besoin d’une version précédente de SSMS, consultez les [versions précédentes de SSMS](release-notes-ssms.md#previous-ssms-releases).

#### <a name="whats-new-in-187"></a>Nouveautés de la version 18.7

[!INCLUDE [ssms-ads-install](../includes/ssms-azure-data-studio-install.md)]

| Nouvel élément | Détails |
|----------|---------|
| Intégration de l’installation Azure Data Studio | L’installation de SSMS installe également Azure Data Studio. |
| Always Encrypted | Pour reconnaître les nouveaux points de terminaison HSM, vous devez mettre à niveau SSMS. Pour ce faire, utilisez le nouveau fournisseur AKV NugetPackage. |
| Importer un fichier plat | Amélioration de la prédiction des types de données grâce à l’apprentissage sur 300 lignes par défaut. |
| Importer un fichier plat | Empêchez les colonnes d’être déclarées en tant que TinyInt qui devrait être SmallInt. |
| Importer un fichier plat | Amélioration dans laquelle les tables DW sont correctement nettoyées, en cas de défaillance de l’importation des données. |
| gouverneur de ressources | Support ajouté pour des valeurs décimales. |
| ShowPlan | Opérateur PREDICT ajouté. |
| Interface utilisateur XEvent | Ajout de la fonctionnalité de script des événements étendus à l’aide du nom de wait_type. Les utilisateurs doivent utiliser la valeur de colonne map_value au lieu de map_key dans le prédicat de filtre wait_type, car la valeur de clé est soumise à modification lors de la mise à niveau de la version. Correctif : Ajout d’une case à cocher et de l’option données aux utilisateurs, pour choisir map_value ou map_key pour la valeur de prédicat de filtre wait_type. |

#### <a name="bug-fixes-in-187"></a>Correctifs de bogues dans la version 18.7

| Nouvel élément | Détails |
|----------|---------|
| Accessibilité | Correction de l’ordre de tabulation des boutons dans la fenêtre « Les paramètres suivants ne sont pas pris en charge par la base de données » qui s’affiche lors de l’interrogation de DW. |
| Accessibilité | Assistant importation et exportation : la mise en page est incorrecte en mode ppp. |
| Moniteur d'activité | Correction d’un problème dans lequel le moniteur d’activité était en cours de suspension lors de l’ouverture de l’onglet « Processus ». Consultez [Commentaires des utilisateurs sur SQL Server](https://feedback.azure.com/forums/908035/suggestions/37050118). |
| Groupe de disponibilité Always On | Correction d’un problème où le basculement du groupe de disponibilité de l’échelle lecture ne fonctionnait pas. |
| Analysis Services | Mise à jour des composants PowerQuery vers 2.84.982 pour les scénarios de modèles tabulaires AS. |
| Analysis Services | Correction d’un problème qui a pu générer une erreur lors de la tentative de connexion à SSAS via msmdpump.dll. Consultez [Commentaires des utilisateurs sur SQL Server](https://feedback.azure.com/forums/908035-sql-server/suggestions/40144696). |
| Sauvegarde/restauration | Correction d’un problème où la sélection de l’option « Afficher les propriétés de connexion » générait une erreur SMO sur l’accessoire HostDistribution manquant pour SQL 2016 et versions antérieures. |
| Concepteur de bases de données | Correction d’un problème qui provoquait un incident SSMS lors du traitement des nombres décimaux. |
| Diagrammes de base de données | Correction d’un problème dans lequel SSMS se plantait ou cessait de répondre lors de l’utilisation de diagrammes de base de données où la boîte de dialogue « Ajouter une table » n’était pas été correctement affichée. |
| Mise en miroir de bases de données | Correction d’un problème qui provoquait l’échec de la configuration du miroir. Consultez [Commentaires des utilisateurs sur SQL Server](https://feedback.azure.com/forums/908035-sql-server/suggestions/32897281). |
| SSMS général | Correction d’un problème lié à la tentative de connexion à une base de données SQL Azure, qui peut prendre plusieurs secondes (connexion SQL dans une base de données utilisateur). |
| SSMS général | Correction d’un problème où SSMS ne gère/n’affiche pas les blocages capturés (fichiers .xdl). |
| SSMS général | Correction d’un problème où la tentative d’ouverture des paramètres du journal des erreurs pour SQL Server 2008 R2 et versions antérieures a échoué avec la propriété ErrorLogSizeKb introuvable. |
| SSMS général | Les correctifs généraux et les améliorations apportées au support [pool SQL serverless Azure Synapse Analytics](https://docs.microsoft.com/azure/synapse-analytics/sql/on-demand-workspace-overview). |
| Importer un fichier plat | Correction d’un problème où l’Assistant n’a pas détecté que le fichier était en cours d’utilisation par une autre application et provoquait à la place une erreur. Consultez [Commentaires des utilisateurs sur SQL Server](https://feedback.azure.com/forums/908035/suggestions/40761574). |
| Application de la couche Données d’importation/exportation | Correction du niveau de service par défaut en mode S0 Standard lors de l’importation d’un bacpac (identique au comportement Portail Azure et SqlPackage.exe). |
| Importer un fichier plat | Correction d’un problème où l’Assistant n’a pas détecté que le fichier était en cours d’utilisation par une autre application et provoquait à la place une erreur. Consultez [Commentaires des utilisateurs sur SQL Server](https://feedback.azure.com/forums/908035/suggestions/40761574). |
| Integration Services | Correction d’un problème dans lequel les éléments de Zone de liste modifiable d’abonnement Azure sont dupliqués dans l’Assistant de création IR et l’Assistant Migration de travail lorsque des abonnements différents portent le même nom. |
| Integration Services | Correction d’un problème qui entraînait parfois l’impossibilité d’activer le bouton Connexion dans l’Assistant Création IR. |
| Integration Services | Correction d’un problème dans lequel les éléments de la Zone de liste modifiable « Utiliser la variable d’environnement » dans la boîte de dialogue « Définir la valeur du paramètre » ne sont pas dans l’ordre. |
| Intellisense | Correction d’un incident dans SSMS qui pouvait se produire lors de l’exécution d’une requête. |
| Intellisense | Correction d’un problème où IntelliSense ne fonctionne pas lorsque l’utilisateur est connecté à un Groupe de disponibilité configuré pour le routage en lecture seule. |
| Serveurs liés | Correction d’un problème où un utilisateur avec l’autorisation CONTROL SERVER (mais pas avec le rôle sysadmin) n’était pas en mesure d’ajouter un serveur lié. |
| Visionneuse du journal | Correction d’un problème où un utilisateur disposant des autorisations VIEW SERVER STATE n’était pas en mesure d’afficher les journaux d’erreurs SQL Server. Consultez [Commentaires des utilisateurs sur SQL Server](https://feedback.azure.com/forums/908035/suggestions/32899204). |
| Explorateur d’objets | Correction d’un problème où le fait de sélectionner le menu **Démarrer PowerShell** sur certains nœuds de l’Explorateur d’objets (comme « gestion des stratégies », « Événements étendus ») risque de ne pas démarrer correctement PowerShell. |
| Serveurs inscrits | Correction d’un problème où SSMS est tombé en panne lors de la tentative d’inscription d’un serveur de gestion centralisée. |
| Serveurs inscrits | Correction du problème où les éléments de menu pour lancer Azure Data Studio à partir de serveurs inscrits étaient manquants. |
| Rapports | Correction d’un problème où, sur le tableau de bord Performances, vous essayez d’accéder à des sous-liens (par exemple, **Requêtes coûteuses**) mais cela ne fonctionne pas. Ce problème était courant dans la plupart des versions non anglaises de SSMS. Consultez [Commentaires des utilisateurs sur SQL Server](https://feedback.azure.com/forums/908035/suggestions/41454499). |
| ShowPlan | Correction d’un problème provoquant l’incident SSMS lors de l’utilisation Rechercher le nœud pour rechercher du texte. Consultez [Commentaires des utilisateurs sur SQL Server](https://feedback.azure.com/forums/908035/suggestions/40421650). |
| ShowPlan | Ajouter le suffixe KB à la ligne d’info-bulle de l’allocation de mémoire |
| Évaluation des vulnérabilités | Correction d’un problème qui provoquait la levée d’une erreur par SSMS lors de la tentative de définition de lignes de base dans l’évaluation des vulnérabilités. Consultez [Commentaires des utilisateurs sur SQL Server](https://feedback.azure.com/forums/908035/suggestions/40578565). |
| Interface utilisateur XEvent | Correction du problème où la touche F1 n’est pas arrivée sur la page appropriée sur DOCS. |
| Interface utilisateur XEvent | Correction du journal dans la visionneuse XEvent où l’info-bulle n’affichait pas correctement le texte contenant du texte encodé à l’aide de paires de substitution. |

#### <a name="known-issues-187"></a>Problèmes connus (18.7)

| Nouvel élément | Détails | Solution de contournement |
|----------|---------|------------|
| Analysis Services | Erreur lors de la connexion à SSAS via msmdpump.dll. Voir [Commentaires des utilisateurs sur SQL Server](https://feedback.azure.com/forums/908035-sql-server/suggestions/40144696). | N/A |
| Analysis Services | Dans de rares cas, lors de l’utilisation de la mise à niveau, il peut y avoir un message d’erreur « L’objet n’est pas défini sur l’instance d’un objet » lors d’une tentative d’ouverture de l’éditeur DAX après la mise à niveau de SSMS. | Pour résoudre ce problème, désinstallez, puis réinstallez SSMS. |
| SSMS général | La boîte de dialogue Nouvelle spécification de l’audit du serveur peut provoquer le blocage de SSMS avec une erreur de violation d’accès. | N/A |
| SSMS général | Les extensions SSMS utilisant SMO doivent être recompilées pour cibler le nouveau package SMO v161 spécifique à SSMS. Une préversion est disponible à l’adresse https://www.nuget.org/packages/Microsoft.SqlServer.SqlManagementObjects.SSMS/ </br></br> Les extensions compilées avec des versions 160 antérieures du package Microsoft.SqlServer.SqlManagementObjects continueront de fonctionner. | N/A |
| Assistant Générer des scripts | L’Assistant échoue lors de la tentative d’énumération des objets de base de données sur SQL Server 2014 et versions antérieures. Voir [Commentaires des utilisateurs sur SQL Server](https://feedback.azure.com/forums/908035-sql-server/suggestions/41885587). | Utilisez SSMS 18.6 pour sélectionner des objets dans l’Assistant Génération de scripts pour SQL 2014 et versions antérieures. |
| Integration Services | Lorsque vous importez ou exportez des packages dans Integration Services ou exportez des packages dans Azure-SSIS Integration Runtime, des scripts sont perdus pour les packages contenant des tâches/composants de script. Solution de contournement : Supprimer le dossier « C:\Program Files (x86)\Microsoft SQL Server Management Studio 18\Common7\IDE\CommonExtensions\MSBuild ». | N/A |
| Integration Services | Les connexions à distance à Integration Services peuvent échouer avec le message « Le service spécifié n’existe pas en tant que service installé. » sur les systèmes d’exploitation récents. Solution de contournement : Identifiez l’emplacement du Registre associé aux services d'intégration sous Computer\HKEY_CLASSES_ROOT\AppID et Computer\HKEY_CLASSES_ROOT\ WOW6432Node\AppID et dans ces ruches, renommez la clé de Registre « LocalService » en « LocalService_A » pour la version spécifique des services d’intégration que vous essayez de connecter | N/A |
| Explorateur d’objets | Les versions de SSMS antérieures à 18.7 ont un changement cassant dans l’Explorateur d’objets en raison des modifications du moteur relatives au [pool SQL serverless Azure Synapse Analytics](https://docs.microsoft.com/azure/synapse-analytics/sql/on-demand-workspace-overview). | Pour continuer à utiliser l’Explorateur d’objets dans SSMS avec le pool SQL serverless Azure Synapse Analytics, vous avez besoin de SSMS 18.7 ou version ultérieure. |
| Magasin des requêtes | Le nœud Explorateur d’objets du Magasin des requêtes génère une erreur lors d’un clic droit. | Accédez directement aux éléments en développant le nœud et en cliquant avec le bouton droit sur les options enfants. |

### <a name="186"></a>18.6

![télécharger](media/download-icon.png) [Télécharger SSMS 18.6](https://go.microsoft.com/fwlink/?linkid=2135491)

- Numéro de version : 18.6
- Numéro de build : 15.0.18338.0
- Date de publication : 22 juillet 2020

[Chinois (simplifié)](https://go.microsoft.com/fwlink/?linkid=2135491&clcid=0x804) | [Chinois (traditionnel)](https://go.microsoft.com/fwlink/?linkid=2135491&clcid=0x404) | [Anglais (États-Unis)](https://go.microsoft.com/fwlink/?linkid=2135491&clcid=0x409) | [Français](https://go.microsoft.com/fwlink/?linkid=2135491&clcid=0x40c) | [Allemand](https://go.microsoft.com/fwlink/?linkid=2135491&clcid=0x407) | [Italien](https://go.microsoft.com/fwlink/?linkid=2135491&clcid=0x410) | [Japonais](https://go.microsoft.com/fwlink/?linkid=2135491&clcid=0x411) | [Coréen](https://go.microsoft.com/fwlink/?linkid=2135491&clcid=0x412) | [Portugais (Brésil)](https://go.microsoft.com/fwlink/?linkid=2135491&clcid=0x416) | [Russe](https://go.microsoft.com/fwlink/?linkid=2135491&clcid=0x419) | [Espagnol](https://go.microsoft.com/fwlink/?linkid=2135491&clcid=0x40a)

#### <a name="whats-new-in-186"></a>Nouveautés de la version 18.6

| Nouvel élément | Détails |
|----------|---------|
| Analysis Services | Mise à jour vers la dernière version des bibliothèques clientes AS. |
| Audit | Ajout de la prise en charge de l’ID d’action SENSITIVE_BATCH_COMPLETED_GROUP (une chaîne au lieu d’un nombre). |
| Audit | Ajout des champs suivants à la visionneuse d’audit : affected_rows, response_rows, connection_id, duration_milliseconds et data_sensitivity_information. |
| Classification des données | Mettez à jour SSMS pour prendre en charge l’importation/exportation d’une stratégie exportée via des applets de commande PowerShell. |
| Importer un fichier plat | Ajout de la prise en charge des fichiers de type largeur fixe et de la détection de type de fichier pour les fichiers .csv/.tsv pour s’assurer qu’ils sont analysés en tant que fichiers csv/tsv, respectivement. |
| Integration Services | Ajout de la prise en charge des travaux de l’agent Azure SQL Managed Instance pour exécuter un package SSIS à partir du magasin de packages dans Azure-SSIS IR. |
| SMO / Création de scripts | Ajout de la prise en charge des scripts de masquage dynamique des données sur [Azure Synapse Analytics](/azure/synapse-analytics/sql-data-warehouse/sql-data-warehouse-overview-what-is) (anciennement SQL Azure DW). |
| SMO / Création de scripts | Support ajouté à la stratégie de sécurité du script sur [Azure Synapse Analytics](/azure/synapse-analytics/sql-data-warehouse/sql-data-warehouse-overview-what-is). |

#### <a name="bug-fixes-in-186"></a>Correctifs de bogues dans la version 18.6

| Nouvel élément | Détails |
|----------|---------|
| Accessibilité | Réglage des couleurs de bordure pour l’accessibilité sur la **page Propriétés générales de la base de données** (la bordure autour de la grille et de la zone de nom est plus sombre pour définir le contraste sur > 3:1). |
| Accessibilité | Ajout de la gestion de l’exécution de la requête pour mettre à jour le narrateur (nécessite l’installation de NetFx 4.8+ sur l’ordinateur). |
| Always Encrypted | Correction d’un problème dans lequel la boîte de dialogue *Nouvelle clé de chiffrement de colonne* indique que la clé CEK ne prend pas en charge l’enclave, même si la clé CMK prend en charge l’enclave. |
| Analysis Services | Résolution d’un problème d’affichage de partitions Analysis Services ayant peut-être provoqué une exception non gérée. |
| **Diagrammes de base de données** | Résolution d’un problème en suspens depuis longtemps au niveau des **diagrammes de base de données**, provoquant l’altération et le blocage des diagrammes existants et de SSMS. Si vous avez créé ou enregistré un diagramme à l’aide de SSMS 18.0 à 18.5.1 et que ce diagramme comprend une *annotation de texte*, vous ne pourrez ouvrir ce diagramme dans aucune version de SSMS. Avec ce correctif, SSMS 18.6 peut ouvrir et enregistrer un diagramme créé par SSMS 17.9.1 et les versions antérieures. SSMS 17.9.1 et les versions précédentes peuvent également ouvrir le diagramme une fois enregistré par SSMS 18.6. Voir [Commentaires des utilisateurs sur SQL Server](https://feedback.azure.com/forums/908035/suggestions/37992649). |
| Classification des données | Correction d’un problème dans lequel le nom de colonne ne s’affiche pas dans le panneau de recommandations du volet de classification des données. |
| SSMS général | Résolution d’un problème dans lequel les propriétés de la base de données *Taille* et *Espace disponible* affichent des valeurs incorrectes pour la base de données SQL Azure (niveau de service Hyperscale). |
| SSMS général | Résolution d’un problème dans lequel les propriétés de la base de données « Taille » affichent la taille maximale au lieu de la taille réelle de la base de données pour les bases de données SQL Azure (remarque : pour DW, la taille maximale est toujours affichée). |
| SSMS général | Résolution de trois sources de blocage courantes dans SSMS. |
| SSMS général | Résolution de quelques problèmes dans lesquels la boîte de dialogue de connexion de SSMS *oublie* des entrées (serveur/utilisateur/mots de passe). Voir [Commentaires des utilisateurs sur SQL Server](https://feedback.azure.com/forums/908035/suggestions/40256401) et [Commentaires des utilisateurs sur SQL Server](https://feedback.azure.com/forums/908035/suggestions/40015519). |
| SSMS général | Résolution d’un problème avec la boîte de dialogue **Propriétés de statistiques**, dans laquelle la case à cocher **Mettre à jour les statistiques pour ces colonnes** et la sélection de l’option **OK** n’a aucun effet. Les statistiques ne sont pas mises à jour, et la tentative de création d’un script pour l’action génère un message d’erreur *Aucune action ne requiert de script.* ). Voir [Commentaires des utilisateurs sur SQL Server](https://feedback.azure.com/forums/908035/suggestions/37799992). |
| SSMS général | Résoudre les problèmes liés à [CVE-2020-1455](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2020-1455). | 
| Application de la couche Données d’importation/exportation | Correction d’un problème dans lequel SSMS provoquait une erreur lors de l’importation d’un fichier bacpac. Voir [Commentaires des utilisateurs sur SQL Server](https://feedback.azure.com/forums/908035/suggestions/40229137). |
| Integration Services | Correction d’un bogue dans lequel les clients ne peuvent pas modifier une étape de travail de l’agent SQL lors de l’utilisation des versions 18.4 ou antérieures de SSMS pour exécuter des packages SSIS dans Azure SQL Managed Instance. |
| Integration Services | Correction d’un bogue dans lequel l'option **Utiliser le runtime 32 bits** est manquante dans l’onglet **Options d'exécution** pour exécuter un package SSIS dans une étape de travail de l’agent SQL pour une instance SQL Server locale. |
| IntelliSense / Éditeur | Correction d’un problème dans lequel une boîte de dialogue d’erreur peut s’afficher lors de la sélection du menu Fichier -> Nouveau -> Requête de moteur de base de données. |
| Explorateur d’objets | Correction d’un problème dans lequel la *fenêtre Propriétés* n’était pas disponible pour les instances Azure SQL Database quand vous cliquez avec le bouton droit sur un nœud Table ou Index dans l’Explorateur d’objets. |
| Explorateur d’objets | Résolution d’un problème dans lequel SSMS ne peut pas développer le nœud des bases de données pour master dans Azure en cas de défaillance du plan de contrôle affectant sys.database_service_objectives. |
| Rapports | Correction de plusieurs rapports standard corrompus sur Linux </br></br> Exemple : Le rapport de consommation de mémoire a échoué avec une erreur semblable à celle-ci : « /var/opt/mssql/log/log_116.trc\log.trc' n’est pas valide… »). |
| SMO / Création de scripts | Mise à jour de la logique pour créer des bases de données dans Azure SQL Database afin d’utiliser Gen5_2 comme SLO par défaut. |
| Interface utilisateur XEvent | Résolution d’un problème en suspens depuis longtemps (introduit dans SSMS 18.0) dans lequel « Enregistrer dans un fichier XEL... » provoquait une erreur. Voir [Commentaires des utilisateurs sur SQL Server](https://feedback.azure.com/forums/908035/suggestions/37695592). |

#### <a name="known-issues-186"></a>Problèmes connus (18.6)

| Nouvel élément | Détails | Solution de contournement |
|----------|---------|------------|
| Analysis Services | Erreur lors de la connexion à SSAS via msmdpump.dll. Voir [Commentaires des utilisateurs sur SQL Server](https://feedback.azure.com/forums/908035-sql-server/suggestions/40144696). | N/A |
| SSMS général | La boîte de dialogue Nouvelle spécification de l’audit du serveur peut provoquer le blocage de SSMS avec une erreur de violation d’accès. | N/A |
| SSMS général | Les extensions SSMS utilisant SMO doivent être recompilées pour cibler le nouveau package SMO v161 spécifique à SSMS. Une préversion est disponible à l’adresse https://www.nuget.org/packages/Microsoft.SqlServer.SqlManagementObjects.SSMS/ </br></br> Les extensions compilées avec des versions 160 antérieures du package Microsoft.SqlServer.SqlManagementObjects continueront de fonctionner. | N/A |
| Integration Services | Lorsque vous importez ou exportez des packages dans Integration Services ou exportez des packages dans Azure-SSIS Integration Runtime, des scripts sont perdus pour les packages contenant des tâches/composants de script. Solution de contournement : Supprimer le dossier « C:\Program Files (x86)\Microsoft SQL Server Management Studio 18\Common7\IDE\CommonExtensions\MSBuild ». | N/A|
| Integration Services | Les connexions à distance à Integration Services peuvent échouer avec le message « Le service spécifié n’existe pas en tant que service installé. » sur les systèmes d’exploitation récents. Solution de contournement : Identifiez l’emplacement du Registre associé aux services d'intégration sous Computer\HKEY_CLASSES_ROOT\AppID & Computer\HKEY_CLASSES_ROOT\ WOW6432Node\AppID, et dans ces ruches, renommez la clé de Registre « LocalService » en « LocalService_A » pour la version spécifique des services d’intégration que vous essayez de connecter | N/A|

### <a name="1851"></a>18.5.1

![télécharger](media/download-icon.png) [Télécharger SSMS 18.5.1](https://go.microsoft.com/fwlink/?linkid=2132606)

- Numéro de version : 18.5.1
- Numéro de build : 15.0.18333.0
- Date de publication : 9 juin 2020

[Chinois (simplifié)](https://go.microsoft.com/fwlink/?linkid=2132606&clcid=0x804) | [Chinois (traditionnel)](https://go.microsoft.com/fwlink/?linkid=2132606&clcid=0x404) | [Anglais (États-Unis)](https://go.microsoft.com/fwlink/?linkid=2132606&clcid=0x409) | [Français](https://go.microsoft.com/fwlink/?linkid=2132606&clcid=0x40c) | [Allemand](https://go.microsoft.com/fwlink/?linkid=2132606&clcid=0x407) | [Italien](https://go.microsoft.com/fwlink/?linkid=2132606&clcid=0x410) | [Japonais](https://go.microsoft.com/fwlink/?linkid=2132606&clcid=0x411) | [Coréen](https://go.microsoft.com/fwlink/?linkid=2132606&clcid=0x412) | [Portugais (Brésil)](https://go.microsoft.com/fwlink/?linkid=2132606&clcid=0x416) | [Russe](https://go.microsoft.com/fwlink/?linkid=2132606&clcid=0x419) | [Espagnol](https://go.microsoft.com/fwlink/?linkid=2132606&clcid=0x40a)

#### <a name="bug-fixes-in-1851"></a>Correctifs de bogues dans la version 18.5.1

| Nouvel élément | Détails |
|----------|---------|
| Analysis Services | Amélioration des performances du développement de la liste des bases de données pendant une connexion à des serveurs AS Azure or Power BI. |
| Analysis Services | Résolution d’un problème qui entraînait une erreur lors d’une tentative d’ouverture de l’Assistant Synchronisation de base de données d’un serveur Analysis Services. |
| Analysis Services | Correction d’un problème qui empêchait les utilisateurs d’interroger SSAS 2017 et les versions antérieures avec les autorisations sur les données des cellules. |
| SSMS général | [Concepteur de tables : suppression du bip sonore émis lors d’une tentative de tabulation dans une grille Concepteur de tables](https://feedback.azure.com/forums/908035/suggestions/40318435) |

#### <a name="known-issues-1851"></a>Problèmes connus (18.5.1)

| Nouvel élément | Détails | Solution de contournement |
|----------|---------|------------|
| SSMS général | Il existe un bogue connu dans la conception de schémas, qui endommage vos schémas existants. Par exemple, vous créez une conception de diagramme avec SSMS 17.9.1, vous la mettez à jour/l’enregistrez avec SSMS 18. x, puis vous essayez de l’ouvrir avec 17.9.1. Voir [Commentaires des utilisateurs sur SQL Server](https://feedback.azure.com/forums/908035/suggestions/37992649) pour plus de détails. | N/A |
| SSMS général | La boîte de dialogue Nouvelle spécification de l’audit du serveur peut provoquer le blocage de SSMS avec une erreur de violation d’accès. | N/A ||
| SMO/Création de scripts | Les extensions SSMS utilisant SMO doivent être recompilées pour cibler le nouveau SMO v160. | N/A |
| Integration Services | Lorsque vous importez ou exportez des packages dans Integration Services ou exportez des packages dans Azure-SSIS Integration Runtime, des scripts sont perdus pour les packages contenant des tâches/composants de script. Solution de contournement : | Supprimer le dossier « C:\Program Files (x86)\Microsoft SQL Server Management Studio 18\Common7\IDE\CommonExtensions\MSBuild ». |

### <a name="185"></a>18.5

![télécharger](media/download-icon.png) [Télécharger SSMS 18.5](https://go.microsoft.com/fwlink/?linkid=2125901)

- Numéro de version : 18.5
- Numéro de build : 15.0.18330.0
- Date de publication : 7 avril 2020

[Chinois (simplifié)](https://go.microsoft.com/fwlink/?linkid=2125901&clcid=0x804) | [Chinois (traditionnel)](https://go.microsoft.com/fwlink/?linkid=2125901&clcid=0x404) | [Anglais (États-Unis)](https://go.microsoft.com/fwlink/?linkid=2125901&clcid=0x409) | [Français](https://go.microsoft.com/fwlink/?linkid=2125901&clcid=0x40c) | [Allemand](https://go.microsoft.com/fwlink/?linkid=2125901&clcid=0x407) | [Italien](https://go.microsoft.com/fwlink/?linkid=2125901&clcid=0x410) | [Japonais](https://go.microsoft.com/fwlink/?linkid=2125901&clcid=0x411) | [Coréen](https://go.microsoft.com/fwlink/?linkid=2125901&clcid=0x412) | [Portugais (Brésil)](https://go.microsoft.com/fwlink/?linkid=2125901&clcid=0x416) | [Russe](https://go.microsoft.com/fwlink/?linkid=2125901&clcid=0x419) | [Espagnol](https://go.microsoft.com/fwlink/?linkid=2125901&clcid=0x40a)

#### <a name="whats-new-in-185"></a>Nouveautés de la version 18.5

| Nouvel élément | Détails |
|----------|---------|
| Analysis Services | Ajout de la prise en charge du point de terminaison Power BI dans Analysis Services, correspondance avec la fonctionnalité Azure Analysis Services. |
| Analysis Services | Profileur : ajout de la prise en charge d’Analysis Services Trace Definition 15.1. |
| Classification des données | Ajout d’un bouton à l’affichage des résultats de l’analyse VA, afin de corriger la règle de classification des données en accédant au volet classification des données. |
| Classification des données | Ajout de la prise en charge du classement de sensibilité dans la classification des données. |
| Hyperscale | Ajout de la prise en charge de *Importer l’application de la couche Données* (.bacpac) pour SQL Azure HyperScale. |
| Integration Services | Prise en charge de l’exécution du package SSIS à partir du système de fichiers dans un travail de l’agent MI. |
| Integration Services | Améliorations conviviales apportées à la configuration de DTExec compatible Azure pour l’appel d’exécutions de packages SSIS sur Azure-SSIS Integration Runtime.
| Integration Services | Prise en charge de la connexion du Runtime d’intégration Azure SSIS et de la gestion ou de l’exécution des packages SSIS dans les magasins de packages.
| Integration Services | Prise en charge la migration de travaux d’agent SSIS locaux vers des pipelines et des déclencheurs ADF.
| Integration Services | Amélioration de l’expérience utilisateur de l’exportation de projets SSIS à partir de la base de données SSIS. Par rapport à l’ancienne exportation, qui chargeait et mettait à niveau des packages dans le projet SSIS, la nouvelle exportation indépendante de la version ne charge pas et ne met pas à niveau les packages dans le projet SSIS. Au lieu de cela, elle conserve les packages dans les projets tels qu’ils sont dans la base de données SSIS, à l’exception du passage au niveau de protection EncryptSensitiveWithUserKey. |
| SMO/Création de scripts | Nouvelle propriété DwMaterializedViewDistribution ajoutée à l’objet Affichage. |
| SMO/Création de scripts | Suppression de la prise en charge des *restriction des fonctionnalités* (cette fonctionnalité en préversion a été supprimée dans SQL Azure et SQL local). |
| SMO/Création de scripts | Ajout de *Notebook* en tant que destination de l’Assistant Générer des scripts. |
| SMO/Création de scripts | Support ajouté au *pool SQL serverless Azure Synapse Analytics*. |
| SMO/Création de scripts | [API SQL Assessment](../tools/sql-assessment-api/sql-assessment-api-overview.md) - Les champs Plateforme, Nom et engineEdition peuvent désormais contenir des listes séparées par des virgules (*Plateforme*) habituelles : \[*Windows*, *Linux*\]), pas seulement les expressions régulières (*Plateforme* : *\/Windows\|Linux\/* )
| SMO/Création de scripts | [API SQL Assessment](../tools/sql-assessment-api/sql-assessment-api-overview.md) : ajout de 13 règles d’évaluation. Pour plus d’informations, accédez à [GitHub](https://github.com/microsoft/sql-server-samples/tree/master/samples/manage/sql-assessment-api)). |

#### <a name="bug-fixes-in-185"></a>Correctifs de bogues dans la version 18.5

| Nouvel élément | Détails |
|----------|---------|
| Accessibilité | SSIS ADF/nouvelle planification : résolution d’un problème où l’ordre du focus n’est pas logique dans le mode d’analyse du narrateur sous l’Assistant *Nouvelle planification*. |
| Accessibilité | Assistant Stretch Database : correction d’un problème où le lecteur d’écran n’indique pas le nom de la table de requête lors de la fourniture d’informations sur la table. |
| Analysis Services | Correction de la connexion mise en cache lors de l’écriture de scripts dans AS avec la connexion Azure AD. |
| Always On | Correction d’un problème où la première base de données ajoutée à Always On AG ne se joint pas correctement.
| Always On | Correction d’un problème où une erreur était affichée lors de la tentative d’affichage du tableau de bord lors de la connexion à un point de terminaison de cluster Big Data. |
| Audit | Correction d’un problème où la fenêtre des fusions des journaux d’audit se bloque lorsqu’un dossier avec un nom vide est présent dans le dossier racine du compte de stockage. |
| Audit | Correction d’un problème où la fenêtre de fusion des journaux d’audit n’affiche pas tous les serveurs lorsqu’il y a trop d’éléments à la racine du conteneur. |
| Classification des données | Correction d’un problème où l’Assistant *Classification des données* ne s’ouvre pas pour les bases de données comportant un grand nombre de tables. |
| Classification des données | Nous appliquons désormais différents GUID pour chaque étiquette/infoType et la structure du GUID dans le processus de validation. |
| Classification des données | Suppression du processus de classification dans SqlServer2019. |
| Classification des données | Correction des tests de validation précédents (ajout de rang, suppression de la propriété non conforme *InformationTypes*) et ajout de nouveaux pour les deux premiers points. |
| Classification des données | Le bouton situé juste au-dessus du tableau des colonnes classifiées réduit désormais le panneau recommandations, comme indiqué. |
| SSMS général | Mise à jour de la version des pilotes MSODBC et MSOLEDB. |
| SSMS général | Correction des blocages et défaillances d’au moins deux sources courantes dans SSMS. |
| SSMS général | Correction d’un autre cas où la *boîte de dialogue de restauration* se bloque lors de la sélection du bouton Parcourir. |
| SSMS général | Correctif de *l’interface graphique utilisateur des nouvelles bases de données* pour le pool SQL serverless Azure Synapse Analytics. |
| SSMS général | Correctif des modèles *Nouvelle table externe...* et *Nouvelle source de données externe...* pour le pool SQL serverless Azure Synapse Analytics. |
| SSMS général | Correctif des propriétés de base de données, propriétés de connexion, masquage de rapports et changement de nom pour le pool SQL serverless Azure Synapse Analytics. |
| SSMS général | Always Encrypted : Correction d’un problème où la liste déroulante du nom de clé devient en lecture seule lors de la sélection d’une nouvelle clé d’enclave. |
| SSMS général | Nettoyage de la grille *Options de propriété de base de données*, qui affichait deux *catégories diverses*. |
| SSMS général | Correction d’un problème où la barre de défilement démarrait du milieu dans la grille « Options de propriétés de base de données ». |
| SSMS général | Correction d’un problème qui provoquait le blocage de SSMS lors de l’ouverture d’un fichier .sql lors de la connexion au serveur Analysis Services. |
| SSMS général | Boîte de dialogue de connexion : correction d’un problème où le fait de décocher l’option « Se souvenir du mot de passe » ne fonctionne pas. |
| SSMS général | Correction d’un problème où les informations d’identification associées au serveur/aux utilisateurs sont toujours mémorisées. Voir [Commentaires des utilisateurs sur SQL Server](https://feedback.azure.com/forums/908035/suggestions/37875172). |
| SSMS général | Correction du problème où les fenêtres de l’éditeur n’étaient parfois pas correctement actualisées. Pour ce faire, il suffit de désactiver l’accélération matérielle dans *Outils > Options > Environnement*. Voir [Commentaires des utilisateurs sur SQL Server](https://feedback.azure.com/forums/908035/suggestions/37474042). |
| SSMS général | Correction d’un problème où l’authentification Azure Active Directory ne fonctionnait pas via un proxy. |
| Résolutions élevées/mise à l’échelle | Correction d’un problème où les contrôles sur les *propriétés d’index* pouvaient être restitués de manière incorrecte (boutons chevauchant la grille). Voir [Commentaires des utilisateurs sur SQL Server](https://feedback.azure.com/forums/908035/suggestions/36030424). |
| Résolutions élevées/mise à l’échelle | Correction de plusieurs problèmes dans la boîte de dialogue *Propriétés de la base de données*, qui peut afficher des contrôles tronqués sur des écrans 4K. |
| Résolutions élevées/mise à l’échelle | Correction des assistants de publication et d’abonnement sur les écrans 4K. |
| Résolutions élevées/mise à l’échelle | Correction mineure sur la page Nouvelle spécification du serveur d’audit. |
| Résolutions élevées/mise à l’échelle | Résolution du problème d’affichage 4K de l’Assistant Haute disponibilité. |
| Résolutions élevées/mise à l’échelle | Correction d’un problème où l’utilisateur n’était pas en mesure d’ajouter une cible dans une fenêtre Nouvelle session Xevent et Définir des filtres d’événement de session dans l’Assistant Session Xevent lorsque la mise à l’échelle de l’affichage est de 125 %. |
| Résolutions élevées/mise à l’échelle | Résolution d’un problème où les contrôles de l’interface utilisateur *Sauvegarder la base de données sur l’URL* ne sont pas affichés sous la mise à l’échelle au-dessus de 100 %. |
|Importer un fichier plat | Mise à jour de l’Assistant Importation d’un fichier plat pour autoriser l’option Vérifier tout pour la colonne Autoriser une valeur Null. Voir [Commentaires des utilisateurs sur SQL Server](https://feedback.azure.com/forums/908035/suggestions/38027137). |
| Explorateur d’objets | Correction d’un problème où l’Explorateur d’objets pouvait afficher des informations incorrectes lors de l’utilisation de chaînes de connexion pour se connecter dans la boîte de dialogue de connexion. |
| Explorateur d’objets | Correction d’un problème où l’Explorateur d’objets était lent dans le développement de tables pour les bases de données avec plusieurs milliers de tables (plus de 20 000). |
| Interface utilisateur du magasin de requêtes | Correction du calcul du nombre d’exécutions dans un rapport TRC (pour la métrique *temps d’attente*) comme la somme du nombre d’exécutions pour chaque catégorie d’attente individuelle, ce qui est incorrect. Toutefois, pour une seule exécution de requête, cela sera enregistré pour chacune des catégories d’attente attendues par la requête. Par conséquent, si TRC effectue simplement une somme sur la catégorie d’attente, le nombre d’exécutions augmente. En fait, il doit s’agir de la valeur maximale de wait_category. |
| Interface utilisateur du magasin de requêtes | Correction de la vue détaillée de TRC qui retourne des données incorrectes lorsque le jeu de résultats est filtré sur le x supérieur. Cela se produit parce que la requête utilise plusieurs expressions de table communes, qui sont ensuite jointes pour créer le jeu de résultats final. Si le x supérieur fait l’objet d’un envoi push dans CTE, il peut parfois filtrer les lignes requises. Cela peut parfois rendre le jeu de résultats non déterministe. Le correctif consiste à ne pas envoyer la clause x supérieur aux CTE. |
| Interface utilisateur du magasin de requêtes | Correction du résumé du plan dans la vue grille ou dans la vue graphique qui a besoin du temps d’attente d’exécution de la dernière requête. L’absence de cette colonne rompt la requête. Cet ensemble de modifications ajoutera cette colonne à la CTE des statistiques d’attente. |
| ShowPlan | Amélioration de la façon dont SSMS affiche le nombre de lignes estimé pour les opérateurs avec plusieurs exécutions : (1) Remplacement de *Nombre de lignes estimé* dans SSMS par « Nombre de lignes estimé par exécution » ; (2) Ajout d’une nouvelle propriété *Nombre de lignes estimé pour toutes les éxecutions* ; (3) Remplacement de la propriété *Nombre réel de lignes* par *Nombre réel de lignes pour toutes les exécutions*. |
| SQL Agent | Correction d’un problème où la tentative de modification d’une étape de travail SQL Agent pouvant entraîner le gel de l’interface utilisateur de SSMS. SSMS autorise désormais l’affichage (bouton *Afficher*) d’un output_file dont le nom est sous forme de jeton (au moins pour les macros/jetons simples pris en charge par SQL Agent qui ne sont pas déterminés au moment de l’exécution). En outre, SSMS ne désactive pas le bouton « Afficher » lorsque l’utilisateur n’a pas accès au fichier (en ce qui concerne les autorisations SQL). Voir [Commentaires des utilisateurs sur SQL Server](https://feedback.azure.com/forums/908035/suggestions/39063124). |
| SQL Agent | Correction de l’ordre des onglets sur la page Étape du travail. |
| SQL Agent | Inversion de la position des boutons « Suivant » et « Précédent » sur la page Étape du travail pour les placer dans un ordre logique. |
| SQL Agent | Ajustement de la fenêtre Planification du travail pour ne pas découper l’interface utilisateur. |
| SMO/Création de scripts | Correctif des scripts de base de données pour le pool SQL serverless Azure Synapse Analytics. |
| SMO/Création de scripts | Suppression du cast sqlvariant explicite (syntaxe T-SQL non conforme pour SqlOnDemand) qui corrige les scripts pour SqlOnDemand. |
| SMO/Création de scripts | Correction d’un problème où FILLFACTOR sur les index pour SQL Azure était ignoré. |
| SMO/Création de scripts | Correction d’un problème lié à l’écriture de scripts d’objets externes. |
| SMO/Création de scripts | Correction d’un problème où *Générer des scripts* n’autorisait pas le choix de l’option de script pour les propriétés étendues sur SQL Database. En outre, correction du script de telles propriétés étendues. |
| SMO/Création de scripts | [API SQL Assessment](../tools/sql-assessment-api/sql-assessment-api-overview.md) - Lien d’aide erroné dans la règle XTPHashAvgChainBuckets. |
| Interface utilisateur XEvent | Correction d’un problème où les éléments de la grille étaient sélectionnés en cas de pointage. Voir [Commentaires des utilisateurs sur SQL Server](https://feedback.azure.com/forums/908035/suggestions/38262124) et [Commentaires des utilisateurs sur SQL Server](https://feedback.azure.com/forums/908035-sql-server/suggestions/37873921). |

#### <a name="known-issues-185"></a>Problèmes connus (18.5)

| Nouvel élément | Détails | Solution de contournement |
|----------|---------|------------|

- Le diagramme de base de données créé à partir de SSMS s’exécutant sur une machine A ne peut pas être modifié sur la machine B (SSMS se plante). Voir [Commentaires des utilisateurs sur SQL Server 37992649](https://feedback.azure.com/forums/908035/suggestions/37992649) pour plus de détails.

- Lorsque vous importez ou exportez des packages dans Integration Services ou exportez des packages dans Azure-SSIS Integration Runtime, des scripts sont perdus pour les packages contenant des tâches/composants de script. Une solution de contournement consiste à supprimer le dossier *C:\Program Files (x86)\Microsoft SQL Server Management Studio 18\Common7\IDE\CommonExtensions\MSBuild*.

- La boîte de dialogue Nouvelle spécification de l’audit du serveur peut provoquer le blocage de SSMS avec une erreur de violation d’accès.

- Les extensions SSMS utilisant SMO doivent être recompilées pour cibler le nouveau SMO v160 (le package sera disponible sur nuget.org juste après la publication de SSMS 18.5).

- [Erreur lors de la connexion à SSAS via msmdpump.dll dans SSMS](https://feedback.azure.com/forums/908035-sql-server/suggestions/40144696-error-when-connecting-to-ssas-via-msmdpump-dll-in).

### <a name="184"></a>18.4

![télécharger](media/download-icon.png) [Télécharger SSMS 18.4](https://go.microsoft.com/fwlink/?linkid=2108895)

- Numéro de version : 18.4
- Numéro de build : 15.0.18206.0
- Date de publication : 4 novembre 2019

[Chinois (simplifié)](https://go.microsoft.com/fwlink/?linkid=2108895&clcid=0x804) | [Chinois (traditionnel)](https://go.microsoft.com/fwlink/?linkid=2108895&clcid=0x404) | [Anglais (États-Unis)](https://go.microsoft.com/fwlink/?linkid=2108895&clcid=0x409) | [Français](https://go.microsoft.com/fwlink/?linkid=2108895&clcid=0x40c) | [Allemand](https://go.microsoft.com/fwlink/?linkid=2108895&clcid=0x407) | [Italien](https://go.microsoft.com/fwlink/?linkid=2108895&clcid=0x410) | [Japonais](https://go.microsoft.com/fwlink/?linkid=2108895&clcid=0x411) | [Coréen](https://go.microsoft.com/fwlink/?linkid=2108895&clcid=0x412) | [Portugais (Brésil)](https://go.microsoft.com/fwlink/?linkid=2108895&clcid=0x416) | [Russe](https://go.microsoft.com/fwlink/?linkid=2108895&clcid=0x419) | [Espagnol](https://go.microsoft.com/fwlink/?linkid=2108895&clcid=0x40a)

| Nouvel élément | Détails |
|----------|---------|
| Classification des données | Ajout de la prise en charge d’une stratégie de protection des informations personnalisée pour la classification des données. |
| Magasin des requêtes | Ajout de la valeur *Plan max par requête* dans les propriétés de la boîte de dialogue. |
| Magasin des requêtes | Ajout de la prise en charge des nouvelles stratégies de capture personnalisées. |
| Magasin des requêtes | Ajout du **Mode de capture des statistiques d’attente** aux options **Propriétés de base de données** du **Magasin des requêtes**. |
| SMO/Création de scripts | Prise en charge du script de vue matérialisée dans SQL DW. |
| SMO/Création de scripts | Support ajouté au *pool SQL serverless Azure Synapse Analytics*. |
| SMO/Création de scripts | [API SQL Assessment](../tools/sql-assessment-api/sql-assessment-api-overview.md) : ajout de 50 règles d’évaluation (détails sur GitHub). |
| SMO/Création de scripts | [API SQL Assessment](../tools/sql-assessment-api/sql-assessment-api-overview.md) : ajout d’expressions mathématiques de base et de comparaisons aux conditions des règles. |
| SMO/Création de scripts | [API SQL Assessment](../tools/sql-assessment-api/sql-assessment-api-overview.md) : ajout de la prise en charge de l’objet RegisteredServer. |
| SMO/Création de scripts | [API SQL Assessment](../tools/sql-assessment-api/sql-assessment-api-overview.md) : mise à jour de la façon dont les règles sont stockées au format JSON et mise à jour du mécanisme d’application des remplacements/personnalisations. |
| SMO/Création de scripts | [API SQL Assessment](../tools/sql-assessment-api/sql-assessment-api-overview.md) : mises à jour des règles pour prendre en charge SQL sur Linux. |
| SMO/Création de scripts | [API SQL Assessment](../tools/sql-assessment-api/sql-assessment-api-overview.md) : mise à jour du format JSON de l’ensemble de règles et ajout de la version du schéma. |
| SMO/Création de scripts | [API SQL Assessment](../tools/sql-assessment-api/sql-assessment-api-overview.md) : mise à jour de la sortie des applets de commande pour améliorer la lisibilité des recommandations. |
| XEvent Profiler | Ajout de l’événement *error_reported* aux sessions XEvent Profiler. |

#### <a name="bug-fixes-in-184"></a>Correctifs de bogues dans la version 18.4

| Nouvel élément | Détails |
|----------|---------|
| Analysis Services | Résolution d’un problème où l’éditeur de script DAX pour les bases de données multidimensionnelles n’affichait pas les tables dans IntelliSense. |
| Analysis Services | Utilisez l’analyseur DAX pour effectuer la conversion en chaîne de moteur. Ceci concerne les séparateurs internationaux, les décimales et les espaces blancs. |
| Always Encrypted | Résolution d’un problème où la *validation des revendications* n’était pas *insensible à la casse*. |
| Always Encrypted | Résolution d’un problème où le rapport d’erreurs/d’avertissements ne fonctionnait pas correctement. |
| Assistant Copie de base de données | Correction des erreurs de troncation et de disposition assorties dans le rendu de cette boîte de dialogue. |
| SSMS général | Résolution d’un problème en suspens depuis longtemps où SSMS n’honorait pas les informations de connexion passées à la ligne de commande quand des fichiers SQL étaient également spécifiés. |
| SSMS général | Correction d’un incident dans SSMS lors de la tentative d’affichage d’éléments sécurisables sur des objets « Filtre de réplication ». |
| SSMS général | Atténuation de la suppression de l’option de ligne de commande -P en demandant à SSMS d’examiner son cache d’informations d’identification : si les informations d’identification nécessaires sont trouvées, la connexion est établie à l’aide de celles-ci. |
| Importer un fichier plat | Résolution d’un problème où la fonctionnalité *Importer un fichier plat* ne gérait pas correctement les qualificateurs de texte. |
| Explorateur d’objets | Résolution d’un problème où la suppression d’une base de données Azure SQL dans l’Explorateur d’objets affichait un message incorrect. |
| Résultats de requête | Résolution d’un problème introduit dans SSMS 18.3.1 où les grilles étaient légèrement trop étroites et affichaient *...* à la fin de la chaîne la plus longue dans chaque colonne. |
| Outils de réplication | Résolution d’un problème qui provoquait une erreur générée par l’application (« Impossible de charger le fichier ou l’assembly... ») lors de la tentative de modification des travaux de l’agent SQL. |
| SMO/Création de scripts | Résolution d’un problème qui empêchait le fonctionnement de *Générer un script de la table en tant que...* pour SQL DW avec le classement Japanese_BIN2.|
| SMO/Création de scripts | Résolution d’un problème où ScriptAlter() finissait par exécuter les instructions sur le serveur.|
| SQL Agent | Résolution d’un problème où l’interface utilisateur de l’opérateur de l’agent ne mettait pas à jour le nom de l’opérateur quand celui-ci était modifié dans l’interface utilisateur et ne faisait pas l’objet d’un script. Voir [Commentaires des utilisateurs sur SQL Server](https://feedback.azure.com/forums/908035/suggestions/32897647) pour plus de détails.|

#### <a name="known-issues-184"></a>Problèmes connus (18.4)

- Le diagramme de base de données créé à partir de SSMS s’exécutant sur une machine A ne peut pas être modifié sur la machine B (SSMS se plante). Voir [Commentaires des utilisateurs sur SQL Server](https://feedback.azure.com/forums/908035/suggestions/37992649) pour plus de détails.

- Il y a des problèmes de regénération du dessin lors du basculement d’une fenêtre de requête vers une autre. Voir [Commentaires des utilisateurs sur SQL Server](https://feedback.azure.com/forums/908035/suggestions/37474042) pour plus de détails. Pour contourner ce problème, vous pouvez désactiver l’accélération matérielle sous *Outils > Options*.

Pour connaître les autres problèmes connus et pour envoyer vos commentaires à l’équipe produit, accédez à [Commentaires des utilisateurs sur SQL Server](https://feedback.azure.com/forums/908035-sql-server).

### <a name="1831"></a>18.3.1

![télécharger](media/download-icon.png) [Télécharger SSMS 18.3.1](https://go.microsoft.com/fwlink/?linkid=2105412)

- Numéro de version : 18.3.1
- Numéro de build : 15.0.18183.0
- Date de publication : 2 octobre 2019

[Chinois (simplifié)](https://go.microsoft.com/fwlink/?linkid=2105412&clcid=0x804) | [Chinois (traditionnel)](https://go.microsoft.com/fwlink/?linkid=2105412&clcid=0x404) | [Anglais (États-Unis)](https://go.microsoft.com/fwlink/?linkid=2105412&clcid=0x409) | [Français](https://go.microsoft.com/fwlink/?linkid=2105412&clcid=0x40c) | [Allemand](https://go.microsoft.com/fwlink/?linkid=2105412&clcid=0x407) | [Italien](https://go.microsoft.com/fwlink/?linkid=2105412&clcid=0x410) | [Japonais](https://go.microsoft.com/fwlink/?linkid=2105412&clcid=0x411) | [Coréen](https://go.microsoft.com/fwlink/?linkid=2105412&clcid=0x412) | [Portugais (Brésil)](https://go.microsoft.com/fwlink/?linkid=2105412&clcid=0x416) | [Russe](https://go.microsoft.com/fwlink/?linkid=2105412&clcid=0x419) | [Espagnol](https://go.microsoft.com/fwlink/?linkid=2105412&clcid=0x40a)

#### <a name="whats-new-in-1831"></a>Nouveautés de la version 18.3.1

| Nouvel élément | Détails |
|----------|---------|
| Classification des données | Ajout des informations de classification des données à l’interface utilisateur des propriétés de colonne (*Type d’informations*, *ID du type d’informations*, *Étiquette de sensibilité* et *ID de l’étiquette de sensibilité* ne sont pas exposés dans l’interface utilisateur de SSMS). |
| IntelliSense/Éditeur | Mise à jour de la prise en charge des fonctionnalités récemment ajoutées à SQL Server 2019 (par exemple *ALTER SERVER CONFIGURATION*). |
| Integration Services | Ajout d’un nouvel élément du menu de sélection `Tools > Migrate to Azure > Configure Azure-enabled DTExec` qui appelle des exécutions de packages SSIS sur Azure-SSIS Integration Runtime comme activités Exécuter le package SSIS dans des pipelines ADF. |
| SMO/Création de scripts | Ajout de la prise en charge des scripts de contrainte unique Azure SQL DW. |
| SMO/Création de scripts | Classification des données </br> - Ajout de la prise en charge de SQL version 10 (SQL 2008) et ultérieur. </br> - Ajout d’un nouvel attribut de sensibilité « rang » pour SQL version 15 (SQL 2019) et ultérieur et Azure SQL Database. |
| SMO/Création de scripts | [API SQL Assessment](../tools/sql-assessment-api/sql-assessment-api-overview.md) - Ajout du contrôle de version au format des ensembles de règles. |
| SMO/Création de scripts | [API SQL Assessment](../tools/sql-assessment-api/sql-assessment-api-overview.md) - Ajout de nouvelles vérifications. |
| SMO/Création de scripts | [API SQL Assessment](../tools/sql-assessment-api/sql-assessment-api-overview.md) - Ajout de la prise en charge d’Azure SQL Managed Instance. |
| SMO/Création de scripts | [API SQL Assessment](../tools/sql-assessment-api/sql-assessment-api-overview.md) - Mise à jour de la vue par défaut des applets de commande pour afficher les résultats sous forme de table. |

#### <a name="bug-fixes-in-1831"></a>Correctifs de bogues dans la version 18.3.1

| Nouvel élément | Détails |
|----------|---------|
| Analysis Services | Correction du problème de mise à l’échelle dans l’éditeur de requête MDX.|
| Analysis Services | Correction d’un problème dans l’interface utilisateur de XEvent qui empêche les utilisateurs de créer une session. |
| Déploiement de base de données sur Azure SQL | Correction d’un problème (dans DacFx) qui faisait que cette fonctionnalité ne fonctionnait pas.|
| SSMS général | Résolution d’un problème qui provoquait le plantage de SSMS lors de l’utilisation de la fonctionnalité de tri dans la visionneuse XEvent. |
| SSMS général | Correction de problèmes en suspens depuis longtemps, où la restauration d’une base de données SSMS peut cesser de répondre indéfiniment. </br></br> Voir les articles Commentaires des utilisateurs sur SQL Server pour plus de détails : </br> [Restore Database - Select Backup Devices Slow to Load](https://feedback.azure.com/forums/908035/suggestions/32899099/).  </br> [SSMS 2016 très lent dans les boîtes de dialogue de restauration de base de données](https://feedback.azure.com/forums/908035/suggestions/32900767/). </br> [La restauration d’une base de données est lente](https://feedback.azure.com/forums/908035/suggestions/32900224/).  </br> [La restauration d’une base de données à partir de l’appareil SE BLOQUE après un clic sur « ... »](https://feedback.azure.com/forums/908035/suggestions/34281658/).  |
| SSMS général | Correction d’un problème où la langue par défaut pour toutes les connexions était affichée en arabe. </br></br> Voir l’article Commentaires des utilisateurs sur SQL Server pour plus de détails : [SSMS 18.2 default language display bug](https://feedback.azure.com/forums/908035/suggestions/38236363). |
| SSMS général | Correction du problème d’affichage difficile de la boîte de dialogue *Options de requête* (quand l’utilisateur clique avec le bouton droit sur la fenêtre de l’éditeur T-SQL) en la rendant redimensionnable.|
| SSMS général | Le message *Heure de fin* visible dans la grille ou le fichier de résultats (introduit dans SSMS 18.2) peut désormais être configuré sous Outils > Options > Exécution de la requête > SQL Server > Avancé > Afficher l’heure de complétion. |
| SSMS général | Dans la boîte de dialogue de connexion, *Active Directory - Mot de passe* and *Active Directory - Intégré* ont été respectivement remplacés par *Azure Active Directory - Mot de passe*  and *Azure Active Directory - Intégré*. |
| SSMS général | Correction d’un problème qui empêche les utilisateurs de pouvoir utiliser SSMS pour configurer l’audit sur Azure SQL - Instances managées quand elles se trouvent dans un fuseau horaire avec un décalage UTC négatif. |
| SSMS général | Résolution d’un problème dans l’interface utilisateur XEvent où le pointage sur la grille entraînait la sélection de lignes. </br></br> Voir l’article Commentaires des utilisateurs sur SQL Server pour plus de détails : [SSMS Extended Events UI Selects Actions on Hover](https://feedback.azure.com/forums/908035/suggestions/38262124). |
| Importer un fichier plat | Correction du problème où Importer un fichier plat n’importait pas toutes les données en permettant à l’utilisateur de choisir entre une détection du type de données simple ou riche.</br></br> Voir l’article Commentaires des utilisateurs sur SQL Server pour plus de détails : [SSMS Import Flat File fails to import all data](https://feedback.azure.com/forums/908035/suggestions/38096989). |
| Integration Services | Ajout d’un nouveau type d’opération *StartNonCatalogExecution* pour le rapport des opérations de SSIS.|
| Integration Services | Correction d’un problème dans les pipelines Azure Data Factory générés par l’utilitaire `DTExec` compatible Azure pour utiliser le type de paramètre correct. (explicite pour 18.3.1) |
| SMO/Création de scripts | Correction d’un problème, provoquant la levée d’erreurs par SMO lors de l’extraction des propriétés quand **SMO.Server.SetDefaultInitFields(true)** était utilisé.|
| Interface utilisateur du magasin de requêtes | Résolution d’un problème où l’axe des Y ne se mettait pas à l’échelle quand la métrique *Nombre d’exécutions* était sélectionnée dans la vue *Requête suivie*. |
| Évaluation des vulnérabilités | Désactivation de l’effacement et de l’approbation de la base de référence pour les bases de données Azure SQL Database.|

#### <a name="known-issues-1831"></a>Problèmes connus (18.3.1)

- Le diagramme de base de données créé à partir de SSMS s’exécutant sur une machine A ne peut pas être modifié sur la machine B (SSMS se plante). Voir [Commentaires des utilisateurs sur SQL Server](https://feedback.azure.com/forums/908035/suggestions/37992649) pour plus de détails.

- Il y a des problèmes de regénération du dessin lors du basculement d’une fenêtre de requête vers une autre. Voir [Commentaires des utilisateurs sur SQL Server](https://feedback.azure.com/forums/908035/suggestions/37474042) pour plus de détails. Pour contourner ce problème, vous pouvez désactiver l’accélération matérielle sous Outils > Options.

Pour connaître les autres problèmes connus et pour envoyer vos commentaires à l’équipe produit, accédez à [Commentaires des utilisateurs sur SQL Server](https://feedback.azure.com/forums/908035-sql-server).

### <a name="182"></a>18.2

![Télécharger](media/download-icon.png) [Télécharger SSMS 18.2](https://go.microsoft.com/fwlink/?linkid=2099720)

- Numéro de version : 18.2
- Numéro de build : 15.0.18142.0
- Date de publication : 25 juillet 2019

[Chinois (simplifié)](https://go.microsoft.com/fwlink/?linkid=2099720&clcid=0x804) | [Chinois (traditionnel)](https://go.microsoft.com/fwlink/?linkid=2099720&clcid=0x404) | [Anglais (États-Unis)](https://go.microsoft.com/fwlink/?linkid=2099720&clcid=0x409) | [Français](https://go.microsoft.com/fwlink/?linkid=2099720&clcid=0x40c) | [Allemand](https://go.microsoft.com/fwlink/?linkid=2099720&clcid=0x407) | [Italien](https://go.microsoft.com/fwlink/?linkid=2094583&clcid=0x410) | [Japonais](https://go.microsoft.com/fwlink/?linkid=2099720&clcid=0x411) | [Coréen](https://go.microsoft.com/fwlink/?linkid=2099720&clcid=0x412) | [Portugais (Brésil)](https://go.microsoft.com/fwlink/?linkid=2099720&clcid=0x416) | [Russe](https://go.microsoft.com/fwlink/?linkid=2099720&clcid=0x419) | [Espagnol](https://go.microsoft.com/fwlink/?linkid=2099720&clcid=0x40a)

#### <a name="whats-new-in-182"></a>Nouveautés de la version 18.2

| Nouvel élément | Détails |
|----------|---------|
| Integration Services (SSIS) | Optimisation des performances pour le planificateur de packages SSIS dans Azure. |
| IntelliSense/Éditeur | Ajout de la prise en charge de la classification des données. |
| OPTIMIZE_FOR_SEQUENTIAL_KEY | Ajout de la prise en charge IntelliSense. |
| OPTIMIZE_FOR_SEQUENTIAL_KEY | Active une optimisation dans le moteur de base de données qui vous aide à améliorer le débit pour les insertions de haute concurrence dans l’index. Cette option vise les index sujets à contention d’insertion de la dernière page, souvent avec des index comportant une clé séquentielle comme une colonne d’identité, une séquence ou une colonne de date/heure. Pour plus d’informations, consultez [CREATE INDEX](../t-sql/statements/create-index-transact-sql.md#sequential-keys). |
| Exécution ou résultats des requêtes | Ajout d’un *délai d’achèvement* dans les messages à suivre lorsque l’exécution d’une requête donnée est terminée. |
| Exécution ou résultats des requêtes | Permet d’afficher plus de données (Résultats dans du texte) et d’en stocker davantage dans des cellules (Résultats dans des grilles). SSMS autorise désormais jusqu’à 2 millions de caractères pour ces deux options (auparavant, les limites étaient fixées à 256 000 caractères pour l’affichage et à 64 000 caractères pour le stockage dans des grilles). Cela règle également le problème des utilisateurs qui ne parvenaient pas à extraire plus de 43 680 caractères des cellules d’une grille. |
| ShowPlan | Ajout d’un nouvel attribut dans QueryPlan quand la [fonctionnalité UDF scalaire Inline](../relational-databases/performance/intelligent-query-processing.md#scalar-udf-inlining) est activée (ContainsInlineScalarTsqludfs). |
| SMO | Ajout de la prise en charge de l’*API SQL Assessment*. Pour plus d’informations, consultez [API SQL Assessment](../tools/sql-assessment-api/sql-assessment-api-overview.md). |
|  |  |

#### <a name="bug-fixes-in-182"></a>Correctifs de bogues dans la version 18.2

| Nouvel élément | Détails |
|------------|-------|
| Accessibilité | Mise à jour de l’interface utilisateur XEvent (la grille) afin de permettre le tri des données par le biais de la touche F3. |
| Always On | Résolution d’un problème lié à l’affichage d’une erreur dans SSMS lorsque vous tentiez de supprimer un groupe de disponibilité. |
| Always On | Résolution d’un problème lié à l’affichage du mauvais Assistant de basculement dans SSMS quand des réplicas sont configurés comme étant synchrones, lors de l’utilisation de groupes de disponibilité à échelle horizontale en lecture (type de cluster = NONE). Désormais, SSMS affiche l’Assistant de l’option Force_Failover_Allow_Data_Loss, qui est la seule autorisée pour les groupes de disponibilité ayant un cluster de type NONE. |
| Always On | Résolution d’un problème lors duquel l’Assistant limitait le nombre de synchronisations autorisées à trois |
| Classification des données | Résolution d’un problème où SSMS provoquait l’affichage de l’erreur *L’index (de base zéro) doit être supérieur ou égal à zéro* lorsque l’utilisateur tentait d’afficher les rapports de classification des données des bases de données avec un CompatLevel inférieur à 150. |
| SSMS général | Correction d’un problème où l’utilisateur ne pouvait pas faire défiler horizontalement le volet des résultats à l’aide de la roulette de la souris. Voir [Commentaires des utilisateurs sur SQL Server](https://feedback.azure.com/forums/908035/suggestions/34145641) pour plus de détails. |
| SSMS général | Mise à jour du *Moniteur d’activité* permettant d’ignorer les types d’attente bénins comme SQLTRACE_WAIT_ENTRIES |
| SSMS général | Résolution d’un problème où certaines options de couleur *(Éditeur de texte > Onglet Éditeur et Barre d’état)* n’étaient pas persistantes. Voir [Commentaires des utilisateurs sur SQL Server](https://feedback.azure.com/forums/908035/suggestions/37924165)
| SSMS général | Dans la boîte de dialogue Connexion, l’option *Azure Active Directory - Authentification universelle avec prise en charge de MFA* a été remplacée par *Azure Active Directory - Authentification universelle avec MFA* (la fonctionnalité reste la même, mais nous espérons que le titre de l’option est désormais plus clair). |
| SSMS général | Mise à jour de SSMS pour que les bonnes valeurs par défaut soient utilisées lors de la création d’une instance Azure SQL Database. |
| SSMS général | Résolution d’un problème où l’utilisateur ne pouvait pas *démarrer PowerShell* à partir d’un nœud dans [Inscrire les serveurs](register-servers/register-servers.md) si le serveur était un [conteneur SQL Linux](../linux/quickstart-install-connect-docker.md). |
| Importer un fichier plat | Résolution d’un problème où l’option *Importer un fichier plat* ne fonctionnait plus après la mise à niveau de SSMS 18.0 vers la version 18.1. Voir [Commentaires des utilisateurs sur SQL Server](https://feedback.azure.com/forums/908035/suggestions/37912636) |
| Importer un fichier plat | Résolution d’un problème où *l’Assistant Importation d’un fichier plat signalait une colonne en double ou non valide* dans un fichier .csv comprenant des en-têtes avec des caractères Unicode. |
| Explorateur d’objets | Résolution d’un problème où certains éléments de menu (par exemple, l’*Assistant Importation et exportation* SQL Server) étaient manquants ou désactivés quand vous vous connectiez à SQL Express. Voir [Commentaires des utilisateurs sur SQL Server](https://feedback.azure.com/forums/908035/suggestions/37500016) pour plus de détails. |
| Explorateur d’objets | Résolution d’un problème qui provoquait le plantage de SSMS quand un objet était glissé de l’Explorateur d’objets vers l’éditeur. Voir [Commentaires des utilisateurs sur SQL Server](https://feedback.azure.com/forums/908035/suggestions/37887988) pour plus de détails. |
| Explorateur d’objets | Résolution d’un problème lors duquel le renommage des bases de données provoquait l’affichage de noms de bases de données incorrects dans l’Explorateur d’objets. Voir [Commentaires des utilisateurs sur SQL Server](https://feedback.azure.com/forums/908035/suggestions/37638472) pour plus de détails. |
| Explorateur d’objets | Résolution d’un problème de longue date où, quand l’utilisateur tentait de développer le nœud *Tables* dans l’Explorateur d’objets pour une base de données qui était configurée pour utiliser un classement qui n’était plus pris en charge par Windows, une erreur était déclenchée (et l’utilisateur ne pouvait plus développer ses tables). Korean_Wansung_Unicode_CI_AS est un exemple de ce type de classement. |
| [Inscrire des serveurs](register-servers/register-servers.md) | Résolution d’un problème où, lorsque l’utilisateur tentait d’exécuter une requête sur plusieurs serveurs (sous un *groupe* dans Serveurs inscrits) alors que les serveurs inscrits utilisaient l’option *Active Directory - Authentification intégrée* ou *Azure Active Directory - Authentification universelle avec MFA*, l’exécution était impossible car SSMS ne parvenait pas à se connecter. |
| [Inscrire des serveurs](register-servers/register-servers.md) | Résolution d’un problème où, lorsque l’utilisateur tentait d’exécuter une requête sur plusieurs serveurs (sous un *groupe* dans Serveurs inscrits) alors que les serveurs inscrits utilisaient l’option *Active Directory - Authentification par mot de passe* ou *Authentification SQL* et que l’utilisateur avait choisi de ne pas enregistrer le mot de passe, SSMS plantait. |
| Rapports | Résolution d’un problème lors duquel les rapports *Utilisation du disque* ne pouvaient pas être générés lorsque les fichiers de données comportaient de nombreuses étendues. |
| Outils de réplication | Résolution d’un problème où le moniteur de réplication ne fonctionnait pas avec la base de données de publication et le serveur de distribution du groupe de disponibilité (ce problème avait été résolu dans SSMS 17.x). |
| SQL Agent | Résolution d’un problème lors duquel l’ajout, l’insertion ou la suppression des étapes d’un travail entraînaient la réinitialisation du focus sur la première ligne au lieu de la ligne active. Voir [Commentaires des utilisateurs sur SQL Server](https://feedback.azure.com/forums/908035/suggestions/38070892) pour plus de détails. |
| SMO/Création de scripts | Résolution d’un problème où *CREATE OR ALTER* ne scriptait pas les objets qui avaient des propriétés étendues. Voir [Commentaires des utilisateurs sur SQL Server](https://feedback.azure.com/forums/908035-sql-server/suggestions/37236748) pour plus de détails. |
| SMO/Création de scripts | Résolution d’un problème où SSMS ne pouvait pas scripter correctement CREATE EXTERNAL LIBRARY. Voir [Commentaires des utilisateurs sur SQL Server](https://feedback.azure.com/forums/908035/suggestions/37868089) pour plus de détails. |
| SMO/Création de scripts | Résolution d’un problème où, lorsque l’utilisateur tentait d’utiliser l’option *Générer des scripts* sur une base de données contenant plusieurs milliers de tables, la boîte de dialogue de progression semblait figée. |
| SMO/Création de scripts | Résolution d’un problème où l’exécution d’un script sur une *table externe* sur SQL 2019 ne fonctionnait pas. |
| SMO/Création de scripts | Résolution d’un problème où l’exécution d’un script sur une *source de données externe* sur SQL 2019 ne fonctionnait pas. Voir [Commentaires des utilisateurs sur SQL Server](https://feedback.azure.com/forums/908035/suggestions/34295080) pour plus de détails. |
| SMO/Création de scripts | Résolution d’un problème où les *propriétés étendues* des colonnes n’étaient pas scriptées lors du ciblage d’Azure SQL Database. Pour plus d’informations, consultez [stackoverflow](https://stackoverflow.com/questions/56952337/how-can-i-script-the-descriptions-of-columns-in-ms-sql-server-management-studio). |
| SMO/Création de scripts | Insertion dans la dernière page : SMO - Ajout de la propriété *Index.IsOptimizedForSequentialKey* |
|**Configuration de SSMS**| **Atténuation d’un problème lors duquel le programme d’installation SSMS bloquait l’installation de SSMS en raison d’une non correspondance des langues. Cela peut être dû à des situations anormales, telles que l’abandon d’une installation ou la mauvaise désinstallation d’une version précédente de SSMS. Voir [Commentaires des utilisateurs sur SQL Server](https://feedback.azure.com/forums/908035/suggestions/37483594/) pour plus de détails.** |
| XEvent Profiler | Résolution du problème de plantage lors de la fermeture de la visionneuse. |

#### <a name="known-issues-182"></a>Problèmes connus (18.2)

- Un diagramme de base de données créé à partir d’une instance SSMS exécutée sur une machine A ne peut pas être modifié sur la machine B (car cela entraînerait le plantage de SSMS). Voir [Commentaires des utilisateurs sur SQL Server](https://feedback.azure.com/forums/908035/suggestions/37992649) pour plus de détails.

- Il y a des problèmes de regénération du dessin lors du basculement d’une fenêtre de requête vers une autre. Voir [Commentaires des utilisateurs sur SQL Server](https://feedback.azure.com/forums/908035/suggestions/37474042). Pour contourner ce problème, vous pouvez désactiver l’accélération matérielle sous **Outils** > **Options**.

- La taille des données affichées dans les résultats de SSMS au format grille, texte ou fichier est limitée.

- Il y a un problème lié à la réception d’une erreur lors de la suppression d’une base de données SQL Azure dans l’Explorateur d’objets, alors que cette opération s’effectue correctement.

- La boîte de dialogue Propriétés de la connexion peut indiquer que la langue par défaut pour les connexions SQL est l’arabe, quelle que soit la langue par défaut réelle définie pour la connexion. Pour afficher la langue par défaut réelle pour une connexion donnée, utilisez T-SQL afin de sélectionner la valeur **default_language_name** de la connexion dans **master.sys.server_principles**.

### <a name="181"></a>18.1

![Télécharger](media/download-icon.png) [Télécharger SSMS 18.1](https://go.microsoft.com/fwlink/?linkid=2094583)

- Numéro de version : 18.1
- Numéro de build : 15.0.18131.0
- Date de publication : 11 juin 2019

[Chinois (simplifié)](https://go.microsoft.com/fwlink/?linkid=2094583&clcid=0x804) | [Chinois (traditionnel)](https://go.microsoft.com/fwlink/?linkid=2094583&clcid=0x404) | [Anglais (États-Unis)](https://go.microsoft.com/fwlink/?linkid=2094583&clcid=0x409) | [Français](https://go.microsoft.com/fwlink/?linkid=2094583&clcid=0x40c) | [Allemand](https://go.microsoft.com/fwlink/?linkid=2094583&clcid=0x407) | [Italien](https://go.microsoft.com/fwlink/?linkid=2094583&clcid=0x410) | [Japonais](https://go.microsoft.com/fwlink/?linkid=2094583&clcid=0x411) | [Coréen](https://go.microsoft.com/fwlink/?linkid=2094583&clcid=0x412) | [Portugais (Brésil)](https://go.microsoft.com/fwlink/?linkid=2094583&clcid=0x416) | [Russe](https://go.microsoft.com/fwlink/?linkid=2094583&clcid=0x419) | [Espagnol](https://go.microsoft.com/fwlink/?linkid=2094583&clcid=0x40a)

#### <a name="whats-new-in-181"></a>Nouveautés de la version 18.1

| Nouvel élément | Détails |
| :-------| :------|
| Diagrammes de base de données | [Les diagrammes de base de données ont été rajoutés à SSMS](https://feedback.azure.com/forums/908035/suggestions/37507828).
| SSBDIAGNOSE.EXE |L’outil de ligne de commande SQL Server Diagnose a été rajouté au package SSMS.|
| Integration Services (SSIS) | Prise en charge de la planification du package SSIS, situé dans le catalogue SSIS dans Azure ou le système de fichiers, dans Azure. Il existe trois entrées pour le lancement de la boîte de dialogue Nouvelle planification, l’élément de menu *Nouvelle planification...* affiché lors d’un clic droit sur le package SSIS dans le catalogue SSIS au sein d’Azure, l’élément de menu *Planifier un package SSIS dans Azure* sous l’élément de menu *Migrer vers Azure* sous l’élément de menu *Outils* et « Planifier SSIS dans Azure » affiché lors d’un clic droit sur le dossier Travaux sous l’agent SQL Server de l’instance managée Azure SQL.|

#### <a name="bug-fixes-in-181"></a>Correctifs de bogues dans la version 18.1

| Nouvel élément | Détails |
|---------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Accessibilité | Amélioration de l’accessibilité de l’interface utilisateur du travail d’agent. |'
| Accessibilité | Amélioration de l’accessibilité dans la page du moniteur Stretch grâce à l’ajout d’un nom accessible pour le bouton *Actualisation automatique* et d’un nom accessible intelligent qui permet aux utilisateurs de savoir sur quel bouton ils se trouvent et ce qui va arriver s’ils appuient sur ce bouton. |
| Intégration d’ADS| Correction d’un plantage possible dans SSMS lors d’une tentative d’utilisation des serveurs inscrits à ADS.|
| Concepteur de bases de données | Ajout de la prise en charge du classement Latin1_General_100_BIN2_UTF8 (disponible dans SQL Server 2019 CTP3.0). |
| Classification des données | Interdiction d’ajouter des étiquettes de sensibilité aux colonnes dans la table d’historique, ce qui n’est pas pris en charge. |
| Classification des données | Résolution d’un problème lié à la gestion incorrecte du niveau de compatibilité (serveur/base de données). |
| Concepteur de bases de données | Ajout de la prise en charge du classement Latin1_General_100_BIN2_UTF8 (disponible dans SQL2019 CTP3.0). |
| SSMS général | Correction d’un problème entraînant la génération d’un script incorrect des colonnes pseudo dans un index. |
| SSMS général | Résolution d’un problème entraînant la levée d’une exception de référence null en cas de clic sur le bouton *Ajouter des informations d’identification* dans la page des propriétés de connexion. |
| SSMS général | Correction de l’affichage de la taille de la colonne Money dans la page des propriétés de l’index. |
| SSMS général | Correction d’un problème lié au fait que SSMS ne respectait pas les paramètres Intellisense provenant d’*Outils/Options* dans la fenêtre de l’éditeur SQL. |
| SSMS général | Correction d’un problème lié au fait que SSMS ne respectait pas les paramètres d’aide (en ligne/hors connexion). |
| Haute résolution | Correction de la disposition des contrôles dans les boîtes de dialogue d’erreur pour les options de requête non prises en charge. |
| Haute résolution | Correction de la disposition des contrôles dans la page *Nouveau groupe de disponibilité* sur certaines versions localisées de SSMS. |
| Haute résolution | Correction de la disposition de la page *Nouvelle planification du travail*. Voir [Commentaires des utilisateurs sur SQL Server](https://feedback.azure.com/forums/908035/suggestions/37632094) pour plus de détails. |
| Importer un fichier plat | Correction d’un problème pouvant entraîner la perte silencieuse de lignes durant l’importation.|
| IntelliSense/éditeur | Réduction du trafic de requêtes basées sur SMO vers Azure SQL Database pour IntelliSense. |
| IntelliSense/éditeur | Correction d’une erreur grammaticale dans l’info-bulle affichée lors de la saisie de T-SQL pour créer un utilisateur. Correction également du message d’erreur pour lever l’ambiguïté entre les utilisateurs et les connexions. |
| Visionneuse du journal | Correction d’un problème lié au fait que SSMS ouvre toujours le journal du serveur (ou de l’agent) actuel, même en cas de double-clic sur un ancien journal d’archivage dans l’Explorateur d’objets. Voir [Commentaires des utilisateurs sur SQL Server](https://feedback.azure.com/forums/908035/suggestions/37633648) pour plus de détails. |
| Installation de SSMS | Résolution du problème provoquant l’échec de l’installation de SSMS quand le chemin du journal d’installation contient des espaces. Voir [Commentaires des utilisateurs sur SQL Server](https://feedback.azure.com/forums/908035/suggestions/37496110) pour plus de détails. |
| Installation de SSMS | Correction d’un problème provoquant la fermeture de SSMS juste après l’affichage de l’écran de démarrage. </br> Pour plus d’informations, consultez les sites suivants : [Commentaires des utilisateurs sur SQL Server](https://feedback.azure.com/forums/908035/suggestions/37502512), [SSMS refuse de démarrer](https://dba.stackexchange.com/questions/238609/ssms-refuses-to-start) et [Administrateurs de base de données](https://dba.stackexchange.com/questions/237086/sql-server-management-studio-18-wont-open-only-splash-screen-pops-up). |
| Explorateur d’objets | Levée de la restriction sur l’activation de *start PowerShell* en cas de connexion à SQL sur Linux. |
| Explorateur d’objets | Correction d’un problème entraînant le plantage de SSMS lors d’une tentative d’extension du nœud du groupe Polybase/Groupe de scale-out (en cas de connexion à un nœud de calcul). |
| Explorateur d’objets | Correction d’un problème lié au fait que l’élément de menu *désactivé* était toujours activé, même après la désactivation d’un index donné. Voir [Commentaires des utilisateurs sur SQL Server](https://feedback.azure.com/forums/908035/suggestions/37735375) pour plus de détails. |
| Rapports | Correction du rapport pour afficher GrantedQueryMemory dans la base de connaissances (rapport de tableau de bord des performances SQL). Voir [Commentaires des utilisateurs sur SQL Server](https://feedback.azure.com/forums/908035/suggestions/37167289) pour plus de détails. |
| Rapports | Amélioration du suivi du bloc de journal dans les scénarios Always-On. |
| ShowPlan | Ajout d’un nouvel élément du plan d’exécution de requêtes *SpillOccurred* au schéma du plan d’exécution de requêtes. |
| ShowPlan | Ajout de lectures à distance (*ActualPageServerReads*, *ActualPageServerReadAheads*, *ActualLobPageServerReads*, *ActualLobPageServerReadAheads* ) au schéma du plan d’exécution. |
| SMO/création de scripts | Évitez d’interroger les contraintes d’arête durant la création de scripts pour les tables non graphiques. |
| SMO/création de scripts | Suppression de la contrainte sur la classification de sensibilité lors de la création de scripts de colonnes avec la *classification des données*. |
| SMO/création de scripts | Correction d’un problème entraînant l’échec de « Générer un script » sur une table de graphe durant la génération de données. Voir [Commentaires des utilisateurs sur SQL Server](https://feedback.azure.com/forums/908035-sql-server/suggestions/32898466) pour plus de détails. |
| SMO/création de scripts | Correction d’un problème empêchant à la méthode EnumObjects() de récupérer le nom de schéma d’un synonyme. |
| SMO/création de scripts | Correction d’un problème dans UIConnectionInfo.LoadFromStream() empêchant la lecture de la section *AdvancedOptions* (quand un mot de passe n’est pas spécifié). |
| SQL Agent | Correction d’un problème provoquant le plantage de SSMS lors de l’utilisation de la fenêtre de propriétés d’un travail. Voir [Commentaires des utilisateurs sur SQL Server](https://feedback.azure.com/forums/908035/suggestions/37164244) pour plus de détails. |
| SQL Agent | Correction d’un problème lié au fait que le bouton « Afficher » situé dans *Propriétés de l’étape du travail* n’était pas toujours activé, ce qui empêchait l’affichage de la sortie d’une étape de travail donnée. |
| Interface utilisateur XEvent | Ajout de la colonne « Package » à la liste XEvents pour distinguer les événements portant le même nom. |
| Interface utilisateur XEvent | Ajout du mappage de type de classe « EXTERNAL LIBRARY » manquant à XEventUI. |

#### <a name="known-issues-181"></a>Problèmes connus (18.1)

- Les utilisateurs peuvent voir une erreur lorsqu’ils font glisser un objet de table de l’Explorateur d’objets à l’éditeur de requête. Nous sommes au courant du problème et le correctif est prévu pour la prochaine version.

- Les options de couleur pour *Grouper les connexions* et *Connexions à un seul serveur* sous Options -> Éditeur de texte -> Onglet d’éditeur et barre d’état -> Couleurs et disposition de la barre d’état ne sont pas conservées après la fermeture de SSMS 18.1. Après avoir rouvert SSMS, l’option Couleurs et disposition de la barre d’état revient à sa définition par défaut (blanc).

- La taille des données affichées dans les résultats de SSMS au format grille, texte ou fichier est limitée.

- Le diagramme de base de données créé à partir de SSMS s’exécutant sur une machine A ne peut pas être modifié sur la machine B (SSMS se plante). Voir [Commentaires des utilisateurs sur SQL Server](https://feedback.azure.com/forums/908035/suggestions/37992649) pour plus de détails.

### <a name="180"></a>18.0

![Télécharger](media/download-icon.png) [Télécharger SSMS 18.0](https://go.microsoft.com/fwlink/?linkid=2088649)

- Numéro de version : 18.0  
- Numéro de build : 15.0.18118.0  
- Date de publication : 24 avril 2019

[Chinois (simplifié)](https://go.microsoft.com/fwlink/?linkid=2088649&clcid=0x804)| [Chinois (traditionnel)](https://go.microsoft.com/fwlink/?linkid=2088649&clcid=0x404)| [Anglais (États-Unis)](https://go.microsoft.com/fwlink/?linkid=2088649&clcid=0x409)| [Français](https://go.microsoft.com/fwlink/?linkid=2088649&clcid=0x40c)| [Allemand](https://go.microsoft.com/fwlink/?linkid=2088649&clcid=0x407)| [Italien](https://go.microsoft.com/fwlink/?linkid=2088649&clcid=0x410)| [Japonais](https://go.microsoft.com/fwlink/?linkid=2088649&clcid=0x411)| [Coréen](https://go.microsoft.com/fwlink/?linkid=2088649&clcid=0x412)| [Portugais (Brésil)](https://go.microsoft.com/fwlink/?linkid=2088649&clcid=0x416)| [Russe](https://go.microsoft.com/fwlink/?linkid=2088649&clcid=0x419)| [Espagnol](https://go.microsoft.com/fwlink/?linkid=2088649&clcid=0x40a)

#### <a name="whats-new-in-180"></a>Nouveautés de la version 18.0

| Nouvel élément| Détails|
| :-------| :------|
|Prise en charge de SQL Server 2019|SSMS 18.0 est la première version qui prend entièrement *en charge* SQL Server 2019 (compatLevel 150).|
|Prise en charge de SQL Server 2019|Prise en charge de « BATCH_STARTED_GROUP » et de « BATCH_COMPLETED_GROUP » dans SQL Server 2019 et SQL Database Managed Instance.|
|Prise en charge de SQL Server 2019|SMO : Ajout de la prise en charge pour l’incorporation (inlining) des fonctions UDF.|
|Prise en charge de SQL Server 2019|GraphDB : Ajout d’un indicateur dans le plan d’exécution de requêtes pour Graph TC Sequence.|
|Prise en charge de SQL Server 2019|Always Encrypted : ajout de la prise en charge d’AEv2/des enclaves.|
|Prise en charge de SQL Server 2019|Always Encrypted : la boîte de dialogue Connexion contient un nouvel onglet « Always Encrypted » quand l’utilisateur clique sur le bouton « Options » pour activer/configurer la prise en charge des enclaves.|
|Taille de téléchargement de SSMS inférieure| La taille actuelle est environ 500 Mo, environ la moitié de l’offre groupée SSMS 17.x.
|SSMS est basé sur le shell isolé Visual Studio 2017|Le nouvel interpréteur de commandes (SSMS est basé sur Visual Studio 2017 15.9.11) déverrouille tous les correctifs d’accessibilité qui ont été intégrés à SSMS et Visual Studio, et inclut les derniers correctifs de sécurité.|
|Améliorations apportées à l’accessibilité de SSMS| Un travail important a été réalisé pour faire face aux problèmes d’accessibilité dans tous les outils (SSMS, DTA et Profiler)|
|SSMS peut désormais être installé dans un dossier personnalisé| Cette option est disponible depuis l’interface de ligne de commande (utile pour l’installation sans assistance) et l’interface utilisateur d’installation. En utilisant la ligne de commande, transmettez cet argument supplémentaire à SSMS-Setup-ENU.exe :   SSMSInstallRoot=C:\MySSMS18  Par défaut, le nouvel emplacement d’installation de SSMS est : %ProgramFiles(x86)%\Microsoft SQL Server Management Studio 18\Common7\IDE\ssms.exe.  Cela ne signifie pas que SSMS est à instances multiples.|
|SSMS permet l’installation dans une langue autre que celle du système d’exploitation|Le blocage sur l’installation en plusieurs langues a été levé. Vous pouvez, par exemple, installer SSMS en allemand sur un Windows français. Si la langue du système d’exploitation ne correspond pas à la langue de SSMS, l’utilisateur doit modifier la langue sous **Outils** > **Options** > **Paramètres internationaux** ; sinon, SSMS affiche l’interface utilisateur en anglais.|
|SSMS ne partage plus de composants avec le moteur SQL|Beaucoup d’efforts ont été déployés pour éviter le partage de composants avec le moteur SQL, qui avait souvent pour effet de nuire à la facilité de gestion, compte tenu du fait que l’un écrasait les fichiers installés par l’autre.|
|SSMS nécessite NetFx 4.7.2 ou une version ultérieure|Nous avons mis à niveau notre configuration minimale requise de NetFx4.6.1 à NetFx4.7.2 : cela nous permet de tirer parti des nouvelles fonctionnalités exposées par la nouvelle infrastructure.|
|Possibilité de migrer les paramètres de SSMS| Au premier démarrage de SSMS 18, l’utilisateur est invité à migrer les paramètres de la version 17.x. Les fichiers de paramètres utilisateur sont maintenant stockés comme un fichier XML simple, ce qui améliore la portabilité et permet éventuellement la modification.|
|Prise en charge des résolutions élevées| La prise en charge des résolutions élevées est désormais activée par défaut.|
|SSMS est livré avec le pilote Microsoft OLE DB| Pour plus d’informations, consultez [Télécharger Microsoft OLE DB Driver pour SQL Server](../connect/oledb/download-oledb-driver-for-sql-server.md).|
|SSMS n’est pas pris en charge sur Windows 8. Windows 10 et Windows Server 2016 nécessitent la version 1607 (10.0.14393) ou ultérieure|Compte tenu de la nouvelle dépendance vis-à-vis de NetFx 4.7.2, SSMS 18.0 ne peut pas être installé sur Windows 8, les anciennes versions de Windows 10 et Windows Server 2016. Le programme d’installation de SSMS bloque ces systèmes. Windows 8.1 est toujours pris en charge.|
|SSMS n’est plus ajouté à la variable d’environnement PATH|Le chemin de SSMS.EXE (et des outils en général) n’est plus ajouté au chemin. Les utilisateurs peuvent l’ajouter manuellement ou, s’ils utilisent une version moderne de Windows, passer par le menu Démarrer.|
|Les ID de package ne sont plus nécessaires pour développer des extensions SSMS| Avant, SSMS chargeait de façon sélective uniquement les packages connus, ce qui contraignait les développeurs à inscrire leur propre package. Cela n’est plus le cas.|
|SSMS général|Exposition de l’option de configuration AUTOGROW_ALL_FILES pour les groupes de fichiers dans SSMS.|
|SSMS général|Suppression des options risquées « regroupement léger » et « renforcement de priorité » de l’interface graphique utilisateur de SSMS. Pour plus d’informations, consultez l’article [Priority boost details – and why it’s not recommended](https://deep.data.blog/2010/01/26/priority-boost-details-and-why-its-not-recommended/) (Détails sur le renforcement de priorité – et pourquoi ce n’est pas recommandé. Texte en anglais).
|SSMS général|Nouveau menu et nouvelles combinaisons de touches pour la création de fichiers : **CTRL+ALT+N**. **Ctrl+N** permet toujours de créer une requête.|
|SSMS général|La boîte de dialogue **Nouvelle règle de pare-feu** permet désormais à l’utilisateur de spécifier un nom de règle, au lieu d’en générer un automatiquement pour le compte de l’utilisateur.|
|SSMS général|Amélioration d’IntelliSense dans l’éditeur, en particulier pour T-SQL v140 et ultérieur.|
|SSMS général|Ajout de la prise en charge d’UTF-8 dans boîte de dialogue de classement de l’interface utilisateur de SSMS.|
|SSMS général|Adoption du « Gestionnaire des informations d’identification Windows » pour les mots de passe utilisés récemment dans la boîte de dialogue de connexion. Cela résout le problème de longue date lié à la persistance des mots de passe, qui n’était pas toujours fiable.|
|SSMS général|Prise en charge améliorée des systèmes multimoniteurs en faisant en sorte que de plus en plus de boîtes de dialogue et de fenêtres s’affichent sur le moniteur approprié.|
|SSMS général|Exposition de « paramètre par défaut de la somme de contrôle de sauvegarde » de la configuration du serveur dans la nouvelle page Paramètres de la base de données de la boîte de dialogue Propriétés du serveur. Pour plus d’informations, consultez [https://feedback.azure.com/forums/08035-sql-server/suggestions/34634974](https://feedback.azure.com/forums/08035-sql-server/suggestions/34634974).|
|SSMS général|Exposition de la taille maximale des fichiers journaux des erreurs sous « Configurer les journaux d’erreurs SQL Server ». Pour plus d’informations, consultez [https://feedback.azure.com/forums/908035/suggestions/33624115](https://feedback.azure.com/forums/908035/suggestions/33624115).|
|SSMS général|Ajout de « Migrer vers Azure » sous le menu Outils : nous avons intégré l’Assistant Migration de base de données et Azure Database Migration Service pour fournir un accès simple et rapide afin d’accélérer vos migrations vers Azure.|
|SSMS général|Ajout d’une logique pour inviter l’utilisateur à valider les transactions ouvertes quand l’option « Modifier la connexion » est utilisée.|
|Intégration d’Azure Data Studio|Ajout d’un élément de menu pour démarrer/télécharger Azure Data Studio.|
|Intégration d’Azure Data Studio|Ajout de l’élément de menu « Démarrer Azure Data Studio » à l’Explorateur d’objets.|
|Intégration d’Azure Data Studio|Après un clic droit sur un nœud de base de données dans l’Explorateur d’objets, des menus contextuels s’affichent pour l’utilisateur afin d’exécuter une requête ou créer un nouveau notebook dans Azure Data Studio.|
|Prise en charge d’Azure SQL| Les propriétés de base de données SLO/Edition/MaxSize acceptent désormais les noms personnalisés, ce qui facilite la prise en charge des futures éditions d’Azure SQL Database.|
|Prise en charge d’Azure SQL| Ajout de la prise en charge des références SKU vCore (Usage général et Critique pour l’entreprise) : Gen4_24 et Gen5 en intégralité.|
|Azure SQL Managed Instance|Ajout de « Connexions Azure AD » comme nouveau type de connexion dans SMO et SSMS quand connexion à Azure SQL Managed Instance est établie.|
|Always On|Rehachage du temps de récupération estimé (RTO) et de la perte de données estimée (RPO) dans le tableau de bord Always On de SSMS. Consultez la documentation mise à jour à l’adresse [https://docs.microsoft.com/sql/database-engine/availability-groups/windows/monitor-performance-for-always-on-availability-groups](../database-engine/availability-groups/windows/monitor-performance-for-always-on-availability-groups.md).|
|Always Encrypted| La case à cocher Activer Always Encrypted sous le nouvel onglet Always Encrypted de la boîte de dialogue Se connecter au serveur offre désormais un moyen simple d’activer/de désactiver Always Encrypted pour une connexion de base de données.|
|Always Encrypted avec enclaves sécurisées| Plusieurs améliorations ont été apportées pour prendre en charge Always Encrypted avec enclaves sécurisées dans SQL Server 2019 :  Champ de texte permettant de spécifier l’URL d’attestation d’enclave dans la boîte de dialogue Se connecter au serveur (sous le nouvel onglet Always Encrypted).  Nouvelle case à cocher dans la boîte de dialogue Nouvelle clé principale de colonne permettant d’indiquer si une nouvelle clé principale de colonne doit autoriser les calculs d’enclave.  D’autres boîtes de dialogue de gestion de clés Always Encrypted exposent désormais des informations indiquant sur quelles clés principales de colonne les calculs d’enclave sont autorisés.|
|Fichiers d’audit|Abandon de la méthode d’authentification basée sur la clé du compte de stockage au profit de l’authentification basée sur Azure AD.|
|Classification des données| Réorganisation du menu des tâches de classification des données : ajout d’un sous-menu au menu des tâches de base de données et ajout d’une option pour ouvrir le rapport à partir du menu sans ouvrir au préalable la fenêtre de classification des données.|
|Classification des données|Ajout d’une nouvelle fonctionnalité de « classification des données » à SMO. L’objet Column expose de nouvelles propriétés : SensitivityLabelName, SensitivityLabelId, SensitivityInformationTypeName, SensitivityInformationTypeId et IsClassified (en lecture seule). Pour plus d’informations, consultez [ADD SENSITIVITY CLASSIFICATION (Transact-SQL)](../t-sql/statements/add-sensitivity-classification-transact-sql.md)|
|Classification des données|Ajout d’un nouvel élément de menu « Rapport de classification » au menu volant « Classification des données ».|
|Classification des données| Recommandations mises à jour.|
|Mise à niveau du niveau de compatibilité de la base de données|Ajout d’une nouvelle option sous **_Nom de la base de données_ *_ > _* _Tâches_ *_ > _* _Mise à niveau de la base de données_ *_. Celle-ci démarre le nouvel _* Assistant Paramétrage de requêtes** pour guider l’utilisateur dans le processus suivant : Collecte d’une ligne de base de performances avant la mise à niveau du niveau de compatibilité de la base de données. Mise à niveau vers le niveau de compatibilité de la base de données souhaité.  Collecte d’un deuxième passage de données de performances sur la même charge de travail. Détection des régressions de la charge de travail et indication de recommandations testées pour améliorer les performances de la charge de travail.  Ce processus est semblable au processus de mise à niveau de base de données documenté dans la rubrique [Scénarios d’utilisation du Magasin des requêtes](../relational-databases/performance/query-store-usage-scenarios.md#CEUpgrade), à l’exception de la dernière étape où l’Assistant Paramétrage de requêtes ne s’appuie pas sur un état valide antérieur connu pour générer des recommandations.|
|Assistant Application de la couche Données|Ajout de la prise en charge de l’importation/exportation de l’application de la couche Données avec des tables de graphe.|
|Assistant Importation d’un fichier plat|Ajout d’une logique pour notifier l’utilisateur que l’importation peut avoir abouti à un renommage des colonnes.|
|Integration Services (SSIS)|Les clients peuvent désormais planifier des packages SSIS sur les runtimes d’intégration Azure-SSIS qui se trouvent dans le cloud Azure Government.|
|Integration Services (SSIS)|Quand vous utilisez SQL Agent d’Azure SQL Managed Instance par SSMS, vous pouvez configurer les paramètres et le Gestionnaire des connexions à l’étape de travail de l’agent SSIS.|
|Integration Services (SSIS)|Quand vous vous connectez à Azure SQL Database/Azure SQL Managed Instance, vous pouvez le faire avec la base de données initiale *par défaut*.|
|Integration Services (SSIS)|Ajout d’un nouvel élément d’entrée **Essayer SSIS dans Azure Data Factory** sous le nœud « Catalogues Integration Services », utilisable pour lancer l’« Assistant Création d’Integration Runtime » et créer rapidement « Azure-SSIS Integration Runtime ».
|Integration Services (SSIS)|Ajout du bouton **Créer SSIS IR** dans l’Assistant Création de catalogue, utilisable pour lancer l’Assistant Création d’Integration Runtime et créer rapidement Azure-SSIS Integration Runtime.|
|Integration Services (SSIS)|ISDeploymentWizard prend désormais en charge l’authentification SQL, l’authentification intégrée à Azure Active Directory et l’authentification par mot de passe Azure Active Directory en mode ligne de commande.|
|Integration Services (SSIS)|L’Assistant de déploiement prend désormais en charge la création et le déploiement sur le Runtime d’intégration SSIS d’Azure Data Factory.|
|Scripts d’objets|Ajout de nouveaux éléments de menu pour les instructions « CREATE ou ALTER » pendant la génération d’un script pour objets.|
|Magasin des requêtes|Amélioration de la facilité d’utilisation de certains rapports (Consommation globale des ressources) avec l’ajout d’un séparateur de milliers aux nombres affichés sur l’axe des ordonnées des graphiques.|
|Magasin des requêtes|Ajout d’un nouveau rapport Statistiques d’attente des requêtes.|
|Magasin des requêtes|Ajout de la métrique « Nombre d’exécutions » à l’affichage « Requête suivie ».|
|Outils de réplication|Ajout du support de la fonctionnalité de spécification de port non-définie par défaut dans le moniteur de réplication et SSMS.|
|ShowPlan|Ajout du temps écoulé réel, des lignes réelles/estimées sous le nœud d’opérateur de plan d’exécution de requêtes, le cas échéant. L’aspect du plan réel est donc cohérent avec celui du plan Statistiques des requêtes en direct.|
|ShowPlan|Modification de l’info-bulle et ajout d’un commentaire quand l’utilisateur clique sur le bouton Modifier la requête pour un plan d’exécution de requêtes, l’objectif étant d’indiquer à l’utilisateur que le plan d’exécution de requêtes risque d’être tronqué par le moteur SQL si la requête comporte plus de 4 000 caractères.|
|ShowPlan|Ajout de la logique pour l’affichage de « Materializer Operator (External Select) ».|
|ShowPlan|Ajout d’un nouvel attribut de plan d’exécution de requêtes « BatchModeOnRowStoreUSed » pour identifier facilement les requêtes qui utilisent la fonctionnalité « analyse en mode batch sur les rowstores ». Chaque fois qu’une requête effectue une analyse en mode batch sur des rowstores, un nouvel attribut (BatchModeOnRowStoreUsed="true") est ajouté à l’élément StmtSimple.|
|ShowPlan|Ajout de la prise en charge du plan d’exécution de requêtes à LocalCube RelOp pour DW ROLLUP et CUBE.|
|ShowPlan|Nouvel opérateur LocalCube pour les nouvelles fonctionnalités d’agrégation ROLLUP et CUBE dans Azure Synapse Analytics.|
|SMO| Prise en charge étendue de SMO pour la création d’index reprenables.|
|SMO| Ajout d’un nouvel événement sur les objets SMO (« PropertyMissing ») pour aider les créateurs d’applications à détecter plus tôt les problèmes de performances de SMO.|
|SMO| Exposition de la nouvelle propriété DefaultBackupChecksum sur l’objet Configuration qui est mappée au « paramètre par défaut de la somme de contrôle de sauvegarde » dans la configuration du serveur.|
|SMO| Exposition de la nouvelle propriété ProductUpdateLevel sur l’objet Server, qui correspond au niveau de maintenance de la version de SQL en cours d’utilisation (par exemple, CU12, RTM).|
|SMO| Exposition de la nouvelle propriété LastGoodCheckDbTime pour l’objet Database, qui correspond à la propriété de base de données « lastgoodcheckdbtime ». Si cette propriété n’est pas disponible, la valeur par défaut 01/01/1900 00:00:00 est retournée.|
|SMO|Déplacement de l’emplacement du fichier RegSrvr.xml (fichier de configuration de serveur inscrit) vers « %AppData%\Microsoft\SQL Server Management Studio » (sans version ; il peut donc être partagé entre les versions de SSMS).|
|SMO|Ajout d’un « témoin cloud » comme nouveau type de quorum et nouveau type de ressource.|
|SMO|Ajout de la prise en charge des « contraintes d’arête » dans SMO et SSMS.|
|SMO|Ajout de la prise en charge de la suppression en cascade aux « contraintes d’arête » dans SMO et SSMS.|
|SMO|Ajout de la prise en charge des autorisations de « lecture/écriture » pour la classification de données.|
|Évaluation des vulnérabilités| Activation du menu de tâches Évaluation des vulnérabilités sur Azure SQL DW.|
|Évaluation des vulnérabilités|Changement de l’ensemble des règles d’évaluation des vulnérabilités exécutées sur Azure SQL Managed Instance, pour que les résultats d’analyse de l’évaluation des vulnérabilités soient cohérents avec ceux d’Azure SQL Database.|
|Évaluation des vulnérabilités| L’évaluation des vulnérabilités prend désormais en charge Azure SQL DW.|
|Évaluation des vulnérabilités|Ajout d’une nouvelle fonctionnalité d’exportation permettant d’exporter les résultats d’analyse de l’évaluation des vulnérabilités dans Excel.|
|Observateur XEvent|Dans l’observateur XEvent, activation d’une fenêtre de plan d’exécution de requêtes pour plus d’événements XEvent.|

#### <a name="bug-fixes-in-180"></a>Correctifs de bogues dans la version 18.0

| Nouvel élément | Détails|
|----------|--------|
|Se bloque et gèle|Correction d’une source courante d’incidents SSMS liés aux objets GDI.|
|Se bloque et gèle|Correction d’une source courante de blocages et de performances médiocres quand « Générer un script en tant que Créer/Mettre à jour/Supprimer » est sélectionné (les objets SMO ne sont plus récupérés inutilement).|
|Se bloque et gèle|Correction d’un problème dans lequel le système cesse de répondre lors de la connexion à une base de données Azure SQL Database avec MFA quand les traces ADAL sont activées.|
|Se bloque et gèle|Correction d’un problème dans lequel le système cessait de répondre (ou semblait ne plus répondre) pendant l’appel aux Statistiques des requêtes en direct à partir du Moniteur d’activité (le problème se manifestait quand l’authentification SQL Server était utilisée sans que « Persist Security Info » soit défini).|
|Se bloque et gèle|Correction d’un problème dans lequel le système cessait de répondre quand l’utilisateur sélectionnait « Rapports » dans l’Explorateur d’objets avec des connexions à latence élevée ou en cas de non-accessibilité temporaire des ressources.|
|Se bloque et gèle|Correction d’un plantage dans SSMS lié à l’utilisation du serveur de gestion centralisée et des serveurs Azure SQL. Pour plus d’informations, consultez [SSMS 17.5 application error + crash when using Central Management Server](https://feedback.azure.com/forums/908035/suggestions/33374884) (Erreur d’application SSMS et plantage lié à l’utilisation du serveur de gestion centralisée).|
|Se bloque et gèle|Correction d’un problème dans lequel le système cessait de répondre dans l’Explorateur d’objets en optimisant la façon dont la propriété IsFullTextEnabled est récupérée.|
|Se bloque et gèle|Correction d’un problème dans lequel le système cessait de répondre dans l’Assistant Copie de base de données en évitant de générer des requêtes inutiles pour récupérer les propriétés de la base de données.|
|Se bloque et gèle|Correction d’un problème dans lequel SSMS cessait de répondre/se bloquait lors de la modification de T-SQL.|
|Se bloque et gèle|Atténuation un problème où SSMS cessait de répondre lors de la modification de scripts T-SQL volumineux.|
|Se bloque et gèle|Correction d’un problème qui provoquait une mémoire insuffisante du SSMS durant la gestion des jeux de données volumineux retournés par les requêtes.|
|SSMS général|Correction d’un problème empêchant la communication du paramètre « ApplicationIntent » dans les connexions des « Serveurs inscrits ».|
|SSMS général|Correction d’un problème lié au rendu incorrect du formulaire de la « nouvelle interface utilisateur de l’Assistant Session XEvent » sur des moniteurs haute résolution.|
|SSMS général|Correction d’un problème lors de l’import d’un fichier bacpac.|
|SSMS général|Correction d’un problème, à la suite d’une tentative d’affichage des propriétés d’une base de données (avec FILEGROWTH > 2048 GB), qui générait une erreur de débordement arithmétique.|
|SSMS général|Correction du problème selon lequel les Rapports Tableau de bord Performances signalaient des attentes PAGELATCH et PAGEIOLATCH introuvables dans les sous-rapports.|
|SSMS général|Autre série de correctifs pour une meilleure prise en charge des écrans multiples par SSMS, en lui faisant ouvrir une boîte de dialogue dans le moniteur approprié.|
|Analysis Services (AS)|Correction d’un problème lié à l’interface utilisateur XEvent AS où les « Paramètres avancés » étaient rognés.|
|Analysis Services (AS)|Correction d’un problème qui entraînait la levée d’une exception de fichier introuvable par une analyse DAX.|
|Azure SQL Database|Correction d’un problème lié au remplissage incorrect de la liste de bases de données pour la fenêtre de requête Azure SQL Database en cas de connexion à une base de données utilisateur dans Azure SQL Database et non à MASTER.|
|Azure SQL Database|Correction d’un problème empêchant l’ajout d’une « Table temporelle » à une base de données Azure SQL.|
|Azure SQL Database|Activation de l’option de sous-menu de propriétés Statistiques sous le menu Statistiques dans Azure, car elle est entièrement prise en charge depuis un certain temps maintenant.|
|Azure SQL - Prise en charge générale|Résolution des problèmes dans un contrôle commun de l’interface utilisateur Azure qui empêchait l’utilisateur d’afficher des abonnements Azure (au-delà de 50). Par ailleurs, le tri a été modifié pour porter sur le nom plutôt que sur l’ID d’abonnement. L’utilisateur pouvait rencontrer ce problème quand il essayait de restaurer une sauvegarde à partir d’une URL, par exemple.|
|Azure SQL - Prise en charge générale|Correction d’un problème dans un contrôle commun de l’interface utilisateur Azure à l’occasion de l’énumération des abonnements qui pouvait générer une erreur « L’index était hors limites. Il ne doit pas être négatif et sa taille doit être inférieure à celle de la collection » quand l’utilisateur ne disposait pas d’abonnements dans certains locataires. L’utilisateur pouvait rencontrer ce problème quand il essayait de restaurer une sauvegarde à partir d’une URL, par exemple.|
|Azure SQL - Prise en charge générale|Correction du problème selon lequel les objectifs de niveau de service étaient codés en dur, ce qui compliquait la prise en charge des nouveaux objectifs Azure SQL par SSMS. Désormais, les utilisateurs peuvent se connecter à Azure et autoriser SSMS à récupérer toutes les données SLO applicables (Édition et Taille maximale).|
|Prise en charge d’Azure SQL Managed Instance|Amélioration/perfectionnement de la prise en charge des instances managées : désactivation des options non prises en charge dans l’interface utilisateur et correction de l’option Afficher les journaux d’audit pour gérer la cible d’audit d’URL.|
|Prise en charge d’Azure SQL Managed Instance|L’Assistant « Génération et publication de scripts » génère un script pour les clauses CREATE DATABASE non prises en charge.|
|Prise en charge d’Azure SQL Managed Instance|Active les Statistiques des requêtes en direct pour les instances managées.|
|Prise en charge d’Azure SQL Managed Instance|Propriétés de la base de données -> Fichiers générait un script incorrect pour ALTER DB ADD FILE.|
|Prise en charge d’Azure SQL Managed Instance|Correction de la régression au niveau du planificateur de l’Agent SQL qui choisissait la planification ONIDLE même quand un autre type de planification était choisi.|
|Prise en charge d’Azure SQL Managed Instance|Ajustement de MAXTRANSFERRATE, MAXBLOCKSIZE pour la création de sauvegardes dans Stockage Azure.|
|Prise en charge d’Azure SQL Managed Instance|Correction d’un problème lié à la génération d’un script pour la sauvegarde de la fin du journal avant l’opération RESTORE (cela n’est pas pris en charge par CL).|
|Prise en charge d’Azure SQL Managed Instance|Correction d’un problème lié à l’Assistant Création d’une base de données qui ne générait pas de script correct pour l’instruction CREATE DATABASE.|
|Prise en charge d’Azure SQL Managed Instance|Traitement spécial des packages SSIS dans SSMS lors des connexions à des instances managées.|
|Prise en charge d’Azure SQL Managed Instance|Correction d’un problème entraînant l’affichage d’une erreur quand le « Moniteur d’activité » était utilisé en étant connecté à des instances managées.|
|Prise en charge d’Azure SQL Managed Instance|Amélioration de la prise en charge des connexions Azure AD (dans l’Explorateur SSMS).|
|Prise en charge d’Azure SQL Managed Instance|Amélioration de la génération de script des objets de groupes de fichiers SMO.|
|Prise en charge d’Azure SQL Managed Instance|Amélioration de l’interface utilisateur des informations d’identification.|
|Prise en charge d’Azure SQL Managed Instance|Ajout de la prise en charge de la réplication logique.|
|Prise en charge d’Azure SQL Managed Instance|Correction d’un problème à l’origine de l’échec du clic droit sur une base de données et du choix de l’option « Importer une application de la couche Données ».|
|Prise en charge d’Azure SQL Managed Instance|Correction d’un problème à l’origine de l’affichage d’erreurs en cas de clic droit sur un « TempDB ».|
|Prise en charge d’Azure SQL Managed Instance|Correction d’un problème lié à l’écriture de scripts pour l’instruction ALTER DB ADD FILE dans SMO, à l’origine de la génération d’un script T-SQL vide.|
|Prise en charge d’Azure SQL Managed Instance|Amélioration de l’affichage des propriétés spécifiques au serveur des instances managées (génération de matériel, niveau de service, stockage utilisé et réservé).|
|Prise en charge d’Azure SQL Managed Instance|Correction d’un problème lié à l’écriture de scripts d’une base de données (« Script comme créer... ») qui ne créait pas de script sur les groupes de fichiers et fichiers supplémentaires. Pour plus d’informations, consultez [https://feedback.azure.com/forums/908035/suggestions/37326799](https://feedback.azure.com/forums/908035/suggestions/37326799). |
|Sauvegarde/restauration/attachement/détachement de base de données|Correction d’un problème qui empêchait l’utilisateur d’attacher une base de données quand le nom de fichier physique du fichier .mdf ne correspondait pas au nom de fichier d’origine.|
|Sauvegarde/restauration/attachement/détachement de base de données|Correction d’un problème empêchant SSMS de trouver un plan de restauration valide ou qui en trouvait un non optimal. Pour plus d’informations, consultez [https://feedback.azure.com/forums/908035-sql-server/suggestions/32897752](https://feedback.azure.com/forums/908035-sql-server/suggestions/32897752). |
|Sauvegarde/restauration/attachement/détachement de base de données|Correction d’un problème empêchant l’Assistant Attachement de base de données d’afficher les fichiers secondaires renommés. À présent, le fichier s’affiche et un commentaire le concernant est ajouté (par exemple « introuvable »). Pour plus d’informations, consultez [https://feedback.azure.com/forums/908035/suggestions/32897434](https://feedback.azure.com/forums/908035/suggestions/32897434). |
|Assistant Copie de base de données|Générer des scripts/Transférer/Assistant Copie de base de données : la tentative de création d’une table avec une table en mémoire ne force pas l’option ansi_padding on.|
|Assistant Copie de base de données|Interruption de Tâche de transfert de bases de données/Assistant Copie de base de données sur SQL Server 2017 et SQL Server 2019.|""
|Assistant Copie de base de données|Générer des scripts/Transférer/Assistant Copie de base de données : création d’une table de script avant la création d’une source de données externe associée.|
|Boîte de dialogue Connexion|Possibilité de supprimer des noms d’utilisateurs de la liste de noms d’utilisateurs précédente en appuyant sur la touche Suppr. Pour plus d’informations, consultez [Allow deletion of users from SSMS login window](https://feedback.azure.com/forums/908035/suggestions/32897632) (Autoriser la suppression d’utilisateurs depuis la fenêtre de connexion SSMS).|
|Assistant Importation de DAC|Correction d’un problème empêchant l’Assistant Importation de DAC de fonctionner avec une connexion utilisant Azure Active Directory (Azure AD).|
|Classification des données|Correction d’un problème lors de l’enregistrement des classifications dans le volet de classification des données alors que d’autres volets de classification des données étaient ouverts sur d’autres bases de données.|
|Assistant Application de la couche Données|Correction d’un problème empêchant l’utilisateur d’importer une application de la couche Données (.dacpac) en raison d’un accès limité au serveur (par exemple, aucun accès à toutes les bases de données se trouvant sur le même serveur).|
|Assistant Application de la couche Données|Correction d’un problème qui ralentissait considérablement l’importation quand de nombreuses bases de données étaient hébergées sur le même serveur SQL Azure.|
|Tables externes|Ajout de la prise en charge de Rejected_Row_Location dans le modèle, SMO, IntelliSense et la grille de propriétés.|
|Assistant Importation d’un fichier plat|Correction d’un problème empêchant l’Assistant Importation de fichier plat de gérer correctement les guillemets doubles (échappement). Pour plus d’informations, consultez [https://feedback.azure.com/forums/908035/suggestions/32897998](https://feedback.azure.com/forums/908035/suggestions/32897998). |
|Assistant Importation d’un fichier plat|Correction d’un problème lié à la gestion incorrecte des types virgule flottante (dans les paramètres régionaux qui utilisent un séparateur différent pour les virgules flottantes).|
|Assistant Importation d’un fichier plat|Correction d’un problème lié à l’importation de bits quand les valeurs sont 0 ou 1. Pour plus d’informations, consultez [https://feedback.azure.com/forums/908035-sql-server/suggestions/32898535](https://feedback.azure.com/forums/908035-sql-server/suggestions/32898535). |
|Assistant Importation d’un fichier plat|Résolution d’un problème lié aux données de type *float* qui étaient entrées comme *valeurs Null*.|
|Assistant Importation d’un fichier plat|Correction d’un problème empêchant l’Assistant Importation de traiter des valeurs décimales négatives.|
|Assistant Importation d’un fichier plat|Correction d’un problème empêchant l’Assistant d’effectuer l’importation à partir de fichiers CSV à colonne unique.|
|Assistant Importation d’un fichier plat|Correction du problème où l’Assistant Importation d’un fichier plat n’autorisait pas le changement de table de destination quand une table existait déjà. Pour plus d’informations, consultez [https://feedback.azure.com/forums/908035-sql-server/suggestions/32896186](https://feedback.azure.com/forums/908035-sql-server/suggestions/32896186). |
|Visionneuse de l’aide|Amélioration de la logique autour de la prise en compte des modes en ligne/hors connexion (il peut rester quelques problèmes nécessitant d’être traités).|
|Visionneuse de l’aide|Correction de l’option « Afficher l’aide » pour la prise en compte des paramètres en ligne/hors connexion. Pour plus d’informations, consultez [https://feedback.azure.com/forums/908035-sql-server/suggestions/32897791](https://feedback.azure.com/forums/908035-sql-server/suggestions/32897791). |
|Haute disponibilité et reprise d’activité après sinistre (HADR)<BR> Groupes de disponibilité (AG)|Correction d’un problème lié à l’affichage systématique des rôles dans l’« Assistant de basculement de groupes de disponibilité » en tant que « Résolution ».|
|Haute disponibilité et reprise d’activité après sinistre (HADR)<BR> Groupes de disponibilité (AG)|Correction d’un problème lié au fait que SSMS affichait des avertissements tronqués dans le tableau de bord Groupe de disponibilité.|
|Integration Services (IS)|Correction d’un problème SxS qui empêche l’Assistant Déploiement de se connecter à SQL Server quand SQL Server 2019 et SSMS 18.0 sont installés sur la même machine.|
|Integration Services (IS)|Correction d’un problème qui empêche la modification d’une tâche de plan de maintenance durant la conception du plan de maintenance.|
|Integration Services (IS)|Correction d’un problème qui entraînait le blocage de l’Assistant Déploiement quand le projet en cours de déploiement était renommé.|
|Integration Services (IS)|Activation du paramètre d’environnement dans la fonctionnalité de planification d’Azure-SSIS IR.|
|Integration Services (IS)|Correction d’un problème lié au fait que l’Assistant Création de SSIS Integration Runtime cesse de répondre quand le compte client appartient à plus d’un locataire.|
|Moniteur d'activité des travaux|Résolution d’un incident pendant l’utilisation du moniteur d’activité des travaux (avec des filtres).|
|Explorateur d’objets|Correction d’un problème lié à SSMS qui levait une exception « l’objet ne peut pas être converti du type DBNull en d’autres types » quand le nœud « Gestion » était développé dans l’Explorateur d’objets (DataCollector mal configuré).|
|Explorateur d’objets|Correction d’un problème empêchant l’échappement des guillemets dans l’Explorateur d’objets avant l’appel de « Modifier les N premières... », source de déroute les concepteurs.|
|Explorateur d’objets|Correction d’un problème qui empêchait le lancement de l’Assistant « Importation de l’application de la couche Données » à partir de l’arborescence de Stockage Azure.|
|Explorateur d’objets|Correction d’un problème dans « Configuration de Database Mail » où l’état de la case à cocher TLS/SSL n’était pas persistant. Pour plus d’informations, consultez [https://feedback.azure.com/forums/908035-sql-server/suggestions/32895541](https://feedback.azure.com/forums/908035-sql-server/suggestions/32895541). |
|Explorateur d’objets|Correction d’un problème lié à SSMS qui grisait l’option permettant de fermer les connexions existantes à l’occasion d’une restauration de base de données avec is_auto_update_stats_async_on.|
|Explorateur d’objets|Correction d’un problème qui se manifestait quand l’utilisateur faisait un clic droit sur un nœud dans l’Explorateur d’objets (par exemple, « Tables ») et qu’il voulait effectuer une action comme filtrer les tables en accédant à Filtrer > Paramètres de filtre ; le formulaire Paramètres de filtre pouvait s’afficher sur un autre écran que celui où SSMS était actif. Pour plus d’informations, consultez [https://feedback.azure.com/forums/908035-sql-server/suggestions/34284106](https://feedback.azure.com/forums/908035-sql-server/suggestions/34284106). |
|Explorateur d’objets|Correction d’un problème de longue date empêchant le fonctionnement de la touche Suppr dans l’Explorateur d’objets en cas de renommage d’un objet. Pour plus d’informations, consultez [https://feedback.azure.com/forums/908035-sql-server/suggestions/33073510](https://feedback.azure.com/forums/908035-sql-server/suggestions/33073510), [https://feedback.azure.com/forums/908035/suggestions/32910247](https://feedback.azure.com/forums/908035/suggestions/32910247) et autres doublons.|
|Explorateur d’objets|Au moment d’afficher les propriétés de fichiers de base de données existants, la taille apparaît sous une colonne « Taille (Mo) » au lieu de « Taille initiale (Mo) » qui s’affiche lors de la création d’une base de données. Pour plus d’informations, consultez [https://feedback.azure.com/forums/908035-sql-server/suggestions/32629024](https://feedback.azure.com/forums/908035-sql-server/suggestions/32629024). |
|Explorateur d’objets|Désactivation de l’élément de menu contextuel « Conception » dans « Tables de graphe », car ces tables ne sont pas prises en charge dans la version actuelle de SSMS.|
|Explorateur d’objets|Correction d’un problème entraînant l’affichage incorrect de la boîte de dialogue « Nouvelle planification du travail » sur les écrans haute résolution. Pour plus d’informations, consultez [https://feedback.azure.com/admin/v3/suggestions/35541262](https://feedback.azure.com/admin/v3/suggestions/35541262). |
|Explorateur d’objets|Correction/amélioration de la façon dont la taille de base de données (« Taille (Mo) ») s’affiche dans l’Explorateur d’objets les détails de l’Explorateur d’objets : utilisation de seulement 2 décimales et mise en forme à l’aide du séparateur de milliers. Pour plus d’informations, consultez [https://feedback.azure.com/forums/908035/suggestions/34379308](https://feedback.azure.com/forums/908035/suggestions/34379308).|
|Explorateur d’objets|Correction d’un problème qui entraînait l’échec de la création d’un « index spatial » avec une erreur telle que « Pour effectuer cette action, définissez la propriété PartitionScheme ».|
|Explorateur d’objets|Optimisation mineure des performances dans l’Explorateur d’objets pour éviter l’émission de requêtes supplémentaires, quand cela est possible.|
|Explorateur d’objets|Extension à tous les objets de schéma de la logique de la demande de confirmation du renommage d’une base de données (vous pouvez paramétrer cette fonctionnalité).|
|Explorateur d’objets|Ajout de la séquence d’échappement appropriée pour le filtrage de l’Explorateur d’objets. Pour plus d’informations, consultez [https://feedback.azure.com/forums/908035/suggestions/36678803](https://feedback.azure.com/forums/908035/suggestions/36678803). |
|Explorateur d’objets|Correction/amélioration de la vue dans les Détails de l’Explorateur d’objets pour afficher les nombres avec les séparateurs appropriés. Pour plus d’informations, consultez [https://feedback.azure.com/forums/908035/suggestions/32900944](https://feedback.azure.com/forums/908035/suggestions/32900944). |
|Explorateur d’objets|Correction liée au menu contextuel au niveau du nœud « Tables » lors d’une connexion à SQL Express, où il manquait le menu volant « Nouveau » et la table avec versions gérées par le système, et où les tables de graphe n’étaient pas correctement répertoriées. Pour plus d’informations, consultez [https://feedback.azure.com/forums/908035/suggestions/37245529](https://feedback.azure.com/forums/908035/suggestions/37245529). |
|Scripts d’objets|Amélioration globale des performances - La génération de scripts de WideWorldImporters prend moitié moins de temps qu’avec SSMS 17.7.|
|Scripts d’objets|Pendant la génération de scripts pour des objets, la configuration dans l’étendue de la base de données qui comporte des valeurs par défaut est omise.|
|Scripts d’objets|Absence de génération de code T-SQL dynamique pendant la génération de scripts. Pour plus d’informations, consultez [https://feedback.azure.com/forums/908035-sql-server/suggestions/32898391](https://feedback.azure.com/forums/908035-sql-server/suggestions/32898391). |
|Scripts d’objets|Omission de la syntaxe de graphe « as edge » et « as node » pendant la génération d’un script pour une table sur SQL Server 2016 et versions antérieures.|
|Scripts d’objets|Correction d’un problème qui entraînait l’échec de la génération de scripts d’objets de base de données lors de la connexion à une base de données Azure SQL à l’aide d’Azure AD avec l’authentification multifacteur (MFA).|
|Scripts d’objets|Correction d’un problème où la tentative de générer un script pour un index spatial avec GEOMETRY_AUTO_GRID/GEOGRAPHY_AUTO_GRID sur une base de données Azure SQL Database générait une erreur.|
|Scripts d’objets|Correction d’un problème à l’origine du ciblage permanent d’une instance SQL Server locale par les scripts de base de données (d’une base de données Azure SQL), même si les paramètres de script de l’Explorateur d’objets sont définis pour correspondre à la source.|
|Scripts d’objets|Correction du problème lié à la tentative d’exécuter un script dans une table de base de données SQL DW impliquant des index cluster et non-cluster, qui générait des instructions T-SQL incorrectes.|
|Scripts d’objets|Correction d’un problème lié à la tentative d’exécuter un script dans une table de base de données SQL DW avec des « index columnstore cluster » et des « index cluster », qui générait des instructions T-SQL incorrectes (instructions en double).|
|Scripts d’objets|Correction des scripts de table partitionnée sans valeurs de plage (bases de données SQL DW).|
|Scripts d’objets|Correction d’un problème où l’utilisateur ne pouvait pas générer de script d’audit/de spécification d’audit SERVER_PERMISSION_CHANGE_GROUP.|
|Scripts d’objets|Correction d’un problème où l’utilisateur est dans l’impossibilité de générer un script pour les statistiques à partir de SQL DW. Pour plus d’informations, consultez [https://feedback.azure.com/forums/908035-sql-server/suggestions/32897296](https://feedback.azure.com/forums/908035-sql-server/suggestions/32897296). |
|Scripts d’objets|Correction d’un problème où l’« Assistant Générer un script » affiche une table incorrecte contenant une erreur de script lorsque le paramètre « Continuer l’exécution du script en cas d’erreur » est défini sur faux.|
|Scripts d’objets|Génération du script améliorée sur SQL Server 2019.|
|Profileur|Ajout de l’événement « Agrégation Table réécrire la requête » aux événements de Profiler.|
|Magasin de données des requêtes|Correction d’un problème où une exception « DocumentFrame (SQLEditors) » pouvait être levée.|
|Magasin de données des requêtes|Correction d’un problème empêchant l’utilisateur de sélectionner AM ou PM sur l’intervalle de début/fin lors de la tentative de définition d’un intervalle de temps personnalisé dans les rapports du magasin de données des requêtes intégré.|
|Grille des résultats|Correction d’un problème qui activait le mode Contraste élevé (numéros de ligne sélectionnés non visibles).|
|Grille des résultats|Correction d’un problème qui générait une exception « Index hors limites » quand vous cliquiez sur la grille.|
|Grille des résultats|Correction du problème où la couleur d’arrière-plan de la grille de résultats était ignorée. Pour plus d’informations, consultez [https://feedback.azure.com/forums/908035/suggestions/32895916](https://feedback.azure.com/forums/908035/suggestions/32895916). |
|ShowPlan|Les propriétés du nouvel opérateur d’allocation de mémoire ne s’affichent pas correctement quand il existe plusieurs threads.|
|ShowPlan|Ajoutez les 4 attributs suivants dans le RunTimeCountersPerThread du plan d’exécution XML réel : HpcRowCount (nombre de lignes traitées par un appareil *hpc*), HpcKernelElapsedUs (temps écoulé à attendre l’exécution du noyau en cours d’utilisation), HpcHostToDeviceBytes (octets transférés de l’hôte vers l’appareil) et HpcDeviceToHostBytes (octets transférés de l’appareil vers l’hôte).|
|ShowPlan|Correction d’un problème où les nœuds de plan similaires sont mis en surbrillance dans une position incorrecte.|
|SMO|Résolution d’un problème qui empêchait SMO/ServerConnection de gérer correctement les connexions basées sur SqlCredential. Pour plus d’informations, consultez [https://feedback.azure.com/forums/908035-sql-server/suggestions/33698941](https://feedback.azure.com/forums/908035-sql-server/suggestions/33698941). |
|SMO|Correction d’un problème où une application écrite à l’aide de SMO rencontrait une erreur si elle essayait d’énumérer les bases de données à partir du même serveur sur plusieurs threads, même si elle utilisait des instances de SqlConnection distinctes sur chaque thread.|
|SMO|Correction de la régression des performances de transfert à partir de tables externes.|
|SMO|Correction d’un problème lié au caractère thread-safe de ServerConnection qui entraînait une fuite des instances SqlConnection par SMO lors du ciblage d’Azure.|
|SMO|Correction d’un problème qui provoquait une erreur StringBuilder.FormatError lors des tentatives de restauration de bases de données contenant des accolades dans leur nom.|
|SMO|Correction du problème où les bases de données Azure dans SMO adoptaient par défaut le classement non sensible à la casse pour toutes les comparaisons de chaînes au lieu d’utiliser le classement spécifié pour la base de données.|
|Éditeur SSMS|Correction d’un problème lié à « Table système SQL » où la restauration des couleurs par défaut sélectionnait la couleur tilleul foncé, et non la couleur verte par défaut, ce qui rendait la lecture sur un arrière-plan blanc difficile. Pour plus d’informations, consultez [Restoring wrong default color for SQL System Table](https://feedback.azure.com/forums/908035-sql-server/suggestions/32896906) (Restauration d’une couleur par défaut incorrecte pour la Table système SQL). Le problème persiste sur les versions non anglaises de SSMS.|
|Éditeur SSMS|Résolution d’un problème empêchant IntelliSense de fonctionner en cas de connexion à Azure SQL DW avec l’authentification Azure Active Directory (Azure AD).|
|Éditeur SSMS|Résolution du problème lié à IntelliSense dans Azure quand l’utilisateur n’a pas d’accès à la base de données **MASTER**.|
|Éditeur SSMS|Correction d’extraits de code pour créer des « tables temporaires », qui étaient rompues quand le classement de la base de données cible était sensible à la casse.|
|Éditeur SSMS|Nouvelle fonction TRANSLATE maintenant reconnue par IntelliSense. Pour plus d’informations, consultez [https://feedback.azure.com/forums/908035-sql-server/suggestions/32898430](https://feedback.azure.com/forums/908035-sql-server/suggestions/32898430). |
|Éditeur SSMS|Amélioration d’IntelliSense sur une fonction intégrée FORMAT. Pour plus d’informations, consultez [https://feedback.azure.com/forums/908035-sql-server/suggestions/32898676](https://feedback.azure.com/forums/908035-sql-server/suggestions/32898676). |
|Éditeur SSMS|LAG et LEAD sont désormais reconnues comme des fonctions intégrées. Pour plus d’informations, consultez [https://feedback.azure.com/forums/908035-sql-server/suggestions/32898757](https://feedback.azure.com/forums/908035-sql-server/suggestions/32898757). |
|Éditeur SSMS|Correction d’un problème où IntelliSense donnait un avertissement lors de l’utilisation de « ALTER TABLE...ADD CONSTRAINT...WITH(ONLINE=ON) ».|
|Éditeur SSMS|Correction d’un problème selon lequel plusieurs vues système et fonctions table n’étaient pas de la bonne couleur.|
|Éditeur SSMS|Correction d’un problème où l’onglet de l’éditeur était susceptible de se fermer au lieu d’être sélectionné au moment de cliquer dessus. Pour plus d’informations, consultez [https://feedback.azure.com/forums/908035/suggestions/37291114](https://feedback.azure.com/forums/908035/suggestions/37291114). |
|Options de SSMS|Correction d’un problème empêchant le bon redimensionnement de la page **Outils** > **Options** > **Explorateur d’objets SQL Server** > **Commandes**.|
|Options de SSMS|Maintenant, SSMS désactive par défaut le téléchargement automatique de la DTD dans l’éditeur XMLA : l’éditeur de script XMLA (qui utilise le service de langage XML) empêche par défaut le téléchargement automatiquement de la DTD pour les fichiers XMLA potentiellement malveillants. Vous pouvez le contrôler en désactivant le paramètre « Télécharger automatiquement les DTD et les schémas » dans **Outils** > **Options** > **Environnement** > **Éditeur de texte** > **XML** > **Divers**.|
|Options de SSMS|Restauration du raccourci **Ctrl+D** tel qu’il existait dans l’ancienne version de SSMS. Pour plus d’informations, consultez [https://feedback.azure.com/forums/908035/suggestions/35544754](https://feedback.azure.com/forums/908035/suggestions/35544754). |
|Concepteur de tables|Correction d’un incident dans « Modifier 200 lignes ».|
|Concepteur de tables|Correction d’un problème lié au concepteur qui autorisait l’ajout d’une table en étant connecté à une base de données Azure SQL Database.|
|Évaluation des vulnérabilités|Résolution d’un problème empêchant le chargement des résultats d’analyse.|
|XEvent|Ajout de deux colonnes « action_name » et « class_type_desc » qui présentent les champs action ID et class type comme des chaînes lisibles.|
|XEvent|Suppression de la limite de l’Observateur XEvent de 1 000 000 événements.|
|XEvent Profiler|Correction d’un problème qui empêchait XEvent Profiler de se lancer en étant connecté à un serveur SQL Server à 96 cœurs.|
|Observateur XEvent|Correction d’un problème entraînant le plantage de l’observateur XEvent en cas de tentative de regroupement des événements à l’aide des « options de barre d’outils d’événements étendus ».|

#### <a name="deprecated-and-removed-features-in-180"></a>Fonctionnalités dépréciées et supprimées dans la version 18.0

Voici les fonctionnalités déconseillées et supprimées de la version 18.0 de SSMS.

- Débogueur T-SQL
- Diagrammes de base de données
- Les outils suivants ne sont plus installés avec SSMS :
  - OSQL.EXE
  - DReplay.exe
  - SQLdiag.exe
  - SSBDiagnose.exe
  - bcp.exe
  - sqlcmd.exe
- Outils du Gestionnaire de configuration :
  - Le Gestionnaire de configuration SQL Server et le Gestionnaire de configuration du serveur de rapports ne sont plus intégrés au programme d’installation de SSMS.
- Stratégies standard DMF
  - Les stratégies ne sont plus installées avec SSMS. Elles sont déplacées vers Git. Les utilisateurs peuvent éventuellement y contribuer et les télécharger/installer.
- Suppression de l’option -P de la ligne de commande SSMS
  - Pour des raisons de sécurité, l’option qui permettait de spécifier des mots de passe en texte clair sur la ligne de commande a été supprimée.
- Suppression de Générer des scripts > Publier sur le service Web
  - Cette fonctionnalité déconseillée a été supprimée de l’interface utilisateur de SSMS.
- Suppression du nœud « Maintenance > Existant » dans l’Explorateur d’objets.
  - Les nœuds anciens « Plan de maintenance de base de données » et « SQL Mail » ne seront plus accessibles. Les nœuds modernes « Database Mail » et « Plans de maintenance » continuent de fonctionner normalement.

#### <a name="known-issues-180"></a>Problèmes connus (18.0)

- Lors de l’installation de la version 18.0, vous pouvez rencontrer un problème qui empêche l’exécution de SQL Server Management Studio. Si vous rencontrez ce problème, suivez les étapes de l’article [SSMS2018 - Installed, but will not run](https://feedback.azure.com/forums/908035-sql-server/suggestions/37502512-ssms2018-installed-but-will-not-run) (SSMS2018 - Installé, mais ne s’exécute pas).

- La taille des données affichées dans les résultats de SSMS au format grille, texte ou fichier est limitée

- Il y a des problèmes de regénération du dessin lors du basculement d’une fenêtre de requête vers une autre. Voir [Commentaires des utilisateurs sur SQL Server](https://feedback.azure.com/forums/908035/suggestions/37474042) pour plus de détails. Pour contourner ce problème, vous pouvez désactiver l’accélération matérielle sous Outils > Options.

### <a name="1791"></a>17.9.1

![Télécharger](media/download-icon.png) [Télécharger SSMS 17.9.1](https://go.microsoft.com/fwlink/?linkid=2043154&clcid=0x409)

- Numéro de version : 17.9.1
- Numéro de build : 14.0.17289.0
- Date de publication : 21 novembre 2018

#### <a name="bug-fixes-in-1791"></a>Correctifs de bogues dans la version 17.9.1

- Correction d’un problème où les utilisateurs qui utilisaient l’authentification « Active Directory - Authentification universelle avec prise en charge de MFA » dans l’Éditeur de requête SQL voyaient leur connexion se fermer et se rouvrir à chaque appel de requête. Cette fermeture pouvait avoir pour effet de supprimer de façon inattendue les tables temporaires globales, voire dans certains cas, d’attribuer un nouveau SPID à la connexion.
- Correction d’un problème de longue date où aucun plan de restauration n’était trouvé, ou lors duquel le plan de restauration était inefficace dans certains cas.
- Correction d’un problème dans l’Assistant « Importer une application de la couche Données », qui pouvait entraîner une erreur en cas de connexion à une base de données SQL Azure.

> [!NOTE]
> Les versions non anglaises localisées de SSMS 17.x nécessitent la [mise à jour de sécurité KB 2862966](https://support.microsoft.com/kb/2862966) si l’installation est effectuée sur : Windows 8, Windows 7, Windows Server 2012 et Windows Server 2008 R2.

[Chinois (simplifié)](https://go.microsoft.com/fwlink/?linkid=2043154&clcid=0x804)| [Chinois (traditionnel)](https://go.microsoft.com/fwlink/?linkid=2043154&clcid=0x404)| [Anglais (États-Unis)](https://go.microsoft.com/fwlink/?linkid=2043154&clcid=0x409)| [Français](https://go.microsoft.com/fwlink/?linkid=2043154&clcid=0x40c)| [Allemand](https://go.microsoft.com/fwlink/?linkid=2043154&clcid=0x407)| [Italien](https://go.microsoft.com/fwlink/?linkid=2043154&clcid=0x410)| [Japonais](https://go.microsoft.com/fwlink/?linkid=2043154&clcid=0x411)| [Coréen](https://go.microsoft.com/fwlink/?linkid=2043154&clcid=0x412)| [Portugais (Brésil)](https://go.microsoft.com/fwlink/?linkid=2043154&clcid=0x416)| [Russe](https://go.microsoft.com/fwlink/?linkid=2043154&clcid=0x419)| [Espagnol](https://go.microsoft.com/fwlink/?linkid=2043154&clcid=0x40a)

#### <a name="uninstall-and-reinstall-ssms-17x"></a>Désinstaller et réinstaller SSMS 17.x

Si votre installation de SSMS rencontre des problèmes et qu’une désinstallation et réinstallation standard ne les résolvent pas, vous pouvez d’abord essayer de [réparer](https://support.microsoft.com/help/4028054/windows-10-repair-or-remove-programs) le shell isolé de Visual Studio 2015. Si la réparation du shell isolé de Visual Studio 2015 ne résout pas le problème, les étapes suivantes ont été trouvées pour résoudre de nombreux problèmes aléatoires :

1. Désinstallez SSMS comme vous désinstallez n’importe quelle application (avec *Applications et fonctionnalités*, *Programmes et fonctionnalités*, selon votre version de Windows).

2. Désinstallez le shell isolé de Visual Studio 2015 **à partir d’une invite de commandes avec élévation de privilèges** :

    ```PUSHD "C:\ProgramData\Package Cache\FE948F0DAB52EB8CB5A740A77D8934B9E1A8E301\redist"```

    ```vs_isoshell.exe /Uninstall /Force /PromptRestart```

3. Désinstallez Microsoft Visual C++ 2015 Redistributable comme vous désinstallez n’importe quelle application. Désinstallez x86 et x64 s’ils sont sur votre ordinateur.

4. Réinstallez le shell isolé de Visual Studio 2015 **à partir d’une invite de commandes avec élévation de privilèges** :  

    ```PUSHD "C:\ProgramData\Package Cache\FE948F0DAB52EB8CB5A740A77D8934B9E1A8E301\redist"```  

    ```vs_isoshell.exe /PromptRestart```

5. Réinstallez SSMS.

6. Effectuez une mise à niveau avec la [version la plus récente de Visual C++ 2015 Redistributable](https://support.microsoft.com/help/2977003/the-latest-supported-visual-c-downloads) si vous n’êtes plus à jour.

### <a name="1653"></a>16.5.3

![Télécharger](media/download-icon.png) [Télécharger SSMS 16.5.3](https://go.microsoft.com/fwlink/?LinkID=840946)

- Numéro de version : 16.5.3  
- Numéro de build : 13.0.16106.4  
- Date de publication : 30 janvier 2017

[Chinois (simplifié)](https://go.microsoft.com/fwlink/?linkid=840946&clcid=0x804)| [Chinois (traditionnel)](https://go.microsoft.com/fwlink/?linkid=840946&clcid=0x404)| [Anglais (États-Unis)](https://go.microsoft.com/fwlink/?linkid=840946&clcid=0x409)| [Français](https://go.microsoft.com/fwlink/?linkid=840946&clcid=0x40c)| [Allemand](https://go.microsoft.com/fwlink/?linkid=840946&clcid=0x407)| [Italien](https://go.microsoft.com/fwlink/?linkid=840946&clcid=0x410)| [Japonais](https://go.microsoft.com/fwlink/?linkid=840946&clcid=0x411)| [Coréen](https://go.microsoft.com/fwlink/?linkid=840946&clcid=0x412)| [Portugais (Brésil)](https://go.microsoft.com/fwlink/?linkid=840946&clcid=0x416)| [Russe](https://go.microsoft.com/fwlink/?linkid=840946&clcid=0x419)| [Espagnol](https://go.microsoft.com/fwlink/?linkid=840946&clcid=0x40a)

#### <a name="bug-fixes-in-1653"></a>Correctifs de bogues dans la version 16.5.3

- Correction d’un problème introduit dans SSMS 16.5.2 qui provoquait l’expansion du nœud « Table » quand la table comportait plusieurs colonnes éparses.

- Les utilisateurs peuvent déployer des packages SSIS contenant le Gestionnaire de connexions OData qui connectent une ressource Microsoft Dynamics AX/CRM Online au catalogue SSIS. Pour plus d’informations, consultez [Gestionnaire de connexions OData](../integration-services/connection-manager/odata-connection-manager.md).

- La configuration d’Always Encrypted sur une table existante échoue avec des erreurs sur des objets non associés. [Microsoft Connect - ID 3103181](https://connect.microsoft.com/SQLServer/feedback/details/3103181/setting-up-always-encrypted-on-an-existing-table-fails-with-errors-on-unrelated-objects)

- La configuration d’Always Encrypted pour une base de données existante avec plusieurs schémas ne fonctionne pas. [Microsoft Connect - ID 3109591](https://connect.microsoft.com/SQLServer/feedback/details/3109591/sql-server-2016-always-encrypted-against-existing-database-with-multiple-schemas-doesnt-work)

- L’Assistant Colonne chiffrée d’Always Encrypted échoue parce que la base de données contient des vues qui référencent des vues système. [Microsoft Connect - ID 3111925](https://connect.microsoft.com/SQLServer/feedback/details/3111925/sql-server-2016-always-encrypted-encrypted-column-wizard-failed-task-failed-due-to-following-error-cannot-save-package-to-file-the-model-has-build-blocking-errors)

- Lors du chiffrement à l’aide d’Always Encrypted, les erreurs provenant de l’actualisation des modules après le chiffrement ne sont pas gérées correctement.

- Le menu *Ouvrir un fichier récent* n’affiche pas les fichiers récemment enregistrés. [Microsoft Connect - ID 3113288](https://connect.microsoft.com/SQLServer/feedback/details/3113288/ssms-2016-open-recent-menu-doesnt-show-recently-saved-files)

- SSMS est lent quand l’utilisateur clique avec le bouton droit sur un index pour une table (via une connexion (Internet) à distance). [Microsoft Connect - ID 3114074](https://connect.microsoft.com/SQLServer/feedback/details/3114074/ssms-slow-when-right-clicking-an-index-for-a-table-over-a-remote-internet-connection)

- Correction d’un problème avec la barre de défilement de SQL Designer. [Microsoft Connect - ID 3114856](https://connect.microsoft.com/SQLServer/feedback/details/3114856/bug-in-scrollbar-on-sql-desginer-in-ssms-2016)

- Le menu contextuel pour les tables cesse de répondre momentanément

- Il peut arriver que SSMS lève des exceptions dans le moniteur d’activité et se bloque. [Microsoft Connect - ID 697527](https://connect.microsoft.com/SQLServer/feedback/details/697527/)

- SSMS 2016 se bloque avec l’erreur « Le processus a été arrêté en raison d’une erreur interne dans le runtime .NET à l’adresse IP 71AF8579 (71AE0000) avec le code de sortie 80131506 »

## <a name="additional-downloads"></a>Téléchargements supplémentaires

Pour obtenir la liste de tous les téléchargements de SQL Server Management Studio, rechercher dans le [Centre de téléchargement Microsoft](https://www.microsoft.com/download/search.aspx?q=sql%20server%20management%20studio&p=0&r=10&t=&s=Relevancy~Descending).  
  
Pour obtenir la dernière version de SQL Server Management Studio, consultez [Télécharger SQL Server Management Studio &#40;SSMS&#41;](../ssms/download-sql-server-management-studio-ssms.md).