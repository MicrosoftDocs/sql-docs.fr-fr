---
title: Fichiers et de groupes de fichiers ALTER DATABASE
description: Mettez à jour les fichiers et groupes de fichiers d’une base de données à l’aide de Transact-SQL.
titleSuffix: SQL Server (Transact-SQL)
ms.custom: seo-lt-2019
ms.date: 02/21/2019
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- ADD FILE
- ADD_FILE_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- deleting files
- removing files
- deleting filegroups
- file modifications [SQL Server]
- databases [SQL Server], size
- relocating files
- databases [SQL Server], modifying
- file additions [SQL Server], ALTER DATABASE
- file moving [SQL Server]
- default filegroups
- ALTER DATABASE statement, files and filegroups
- initializing files [SQL Server]
- database files [SQL Server]
- moving files
- filegroups [SQL Server], deleting
- adding filegroups
- adding files
- database filegroups [SQL Server]
- adding log files
- modifying files
- filegroups [SQL Server], adding
- file initialization [SQL Server]
- files [SQL Server], deleting
- files [SQL Server], adding
- databases [SQL Server], moving
ms.assetid: 1f635762-f7aa-4241-9b7a-b51b22292b07
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-mi-current||>=sql-server-2016||>=sql-server-linux-2017
ms.openlocfilehash: 46e3b216907532df7107ee1b3a211f1b8397e24f
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/01/2021
ms.locfileid: "99235899"
---
# <a name="alter-database-transact-sql-file-and-filegroup-options"></a>Options de fichiers et de groupes de fichiers ALTER DATABASE (Transact-SQL)

Modifie les fichiers et groupes de fichiers associés à la base de données. Ajoute ou supprime des fichiers et des groupes de fichiers d'une base de données et modifie ses attributs ou ses fichiers et groupes de fichiers. Pour connaître les autres options d’ALTER DATABASE, voir [ALTER DATABASE](../../t-sql/statements/alter-database-transact-sql.md).

Pour plus d’informations sur les conventions de la syntaxe, consultez [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md).

[!INCLUDE[select-product](../../includes/select-product.md)]

::: moniker range=">=sql-server-2016||>=sql-server-linux-2017"

:::row:::
    :::column:::
        **_\* SQL Server \*_** &nbsp;
    :::column-end:::
    :::column:::
        [SQL Managed Instance](alter-database-transact-sql-file-and-filegroup-options.md?view=azuresqldb-mi-current&preserve-view=true)
    :::column-end:::
:::row-end:::

&nbsp;

## <a name="syntax"></a>Syntaxe

```syntaxsql
ALTER DATABASE database_name
{
    <add_or_modify_files>
  | <add_or_modify_filegroups>
}

<add_or_modify_files>::=
{
    ADD FILE <filespec> [ ,...n ]
        [ TO FILEGROUP { filegroup_name } ]
  | ADD LOG FILE <filespec> [ ,...n ]
  | REMOVE FILE logical_file_name
  | MODIFY FILE <filespec>
}

<filespec>::=
(
    NAME = logical_file_name
    [ , NEWNAME = new_logical_name ]
    [ , FILENAME = {'os_file_name' | 'filestream_path' | 'memory_optimized_data_path' } ]
    [ , SIZE = size [ KB | MB | GB | TB ] ]
    [ , MAXSIZE = { max_size [ KB | MB | GB | TB ] | UNLIMITED } ]
    [ , FILEGROWTH = growth_increment [ KB | MB | GB | TB| % ] ]
    [ , OFFLINE ]
)

<add_or_modify_filegroups>::=
{
    | ADD FILEGROUP filegroup_name
        [ CONTAINS FILESTREAM | CONTAINS MEMORY_OPTIMIZED_DATA ]
    | REMOVE FILEGROUP filegroup_name
    | MODIFY FILEGROUP filegroup_name
        { <filegroup_updatability_option>
        | DEFAULT
        | NAME = new_filegroup_name
        | { AUTOGROW_SINGLE_FILE | AUTOGROW_ALL_FILES }
        }
}
<filegroup_updatability_option>::=
{
    { READONLY | READWRITE }
    | { READ_ONLY | READ_WRITE }
}
```

## <a name="arguments"></a>Arguments

**\<add_or_modify_files>::=**

Spécifie le fichier à ajouter, supprimer ou modifier.

*database_name* Spécifie le nom de la base de données à modifier.

ADD FILE Ajoute un fichier à la base de données.

TO FILEGROUP { *filegroup_name* } Spécifie le groupe de fichiers auquel le fichier spécifié doit être ajouté. Pour afficher les groupes de fichiers existants et le groupe de fichiers par défaut actuel, utilisez la vue de catalogue [sys.filegroups](../../relational-databases/system-catalog-views/sys-filegroups-transact-sql.md).

ADD LOG FILE Ajoute un fichier journal à la base de données spécifiée.

REMOVE FILE *logical_file_name* Supprime la description du fichier logique d’une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et supprime le fichier physique. Le fichier ne peut pas être supprimé s'il n'est pas vide.

*logical_file_name* Spécifie le nom logique utilisé pour référencer le fichier dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].

> [!WARNING]
> Vous pouvez supprimer un fichier de base de données qui est associé à des sauvegardes `FILE_SNAPSHOT`, mais les instantanés associés ne sont pas supprimés pour éviter l’invalidation des sauvegardes qui font référence au fichier de base de données. Le fichier est tronqué, mais il n’est pas supprimé physiquement afin de conserver les sauvegardes FILE_SNAPSHOT. Pour plus d’informations, voir [SQL Server Backup and Restore with Microsoft Azure Blob Storage Service](../../relational-databases/backup-restore/sql-server-backup-and-restore-with-microsoft-azure-blob-storage-service.md)(en anglais). **S’applique à** : [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] et versions ultérieures).

MODIFY FILE Spécifie le fichier à modifier. Vous pouvez changer une seule propriété \<filespec> à la fois. La clause NAME doit toujours être spécifiée dans \<filespec> pour identifier le fichier à modifier. Si vous définissez l'option SIZE, la nouvelle taille doit être supérieure à la taille actuelle du fichier.

Pour modifier le nom logique d'un fichier de données ou d'un fichier journal, spécifiez le nom logique du fichier à renommer dans la clause `NAME` et indiquez le nouveau nom logique à appliquer dans la clause `NEWNAME`. Par exemple :

```sql
MODIFY FILE ( NAME = logical_file_name, NEWNAME = new_logical_name )
```

Pour déplacer un fichier de données ou un fichier journal vers un nouvel emplacement, spécifiez le nom de fichier logique actuel dans la clause `NAME` et les nouveaux chemin d'accès et nom de fichier de système d'exploitation dans la clause `FILENAME`. Par exemple :

```sql
MODIFY FILE ( NAME = logical_file_name, FILENAME = ' new_path/os_file_name ' )
```

Lorsque vous déplacez un catalogue de texte intégral, spécifiez uniquement le nouveau fichier dans la clause FILENAME. N'indiquez pas le nom de fichier du système d'exploitation.

Pour plus d’informations, consultez [Déplacer des fichiers de bases de données](../../relational-databases/databases/move-database-files.md).

Pour un groupe de fichiers FILESTREAM, NAME peut être modifié en ligne. FILENAME peut être modifié en ligne ; toutefois, la modification n'entre pas en vigueur tant que le conteneur n'a pas été déplacé physiquement et le serveur arrêté puis redémarré.

