---
description: sp_datatype_info_90 (Azure Synapse Analytics)
title: sp_datatype_info_90 (Azure Synapse Analytics)
ms.custom: ''
ms.date: 03/13/2017
ms.service: sql-data-warehouse
ms.reviewer: ''
ms.topic: reference
dev_langs:
- TSQL
ms.assetid: 1d043964-dc6e-4c3e-ab61-bc444d5e25ae
author: ronortloff
ms.author: rortloff
monikerRange: '>= aps-pdw-2016 || = azure-sqldw-latest'
ms.openlocfilehash: 2d3f79c55a1e48fd3f7b1d18671e1d710658f345
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99201327"
---
# <a name="sp_datatype_info_90-azure-synapse-analytics"></a>sp_datatype_info_90 (Azure Synapse Analytics)
[!INCLUDE[applies-to-version/asa-pdw](../../includes/applies-to-version/asa-pdw.md)]

  Retourne des informations sur les types de données pris en charge par l'environnement actuel.  
  
 ![Icône du lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL &#40;Transact-SQL&#41;](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql  
-- Syntax for Azure Synapse Analytics and Parallel Data Warehouse  
  
sp_datatype_info_90 [ [ @data_type = ] data_type ]   
     [ , [ @ODBCVer = ] odbc_version ]   
```  

[!INCLUDE[synapse-analytics-od-unsupported-syntax](../../includes/synapse-analytics-od-unsupported-syntax.md)]
  
## <a name="arguments"></a>Arguments  
`[ @data_type = ] data_type` Numéro de code pour le type de données spécifié. Pour obtenir une liste de tous les types de données, omettez ce paramètre. *data_type* est de **type int**, avec 0 comme valeur par défaut.  
  
`[ @ODBCVer = ] odbc_version` Version d’ODBC utilisée. *odbc_version* est de **type tinyint**, avec 2 comme valeur par défaut.  
  
## <a name="return-code-values"></a>Codet de retour  
 Aucun  
  
## <a name="result-sets"></a>Jeux de résultats  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|TYPE_NAME|**sysname**|Type de données dépendant du SGBD (système de gestion de base de données)|  
|DATA_TYPE|**smallint**|Code pour le type ODBC sur lequel toutes les colonnes de ce type sont mappées.|  
|PRECISION|**int**|Précision maximale du type de données de la source de données. La valeur NULL est retournée pour les types de données pour lesquels la précision n'est pas applicable. La valeur de retour pour la colonne PRECISION est en base 10.|  
|LITERAL_PREFIX|**varchar (** 32 **)**|Caractères utilisés devant une constante. Par exemple, un guillemet simple (**'**) pour les types de caractères et 0x pour binaire.|  
|LITERAL_SUFFIX|**varchar (** 32 **)**|Caractères utilisés pour terminer une constante. Par exemple, un guillemet simple (**'**) pour les types de caractères et sans guillemets pour le binaire.|  
|CREATE_PARAMS|**varchar (** 32 **)**|Description des paramètres de création de ce type de données. Par exemple, **Decimal** est « Precision, Scale », **float** est null et **varchar** est « max_length ».|  
|NULLABLE|**smallint**|Spécifie la possibilité de contenir une valeur NULL.<br /><br /> 1 = Autorise les valeurs NULL<br /><br /> 0 = N'autorise pas les valeurs NULL|  
|CASE_SENSITIVE|**smallint**|Spécifie le respect de la casse.<br /><br /> 1 = Toutes les colonnes de ce type respectent la casse (pour les classements).<br /><br /> 0 = Toutes les colonnes de ce type ne respectent pas la casse.|  
|POSSIBILITÉ DE RECHERCHE|**smallint**|Spécifie la capacité de recherche du type de colonne :<br /><br /> 1 = Recherche impossible.<br /><br /> 2 = Recherche possible avec LIKE.<br /><br /> 3 = Recherche possible avec WHERE.<br /><br /> 4 = Recherche possible avec WHERE ou LIKE.|  
|UNSIGNED_ATTRIBUTE|**smallint**|Spécifie la signature du type de données.<br /><br /> 1 = Type de données non signé.<br /><br /> 0 = Type de données signé.|  
|MONEY|**smallint**|Spécifie le type de données **Money** .<br /><br /> 1 = type de données **Money** .<br /><br /> 0 = n’est pas un type de données **Money** .|  
|AUTO_INCREMENT|**smallint**|Spécifie l'auto-incrémentation.<br /><br /> 1 = Auto-incrémentation<br /><br /> 0 = Pas d'auto-incrémentation<br /><br /> NULL = Attribut non applicable<br /><br /> Une application peut insérer des valeurs dans une colonne possédant cet attribut, mais elle ne peut pas mettre à jour les valeurs dans la colonne. À l’exception du type de données **bit** , AUTO_INCREMENT est valide uniquement pour les types de données qui appartiennent aux catégories de types de données numériques exactes et approximatives.|  
|LOCAL_TYPE_NAME|**sysname**|Version localisée du nom de type de données dépendant de la source de données. Par exemple, DECIMAL est DECIMALE en français. La valeur NULL est retournée si un nom localisé n'est pas pris en charge par la source de données.|  
|MINIMUM_SCALE|**smallint**|Échelle minimale du type de données de la source de données. Si un type de données possède une échelle fixe, les colonnes MINIMUM_SCALE et MAXIMUM_SCALE contiennent toutes les deux cette valeur. La valeur NULL est retournée lorsque l'échelle n'est pas applicable.|  
|MAXIMUM_SCALE|**smallint**|Échelle maximale du type de données de la source de données. Si l'échelle maximale n'est pas définie séparément dans la source de données, mais si au contraire elle est définie comme étant identique à la précision maximale, cette colonne contient la même valeur que la colonne PRECISION.|  
|SQL_DATA_TYPE|**smallint**|Valeur du type de données SQL tel qu'il apparaît dans le champ TYPE du descripteur. Cette colonne est la même que la colonne DATA_TYPE, à l’exception des types de données **DateTime** et ANSI **Interval** . Ce champ retourne toujours une valeur.|  
|SQL_DATETIME_SUB|**smallint**|sous-code **DateTime** ou ANSI **Interval** si la valeur de SQL_DATA_TYPE est SQL_DATETIME ou SQL_INTERVAL. Pour les types de données autres que **DateTime** et **Interval** ANSI, ce champ a la valeur null.|  
|NUM_PREC_RADIX|**int**|Nombre de bits ou de chiffres pour le calcul du nombre maximal qu’une colonne peut contenir. Si le type de données est un type de données numérique approximatif, cette colonne contient la valeur 2 pour indiquer plusieurs bits. Pour les types numériques exacts, cette colonne contient la valeur 10 pour indiquer plusieurs chiffres décimaux. Sinon, cette colonne est NULL. En combinant la précision et la base, l'application peut calculer le nombre maximal que la colonne peut contenir.|  
|INTERVAL_PRECISION|**smallint**|Valeur de la précision de début de l’intervalle si *data_type* est **Interval**; Sinon, NULL.|  
|USERTYPE|**smallint**|valeur **usertype** de la table systypes.|  
  
## <a name="remarks"></a>Notes  
 sp_datatype_info équivaut à SQLGetTypeInfo dans ODBC. Les résultats retournés sont triés par DATA_TYPE, puis en fonction du niveau de précision de la concordance entre le type de données et le type de données ODBC SQL correspondant.  
  
## <a name="permissions"></a>Autorisations  
 Nécessite l'appartenance au rôle public.  
  
## <a name="examples-sssdwfull-and-sspdw"></a>Exemples : [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] et [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
 L’exemple suivant récupère des informations pour les types de données **sysname** et **nvarchar** en spécifiant le *data_type* valeur de `-9` .  
  
```sql  
USE master;  
GO  
EXEC sp_datatype_info_90 -9;  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Procédures stockées Azure Synapse Analytics](../../relational-databases/system-stored-procedures/sql-data-warehouse-stored-procedures.md)   
 [Types de données &#40;Transact-SQL&#41;](../../t-sql/data-types/data-types-transact-sql.md)  
  
  

