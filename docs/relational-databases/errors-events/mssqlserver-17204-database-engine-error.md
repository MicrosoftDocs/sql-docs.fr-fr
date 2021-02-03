---
description: MSSQLSERVER_17204
title: MSSQLSERVER_17204 | Microsoft Docs
ms.custom: ''
ms.date: 07/25/2020
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 17204 (Database Engine error)
ms.assetid: 40db66f9-dd5e-478c-891e-a06d363a2552
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 3a09686fc277f95bfac11c97aeda51a6b8cf0e85
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99196713"
---
# <a name="mssqlserver_17204"></a>MSSQLSERVER_17204
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>Détails  
  
| Attribut | Valeur |
| :-------- | :---- |
|Nom du produit|SQL Server|  
|ID de l’événement|17204|  
|Source de l’événement|MSSQLSERVER|  
|Composant|SQLEngine|  
|Nom symbolique|DBLKIO_DEVOPENFAILED|  
|Texte du message|%ls : impossible d’ouvrir le fichier %ls pour le numéro de fichier %d.  Erreur de système d'exploitation : %ls.|  
  
## <a name="explanation"></a>Explication  
SQL Server n'a pas pu ouvrir le fichier spécifié en raison de l'erreur de système d’exploitation spécifiée.  

Vous pouvez voir l’erreur 17204 dans l’événement d’application Windows ou le journal des erreurs [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] lorsque SQL Server ne peut pas ouvrir une base de données et/ou des fichiers journaux de transactions. Voici un exemple de ce à quoi cette erreur peut ressembler :

``` 
Error: 17204, Severity: 16, State: 1.
FCB::Open failed: Could not open file c:\Program Files\Microsoft SQL Server\MSSQL10.SQL2008\MSSQL\data\MyDB_Prm.mdf for file number 1.  OS error: 5(Access is denied.).
```

Vous pouvez voir ces erreurs pendant le processus de démarrage de l’instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou toute opération de base de données qui tente de démarrer la base de données (par exemple ALTER DATABASE). Dans certains scénarios, vous pouvez voir les deux erreurs 17204 et 17207. Dans d’autres occasions, vous ne verrez qu’une d’entre elles.

Si une base de données utilisateur rencontre ces erreurs, cette base de données reste dans l’état RECOVERY_PENDING et les applications ne peuvent pas y accéder. Si une base de données système rencontre ces erreurs, l’instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ne démarre pas et vous ne pouvez pas vous connecter à cette instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Un échec avec une base de données système peut également entraîner la mise hors connexion d’une ressource de cluster de basculement [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].

## <a name="cause"></a>Cause
Avant de pouvoir utiliser une base de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], la base de données doit être démarrée. Le processus de démarrage de la base de données implique : 
1. Initialisation de diverses structures de données qui représentent la base de données et les fichiers de base de données
1. Ouverture de tous les fichiers qui appartiennent à la base de données
1. Exécution de la récupération sur la base de données 

[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] utilise la fonction d’API Windows [CreateFile](/windows/win32/api/fileapi/nf-fileapi-createfilea) pour ouvrir les fichiers qui appartiennent à une base de données.
 
Les messages 17204 (et 17207) indiquent qu’une erreur s’est produite lorsque [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] a tenté d’ouvrir des fichiers de base de données pendant le processus de démarrage.
 
Ces messages d’erreur contiennent les informations suivantes :
1. Nom de la fonction [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] qui tente d’ouvrir le fichier. Le nom de la fonction que vous observez normalement dans ces messages d’erreur est l’un des suivants :
   - FCB::Open - le fichier a rencontré une erreur quand [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] a tenté de l’ouvrir
   - FileMgr::StartPrimaryDataFiles - un fichier de données principal ou un fichier appartenant au groupe de fichiers principal
   - FileMgr::StartSecondaryDataFiles - un fichier appartenant à un groupe de fichiers secondaire
   - FileMgr::StartLogFiles - un fichier de journal des transactions
   - STREAMFCB::Startup - un conteneur FileStream SQL
   - FCB::RemoveAlternateStreams
  
      