Vous pouvez définir un fichier FILESTREAM sur OFFLINE. Lorsqu'un fichier FILESTREAM est hors connexion, son groupe de fichiers parent est marqué en interne comme hors connexion ; par conséquent, tout accès aux données FILESTREAM dans ce groupe de fichiers échoue.

> [!NOTE]
> Les options \<add_or_modify_files> ne sont pas disponibles dans une base de données autonome.

**\<filespec>::=**

Contrôle les propriétés des fichiers.

NAME *logical_file_name* Spécifie le nom logique du fichier.

*logical_file_name* Spécifie le nom logique utilisé pour référencer le fichier dans une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].

NEWNAME *new_logical_file_name* Spécifie un nouveau nom logique pour le fichier.

*new_logical_file_name* Spécifie le nom remplaçant le nom de fichier logique existant. Le nom doit être unique dans la base de données et doit respecter les règles relatives aux [identificateurs](../../relational-databases/databases/database-identifiers.md). Il peut s'agir d'une constante de type caractère ou Unicode, d'un identificateur régulier ou d'un identificateur délimité.

FILENAME { **'** _os\_file\_name_ **'** | **'** _filestream\_path_ **'** | **'** _memory\_optimized\_data\_path_ **'** } Spécifie le nom de fichier (physique) du système d’exploitation.

' *os_file_name* ' Pour un groupe de fichiers standard (ROWS), il s’agit du chemin d’accès et du nom de fichier utilisés par le système d’exploitation lorsque vous créez le fichier. Le fichier doit résider sur le serveur hébergeant [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Le chemin d'accès spécifié doit exister avant l'exécution de l'instruction ALTER DATABASE.

> [!NOTE]
> Les paramètres `SIZE`, `MAXSIZE` et `FILEGROWTH` ne peuvent pas être définis lorsqu’un chemin d’accès UNC est spécifié pour le fichier.
>
> Les bases de données système ne peuvent pas résider dans les répertoires partagés UNC.

Les fichiers de données ne doivent pas être placés sur des systèmes de fichiers compressés à moins qu'il ne s'agisse de fichiers secondaires en lecture seule ou que la base de données soit en lecture seule. Les fichiers journaux ne doivent jamais être placés sur des systèmes de fichiers compressés.

Si le fichier se trouve sur une partition brute, *os_file_name* doit spécifier uniquement la lettre d’une unité correspondant à une partition brute existante. Chaque partition brute ne peut contenir qu'un seul fichier.

**'** *filestream_path* **'** Pour un groupe de fichiers FILESTREAM, FILENAME fait référence à un chemin où les données FILESTREAM seront stockées. Le chemin d'accès jusqu'au dernier dossier doit exister, et le dernier dossier ne doit pas exister. Par exemple, si vous spécifiez le chemin `C:\MyFiles\MyFilestreamData`, `C:\MyFiles` doit exister avant l’exécution de ALTER DATABASE, mais le dossier `MyFilestreamData` ne doit pas exister.

> [!NOTE]
> Les propriétés SIZE et FILEGROWTH ne s'appliquent pas à un groupe de fichiers FILESTREAM.

**'** *memory_optimized_data_path* **'** Pour un groupe de fichiers à mémoire optimisée, FILENAME fait référence à un chemin où les données à mémoire optimisée seront stockées. Le chemin d'accès jusqu'au dernier dossier doit exister, et le dernier dossier ne doit pas exister. Par exemple, si vous spécifiez le chemin `C:\MyFiles\MyData`, `C:\MyFiles` doit exister avant l’exécution de ALTER DATABASE, mais le dossier `MyData` ne doit pas exister.

Le groupe de fichiers et le fichier (`<filespec>`) doivent être créés dans la même instruction.

> [!NOTE]
> Les propriétés SIZE et FILEGROWTH ne s’appliquent pas à un groupe de fichiers MEMORY_OPTIMIZED_DATA.

Pour plus d’informations sur les groupes de fichiers à mémoire optimisée, consultez [Groupes de fichiers à mémoire optimisée](../../relational-databases/in-memory-oltp/the-memory-optimized-filegroup.md).

SIZE *size* Spécifie la taille du fichier. SIZE ne s'applique pas aux groupes de fichiers FILESTREAM.

*size* Spécifie la taille du fichier.

Quand elle est spécifiée avec l’instruction ADD FILE, la propriété *size* correspond à la taille initiale du fichier. Quand elle est spécifiée avec MODIFY FILE, *size* représente la nouvelle taille du fichier et doit avoir une valeur supérieure à la taille actuelle du fichier.

Quand la propriété *size* n’est pas spécifiée pour le fichier primaire, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] utilise la taille du fichier primaire dans la base de données **model**. Quand un fichier de données secondaire ou un fichier journal est spécifié sans sa propriété *size*, [!INCLUDE[ssDE](../../includes/ssde-md.md)] utilise une taille de fichier égale à 1 Mo.

Les indications Ko, Mo, Go et To peuvent être utilisées pour indiquer qu'il s'agit de kilo-octets, mégaoctets, gigaoctets ou téraoctets. La valeur par défaut est Mo. Précisez un nombre entier sans aucune décimale. Pour spécifier une fraction d'un mégaoctet, convertissez la valeur en kilo-octet en multipliant le nombre par 1024. Par exemple, spécifiez 1536 Ko au lieu de 1,5 MB (1,5 x 1024 = 1536).

> [!NOTE]
> `SIZE` ne peut pas être défini :
>
> - Quand un chemin UNC est spécifié pour le fichier
> - Pour les groupes de fichiers `FILESTREAM` et `MEMORY_OPTIMIZED_DATA`

MAXSIZE { *max_size*| UNLIMITED } Spécifie la taille maximale que peut atteindre le fichier.

*max_size* Spécifie la taille de fichier maximale. Les indications Ko, Mo, Go et To peuvent être utilisées pour indiquer qu'il s'agit de kilo-octets, mégaoctets, gigaoctets ou téraoctets. La valeur par défaut est Mo. Précisez un nombre entier sans aucune décimale. Si vous ne spécifiez pas la propriété *max_size*, le fichier peut s’accroître jusqu’à occuper tout l’espace disque disponible.

UNLIMITED Spécifie que la taille du fichier peut croître jusqu’à ce que le disque soit plein. Dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], un fichier journal spécifié avec une croissance illimitée a une taille maximale de 2 To et un fichier de données une taille maximale de 16 To. Aucune taille maximale n'est définie lorsque cette option est spécifiée pour un conteneur FILESTREAM. Il continue à grandir jusqu'à ce que le disque soit saturé.

> [!NOTE]
> `MAXSIZE` ne peut pas être défini quand un chemin UNC est spécifié pour le fichier.

FILEGROWTH *growth_increment* Spécifie l’incrément de croissance automatique du fichier. Le paramètre FILEGROWTH d'un fichier ne peut dépasser le paramètre MAXSIZE. FILEGROWTH ne s'applique pas aux groupes de fichiers FILESTREAM.

*growth_increment* Spécifie la quantité d’espace ajoutée au fichier quand de l’espace supplémentaire est nécessaire.

