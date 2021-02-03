---
description: MSSQLSERVER_3023
title: MSSQLSERVER_3023
ms.custom: ''
ms.date: 08/20/2020
ms.prod: sql
ms.reviewer: ramakoni1, pijocoder, suresh-kandoth, Masha
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 3023 (Database Engine error)
ms.assetid: ''
author: suresh-kandoth
ms.author: ramakoni
ms.openlocfilehash: 096d1fd1e736ea731a8fcf72d36507cd64efae1d
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99198152"
---
# <a name="mssqlserver_3023"></a>MSSQLSERVER_3023
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

## <a name="details"></a>Détails

|Attribut|Valeur|
|---|---|
|Nom du produit|SQL Server|
|ID de l’événement|3023|
|Source de l’événement|MSSQLSERVER|
|Composant|SQLEngine|
|Nom symbolique|DB_IN_USE_DUMP|
|Texte du message|Les opérations de sauvegarde et de manipulation de fichiers (comme ALTER DATABASE ADD FILE) sur une base de données doivent être sérialisées. Réémettez l’instruction à la fin de l’opération de sauvegarde ou de manipulation de fichiers en cours|
||

## <a name="explanation"></a>Explication

Vous essayez d’exécuter une commande de sauvegarde, de réduction ou de modification de base de données dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], et vous rencontrez les messages suivants :

> Message 3023, niveau 16, état 2, ligne 1  
Les opérations de sauvegarde et de manipulation de fichiers (comme ALTER DATABASE ADD FILE) sur une base de données doivent être sérialisées. Réexécutez l'instruction après la fin de la sauvegarde en cours ou de la manipulation de fichiers.

> Message 3013, niveau 16, état 1, ligne 1  
La sauvegarde de base de données s'est terminée anormalement.

De plus, le journal des erreurs [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] contient des messages tels que les suivants :

> \<Datetime> Erreur de sauvegarde : 3041, gravité : 16, état : 1.  
\<Datetime> La sauvegarde BACKUP n’a pas pu exécuter la commande BACKUP DATABASE MyDatabase WITH DIFFERENTIAL. Consultez le journal de sauvegarde des applications pour des messages détaillés.

Vous pouvez également remarquer que ces commandes rencontrent des `wait_type = LCK_M_U` et `wait_resource = DATABASE: <id> [BULKOP_BACKUP_DB]` quand leur état est affiché à partir des différentes vues de gestion dynamique, par exemple depuis `sys.dm_exec_requests` ou `sys.dm_os_waiting_tasks`.

## <a name="possible-causes"></a>Causes possibles

Il existe plusieurs règles selon lesquelles les opérations sont autorisées ou non quand une base de données complète est actuellement en cours sur une base de données. Voici quelques exemples :

- Une seule sauvegarde de données peut se produire à la fois (quand une sauvegarde complète de base de données se produit, des sauvegardes différentielles ou incrémentielles ne peuvent pas se dérouler en même temps).
- Une seule sauvegarde de fichier journal peut se produire à la fois (une sauvegarde de fichier journal est autorisée lorsqu’une sauvegarde complète de base de données est en cours).
- Vous ne pouvez pas ajouter ni supprimer des fichiers dans une base de données pendant qu’une sauvegarde est en cours.
- Vous ne pouvez pas réduire les fichiers pendant que des sauvegardes de base de données se produisent.
- Des changements de mode de récupération limités sont autorisés lors des sauvegardes.

Quand l’une de ces opérations en conflit est effectuée, les commandes rencontrent les attentes de verrous qui sont mentionnées dans la section « Explication », et vous recevez les messages 3023 et 3041.

## <a name="user-action"></a>Action requise

Examinez les planifications des différentes activités de maintenance de la base de données, puis ajustez-les afin que ces opérations ou commandes n’entrent pas en conflit.

## <a name="more-information"></a>Informations complémentaires

[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] enregistre l’heure de début et l’heure de fin de la sauvegarde dans la base de données `msdb`. Vous pouvez examiner l’historique des sauvegardes pour déterminer si une sauvegarde complète de la base de données a eu lieu lors d’une tentative de sauvegarde incrémentielle et a par conséquent provoqué l’erreur. Vous pouvez utiliser la requête suivante pour vous aider dans ce processus :

```sql
select database_name, type, backup_start_date, backup_finish_date
from msdb.dbo.backupset
order by database_name, type, backup_start_date, backup_finish_date
go
```

Vous pouvez également utiliser l’événement **User Error Message** dans la trace du Générateur de profils SQL ou l’événement **error_reported** dans les événements étendus pour suivre le signalement des messages 3023 à l’application qui a lancé la sauvegarde ou une autre commande de maintenance.
