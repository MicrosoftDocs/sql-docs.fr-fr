---
description: MSSQLSERVER_5120
title: MSSQLSERVER_5120
ms.custom: ''
ms.date: 07/25/2020
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 5120 (Database Engine error)
ms.assetid: ''
author: PijoCoder
ms.author: mathoma
ms.openlocfilehash: 7e9ec54be052303c0b350a0037aa3f2ee9caa0d9
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100348748"
---
# <a name="mssqlserver_5120"></a>MSSQLSERVER_5120
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>Détails  
  
| Attribut | Valeur |  
| :-------- | :---- |  
|Nom du produit|SQL Server|  
|ID de l’événement|5120|  
|Source de l’événement|MSSQLSERVER|  
|Composant|SQLEngine|  
|Nom symbolique|DSK_FCB_FAILURE|  
|Texte du message|Erreur de table, Impossible d'ouvrir le fichier physique "%.*ls". Erreur %d du système d'exploitation : "%ls".|  
  
## <a name="explanation"></a>Explication  
[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] n’a pas pu ouvrir un fichier de base de données.  L’erreur de système d’exploitation fournie dans le message pointe vers des raisons sous-jacentes plus spécifiques pour l’échec. Cette erreur est souvent associée à d'autres erreurs telles que [17204](mssqlserver-17204-database-engine-error.md) ou [17207](mssqlserver-17207-database-engine-error.md) .
  
## <a name="user-action"></a>Action requise  
  
  Analysez et corrigez cette erreur de système d'exploitation, puis tentez à nouveau l'opération. Plusieurs états peuvent aider Microsoft à limiter la zone dans le produit où la partie à problème se trouve. 
  
### <a name="access-is-denied"></a>l’accès est refusé. 
Si vous recevez l’erreur de système d’exploitation `Access is Denied` = 5, envisagez les méthodes suivantes :
   -  Vérifiez les autorisations définies pour le fichier en examinant les propriétés du fichier dans l’Explorateur Windows. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] utilise des groupes Windows pour approvisionner Access Control sur les diverses ressources de fichiers. Assurez-vous que le groupe approprié [avec des noms tels que SQLServerMSSQLUser$NomOrdinateur$MSSQLSERVER ou SQLServerMSSQLUser$NomOrdinateur$NomInstance] dispose des autorisations requises sur le fichier de base de données mentionné dans le message d’erreur. Consultez [Configurer les autorisations du système de fichiers pour l'accès au moteur de base de données](/previous-versions/sql/2014/database-engine/configure-windows/configure-file-system-permissions-for-database-engine-access?view=sql-server-2014&preserve-view=true) pour plus de détails. Assurez-vous que le groupe Windows comprend réellement le compte de démarrage du service [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou le SID du service.
   -  Examinez le compte d’utilisateur sous lequel le service [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] est en cours d’exécution. Vous pouvez utiliser le gestionnaire des tâches de Windows pour accéder à ces informations. Recherchez la valeur « Nom d’utilisateur » pour l’exécutable « sqlservr.exe ». En outre, si vous avez récemment modifié le compte de service [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], sachez que la méthode prise en charge pour effectuer cette opération consiste à utiliser l’utilitaire [Gestionnaire de configuration SQL Server](../sql-server-configuration-manager.md). 
   -  Selon le type d’opération (ouverture de bases de données au démarrage serveur, attachement d’une base de données, restauration d’une base de données, etc.), le compte utilisé pour l’emprunt d’identité et l’accès au fichier de base de données peut être différent. Passez en revue la rubrique [Sécurisation des fichiers de données et des fichiers journaux](/previous-versions/sql/sql-server-2008-r2/ms189128(v=sql.105)) pour savoir quelle opération définit quelles autorisations pour quels comptes. Utilisez un outil tel que Windows SysInternals [Process Monitor](/sysinternals/downloads/procmon) pour déterminer si l’accès au fichier se produit dans le contexte de sécurité du compte de démarrage du service d’instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [ou du SID de service] ou d’un compte avec emprunt d’identité.

      Si [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] emprunte les informations d’identification de l’utilisateur de la connexion qui exécute l’opération ALTER DATABASE ou CREATE DATABASE, vous remarquerez les informations suivantes dans l’outil Process Monitor (exemple).
      
        ```
        Date & Time:      3/27/2010 8:26:08 PM
        Event Class:        File System
        Operation:          CreateFile
        Result:                ACCESS DENIED
        Path:                  C:\Program Files\Microsoft SQL Server\MSSQL13.SQL2016\MSSQL\DATA\attach_test.mdf
        TID:                   4288
        Duration:             0.0000366
        Desired Access:Generic Read/Write
        Disposition:        Open
        Options:            Synchronous IO Non-Alert, Non-Directory File, Open No Recall
        Attributes:          N
        ShareMode:       Read
        AllocationSize:   n/a
        Impersonating: DomainName\UserName
        ```
  
  
### <a name="attaching-files-that-reside-on-a-network-attached-storage"></a>Attachement de fichiers résidant sur un stockage connecté au réseau  
Si vous ne pouvez pas rattacher une base de données qui réside sur un stockage attaché au réseau, un message similaire à celui-ci peut être consigné dans le Journal des applications.

`Msg 5120, Level 16, State 101, Line 1 Unable to open the physical file "\\servername\sharename\filename.mdf". Operating system error 5: (Access is denied.).`

Ce problème se produit parce que [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] réinitialise les autorisations de fichier lorsque la base de données est détachée. Lorsque vous essayez de rattacher la base de données, une erreur se produit en raison d’autorisations de partage limitées.

Pour résoudre ce problème, effectuez les étapes suivantes :
1. Utilisez l’option de démarrage-T pour démarrer [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Utilisez cette option de démarrage pour activer l’indicateur de trace 1802 dans le [Gestionnaire de configuration SQL Server](../sql-server-configuration-manager.md) (consultez [Indicateurs de trace](../../t-sql/database-console-commands/dbcc-traceon-transact-sql.md) pour plus d’informations sur 1802). Pour plus d’informations sur comment modifier les options de démarrage de service, consultez [Options de démarrage du service du Moteur de base de données](../../database-engine/configure-windows/database-engine-service-startup-options.md).

2. Utilisez la commande suivante pour détacher la base de données.
   ```sql
    exec sp_detach_db DatabaseName
    go 
   ```

3. Utilisez la commande suivante pour rattacher la base de données.
   ```sql
   exec sp_attach_db DatabaseName, '\\Network-attached storage_Path\DatabaseMDFFile.mdf', '\\Network-attached storage_Path\DatabaseLDFFile.ldf'
   go
   ```
