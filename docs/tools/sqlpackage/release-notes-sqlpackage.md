---
title: Notes de publication de DacFx et de SqlPackage
description: Notes de publication de Microsoft SqlPackage.
ms.custom: tools|sos
ms.date: 03/10/2021
ms.prod: sql
ms.reviewer: llali; sstein
ms.prod_service: sql-tools
ms.topic: conceptual
author: dzsquared
ms.author: drskwier
ms.openlocfilehash: 3a5d63b5c727508473fe83c1de3668b5b3df85c0
ms.sourcegitcommit: 81ee3cd57526d255de93afb84186074a3fb9885f
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/10/2021
ms.locfileid: "102622545"
---
# <a name="release-notes-for-sqlpackageexe"></a>Notes de version de SqlPackage.exe

**[Télécharger la version la plus récente](sqlpackage-download.md)**

Cet article présente les fonctionnalités et les correctifs fournis par les versions commerciales de SqlPackage.exe.

## <a name="187-sqlpackage"></a>sqlpackage 18.7

|Plateforme|Téléchargement|Date de publication|Version|Build
|:---|:---|:---|:---|:---|
| Windows|[Programme d’installation MSI](https://go.microsoft.com/fwlink/?linkid=2157201)|10 mars 2021|18,7|15.0.5084.2|
|macOS .NET Core |[Fichier zip](https://go.microsoft.com/fwlink/?linkid=2157203)|10 mars 2021| 18,7|15.0.5084.2|
|Linux .NET Core |[Fichier zip](https://go.microsoft.com/fwlink/?linkid=2157202)|10 mars 2021| 18,7|15.0.5084.2|
|Windows .NET Core |[Fichier zip](https://go.microsoft.com/fwlink/?linkid=2157302)|10 mars 2021| 18,7|15.0.5084.2|

### <a name="features"></a>Fonctionnalités
| Fonctionnalité | Détails |
| :------ | :------ |
| Déploiement | Extrayez/publiez des données Big Data vers/à partir de Stockage Azure. Pour plus d’informations, consultez [SqlPackage pour le Big Data](sqlpackage-for-azure-synapse-analytics.md) |
| Azure Synapse Analytics | Prise en charge de la sécurité au niveau des lignes (fonction table inline, stratégie de sécurité, prédicat de sécurité)  |
| Azure Synapse Analytics | Prise en charge de la classification des charges de travail |
| Azure SQL Edge | Prise en charge des travaux de streaming externe |
| Azure SQL Edge | Ajout d’options de table et de base de données pour la conservation des données. |
| Importer | Ajout de deux nouvelles propriétés d’option d’index pour les opérations d’importation. *DisableIndexesForDataPhase* (Désactiver les index avant l’importation de données dans SQL Server, true par défaut) et *RebuildIndexesOfflineForDataPhase* (Reconstruire les index en mode hors connexion après l’importation de données dans SQL Server, false par défaut) |
| Journalisation | Ajout d’une propriété pour toutes les opérations (HashObjectNamesInLogs) qui transforment tous les noms d’objets en chaîne de hachage dans les messages du journal. |
| Performances | Améliorations apportées aux performances d’importation et d’exportation, avec notamment une journalisation complémentaire pour faciliter l’identification des goulots d’étranglement supplémentaires. |
| SQLCMD | Ajout d’une propriété pour le déploiement et la comparaison de schémas (DoNotEvaluateSqlCmdVariables) qui indique si les variables SQLCMD seront remplacées par des valeurs. |



### <a name="fixes"></a>Correctifs
| Fonctionnalité | Détails |
| :------ | :------ | 
| Déploiement | Valeur par défaut de MAXDOP passée de 0 à 8 pour [Azure SQL](https://techcommunity.microsoft.com/t5/azure-sql/changing-default-maxdop-in-azure-sql-database/ba-p/1538528), mettant à jour la valeur par défaut du modèle de schéma dans DacFx | 
| Comparaison de schémas | Procédures stockées utilisant les mots clés OUT et OUTPUT à ignorer comme différence |
| Déploiement | Validation supplémentaire pour les jetons Big Data |
| Build/Déploiement | Nettoyage complet du modèle de schéma des tables externes temporaires pour la cohérence dacpac finale.  |
| Build/Déploiement | Ajout de la gestion des erreurs et correction de l’erreur « non-Edge 150 RE ». |
| Importation/déploiement | Valeur de séquence restaurée pendant le déploiement |
| Déploiement | Résolution d’un problème où la modification de l’option de compression sur un index cluster provoquait la recréation de la table et non la modification de l’index. |
| Déploiement | Correction d’un problème qui entraînait la suppression et la recréation d’un index cluster columnstore si une colonne de table était modifiée. |
| Déploiement | Correction de la suppression et de la recréation des utilisateurs externes pendant le déploiement. |
| Comparaison de schémas | Correction du problème de comparaison de schémas avec les travaux de streaming externe. |
| Importer | Exception de référence null levée quand le paramètre ambiant ReliableDdlEnabled est activé pendant la création du script d’un rapport de déploiement.|
| Déploiement | Correction d’un problème où les étapes de déploiement contenant le contrôle de version système étaient créées dans le mauvais ordre. |
| Déploiement | Correction d’un problème où la mise à jour de la comparaison de schémas ou le déploiement dacpac échouait en raison de la présence de tables temporelles dans la cible. |
| Déploiement | Réamorce la valeur d’identité après le déploiement sur la base de la dernière valeur précédente de la cible. |

### <a name="known-issues"></a>Problèmes connus
| Fonctionnalité | Détails |
| :------ | :------ |
| Déploiement | La fonctionnalité de gestion des charges de travail Azure Synapse Analytics (groupes de charge de travail et classifieurs de charges de travail) n’est pas encore prise en charge | 

## <a name="186-sqlpackage"></a>18.6 sqlpackage

|Plateforme|Téléchargement|Date de publication|Version|Build
|:---|:---|:---|:---|:---|
| Windows|[Programme d’installation MSI](https://go.microsoft.com/fwlink/?linkid=2143544)|18 septembre 2020|18.6|15.0.4897.1|
|macOS .NET Core |[Fichier zip](https://go.microsoft.com/fwlink/?linkid=2143659)|18 septembre 2020| 18.6|15.0.4897.1|
|Linux .NET Core |[Fichier zip](https://go.microsoft.com/fwlink/?linkid=2143497)|18 septembre 2020| 18.6|15.0.4897.1|
|Windows .NET Core |[Fichier zip](https://go.microsoft.com/fwlink/?linkid=2143496)|18 septembre 2020| 18.6|15.0.4897.1|

### <a name="features"></a>Fonctionnalités
| Fonctionnalité | Détails |
| :------ | :------ |
| Plateforme | Mise à jour de sqlpackage pour la version .NET Core vers .NET Core 3.1 |
| Always Encrypted | Ajout du support de l’importation et de l’exportation de l’enclave sécurisée pour SQL Server 2019 |
| Déploiement | Ajout de la prise en charge pour ignorer les tables de capture des changements de données activées lors de l’exportation à partir Azure SQL Database |
| Déploiement | Support supplémentaire pour l’option d’index OPTIMIZE_FOR_SEQUENTIAL_KEY dans Azure SQL Database |
| Déploiement | Ajout de la prise en charge des colonnes d’identité pour Azure Synapse Analytics | 
| Aide | Sortie de la version sqlpackage dans l’aide (/?) et prise en charge du paramètre/version | 

### <a name="fixes"></a>Correctifs
| Fonctionnalité | Détails |
| :------ | :------ | 
| Déploiement | Correction d’un script de déploiement incorrect qui était généré lorsqu’un utilisateur non sysadmin ciblait Azure SQL Managed Instance  | 
| Déploiement | Résolution du chargement des contributeurs de déploiement lors de l’exécution d’actions de script | 
| Aide | Sortie correcte de la durée calendaire dans sqlpackage lorsque l’opération prend plus d’une journée | 
| Déploiement | Correction de l’inscription dacpac lors du déploiement de .NET Core | 
| Déploiement | Correction de sqlpackage sur la gestion .NET Core du paramètre/accessToken (/at) | 
| Déploiement | Autoriser les instructions ALTER TABLE dans les procédures stockées comme des instructions de niveau non supérieur | 
| Déploiement | Correction de la validation Azure Synapse Analytics des vues matérialisés pour ne pas respecter la casse | 

### <a name="known-issues"></a>Problèmes connus
| Fonctionnalité | Détails |
| :------ | :------ |
| Déploiement | La fonctionnalité de gestion des charges de travail Azure Synapse Analytics (groupes de charge de travail et classifieurs de charges de travail) n’est pas encore prise en charge | 

## <a name="1851-sqlpackage"></a>18.5.1 sqlpackage

|Plateforme|Téléchargement|Date de publication|Version|Build
|:---|:---|:---|:---|:---|
| Windows|[Programme d’installation MSI](https://go.microsoft.com/fwlink/?linkid=2134206)|24 juin 2020|18.5.1|15.0.4826.1|
|macOS .NET Core |[Fichier zip](https://go.microsoft.com/fwlink/?linkid=2134312)|24 juin 2020| 18.5.1|15.0.4826.1|
|Linux .NET Core |[Fichier zip](https://go.microsoft.com/fwlink/?linkid=2134311)|24 juin 2020| 18.5.1|15.0.4826.1|
|Windows .NET Core |[Fichier zip](https://go.microsoft.com/fwlink/?linkid=2134310)|24 juin 2020| 18.5.1|15.0.4826.1|

### <a name="fixes"></a>Correctifs
| Fonctionnalité | Détails |
| :------ | :------ |
| Déploiement | Correction d’une régression introduite dans la version 18.5, provoquant une erreur « Syntaxe incorrecte près de 'type' » lors du déploiement d’un dacpac ou de l’importation d’un bacpac avec un utilisateur disposant d’une connexion externe vers local | 

## <a name="185-sqlpackage"></a>18.5 sqlpackage

|Plateforme|Téléchargement|Date de publication|Version|Build
|:---|:---|:---|:---|:---|
| Windows|[Programme d’installation MSI](https://go.microsoft.com/fwlink/?linkid=2128142)|28 avril 2020|18.5|15.0.4769.1|
|macOS .NET Core |[Fichier zip](https://go.microsoft.com/fwlink/?linkid=2128145)|28 avril 2020| 18.5|15.0.4769.1|
|Linux .NET Core |[Fichier zip](https://go.microsoft.com/fwlink/?linkid=2128144)|28 avril 2020| 18.5|15.0.4769.1|
|Windows .NET Core |[Fichier zip](https://go.microsoft.com/fwlink/?linkid=2128143)|28 avril 2020| 18.5|15.0.4769.1|

### <a name="features"></a>Fonctionnalités
| Fonctionnalité | Détails |
| :------ | :------ |
| Déploiement | La classification de la sensibilité des données est maintenant prise en charge pour SQL Server 2008 et versions ultérieures, Azure SQL Database et Azure Synapse Analytics |
| Déploiement | Ajout de la prise en charge des contraintes de table dans Azure Synapse Analytics |
| Déploiement | Ajout de la prise en charge de l’index columnstore en cluster ordonné dans Azure Synapse Analytics |
| Déploiement | Ajout de la prise en charge de la source de données externe (pour Oracle, Teradata, MongoDB/CosmosDB, ODBC, cluster Big Data) et de la table externe pour le cluster Big Data SQL Server 2019 |
| Déploiement | Ajout de SQL Database Edge comme édition prise en charge |
| Déploiement | Prise en charge des noms de serveur Managed Instance au format '\<server>.\<dnszone>.database.windows.net' |
| Déploiement | Ajout de la prise en charge de la commande copy dans Azure Synapse Analytics |
| Déploiement | Ajout de l’option de déploiement « IgnoreTablePartitionOptions » durant la publication pour éviter la recréation de la table en cas de modification de la fonction de partition sur la table pour Azure Synapse Analytics |
| .NET Core | Ajout de la prise en charge de Microsoft.Data.SqlClient dans la version .NET Core de sqlpackage |
| &nbsp; | &nbsp; |

### <a name="fixes"></a>Correctifs
| Fix | Détails |
| :-- | :------ |
| Déploiement | Correction de l’analyse du chemin JSON en tant qu’expression |
| Déploiement | Correction de la génération d’instructions GRANT pour les autorisations AlterAnyDatabaseScopedConfiguration et AlterAnySensitivityClassification |
| Déploiement | Correction d’un problème entraînant la non-reconnaissance d’une autorisation de script externe |
| Déploiement | Correction de la propriété inline : l’ajout implicite de la propriété ne doit pas apparaître dans la différence, mais une mention explicite doit apparaître dans le script |
| Déploiement | Résolution d’un problème où la modification d’une table référencée par une vue matérialisée entraîne la génération d’instructions Alter View qui ne sont pas prises en charge dans les vues matérialisées pour Azure Synapse Analytics |
| Déploiement | Correction de l’échec de la publication lors de l’ajout d’une colonne à une table avec des données pour Azure Synapse Analytics |
| Déploiement | Correction du script de mise à jour devant déplacer les données vers une nouvelle table lors de la modification du type de colonne de distribution (scénario de perte de données) pour Azure Synapse Analytics |
| ScriptDom | Correction d’un bogue dans ScriptDom entraînant la non-reconnaissance des contraintes inline définies après un index inline |
| ScriptDom | Correction dans ScriptDom d’une parenthèse fermante manquante SYSTEM_TIME dans une instruction de traitement par lots |
| Always Encrypted | Correction de l’échec de la suppression de la table #tmpErrors lorsque sqlpackage se reconnecte et que la table temporaire est déjà supprimée (la table temporaire disparaissant quand la connexion est perdue) |
| &nbsp; | &nbsp; |

### <a name="known-issues"></a>Problèmes connus
| Fonctionnalité | Détails |
| :------ | :------ |
| Déploiement |  Une régression a été introduite dans la version 18.5, provoquant une erreur « Syntaxe incorrecte près de 'type' » lors du déploiement d’un dacpac ou de l’importation d’un bacpac avec un utilisateur disposant d’une connexion externe vers local. Contournez ce problème en utilisant sqlpackage 18.4. Ce sera corrigé dans la prochaine version de sqlpackage. | 
| .NET Core | L’importation de bacpacs avec Sensitivity Classification échoue avec le message « Erreur irrécupérable de connexion interne » en raison de ce [problème connu](https://github.com/dotnet/SqlClient/issues/559) dans Microsoft.Data.SqlClient. Ce problème sera résolu dans la prochaine version de sqlpackage. |
| &nbsp; | &nbsp; |

## <a name="1841-sqlpackage"></a>sqlpackage 18.4.1

|Plateforme|Téléchargement|Date de publication|Version|Build
|:---|:---|:---|:---|:---|
| Windows|[Programme d’installation MSI](https://go.microsoft.com/fwlink/?linkid=2113703)|13 décembre 2019|18.4.1|15.0.4630.1|
|macOS .NET Core |[Fichier zip](https://go.microsoft.com/fwlink/?linkid=2113705)|13 décembre 2019| 18.4.1|15.0.4630.1|
|Linux .NET Core |[Fichier zip](https://go.microsoft.com/fwlink/?linkid=2113331)|13 décembre 2019| 18.4.1|15.0.4630.1|
|Windows .NET Core |[Fichier zip](https://go.microsoft.com/fwlink/?linkid=2113704)|13 décembre 2019| 18.4.1|15.0.4630.1|

### <a name="fixes"></a>Correctifs
| Fix | Détails |
| :-- | :------ |
| ScriptDom |  Une régression de l’analyse ScriptDom a été introduite dans la version 18.3.1, où ’RENAME’ n’est pas traité correctement comme un jeton de niveau supérieur, ce qui entraîne l’échec de l’analyse.
| &nbsp; | &nbsp; |

### <a name="known-issues"></a>Problèmes connus 

| Fonctionnalité | Détails |
| :------ | :------ |
| Déploiement |  Une régression introduite dans la version 18.4.1 provoque l’erreur « La référence d’objet n’a pas pour valeur une instance d’un objet. » lors du déploiement d’un dacpac ou de l’importation d’un bacpac avec un utilisateur disposant d’une connexion externe. Contournez ce problème en utilisant sqlpackage 18.4. Ce sera corrigé dans la prochaine version de sqlpackage. | 
| &nbsp; | &nbsp; |

## <a name="184-sqlpackage"></a>sqlpackage 18.4

|Plateforme|Téléchargement|Date de publication|Version|Build
|:---|:---|:---|:---|:---|
| Windows|[Programme d’installation MSI](https://go.microsoft.com/fwlink/?linkid=2108813)|29 octobre 2019|18.4|15.0.4573.2|
|macOS .NET Core |[Fichier zip](https://go.microsoft.com/fwlink/?linkid=2108815)|29 octobre 2019| 18.4|15.0.4573.2|
|Linux .NET Core |[Fichier zip](https://go.microsoft.com/fwlink/?linkid=2108814)|29 octobre 2019| 18.4|15.0.4573.2|
|Windows .NET Core |[Fichier zip](https://go.microsoft.com/fwlink/?linkid=2109019)|29 octobre 2019| 18.4|15.0.4573.2|

### <a name="features"></a>Fonctionnalités

| Fonctionnalité | Détails |
| :------ | :------ |
| Déploiement | Ajout de la prise en charge pour le déploiement sur Azure Synapse Analytics (GA). | 
| Plateforme | sqlpackage .NET Core GA pour macOS, Linux et Windows. | 
| Sécurité | Suppression de la signature du code SHA1. |
| Déploiement | Ajout de la prise en charge des nouvelles éditions des bases de données Azure : GeneralPurpose, BusinessCritical, Hyperscale |
| Déploiement | Ajout de la prise en charge de Managed Instance pour les groupes et utilisateurs Azure Active Directory. |
| Déploiement | Prise en charge du paramètre /AccessToken pour sqlpackage sur .NET Core. |
| &nbsp; | &nbsp; |

### <a name="known-issues"></a>Problèmes connus 

| Fonctionnalité | Détails |
| :------ | :------ |
| ScriptDom |  Une régression de l’analyse ScriptDom a été introduite dans la version 18.3.1, où ’RENAME’ n’est pas traité correctement comme un jeton de niveau supérieur, ce qui entraîne l’échec de l’analyse. Ce problème sera résolu dans la prochaine version de sqlpackage. | 
| &nbsp; | &nbsp; |

### <a name="known-issues-for-net-core"></a>Problèmes connus pour .NET Core

| Fonctionnalité | Détails |
| :------ | :------ |
| Importer |  Pour les fichiers .BacPac avec des fichiers compressés d’une taille supérieure à 4 Go, vous devrez peut-être utiliser la version .NET Core de sqlpackage pour effectuer l’importation.  Ce comportement est dû au fait que .NET Core génère des en-têtes zip qui, bien qu’ils soient valides, ne sont pas lisibles par la version .NET Framework complète de sqlpackage. | 
| Déploiement | Le paramètre /p:Storage=File n’est pas pris en charge. Seul Memory est pris en charge sur .NET Core. | 
| Always Encrypted | sqlpackage .NET Core ne prend pas en charge les colonnes Always Encrypted. | 
| Sécurité | sqlpackage .NET Core ne prend pas en charge le paramètre /ua pour l’authentification multifacteur. | 
| Déploiement | Les anciens fichiers .dacpac et .bacpac V2 qui utilisent la sérialisation de données JSON ne sont pas pris en charge. |
| &nbsp; | &nbsp; |

## <a name="1831-sqlpackage"></a>SqlPackage 18.3.1

|Plateforme|Téléchargement|Date de publication|Version|Build
|:---|:---|:---|:---|:---|
| Windows|[Programme d’installation MSI](https://go.microsoft.com/fwlink/?linkid=2102893)|13 septembre 2019|18.3.1|15.0.4538.1|
|macOS .NET Core (préversion)|[Fichier zip](https://go.microsoft.com/fwlink/?linkid=2102894)|13 septembre 2019| 18.3.1|15.0.4538.1|
|Linux .NET Core (préversion)|[Fichier zip](https://go.microsoft.com/fwlink/?linkid=2102978)|13 septembre 2019| 18.3.1|15.0.4538.1|
|Windows .NET Core (préversion)|[Fichier zip](https://go.microsoft.com/fwlink/?linkid=2102979)|13 septembre 2019| 18.13.1|15.0.4538.1|

### <a name="features"></a>Fonctionnalités

| Fonctionnalité | Détails |
| :------ | :------ |
| Déploiement | Ajout de la prise en charge pour le déploiement sur Azure Synapse Analytics (préversion). | 
| Déploiement | Ajout du paramètre /p:DatabaseLockTimeout=(INT32 '60') à sqlpackage. | 
| Déploiement | Ajout du paramètre /p:LongRunningCommandTimeout=(INT32) à sqlpackage. |
| Exportation/Extraction | Ajout du paramètre /p:TempDirectoryForTableData=(STRING) à sqlpackage. |
| Déploiement | Autorisation du chargement de contributeurs de déploiement à partir d’emplacements supplémentaires. Les contributeurs de déploiement sont chargés à partir du même répertoire que la cible .dacpac en cours de déploiement, du répertoire des extensions par rapport au fichier binaire sqlpackage.exe, et du paramètre /p:AdditionalDeploymentContributorPaths=(STRING) ajouté à sqlpackage où des emplacements de répertoire supplémentaires peuvent être spécifiés. |
| Déploiement | Ajoute de la prise en charge de OPTIMIZE_FOR_SEQUENTIAL_KEY. |
| &nbsp; | &nbsp; |

### <a name="fixes"></a>Correctifs

| Fix | Détails |
| :-- | :------ |
| Déploiement | Correction pour ignorer les index automatiques afin qu’ils ne soient pas supprimés lors du déploiement. | 
| Always Encrypted | Correction de la gestion des colonnes varchar Always Encrypted. | 
| Build/Déploiement | Correctif pour résoudre la méthode nodes() pour les jeux de colonnes XML.| 
| ScriptDom | Correction des cas supplémentaires où la chaîne « URL » était interprétée comme un jeton de niveau supérieur. | 
| Graph | Correction du TSQL généré pour les références de pseudo-colonnes dans les contraintes.  | 
| Exporter | Génération de mots de passe aléatoires conformes aux exigences de complexité. | 
| Déploiement | Correctif pour honorer les délais d’attente des commandes lors de la récupération de contraintes. | 
| .NET Core (préversion) | Correction de la journalisation des diagnostics dans un fichier. | 
| .NET Core (préversion) | Utilisation de la diffusion en continu pour exporter des données de table afin de prendre en charge les tables volumineuses. | 
| &nbsp; | &nbsp; |

## <a name="182-sqlpackage"></a>SqlPackage 18.2

|Plateforme|Téléchargement|Date de publication|Version|Build
|:---|:---|:---|:---|:---|
| Windows|[Programme d’installation MSI](https://go.microsoft.com/fwlink/?linkid=2087429)|15 avril 2019|18.2|15.0.4384.2|
|macOS .NET Core (préversion)|[Fichier zip](https://go.microsoft.com/fwlink/?linkid=2087247)|15 avril 2019 | 18.2 |15.0.4384.2|
|Linux .NET Core (préversion)|[Fichier zip](https://go.microsoft.com/fwlink/?linkid=2087431)|15 avril 2019 | 18.2 |15.0.4384.2|

### <a name="features"></a>Fonctionnalités

| Fonctionnalité | Détails |
| :------ | :------ |
| Graph | Ajout de la prise en charge des tables de graphe pour les contraintes de bord et les clauses de contrainte de bord. |
| Déploiement | Activation de la règle de validation de modèle afin de prendre en charge 32 colonnes pour les clés d’index avec SQL Server 2016 et les versions ultérieures. |
| &nbsp; | &nbsp; |

### <a name="fixes"></a>Correctifs

| Fix | Détails |
| :-- | :------ |
| Déploiement | Correction par rétroconception d’une base de données SQL Server 2016 RTM en raison d’un indicateur de requête non pris en charge. |
| Déploiement | Correction de l’ordre de déploiement des instructions AUTO CLOSE ALTER pour qu’elles se produisent avant les instructions CREATE FILEGROUP. |
| ScriptDom | Correction de la régression de l’analyse ScriptDom selon laquelle la chaîne « URL » était interprétée comme un jeton de niveau supérieur. |
| Déploiement | Correction d’une exception de référence Null lors de l’analyse d’une instruction ALTER TABLE ADD INDEX. | 
| Comparaison de schémas | Correction de la comparaison de schéma des colonnes calculées persistantes Nullable qui s’affichent toujours comme différentes.|
| &nbsp; | &nbsp; |

## <a name="181-sqlpackage"></a>SqlPackage 18.1

Date de publication : &nbsp; 1er février 2019  
Build : &nbsp; 15.0.4316.1  
Préversion.

### <a name="features"></a>Fonctionnalités

| Fonctionnalité | Détails |
| :------ | :------ |
| Déploiement | Ajout de la prise en charge des classements UTF-8. |
| Déploiement | Activation des index columnstore non cluster sur une vue indexée. |
| Plateforme | Déplacement vers .NET Core 2.2. | 
| Comparaison de schémas | Utilisation du stockage sur mémoire pour la comparaison de schémas sur .NET Core. |
| &nbsp; | &nbsp; |

### <a name="fixes"></a>Correctifs

| Fix | Détails |
| :-- | :------ |
| Performances | Correction des performances afin d’utiliser l’ancien estimateur de cardinalité pour les requêtes de rétroconception. | 
| Performances | Correction d’un problème important de performances de la comparaison de schéma lors de la génération d’un script. | 
| Comparaison de schémas | Correction de la logique de détection de dérive du schéma afin d’ignorer certaines sessions d’événements étendus (XEvent). |
| Graph | Correction de l’ordre d’importation des tables de graphe. | 
| Exporter | Correction de l’exportation de tables externes comportant des autorisations d’objet. |
| &nbsp; | &nbsp; |

### <a name="known-issues"></a>Problèmes connus

Cette version inclut les versions d’évaluation multiplateformes de SqlPackage qui ciblent .NET Core 2.2. SqlPackage peut s’exécuter sur macOS et Linux.

| Problème connu | Détails |
| :---------- | :------ |
| Déploiement | Pour .NET Core, les collaborateurs de build et de déploiement ne sont pas pris en charge. | 
| Déploiement | Pour .NET Core, les anciens fichiers .dacpac et .bacpac qui utilisent la sérialisation de données JSON ne sont pas pris en charge. | 
| Déploiement | Pour .NET Core, il peut arriver que les fichiers .dacpac référencés (par exemple, master.dacpac) ne se résolvent pas en raison de problèmes avec les systèmes de fichiers sensibles à la casse. Pour contourner le problème, il suffit de mettre en majuscules le nom du fichier de référence (par exemple, MASTER.BACPAC). |
| &nbsp; | &nbsp; |

## <a name="180-sqlpackage"></a>SqlPackage 18.0

Date de publication : &nbsp; 24 octobre 2018  
Build : &nbsp; 15.0.4200.1

### <a name="features"></a>Fonctionnalités

| Fonctionnalité | Détails |
| :------ | :------ |
| Déploiement | Ajout de la prise en charge du niveau 150 de compatibilité de base de données. | 
| Déploiement | Ajout de la prise en charge de Managed Instance. | 
| Performances | Ajout du paramètre de ligne de commande MaxParallelism pour spécifier le degré de parallélisme des opérations de base de données. | 
| Sécurité | Ajout du paramètre de ligne de commande AccessToken pour spécifier un jeton d’authentification lors de la connexion à SQL Server. | 
| Importer | Ajout de la prise en charge des flux de types de données BLOB/CLOB pour les importations. | 
| Déploiement | Ajout de la prise en charge de l’option « INLINE » des fonctions UDF scalaires. | 
| Graph | Ajout de la prise en charge de la syntaxe « MERGE » des tables de graphe. |
| &nbsp; | &nbsp; |

### <a name="fixes"></a>Correctifs

| Fix | Détails |
| :-- | :------ |
| Graph | Correction des pseudo-colonnes non résolues pour les tables de graphe. |
| Déploiement | Correction de la création d’une base de données avec des groupes de fichiers à mémoire optimisée lorsque des tables à mémoire optimisée sont utilisées. |
| Déploiement | Correction de l’intégration de propriétés étendues sur les tables externes. |
| &nbsp; | &nbsp; |

## <a name="178-sqlpackage"></a>SqlPackage 17.8

Date de publication : &nbsp; 22 juin 2018  
Build : &nbsp; 14.0.4079.2

### <a name="features"></a>Fonctionnalités

| Fonctionnalité | Détails |
| :------ | :------ |
| Diagnostics | Amélioration des messages d’erreur en cas d’échec de connexion, y compris le message d’exception SqlClient. |
| Déploiement | Prise en charge de la compression des index à partition unique pour l’importation/exportation. |
| &nbsp; | &nbsp; |

### <a name="fixes"></a>Correctifs

| Fix | Détails |
| :-- | :------ |
| Déploiement | Correction d’un problème de rétroconception pour les jeux de colonnes XML avec SQL 2017 et les versions ultérieures. | 
| Déploiement | Correction du problème selon lequel les scripts du niveau de compatibilité de la base de données 140 étaient ignorés pour Azure SQL Database. |
| &nbsp; | &nbsp; |

## <a name="1741-sqlpackage"></a>SqlPackage 17.4.1

Date de publication : &nbsp; 25 janvier 2018  
Build : &nbsp; 14.0.3917.1

### <a name="features"></a>Fonctionnalités

| Fonctionnalité | Détails |
| :------ | :------ |
| Importer/Exporter | Ajout du paramètre de ligne de commande ThreadMaxStackSize pour analyser du code Transact-SQL comportant de nombreuses instructions imbriquées. |
| Déploiement | Prise en charge du classement de catalogue de base de données. | 
| &nbsp; | &nbsp; |

### <a name="fixes"></a>Correctifs

| Fix | Détails |
| :-- | :------ |
| Importer | Lors de l’importation d’un .bacpac Azure SQL Database dans une instance locale, correction des erreurs liées au fait que _Les clés principales de base de données sans mot de passe ne sont pas prises en charge dans cette version de SQL Server_. |
| Graph | Correction d’une erreur de pseudo-colonnes non résolue pour les tables de graphe. |
| Comparaison de schémas | Correction de l’authentification SQL pour comparer les schémas. | 
| &nbsp; | &nbsp; |

## <a name="1740-sqlpackage"></a>SqlPackage 17.4.0

Date de publication : &nbsp; 12 décembre 2017  
Build : &nbsp; 14.0.3881.1

### <a name="features"></a>Fonctionnalités

| Fonctionnalité | Détails |
| :------ | :------ |
| Déploiement |  Ajout de la prise en charge de la _stratégie de rétention temporelle_ sur SQL 2017 et versions ultérieures et sur Azure SQL Database. | 
| Diagnostics | Ajout du paramètre de ligne de commande /DiagnosticsFile:"C:\Temp\sqlpackage.log" pour spécifier un chemin de fichier permettant d’enregistrer les informations de diagnostic. | 
| Diagnostics | Ajout du paramètre de ligne de commande /Diagnostics pour consigner les informations de diagnostic dans la console. |
| &nbsp; | &nbsp; |

### <a name="fixes"></a>Correctifs

| Fix | Détails |
| :-- | :------ |
| Déploiement | Suppression du blocage en cas de niveau de compatibilité de base de données non compris. La dernière version de Azure SQL Database ou la dernière plateforme sur site sera présumée. |
| &nbsp; | &nbsp; |
