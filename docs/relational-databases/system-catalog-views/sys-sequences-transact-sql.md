---
description: sys.sequences (Transact-SQL)
title: sys. sequences (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.sequences_TSQL
- sys.sequences
- sequences
- sequences_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sequence number object, sys. sequences catalog view
- sys.sequences catalog view
ms.assetid: 0e1b0e32-1cce-40f7-83c8-860ec660138a
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: fabf7eae6ee156918305e4e84f5ef549b7d67364
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99182458"
---
# <a name="syssequences-transact-sql"></a>sys.sequences (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Contient une ligne pour chaque objet séquence dans une base de données.  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|\<inherited columns>||Hérite de toutes les colonnes de [sys. Objects](../../relational-databases/system-catalog-views/sys-objects-transact-sql.md).|  
|**start_value**|**sql_variant pas NULL**|Valeur de départ pour l'objet séquence. Si l'objet séquence est redémarré à l'aide d'ALTER SEQUENCE, il redémarrera à cette valeur. Lorsque l’objet séquence se poursuit, il passe à la **minimum_value** ou **maximum_value**, et non à la **start_value**.|  
|**increment**|**sql_variant pas NULL**|Valeur utilisée pour incrémenter l'objet séquence après chaque valeur générée.|  
|**minimum_value**|**sql_variant NULL**|Valeur minimale qui peut être générée par l'objet séquence. Une fois cette valeur atteinte, l’objet séquence retourne une erreur lors de la tentative de génération d’autres valeurs ou le redémarrage si l’option CYCLE est spécifiée. Si aucun MINVALUE n’a été spécifié, cette colonne retourne la valeur minimale prise en charge par le type de données du générateur de séquence.|  
|**maximum_value**|**sql_variant NULL**|Valeur maximale qui peut être générée par l'objet séquence. Une fois cette valeur atteinte, l'objet séquence retourne une erreur lors de la tentative de génération de plus de valeurs ou redémarre si l'option CYCLE est spécifiée. Si aucun MAXVALUE n'a été spécifié, cette colonne retourne la valeur maximale prise en charge par le type de données de l'objet séquence.|  
|**is_cycling**|**bit NOT NULL**|Retourne 0 si NO CYCLE a été spécifié pour l'objet séquence et 1 si CYCLE a été spécifié.|  
|**is_cached**|**bit NOT NULL**|Retourne 0 si NO CACHE a été spécifié pour l'objet séquence et 1 si CACHE a été spécifié.|  
|**cache_size**|**int NULL**|Retourne la taille de cache spécifiée pour l'objet séquence. Cette colonne contient NULL si la séquence a été créée avec l'option NO CACHE ou si CACHE a été spécifié sans indication de taille de cache. Si la valeur spécifiée par la taille du cache est supérieure au nombre maximal de valeurs qui peuvent être retournées par l'objet séquence, cette taille du cache impossible à obtenir est toujours affichée.|  
|**system_type_id**|**tinyint non NULL**|ID du type de système pour le type de données de l’objet séquence.|  
|**user_type_id**|**int NOT NULL**|ID du type de données pour l'objet séquence, comme défini par l'utilisateur.|  
|**precision**|**tinyint non NULL**|Précision maximale du type de données.|  
|**scale**|**tinyint non NULL**|Échelle maximale du type. L'échelle est retournée avec la précision afin de donner des métadonnées complètes aux utilisateurs. L'échelle est toujours de 0 pour les objets séquences car seuls les types entiers sont autorisés.|  
|**current_value**|**sql_variant pas NULL**|La dernière valeur engagée. Autrement dit, la valeur retournée par l’exécution la plus récente de la fonction NEXT VALUE FOR ou de la dernière valeur issue de l’exécution de la procédure **sp_sequence_get_range** . Retourne la valeur START WITH si la séquence n'a jamais été utilisée.|  
|**is_exhausted**|**bit NOT NULL**|0 indique que davantage de valeurs peuvent être générées à partir de la séquence. 1 indique que l'objet séquence a atteint le paramètre MAXVALUE et que la séquence n'est pas définie sur CYCLE. La fonction NEXT VALUE FOR retourne une erreur jusqu'à ce que la séquence soit redémarrée à l'aide d'ALTER SEQUENCE.|  
|**last_used_value**|**sql_variant NULL**|Retourne la dernière valeur générée par la fonction [Next value for](../../t-sql/functions/next-value-for-transact-sql.md) . S’applique à SQL Server 2017 et versions ultérieures.|  
  
## <a name="permissions"></a>Autorisations  
 Dans [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] et les versions ultérieures, la visibilité des métadonnées dans les affichages catalogue est limitée aux éléments sécurisables qu'un utilisateur détient ou pour lesquels des autorisations lui ont été accordées. Pour plus d'informations, consultez [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="see-also"></a>Voir aussi  
 [Numéros séquentiels](../../relational-databases/sequence-numbers/sequence-numbers.md)   
 [CREATE SEQUENCE &#40;Transact-SQL&#41;](../../t-sql/statements/create-sequence-transact-sql.md)   
 [ALTER SEQUENCE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-sequence-transact-sql.md)   
 [DROP SEQUENCE &#40;Transact-SQL&#41;](../../t-sql/statements/drop-sequence-transact-sql.md)   
 [NEXT VALUE FOR &#40;Transact-SQL&#41;](../../t-sql/functions/next-value-for-transact-sql.md)   
 [sp_sequence_get_range &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-sequence-get-range-transact-sql.md)  
  
  
