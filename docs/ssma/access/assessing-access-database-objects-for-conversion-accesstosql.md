---
description: Évaluation des objets de base de données Access pour la conversion (AccessToSQL)
title: Évaluation des objets de base de données Access pour la conversion (AccessToSQL) | Microsoft Docs
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
helpviewer_keywords:
- assessing SQL
- assessing syntax
- assessment reports
- creating assessment reports
- estimating migration effort
- reports
- SQL, assessing
- syntax, assessing
ms.assetid: 8b9e23d6-da62-437a-8c05-8ad2628b9441
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: 07adac4e991afd70b70a5f087a881f1933baaae1
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100076420"
---
# <a name="assessing-access-database-objects-for-conversion-accesstosql"></a>Évaluation des objets de base de données Access pour la conversion (AccessToSQL)
Avant de charger des objets et de migrer des données vers [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou SQL Azure, vous devez déterminer la quantité de migration réussie et la durée de la conversion. SSMA peut créer un rapport d’évaluation qui indique le pourcentage d’objets qui ont été convertis en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] SQL Azure syntaxe et les estimations de temps pour effectuer la migration. SSMA vous permet également de consulter les problèmes spécifiques qui ont provoqué des échecs de conversion.  
  
## <a name="creating-assessment-reports"></a>Création de rapports d’évaluation  
Lorsqu’il crée un rapport d’évaluation, SSMA convertit les objets de base de données Access sélectionnés en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] SQL Azure syntaxe, puis affiche les résultats.  
  
**Pour créer un rapport d’évaluation**  
  
1.  Dans l’Explorateur de métadonnées Access, sélectionnez la ou les bases de données que vous souhaitez évaluer.  
  
2.  Pour omettre des objets spécifiques, désactivez les cases à cocher en regard des objets que vous ne souhaitez pas évaluer.  
  
3.  Cliquez avec le bouton droit sur **bases de données**, puis sélectionnez **créer un rapport**.  
  
    Vous pouvez également analyser des objets individuels en cliquant avec le bouton droit sur un objet, puis en sélectionnant **créer un rapport**.  
  
    SSMA affiche la progression dans la barre d’État en bas de la fenêtre. Si le volet de sortie est visible, vous verrez également des messages dans le volet de sortie.  
  
Une fois l’évaluation terminée, la [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] fenêtre Assistant Migration pour Access : rapport d’évaluation s’affiche.  
  
## <a name="using-assessment-reports"></a>Utilisation des rapports d’évaluation  
La fenêtre rapport d’évaluation contient trois volets : un explorateur, un volet d’informations et un volet de message.  
  
-   Le volet Explorateur vous permet de parcourir les objets qui ont été évalués. Vous pouvez cliquer sur des éléments dans ce volet pour accéder à des tables, des index et des clés individuels.  
  
-   Le volet d’informations affiche les statistiques de conversion pour l’objet sélectionné.  
  
-   Le volet message affiche les erreurs, les avertissements et les messages d’information relatifs à la conversion, ainsi que les délais d’exécution des étapes de migration et de correction d’erreurs individuelles.  
  
Vous devez corriger les erreurs avant d’exécuter à nouveau le rapport d’évaluation ou de convertir les schémas. Pour trouver des erreurs, cliquez sur le bouton **Erreurs** dans le volet messages, puis développez chaque erreur pour afficher la liste des objets où l’erreur s’est produite. Si vous cliquez sur un objet dans le volet messages, toutes les erreurs et tous les avertissements de cet objet s’affichent dans le volet d’informations.  
  
## <a name="next-step"></a>étape suivante  
[Convertir des objets de base de données Access](converting-access-database-objects-accesstosql.md)  
  
## <a name="see-also"></a>Voir aussi  
[Migration de bases de données Access vers SQL Server](migrating-access-databases-to-sql-server-azure-sql-db-accesstosql.md)  
  
