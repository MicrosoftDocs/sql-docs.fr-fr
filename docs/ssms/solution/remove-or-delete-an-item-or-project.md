---
description: Enlever ou supprimer un élément ou un projet
title: Enlever ou supprimer un élément ou un projet
ms.custom: seo-lt-2019
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- deleting project items
- projects [SQL Server Management Studio], item removal
- removing project items
ms.assetid: 3fd92434-70f5-466e-bef0-7e0fd73ddb1c
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 5e8389848928507d29be094faaf626f99ba9c8ef
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/17/2020
ms.locfileid: "88491819"
---
# <a name="remove-or-delete-an-item-or-project"></a>Enlever ou supprimer un élément ou un projet
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
Les éléments de projet des projets [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] sont les requêtes, les connexions et les fichiers divers. Vous pouvez enlever des requêtes et des fichiers divers de projet d'une solution sans effacer du support de stockage les fichiers. Supprimez un projet ou un élément lorsqu'il n'est plus utile dans la solution en cours mais que vous souhaitez l'insérer dans une autre solution.  
  
### <a name="to-remove-a-project-item"></a>Pour supprimer un élément de projet  
  
1.  Dans l'Explorateur de solutions, sélectionnez l'élément de projet à enlever.  
  
2.  Dans le menu **Edition** , cliquez sur **Enlever**.  
  
3.  Dans la boîte de dialogue de confirmation, cliquez sur **Enlever** pour enlever l’élément du projet.  
  
Un élément enlevé existe toujours dans le système de fichiers. Par conséquent, après avoir enlevé un élément, vous pouvez toujours l'ajouter à sa solution initiale ou à une autre solution.  
  
#### <a name="to-remove-a-project"></a>Pour enlever un projet  
  
1.  Dans l'Explorateur de solutions, sélectionnez le projet à enlever.  
  
2.  Dans le menu **Edition** , cliquez sur **Enlever**.  
  
3.  Dans la boîte de dialogue de confirmation, cliquez sur **OK**pour enlever le projet de la solution.  
  
Vous pouvez supprimer de façon permanente un projet, mais vous devez d'abord enlever les références au projet des solutions [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] , puis utiliser l'Explorateur [!INCLUDE[msCoName](../../includes/msconame_md.md)] Windows pour supprimer définitivement du système de stockage les fichiers associés.  
  
#### <a name="to-delete-a-project"></a>Pour supprimer un projet  
  
1.  Dans l'Explorateur de solutions, enlevez le projet que vous souhaitez supprimer de la solution.  
  
2.  Dans l'Explorateur Windows, recherchez et sélectionnez les fichiers associés au projet ou à l'élément que vous souhaitez supprimer.  
  
3.  Dans le menu **Fichier** , cliquez sur **Supprimer**.  
  
## <a name="see-also"></a>Voir aussi  
[Explorateur de solutions](../../ssms/solution/solution-explorer.md)  
[Ajouter de nouveaux éléments à un projet](../../ssms/solution/add-new-items-to-a-project.md)  
[Ajouter des éléments existants à un projet](../../ssms/solution/add-existing-items-to-a-project.md)  
  
