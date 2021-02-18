---
title: Analyser les blocages
titleSuffix: SQL Server Profiler
description: Identifiez la cause d’un blocage en relisant et en affichant les événements de blocage pour l’analyse dans SQL Server Profiler et en générant des graphiques d’attente (WFG).
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.technology: profiler
ms.topic: conceptual
ms.assetid: 72d6718f-501b-4ea6-b344-c0e653f19561
author: markingmyname
ms.author: maghan
ms.custom: seo-lt-2019
ms.date: 03/03/2017
ms.openlocfilehash: 373bcce8731dad1375efd9a4c2196ab184b38954
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100349459"
---
# <a name="analyze-deadlocks-with-sql-server-profiler"></a>Analyser des blocages à l'aide de SQL Server Profiler

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

Utilisez le [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] pour identifier la cause d'un interblocage. Un interblocage se produit quand il y a une dépendance cyclique entre au moins deux threads ou processus pour un jeu de ressources dans SQL Server. Le [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)]vous permet de créer une trace qui enregistre, relit et affiche les événements de blocage dans le cadre d'une analyse.  
  
 Pour tracer les événements de blocage, ajoutez la classe d’événements **Deadlock graph** à une trace. Cette classe d’événements remplit la colonne de données **TextData** dans la trace avec des données XML relatives aux processus et objets impliqués dans le blocage. [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] peut extraire le document XML dans un fichier XML de blocages (.xdl) que vous pouvez afficher ultérieurement dans SQL Server Management Studio. Vous pouvez configurer le [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] de manière à extraire les événements **Deadlock graph** vers un fichier unique contenant tous les événements **Deadlock graph** , ou bien vers des fichiers distincts. Cette extraction peut être réalisée de l'une des manières suivantes :  
  
-   Au moment de la configuration de la trace, à l’aide de l’onglet **Paramètres d’extraction des événements** . Cet onglet n’apparaît que si vous sélectionnez l’événement **Deadlock graph** sous l’onglet **Sélection des événements** .  
  
-   À l’aide de l’option **Extraire les événements SQL Server** du menu **Fichier** .  
  
-   Vous pouvez également extraire et enregistrer un événement donné en cliquant avec le bouton droit sur celui-ci et en choisissant **Extraire les données d’événement**.  
  
## <a name="deadlock-graphs"></a>Graphiques de blocage  
 [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] et [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] utilisent un graphique WAITFOR de blocage pour décrire un blocage. Le graphique WAITFOR de blocage contient des nœuds de processus, des nœuds de ressources et des arêtes qui représentent les relations entre les processus et les ressources. Les composants des graphiques WAITFOR sont définis dans la table suivante :  
  
 Nœud de processus  
 Thread qui réalise une tâche ; par exemple, INSERT, UPDATE ou DELETE.  
  
 Nœud de ressource  
 Objet de base de données ; par exemple, une table, un index ou une ligne.  
  
 Edge  
 Relation entre un processus et une ressource. Une arête **request** se produit quand un processus attend une ressource. Une arête **owner** se produit quand une ressource attend un processus. Le mode de verrouillage est inclus dans la description des arêtes. Par exemple, **Mode : X**.  
  
## <a name="deadlock-process-node"></a>Nœud de processus de blocage  
 Dans un graphique WAITFOR, le nœud de processus contient des informations sur le processus. Le tableau suivant décrit les composants d'un processus.  
  
|Composant|Définition|  
|---------------|----------------|  
|ID de processus serveur|SPID, identificateur affecté par un serveur pour le processus détenant le verrou.|  
|ID de traitement du serveur|Identificateur de traitement du serveur (SBID).|  
|ID du contexte d'exécution|Identificateur du contexte d'exécution (ECID). ID de contexte d'exécution d'un thread particulier associé à un SPID spécifique<br /><br /> ECID = {0,1,2,3, *...n*}, où 0 représente toujours le thread principal ou parent et {1,2,3, *...n*} les sous-threads.|  
|Priorité de blocage|Priorité de blocage du processus. Pour plus d’informations sur les valeurs possibles, consultez [SET DEADLOCK_PRIORITY &#40;Transact-SQL&#41;](../../t-sql/statements/set-deadlock-priority-transact-sql.md).|  
|Journal utilisé|Quantité d'espace journal utilisée par le processus.|  
|ID de propriétaire|ID de transaction des processus qui utilisent des transactions et qui attendent un verrou.|  
|Descripteur de transaction|Pointeur vers le descripteur de transaction qui décrit l'état de la transaction.|  
|Mémoire tampon d'entrée|Mémoire tampon d'entrée du processus actuel, qui définit le type d'événement et l'instruction en cours d'exécution. Les valeurs possibles incluent :<br /><br /> **Langage**<br /><br /> **RPC**<br /><br /> **Aucun**|  
|.|Type d'instruction. Les valeurs possibles sont les suivantes :<br /><br /> **NOP**<br /><br /> **SELECT**<br /><br /> **UPDATE**<br /><br /> **INSERT**<br /><br /> **DELETE**<br /><br /> **Unknown**|  
  
## <a name="deadlock-resource-node"></a>Nœud de ressource de blocage  
 Dans un blocage, chacun des deux processus attend une ressource détenue par l'autre processus. Dans un graphique de blocage, les ressources apparaissent sous la forme de nœuds de ressources.  
  
  
