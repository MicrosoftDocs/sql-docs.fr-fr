---
title: Spécifier les événements et les colonnes de données d’un fichier de trace
titleSuffix: SQL Server Profiler
description: Découvrez comment spécifier les classes d’événements et les colonnes de données que SQL Server Profiler comprend lorsqu’il capture des données d’événement dans des traces.
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.technology: profiler
ms.topic: conceptual
ms.assetid: 7da715a3-2f03-4063-b6a4-20fd7b44e675
author: markingmyname
ms.author: maghan
ms.custom: seo-lt-2019
ms.date: 03/01/2017
ms.openlocfilehash: 660917a46529e62a33565c75ca92501ddfb8a140
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100345741"
---
# <a name="specify-events-and-data-columns-for-a-trace-file-sql-server-profiler"></a>Spécifier les événements et les colonnes de données d'un fichier de trace (SQL Server Profiler)

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

Cette rubrique explique comment spécifier les classes d'événements et les colonnes de données des traces à l'aide du [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)].  
  
### <a name="to-specify-events-and-data-columns-for-a-trace"></a>Pour spécifier les événements et les colonnes de données d'une trace  
  
1.  Dans la boîte de dialogue **Propriétés de la trace** ou **Propriétés du modèle de trace** , cliquez sur l’onglet **Sélection des événements** .  
  
     L’onglet **Sélection des événements** contient un contrôle de grille. Le contrôle de grille est une table qui contient chacune des classes d'événements traçables. La table contient une ligne par classe d'événements. Les classes d'événements peuvent différer légèrement selon le type et la version du serveur auquel vous êtes connecté. Les classes d’événements sont identifiées dans la colonne **Events** de la grille, et groupées par catégorie d’événement. Les autres colonnes répertorient les colonnes de données pouvant être retournées pour chaque classe d'événements.  
  
2.  Sous l’onglet **Sélection des événements**, utilisez la grille pour ajouter ou supprimer des événements et des colonnes de données dans le fichier de trace.  
  
3.  Pour supprimer des événements de la trace, décochez la case dans la colonne **Événements** pour chaque classe d’événements.  
  
4.  Pour inclure des événements dans une trace, cochez la case dans la colonne **Événements** pour chaque classe d’événements ou activez une colonne de données qui correspond à un événement.  
  
> [!IMPORTANT]  
>  Si vous envisagez de corréler la trace avec les données du Moniteur système ou de l’Analyseur de performances, vous devez inclure dans la trace les colonnes de données **Heure de début** et **Heure de fin** .  
  
 Lorsque vous incluez une classe d'événements, chaque colonne de données associée est également insérée dans la trace, sous réserve que vous ayez activé la case à cocher d'un événement. Si vous avez activé la case à cocher d'une colonne particulière, seule cette colonne est incluse dans la trace.  
  
1.  Pour supprimer des colonnes de données d’une classe d’événements, décochez les cases de la colonne de données dans la ligne de la classe d’événements, ou cliquez avec le bouton droit sur l’en-tête de colonne, puis sélectionnez l’option **Désélectionner la colonne** .  
  
2.  Appliquez éventuellement des filtres à la trace. Pour plus d’informations, consultez [Filtrer des événements dans une trace &#40;SQL Server Profiler&#41;](../../tools/sql-server-profiler/filter-events-in-a-trace-sql-server-profiler.md)  
  
## <a name="see-also"></a>Voir aussi  
 [SQL Server Profiler](../../tools/sql-server-profiler/sql-server-profiler.md)  
  
  