1. Les informations d’état distinguent plusieurs emplacements au sein d’une fonction qui peuvent générer ce message d’erreur
1. Le chemin d’accès physique complet du fichier
1. ID du fichier correspondant au fichier
1. Code d’erreur du système d’exploitation et description de l’erreur. Dans certains cas, vous ne verrez que le code d’erreur.
 
Les informations sur l’erreur du système d’exploitation imprimées dans ces messages d’erreur sont la cause principale de l’erreur 17204. Les causes courantes de ces messages d’erreur sont un problème d’autorisation ou un chemin d’accès au fichier incorrect.


## <a name="user-action"></a>Action de l'utilisateur  
1. La résolution de l’erreur 17204 implique la compréhension du code d’erreur de système d’exploitation associé et le diagnostic de cette erreur. Une fois la condition d’erreur du système d’exploitation résolue, vous pouvez tenter de redémarrer la base de données (à l’aide de l’instruction ALTER DATABASE SET ONLINE, par exemple) ou de l’instance [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pour mettre en ligne la base de données affectée. Dans certains cas, il se peut que vous ne puissiez pas résoudre l’erreur du système d’exploitation. Ensuite, vous devez prendre des mesures correctives spécifiques. Nous aborderons ces actions dans cette section.
1. Si le message d’erreur 17204 contient uniquement un code d’erreur et non une description d’erreur, vous pouvez essayer de résoudre le code d’erreur en utilisant la commande suivante à partir d’un interpréteur de commandes de système d’exploitation : net helpmsg <error code>. Si vous obtenez un code d’état à 8 chiffres comme code d’erreur, vous pouvez vous reporter aux sources d’informations comme [Comment convertir un HRESULT en code d’erreur Win32 ?](https://devblogs.microsoft.com/oldnewthing/20061103-07/?p=29133) afin de décoder les codes d’état en erreurs de système d’exploitation.
1. Si vous recevez l’erreur de système d’exploitation `Access is Denied` = 5, envisagez les méthodes suivantes :
   -  Vérifiez les autorisations définies pour le fichier en examinant les propriétés du fichier dans l’Explorateur Windows. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] utilise des groupes Windows pour approvisionner Access Control sur les diverses ressources de fichiers. Assurez-vous que le groupe approprié [avec des noms tels que SQLServerMSSQLUser$NomOrdinateur$MSSQLSERVER ou SQLServerMSSQLUser$NomOrdinateur$NomInstance] dispose des autorisations requises sur le fichier de base de données mentionné dans le message d’erreur. Consultez [Configurer les autorisations du système de fichiers pour l'accès au moteur de base de données](/previous-versions/sql/2014/database-engine/configure-windows/configure-file-system-permissions-for-database-engine-access) pour plus de détails. Assurez-vous que le groupe Windows comprend réellement le compte de démarrage du service [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou le SID du service.
   -  Examinez le compte d’utilisateur sous lequel le service [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] est en cours d’exécution. Vous pouvez utiliser le gestionnaire des tâches de Windows pour accéder à ces informations. Recherchez la valeur « Nom d’utilisateur » pour l’exécutable « sqlservr.exe ». En outre, si vous avez récemment modifié le compte de service [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], sachez que la méthode prise en charge pour effectuer cette opération consiste à se servir de l’utilitaire Gestionnaire de configuration SQL Server. Pour plus d’informations à ce propos, consultez [Gestionnaire de configuration SQL Server](../sql-server-configuration-manager.md). 
   -  Selon le type d’opération (ouverture de bases de données au démarrage serveur, attachement d’une base de données, restauration d’une base de données, etc.), le compte utilisé pour l’emprunt d’identité et l’accès au fichier de base de données peut être différent. Passez en revue la rubrique [Sécurisation des fichiers de données et des fichiers journaux](/previous-versions/sql/sql-server-2008-r2/ms189128(v=sql.105)) pour savoir quelle opération définit quelles autorisations pour quels comptes. Utilisez un outil tel que Windows SysInternals [Process Monitor](/sysinternals/downloads/procmon) pour déterminer si l’accès au fichier se produit dans le contexte de sécurité du compte de démarrage du service d’instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [ou du SID de service] ou d’un compte avec emprunt d’identité.

      Si [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] emprunte les informations d’identification de l’utilisateur pour la connexion qui exécute l’opération ALTER DATABASE ou CREATE DATABASE, vous remarquerez les informations suivantes dans l’outil Process Monitor (un exemple) :
        
        ```output
        Date & Time:      3/27/2010 8:26:08 PM
        Event Class:        File System
        Operation:          CreateFile
        Result:                ACCESS DENIED
        Path:                  C:\Program Files\Microsoft SQL Server\MSSQL10.SQL2008\MSSQL\DATA\attach_test.mdf
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
  
1. Si vous obtenez l’erreur de système d’exploitation `The system cannot find the file specified` = 3:
   - Examinez le chemin complet indiqué dans le message d’erreur.
   - Vérifiez que le lecteur de disque et le chemin du dossier sont visibles et accessibles à partir de l’Explorateur Windows.
   - Consultez le journal des événements Windows pour déterminer s’il existe des problèmes avec ce lecteur de disque.
   - Si le chemin est incorrect et que cette base de données existe déjà dans le système, vous pouvez modifier le chemin des fichiers de base de données suivant les méthodes décrites dans la rubrique [Déplacement de fichiers de base de données](../databases/move-database-files.md). Cette procédure peut se révéler nécessaire, en particulier, pour les fichiers de base de données système qui rencontrent l’erreur 17204 ou 17207, et dans les scénarios de récupération d’urgence dans lesquels les lecteurs de disque spécifiés ne sont pas disponibles. Cette rubrique explique également comment identifier l’emplacement actuel des différentes bases de données système [master, model, tempdb, msdb et mssqlsystemresource].
   - Si vous voyez cette erreur parce que les fichiers de base de données sont absents, vous devez restaurer la base de données à partir d’une sauvegarde valide.
     - Si le fichier de base de données associé à l’erreur appartient à un groupe de fichiers secondaire, vous pouvez éventuellement marquer ce groupe de fichiers hors connexion, mettre la base de données en ligne, puis effectuer une restauration de ce groupe de fichiers uniquement. Pour plus d’informations, reportez-vous à la section OFFLINE de la rubrique [Options de fichiers et de groupes de fichiers ALTER DATABASE (Transact-SQL)](../../t-sql/statements/alter-database-transact-sql-file-and-filegroup-options.md).
     - Si le fichier qui a généré l’erreur est un fichier journal de transactions, passez en revue les informations contenues dans les sections « FOR ATTACH » et « FOR ATTACH_REBUILD_LOG » de la rubrique [CREATE DATABASE (Transact-SQL)](../../t-sql/statements/create-database-transact-sql.md) pour comprendre comment vous pouvez recréer les fichiers journaux de transactions manquants.
   - Assurez-vous que tous les emplacements de disque ou réseau [comme un lecteur iSCSI] sont disponibles avant que [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ne tente d’accéder aux fichiers de base de données à ces emplacements. Si nécessaire, créez les dépendances requises dans l’Administrateur de cluster ou le Gestionnaire de contrôle des services.
1. Si vous obtenez l’erreur de système d’exploitation `The process cannot access the file because it is being used by another process` = 32 :
   - Utilisez un outil comme [Process Explorer](/sysinternals/downloads/process-explorer) ou [Handle](/sysinternals/downloads/handle) dans Windows Sysinternals pour déterminer si un autre processus ou service a acquis un verrou exclusif sur ce fichier de base de données.
   - Empêchez ce processus d’accéder aux fichiers de base de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Les antivirus en sont des exemples courants (pour plus d’informations sur les exclusions de fichiers, consultez [l’article de la Base de connaissances](https://support.microsoft.com/help/309422/choosing-antivirus-software-for-computers-that-run-sql-server) suivant).
   - Dans un environnement de cluster, assurez-vous que le processus sqlservr. exe du nœud propriétaire précédent a effectivement libéré les descripteurs des fichiers de base de données. Cela n’arrive normalement pas, mais des erreurs de configuration du cluster ou des chemins d’E/S peuvent provoquer ce type de problème.
