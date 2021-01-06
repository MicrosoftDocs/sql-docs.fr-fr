---
title: Utilitaires d’invite de commandes SQL (moteur de base de données)
description: Les utilitaires d’invite de commandes vous permettent d’écrire des scripts d’opérations SQL Server. Cet article répertorie de nombreux utilitaires d’invite de commandes fournis avec SQL Server.
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: tools-other
ms.topic: conceptual
helpviewer_keywords:
- command prompt utilities [SQL Server]
- command prompt utilities [SQL Server], about command prompt utilities
- command prompt [SQL Server]
- utilities [SQL Server], command prompt
- command prompt [SQL Server], utilities
ms.assetid: 48364bd9-6ea7-45e9-a332-acf3d81bbfae
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 03/14/2017
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017'
ms.openlocfilehash: 39e73066aad7c8913225c9b7c0e06f123e53a123
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/17/2020
ms.locfileid: "97644149"
---
# <a name="sql-command-prompt-utilities-database-engine"></a>Utilitaires d’invite de commandes SQL (moteur de base de données)

[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Les utilitaires d'invite de commandes vous permettent d'écrire des scripts d'opérations [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] . Le tableau suivant présente la plupart des utilitaires d’invite de commandes fournis avec [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)].

Pour plus d’informations sur l’interface utilisateur graphique et les outils en ligne de commande SQL *principaux*, consultez [Vue d’ensemble des outils SQL](overview-sql-tools.md).

|**Utilitaire**|**Description**|**Installé dans**|  
|-----------------|---------------------|----------------------|  
|[Utilitaire bcp](../tools/bcp-utility.md)|Utilisé pour copier des données entre une instance de [!INCLUDE[msCoName](../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] et un fichier de données dans un format spécifié par l’utilisateur.|\<*drive*:>\Program Files\\[!INCLUDE[msCoName](../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]\Client SDK\ODBC\110\Tools\Binn|  
|[Utilitaire dta](../tools/dta/dta-utility.md)|Sert à analyser une charge de travail et à recommander des structures PDS (Physical Design Structures) permettant d'optimiser les performances du serveur pour cette charge de travail.|[!INCLUDE[ssInstallPathVar](../includes/ssinstallpathvar-md.md)]Tools\Binn|  
|[Utilitaire dtexec](../integration-services/packages/dtexec-utility.md)|Sert à configurer et à exécuter un package [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] . La version interface utilisateur de cet utilitaire d’invite de commandes se nomme **DTExecUI** et ouvre l’utilitaire d’exécution de package.|[!INCLUDE[ssInstallPathVar](../includes/ssinstallpathvar-md.md)]DTS\Binn|  
|[Utilitaire dtutil](../integration-services/dtutil-utility.md)|Utilisé pour gérer les packages SSIS.|[!INCLUDE[ssInstallPathVar](../includes/ssinstallpathvar-md.md)]DTS\Binn|  
|[Déployer des solutions de modèle avec l’utilitaire de déploiement](/analysis-services/multidimensional-models/deploy-model-solutions-with-the-deployment-utility)|Sert à déployer les projets [!INCLUDE[ssASnoversion](../includes/ssasnoversion-md.md)] dans des instances d' [!INCLUDE[ssASnoversion](../includes/ssasnoversion-md.md)].|[!INCLUDE[ssInstallPathVar](../includes/ssinstallpathvar-md.md)]Tools\Binn\VShell\Common7\IDE|   
|[Utilitaire osql](../tools/osql-utility.md)|Vous permet d'entrer des instructions, des procédures système et des fichiers de script [!INCLUDE[tsql](../includes/tsql-md.md)] à l'invite de commandes.|[!INCLUDE[ssInstallPathVar](../includes/ssinstallpathvar-md.md)]Tools\Binn|  
|[Utilitaire profiler](../tools/profiler-utility.md)|Sert à démarrer [!INCLUDE[ssSqlProfiler](../includes/sssqlprofiler-md.md)] à partir d'une invite de commandes.|[!INCLUDE[ssInstallPathVar](../includes/ssinstallpathvar-md.md)]Tools\Binn|  
|[Utilitaire RS.exe &#40;SSRS&#41;](../reporting-services/tools/rs-exe-utility-ssrs.md)|Sert à exécuter des scripts conçus pour gérer des serveurs de rapports [!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)] .|[!INCLUDE[ssInstallPathVar](../includes/ssinstallpathvar-md.md)]Tools\Binn|  
|[Utilitaire rsconfig &#40;SSRS&#41;](../reporting-services/tools/rsconfig-utility-ssrs.md)|Utilisé pour configurer une connexion de serveur de rapports.|[!INCLUDE[ssInstallPathVar](../includes/ssinstallpathvar-md.md)]Tools\Binn|  
|[Utilitaire rskeymgmt &#40;SSRS&#41;](../reporting-services/tools/rskeymgmt-utility-ssrs.md)|Sert à gérer des clés de chiffrement sur un serveur de rapports.|[!INCLUDE[ssInstallPathVar](../includes/ssinstallpathvar-md.md)]Tools\Binn|  
|[Application sqlagent90](../tools/sqlagent90-application.md)|Utilisé pour démarrer [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] Agent à partir d'une invite de commandes.|\<drive>:\Program Files\Microsoft SQL Server\\<*instance_name*>\MSSQL\Binn|  
|[Utilitaire sqlcmd](../tools/sqlcmd-utility.md)|Vous permet d'entrer des instructions, des procédures système et des fichiers de script [!INCLUDE[tsql](../includes/tsql-md.md)] à l'invite de commandes.|\<*drive*:>\Program Files\\[!INCLUDE[msCoName](../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]\Client SDK\ODBC\110\Tools\Binn|  
|[Utilitaire SQLdiag](../tools/sqldiag-utility.md)|Sert à recueillir des informations de diagnostic pour le service de support technique [!INCLUDE[msCoName](../includes/msconame-md.md)] .|[!INCLUDE[ssInstallPathVar](../includes/ssinstallpathvar-md.md)]Tools\Binn|  
|[Application sqllogship](../tools/sqllogship-application.md)|Permet aux applications d'effectuer des opérations de restauration, de copie et de sauvegarde et les tâches de nettoyage associées pour une configuration de l'envoi de journaux sans effectuer de travaux de restauration, de copie et de sauvegarde.|[!INCLUDE[ssInstallPathVar](../includes/ssinstallpathvar-md.md)]Tools\Binn|  
|[Utilitaire SqlLocalDB](../tools/sqllocaldb-utility.md)|Mode d'exécution de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] destiné aux développeurs de programme.|[!INCLUDE[ssInstallPathVar](../includes/ssinstallpathvar-md.md)]Tools\Binn|  
|[Utilitaire sqlmaint](../tools/sqlmaint-utility.md)|Sert à exécuter des plans de maintenance de bases de données créés dans des versions précédentes de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)].|\<drive>:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\Binn|  
|[Utilitaire sqlps](../tools/sqlps-utility.md)|Sert à exécuter des commandes et des scripts PowerShell. Charge et inscrit le fournisseur PowerShell [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] et les cmdlets.|[!INCLUDE[ssInstallPathVar](../includes/ssinstallpathvar-md.md)]Tools\Binn|  
|[Application sqlservr](../tools/sqlservr-application.md)|Sert à démarrer et arrêter une instance de [!INCLUDE[ssDE](../includes/ssde-md.md)] à partir de l'invite de commandes pour le dépannage.|\<drive>:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\Binn|  
|[Utilitaire Ssms](../ssms/ssms-utility.md)|Sert à démarrer [!INCLUDE[ssManStudioFull](../includes/ssmanstudiofull-md.md)] à partir d'une invite de commandes.|[!INCLUDE[ssInstallPathVar](../includes/ssinstallpathvar-md.md)]Tools\Binn\VSShell\Common7\IDE|  
|[Utilitaire tablediff](../tools/tablediff-utility.md)|Sert à comparer les données de deux tables pour détecter une non-convergence, ce qui s'avère utile lors du dépannage d'une topologie de réplication.|[!INCLUDE[ssInstallPathVar](../includes/ssinstallpathvar-md.md)]COM|  

## <a name="command-prompt-utilities-syntax-conventions"></a>Conventions régissant la syntaxe des utilitaires de ligne de commande  
  
|**Convention**|**Utilisé pour**|  
|--------------------|------------------|  
|MAJUSCULES|Instructions et termes utilisés au niveau du système d'exploitation.|  
|`monospace`|Exemples de commandes et de code de programmation.|  
|*italique*|Paramètres fournis par l'utilisateur.|  
|**gras**|Commandes, paramètres et autres éléments de la syntaxe qui doivent être tapés exactement de la manière indiquée.|  

## <a name="see-also"></a>Voir aussi

* [Replication Distribution Agent](../relational-databases/replication/agents/replication-distribution-agent.md)
* [Agent de lecture du journal des réplications](../relational-databases/replication/agents/replication-log-reader-agent.md)
* [Replication Merge Agent](../relational-databases/replication/agents/replication-merge-agent.md)
* [Agent de lecture de la file d’attente de réplication](../relational-databases/replication/agents/replication-queue-reader-agent.md)
* [Replication Snapshot Agent](../relational-databases/replication/agents/replication-snapshot-agent.md)