La valeur peut être exprimée en Mo, Ko, Go, To ou pourcentage (%). Si un nombre est mentionné sans spécifier Mo, Ko ou %, la valeur par défaut est Mo. Lorsque % est spécifié, la taille de l'incrément de croissance est le pourcentage choisi de la taille du fichier au moment où l'incrémentation a lieu. La taille spécifiée est arrondie à la valeur multiple de 64 Ko la plus proche.

Une valeur 0 indique que la croissance automatique est désactivée et qu'aucun espace supplémentaire n'est autorisé.

Si FILEGROWTH n’est pas spécifié, les valeurs par défaut sont les suivantes :

|Version|Valeurs par défaut|
|-------------|--------------------|
|À compter de [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)]|64 Mo de données. 64 Mo de fichiers journaux.|
|À compter de [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)]|1 Mo de données. 10 % de fichiers journaux.|
|Avant [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)]|10 % de données. 10 % de fichiers journaux.|

> [!NOTE]
> `FILEGROWTH` ne peut pas être défini :
>
> - Quand un chemin UNC est spécifié pour le fichier
> - Pour les groupes de fichiers `FILESTREAM` et `MEMORY_OPTIMIZED_DATA`

OFFLINE Place le fichier en mode hors connexion et rend tous les objets du groupe de fichiers inaccessibles.

> [!CAUTION]
> Utilisez cette option uniquement lorsque le fichier est endommagé et peut être restauré. Un fichier configuré avec l'option OFFLINE ne peut être remis en ligne qu'en le restaurant à partir d'une sauvegarde. Pour plus d’informations sur la restauration d’un fichier unique, consultez l’article sur l’instruction [RESTORE](../../t-sql/statements/restore-statements-transact-sql.md).
>
> Les options \<filespec> ne sont pas disponibles dans une base de données autonome.

**\<add_or_modify_filegroups>::=**

Ajoute, modifie ou supprime un groupe de fichiers à partir de la base de données.

ADD FILEGROUP *filegroup_name* Ajoute un groupe de fichiers à la base de données.

CONTAINS FILESTREAM Spécifie que le groupe de fichiers stocke des objets BLOB (Binary Large Objects) FILESTREAM dans le système de fichiers.

CONTAINS MEMORY_OPTIMIZED_DATA

**S’applique à** : [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] et versions ultérieures)

Spécifie que le groupe de fichiers stocke des données mémoire optimisées dans le système de fichiers. Pour plus d’informations, consultez l’article [OLTP en mémoire (optimisation en mémoire)](../../relational-databases/in-memory-oltp/in-memory-oltp-in-memory-optimization.md). Un seul groupe de fichiers `MEMORY_OPTIMIZED_DATA` est autorisé par base de données. Pour créer des tables mémoire optimisées, le groupe de fichiers ne peut pas être vide. Il doit y avoir au moins un fichier. *filegroup_name* fait référence à un chemin. Le chemin d'accès jusqu'au dernier dossier doit exister, et le dernier dossier ne doit pas exister.

REMOVE FILEGROUP *filegroup_name* Supprime un groupe de fichiers de la base de données. Le groupe de fichiers ne peut pas être supprimé s'il n'est pas vide. Commencez par supprimer tous les fichiers du groupe. Pour plus d’informations, consultez « REMOVE FILE *logical_file_name* », plus haut dans cette rubrique.

> [!NOTE]
> Sauf dans le cas où le garbage collector FILESTREAM a supprimé tous les fichiers d’un conteneur FILESTREAM, l’opération `ALTER DATABASE REMOVE FILE` pour supprimer un conteneur FILESTREAM échoue et retourne une erreur. Consultez la section [Supprimer un conteneur FILESTREAM](#removing-a-filestream-container) plus loin dans cette rubrique.

MODIFY FILEGROUP *filegroup_name* { \<filegroup_updatability_option> | DEFAULT | NAME **=** _new\_filegroup\_name_ } Modifie le groupe de fichiers en définissant l’état sur READ_ONLY ou READ_WRITE, en définissant le groupe de fichiers comme groupe par défaut pour la base de données ou en changeant le nom du groupe de fichiers.

\<filegroup_updatability_option> Définit la propriété read-only ou read/write du groupe de fichiers.

DEFAULT Remplace le groupe de fichiers par défaut de la base de données par *filegroup_name*. Dans une base de données, un seul groupe de fichiers peut être choisi comme groupe de fichiers par défaut. Pour plus d'informations, consultez [Database Files and Filegroups](../../relational-databases/databases/database-files-and-filegroups.md).

NAME = *new_filegroup_name* Remplace le nom du groupe de fichiers par *new_filegroup_name*.

AUTOGROW_SINGLE_FILE **S’applique à** : [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] et versions ultérieures)

Quand un fichier du groupe de fichiers atteint le seuil de croissance automatique, seule sa taille augmente. Il s’agit de la valeur par défaut.

AUTOGROW_ALL_FILES

**S’applique à** : [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] et versions ultérieures)

Quand un fichier du groupe de fichiers atteint le seuil de croissance automatique, la taille de tous les fichiers augmente.

> [!NOTE]
> Il s’agit de la valeur par défaut pour tempdb.

**\<filegroup_updatability_option>::=**

Définit la propriété read-only ou read/write du groupe de fichiers.

READ_ONLY | READONLY Spécifie que le groupe de fichiers est en lecture seule. La mise à jour des objets n'est pas autorisée. Le groupe de fichiers principal ne peut pas être en mode lecture seule. Pour modifier cet état, vous devez bénéficier d'un accès exclusif à la base de données. Pour plus d'informations, consultez la clause SINGLE_USER.

Étant donné qu'une base de données en lecture seule interdit la modification des données :

- La récupération automatique est ignorée au démarrage du système.
- Le compactage de la base de données est impossible.
- Tout verrouillage est impossible dans les bases de données en lecture seule, ce qui améliore les performances des requêtes.

