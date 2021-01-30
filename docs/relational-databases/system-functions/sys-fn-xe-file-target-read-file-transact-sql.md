---
description: sys.fn_xe_file_target_read_file (Transact-SQL)
title: sys.fn_xe_file_target_read_file (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/22/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- fn_xe_file_target_read_file_TSQL
- fn_xe_file_target_read_file
- sys.fn_xe_file_target_read_file_TSQL
- sys.fn_xe_file_target_read_file
dev_langs:
- TSQL
helpviewer_keywords:
- extended events [SQL Server], functions
- fn_xe_file_target_read_file function
- sys.fn_xe_file_target_read_file function
ms.assetid: cc0351ae-4882-4b67-b0d8-bd235d20c901
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: a592609cce6805105e61679e54857312e6296b6f
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99187372"
---
# <a name="sysfn_xe_file_target_read_file-transact-sql"></a>sys.fn_xe_file_target_read_file (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Lit des fichiers créés par la cible de fichier asynchrone d'événements étendus. Au format XML, un événement par ligne est retourné.  
  
> [!WARNING]  
>  [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] et [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)] acceptent les résultats de trace générés au format Xel et Xem. [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] Les événements étendus prennent uniquement en charge les résultats de trace au format XEL. Nous vous recommandons d'utiliser SQL Server Management Studio pour lire les résultats de trace au format XEL.    
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sys.fn_xe_file_target_read_file ( path, mdpath, initial_file_name, initial_offset )  
```  
  
## <a name="arguments"></a>Arguments  
 *path*  
 Chemin d'accès aux fichiers à lire. le *chemin d’accès* peut contenir des caractères génériques et inclure le nom d’un fichier. *path* est **de type nvarchar (260)**. Il n'y a pas de valeur par défaut. Dans le contexte de Azure SQL Database, cette valeur est une URL HTTP vers un fichier dans le stockage Azure.
  
 *mdpath*  
 Chemin d’accès au fichier de métadonnées qui correspond aux fichiers spécifiés par l’argument *path* . *mdpath* est **de type nvarchar (260)**. Il n'y a pas de valeur par défaut. À compter de SQL Server 2016, ce paramètre peut être spécifié comme null.
  
> [!NOTE]  
>  [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] ne nécessite pas le paramètre *mdpath* . Toutefois, celui-ci est conservé pour la compatibilité descendante des fichiers journaux générés dans les versions antérieures de SQL Server.  
  
 *initial_file_name*  
 Premier fichier à lire à partir du *chemin d’accès*. *initial_file_name* est **de type nvarchar (260)**. Il n'y a pas de valeur par défaut. Si **null** est spécifié en tant qu’argument, tous les fichiers trouvés dans le *chemin d’accès* sont lus.  
  
> [!NOTE]  
>  *initial_file_name* et *initial_offset* sont des arguments associés. Si vous spécifiez une valeur pour l'un des arguments, vous devez en spécifier une pour l'autre.  
  
 *initial_offset*  
 Utilisé pour spécifier le dernier décalage lu et ignorer tous les événements jusqu'au décalage (inclus). Commence l'énumération d'événements après le décalage spécifié. *initial_offset* est de type **bigint**. Si **null** est spécifié en tant qu’argument, la totalité du fichier est lue.  
  
## <a name="table-returned"></a>Table retournée  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|module_guid|**uniqueidentifier**|GUID du module d'événements. N'accepte pas la valeur NULL.|  
|package_guid|**uniqueidentifier**|GUID du package d'événement. N'accepte pas la valeur NULL.|  
|object_name|**nvarchar (256)**|Nom de l’événement. N'accepte pas la valeur NULL.|  
|event_data|**nvarchar(max)**|Contenu de l'événement, au format XML. N'accepte pas la valeur NULL.|  
|file_name|**nvarchar(260)**|Nom du fichier qui contient l'événement. N'accepte pas la valeur NULL.|  
|file_offset|**bigint**|Offset du bloc dans le fichier qui contient l'événement. N'accepte pas la valeur NULL.|  
|timestamp_utc|**datetime2**|**S’applique à** : [!INCLUDE[ssSQLv14](../../includes/sssqlv14-md.md)] et versions ultérieures et [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].<br /><br />Date et heure (fuseau horaire UTC) de l’événement. N'accepte pas la valeur NULL.|  

  
## <a name="remarks"></a>Notes  
 La lecture de jeux de résultats volumineux en exécutant **sys.fn_xe_file_target_read_file** dans [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] peut entraîner une erreur. Utilisez les **résultats** en mode fichier (**Ctrl + Maj + F**) pour exporter de grands jeux de résultats dans un fichier et lisez le fichier avec un autre outil à la place.  
  
## <a name="permissions"></a>Autorisations  
 requièrent l'autorisation VIEW SERVER STATE sur le serveur.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-retrieving-data-from-file-targets"></a>R. Récupération des données de cibles de fichiers  
 L'exemple ci-dessous obtient toutes les lignes de tous les fichiers. Dans cet exemple, les cibles de fichiers et les métafichiers se trouvent dans le dossier de trace sur le lecteur C:\.  
  
```  
SELECT * FROM sys.fn_xe_file_target_read_file('C:\traces\*.xel', 'C:\traces\metafile.xem', null, null);  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Vues de gestion dynamique des événements étendus](../../relational-databases/system-dynamic-management-views/extended-events-dynamic-management-views.md)   
 [Affichages catalogue des événements étendus &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/extended-events-catalog-views-transact-sql.md)   
 [Événements étendus](../../relational-databases/extended-events/extended-events.md)  
  
  
