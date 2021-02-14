---
title: se connecter à une base de données et parcourir les objets existants
description: Découvrez comment utiliser l’Explorateur d’objets SQL Server dans Visual Studio pour vous connecter à des instances SQL locales et hors site.
ms.prod: sql
ms.technology: ssdt
ms.topic: conceptual
f1_keywords:
- sql.data.tools.connectionpicker.f1
ms.assetid: 9b331800-3806-4459-ac58-88cdc98124d3
author: markingmyname
ms.author: maghan
ms.reviewer: “”
ms.custom: seo-lt-2019
ms.date: 02/09/2017
ms.openlocfilehash: b2a8c7ae0bbd50598d6c476c0a065786e6efcddb
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100018419"
---
# <a name="how-to-connect-to-a-database-and-browse-existing-objects"></a>Procédure : se connecter à une base de données et parcourir les objets existants

La connexion à une base de données active, la conception ou la navigation dans son schéma et l'exécution de requêtes relatives à ses objets sont des tâches très courantes pour les administrateurs et les développeurs de base de données. L'Explorateur d'objets SQL Server de Visual Studio contient à présent un nœud **SQL Server** dédié, sous lequel toutes les instances SQL Server connectées et leurs bases de données sont regroupées dans une hiérarchie de type SSMS. Les instances SQL Server connectées peuvent être une instance sur site, telle que le serveur SQL Server 2008 en cours d'exécution, ou une instance SQL Azure hors site.  
  
La procédure suivante suppose que vous avez déjà installé l'exemple de base de données AdventureWorks. Utilisez [GitHub](https://github.com/Microsoft/sql-server-samples/releases/tag/adventureworks) pour rechercher et installer des exemples de bases de données pour différentes versions de SQL Server. Si vous préférez, vous pouvez aussi suivre la procédure et utiliser une base de données existante sur votre serveur.  
  
### <a name="to-connect-to-a-database-instance"></a>Pour se connecter à une instance de base de données  
  
1.  Dans Visual Studio, vérifiez que l’**Explorateur d’objets SQL Server** est ouvert. Si ce n’est pas le cas, cliquez sur le menu **Affichage** et sélectionnez **Explorateur d’objets SQL Server**.  
  
2.  Cliquez avec le bouton droit sur le nœud **SQL Server** dans l'**Explorateur d'objets SQL Server**, puis sélectionnez **Ajouter SQL Server**.  
  
3.  Dans la boîte de dialogue **Se connecter au serveur** , entrez le **Nom du serveur** de l'instance de serveur à laquelle vous souhaitez vous connecter, entrez vos informations d'identification, puis cliquez sur **Connecter**.  
  
4.  Dans l'**Explorateur d'objets SQL Server**, développez le nœud **Bases de données** sous votre instance de serveur. Vous verrez toutes les bases de données résidant dans cette instance ajoutées sous ce nœud **Bases de données** .  
  
5.  Développez le nœud **AdventureWorks** (ou le nœud d'une autre base de données). Notez que toutes les entités de base de données sont organisées dans une hiérarchie similaire à celle de SQL Server Management Studio.  
  
