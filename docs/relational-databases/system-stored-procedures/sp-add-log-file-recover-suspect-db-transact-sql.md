---
description: sp_add_log_file_recover_suspect_db (Transact-SQL)
title: sp_add_log_file_recover_suspect_db (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_add_log_file_recover_suspect_db_TSQL
- sp_add_log_file_recover_suspect_db
dev_langs:
- TSQL
helpviewer_keywords:
- sp_add_log_file_recover_suspect_db
ms.assetid: b41ca3a5-7222-4c22-a012-e66a577a82f6
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: ecde40e66d5754638872a12c79ca580b0b5f2982
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99206734"
---
# <a name="sp_add_log_file_recover_suspect_db-transact-sql"></a>sp_add_log_file_recover_suspect_db (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Ajoute un fichier journal à un groupe de fichiers lorsque la récupération d'une base de données ne peut se terminer en raison d'une erreur 9002 (espace journal insuffisant). Une fois le fichier ajouté, **sp_add_log_file_recover_suspect_db** désactive le paramètre suspect et termine la récupération de la base de données. Les paramètres sont les mêmes que ceux de ALTER DATABASE *database_name* Add log file.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_add_log_file_recover_suspect_db [ @dbName= ] 'database' ,   
    [ @name = ] 'logical_file_name' ,   
    [ @filename= ] 'os_file_name' ,   
    [ @size = ] 'size' ,   
    [ @maxsize = ] 'max_size' ,   
    [ @filegrowth = ] 'growth_increment'  
```  
  
## <a name="arguments"></a>Arguments  
`[ @dbName = ] 'database'` Nom de la base de données. *Database est de* **type sysname**, sans valeur par défaut.  
  
`[ @name = ] 'logical_file_name'` Nom utilisé dans le lors du [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] référencement du fichier. Il doit être unique dans le serveur. *logical_file_name* est de type **nvarchar (260)**, sans valeur par défaut.  
  
`[ @filename = ] 'os_file_name'` Est le chemin d’accès et le nom de fichier utilisés par le système d’exploitation pour le fichier. Le fichier doit résider dans le serveur hébergeant [!INCLUDE[ssDE](../../includes/ssde-md.md)]. *os_file_name* est de type **nvarchar (260)**, sans valeur par défaut.  
  
`[ @size = ] 'size_ '` Taille initiale du fichier. *Size* est de type **nvarchar (20)**, avec NULL comme valeur par défaut. Indiquez un nombre entier sans aucune décimale. Les indications Mo et Ko peuvent être utilisées pour indiquer qu'il s'agit de mégaoctets ou de kilo-octets. La valeur par défaut est Mo. La valeur minimale est 512 Ko. Si la *taille* n’est pas spécifiée, la valeur par défaut est 1 Mo.  
  
`[ @maxsize = ] 'max_size_ '` Taille maximale que peut atteindre le fichier. *max_size* est de type **nvarchar (20)**, avec NULL comme valeur par défaut. Indiquez un nombre entier sans aucune décimale. Les indications Mo et Ko peuvent être utilisées pour indiquer qu'il s'agit de mégaoctets ou de kilo-octets. La valeur par défaut est Mo.  
  
 Si *max_size* n’est pas spécifié, le fichier s’agrandit jusqu’à ce que le disque soit plein. Le journal des applications [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows avertit l'administrateur lorsqu'un disque est sur le point d'être saturé.  
  
`[ @filegrowth = ] 'growth_increment_ '` Quantité d’espace ajoutée au fichier chaque fois que de l’espace est nécessaire. *growth_increment* est de type **nvarchar (20)**, avec NULL comme valeur par défaut. La valeur 0 indique qu'aucun accroissement n'est autorisé. Indiquez un nombre entier sans aucune décimale. La valeur peut être exprimée en Mo, en Ko ou en pourcentage (%). Lorsque % est spécifié, la taille de l'incrément de croissance est le pourcentage spécifié de la taille du fichier au moment où l'incrémentation a lieu. Si un nombre est mentionné sans spécifier Mo, Ko ou %, la valeur par défaut est Mo.  
  
 Si *growth_increment* a la valeur null, la valeur par défaut est 10% et la valeur de taille minimale est de 64 Ko. La taille spécifiée est arrondie à la valeur multiple de 64 Ko la plus proche.  
  
## <a name="return-code-values"></a>Codet de retour  
 0 (réussite) ou 1 (échec)  
  
## <a name="result-sets"></a>Jeux de résultats  
 None  
  
## <a name="permissions"></a>Autorisations  
 Les autorisations d’exécution reviennent par défaut aux membres du rôle serveur fixe **sysadmin** . Ces autorisations ne sont pas transférables.  
  
## <a name="examples"></a>Exemples  
 Dans l'exemple suivant, la base de données `db1` a été déclarée suspecte lors de sa récupération en raison d'un espace journal insuffisant (erreur 9002).  
  
```  
USE master;  
GO  
EXEC sp_add_log_file_recover_suspect_db db1, logfile2,  
'C:\Program Files\Microsoft SQL  
    Server\MSSQL13.MSSQLSERVER\MSSQL\Data\db1_logfile2.ldf',   
    '1MB';  
```  
  
## <a name="see-also"></a>Voir aussi  
 [ALTER DATABASE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql.md)   
 [sp_add_data_file_recover_suspect_db &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-add-data-file-recover-suspect-db-transact-sql.md)   
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
