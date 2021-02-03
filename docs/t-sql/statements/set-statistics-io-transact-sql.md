---
description: SET STATISTICS IO (Transact-SQL)
title: SET STATISTICS IO (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 11/10/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- SET_STATISTICS_IO_TSQL
- IO
- IO_TSQL
- SET STATISTICS IO
dev_langs:
- TSQL
helpviewer_keywords:
- disk I/O statistics [SQL Server]
- I/O [SQL Server], disk activity information
- disks [SQL Server], statement statistics
- STATISTICS IO option
- statements [SQL Server], statistical information
- SET STATISTICS IO statement
- statistical information [SQL Server], disk activity
ms.assetid: 7033aac9-a944-4156-9ff4-6ef65717a28b
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 27084c92d8c3dfcd8b85317d1d2a9d0a70dd713d
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99206940"
---
# <a name="set-statistics-io-transact-sql"></a>SET STATISTICS IO (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Force [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] à afficher des informations sur la quantité d’activité générée sur le disque par les instructions [!INCLUDE[tsql](../../includes/tsql-md.md)].  
  
 ![Icône du lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql
  
SET STATISTICS IO { ON | OFF }  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="remarks"></a>Notes
 Si l’option STATISTICS IO est activée, les informations statistiques sont affichées et, si l’option est désactivée, ces informations ne sont pas affichées.   
  
 Une fois l’option activée, toutes les instructions [!INCLUDE[tsql](../../includes/tsql-md.md)] retournent les informations statistiques jusqu’à ce que l’option soit désactivée.  
  
 Le tableau ci-dessous répertorie et décrit les éléments de sortie.  
  
|Élément de sortie|Signification|  
|-----------------|-------------|  
|**Table**|Nom de la table.|  
|**Nombre d’analyses**|Nombre de recherches ou d’analyses démarrées après avoir atteint le niveau feuille dans n’importe quelle direction afin de récupérer toutes les valeurs pour construire le dataset final de la sortie.<br /><br /> Le nombre d’analyses est 0 si l’index utilisé est un index unique ou un index cluster sur une clé principale et que vous recherchez une seule valeur. Par exemple : `WHERE Primary_Key_Column = <value>`.<br /><br /> Le nombre d’analyses est 1 quand vous recherchez une valeur à l’aide d’un index cluster non unique défini sur une colonne clé non primaire. Ce processus permet de rechercher les valeurs en double de la valeur de clé que vous recherchez. Par exemple : `WHERE Clustered_Index_Key_Column = <value>`.<br /><br /> Le nombre d’analyses est N quand N correspond au nombre de recherches ou d’analyses différentes à gauche ou à droite au niveau feuille après avoir recherché une valeur de clé à l’aide de la clé d’index.|  
|**Lectures logiques**|Nombre de pages lues à partir du cache de données.|  
|**Lectures physiques**|Nombre de pages lues depuis le disque.|  
|**Lectures anticipées**|Nombre de pages placées dans le cache pour la requête.|  
|**Lectures logiques LOB**|Nombre de pages lues à partir du cache de données. Inclut les pages d’index columnstore, **text**, **ntext**, **image**, **varchar(max)**, **nvarchar(max)** et **varbinary(max)**.|  
|**Lectures physiques LOB**|Nombre de pages lues depuis le disque. Inclut les pages d’index columnstore, **text**, **ntext**, **image**, **varchar(max)**, **nvarchar(max)** et **varbinary(max)**.|  
|**Lectures anticipées LOB**|Nombre de pages placées dans le cache pour la requête. Inclut les pages d’index columnstore, **text**, **ntext**, **image**, **varchar(max)**, **nvarchar(max)** et **varbinary(max)**.|

 L'option SET STATISTICS IO est définie lors de l'exécution, et non pas durant l'analyse.

> [!NOTE]  
> Lorsque des instructions Transact-SQL extraient des colonnes LOB, certaines opérations d'extraction peuvent entraîner le parcours de l'arbre LOB à de multiples reprises. L'option SET STATISTICS IO peut alors signaler un nombre de lectures logiques plus élevé que prévu.

## <a name="permissions"></a>Autorisations  
 Pour utiliser SET STATISTICS IO, les utilisateurs doivent disposer des autorisations appropriées pour exécuter l'instruction [!INCLUDE[tsql](../../includes/tsql-md.md)]. L’autorisation SHOWPLAN n’est pas nécessaire.  
  
## <a name="examples"></a>Exemples  
 L'exemple suivant illustre comment de nombreuses lectures logiques et physiques sont utilisées par [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], durant le traitement d'instructions.  
  
```sql
USE AdventureWorks2012;  
GO         
SET STATISTICS IO ON;  
GO  
SELECT *   
FROM Production.ProductCostHistory  
WHERE StandardCost < 500.00;  
GO  
SET STATISTICS IO OFF;  
GO  
```  
  
 Voici l'ensemble de résultats obtenu :  
  
```  
Table 'ProductCostHistory'. Scan count 1, logical reads 5, physical   
reads 0, read-ahead reads 0, lob logical reads 0, lob physical reads 0,   
lob read-ahead reads 0.  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Instructions SET &#40;Transact-SQL&#41;](../../t-sql/statements/set-statements-transact-sql.md)   
 [SET SHOWPLAN_ALL &#40;Transact-SQL&#41;](../../t-sql/statements/set-showplan-all-transact-sql.md)   
 [SET STATISTICS TIME &#40;Transact-SQL&#41;](../../t-sql/statements/set-statistics-time-transact-sql.md)  
  
  
