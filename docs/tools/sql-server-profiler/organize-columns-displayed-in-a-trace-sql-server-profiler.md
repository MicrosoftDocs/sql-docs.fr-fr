---
title: Organiser les colonnes affichées dans une trace
titleSuffix: SQL Server Profiler
description: Facilitez l’analyse de la sortie de suivi SQL Server Profiler en regroupant et en agrégeant des événements lorsque vous définissez ou affichez une trace.
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.technology: profiler
ms.topic: conceptual
ms.assetid: 6b923f94-0eb1-467e-82f6-ceed43f77017
author: markingmyname
ms.author: maghan
ms.custom: seo-lt-2019
ms.date: 03/01/2017
ms.openlocfilehash: 2727aa7ed5e95881e21eddb73a94ddef97795c08
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100353413"
---
# <a name="organize-columns-displayed-in-a-trace-sql-server-profiler"></a>Organiser les colonnes affichées dans une trace (SQL Server Profiler)

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

 Vous pouvez grouper les colonnes de données dans une trace en sélectionnant **Organiser les colonnes** dans la table de trace ou dans la boîte de dialogue **Propriétés du fichier de suivi**, ou bien quand vous définissez une trace. Le regroupement des colonnes de données vous permet de mieux analyser la sortie de trace du [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] . Pour plus d’informations, consultez [Afficher et analyser des traces avec SQL Server Profiler](../../tools/sql-server-profiler/view-and-analyze-traces-with-sql-server-profiler.md).  
  
 L’option **Organiser les colonnes** vous permet de grouper les événements de trace ou de les grouper et de les agréger en fonction des colonnes de données que vous sélectionnez.  
  
-   Choisissez plusieurs colonnes de données pour regrouper seulement des événements de trace. Lorsque vous choisissez plusieurs colonnes de données de regroupement, la fenêtre de trace affiche les événements en les groupant en fonction des valeurs de ces colonnes de données. L’exemple suivant indique comment la grille de la fenêtre de trace apparaît si vous choisissez les colonnes de données **Duration** et **StartTime** pour le regroupement. Notez que les valeurs de la colonne **Duration** apparaissent en ordre croissant, suivies de celles de la colonne **StartTime** .  
  
|Duration|StartTime|EventClass|ClientProcessID|  
|--------------|---------------|----------------|---------------------|  
||12/12/2006 3:16:43 PM|SQL:StmtStarting|2124|  
|0|12/12/2006 5:39:23 PM|Audit Login|648|  
|1|12/12/2006 5:24:44 PM|SQL:StmtStarting|2124|  
|25|12/12/2006 5:24:44 PM|SQL:StmtCompleted|648|  
  
-   Choisissez une seule colonne de regroupement pour grouper et agréger les événements de trace. Lorsque vous ne choisissez qu'une colonne de données pour le regroupement, la fenêtre de trace affiche les événements en les groupant en fonction des valeurs de cette colonne de données et réduit tous les événements sous celle-ci. Un signe positif ( **+** ) apparaît à gauche de l’événement dans la colonne de données choisie pour le regroupement, tandis que le nombre d’événements réduits sous celle-ci apparaît entre parenthèses à droite de l’événement. L’exemple suivant indique comment la grille de la fenêtre de trace apparaît si vous choisissez uniquement la colonne de données **EventClass** pour le regroupement. Notez que tous les événements sont organisés sous la colonne de données **EventClass** . Pour afficher tous les événements, cliquez sur le signe plus pour développer et afficher toutes les classes d'événements correspondantes.  
  
|EventClass|StartTime|Duration|ClientProcessID|  
|----------------|---------------|--------------|---------------------|  
|+ ExistingConnection (6)||||  
|+ SQL:BatchStarting (25)||||  
|+ SQL:StmtCompleted (11)||||  
|+ SQL:SmtStarting (21)||||  
  
### <a name="to-group-data-columns-displayed-in-a-trace"></a>Pour grouper des colonnes de données affichées dans une trace  
  
1.  Ouvrez une table de trace ou un fichier existant.  
  
2.  Dans le menu **Fichier** , cliquez sur **Propriétés**.  
  
3.  Dans la boîte de dialogue **Propriétés du fichier de trace** ou **Propriétés de la table de trace** , cliquez sur l’onglet **Sélection des événements** .  
  
4.  Sous l’onglet **Sélection des événements** , cliquez sur **Organiser les colonnes**.  
  
