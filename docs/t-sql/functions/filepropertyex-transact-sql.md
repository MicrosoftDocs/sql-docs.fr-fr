---
description: FILEPROPERTYEX (Transact-SQL)
title: FILEPROPERTYEX (Transact-SQL) | Microsoft Docs
ms.date: 07/23/2019
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- FILEPROPERTYEX_TSQL
- FILEPROPERTYEX
dev_langs:
- TSQL
helpviewer_keywords:
- viewing file properties
- displaying file properties
- file properties [SQL Server]
- FILEPROPERTYEX function
- file names [SQL Server], FILEPROPERTYEX
author: stevestein
ms.author: sstein
ms.openlocfilehash: 7335b9e78a8b0cdce4ca341a8de62254003e2e1c
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99209577"
---
# <a name="filepropertyex-transact-sql"></a>FILEPROPERTYEX (Transact-SQL)
[!INCLUDE[Azure SQL Database Azure SQL Managed Instance](../../includes/applies-to-version/asdb-asdbmi.md)]

  Retourne la valeur de propriétés de fichier étendues spécifiée lorsqu'un nom de fichier dans la base de données active et un nom de propriété sont fournis. Retourne la valeur NULL pour les fichiers qui ne sont pas dans la base de données active ou pour les propriétés de fichier étendues qui n’existent pas. Actuellement, les propriétés de fichier étendues s’appliquent uniquement aux bases de données qui se trouvent dans le stockage blob Azure.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql  
FILEPROPERTYEX ( name , property )  
```  
  
## <a name="arguments"></a>Arguments  
 *name*  
 Expression composée du nom du fichier associé à celui de la base de données actuelle dont les informations de propriété doivent être retournées. *file_name* est de type **nchar(128)**.  
  
 *property*  
 Expression contenant le nom de la propriété de fichier à renvoyer. *property* est de type **varchar(128)** et peut prendre l’une des valeurs suivantes.  


  
|Value|Description|
|-----------|-----------------|  
|**BlobTier**|Le niveau de l’objet blob de pages Azure cible. S’applique uniquement aux bases de données Standard et GeneralPurpose qui utilisent le stockage d’objets blob de pages Azure.|
|**AccountType**|Le type de compte de stockage indiquant s’il s’agit d’un stockage blob ou d’un stockage de fichiers, et s’il s’agit d’un stockage standard ou Premium.|
|**IsInferredTier**|Indique si le niveau est un niveau implicite (déduit) qui peut croître avec la taille des données ou un niveau explicite (fixe).|
|**IsPageBlob**|Indique si le blob cible est un objet blob de pages ou non.|
  
## <a name="return-types"></a>Types de retour  
 **sql_variant**  
  
## <a name="remarks"></a>Notes  
 *file_name* correspond à la colonne **name** de la vue de catalogue **sys.master_files** ou **sys.database_files**.  
  
## <a name="examples"></a>Exemples  
 L'exemple suivant renvoie le paramètre pour les fichiers de base données :
```sql
SELECT s.file_id,
       s.type_desc,
       s.name,
       FILEPROPERTYEX(s.name, 'BlobTier') AS BlobTier,
       FILEPROPERTYEX(s.name, 'AccountType') AS AccountType,
       FILEPROPERTYEX(s.name, 'IsInferredTier') AS IsInferredTier,
       FILEPROPERTYEX(s.name, 'IsPageBlob') AS IsPageBlob
FROM sys.database_files AS s
WHERE s.type_desc IN ('ROWS', 'LOG');
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```
file_id  type_desc  name  BlobTier  AccountType  IsInferredTier  IsPageBlob
--------------------------------------------------------------------------------------
1     ROWS      data_0  P30  PremiumBlobStorage  0   1
2     LOG       log     P30  PremiumBlobStorage  0   1

(2 rows affected)
```  
  
## <a name="see-also"></a>Voir aussi  
 [FILEGROUPPROPERTY &#40;Transact-SQL&#41;](../../t-sql/functions/filegroupproperty-transact-sql.md)   
 [Fonctions de métadonnées &#40;Transact-SQL&#41;](../../t-sql/functions/metadata-functions-transact-sql.md)   
 [sp_spaceused &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-spaceused-transact-sql.md)   
 [sys.database_files &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-database-files-transact-sql.md)   
 [sys.master_files &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-master-files-transact-sql.md)  
  
  
