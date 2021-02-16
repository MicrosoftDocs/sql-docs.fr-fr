---
title: 'Sauvegarder plusieurs bases de données : Stockage Blob Azure'
description: Cet article fournit des exemples de script pouvant être utilisés pour automatiser les sauvegardes dans SQL Server et les envoyer vers le service Stockage Blob Azure à l’aide d’applets de commande PowerShell.
titleSuffix: PowerShell
ms.custom: seo-lt-2019
ms.date: 12/17/2019
ms.prod: sql
ms.prod_service: backup-restore
ms.reviewer: ''
ms.technology: backup-restore
ms.topic: conceptual
ms.assetid: f7008339-e69d-4e20-9265-d649da670460
author: cawrites
ms.author: chadam
ms.openlocfilehash: e3fb357b16572f34c906942e79be2d8488b49a15
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100347726"
---
# <a name="back-up-multiple-databases-to-azure-blob-storage---powershell"></a>Sauvegarder plusieurs bases de données dans le service Stockage Blob Azure - PowerShell

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

Cette rubrique fournit des exemples de script pouvant être utilisés pour automatiser les sauvegardes dans le service Stockage Blob Azure à l’aide d’applets de commande PowerShell.  
  
## <a name="overview-of-powershell-cmdlets-for-backup-and-restore"></a>Présentation des applets de commande PowerShell pour la sauvegarde et la restauration

**Backup-SqlDatabase** et **Restore-SqlDatabase** sont les deux principales applets de commande disponibles pour effectuer des opérations de sauvegarde et de restauration.

En outre, il existe d’autres applets de commande qui peuvent être nécessaires pour automatiser les sauvegardes dans le Stockage Blob Microsoft Azure, comme le jeu d’applets de commande **SqlCredential**.

Pour obtenir la liste de toutes les applets de commande disponibles, consultez [Applets de commande SqlServer](/powershell/module/sqlserver).
  
> [!TIP]  
> Les applets de commande **SqlCredential** sont utilisées dans les scénarios de sauvegarde et de restauration dans Stockage Blob Azure. Pour plus d’informations sur les informations d’identification et leur utilisation, consultez [Sauvegarde et restauration SQL Server avec le service Stockage Blob Microsoft Azure](../../relational-databases/backup-restore/sql-server-backup-and-restore-with-microsoft-azure-blob-storage-service.md).
  
### <a name="powershell-for-multi-database-multi-instance-backup-operations"></a>PowerShell pour des opérations de sauvegarde de plusieurs bases de données, sur plusieurs instances

Les sections suivantes contiennent des scripts pour différentes opérations, notamment la création d’informations d’identification SQL sur plusieurs instances de SQL Server, la sauvegarde de toutes les bases de données utilisateur dans une instance de SQL Server, etc. Utilisez ces scripts pour automatiser ou planifier des opérations de sauvegarde en fonction des spécifications de votre environnement. Les scripts fournis ici sont des exemples, et peuvent être modifiés ou étendus pour votre environnement.  
  
Voici quelques observations concernant les exemples de script :  
  
- SQL Server PowerShell implémente des applets de commande pour parcourir la structure de chemin qui représente la hiérarchie des objets pris en charge par un fournisseur PowerShell. Une fois que vous avez accédé à un nœud dans le chemin d'accès, vous pouvez utiliser d'autres applets de commande pour exécuter des opérations de base sur l'objet actif.

  Pour plus d’informations, consultez [Navigate SQL Server PowerShell Paths](../../powershell/navigate-sql-server-powershell-paths.md).

- Applet de commande **Get-ChildItem** : les informations retournées par **Get-ChildItem** dépendent de l’emplacement dans un chemin SQL Server PowerShell. Par exemple, si l'emplacement est au niveau de l'ordinateur, cette applet de commande retourne toutes les instances du moteur de base de données SQL Server installées sur l'ordinateur. Sinon, si l’emplacement est au niveau de l’objet, par exemple les bases de données, cette applet de commande retourne une liste d’objets de base de données. Par défaut, l’applet de commande **Get-ChildItem** ne retourne pas d’objets système. Utilisez le paramètre `–Force` pour afficher les objets système.

- Un compte de stockage Azure et des informations d’identification SQL sont des prérequis pour toutes les opérations de sauvegarde et de restauration dans le service Stockage Blob Azure.
  
### <a name="create-a-sql-credential-on-all-instances-of-sql-server"></a>Créer des informations d’identification SQL sur toutes les instances de SQL Server

Le script suivant permet de créer des informations d’identification SQL génériques sur toutes les instances de SQL Server sur un ordinateur. S’il existe déjà des informations d’identification portant le même nom sur l’une des instances de l’ordinateur, le script affiche l’erreur et continue.  
  
