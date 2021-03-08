---
title: Limitations et problèmes connus pour SSIS sur Linux
description: Cet article décrit les limitations et les problèmes connus pour SQL Server Integration Services (SSIS) sur les ordinateurs Linux
author: lrtoyou1223
ms.author: lle
ms.reviewer: maghan
ms.date: 06/06/2018
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
ms.openlocfilehash: ce07fb02a81b4e44375d80f643713b9a0b62d80d
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/04/2021
ms.locfileid: "101838994"
---
# <a name="limitations-and-known-issues-for-ssis-on-linux"></a>Limitations et problèmes connus pour SSIS sur Linux

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

Cet article décrit les limitations et les problèmes connus pour SQL Server Integration Services (SSIS) sur Linux.

## <a name="general-limitations-and-known-issues"></a>Limitations générales et problèmes connus

Les fonctions suivantes ne sont pas prises en charge dans cette mise en production de SSIS sur Linux :
  - Base de données de catalogues SSIS
  - Exécution planifiée du package par l’agent SQL
  - Authentification Windows
  - Composants tiers
  - Capture de données modifiées (CDC)
  - SSIS Scale Out
  - Feature Pack Azure pour SSIS
  - Support Hadoop et HDFS
  - Microsoft Connector pour SAP BW

Pour d’autres limitations et problèmes connus liés à SSIS sur Linux, consultez les [Notes de publication](sql-server-linux-release-notes.md#ssis).

## <a name="supported-and-unsupported-components"></a><a name="components"></a> Composants pris en charge et non pris en charge

Les composants Integration Services intégrés suivants sont pris en charge sur Linux. Certains ont des limitations sur la plateforme Linux. Les composants intégrés qui ne sont pas répertoriés ici ne sont pas pris en charge sur Linux.

## <a name="supported-control-flow-tasks"></a>Tâches de flux de contrôle prises en charge
- tâche d'insertion en bloc
- tâche de flux de données
- Tâche de profilage des données
- Tâche d’exécution de requêtes SQL
- Tâche Exécuter l'instruction T-SQL
- Tâche d'expression
- Tâche FTP
- Tâche de service Web
- Tâche XML

## <a name="control-flow-tasks-supported-with-limitations"></a>Tâches de flux de contrôle prises en charge avec limitations

| Tâche | Limites |
|------------|---|
| Tâche d’exécution de processus | Prend uniquement en charge le mode in-process. |
| Tâche de système de fichiers | Les actions *Déplacer le répertoire* et *Définir les attributs du fichier* ne sont pas prises en charge. |
| tâche de script | Prend uniquement en charge les API .NET Framework standard. |
| tache Envoyer un message | Prend uniquement en charge le mode utilisateur anonyme. |
| Tâche de transfert de bases de données | Les chemins d'accès UNC ne sont pas pris en charge. |
| | |

## <a name="supported-and-unsupported-maintenance-plan-tasks"></a>Tâches de plan de maintenance prises en charge et non prises en charge

Dans un plan de maintenance SQL Server, vous pouvez généralement utiliser plusieurs tâches SSIS.

Les tâches de plan de maintenance suivantes ne sont pas prises en charge sur Linux :
- Notifier l'opérateur
- Exécuter la tâche SQL Server Agent

Les tâches de plan de maintenance suivantes sont prises en charge sur Linux :
- Vérifier l'intégrité de la base de données
- Réduire la base de données
- Réorganiser l'index
- Régénérer l'index
- Mettre à jour les statistiques
- Nettoyer l'historique
- Sauvegarder la base de données
- Instruction T-SQL

## <a name="supported-control-flow-containers"></a>Conteneurs de flux de contrôle pris en charge
- conteneur de séquences
- Conteneur de boucles For
- Conteneur de boucles Foreach

## <a name="supported-data-flow-sources-and-destinations"></a>Sources et destinations de flux de données prises en charge
- Source et destination de fichier brut
- Source XML

## <a name="data-flow-sources-and-destinations-supported-with-limitations"></a>Sources et destinations du flux de données prises en charge avec des limitations

| Composant | Limites |
|------------|---|
| Source et destination ADO.NET | Ne prend en charge que le fournisseur de données SQLClient. |
| Source et destination fichier plat | 1. Ne prend en charge que les chemins d’accès aux fichiers de style Windows auxquels la règle de mappage de chemin d’accès par défaut est appliquée. Par exemple `D:\home\ssis\travel.csv` devient `/home/ssis/travel.csv`. 2. Le partage de fichiers Azure monté sur Red Hat 7 n’est pas pris en charge. |
| Source OData | Ne prend en charge que l’authentification de base. |
| Source et destination ODBC | Prend en charge les pilotes ODBC Unicode 64 bits sur Linux. Dépend du gestionnaire de pilotes UnixODBC sur Linux. |
| Source et destination OLE DB | Ne prend en charge que SQL Server Native Client 11.0 et Fournisseur Microsoft OLE DB pour SQL Server. |
| | |

## <a name="supported-data-flow-transformations"></a>Transformations du flux de données prises en charge
- Agrégat
- Audit
- Distributeur de données équilibrées
- Table des caractères
- Fractionnement conditionnel
- Copie de colonnes
- Conversion de données
- Colonne dérivée
- Exportation de colonne
- Regroupement probable
- Recherche floue
- Importation de colonne
- Recherche
- Fusionner
- Merge Join
- Multidiffusion
- Tableau croisé dynamique
- Nombre de lignes
- Dimension à variation lente
- Trier
- Recherche de terme
- Union ensembliste
- Supprimer le tableau croisé dynamique

## <a name="data-flow-transformations-supported-with-limitations"></a>Transformations du flux de données prises en charge avec limitations

| Composant | Limites |
|------------|---|
| transformation de commande OLE DB | Mêmes limitations que la source et la destination OLE DB. |
| composant Script | Prend uniquement en charge les API .NET Framework standard. |
| | |

## <a name="supported-and-unsupported-log-providers"></a>Modules fournisseur d'informations pris en charge et non pris en charge
Tous les modules fournisseur d’informations SSIS intégrés sont pris en charge sur Linux, à l’exception du Fournisseur du journal des événements Windows.

Le module fournisseur d’informations SQL Server prend en charge uniquement l’authentification SQL ; il ne prend pas en charge l’authentification Windows.

Les modules fournisseurs d’informations SSIS pour les fichiers texte, les fichiers XML et pour SQL Server Profiler, écrivent leur sortie dans un fichier que vous spécifiez. Les remarques suivantes s’appliquent au chemin d'accès du fichier :
-   Si vous ne fournissez pas de chemin d’accès, le module fournisseur d’informations écrit dans le répertoire actif de l’hôte. Si l’utilisateur actuel n’est pas autorisé à écrire dans le répertoire actif de l’hôte, le module fournisseur d’informations génère une erreur.
-   Vous ne pouvez pas utiliser une variable d’environnement dans un chemin d’accès du fichier. Si vous spécifiez une variable d’environnement, le texte littéral que vous spécifiez s’affiche dans le chemin d’accès du fichier. Par exemple, si vous spécifiez `%TMP%/log.txt`, le module fournisseur d’informations ajoute le texte littéral `/%TMP%/log.txt` au répertoire de l’hôte actuel.

## <a name="related-content-about-ssis-on-linux"></a>Contenu connexe relatif à SSIS sur Linux
-   [Extraire, transformer et charger des données sur Linux avec SSIS](sql-server-linux-migrate-ssis.md)
-   [Installer SQL Server Integration Services (SSIS) sur Linux](sql-server-linux-setup-ssis.md)
-   [Configurer SQL Server Integration Services sur Linux avec ssis-conf](sql-server-linux-configure-ssis.md)
-   [Planifier l’exécution du package SQL Server Integration Services sur Linux avec cron](sql-server-linux-schedule-ssis-packages.md)
