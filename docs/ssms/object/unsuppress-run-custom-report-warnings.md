---
description: Annuler la suppression des avertissements d'exécution de rapports personnalisés
title: Annuler la suppression des avertissements d'exécution de rapports personnalisés
ms.custom: seo-lt-2019
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- SQL Server Management Studio [SQL Server], custom reports
ms.assetid: 0deed900-c910-4d12-aac0-6ab9e39eb068
author: markingmyname
ms.author: maghan
ms.openlocfilehash: cf87ce7e04e50ea64a6f98db0b29dfc7e90f813d
ms.sourcegitcommit: 38e055eda82d293bf5fe9db14549666cf0d0f3c0
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/02/2021
ms.locfileid: "99250488"
---
# <a name="unsuppress-run-custom-report-warnings"></a>Annuler la suppression des avertissements d'exécution de rapports personnalisés
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
Il existe deux boîtes de dialogue d'avertissement pour les rapports personnalisés. Cette rubrique décrit comment annuler la suppression de l'affichage de ces zones dans [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] à l'aide de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)].  
  
Par défaut, la boîte de dialogue **Exécuter le rapport personnalisé** apparaît avant l’exécution d’un rapport personnalisé. Elle n’apparaît plus si vous cochez la case **Ne plus afficher ce message** . De même, toujours par défaut, la boîte de dialogue **Exécuter le rapport personnalisé** s’affiche quand vous ouvrez un rapport personnalisé, puis cliquez sur un lien pour en ouvrir un autre. Cette boîte de dialogue affiche le chemin du fichier de rapport d'extraction personnalisé. Elle n’apparaît plus si vous cochez la case **Ne plus afficher ce message** .  
  
## <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a>Utilisation de SQL Server Management Studio  
  
#### <a name="to-unsuppress-the-main-custom-report-warning-dialog-box"></a>Pour annuler la suppression de la boîte de dialogue d'avertissement principale des rapports personnalisés  
  
1.  Connectez-vous à \<*Server*>\\<*Partager*>|\<*Drive*>\Documents et paramètres\\<UserProfile>\Application Data\Microsoft\Microsoft SQL Server\130\Tools\Shell\reports.xml.  
  
2.  Cliquez avec le bouton droit sur le fichier **reports.xml**, puis sélectionnez **Modifier**.  
  
3.  Changez **<SuppressWarning>true\<\/SuppressWarning> par <SuppressWarning>false\<\/SuppressWarning>** .  
  
4.  Redémarrez [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)].  
  
#### <a name="to-unsuppress-the-drill-through-custom-report-warning-dialog-box"></a>Pour annuler la suppression de la boîte de dialogue d'avertissement principale des rapports d'extraction personnalisés  
  
1.  Connectez-vous à \<*Server*>\\<*Partager*>|\<*Drive*>\Documents et paramètres\\<UserProfile>\Application Data\Microsoft\Microsoft SQL Server\130\Tools\Shell\reports.xml.  
  
2.  Cliquez avec le bouton droit sur **reports.xml**, puis sélectionnez **Modifier**.  
  
3.  Changez **<SuppressDrillthroughWarning>true\<\/SuppressDrillthroughWarning>par <SuppressDrillthroughWarning>false\<\/SuppressDrillthroughWarning>** .  
  
4.  Redémarrez [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)].  
  
## <a name="see-also"></a>Voir aussi  
[Rapports personnalisés dans Management Studio](../../ssms/object/custom-reports-in-management-studio.md)  
[Ajouter un rapport personnalisé à Management Studio](../../ssms/object/add-a-custom-report-to-management-studio.md)  
[Utiliser des rapports personnalisés avec les propriétés de nœud de l’Explorateur d’objets](../../ssms/object/use-custom-reports-with-object-explorer-node-properties.md)  
  