> [!NOTE]
> Le mot clé `READONLY` sera supprimé dans une version ultérieure de [!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Évitez d’utiliser `READONLY` dans les nouveaux développements et prévoyez de modifier les applications qui utilisent actuellement `READONLY`. Utilisez `READ_ONLY` à la place.

READ_WRITE | READWRITE Spécifie que le groupe est en lecture/écriture. Les objets du groupe de fichiers peuvent être mis à jour. Pour modifier cet état, vous devez bénéficier d'un accès exclusif à la base de données. Pour plus d'informations, consultez la clause SINGLE_USER.

> [!NOTE]
> Le mot clé `READWRITE` sera supprimé dans une version ultérieure de [!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Évitez d’utiliser `READWRITE` dans les nouveaux travaux de développement et prévoyez de modifier les applications qui utilisent `READWRITE` actuellement pour qu’elles utilisent `READ_WRITE` à la place.
> [!TIP]
> Vous pouvez déterminer l’état de ces options en consultant la colonne **is_read_only** de la vue de catalogue **sys.databases** ou la propriété **Updateability** de la fonction `DATABASEPROPERTYEX`.

## <a name="remarks"></a>Notes

Pour diminuer la taille d'une base de données, utilisez [DBCC SHRINKDATABASE](../../t-sql/database-console-commands/dbcc-shrinkdatabase-transact-sql.md).

Vous ne pouvez pas ajouter ou supprimer de fichier quand une instruction `BACKUP` est en cours d’exécution.

Un maximum de 32 767 fichiers et 32 767 groupes de fichiers peut être spécifié pour chaque base de données.

Dans [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] ou une version ultérieure, l’état d’un fichier de base de données (par exemple, en ligne ou hors connexion) est géré indépendamment de l’état de la base de données. Pour plus d’informations, consultez [États des fichiers](../../relational-databases/databases/file-states.md).

- L'état des fichiers dans un groupe de fichiers détermine la disponibilité de tout le groupe de fichiers. Pour qu'un groupe de fichiers soit disponible, tous ses fichiers doivent être en ligne.
- Si un groupe de fichiers est hors connexion, toute tentative d'accès au groupe par une instruction SQL échoue avec une erreur. Quand vous créez des plans de requête pour des instructions `SELECT`, l’optimiseur de requête n’utilise pas les index non-cluster et les vues indexées qui se trouvent dans les groupes de fichiers hors connexion. Cela permet aux instructions de s'exécuter correctement. Cependant, si le groupe de fichiers hors connexion contient le segment ou l’index cluster d’une table cible, les instructions `SELECT` échouent. Les instructions `INSERT`, `UPDATE` ou `DELETE` qui modifient une table avec un index dans un groupe de fichiers hors connexion échouent également.

Les paramètres SIZE, MAXSIZE et FILEGROWTH ne peuvent pas être définis lorsqu'un chemin d'accès UNC est spécifié pour le fichier.

Les paramètres SIZE et FILEGROWTH ne peuvent pas être définis pour les groupes de fichiers à mémoire optimisée.

Le mot clé `READONLY` sera supprimé dans une version ultérieure de [!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Évitez d’utiliser `READONLY` dans les nouveaux développements et prévoyez de modifier les applications qui l’utilisent actuellement. Utilisez `READ_ONLY` à la place.

Le mot clé `READWRITE` sera supprimé dans une version ultérieure de [!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Évitez d’utiliser `READWRITE` dans les nouveaux travaux de développement et prévoyez de modifier les applications qui utilisent `READWRITE` actuellement pour qu’elles utilisent `READ_WRITE` à la place.

## <a name="moving-files"></a>Déplacement de fichiers

Vous pouvez déplacer des données système ou définies par l'utilisateur ainsi que des fichiers journaux en spécifiant le nouvel emplacement dans FILENAME. Cela peut être utile dans les cas suivants :

- Récupération après défaillance. Par exemple, la base de données est en mode suspect ou arrêtée à cause d'une défaillance matérielle.
- Déplacement prévu.
- Déplacement en vue d'une maintenance de disque planifiée.

Pour plus d’informations, consultez [Déplacer des fichiers de bases de données](../../relational-databases/databases/move-database-files.md).

## <a name="initializing-files"></a>Initialisation des fichiers

Par défaut, les fichiers de données et journaux sont initialisés en remplissant les fichiers avec des zéros lorsque vous effectuez l'une des opérations suivantes :

- Créer une base de données.
- Ajouter des fichiers à une base de données existante.
- Augmenter la taille d'un fichier existant.
- Restaurer une base de données ou un groupe de fichiers.

Les fichiers de données peuvent être initialisés de manière instantanée. Dès lors, l'exécution de ces opérations de fichiers est très rapide. Pour plus d’informations, consultez [Initialisation des fichiers de base de données](../../relational-databases/databases/database-instant-file-initialization.md).

## <a name="removing-a-filestream-container"></a><a name="removing-a-filestream-container"></a> Suppression d’un conteneur FILESTREAM

Même si le conteneur FILESTREAM peut avoir été vidé avec l’opération « DBCC SHRINKFILE », la base de données peut encore conserver des références aux fichiers supprimés pour différentes raisons de maintenance du système. [sp_filestream_force_garbage_collection](../../relational-databases/system-stored-procedures/filestream-and-filetable-sp-filestream-force-garbage-collection.md) exécutera le récupérateur de mémoire FILESTREAM pour supprimer ces fichiers quand l’opération pourra être effectuée en toute sécurité. Sauf dans le cas où le récupérateur de mémoire FILESTREAM a supprimé tous les fichiers d’un conteneur FILESTREAM, l’opération ALTER DATABASE REMOVE FILE ne parviendra pas à supprimer le conteneur FILESTREAM et retournera une erreur. Le processus suivant est recommandé pour supprimer un conteneur FILESTREAM.

1. Exécutez [DBCC SHRINKFILE](../../t-sql/database-console-commands/dbcc-shrinkfile-transact-sql.md) avec l’option EMPTYFILE pour déplacer le contenu actif de ce conteneur vers d’autres conteneurs.
2. Assurez-vous que les sauvegardes de journal ont été effectuées, en mode de récupération FULL ou BULK_LOGGED.
3. Assurez-vous que le travail du lecteur du journal de la réplication a été exécuté, le cas échéant.
4. Exécutez [sp_filestream_force_garbage_collection](../../relational-databases/system-stored-procedures/filestream-and-filetable-sp-filestream-force-garbage-collection.md) pour forcer le récupérateur de mémoire à supprimer tous les fichiers inutiles dans ce conteneur.
5. Exécutez ALTER DATABASE avec l'option REMOVE FILE pour supprimer ce conteneur.
6. Répétez encore une fois les étapes 2 à 4 pour terminer le garbage collection.
7. Utilisez ALTER base de données...REMOVE FILE pour supprimer ce conteneur.

## <a name="examples"></a>Exemples

### <a name="a-adding-a-file-to-a-database"></a>R. Ajout d'un fichier à une base de données

L'exemple suivant ajoute un fichier de données de 5 Mo à la base de données [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)].

```sql
USE master;
GO
ALTER DATABASE AdventureWorks2012
ADD FILE
(
    NAME = Test1dat2,
    FILENAME = 'C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\DATA\t1dat2.ndf',
    SIZE = 5MB,
    MAXSIZE = 100MB,
    FILEGROWTH = 5MB
);
GO
```

### <a name="b-adding-a-filegroup-with-two-files-to-a-database"></a>B. Ajout d'un groupe de deux fichiers à une base de données

L'exemple suivant crée le groupe de fichiers `Test1FG1` dans la base de données [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)] et ajoute deux fichiers de 5 Mo au groupe de fichiers.

```sql
USE master
GO
ALTER DATABASE AdventureWorks2012
ADD FILEGROUP Test1FG1;
GO
ALTER DATABASE AdventureWorks2012
ADD FILE
(
    NAME = test1dat3,
    FILENAME = 'C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\DATA\t1dat3.ndf',
    SIZE = 5MB,
    MAXSIZE = 100MB,
    FILEGROWTH = 5MB
),  
(  
    NAME = test1dat4,
    FILENAME = 'C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\DATA\t1dat4.ndf',
    SIZE = 5MB,
    MAXSIZE = 100MB,
    FILEGROWTH = 5MB
)  
TO FILEGROUP Test1FG1;
GO
```

### <a name="c-adding-two-log-files-to-a-database"></a>C. Ajout de deux fichiers journaux à une base de données

L'exemple suivant ajoute deux fichiers journaux de 5 Mo à la base de données [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)].

```sql
USE master;
GO
ALTER DATABASE AdventureWorks2012
ADD LOG FILE
(
    NAME = test1log2,
    FILENAME = 'C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\DATA\test2log.ldf',
    SIZE = 5MB,
    MAXSIZE = 100MB,
    FILEGROWTH = 5MB
),
(
    NAME = test1log3,
    FILENAME = 'C:\Program Files\Microsoft SQL Server\MSSQL10_50.MSSQLSERVER\MSSQL\DATA\test3log.ldf',
    SIZE = 5MB,
    MAXSIZE = 100MB,
    FILEGROWTH = 5MB
);
GO
```

### <a name="d-removing-a-file-from-a-database"></a>D. Suppression d'un fichier d'une base de données

L'exemple suivant supprime l'un des fichiers ajoutés dans l'exemple B.

```sql
USE master;
GO
ALTER DATABASE AdventureWorks2012
REMOVE FILE test1dat4;
GO
```

### <a name="e-modifying-a-file"></a>E. Modification d'un fichier

L’exemple suivant augmente la taille de l’un des fichiers ajoutés dans l’exemple B. L’utilisation d’ALTER DATABASE avec l’option MODIFY FILE permet uniquement d’augmenter la taille de fichier. Si vous avez besoin de réduire la taille de fichier, utilisez DBCC SHRINKFILE.

```sql
USE master;
GO

ALTER DATABASE AdventureWorks2012
MODIFY FILE
(NAME = test1dat3,
SIZE = 200MB);
GO
```

Cet exemple réduit la taille d’un fichier de données à 100 Mo, puis spécifie une taille de fichier.

```sql
USE AdventureWorks2012;
GO

DBCC SHRINKFILE (AdventureWorks2012_data, 100);
GO

USE master;
GO
ALTER DATABASE AdventureWorks2012
MODIFY FILE
(NAME = test1dat3,
SIZE = 200MB);
GO
```

### <a name="f-moving-a-file-to-a-new-location"></a>F. Déplacement d'un fichier vers un nouvel emplacement

L'exemple suivant déplace le fichier `Test1dat2` créé dans l'exemple A vers un nouveau répertoire.

> [!NOTE]
> Vous devez déplacer physiquement le fichier vers le nouveau répertoire avant d'exécuter cet exemple. Après quoi, arrêtez et démarrez l'instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou mettez la base de données [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] hors connexion (OFFLINE) puis remettez-la en ligne (ONLINE) pour implémenter la modification.

```sql
USE master;
GO
ALTER DATABASE AdventureWorks2012
MODIFY FILE
(
    NAME = Test1dat2,
    FILENAME = N'c:\t1dat2.ndf'
);
GO
```

### <a name="g-moving-tempdb-to-a-new-location"></a>G. Déplacement de tempdb vers un nouvel emplacement

L'exemple suivant déplace `tempdb` de son emplacement actuel vers un autre emplacement du disque. Étant donné que `tempdb` est recréée à chaque démarrage du service MSSQLSERVER, il n'est pas nécessaire de déplacer physiquement les fichiers de données et les fichiers journaux. Les fichiers sont créés lorsque le service est redémarré à l'étape 3. Tant que le service n'a pas été redémarré, `tempdb` continue à fonctionner dans son emplacement existant.

1. Déterminez les noms de fichiers logiques de la base de données `tempdb` et leur emplacement actuel sur le disque.

    ```sql
    SELECT name, physical_name
    FROM sys.master_files
    WHERE database_id = DB_ID('tempdb');
    GO
    ```

2. Modifiez l'emplacement de chaque fichier à l'aide de `ALTER DATABASE`.

    ```sql
    USE master;
    GO
    ALTER DATABASE tempdb
    MODIFY FILE (NAME = tempdev, FILENAME = 'E:\SQLData\tempdb.mdf');
    GO
    ALTER DATABASE tempdb
    MODIFY FILE (NAME = templog, FILENAME = 'E:\SQLData\templog.ldf');
    GO
    ```

3. Arrêtez et redémarrez l'instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].

4. Vérifiez que la modification des fichiers a bien eu lieu.

    ```sql
    SELECT name, physical_name
    FROM sys.master_files
    WHERE database_id = DB_ID('tempdb');
    ```

5. Supprimez les fichiers empdb.mdf et templog.ldf de leur emplacement d'origine.

### <a name="h-making-a-filegroup-the-default"></a>H. Configuration d'un groupe de fichiers comme groupe par défaut

L’exemple suivant configure le groupe de fichiers `Test1FG1` créé dans l’exemple B comme groupe de fichiers par défaut. Ensuite, le groupe de fichiers par défaut est reconfiguré en groupe de fichiers `PRIMARY`. Notez que `PRIMARY` doit être délimité par des crochets ou des guillemets.

```sql
USE master;
GO
ALTER DATABASE AdventureWorks2012
MODIFY FILEGROUP Test1FG1 DEFAULT;
GO
ALTER DATABASE AdventureWorks2012
MODIFY FILEGROUP [PRIMARY] DEFAULT;
GO
```

### <a name="i-adding-a-filegroup-using-alter-database"></a>I. Ajout d'un groupe de fichiers à l'aide d'ALTER DATABASE

L'exemple de code suivant ajoute un `FILEGROUP` qui contient la clause `FILESTREAM` à la base de données `FileStreamPhotoDB`.

```sql
--Create and add a FILEGROUP that CONTAINS the FILESTREAM clause.
ALTER DATABASE FileStreamPhotoDB
ADD FILEGROUP TodaysPhotoShoot
CONTAINS FILESTREAM;
GO

--Add a file for storing database photos to FILEGROUP
ALTER DATABASE FileStreamPhotoDB
ADD FILE
(
  NAME= 'PhotoShoot1',
  FILENAME = 'C:\Users\Administrator\Pictures\TodaysPhotoShoot.ndf'
)
TO FILEGROUP TodaysPhotoShoot;
GO
```

L'exemple de code suivant ajoute un `FILEGROUP` qui contient la clause `MEMORY_OPTIMIZED_DATA` à la base de données `xtp_db`. Le groupe de fichiers stocke des données à mémoire optimisée.

```sql
--Create and add a FILEGROUP that CONTAINS the MEMORY_OPTIMIZED_DATA clause.
ALTER DATABASE xtp_db
ADD FILEGROUP xtp_fg
CONTAINS MEMORY_OPTIMIZED_DATA;
GO

--Add a file for storing memory optimized data to FILEGROUP
ALTER DATABASE xtp_db
ADD FILE
(
  NAME='xtp_mod',
  FILENAME='d:\data\xtp_mod'
)
TO FILEGROUP xtp_fg;
GO
```

### <a name="j-change-filegroup-so-that-when-a-file-in-the-filegroup-meets-the-autogrow-threshold-all-files-in-the-filegroup-grow"></a>J. Changement d’un groupe de fichiers pour que la taille de tous les fichiers augmente quand un fichier du groupe de fichiers atteint le seuil de croissance automatique

 L’exemple suivant génère les instructions `ALTER DATABASE` nécessaires pour modifier les groupes de fichiers en lecture-écriture avec le paramètre `AUTOGROW_ALL_FILES`.

```sql
--Generate ALTER DATABASE ... MODIFY FILEGROUP statements
--so that all read-write filegroups grow at the same time.
SET NOCOUNT ON;

DROP TABLE IF EXISTS #tmpdbs
CREATE TABLE #tmpdbs (id INT IDENTITY(1,1), [dbid] INT, [dbname] sysname, isdone BIT);

DROP TABLE IF EXISTS #tmpfgs
CREATE TABLE #tmpfgs (id INT IDENTITY(1,1), [dbid] INT, [dbname] sysname, fgname sysname, isdone BIT);

INSERT INTO #tmpdbs ([dbid], [dbname], [isdone])
SELECT database_id, name, 0 FROM master.sys.databases (NOLOCK) WHERE is_read_only = 0 AND state = 0;

DECLARE @dbid INT, @query VARCHAR(1000), @dbname sysname, @fgname sysname

WHILE (SELECT COUNT(id) FROM #tmpdbs WHERE isdone = 0) > 0
BEGIN
  SELECT TOP 1 @dbname = [dbname], @dbid = [dbid] FROM #tmpdbs WHERE isdone = 0

  SET @query = 'SELECT ' + CAST(@dbid AS NVARCHAR) + ', ''' + @dbname + ''', [name], 0 FROM [' + @dbname + '].sys.filegroups WHERE [type] = ''FG'' AND is_read_only = 0;'
  INSERT INTO #tmpfgs
  EXEC (@query)

  UPDATE #tmpdbs
  SET isdone = 1
  WHERE [dbid] = @dbid
END;

IF (SELECT COUNT(ID) FROM #tmpfgs) > 0
BEGIN
  WHILE (SELECT COUNT(id) FROM #tmpfgs WHERE isdone = 0) > 0
  BEGIN
    SELECT TOP 1 @dbname = [dbname], @dbid = [dbid], @fgname = fgname FROM #tmpfgs WHERE isdone = 0

    SET @query = 'ALTER DATABASE [' + @dbname + '] MODIFY FILEGROUP [' + @fgname + '] AUTOGROW_ALL_FILES;'

    PRINT @query

    UPDATE #tmpfgs
    SET isdone = 1
    WHERE [dbid] = @dbid AND fgname = @fgname
  END
END;
GO
```

## <a name="see-also"></a>Voir aussi

- [CREATE DATABASE](../../t-sql/statements/create-database-transact-sql.md)
- [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md)
- [DROP DATABASE](../../t-sql/statements/drop-database-transact-sql.md)
- [sp_spaceused](../../relational-databases/system-stored-procedures/sp-spaceused-transact-sql.md)
- [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md)
- [sys.database_files](../../relational-databases/system-catalog-views/sys-database-files-transact-sql.md)
- [sys.data_spaces](../../relational-databases/system-catalog-views/sys-data-spaces-transact-sql.md)
- [sys.filegroups](../../relational-databases/system-catalog-views/sys-filegroups-transact-sql.md)
- [sys.master_files](../../relational-databases/system-catalog-views/sys-master-files-transact-sql.md)
- [Données blob (Binary Large Object)](../../relational-databases/blob/binary-large-object-blob-data-sql-server.md)
- [DBCC SHRINKFIL](../../t-sql/database-console-commands/dbcc-shrinkfile-transact-sql.md)
- [sp_filestream_force_garbage_collection](../../relational-databases/system-stored-procedures/filestream-and-filetable-sp-filestream-force-garbage-collection.md)
- [Initialisation des fichiers de base de données](../../relational-databases/databases/database-instant-file-initialization.md)

::: moniker-end
::: moniker range="=azuresqldb-mi-current"

:::row:::
    :::column:::
        [SQL Server](alter-database-transact-sql-file-and-filegroup-options.md?view=sql-server-ver15&preserve-view=true)
    :::column-end:::
    :::column:::
        **_\* SQL Managed Instance \*_**<br />&nbsp;
    :::column-end:::
:::row-end:::

&nbsp;

## <a name="azure-sql-managed-instance"></a>Azure SQL Managed Instance

Utilisez cette instruction sur une base de données dans Azure SQL Managed Instance.

## <a name="syntax-for-azure-sql-managed-instance"></a>Syntaxe d’Azure SQL Managed Instance

```syntaxsql
ALTER DATABASE database_name
{
    <add_or_modify_files>
  | <add_or_modify_filegroups>
}
[;]

<add_or_modify_files>::=
{
    ADD FILE <filespec> [ ,...n ]
        [ TO FILEGROUP { filegroup_name } ]
  | REMOVE FILE logical_file_name
  | MODIFY FILE <filespec>
}
  
<filespec>::=
(
    NAME = logical_file_name
    [ , SIZE = size [ KB | MB | GB | TB ] ]
    [ , MAXSIZE = { max_size [ KB | MB | GB | TB ] | UNLIMITED } ]
    [ , FILEGROWTH = growth_increment [ KB | MB | GB | TB| % ] ]
)

<add_or_modify_filegroups>::=
{
    | ADD FILEGROUP filegroup_name
    | REMOVE FILEGROUP filegroup_name
    | MODIFY FILEGROUP filegroup_name
        { <filegroup_updatability_option>
        | DEFAULT
        | NAME = new_filegroup_name
        | { AUTOGROW_SINGLE_FILE | AUTOGROW_ALL_FILES }
        }
}  
<filegroup_updatability_option>::=
{
    { READONLY | READWRITE }
    | { READ_ONLY | READ_WRITE }
}
```

## <a name="arguments"></a>Arguments

**\<add_or_modify_files>::=**

Spécifie le fichier à ajouter, supprimer ou modifier.

*database_name* Spécifie le nom de la base de données à modifier.

ADD FILE Ajoute un fichier à la base de données.

TO FILEGROUP { *filegroup_name* } Spécifie le groupe de fichiers auquel le fichier spécifié doit être ajouté. Pour afficher les groupes de fichiers existants et le groupe de fichiers par défaut actuel, utilisez la vue de catalogue [sys.filegroups](../../relational-databases/system-catalog-views/sys-filegroups-transact-sql.md).

REMOVE FILE *logical_file_name* Supprime la description du fichier logique d’une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et supprime le fichier physique. Le fichier ne peut pas être supprimé s'il n'est pas vide.

*logical_file_name* Spécifie le nom logique utilisé pour référencer le fichier dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].

MODIFY FILE Spécifie le fichier à modifier. Vous pouvez changer une seule propriété \<filespec> à la fois. La clause NAME doit toujours être spécifiée dans \<filespec> pour identifier le fichier à modifier. Si vous définissez l'option SIZE, la nouvelle taille doit être supérieure à la taille actuelle du fichier.

**\<filespec>::=**

Contrôle les propriétés des fichiers.

NAME *logical_file_name* Spécifie le nom logique du fichier.

*logical_file_name* Spécifie le nom logique utilisé pour référencer le fichier dans une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].

NEWNAME *new_logical_file_name* Spécifie un nouveau nom logique pour le fichier.

*new_logical_file_name* Spécifie le nom remplaçant le nom de fichier logique existant. Le nom doit être unique dans la base de données et doit respecter les règles relatives aux [identificateurs](../../relational-databases/databases/database-identifiers.md). Il peut s'agir d'une constante de type caractère ou Unicode, d'un identificateur régulier ou d'un identificateur délimité.

SIZE *size* Spécifie la taille du fichier.

*size* Spécifie la taille du fichier.

Quand elle est spécifiée avec l’instruction ADD FILE, la propriété *size* correspond à la taille initiale du fichier. Quand elle est spécifiée avec MODIFY FILE, *size* représente la nouvelle taille du fichier et doit avoir une valeur supérieure à la taille actuelle du fichier.

Quand la propriété *size* n’est pas spécifiée pour le fichier primaire, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] utilise la taille du fichier primaire dans la base de données **model**. Quand un fichier de données secondaire ou un fichier journal est spécifié sans sa propriété *size*, [!INCLUDE[ssDE](../../includes/ssde-md.md)] utilise une taille de fichier égale à 1 Mo.

Les indications Ko, Mo, Go et To peuvent être utilisées pour indiquer qu'il s'agit de kilo-octets, mégaoctets, gigaoctets ou téraoctets. La valeur par défaut est Mo. Précisez un nombre entier sans aucune décimale. Pour spécifier une fraction d'un mégaoctet, convertissez la valeur en kilo-octet en multipliant le nombre par 1024. Par exemple, spécifiez 1536 Ko au lieu de 1,5 MB (1,5 x 1024 = 1536).

MAXSIZE { *max_size*| UNLIMITED } Spécifie la taille maximale que peut atteindre le fichier.

*max_size* Spécifie la taille de fichier maximale. Les indications Ko, Mo, Go et To peuvent être utilisées pour indiquer qu'il s'agit de kilo-octets, mégaoctets, gigaoctets ou téraoctets. La valeur par défaut est Mo. Précisez un nombre entier sans aucune décimale. Si vous ne spécifiez pas la propriété *max_size*, le fichier peut s’accroître jusqu’à occuper tout l’espace disque disponible.

UNLIMITED Spécifie que la taille du fichier peut croître jusqu’à ce que le disque soit plein. Dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], un fichier journal spécifié avec une croissance illimitée a une taille maximale de 2 To et un fichier de données une taille maximale de 16 To.

