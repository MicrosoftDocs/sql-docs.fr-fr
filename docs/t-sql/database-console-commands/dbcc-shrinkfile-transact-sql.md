---
description: DBCC SHRINKFILE (Transact-SQL)
title: DBCC SHRINKFILE (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 11/14/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- SHRINKFILE
- DBCC_SHRINKFILE_TSQL
- DBCC SHRINKFILE
- SHRINKFILE_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- data shrinking [SQL Server]
- TRUNCATEONLY option
- shrinking files
- NOTRUNCATE option
- shrinking databases
- decreasing database size
- file shrinking [SQL Server]
- database shrinking [SQL Server]
- logs [SQL Server], shrinking
- reducing database size
- DBCC SHRINKFILE statement
ms.assetid: e02b2318-bee9-4d84-a61f-2fddcf268c9f
author: pmasl
ms.author: umajay
ms.openlocfilehash: 478c7c784c25b1b71bcafd2279ac93bc7545c188
ms.sourcegitcommit: e40e75055c1435c5e3f9b6e3246be55526807b4c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/13/2021
ms.locfileid: "98151307"
---
# <a name="dbcc-shrinkfile-transact-sql"></a>DBCC SHRINKFILE (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Réduit la taille de fichier journal ou de données de la base de données active. Vous pouvez l’utiliser pour déplacer des données entre fichiers du même groupe de fichiers, ce qui a pour effet de supprimer le fichier d’origine et de permettre sa suppression de la base de données. Il est possible de réduire un fichier à une taille inférieure à celle qu’il avait à sa création, réinitialisant ainsi la taille de fichier minimale sur la nouvelle valeur.
  
![Icône Lien d’article](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql
DBCC SHRINKFILE   
(  
    { file_name | file_id }   
    { [ , EMPTYFILE ]   
    | [ [ , target_size ] [ , { NOTRUNCATE | TRUNCATEONLY } ] ]  
    }  
)  
[ WITH NO_INFOMSGS ]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments
*file_name*  
Nom logique du fichier à réduire.
  
*file_id*  
Numéro d'identification (ID) du fichier à réduire. Pour récupérer l’ID d’un fichier, utilisez la fonction système [FILE_IDEX](../../t-sql/functions/file-idex-transact-sql.md) ou interrogez l’affichage catalogue [sys.database_files](../../relational-databases/system-catalog-views/sys-database-files-transact-sql.md) dans la base de données active.
  
*target_size*  
Nombre entier correspondant à la nouvelle taille du fichier en mégaoctets. Si cette valeur n’est pas spécifiée ou si elle est égale à 0, DBCC SHRINKFILE réduit le fichier à la taille qu’il avait à sa création.
  
> [!NOTE]  
>  Vous pouvez réduire la taille par défaut d’un fichier vide avec DBCC SHRINKFILE *target_size*. Par exemple, si vous créez un fichier de 5 Mo, puis que vous le réduisez à 3 Mo pendant que le fichier est encore vide, la taille de fichier par défaut est fixée à 3 Mo. Cela s'applique uniquement aux fichiers vides qui n'ont jamais contenu des données.  
  
Cette option n'est pas prise en charge pour les conteneurs de groupe de fichiers FILESTREAM.  
Si elle est spécifiée, DBCC SHRINKFILE tente de réduire le fichier à la taille *target_size*. Les pages utilisées dans la zone du fichier à libérer sont déplacées dans l’espace libre des zones conservées du fichier. Par exemple, avec un fichier de données de 10 Mo, une opération DBCC SHRINKFILE avec *target_size* égal à 8 déplace toutes les pages utilisées des 2 derniers mégaoctets du fichier dans les pages non allouées des 8 premiers mégaoctets du fichier. DBCC SHRINKFILE ne réduit un fichier au-delà de la taille des données stockées nécessaire. De fait, s’il s’agit d’un fichier de 10 Mo et que 7 Mo sont utilisés, l’instruction DBCC SHRINKFILE avec une valeur de 6 pour *target_size* réduit la taille à 7 Mo et non pas à 6 Mo.
  
EMPTYFILE  
Permet la migration de toutes les données du fichier spécifié vers d’autres fichiers dans le **même groupe de fichiers**. En d’autres termes, EMPTYFILE migre les données du fichier spécifié vers d’autres fichiers du même groupe de fichiers. EMPTYFILE permet de garantir qu’aucune nouvelle donnée n’est ajoutée au fichier, bien qu’il ne soit pas en lecture seule. Vous pouvez utiliser l’instruction [ALTER DATABASE](../../t-sql/statements/alter-database-transact-sql.md) pour supprimer un fichier. Si l’instruction [ALTER DATABASE](../../t-sql/statements/alter-database-transact-sql.md) est employée pour modifier la taille de fichier, l’indicateur de lecture seule est réinitialisé et il devient possible d’ajouter des données.

Pour les conteneurs de groupes de fichiers FILESTREAM, il n’est pas possible de supprimer un fichier avec ALTER DATABASE tant que le récupérateur de mémoire FILESTREAM n'a pas été exécuté pour supprimer tous les fichiers conteneurs de groupes de fichiers inutiles qu'EMPTYFILE a copiés dans un autre conteneur. Pour plus d’informations, consultez [sp_filestream_force_garbage_collection &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/filestream-and-filetable-sp-filestream-force-garbage-collection.md)
  
> [!NOTE]  
>  Pour plus d’informations sur la suppression d’un conteneur FILESTREAM, consultez la section correspondante dans [Options de fichiers et de groupes de fichiers ALTER DATABASE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql-file-and-filegroup-options.md)  
  
NOTRUNCATE  
Déplace des pages allouées de la fin d’un fichier de données vers les pages non allouées du début d’un fichier, que *target_percent* soit spécifié ou non. L'espace libre à la fin du fichier n'est pas restitué au système d'exploitation et la taille physique du fichier ne change pas. Par conséquent, si NOTRUNCATE est spécifié, le fichier ne semble pas se réduire.
NOTRUNCATE n'est applicable qu'aux fichiers de données. Les fichiers journaux ne sont pas affectés.   Cette option n'est pas prise en charge pour les conteneurs de groupe de fichiers FILESTREAM.
  
TRUNCATEONLY  
Libère tout l'espace libre à la fin du fichier pour le système d'exploitation, mais n'effectue aucun déplacement de page au sein du fichier. Le fichier de données est réduit seulement jusqu'à la dernière extension allouée.
*target_size* est ignoré si spécifié avec TRUNCATEONLY.  
L'option TRUNCATEONLY ne déplace pas d'informations dans le journal, mais supprime les fichiers journaux virtuels inactifs de la fin du fichier journal. Cette option n'est pas prise en charge pour les conteneurs de groupe de fichiers FILESTREAM.
  
WITH NO_INFOMSGS  
Supprime tous les messages d'information.
  
## <a name="result-sets"></a>Jeux de résultats  
Le tableau suivant décrit les colonnes du jeu de résultats.
  
|Nom de la colonne|Description|  
|---|---|
|**DbId**|Numéro d'identification de base de données du fichier que le [!INCLUDE[ssDE](../../includes/ssde-md.md)] tente de réduire.|  
|**FileId**|Numéro d’identification du fichier que le [!INCLUDE[ssDE](../../includes/ssde-md.md)] a tenté de réduire.|  
|**CurrentSize**|Nombre de pages de 8 Ko que le fichier occupe actuellement.|  
|**MinimumSize**|Nombre de pages de 8 Ko que le fichier pourrait occuper au minimum. Ce nombre correspond à la taille minimale ou à la taille de création d'un fichier.|  
|**UsedPages**|Nombre de pages de 8 Ko que le fichier utilise actuellement.|  
|**EstimatedPages**|Nombre de pages de 8 Ko estimé par le [!INCLUDE[ssDE](../../includes/ssde-md.md)] auquel la taille du fichier peut être ramenée.|  
  
## <a name="remarks"></a>Notes  
DBCC SHRINKFILE s'applique aux fichiers de la base de données active. Pour plus d’informations sur le changement de base de données active, consultez [USE &#40;Transact-SQL&#41;](../../t-sql/language-elements/use-transact-sql.md).
  
Il est à tout moment possible d’arrêter les opérations DBCC SHRINKFILE ; le travail accompli est alors conservé. Si vous utilisez le paramètre EMPTYFILE et annulez l’opération, le fichier n’est pas marqué pour empêcher l’ajout de données supplémentaires.
  
Une erreur est générée quand une opération DBCC SHRINKFILE échoue.
  
 D’autres utilisateurs peuvent travailler dans la base de données pendant la réduction des fichiers, même si la base de données n’est pas en mode mono-utilisateur. Il n'est pas nécessaire d'exécuter l'instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en mode mono-utilisateur pour réduire les bases de données système.  
  
## <a name="shrinking-a-log-file"></a>Réduction d'un fichier journal  

Dans le cas des fichiers journaux, le [!INCLUDE[ssDE](../../includes/ssde-md.md)] utilise *target_size* pour calculer la taille cible de l’ensemble du journal. Par conséquent, *target_size* correspond à l’espace libre du journal après l’opération de réduction. La taille cible de l'ensemble du journal est ensuite convertie en taille cible de chaque fichier journal. DBCC SHRINKFILE tente de ramener immédiatement la taille de chaque fichier journal physique à la taille cible. Toutefois, si une partie du journal logique se trouve dans les journaux virtuels au-delà de la taille cible, le [!INCLUDE[ssDE](../../includes/ssde-md.md)] libère autant d'espace que possible, puis envoie un message d'information. Le message décrit les actions à effectuer pour déplacer le journal logique à partir des journaux virtuels à la fin du fichier. Lorsque les actions sont effectuées, DBCC SHRINKFILE peut être utilisé pour libérer l'espace restant.
  
Comme un fichier journal ne peut être réduit que jusqu'à une limite de fichier journal virtuel, il n’est pas toujours possible de descendre au-dessous de cette taille limite, même si le fichier n'est pas utilisé. Le [!INCLUDE[ssDE](../../includes/ssde-md.md)] sélectionne dynamiquement la taille du fichier journal virtuel lorsque les fichiers journaux sont créés ou étendus.
  
## <a name="best-practices"></a>Meilleures pratiques 
 
Prenez en compte les informations suivantes lorsque vous envisagez de réduire un fichier :
-   Une opération de réduction est plus efficace après une opération qui crée une grande quantité d'espace inutilisé, par exemple TRUNCATE TABLE ou DROP TABLE.  

-   La plupart des bases de données ont besoin d’espace libre pour les opérations quotidiennes courantes. Si vous réduisez à plusieurs reprises une base de données et que sa taille augmente de nouveau, c’est probablement le signe que l’espace réduit est nécessaire à ces opérations régulières. Dans ce cas, la réduction de la taille de la base de données ne sert à rien.  

-   Une opération de réduction ne conserve pas l'état de fragmentation des index de la base de données ; en général, elle augmente la fragmentation dans une certaine mesure. Il s'agit là d'une raison supplémentaire de ne pas réduire trop souvent la base de données.  

-   Réduisez plusieurs fichiers dans la même base de données de manière séquentielle plutôt que simultanément. La contention sur les tables système peut entraîner des blocages et des délais.  
  
## <a name="troubleshooting"></a>Résolution des problèmes  
Cette section décrit comment diagnostiquer et corriger les problèmes qui peuvent se produire lors de l'exécution de la commande DBCC SHRINKFILE.
  
### <a name="the-file-doesnt-shrink"></a>Le fichier ne se réduit pas
  
Si la taille du fichier ne change pas après une opération de réduction sans erreur, essayez la procédure suivante pour vérifier que le fichier dispose de suffisamment d’espace libre :

- Exécutez la requête suivante.  
  
```sql
SELECT name ,size/128.0 - CAST(FILEPROPERTY(name, 'SpaceUsed') AS int)/128.0 AS AvailableSpaceInMB
FROM sys.database_files;
```

-   Exécutez la commande [DBCC SQLPERF](../../t-sql/database-console-commands/dbcc-sqlperf-transact-sql.md) pour restituer l’espace utilisé dans le journal des transactions.  

L'opération de réduction ne peut pas réduire la taille du fichier si l'espace libre est insuffisant.
  
C'est en général le fichier journal qui ne semble pas se réduire. La raison en est souvent qu’il n'a pas été tronqué. Pour tronquer le journal, vous pouvez affecter la valeur SIMPLE au mode de récupération de la base de données, ou sauvegarder le journal avant de réexécuter l'opération DBCC SHRINKFILE.
  
### <a name="the-shrink-operation-is-blocked"></a>L'opération de réduction est bloquée  

Une transaction qui s’exécute sous un [niveau d’isolement basé sur le contrôle de version de ligne](../../t-sql/statements/set-transaction-isolation-level-transact-sql.md) peut bloquer les opérations de réduction. Par exemple, si une opération de suppression de grande envergure sous un niveau d'isolation basé sur le contrôle de version de ligne se déroule parallèlement à une opération DBCC SHRINK DATABASE, l'opération de réduction attend la fin de l'opération de suppression pour continuer. Dans ce cas, les opérations DBCC SHRINKFILE et DBCC SHRINKDATABASE envoient un message d'information (5202 pour SHRINKDATABASE et 5203 pour SHRINKFILE) au journal des erreurs de SQL Server. Ce message est consigné toutes les cinq minutes pendant la première heure, puis toutes les heures. Par exemple, si le journal des erreurs contient le message d'erreur suivant, l'erreur suivante se produit :
  
```
DBCC SHRINKFILE for file ID 1 is waiting for the snapshot   
transaction with timestamp 15 and other snapshot transactions linked to   
timestamp 15 or with timestamps older than 109 to finish.  
```  
  
Ce message signifie que des transactions de capture instantanée présentant un timestamp antérieur à 109 (dernière transaction effectuée par l'opération de réduction) bloquent l'opération de réduction. Il indique également que la colonne **transaction_sequence_num** ou la colonne **first_snapshot_sequence_num** de la vue de gestion dynamique [sys.dm_tran_active_snapshot_database_transactions](../../relational-databases/system-dynamic-management-views/sys-dm-tran-active-snapshot-database-transactions-transact-sql.md) contient la valeur 15. Si la colonne **transaction_sequence_num** ou la colonne **first_snapshot_sequence_num** contient un numéro inférieur à la dernière transaction effectuée par une opération de réduction (109), l'opération de réduction attend la fin de ces transactions.
  
Pour résoudre ce problème, vous pouvez effectuer l'une des opérations suivantes :
-   Mettez fin à la transaction qui bloque l'opération de réduction.
-   Mettez fin à l'opération de réduction. Le travail accompli sera conservé.  
-   Laissez simplement l'opération de réduction attendre que la transaction bloquante s'achève.  
  
## <a name="permissions"></a>Autorisations  
Nécessite l’appartenance au rôle de serveur fixe **sysadmin** ou au rôle de base de données fixe **db_owner** .
  
## <a name="examples"></a>Exemples  
  
### <a name="shrinking-a-data-file-to-a-specified-target-size"></a>Réduction d'un fichier de données à une taille cible spécifiée  
L’exemple suivant ramène la taille d’un fichier de données nommé `DataFile1` de la base de données utilisateur `UserDB` à 7 Mo.
  
```sql  
USE UserDB;  
GO  
DBCC SHRINKFILE (DataFile1, 7);  
GO  
```  
  
### <a name="shrinking-a-log-file-to-a-specified-target-size"></a>Réduction d'un fichier journal à une taille cible spécifiée  
L'exemple suivant ramène la taille du fichier journal de la base de données `AdventureWorks` à 1 Mo. Pour que la commande DBCC SHRINKFILE puisse réduire la taille du fichier, le système tronque d'abord celui-ci en attribuant la valeur SIMPLE au mode de récupération de base de données.
  
```sql  
USE AdventureWorks2012;  
GO  
-- Truncate the log by changing the database recovery model to SIMPLE.  
ALTER DATABASE AdventureWorks2012  
SET RECOVERY SIMPLE;  
GO  
-- Shrink the truncated log file to 1 MB.  
DBCC SHRINKFILE (AdventureWorks2012_Log, 1);  
GO  
-- Reset the database recovery model.  
ALTER DATABASE AdventureWorks2012  
SET RECOVERY FULL;  
GO  
```  
  
### <a name="c-truncating-a-data-file"></a>C. Troncation d'un fichier de données  
L'exemple suivant tronque le fichier de données primaire dans la base de données `AdventureWorks`. Le système interroge l'affichage catalogue `sys.database_files` afin d'obtenir la valeur `file_id` du fichier de données.
  
```sql  
USE AdventureWorks2012;  
GO  
SELECT file_id, name  
FROM sys.database_files;  
GO  
DBCC SHRINKFILE (1, TRUNCATEONLY);  
```  
  
### <a name="d-emptying-a-file"></a>D. Vidage d'un fichier  
L'exemple suivant montre comment vider un fichier de manière à ce qu'il puisse être supprimé de la base de données. Dans ce cadre, un fichier de données est d’abord créé ; il contient des données.
  
```sql  
USE AdventureWorks2012;  
GO  
-- Create a data file and assume it contains data.  
ALTER DATABASE AdventureWorks2012   
ADD FILE (  
    NAME = Test1data,  
    FILENAME = 'C:\t1data.ndf',  
    SIZE = 5MB  
    );  
GO  
-- Empty the data file.  
DBCC SHRINKFILE (Test1data, EMPTYFILE);  
GO  
-- Remove the data file from the database.  
ALTER DATABASE AdventureWorks2012  
REMOVE FILE Test1data;  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
[ALTER DATABASE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql.md)  
[DBCC &#40;Transact-SQL&#41;](../../t-sql/database-console-commands/dbcc-transact-sql.md)  
[DBCC SHRINKDATABASE &#40;Transact-SQL&#41;](../../t-sql/database-console-commands/dbcc-shrinkdatabase-transact-sql.md)  
[FILE_ID &#40;Transact-SQL&#41;](../../t-sql/functions/file-id-transact-sql.md)  
[sys.database_files &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-database-files-transact-sql.md)  
[Réduire un fichier](../../relational-databases/databases/shrink-a-file.md)
  
  
