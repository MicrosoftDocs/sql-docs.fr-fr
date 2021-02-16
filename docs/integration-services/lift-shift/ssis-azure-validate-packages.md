---
title: Valider les packages SSIS déployés sur Azure | Microsoft Docs
description: Découvrez comment l’Assistant Déploiement de packages SSIS recherche dans les packages les problèmes connus qui peuvent les empêcher de s’exécuter comme prévu dans Azure.
ms.date: 11/27/2017
ms.topic: conceptual
ms.prod: sql
ms.prod_service: integration-services
ms.custom: ''
ms.technology: integration-services
author: swinarko
ms.author: sawinark
ms.reviewer: maghan
ms.openlocfilehash: 3da89cbe8d0028a2b362e6358d1b30b3075e030c
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100345683"
---
# <a name="validate-sql-server-integration-services-ssis-packages-deployed-to-azure"></a>Valider les packages SSIS (SQL Server Integration Services) déployés sur Azure

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]



Quand vous déployez un projet SQL Server Integration Services (SSIS) sur le catalogue SSIS (SSISDB) sur un serveur Azure, l’Assistant Déploiement de package ajoute une étape de validation supplémentaire après la page **Passer en revue**. Cette étape de validation inspecte les packages du projet à la recherche de problèmes connus susceptibles d’impacter leur exécution dans Azure SSIS Integration Runtime. L’Assistant affiche ensuite tous les avertissements applicables dans la page **Valider**.

> [!IMPORTANT]
> La validation décrite dans cet article se produit quand vous déployez un projet avec SQL Server Data Tools (SSDT) version 17.4 ou ultérieure. Pour obtenir la dernière version de SSDT, consultez [Télécharger SSDT (SQL Server Data Tools)](../../ssdt/download-sql-server-data-tools-ssdt.md).

Pour plus d’informations sur l’Assistant Déploiement de package, consultez [Déployer des projets et des packages Integration Services (SSIS)](../packages/deploy-integration-services-ssis-projects-and-packages.md).

## <a name="validate-connection-managers"></a>Valider les gestionnaires de connexions

L’Assistant inspecte certains gestionnaires de connexions à la recherche des problèmes suivants qui peuvent entraîner l’échec de la connexion :
- **Authentification Windows**. Si une chaîne de connexion utilise l’authentification Windows, la validation génère un avertissement. L’authentification Windows nécessite des étapes de configuration supplémentaires. Pour plus d’informations, consultez [Se connecter à des données et des partages de fichiers avec l’authentification Windows](/azure/data-factory/ssis-azure-connect-with-windows-auth).
- **Chemin du fichier**. Si une chaîne de connexion contient un chemin de fichier local codé en dur, comme `C:\\...`, la validation génère un avertissement. Les packages contenant un chemin absolu peuvent échouer.
- **Chemin UNC**. Si une chaîne de connexion contient un chemin UNC, la validation génère un avertissement. Les packages contenant un chemin UNC peuvent échouer, car ce chemin nécessite généralement l’authentification Windows pour l’octroi de l’accès.
- **Nom d’hôte**. Si une propriété de serveur contient un nom d’hôte au lieu d’une adresse IP, la validation génère un avertissement. Les packages contenant un nom d’hôte peuvent échouer, car le réseau virtuel Azure exige généralement la bonne configuration DNS pour prendre en charge la résolution de noms DNS.
- **Fournisseur ou pilote**. Si un fournisseur ou un pilote n’est pas pris en charge, la validation génère un avertissement. Seul un petit nombre de fournisseurs et de pilotes intégrés sont pris en charge pour l’instant.

L’Assistant effectue les contrôles de validation suivants pour les gestionnaires de connexions dans la liste.

| Gestionnaire de connexions | Authentification Windows | Chemins d'accès au fichier | Chemin UNC | Nom de l’hôte | Fournisseur ou pilote |
|--------------------|----------|-----------|-----|-----------|-------------------|
| Ado                | âœ“        |           |     | âœ“         | âœ“                 |
| AdoNet             | âœ“        |           |     | âœ“         | âœ“                 |
| Cache              |          | âœ“         | âœ“   |           |                   |
| Excel              |          | âœ“         | âœ“   |           |                   |
| Fichier               |          | âœ“         | âœ“   |           |                   |
| FlatFile           |          | âœ“         | âœ“   |           |                   |
| Ftp                |          |           |     | âœ“         |                   |
| MsOLAP100          |          |           |     | âœ“         | âœ“                 |
| MultiFile          |          | âœ“         | âœ“   |           |                   |
| MultiFlatFile      |          | âœ“         | âœ“   |           |                   |
| OData              | âœ“        |           |     | âœ“         |                   |
| Odbc               | âœ“        |           |     | âœ“         | âœ“                 |
| OleDb              | âœ“        |           |     | âœ“         | âœ“                 |
| SmoServer          | âœ“        |           |     | âœ“         |                   |
| Smtp               | âœ“        |           |     | âœ“         |                   |
| SqlMobile          |          | âœ“         | âœ“   |           |                   |
| Wmi                | âœ“        |           |     |           |                   |
|||||||

## <a name="validate-sources-and-destinations"></a>Valider les sources et les destinations
Les sources et destinations tierces suivantes ne sont pas prises en charge :

-   Source et destination Oracle par Attunity
-   Source et destination Teradata par Attunity
-   Source et destination SAP BW

## <a name="validate-tasks-and-components"></a>Valider les tâches et les composants

### <a name="execute-process-task"></a>Tâche d'exécution de processus

La validation génère un avertissement si une commande pointe vers un fichier local avec un chemin absolu ou vers un fichier avec un chemin UNC. Ces chemins peuvent provoquer l’échec de l’exécution sur Azure.

### <a name="script-task-and-script-component"></a>Tâche de script et composant de script

La validation génère un avertissement si un package contient une tâche de script ou un composant de script qui peut référencer ou appeler des assemblys non pris en charge. Ces références ou appels peuvent provoquer l’échec de l’exécution.

### <a name="other-components"></a>Autres composants

Le format Orc n’est pas pris en charge dans la destination HDFS et la destination Azure Data Lake Store.

## <a name="next-steps"></a>Étapes suivantes
Pour découvrir comment planifier l’exécution d’un package sur Azure, consultez [Planifier l’exécution de packages SSIS sur Azure](ssis-azure-schedule-packages.md).