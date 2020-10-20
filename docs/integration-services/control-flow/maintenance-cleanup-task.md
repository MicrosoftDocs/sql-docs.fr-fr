---
description: Tâche de nettoyage de maintenance
title: Nettoyage de maintenance, tâche | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- sql13.dts.designer.maintenancecleanuptask.f1
helpviewer_keywords:
- deleting files
- removing files
- Maintenance Cleanup task
ms.assetid: 73ad3cd6-9a6d-44cf-905f-c56aa658bf42
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 0994268d1e6ba95e7f4f3bc1bbe3875ec4f57358
ms.sourcegitcommit: cfa04a73b26312bf18d8f6296891679166e2754d
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/19/2020
ms.locfileid: "92197183"
---
# <a name="maintenance-cleanup-task"></a>Tâche de nettoyage de maintenance

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  La tâche de nettoyage de maintenance supprime les fichiers associés aux plans de maintenance, notamment des fichiers de sauvegarde de base de données et des rapports créés par les plans de maintenance. Pour plus d’informations, consultez [Plans de maintenance](../../relational-databases/maintenance-plans/maintenance-plans.md) et [Sauvegarde et restauration des bases de données SQL Server](../../relational-databases/backup-restore/back-up-and-restore-of-sql-server-databases.md).  
  
 En utilisant la tâche de nettoyage de maintenance, un package peut supprimer les fichiers de sauvegarde ou les rapports de plan de maintenance sur le serveur spécifié. La tâche de nettoyage de maintenance inclut une option permettant de supprimer un fichier spécifique ou un groupe de fichiers dans un dossier. Vous pouvez facultativement spécifier l'extension des fichiers à supprimer.  
  
 Lorsque vous configurez la tâche de nettoyage de maintenance pour supprimer des fichiers de sauvegarde, l'extension de nom de fichier par défaut est BAK. Pour les fichiers de rapport, l'extension de nom de fichier par défaut est TXT. Vous pouvez mettre à jour les extensions en fonction de vos besoins à condition qu'elles ne comportent pas plus de 256 caractères.  
  
 Il convient généralement de supprimer les anciens fichiers devenus inutiles, et la tâche de nettoyage de maintenance peut être configurée pour supprimer les fichiers ayant atteint un âge spécifié. Par exemple, la tâche peut être configurée pour supprimer les fichiers datant de plus de quatre semaines. Vous pouvez spécifier l'âge des fichiers à supprimer en indiquant des jours, des semaines, des mois ou des années. Si vous ne spécifiez pas l'âge minimal des fichiers à supprimer, tous les fichiers du type spécifié sont supprimés.  
  
 Contrairement aux versions antérieures de la tâche de nettoyage de maintenance, la version [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ne supprime pas automatiquement les fichiers dans les sous-répertoires du répertoire spécifié. Cette contrainte réduit la surface d'exposition d'une attaque pouvant exploiter la fonctionnalité de la tâche de nettoyage de maintenance pour supprimer des fichiers par malveillance. Pour supprimer les sous-dossiers de premier niveau, vous devez choisir de le faire de manière explicite en activant l’option **Inclure les sous-dossiers de premier niveau** dans la boîte de dialogue **Tâche de nettoyage de maintenance** .  
  
## <a name="configuration-of-the-maintenance-cleanup-task"></a>Configuration de la tâche de nettoyage de maintenance  
 Vous pouvez définir les propriétés par le biais du concepteur [!INCLUDE[ssIS](../../includes/ssis-md.md)] . Cette tâche se trouve dans la section **Tâches du plan de maintenance** de la **boîte à outils** du concepteur [!INCLUDE[ssIS](../../includes/ssis-md.md)] .  
  
 Pour plus d'informations sur les propriétés définissables dans le concepteur [!INCLUDE[ssIS](../../includes/ssis-md.md)] , cliquez sur la rubrique suivante :  
  
-   [Tâche de nettoyage de maintenance &#40;Plan de maintenance&#41;](../../relational-databases/maintenance-plans/maintenance-cleanup-task-maintenance-plan.md)  
  
## <a name="related-tasks"></a>Tâches associées  
 Pour plus d’informations sur la définition de ces propriétés dans le concepteur [!INCLUDE[ssIS](../../includes/ssis-md.md)] , consultez [Définir les propriétés d’une tâche ou d’un conteneur](./add-or-delete-a-task-or-a-container-in-a-control-flow.md).  
  
## <a name="see-also"></a>Voir aussi  
 [Tâches Integration Services](../../integration-services/control-flow/integration-services-tasks.md)   
 [Flux de contrôle](../../integration-services/control-flow/control-flow.md)  
  