FILEGROWTH *growth_increment* Spécifie l’incrément de croissance automatique du fichier. Le paramètre FILEGROWTH d'un fichier ne peut dépasser le paramètre MAXSIZE.

*growth_increment* Spécifie la quantité d’espace ajoutée au fichier quand de l’espace supplémentaire est nécessaire.

La valeur peut être exprimée en Mo, Ko, Go, To ou pourcentage (%). Si un nombre est mentionné sans spécifier Mo, Ko ou %, la valeur par défaut est Mo. Lorsque % est spécifié, la taille de l'incrément de croissance est le pourcentage choisi de la taille du fichier au moment où l'incrémentation a lieu. La taille spécifiée est arrondie à la valeur multiple de 64 Ko la plus proche.

Une valeur 0 indique que la croissance automatique est désactivée et qu'aucun espace supplémentaire n'est autorisé.

Si FILEGROWTH n’est pas spécifié, les valeurs par défaut sont les suivantes :

- 64 Mo de données ;
- 64 Mo de fichiers journaux.

**\<add_or_modify_filegroups>::=**

Ajoute, modifie ou supprime un groupe de fichiers à partir de la base de données.

ADD FILEGROUP *filegroup_name* Ajoute un groupe de fichiers à la base de données.

L’exemple suivant crée un groupe de fichiers qui est ajouté à une base de données nommée sql_db_mi, puis ajoute un fichier au groupe de fichiers.

