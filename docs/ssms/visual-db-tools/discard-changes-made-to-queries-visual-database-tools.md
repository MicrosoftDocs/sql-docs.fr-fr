---
title: Ignorer les modifications apportées aux requêtes
ms.custom: seo-lt-2019
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- reverting queries
- queries [SQL Server], discarding changes
- discarding query changes
ms.assetid: 7bb17ece-1222-4622-b476-5789d7641c64
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.openlocfilehash: 895bc5a3f68d36b4948d5697a901e6b4711025ec
ms.sourcegitcommit: f3321ed29d6d8725ba6378d207277a57cb5fe8c2
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/06/2020
ms.locfileid: "85999699"
---
# <a name="discard-changes-made-to-queries-visual-database-tools"></a>Abandonner les modifications apportées aux requêtes (Visual Database Tools)
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
Vous pouvez abandonner les modifications apportées à une définition de requête avant de les enregistrer. Une fois enregistrées, leur état antérieur ne peut pas être rétabli.  
  
> [!NOTE]  
> Pour annuler une modification apportée à des valeurs dans le volet Résultats, appuyez sur la touche Échap avant de quitter l'enregistrement. Si, lorsque vous quittez un enregistrement, un message vous avertit que les modifications ne seront pas validées dans la base de données, vous pouvez également appuyer sur la touche Échap pour rétablir la valeur antérieure.  
  
### <a name="to-discard-changes-made-to-a-query-definition"></a>Pour ignorer les modifications apportées à une définition de requête  
  
1.  Quand la requête est ouverte dans le Concepteur de requêtes et de vues, cliquez sur **Fermer** dans le menu **Fichier**.  
  
2.  Dans la boîte de dialogue **Microsoft SQL Server Management Studio** , cliquez sur **Non**.  
  
    La définition de requête retrouve l'état qui était le sien lors du dernier enregistrement.  
  
## <a name="see-also"></a>Voir aussi  
[Enregistrer des requêtes](../../ssms/visual-db-tools/save-queries-visual-database-tools.md)  
[Rubriques de procédures relatives au Concepteur de vues et de requêtes](../../ssms/visual-db-tools/design-queries-and-views-how-to-topics-visual-database-tools.md)  
[Effectuer des opérations de base avec des requêtes](../../ssms/visual-db-tools/perform-basic-operations-with-queries-visual-database-tools.md)  
[Utiliser des données du volet de résultats](../../ssms/visual-db-tools/work-with-data-in-the-results-pane-visual-database-tools.md)  
  