```powershell
# load the sqlps module
import-module sqlps  
  
# set parameters
$sqlPath = "sqlserver:\sql\$($env:COMPUTERNAME)"
$storageAccount = "<myStorageAccount>"  
$storageKey = "<myStorageAccessKey>"  
$secureString = ConvertTo-SecureString $storageKey -AsPlainText -Force  
$credentialName = "myCredential-$(Get-Random)"

Write-Host "Generate credential: " $credentialName
  
#cd to sql server and get instances  
cd $sqlPath
$instances = Get-ChildItem

#loop through instances and create a SQL credential, output any errors
foreach ($instance in $instances)  {
    try {
        $path = "$($sqlPath)\$($instance.DisplayName)\credentials"
        New-SqlCredential -Name $credentialName -Identity $storageAccount -Secret $secureString -Path $path -ea Stop | Out-Null
        Write-Host "...generated credential $($path)\$($credentialName)."  }
    catch { Write-Host $_.Exception.Message } }
```

> [!NOTE]
> L’instruction `-ea Stop | Out-Null` est utilisée pour les sorties d’exceptions définies par l’utilisateur. Si vous préférez des messages d’erreur PowerShell par défaut, cette instruction peut être supprimée. 

### <a name="remove-a-sql-credential-from-all-instances-of-sql-server"></a>Supprimer des informations d’identification SQL de toutes les instances de SQL Server

Le script suivant permet de supprimer des informations d’identification spécifiques de toutes les instances de SQL Server installées sur l’ordinateur. Si les informations d’identification n’existent pas sur une instance spécifique, un message d’erreur s’affiche et le script continue jusqu’à ce que toutes les instances soient vérifiées.  
  
```powershell
import-module sqlps

$sqlPath = "sqlserver:\sql\$($env:COMPUTERNAME)"
$credentialName = "<myCredential>"

Write-Host "Delete credential: " $credentialName

cd $sqlPath
$instances = Get-ChildItem

#loop through instances and delete a SQL credential
foreach ($instance in $instances)  {
    try {
        $path = "$($sqlPath)\$($instance.DisplayName)\credentials\$($credentialName)"
        Remove-SqlCredential -Path $path -ea Stop | Out-Null
        Write-Host "...deleted credential $($path)."  }
    catch { Write-Host $_.Exception.Message } }
```  
  
### <a name="full-backup-for-all-databases"></a>Sauvegarde complète de toutes les bases de données

Le script suivant crée des sauvegardes de toutes les bases de données sur l'ordinateur. Cela inclut les bases de données utilisateur et la base de données système **msdb** . Le script exclut les bases de données système **tempdb** et **model** .  
  
```powershell
import-module sqlps  

$sqlPath = "sqlserver:\sql\$($env:COMPUTERNAME)"
$storageAccount = "<myStorageAccount>"  
$blobContainer = "<myBlobContainer>"  
$backupUrlContainer = "https://$storageAccount.blob.core.windows.net/$blobContainer/"  
$credentialName = "<myCredential>"

Write-Host "Backup database: " $backupUrlContainer
  
cd $sqlPath
$instances = Get-ChildItem

#loop through instances and backup all databases (excluding tempdb and model)
foreach ($instance in $instances)  {
    $path = "$($sqlPath)\$($instance.DisplayName)\databases"
    $databases = Get-ChildItem -Force -Path $path | Where-object {$_.name -ne "tempdb" -and $_.name -ne "model"}

    foreach ($database in $databases) {
        try {
            $databasePath = "$($path)\$($database.Name)"
            Write-Host "...starting backup: " $databasePath
            Backup-SqlDatabase -Database $database.Name -Path $path -BackupContainer $backupUrlContainer -SqlCredential $credentialName -Compression On
            Write-Host "...backup complete."  }
        catch { Write-Host $_.Exception.Message } } }
```  
  
### <a name="full-backup-for-system-databases-only-on-a-specific-instance-of-sql-server"></a>Sauvegarde complète des bases de données système uniquement sur une instance spécifique de SQL Server

Le script complet peut être utilisé pour la sauvegarde des bases de données **master** et **msdb** sur une instance nommée de SQL Server. Le même script peut être utilisé pour n’importe quelle instance en modifiant la valeur du paramètre d’instance. L’instance par défaut de SQL Server est nommée `DEFAULT`.
  
```powershell
import-module sqlps  

$sqlPath = "sqlserver:\sql\$($env:COMPUTERNAME)"
$instanceName = "DEFAULT"
$storageAccount = "<myStorageAccount>"  
$blobContainer = "<myBlobContainer>"  
$backupUrlContainer = "https://$storageAccount.blob.core.windows.net/$blobContainer/"  
$credentialName = "<myCredential>"

Write-Host "Backup database: " $instanceName " to " $backupUrlContainer
  
cd "$($sqlPath)\$($instanceName)"

#loop through instance and backup specific databases
$databases = "master", "msdb"  
foreach ($database in $databases) {
    try {
        Write-Host "...starting backup: " $database
        Backup-SqlDatabase -Database $database -BackupContainer $backupUrlContainer -SqlCredential $credentialName -Compression On
        Write-Host "...backup complete." }
    catch { Write-Host $_.Exception.Message } }
```  
  
## <a name="see-also"></a>Voir aussi

[Sauvegarde et restauration SQL Server avec le service Stockage Blob Microsoft Azure](../../relational-databases/backup-restore/sql-server-backup-and-restore-with-microsoft-azure-blob-storage-service.md)

[Bonnes pratiques et résolution des problèmes liés à la sauvegarde SQL Server vers une URL](../../relational-databases/backup-restore/sql-server-backup-to-url-best-practices-and-troubleshooting.md)