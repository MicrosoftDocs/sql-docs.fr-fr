---
description: MSSQLSERVER_3313
title: MSSQLSERVER_3313 | Microsoft Docs
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: language-reference
helpviewer_keywords:
- 3313 (Database Engine error)
ms.assetid: a244227b-8553-42df-9435-034f906c4c74
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: d176bcd729ca1d72b8f1e6b3d3951cb94ca000e9
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/17/2020
ms.locfileid: "88456244"
---
# <a name="mssqlserver_3313"></a>MSSQLSERVER_3313
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>Détails  
  
| Attribut | Valeur |  
| :-------- | :---- |  
|Nom du produit|SQL Server|  
|ID de l’événement|3313|  
|Source de l’événement|MSSQLSERVER|  
|Composant|SQLEngine|  
|Nom symbolique|ERR_LOG_RID1|  
|Texte du message|Une erreur s'est produite sur l'enregistrement de journal ID %S_LSN en refaisant une opération journalisée dans la base de données '%.*ls'. En général, l’erreur spécifique est déjà consignée dans le service de journal d’événements Windows. Restaurez la base de données à partir d'une sauvegarde complète ou réparez-la.|  
  
## <a name="explanation"></a>Explication  
Il s'agit d'une erreur de cumul pour la récupération de type REDO. Cette erreur a placé la base de données dans l'état SUSPECT. Le groupe de fichiers primaire et éventuellement d'autres groupes de fichiers sont suspects et peut-être endommagés. La base de données ne peut pas être récupérée au démarrage de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et n'est par conséquent pas disponible. Une action est requise de la part de l'utilisateur pour résoudre le problème.  
  
Notez que si cette erreur se produit pour **tempdb**, l'instance [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] s'arrête.  
  
## <a name="user-action"></a>Action de l'utilisateur  
Cette erreur peut être provoquée par une situation temporaire qui existait sur le système lors d'une tentative donnée de démarrer l'instance du serveur ou de récupérer une base de données. Cette erreur peut également être provoquée par un échec permanent qui se produit à chaque tentative de démarrage de la base de données. Pour plus d'informations sur la cause, examinez le journal des événements Windows pour rechercher une erreur précédente qui indique l'échec spécifique.  
  
Notez que lorsque cette condition d’erreur est rencontrée, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] génère généralement trois fichiers dans le dossier **LOG** de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Le fichier SQLDump*nnnn*.txt contient des informations de diagnostic avancées concernant les échecs, notamment des détails relatifs à la transaction et la page qui a rencontré le problème. Ces informations sont généralement utilisées par l'équipe du Support technique pour analyser la raison de l'échec.  
  
Pour plus d’informations sur la cause de l’occurrence de l’erreur 3313, examinez le journal des événements Windows pour rechercher une erreur précédente qui indique l’échec spécifique. L'action utilisateur appropriée varie selon que les informations dans le Journal des événements Windows indiquent que l'erreur [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] a été provoquée par une condition transitoire ou un échec permanent. Pour plus d’informations sur les actions utilisateur requises pour le dépannage de l’erreur 3313, consultez la documentation en ligne de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
## <a name="see-also"></a>Voir aussi  
[ALTER DATABASE &#40;Transact-SQL&#41;](~/t-sql/statements/alter-database-transact-sql-set-options.md)  
[DBCC CHECKDB &#40;Transact-SQL&#41;](~/t-sql/database-console-commands/dbcc-checkdb-transact-sql.md)  
[Restaurations complètes de bases de données &#40;mode de récupération simple&#41;](~/relational-databases/backup-restore/complete-database-restores-simple-recovery-model.md)  
[MSSQLSERVER_824](~/relational-databases/errors-events/mssqlserver-824-database-engine-error.md)  
[sys.databases &#40;Transact-SQL&#41;](~/relational-databases/system-catalog-views/sys-databases-transact-sql.md)  
  