```sql
ALTER DATABASE sql_db_mi ADD FILEGROUP sql_db_mi_fg;
GO
ALTER DATABASE sql_db_mi ADD FILE (NAME='sql_db_mi_mod') TO FILEGROUP sql_db_mi_fg;
```

REMOVE FILEGROUP *filegroup_name* Supprime un groupe de fichiers de la base de données. Le groupe de fichiers ne peut pas être supprimé s'il n'est pas vide. Commencez par supprimer tous les fichiers du groupe. Pour plus d’informations, consultez « REMOVE FILE *logical_file_name* », plus haut dans cette rubrique.

MODIFY FILEGROUP _filegroup\_name_ { \<filegroup_updatability_option> | DEFAULT | NAME **=** _new\_filegroup\_name_ } Modifie le groupe de fichiers en définissant l’état sur READ_ONLY ou READ_WRITE, en définissant le groupe de fichiers comme groupe par défaut pour la base de données ou en changeant le nom du groupe de fichiers.

\<filegroup_updatability_option> Définit la propriété read-only ou read/write du groupe de fichiers.

DEFAULT Remplace le groupe de fichiers par défaut de la base de données par *filegroup_name*. Dans une base de données, un seul groupe de fichiers peut être choisi comme groupe de fichiers par défaut. Pour plus d'informations, consultez [Database Files and Filegroups](../../relational-databases/databases/database-files-and-filegroups.md).

