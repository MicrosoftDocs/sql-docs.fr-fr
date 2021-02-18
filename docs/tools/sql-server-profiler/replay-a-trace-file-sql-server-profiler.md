---
title: Relire un fichier de trace
titleSuffix: SQL Server Profiler
description: Obtenir de l’aide pour la résolution des problèmes en relisant les fichiers de trace dans SQL Server Profiler. En savoir plus sur les fonctionnalités et les options de relecture.
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.technology: profiler
ms.topic: conceptual
ms.assetid: 9e361275-c8fd-4499-8389-242cf8e27415
author: markingmyname
ms.author: maghan
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.openlocfilehash: b7d6139941e7ee3b80e62596e95f0d5d492cbc1d
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100349386"
---
# <a name="replay-a-trace-file-sql-server-profiler"></a>Relire un fichier de trace (SQL Server Profiler)

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

La relecture est la possibilité d'ouvrir une trace enregistrée et de la relire. [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] contient un moteur de lecture à plusieurs threads capable de simuler les connexions utilisateur et l’authentification [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . La relecture est utile pour résoudre les problèmes d'applications ou de processus. Lorsque vous identifiez le problème et appliquez des corrections, exécutez la trace qui a détecté le problème potentiel sur l'application ou le processus corrigé. Relisez ensuite la trace d'origine et comparez les résultats.  
  
 En plus des autres classes d'événements que vous voulez analyser, des classes d'événements spécifiques doivent être capturées pour permettre la relecture. Ces événements sont capturés par défaut si vous utilisez le modèle de trace **TSQL_Replay** . Pour plus d’informations, consultez [Conditions préalables à la relecture](../../tools/sql-server-profiler/replay-requirements.md).  
  
### <a name="to-replay-a-trace-file"></a>Pour relire un fichier de trace  
  
1.  Dans le menu **Fichier** , pointez sur **Ouvrir**, puis cliquez sur **Fichier de trace**. Sélectionnez un fichier de trace qui contient les classes d'événements indispensables pour la relecture.  
  
2.  Dans le menu **Relire** , cliquez sur **Démarrer**, puis connectez-vous à l’instance de serveur sur laquelle vous voulez relire la trace.  
  
3.  Dans la boîte de dialogue **Configuration de la relecture** , sous l’onglet **Options de relecture de base** , spécifiez le **Serveur de relecture**. Cliquez sur **Modifier** pour changer le serveur affiché dans la zone **Serveur de relecture** .  
  
4.  Si vous le souhaitez, sélectionnez l'une des destinations suivantes afin d'y enregistrer la relecture :  
  
    -   **Enregistrer dans le fichier**: spécifie un fichier dans lequel la relecture sera enregistrée.  
  
    -   **Enregistrer dans la table**: spécifie une table de la base de données dans laquelle la relecture sera enregistrée.  
  
5.  Choisissez **Relire les événements selon leur ordre de suivi** ou **Relire les événements en utilisant plusieurs threads**. Le tableau suivant explique la différence entre ces paramètres.  
  
    |Option|Description|  
    |------------|-----------------|  
    |**Relire les événements selon leur ordre de suivi**|Permet de relire les événements selon l'ordre dans lequel ils ont été enregistrés. Cette option active le débogage.|  
    |**Relire les événements en utilisant plusieurs threads**|Cette option utilise plusieurs threads pour relire chaque événement indépendamment de la séquence. Cette option optimise les performances.|  
  
6.  Sélectionnez **Afficher les résultats de relecture** pour consulter la relecture lorsqu’elle a lieu.  
  
7.  Si vous le souhaitez, cliquez sur l’onglet **Options de relecture avancées** pour configurer les options suivantes :  
  
    -   Pour relire tous les ID de processus de serveur (SPID), sélectionnez **Relire les SPID système**.  
  
    -   Pour limiter la relecture aux processus appartenant à un SPID spécifique, sélectionnez **Relire un SPID uniquement**. Dans la zone **SPID à relire** , tapez le SPID.  
  
    -   Pour relire les événements qui se sont produits durant une période spécifique, sélectionnez **Limiter la relecture par date et heure**. Sélectionnez une date et une heure pour **Heure de début** et **Heure de fin** afin de spécifier la période à inclure dans la relecture.  
  
    -   Pour déterminer la manière dont SQL Server gèrera les processus durant la relecture, configurez les **Options du moniteur d’intégrité**.  
  
## <a name="see-also"></a>Voir aussi  
 [Autorisations nécessaires pour exécuter SQL Server Profiler](../../tools/sql-server-profiler/permissions-required-to-run-sql-server-profiler.md)   
 [Relire des traces](../../tools/sql-server-profiler/replay-traces.md)   
 [Ouvrir un fichier de trace &#40;SQL Server Profiler&#41;](../../tools/sql-server-profiler/open-a-trace-file-sql-server-profiler.md)   
 [SQL Server Profiler](../../tools/sql-server-profiler/sql-server-profiler.md)  
  
  
