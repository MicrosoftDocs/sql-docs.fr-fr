---
title: Filtrer les ID de processus serveur (SPID) dans un fichier de trace
titleSuffix: SQL Server Profiler
description: Découvrez comment limiter la sortie de trace dans SQL Server Profiler en appliquant un filtre sur l’ID de processus serveur (SPID).
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.technology: profiler
ms.topic: conceptual
ms.assetid: f5945c39-be6b-4632-91cb-92066c80e188
author: markingmyname
ms.author: maghan
ms.custom: seo-lt-2019
ms.date: 03/01/2017
ms.openlocfilehash: de293e3db8fa0ec05039ecac98534338375fd89f
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100335929"
---
# <a name="filter-server-process-ids-spids-in-a-trace-sql-server-profiler"></a>Filtrer des identificateurs de processus serveur (SPID, Server Process ID) dans une trace (SQL Server Profiler)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  Cette rubrique décrit comment filtrer les identificateurs de processus serveur (SPID, Server Process ID) dans une trace grâce au [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)].  
  
### <a name="to-filter-system-ids-in-a-trace"></a>Pour filtrer des ID système dans une trace  
  
1.  Dans le menu **Fichier** , cliquez sur **Nouvelle trace**, puis connectez-vous à une instance de SQL Server.  
  
     La boîte de dialogue **Propriétés de la trace** s'affiche.  
  
    > [!NOTE]  
    >  Si la case **Démarrer le suivi juste après avoir établi la connexion** est cochée, la boîte de dialogue **Propriétés de la trace** ne s’affiche pas et le suivi se lance. Pour désactiver ce paramètre, dans le menu **Outils**, cliquez sur **Options**, puis désactivez la case à cocher **Démarrer le suivi juste après avoir établi la connexion**.  
  
2.  Dans la zone **Nom de la trace** , tapez un nom pour la trace.  
  
3.  Dans la liste **Utiliser le modèle**, sélectionnez un modèle de trace.  
  
4.  Le cas échéant, spécifiez un fichier ou une table de destination pour y enregistrer les résultats du suivi.  
  
5.  Sous l’onglet **Sélection des événements**, cliquez sur l’en-tête de la colonne **SPID** pour ouvrir la boîte de dialogue **Modifier le filtre**. Vous pouvez également cliquer avec le bouton droit de la souris sur l’en-tête de colonne et choisir **Modifier le filtre des colonnes** parmi les éléments du menu contextuel. Si la colonne **SPID** n’apparaît pas, cochez la case **Afficher toutes les colonnes** .  
  
6.  Dans la boîte de dialogue **Modifier le filtre** , développez l’opérateur de comparaison voulu et entrez le SPID correspondant à la valeur de comparaison.  
  
## <a name="see-also"></a>Voir aussi  
 [SQL Server Profiler](../../tools/sql-server-profiler/sql-server-profiler.md)  
  
  