NAME = *new_filegroup_name* Remplace le nom du groupe de fichiers par *new_filegroup_name*.

AUTOGROW_SINGLE_FILE

Quand un fichier du groupe de fichiers atteint le seuil de croissance automatique, seule sa taille augmente. Il s’agit de la valeur par défaut.

AUTOGROW_ALL_FILES

Quand un fichier du groupe de fichiers atteint le seuil de croissance automatique, la taille de tous les fichiers augmente.

**\<filegroup_updatability_option>::=**

Définit la propriété read-only ou read/write du groupe de fichiers.

READ_ONLY | READONLY Spécifie que le groupe de fichiers est en lecture seule. La mise à jour des objets n'est pas autorisée. Le groupe de fichiers principal ne peut pas être en mode lecture seule. Pour modifier cet état, vous devez bénéficier d'un accès exclusif à la base de données. Pour plus d'informations, consultez la clause SINGLE_USER.

Étant donné qu'une base de données en lecture seule interdit la modification des données :

- La récupération automatique est ignorée au démarrage du système.
- Le compactage de la base de données est impossible.
- Tout verrouillage est impossible dans les bases de données en lecture seule, ce qui améliore les performances des requêtes.

> [!NOTE]
> Le mot clé READONLY sera supprimé dans une version ultérieure de [!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Évitez d'utiliser READONLY dans le développement de nouvelles applications et prévoyez de modifier celles qui utilisent actuellement READONLY. Utilisez à la place READ_ONLY.

READ_WRITE | READWRITE Spécifie que le groupe est en lecture/écriture. Les objets du groupe de fichiers peuvent être mis à jour. Pour modifier cet état, vous devez bénéficier d'un accès exclusif à la base de données. Pour plus d'informations, consultez la clause SINGLE_USER.

> [!NOTE]
> Le mot clé `READWRITE` sera supprimé dans une version ultérieure de [!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Évitez d’utiliser `READWRITE` dans les nouveaux travaux de développement et prévoyez de modifier les applications qui utilisent `READWRITE` actuellement pour qu’elles utilisent `READ_WRITE` à la place.

Vous pouvez déterminer l’état de ces options en consultant la colonne **is_read_only** de la vue de catalogue **sys.databases** ou la propriété **Updateability** de la fonction `DATABASEPROPERTYEX`.

## <a name="remarks"></a>Notes

Pour diminuer la taille d'une base de données, utilisez [DBCC SHRINKDATABASE](../../t-sql/database-console-commands/dbcc-shrinkdatabase-transact-sql.md).

Vous ne pouvez pas ajouter ou supprimer de fichier quand une instruction `BACKUP` est en cours d’exécution.

Un maximum de 32 767 fichiers et 32 767 groupes de fichiers peut être spécifié pour chaque base de données.

## <a name="examples"></a>Exemples

### <a name="a-adding-a-file-to-a-database"></a>R. Ajout d'un fichier à une base de données

L'exemple suivant ajoute un fichier de données de 5 Mo à la base de données [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)].

```sql
USE master;
GO
ALTER DATABASE AdventureWorks2012
ADD FILE
(
  NAME = Test1dat2,
    SIZE = 5MB,
    MAXSIZE = 100MB,
    FILEGROWTH = 5MB
);
GO
```

### <a name="b-adding-a-filegroup-with-two-files-to-a-database"></a>B. Ajout d'un groupe de deux fichiers à une base de données

L'exemple suivant crée le groupe de fichiers `Test1FG1` dans la base de données [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)] et ajoute deux fichiers de 5 Mo au groupe de fichiers.

```sql
USE master
GO
ALTER DATABASE AdventureWorks2012
ADD FILEGROUP Test1FG1;
GO
ALTER DATABASE AdventureWorks2012
ADD FILE
(
    NAME = test1dat3,
    SIZE = 5MB,
    MAXSIZE = 100MB,
    FILEGROWTH = 5MB
),
(
    NAME = test1dat4,
    SIZE = 5MB,
    MAXSIZE = 100MB,
    FILEGROWTH = 5MB
)  
TO FILEGROUP Test1FG1;
GO
```

### <a name="c-removing-a-file-from-a-database"></a>C. Suppression d'un fichier d'une base de données

L'exemple suivant supprime l'un des fichiers ajoutés dans l'exemple B.

```sql
USE master;
GO
ALTER DATABASE AdventureWorks2012
REMOVE FILE test1dat4;
GO
```

### <a name="d-modifying-a-file"></a>D. Modification d'un fichier

L’exemple suivant augmente la taille de l’un des fichiers ajoutés dans l’exemple B. L’utilisation d’ALTER DATABASE avec l’option MODIFY FILE permet uniquement d’augmenter la taille de fichier. Si vous avez besoin de réduire la taille de fichier, utilisez DBCC SHRINKFILE.

```sql
USE master;
GO

ALTER DATABASE AdventureWorks2012
MODIFY FILE
(NAME = test1dat3,
SIZE = 200MB);
GO
```

Cet exemple réduit la taille d’un fichier de données à 100 Mo, puis spécifie une taille de fichier.

```sql
USE AdventureWorks2012;
GO

DBCC SHRINKFILE (AdventureWorks2012_data, 100);
GO

USE master;
GO

ALTER DATABASE AdventureWorks2012
MODIFY FILE
(NAME = test1dat3,
SIZE = 200MB);
GO
```

### <a name="e-making-a-filegroup-the-default"></a>E. Configuration d'un groupe de fichiers comme groupe par défaut

L’exemple suivant configure le groupe de fichiers `Test1FG1` créé dans l’exemple B comme groupe de fichiers par défaut. Ensuite, le groupe de fichiers par défaut est reconfiguré en groupe de fichiers `PRIMARY`. Notez que `PRIMARY` doit être délimité par des crochets ou des guillemets.

```sql
USE master;
GO
ALTER DATABASE AdventureWorks2012
MODIFY FILEGROUP Test1FG1 DEFAULT;
GO
ALTER DATABASE AdventureWorks2012
MODIFY FILEGROUP [PRIMARY] DEFAULT;
GO
```

### <a name="f-adding-a-filegroup-using-alter-database"></a>F. Ajout d'un groupe de fichiers à l'aide d'ALTER DATABASE

L’exemple suivant ajoute un `FILEGROUP` à la base de données `MyDB`.

```sql
--Create and add a FILEGROUP
ALTER DATABASE MyDB
ADD FILEGROUP NewFG;
GO

--Add a file to FILEGROUP
ALTER DATABASE MyDB
ADD FILE
(
    NAME= 'MyFile',
)
TO FILEGROUP NewFG;
GO
```

### <a name="g-change-filegroup-so-that-when-a-file-in-the-filegroup-meets-the-autogrow-threshold-all-files-in-the-filegroup-grow"></a>G. Changement d’un groupe de fichiers pour que la taille de tous les fichiers augmente quand un fichier du groupe de fichiers atteint le seuil de croissance automatique

L’exemple suivant génère les instructions `ALTER DATABASE` nécessaires pour modifier les groupes de fichiers en lecture-écriture avec le paramètre `AUTOGROW_ALL_FILES`.

```sql
--Generate ALTER DATABASE ... MODIFY FILEGROUP statements
--so that all read-write filegroups grow at the same time.
SET NOCOUNT ON;

DROP TABLE IF EXISTS #tmpdbs
CREATE TABLE #tmpdbs (id INT IDENTITY(1,1), [dbid] INT, [dbname] sysname, isdone BIT);

DROP TABLE IF EXISTS #tmpfgs
CREATE TABLE #tmpfgs (id INT IDENTITY(1,1), [dbid] INT, [dbname] sysname, fgname sysname, isdone BIT);

INSERT INTO #tmpdbs ([dbid], [dbname], [isdone])
SELECT database_id, name, 0 FROM master.sys.databases (NOLOCK) WHERE is_read_only = 0 AND state = 0;

DECLARE @dbid INT, @query VARCHAR(1000), @dbname sysname, @fgname sysname

WHILE (SELECT COUNT(id) FROM #tmpdbs WHERE isdone = 0) > 0
BEGIN
    SELECT TOP 1 @dbname = [dbname], @dbid = [dbid] FROM #tmpdbs WHERE isdone = 0

    SET @query = 'SELECT ' + CAST(@dbid AS NVARCHAR) + ', ''' + @dbname + ''', [name], 0 FROM [' + @dbname + '].sys.filegroups WHERE [type] = ''FG'' AND is_read_only = 0;'
    INSERT INTO #tmpfgs
    EXEC (@query)

    UPDATE #tmpdbs
    SET isdone = 1
    WHERE [dbid] = @dbid
END;

IF (SELECT COUNT(ID) FROM #tmpfgs) > 0
BEGIN
    WHILE (SELECT COUNT(id) FROM #tmpfgs WHERE isdone = 0) > 0
    BEGIN
        SELECT TOP 1 @dbname = [dbname], @dbid = [dbid], @fgname = fgname FROM #tmpfgs WHERE isdone = 0

        SET @query = 'ALTER DATABASE [' + @dbname + '] MODIFY FILEGROUP [' + @fgname + '] AUTOGROW_ALL_FILES;'

        PRINT @query

        UPDATE #tmpfgs
        SET isdone = 1
        WHERE [dbid] = @dbid AND fgname = @fgname
    END
END;
GO
```

## <a name="see-also"></a>Voir aussi

- [CREATE DATABASE](../../t-sql/statements/create-database-transact-sql.md?view=azuresqldb-mi-current&preserve-view=true)
- [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md)
- [DROP DATABASE](../../t-sql/statements/drop-database-transact-sql.md)
- [sp_spaceused](../../relational-databases/system-stored-procedures/sp-spaceused-transact-sql.md)
- [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md)
- [sys.database_files](../../relational-databases/system-catalog-views/sys-database-files-transact-sql.md)
- [sys.data_spaces](../../relational-databases/system-catalog-views/sys-data-spaces-transact-sql.md)
- [sys.filegroups](../../relational-databases/system-catalog-views/sys-filegroups-transact-sql.md)
- [sys.master_files](../../relational-databases/system-catalog-views/sys-master-files-transact-sql.md)
- [DBCC SHRINKFILE](../../t-sql/database-console-commands/dbcc-shrinkfile-transact-sql.md)
- [Groupe de fichiers à mémoire optimisée](../../relational-databases/in-memory-oltp/the-memory-optimized-filegroup.md)

::: moniker-end
