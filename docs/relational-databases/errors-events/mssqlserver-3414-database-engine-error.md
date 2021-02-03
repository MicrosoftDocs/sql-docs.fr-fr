---
description: MSSQLSERVER_3414
title: MSSQLSERVER_3414 | Microsoft Docs
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 3414 (Database Engine error)
ms.assetid: f25852f9-b91c-4356-b817-78bec9ec8db4
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: fe68125b8b7b5d98e707a9e38d956d1d114db74e
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99195168"
---
# <a name="mssqlserver_3414"></a>MSSQLSERVER_3414
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>Détails  
  
| Attribut | Valeur |  
| :-------- | :---- |  
|Nom du produit|SQL Server|  
|ID de l’événement|3414|  
|Source de l’événement|MSSQLSERVER|  
|Composant|SQLEngine|  
|Nom symbolique|REC_GIVEUP|  
|Texte du message|Une erreur s'est produite pendant la récupération, empêchant la base de données '%.*ls' (ID %d) de redémarrer. Diagnostiquez les erreurs de récupération et corrigez-les, ou procédez à une restauration à partir d'une sauvegarde connue. Si les erreurs persistent, contactez le Support technique.|  
  
## <a name="explanation"></a>Explication  
La base de données spécifiée a été récupérée mais n'a pas pu démarrer, car des erreurs se sont produites au cours de la récupération. Cette erreur a mis la base de données à l’état SUSPECT. Le groupe de fichiers primaire et éventuellement d'autres groupes de fichiers sont suspects et peut-être endommagés. La base de données ne peut pas être récupérée au démarrage de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et n'est par conséquent pas disponible. Une action est requise de la part de l'utilisateur pour résoudre le problème. L’état SUSPECT sera indiqué dans SQL Server Management Studio (en regard de l’icône de base de données) et dans la colonne sys.databases.state_desc. Toute tentative d’utilisation d’une base de données dans cet état entraînera l’erreur suivante :

```
Msg 926, Level 14, State 1, Line 1 
Database 'mydb' cannot be opened. It has been marked SUSPECT by recovery. See the SQL Server errorlog for more information
```
  
Notez que lorsque cette erreur se produit dans **tempdb**, l’instance [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] s’arrête.  

## <a name="cause"></a>Cause
Cette erreur peut être provoquée par une situation temporaire qui existait sur le système lors d'une tentative donnée de démarrer l'instance du serveur ou de récupérer une base de données. Cette erreur peut également être provoquée par un échec permanent qui se produit à chaque tentative de démarrage de la base de données. La cause de l’échec de la récupération se trouve généralement dans une ou plusieurs erreurs qui précèdent l’erreur 3414 dans le journal ERRORLOG ou le journal des événements. L’erreur précédente dans le fichier journal contient la même valeur spid<n>. Par exemple, l’échec de récupération suivant est dû à une erreur de somme de contrôle pendant une tentative de lecture d’un bloc de journal. Remarquez que *spid15s* est présent dans toutes les lignes :

```
2020-03-31 17:33:13.00 spid15s     Error: 824, Severity: 24, State: 4.  
2020-03-31 17:33:13.00 spid15s     SQL Server detected a logical consistency-based I/O error: (bad checksum). It occurred during a read of page (0:-1) in database ID 13 at offset 0x0000000000b800 in file 'C:\Program Files\Microsoft SQL Server\MSSQL10.SQL2008\MSSQL\DATA\mydb_log.LDF'.  Additional messages in the SQL Server error log or system event log may provide more detail. This is a severe error condition that threatens database integrity and must be corrected immediately. Complete a full database consistency check (DBCC CHECKDB). This error can be caused by many factors; for more information, see SQL Server Books Online.   
2020-03-31 17:33:13.16 spid15s     Error: 3414, Severity: 21, State: 1.  
2020-03-31 17:33:13.16 spid15s     An error occurred during recovery, preventing the database 'mydb' (database ID 13) from restarting. Diagnose the recovery errors and fix them, or restore from a known good backup. If errors are not corrected or expected, contact Technical Support
```


