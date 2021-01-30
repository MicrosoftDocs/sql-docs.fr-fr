---
description: sp_pkeys (Transact-SQL)
title: sp_pkeys (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_pkeys
- sp_pkeys_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_pkeys
ms.assetid: e614c75d-847b-4726-8f6f-cd18de688eda
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: bfb9d942ae25c8bf690e1ed9740f56c021cf413d
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99190030"
---
# <a name="sp_pkeys-transact-sql"></a>sp_pkeys (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Renvoie des informations de clé primaire pour une table de l'environnement actif.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql  
-- Syntax for SQL Server, Azure SQL Database, Azure Synapse Analytics, Parallel Data Warehouse  
  
sp_pkeys [ @table_name = ] 'name'       
    [ , [ @table_owner = ] 'owner' ]   
    [ , [ @table_qualifier = ] 'qualifier' ]  
```  
  
## <a name="arguments"></a>Arguments  
 [ @table_name =] '*nom*'  
 Table pour laquelle les informations doivent être retournées. *Name* est de **type sysname**, sans valeur par défaut. La recherche de correspondance avec des caractères génériques n'est pas prise en charge.  
  
 [ @table_owner =] '*propriétaire*'  
 Spécifie le propriétaire de la table désignée. *owner* est de **type sysname**, avec NULL comme valeur par défaut. La recherche de correspondance avec des caractères génériques n'est pas prise en charge. Si *owner* n’est pas spécifié, les règles de visibilité de table par défaut du SGBD sous-jacent s’appliquent.  
  
 Dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], si l'utilisateur actuel est propriétaire d'une table portant le nom spécifié, les colonnes de cette table sont renvoyées. Si le *propriétaire* n’est pas spécifié et que l’utilisateur actuel ne possède pas de table portant le *nom* spécifié, cette procédure recherche une table portant le *nom* spécifié, détenue par le propriétaire de la base de données. Si la recherche de la table aboutit, ce sont les colonnes de cette dernière qui sont retournées.  
  
 [ @table_qualifier =] '*qualificateur*'  
 Qualificateur de la table. *qualifier* est de **type sysname**, avec NULL comme valeur par défaut. Divers produits SGBD prennent en charge les noms de tables en trois parties (_qualificateur_**.** _propriétaire_**.** _nom_). Dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , cette colonne représente le nom de la base de données. Dans certains produits, elle représente le nom du serveur de l'environnement de base de données de la table.  
  
## <a name="return-code-values"></a>Codet de retour  
 Aucun  
  
## <a name="result-sets"></a>Jeux de résultats  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|TABLE_QUALIFIER|**sysname**|Nom du qualificateur de la table. Ce champ peut contenir la valeur NULL.|  
|TABLE_OWNER|**sysname**|Nom du propriétaire de la table. Ce champ retourne toujours une valeur.|  
|TABLE_NAME|**sysname**|Nom de la table. Dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], cette colonne représente le nom de la table tel qu'il figure dans la table sysobjects. Ce champ retourne toujours une valeur.|  
|COLUMN_NAME|**sysname**|Nom de la colonne, pour chaque colonne renvoyée de la table TABLE_NAME. Dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], cette colonne représente le nom de la colonne tel qu'il figure dans la table sys.columns. Ce champ retourne toujours une valeur.|  
|KEY_SEQ|**smallint**|Numéro de séquence de la colonne dans une clé primaire multicolonne.|  
|PK_NAME|**sysname**|Identificateur de clé primaire. Retourne NULL s'il n'est pas applicable à la source de données.|  
  
## <a name="remarks"></a>Notes  
 sp_pkeys renvoie des informations sur les colonnes explicitement définies avec une contrainte PRIMARY KEY. Comme tous les systèmes ne prennent en charge pas les clés primaires explicitement nommées, la personne chargée de la mise en œuvre des passerelles détermine ce qui constitue une clé primaire. Notez que le terme « clé primaire » fait référence à une clé primaire logique pour une table. À chaque clé indiquée comme étant une clé primaire logique doit correspondre un seul index défini sur cette clé. Cet index unique est également renvoyé dans sp_statistics.  
  
 La procédure stockée sp_pkeys est équivalente à SQLPrimaryKeys dans ODBC. Les résultats renvoyés sont triés par TABLE_QUALIFIER, TABLE_OWNER, TABLE_NAME et KEY_SEQ.  
  
## <a name="permissions"></a>Autorisations  
 Nécessite l'autorisation SELECT sur le schéma.  
  
## <a name="examples"></a>Exemples  
 L'exemple suivant extrait la clé primaire de la table `HumanResources.Department` dans la base de données `AdventureWorks2012`.  
  
```sql  
USE AdventureWorks2012;  
GO  
EXEC sp_pkeys @table_name = N'Department'  
    ,@table_owner = N'HumanResources';  
```  
  
## <a name="examples-sssdwfull-and-sspdw"></a>Exemples : [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] et [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
 L'exemple suivant extrait la clé primaire de la table `DimAccount` dans la base de données `AdventureWorksPDW2012`. Elle ne retourne aucune ligne indiquant que la table n’a pas de clé primaire.  
  
```sql  
-- Uses AdventureWorks  
  
EXEC sp_pkeys @table_name = N'DimAccount';  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Procédures stockées de catalogue &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/catalog-stored-procedures-transact-sql.md)   
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  

