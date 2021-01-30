---
description: SCHEMATA (Transact-SQL)
title: SCHEMATA (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 09/08/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- SCHEMATA_TSQL
- SCHEMATA
dev_langs:
- TSQL
helpviewer_keywords:
- INFORMATION_SCHEMA.SCHEMATA view
- SCHEMATA view
ms.assetid: 69617642-0f54-4b25-b62f-5f39c8909601
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: cbb311366b6d2d036d44570761a7d198450b37a7
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99185514"
---
# <a name="schemata-transact-sql"></a>SCHEMATA (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Renvoie une ligne pour chaque schéma de la base de données active. Pour récupérer des informations de ces vues, spécifiez le nom complet de **INFORMATION_SCHEMA.** _view_name_. Pour récupérer des informations sur toutes les bases de données d’une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , interrogez la vue de catalogue [sys. databases &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md) .  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**CATALOG_NAME**|**sysname**|Nom de la base de données active|  
|**SCHEMA_NAME**|**nvarchar (** 128 **)**|Renvoie le nom du schéma|  
|**SCHEMA_OWNER**|**nvarchar (** 128 **)**|Nom du propriétaire du schéma<br /><br /> **&#42;&#42;  &#42;&#42;importante** N’utilisez pas de vues de INFORMATION_SCHEMA pour déterminer le schéma d’un objet. Les vues de INFORMATION_SCHEMA représentent seulement un sous-ensemble des métadonnées d’un objet. La seule méthode fiable pour rechercher le schéma d'un objet est d'interroger l'affichage catalogue sys.objects.|  
|**DEFAULT_CHARACTER_SET_CATALOG**|**varchar (** 6 **)**|Retourne toujours la valeur Null.|  
|**DEFAULT_CHARACTER_SET_SCHEMA**|**varchar (** 3 **)**|Retourne toujours la valeur Null.|  
|**DEFAULT_CHARACTER_SET_NAME**|**sysname**|Renvoie le nom du jeu de caractères par défaut|  

**Exemple**  
L’exemple suivant retourne des informations sur les schémas de la base de données Master :  
```sql  
SELECT * FROM master.INFORMATION_SCHEMA.SCHEMATA;
```  

## <a name="see-also"></a>Voir aussi  
 [Vues système &#40;&#41;Transact-SQL ](../../t-sql/language-reference.md)   
 [Vues de schémas d’informations &#40;Transact-SQL&#41;](~/relational-databases/system-information-schema-views/system-information-schema-views-transact-sql.md)   
 [sys.databases &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md)   
 [sys.schemas &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/schemas-catalog-views-sys-schemas.md)   
 [ Jeux desys.sys&#40;Transact-SQL&#41;](../../relational-databases/system-compatibility-views/sys-syscharsets-transact-sql.md)  
  