Les erreurs susceptibles de provoquer l’échec de la récupération de la base de données sont diverses et variées. Bien qu’il soit indispensable d’évaluer chaque erreur au cas par cas, la résolution d’un échec de récupération de base de données est généralement identique à ce qui est décrit dans la section Action requise ci-dessous.

## <a name="user-action"></a>Action requise  
 
Pour plus d'informations sur la cause de l’occurrence de l’erreur 3414, examinez le journal des événements Windows ou le journal ERRORLOG pour rechercher une erreur précédente qui indique l’échec en question. L'action utilisateur appropriée varie selon que les informations dans le Journal des événements Windows indiquent que l'erreur [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] a été provoquée par une condition transitoire ou un échec permanent. Le message d’erreur invite à diagnostiquer les erreurs de récupération et de les corriger, ou de procéder à une restauration à partir d’une sauvegarde fiable connue. Par conséquent, vous pouvez essayer de corriger l’erreur que vous rencontrez pour permettre à la récupération d’aboutir (consultez [Erreurs corrigibles et transactions différées](#correctable-errors-and-deferred-transactions)).

Si les erreurs ne peuvent pas être corrigées, l’option à privilégier pour résoudre ce problème est la restauration à partir d’une sauvegarde fiable. Cependant, si vous ne pouvez pas récupérer à partir d’une sauvegarde, deux autres options s’offrent à vous, sans garantie de récupération complète des données : recourir à la réparation d’urgence avec [DBCC CHECKDB](../../t-sql/database-console-commands/dbcc-checkdb-transact-sql.md) ou essayer de copier le plus de données possible dans une autre base de données. 

 1. Restaurer à partir de la dernière sauvegarde de base de données fiable connue.
 1. Utilisez la méthode de réparation d’urgence fournie par [DBCC CHECKDB](../../t-sql/database-console-commands/dbcc-checkdb-transact-sql.md).
 1. Essayez de copier le plus de données possible dans une autre base de données.

La première méthode de restauration d’une sauvegarde de base de données fiable est le meilleur choix pour ramener une base de données à un état cohérent connu.  

