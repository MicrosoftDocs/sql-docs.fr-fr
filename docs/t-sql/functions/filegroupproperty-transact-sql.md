---
description: FILEGROUPPROPERTY (Transact-SQL)
title: FILEGROUPPROPERTY (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- FILEGROUPPROPERTY_TSQL
- FILEGROUPPROPERTY
dev_langs:
- TSQL
helpviewer_keywords:
- filegroups [SQL Server], property values
- FILEGROUPPROPERTY function
- viewing filegroup properties
- displaying filegroup properties
ms.assetid: b3a930e6-df05-4034-929c-f681f5f6fc6e
author: cawrites
ms.author: chadam
ms.openlocfilehash: 05e92cdfb95110701fd41d4d02462b35477d8c61
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/11/2021
ms.locfileid: "98087790"
---
# <a name="filegroupproperty-transact-sql"></a>FILEGROUPPROPERTY (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

Cette fonction retourne la valeur de propriété du groupe de fichiers pour un nom spécifié et une valeur de groupe de fichiers.  
  
 ![Icône du lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql  
FILEGROUPPROPERTY ( filegroup_name, property )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments
 *filegroup_name*  
Expression de type **sysname** représentant le nom du groupe de fichiers pour lequel `FILEGROUPPROPERTY` retourne les informations sur la propriété nommée.  
  
 *property*  
Expression de type **varchar(128)** qui retourne le nom de la propriété du groupe de fichiers. *property* peut retourner l’une des valeurs suivantes :  
  
|Valeur|Description|Valeur retournée|  
|-----------|-----------------|--------------------|  
|**IsReadOnly**|Groupe de fichiers en lecture seule.|1 = TRUE<br /><br /> 0 = FALSE<br /><br /> NULL = entrée non valide.|  
|**IsUserDefinedFG**|Groupe de fichiers défini par l'utilisateur.|1 = TRUE<br /><br /> 0 = FALSE<br /><br /> NULL = entrée non valide.|  
|**IsDefault**|Groupe de fichiers par défaut.|1 = TRUE<br /><br /> 0 = FALSE<br /><br /> NULL = entrée non valide.|  
  
## <a name="return-types"></a>Types de retour  
**int**  
  
## <a name="remarks"></a>Notes  
*filegroup_name* correspond à la colonne **name** de la vue de catalogue **sys.filegroups**.  
  
## <a name="examples"></a>Exemples  
Cet exemple retourne le paramètre de la propriété `IsDefault` relatif au groupe de fichiers principal de la base de données [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)].  
  
```sql  
SELECT FILEGROUPPROPERTY('PRIMARY', 'IsDefault') AS 'Default Filegroup';  
GO  
```  

 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]   
```  
Default Filegroup   
---------------------   
1  
  
(1 row(s) affected)  
```  
  
## <a name="see-also"></a>Voir aussi  
 [FILEGROUP_ID &#40;Transact-SQL&#41;](../../t-sql/functions/filegroup-id-transact-sql.md)   
 [FILEGROUP_NAME &#40;Transact-SQL&#41;](../../t-sql/functions/filegroup-name-transact-sql.md)   
 [Fonctions de métadonnées &#40;Transact-SQL&#41;](../../t-sql/functions/metadata-functions-transact-sql.md)   
 [SELECT &#40;Transact-SQL&#41;](../../t-sql/queries/select-transact-sql.md)   
 [sys.filegroups &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-filegroups-transact-sql.md)  
  
  
