---
description: Rapprocher un diagramme de base de données et une base de données modifiée (Visual Database Tools)
title: Rapprocher un diagramme de base de données et une base de données modifiée
ms.custom: seo-lt-2019
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- updating diagram to match database
- reconciling database diagrams
- diagrams [SQL Server], reconciling changes
- updating database to match diagram
- database diagrams [SQL Server], reconciling changes
ms.assetid: eda8dea2-eedd-43a7-85aa-92bd97783b5f
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.openlocfilehash: b8ee9cf516085b10f221caf86e0850fdccaf8c85
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100352876"
---
# <a name="reconcile-a-database-diagram-with-a-modified-database-visual-database-tools"></a>Rapprocher un diagramme de base de données et une base de données modifiée (Visual Database Tools)
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
Vous enregistrez votre diagramme de base de données lorsque vous êtes prêt à mettre à jour la base de données par rapport à ce diagramme. Toutefois, si d'autres utilisateurs ont mis à jour la base de données depuis l'ouverture de votre schéma, leurs modifications risquent d'avoir une incidence sur le schéma et vice versa.  
  
L'enregistrement de votre schéma provoquera un rapprochement entre votre base de données et votre schéma en écrasant les autres modifications des utilisateurs, de telle sorte que votre base de données corresponde à votre schéma.  
  
### <a name="to-update-a-database-to-match-your-diagram"></a>Pour mettre à jour une base de données afin qu'elle corresponde à votre schéma  
  
1.  Enregistrez votre diagramme de base de données.  
  
    Si vous n’avez pas encore enregistré votre diagramme, tapez le nom que vous voulez lui attribuer dans la boîte de dialogue **Enregistrer un nouveau diagramme de base de données** et cliquez sur **OK**.  
  
2.  La boîte de dialogue **Enregistrer** affiche la liste des tables qui seront affectées quand vous enregistrerez votre schéma. Choisissez **Oui** pour continuer.  
  
3.  La boîte de dialogue **Modifications détectées dans la base de données** affiche la liste des objets qui ont changé et qui seront modifiés de façon à correspondre à votre schéma. Choisissez **Oui** pour enregistrer le schéma et accepter la liste des changements.  
  
    > [!NOTE]  
    > Si votre schéma contient des tables et des colonnes qui ont été supprimées de la base de données, seules leurs définitions sont recréées dans la base de données lorsque vous enregistrez votre schéma. Ce processus ne restaure pas les données qui existaient dans ces objets avant leur suppression.  
  
### <a name="to-update-your-diagram-to-match-a-modified-database"></a>Pour mettre à jour votre schéma afin qu'il corresponde à une base de données modifiée  
  
1.  Fermez votre schéma sans enregistrer les modifications.  
  
2.  Cliquez avec le bouton droit sur le schéma dans l'Explorateur d'objets.  
  
3.  Dans le menu contextuel, cliquez sur **Actualiser**.  
  
4.  Rouvrez le schéma.  
  
## <a name="see-also"></a>Voir aussi  
[Utiliser des diagrammes de base de données &#40;Visual Database Tools&#41;](../../ssms/visual-db-tools/work-with-database-diagrams-visual-database-tools.md)  
  