Le deuxième meilleur choix, si aucune sauvegarde n’est disponible, est de mettre la base de données en ligne et de la rendre accessible. Cependant, vous devez savoir que la cohérence transactionnelle ne peut pas être garantie puisque la récupération a échoué. Il n’existe aucun moyen d’identifier les transactions qui auraient dû être restaurées ou restaurées par progression, mais qui n’y ont pas été autorisées en raison de l’échec de la récupération. Les étapes de la réparation d’urgence sont décrites dans la section intitulée [Résolution des erreurs de base de données en mode urgence](../../t-sql/database-console-commands/dbcc-checkdb-transact-sql.md#resolving-errors-in-database-emergency-mode) dans la documentation DBCC CHECKDB. 

Si la réparation d’urgence ne fonctionne pas et que vous voulez essayer de sauver quelques données dans une autre base de données, le moyen d’accéder à la base de données consiste à définir la base de données en mode urgence via la commande ALTER DATABASE <dbname> SET EMERGENCY. Vous pouvez ensuite essayer de copier les données des tables.

### <a name="correctable-errors-and-deferred-transactions"></a>Erreurs corrigibles et transactions différées
Toutes les erreurs rencontrées pendant la récupération de la base de données ne débouchent pas sur un échec de la récupération et sur une base de données suspecte :

Les erreurs qui surviennent à la première ouverture des fichiers de base de données et/ou des fichiers journaux des transactions se produisent avant la récupération. Les erreurs [17204](mssqlserver-17204-database-engine-error.md) et [17207](mssqlserver-17207-database-engine-error.md) en sont des exemples. Une fois ces erreurs corrigées, la récupération peut être autorisée à se poursuivre (mais sans garantie qu’elle aboutisse si d’autres erreurs de récupération se produisent). Certaines erreurs comme 17204 et 17207 n’aboutissent pas à une base de données SUSPECT. En fait, quand ces problèmes se produisent, l’état de la base de données est RECOVERY_PENDING. 

[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] permet à la récupération d’arriver à son terme même quand une erreur se produit au niveau de la page et préserve toujours la cohérence transactionnelle. Ce processus a réduit le nombre de scénarios aboutissant à une base de données SUSPECT. Ce concept est connu sous le nom de [transactions différées](../backup-restore/deferred-transactions-sql-server.md).

Si l’erreur rencontrée pendant la récupération indique un problème au niveau d’une page de base de données, par exemple sous la forme d’une erreur de somme de contrôle ou d’un message 824, la récupération peut être autorisée à se terminer avec des erreurs en attente. Dans le cas où une transaction n’est pas validée, une erreur sur une page peut aboutir à une [transaction différée](../backup-restore/deferred-transactions-sql-server.md) autorisant la récupération à se terminer.  

Les entrées ERRORLOG suivantes montrent un exemple de message de l’erreur [824](mssqlserver-824-database-engine-error.md) rencontrée pendant la récupération, mais la récupération a été autorisée à se terminer avec une transaction différée. Notez l’absence de l’erreur 3414 dans ce cas et le message indiquant que la récupération a abouti pour la base de données :

```2010-03-31 19:17:18.45 spid7s      Error: 824, Severity: 24, State: 2.   
2010-03-31 19:17:18.45 spid7s      SQL Server detected a logical consistency-based I/O error: incorrect checksum (expected: 0xb2c87a0a; actual: 0xb6c0a5e2). It occurred during a read of page (1:153) in database ID 13 at offset 0x00000000132000 in file 'C:\Program Files\Microsoft SQL Server\MSSQL10.SQL2008\MSSQL\DATA\mydb.mdf'.  Additional messages in the SQL Server error log or system event log may provide more detail. This is a severe error condition that threatens database integrity and must be corrected immediately. Complete a full database consistency check (DBCC CHECKDB). This error can be caused by many factors; for more information, see SQL Server Books Online.   
2010-03-31 19:17:18.45 spid7s      Error: 3314, Severity: 21, State: 1.   
2010-03-31 19:17:18.45 spid7s      During undoing of a logged operation in database 'mydb', an error occurred at log record ID (25:100:19). Typically, the specific failure is logged previously as an error in the Windows Event Log service. Restore the database or file from a backup, or repair the database.
2010-03-31 19:17:18.45 spid7s      Errors occurred during recovery while rolling back a transaction. The transaction was deferred. Restore the bad page or file, and re-run recovery.   
2010-03-31 19:17:18.45 spid7s      Recovery completed for database mydb (database ID 13) in 2 second(s) (analysis 204 ms, redo 25 ms, undo 1832 ms.) This is an informational message only. No user action is required.   
```

Si une transaction validée doit être restaurée par progression, la page peut être marquée comme étant inaccessible (toutes les futures tentatives d’accès à la page feront apparaître le message 829) et la récupération peut aboutir. Dans ce cas, l’erreur doit être corrigée en restaurant la page à partir d’une sauvegarde ou en désallouant la page en utilisant DBCC CHECKDB avec la réparation.


  
## <a name="see-also"></a>Voir aussi  
[ALTER DATABASE &#40;Transact-SQL&#41;](~/t-sql/statements/alter-database-transact-sql-set-options.md)  
[DBCC CHECKDB &#40;Transact-SQL&#41;](~/t-sql/database-console-commands/dbcc-checkdb-transact-sql.md)  
[Restaurations complètes de bases de données &#40;mode de récupération simple&#41;](~/relational-databases/backup-restore/complete-database-restores-simple-recovery-model.md)  
[Transactions différées](../backup-restore/deferred-transactions-sql-server.md)  
[MSSQLSERVER_824](~/relational-databases/errors-events/mssqlserver-824-database-engine-error.md)  
[sys.databases &#40;Transact-SQL&#41;](~/relational-databases/system-catalog-views/sys-databases-transact-sql.md)

  
