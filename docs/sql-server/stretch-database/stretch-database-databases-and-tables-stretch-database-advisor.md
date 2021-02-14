---
description: Identifier des bases de données et des tables pour Stretch Database avec Data Migration Assistant
title: Identifier les bases de données et les tables
ms.date: 10/30/2017
ms.service: sql-server-stretch-database
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- Stretch Database, identifying databases
- Stretch Database, identifying tables
- identifying databases for Stretch Database
- identifying tables for Stretch Database
ms.assetid: 81bd93d8-eef8-4572-88d7-5c37ab5ac2bf
author: rothja
ms.author: jroth
ms.custom: seo-dt-2019
ms.openlocfilehash: 0535dd0dbc410444f0dd9423da6e1bf40a4026bd
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100079870"
---
# <a name="identify-databases-and-tables-for-stretch-database-with-data-migration-assistant"></a>Identifier des bases de données et des tables pour Stretch Database avec Data Migration Assistant
[!INCLUDE [sqlserver2016-windows-only](../../includes/applies-to-version/sqlserver2016-windows-only.md)]


  Pour identifier des bases de données et des tables candidates pour Stretch Database, ainsi que les problèmes de blocage éventuels, téléchargez et exécutez Microsoft Data Migration Assistant.
  
## <a name="get-data-migration-assistant"></a>Obtenir Data Migration Assistant
 Téléchargez et installez Data Migration Assistant à partir [d’ici](https://www.microsoft.com/download/details.aspx?id=53595). Cet outil n’est pas fourni sur le support d’installation de SQL Server.  
  
## <a name="run-data-migration-assistant"></a>Exécuter Data Migration Assistant  
  
1.  Exécutez Microsoft Data Migration Assistant.  

2.  Créez un projet de type **Assessment** (Évaluation) et donnez-lui un nom.

3.  Sélectionnez **SQL Server** à la fois comme **Source server type** (Type de serveur source) et **Target server type** (Type de serveur cible).

4.  Sélectionnez **Créer**. 

5. Dans la page **Options** (étape 1), sélectionnez **New features recommendation** (Nouvelle recommandation de fonctionnalités). Vous pouvez éventuellement effacer la sélection pour **Compatibility issues** (Problèmes de compatibilité).

6.  Dans la page **Select sources** (Sélectionner des sources) (étape 2), connectez-vous à un serveur, sélectionnez une base de données, puis sélectionnez **Add** (Ajouter).

7.  Sélectionnez **Start Assessment** (Démarrer l’évaluation).

## <a name="review-the-results"></a>Passer en revue les résultats.  
  
1.  Quand l’analyse est terminée, dans la page **Review results** (Passer en revue les résultats) (étape 3), sélectionnez l’option **Feature recommendations** (Recommandations de fonctionnalités), puis sélectionnez l’onglet **Storage** (Stockage).

2.  Passez en revue les recommandations relatives à la Stretch Database. Chaque recommandation liste les tables pour lesquelles Stretch Database peut s’avérer approprié, ainsi que les problèmes de blocage éventuels.

## <a name="historical-note"></a>Remarque sur l’historique
Avant, Stretch Database Advisor était un composant de SQL Server 2016 Upgrade Advisor. Vous deviez alors sélectionner Stretch Database Advisor et l’exécuter à part.

Avec la publication de Data Migration Assistant, qui remplace et étend le Conseiller de mise à niveau, les fonctionnalités de Stretch Database Advisor sont intégrées à ce nouvel outil. Vous n’êtes pas obligé de sélectionner des options pour obtenir des recommandations relatives à Stretch Database. Quand vous exécutez une évaluation dans Data Migration Assistant, les résultats liés à Stretch Database s’affichent sous l’onglet **Storage** (Stockage) des **Feature recommendations** (Recommandations de fonctionnalité).
  
## <a name="next-step"></a>Étape suivante  
 Activez Stretch Database.  
  
-   Pour activer Stretch Database sur une **base de données**, consultez [Enable Stretch Database for a database](../../sql-server/stretch-database/enable-stretch-database-for-a-database.md).  
  
-   Pour activer Stretch Database sur une autre **table**, alors que Stretch est déjà activé sur la base de données, consultez [Enable Stretch Database for a table](../../sql-server/stretch-database/enable-stretch-database-for-a-table.md). 
  
## <a name="see-also"></a>Voir aussi  
 [Limitations concernant Stretch Database](../../sql-server/stretch-database/limitations-for-stretch-database.md)   
 [Activer Stretch Database pour une base de données](../../sql-server/stretch-database/enable-stretch-database-for-a-database.md)   
 [Activer Stretch Database pour une table](../../sql-server/stretch-database/enable-stretch-database-for-a-table.md)  
  
  
