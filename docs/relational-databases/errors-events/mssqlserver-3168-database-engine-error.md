---
description: MSSQLSERVER_3168
title: MSSQLSERVER_3168 | Microsoft Docs
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 3168 (Database Engine error)
ms.assetid: 991111d9-1eb3-43e9-9333-a75a775c3200
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 9e57fa84cf49932c0a550290c11f267d6a055921
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99194207"
---
# <a name="mssqlserver_3168"></a>MSSQLSERVER_3168
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>Détails  
  
| Attribut | Valeur |  
| :-------- | :---- |  
|Nom du produit|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]|  
|ID de l’événement|3168|  
|Source de l’événement|MSSQLSERVER|  
|Composant|SQLEngine|  
|Nom symbolique|LDDB_SYSTEMWRONGVER|  
|Texte du message|Impossible de restaurer la sauvegarde de la base de données système de l'unité %ls car elle a été créée par une autre version de serveur (%ls) que celle-ci (%ls).|  
  
## <a name="explanation"></a>Explication  
Vous ne pouvez pas restaurer la sauvegarde d’une base de données système (**MASTER**, **model** ou **msdb**) sur une version de serveur différente de la version sur laquelle la sauvegarde a été effectuée à l’origine.  
  
> [!NOTE]  
> L'installation d'un Service Pack ou d'un correctif modifie le numéro de build du serveur et les versions des serveurs sont toujours incrémentielles.  
  
### <a name="possible-causes"></a>Causes possibles  
Le schéma de la base de données des bases de données système a peut-être été changé sur les versions de serveur. Pour vous assurer qu'une modification de schéma ne produit pas des incohérences, l'instruction RESTORE compare le numéro de build du serveur dans le fichier de sauvegarde avec le numéro de build du serveur sur lequel vous tentez de restaurer la sauvegarde. Si les versions diffèrent, l'instruction émet le message d'erreur 3168 et l'opération de restauration se termine anormalement.  
  
Certains scénarios dans lesquels ce problème est susceptible de se poser sont les suivants :  
  
-   Un utilisateur tente de restaurer une base de données système sur le serveur A à partir d'une sauvegarde réalisée sur le serveur B. Les serveurs A et B se trouvent sur des serveurs de version différente. Par exemple, le serveur A peut se situer sur la version d'origine et le serveur B sur une version de Service Pack 1 (SP1).  
  
-   Un utilisateur tente de restaurer une base de données système à partir d'une sauvegarde réalisée sur le même serveur. Cependant, le serveur exécutait une version différente au moment de la sauvegarde. Cela signifie que le serveur a été mis à niveau depuis la réalisation de la sauvegarde.  
  
## <a name="user-action"></a>Action de l'utilisateur  
Le processus de restauration intervient en bonne partie dans cette situation et n'est utilisé qu'en dernier recours. Pour plus d’informations, consultez «[You cannot restore system database backups to a different build of SQL Server](https://support.microsoft.com/kb/264474)».  
  
## <a name="see-also"></a>Voir aussi  
[Limitations sur la restauration des bases de données système &#40;SQL Server&#41;](~/relational-databases/backup-restore/back-up-and-restore-of-system-databases-sql-server.md#limitations-on-restoring-system-databases)  
  