5.  Dans la boîte de dialogue **Organiser les colonnes** , sélectionnez les colonnes à afficher dans un groupe, puis cliquez sur **Monter** pour les déplacer sous **Groupes**. Après avoir déplacé les colonnes de votre choix sous **Groupes**, vous pouvez utiliser les boutons **Monter** et **Descendre** pour réorganiser leur ordre.  
  
     Lorsque vous déplacez des noms de colonne de données dans la liste **Groupes** , la trace affichée est d’abord organisée en fonction des valeurs de la colonne de données qui occupe la première position dans la liste **Groupes** , puis en fonction de celles de la deuxième colonne de données dans la liste **Groupes** , et ainsi de suite.  
  
6.  Cliquez sur **OK** dans la boîte de dialogue **Organiser les colonnes** , puis sur **OK** dans la boîte de dialogue **Propriétés de la table de trace** ou **Propriétés du fichier de trace** .  
  
     Une fois que vous avez cliqué sur **OK** dans la boîte de dialogue **Propriétés de la table de trace** ou **Propriétés du fichier de trace** , les colonnes de données sont réorganisées dans la trace affichée. La colonne de données que vous avez déplacée en première position de la liste **Groupes** occupe la première place dans l’affichage de la trace lorsque vous lisez la grille de la gauche vers la droite. Les lignes de la trace sont organisées dans l’ordre croissant en fonction des valeurs des colonnes de données que vous avez incluses dans la liste **Groupes** . Les colonnes choisies pour le regroupement occupent une position fixe dans l'affichage, mais vous pouvez faire défiler celui-ci vers la droite ou vers la gauche pour afficher les autres colonnes.  
  
7.  Pour dissocier les données de trace affichées, cliquez sur **Vue groupée** dans le menu **Affichage** pour annuler la sélection. Si vous souhaitez revenir à la vue groupée, cliquez de nouveau sur **Vue groupée** dans le menu **Affichage** afin de la sélectionner à nouveau.  
  
### <a name="to-group-and-aggregate-data-columns-in-a-trace"></a>Pour grouper et agréger des colonnes de données dans une trace  
  
1.  Ouvrez une table de trace ou un fichier existant.  
  
2.  Dans le menu **Fichier** , cliquez sur **Propriétés**.  
  
3.  Dans la boîte de dialogue **Propriétés du fichier de trace** ou **Propriétés de la table de trace** , cliquez sur l’onglet **Sélection des événements** .  
  
4.  Sous l’onglet **Sélection des événements** , cliquez sur **Organiser les colonnes**.  
  
5.  Dans la boîte de dialogue **Organiser les colonnes** , sélectionnez une colonne à partir de laquelle vous souhaitez grouper et agréger les événements de trace affichés. Cliquez sur **Monter** pour déplacer le nom de colonne sous **Groupes**. Si nécessaire, vous pouvez utiliser les boutons **Monter** et **Descendre** pour réorganiser les autres colonnes sous **Colonnes** .  
  
6.  Cliquez sur **OK** dans la boîte de dialogue **Organiser les colonnes** , puis sur **OK** dans la boîte de dialogue **Propriétés de la table de trace** ou **Propriétés du fichier de trace** .  
  
     Une fois que vous avez cliqué sur **OK** dans la boîte de dialogue **Propriétés de la table de trace** ou **Propriétés du fichier de trace** , les colonnes de données sont réorganisées dans la trace affichée. Tous les autres événements de colonne de données sont agrégés sous la colonne de données que vous avez déplacée dans la liste **Groupes** . Cliquez sur le signe plus ( **+** ) à gauche de l’événement dans la colonne de données que vous avez choisie pour l’agrégation pour le développer et afficher tous les événements de ce type. Le colonne choisie pour l'agrégation occupe une position fixe dans l'affichage, mais vous pouvez faire défiler celui-ci vers la droite ou vers la gauche pour afficher les autres colonnes.  
  
7.  Pour revenir à une vue normale des données de trace, cliquez sur **Vue agrégée** dans le menu **Affichage** , pour annuler la sélection. Si vous souhaitez revenir à la vue agrégée, cliquez de nouveau sur **Vue agrégée** dans le menu **Affichage** afin de la sélectionner à nouveau. Vous pouvez également cliquer sur **Vue groupée** dans le menu **Affichage** pour afficher les événements de trace groupés sans les réduire.  
  
## <a name="see-also"></a>Voir aussi  
 [Créer une trace &#40;SQL Server Profiler&#41;](../../tools/sql-server-profiler/create-a-trace-sql-server-profiler.md)   
 [Ouvrir une table de trace &#40;SQL Server Profiler&#41;](../../tools/sql-server-profiler/open-a-trace-table-sql-server-profiler.md)   
 [Ouvrir un fichier de trace &#40;SQL Server Profiler&#41;](../../tools/sql-server-profiler/open-a-trace-file-sql-server-profiler.md)  
  
  
