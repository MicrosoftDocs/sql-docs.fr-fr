---
title: Fenêtre Threads
description: En savoir plus sur la fenêtre Threads, qui affiche des informations sur le thread Moteur de base de données en cours de débogage. Les informations s’affichent uniquement en mode débogage.
titleSuffix: T-SQL debugger
ms.prod: sql
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- Threads Window [Transact-SQL]
ms.assetid: e153f619-0049-4162-9076-c24a454f3278
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 12/04/2019
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 1f4d8d5f4c490fbecf8a602142ced27098cf5224
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100340986"
---
# <a name="transact-sql-debugger---threads-window"></a>Débogueur Transact-SQL - Fenêtre Threads

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

La fenêtre **Threads** affiche des informations sur le thread du [!INCLUDE[ssDE](../../includes/ssde-md.md)] utilisé par la session de l’éditeur de requête du [!INCLUDE[ssDE](../../includes/ssde-md.md)] en cours de débogage. Vous devez être en mode débogage pour afficher les informations sur le thread.  

[!INCLUDE[ssms-old-versions](../../includes/ssms-old-versions.md)]

## <a name="task-list"></a>Liste des tâches

**Pour accéder à la fenêtre Threads**
  
-   Dans le menu **Déboguer** , cliquez sur **Fenêtres**, puis sur **Threads**.  
  
## <a name="columns"></a>Colonnes  
 **Identifiant**  
 Numéro d'identification unique que le débogueur [!INCLUDE[tsql](../../includes/tsql-md.md)] attribue au thread. Vous pouvez obtenir des d'informations supplémentaires sur le thread en sélectionnant une ligne dans la vue de gestion dynamique sys.dm_os_threads.  
  
 Si vous n’êtes pas en mode Regroupement léger, sélectionnez la ligne dont la valeur dans os_thread_id correspond à la valeur dans la colonne **ID** . Si vous êtes en mode Regroupement léger, sélectionnez la ligne dont la valeur dans fiber_context_address correspond à la valeur dans la colonne **ID** .  
  
 **Nom**  
 Affiche des informations sur la session du [!INCLUDE[ssDE](../../includes/ssde-md.md)] au format **nom_ordinateur/nom_instance [SPID]** .  
  
 **ComputerName**  
 Nom de l'ordinateur qui exécute l'instance du [!INCLUDE[ssDE](../../includes/ssde-md.md)] à laquelle la session de l'éditeur de requête est connectée.  
  
 **InstanceName**  
 Nom de l'instance du [!INCLUDE[ssDE](../../includes/ssde-md.md)] à laquelle la session de l'éditeur de requête est connectée.  
  
 **[SPID]**  
 ID de processus de session [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] qui identifie de façon unique cette session. Vous pouvez obtenir des informations supplémentaires sur la session en sélectionnant la ligne dans la vue sys.sysprocesses dont la valeur est la même dans la colonne spid.  
  
 **Lieu**  
 Affiche le nom du fichier de script utilisé dans la session de l'éditeur de requête en cours de débogage.  
  
 **Priorité**  
 Le débogueur [!INCLUDE[tsql](../../includes/tsql-md.md)] ne prend pas en charge cette fonctionnalité.  
  
 **Suspendre**  
 Le débogueur [!INCLUDE[tsql](../../includes/tsql-md.md)] ne prend pas en charge cette fonctionnalité.  
  
## <a name="see-also"></a>Voir aussi  
 [Débogueur Transact-SQL](./transact-sql-debugger.md)   
 [Informations du débogueur Transact-SQL](./transact-sql-debugger-information.md)   
 [sys.dm_os_threads &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-os-threads-transact-sql.md)   
 [sys.sysprocesses &#40;Transact-SQL&#41;](../../relational-databases/system-compatibility-views/sys-sysprocesses-transact-sql.md)