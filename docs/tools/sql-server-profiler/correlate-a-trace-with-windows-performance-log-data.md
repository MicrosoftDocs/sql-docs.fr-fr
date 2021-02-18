---
title: Mettre en corrélation une trace avec les données du journal de performances Windows
titleSuffix: SQL Server Profiler
description: Découvrez comment corréler des journaux de performances Windows avec une trace afin de pouvoir afficher les données des objets et des compteurs du moniteur système dans SQL Server Profiler.
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.technology: profiler
ms.topic: conceptual
ms.assetid: 1e4412c8-d27c-4aae-9b35-214128d1d00a
author: markingmyname
ms.author: maghan
ms.custom: seo-lt-2019
ms.date: 07/12/2017
ms.openlocfilehash: 9279078aee9a27b647a9f350756e317ae06cbda5
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100349408"
---
# <a name="correlate-a-trace-with-windows-performance-log-data"></a>Mettre en corrélation une trace avec les données du journal de performances Windows

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

[!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)]permet d'ouvrir un journal de performances Microsoft Windows, de choisir les compteurs que vous voulez corréler avec une trace et d'afficher les compteurs de performances sélectionnés en même temps que la trace dans l'interface utilisateur graphique du [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] . Lorsque vous sélectionnez un événement dans la fenêtre de trace, une barre verticale rouge dans le volet Moniteur système du [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] indique les données du journal de performances en corrélation avec l'événement de trace sélectionné.  
  
 Pour mettre en corrélation une trace avec des compteurs de performances, ouvrez un fichier de trace ou une table qui contient les colonnes **StartTime** et **EndTime** data columns, et cliquez sur **Importer les données de performances** dans le menu [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] **File**. Vous pouvez ouvrir un journal de performances et sélectionner les objets et compteurs du Moniteur système que vous voulez mettre en corrélation avec la trace.  
  
### <a name="to-correlate-a-trace-with-performance-log-data"></a>Pour corréler une trace aux données du journal de performances  
  
1.  Dans [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)], ouvrez un fichier de trace ou une table de trace enregistrée. Il est impossible de corréler une trace d'exécution qui regroupe encore des données d'événement. Pour garantir la précision de la corrélation aux données du Moniteur système, la trace doit contenir les colonnes de données **StartTime** et **EndTime** .  
  
2.  Dans le menu [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] **File**, cliquez sur **Importer les données de performance**.  
  
3.  Dans la boîte de dialogue **Ouvrir** , sélectionnez un fichier qui contient un journal de performances. Les données du journal de performances doivent être capturées pendant la même période que celle de la capture des données de trace.  
  
4.  Dans la boîte de dialogue **Limite des compteurs de performances** , activez les cases à cocher correspondant aux objets et aux compteurs du Moniteur système que vous souhaitez afficher aux côtés de la trace. Cliquez sur **OK**.  
  
5.  Sélectionnez un événement dans la fenêtre des événements de trace ou faites défiler plusieurs lignes adjacentes dans la fenêtre des événements de trace à l'aide des touches de direction. La barre rouge verticale de la fenêtre **Données du Moniteur système** indique les données du journal de performances corrélées à l’événement de trace sélectionné.  
  
6.  Cliquez sur un point qui vous intéresse dans le graphique du Moniteur système. La ligne de trace correspondante la plus proche dans le temps est sélectionnée. Pour agrandir un intervalle de temps, cliquez sur le pointeur de la souris et faites-le glisser dans le graphique du Moniteur système.  
  
### <a name="to-create-performance-logs-that-can-be-shared-among-different-versions-of-windows"></a>Pour créer des journaux de performances pouvant être partagés avec différentes versions de Windows  
  
1.  Dans le Panneau de configuration, ouvrez **Outils d’administration**, puis double-cliquez sur **Performances**.  
  
2.  Dans la boîte de dialogue **Performances** , développez **Journaux et alertes de performances**, cliquez avec le bouton droit sur **Journaux de compteurs**, puis cliquez sur **Nouveaux paramètres de journal**.  
  
3.  Tapez un nom pour le journal de compteurs, puis cliquez sur **OK**.  
  
4.  Sous l’onglet **Général** , cliquez sur **Ajouter des compteurs**.  
  
5.  Dans la liste **Objet de performance** , sélectionnez un objet de performance à surveiller. Les noms des objets de performances [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pour des instances par défaut de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] commencent par [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et les instances nommées commencent par MSSQL$*instanceName*.  
  
6.  Ajoutez autant de compteurs que nécessaire pour votre instance [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et d'autres valeurs importantes, telles que le temps processeur et le temps du disque.  
  
7.  Quand vous avez terminé d’ajouter les compteurs, cliquez sur **Fermer**.  
  
8.  Définissez les valeurs de l’intervalle **Période d’échantillonnage des données** . Commencez par un intervalle modeste, par exemple 5 minutes, puis modifiez-le si nécessaire.  
  
9. Sous l’onglet **Fichiers journaux** choisissez **Fichier texte (séparé par des virgules)** dans la liste **Type de fichier journal** . Vous pouvez partager entre plusieurs versions de Windows les fichiers texte de journal délimités par une virgule et les afficher ultérieurement dans des outils de création de rapports tels que Microsoft Excel.  
  
10. Sous l’onglet **Planification** , spécifiez une planification de surveillance.  
  
11. Cliquez sur **OK** pour créer le journal de performances.  
