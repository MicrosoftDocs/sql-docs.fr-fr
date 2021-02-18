---
title: Relire une table de trace
titleSuffix: SQL Server Profiler
description: Obtenir de l’aide pour la résolution des problèmes en relisant les tables de trace dans SQL Server Profiler. En savoir plus sur les fonctionnalités et les options de relecture.
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.technology: profiler
ms.topic: conceptual
ms.assetid: 6a0ad817-3d8d-4495-889d-c66a7ef9e8bb
author: markingmyname
ms.author: maghan
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.openlocfilehash: 1e56ef7a889826c2a3d6f51e06b403b88ecc8cda
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100353440"
---
# <a name="replay-a-trace-table-sql-server-profiler"></a>Relire une table de trace (SQL Server Profiler)

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

La relecture est la possibilité d'ouvrir une trace enregistrée et de la relire. [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] contient un moteur de lecture à plusieurs threads capable de simuler les connexions utilisateur et l’authentification [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . La relecture est utile pour résoudre les problèmes d'applications ou de processus. Lorsque vous identifiez le problème et appliquez des corrections, exécutez la trace qui a détecté le problème potentiel sur l'application ou le processus corrigé. Relisez ensuite la trace d'origine et comparez les résultats.  
  
 En plus des autres classes d'événements que vous voulez analyser, des classes d'événements spécifiques doivent être capturées pour permettre la relecture. Ces événements sont capturés par défaut si vous utilisez le modèle de trace **TSQL_Replay** . Pour plus d’informations, consultez [Conditions préalables à la relecture](../../tools/sql-server-profiler/replay-requirements.md).  
  
## <a name="to-replay-a-trace-table"></a>Pour relire une table de trace
  
1.  Ouvrez une table de trace contenant les classes d'événements nécessaires à la relecture.  
  
2.  Dans le menu **Relire** , cliquez sur **Démarrer**, puis connectez-vous à l’instance de serveur sur laquelle vous voulez relire la trace.  
  
3.  Dans la boîte de dialogue **Configuration de la relecture** , sous l’onglet **Options de relecture de base** , spécifiez **Serveur de relecture**. Cliquez sur **Modifier** pour modifier le serveur affiché dans la zone **Serveur de relecture** .  
  
4.  Si vous le souhaitez, sélectionnez l'une des destinations suivantes afin d'y enregistrer la relecture :  
  
    -   **Enregistrer dans le fichier** , ce qui spécifie un fichier dans lequel la relecture sera enregistrée.  
  
    -   **Enregistrer dans la table**: spécifie une table de la base de données dans laquelle la relecture sera enregistrée.  
  
5.  Choisissez **Relire les événements selon leur ordre de suivi** ou **Relire les événements en utilisant plusieurs threads**. Le tableau suivant explique la différence entre ces paramètres.  
  
    |Option|Description|  
    |------------|-----------------|  
    |**Relire les événements selon leur ordre de suivi**|Permet de relire les événements selon l'ordre dans lequel ils ont été enregistrés. Cette option active le débogage.|  
    |**Relire les événements en utilisant plusieurs threads**|Cette option utilise plusieurs threads pour relire chaque événement indépendamment de la séquence. Cette option optimise les performances.|  
  
6.  Sélectionnez **Afficher les résultats de relecture** pour consulter la relecture lorsqu’elle a lieu.  
  
7.  Vous pouvez aussi cliquer sur l’onglet **Options de relecture avancées** pour spécifier les options suivantes :  
  
    -   Pour relire tous les ID de processus de serveur (SPID), sélectionnez **Relire les SPID système**.  
  
    -   Pour limiter la relecture aux processus appartenant à un SPID spécifique, sélectionnez **Relire un SPID uniquement**. Dans la zone **SPID à relire**, tapez le SPID.  
  
    -   Pour relire les événements qui se sont produits pendant une période de temps spécifique, sélectionnez **Limiter la relecture par date et heure**. Sélectionnez une date et une heure pour **Heure de début** et **Heure de fin** afin de spécifier la période à inclure dans la relecture.  
  
    -   Pour contrôler la façon dont [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] gère les processus pendant la relecture, configurez les **Options du moniteur d’intégrité**.  
  
## <a name="see-also"></a>Voir aussi  
 [Autorisations nécessaires pour exécuter SQL Server Profiler](../../tools/sql-server-profiler/permissions-required-to-run-sql-server-profiler.md)   
 [Relire des traces](../../tools/sql-server-profiler/replay-traces.md)   
 [Ouvrir une table de trace &#40;SQL Server Profiler&#41;](../../tools/sql-server-profiler/open-a-trace-table-sql-server-profiler.md)   
 [SQL Server Profiler](../../tools/sql-server-profiler/sql-server-profiler.md)  
  
  
