---
description: SET ROWCOUNT (Transact-SQL)
title: SET ROWCOUNT (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- SET_ROWCOUNT_TSQL
- ROWCOUNT_TSQL
- SET ROWCOUNT
- ROWCOUNT
dev_langs:
- TSQL
helpviewer_keywords:
- row return limitations [SQL Server]
- SET ROWCOUNT statement
- number of rows affected by statement
- ROWCOUNT option
- counting rows
- stopping queries
- limiting rows returned
- queries [SQL Server], stopping
ms.assetid: c6966fb7-6421-47ef-98f3-82351f2f6bdc
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: eb2c6ccffd4bc62b0f9e43ffba68e1e058496804
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99206987"
---
# <a name="set-rowcount-transact-sql"></a>SET ROWCOUNT (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Demande à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] d'arrêter l'exécution de la requête après avoir renvoyé le nombre de lignes spécifié.  
  
 ![Icône du lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql
SET ROWCOUNT { number | @number_var }   
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments
 *number* | @*number_var*  
 Nombre (entier) de lignes à traiter avant l'arrêt de la requête spécifiée.  
  
## <a name="remarks"></a>Notes  
  
> [!IMPORTANT]  
>  L'utilisation de SET ROWCOUNT n'affectera en rien les instructions DELETE, INSERT et UPDATE dans la version ultérieure de SQL Server. Évitez d’utiliser SET ROWCOUNT avec les instructions DELETE, INSERT et UPDATE dans les nouvelles tâches de développement et pensez à modifier les applications qui l’utilisent actuellement. Pour un comportement semblable, utilisez la syntaxe TOP. Pour plus d’informations, consultez [TOP &#40;Transact-SQL&#41;](../../t-sql/queries/top-transact-sql.md).  
  
 Pour désactiver cette option de manière à renvoyer toutes les lignes, utilisez SET ROWCOUNT 0.  
  
 L'utilisation de l'option SET ROWCOUNT arrête le traitement de la plupart des instructions [!INCLUDE[tsql](../../includes/tsql-md.md)] lorsqu'elles ont atteint le nombre de lignes spécifié, Cela inclut les déclencheurs. L'option ROWCOUNT n'a aucun effet sur les curseurs dynamiques, mais elle limite l'ensemble de lignes de curseurs de type KEYSET et INSENSITIVE. Il convient d'être prudent lors de l'utilisation de cette option.  
  
 SET ROWCOUNT a priorité sur le mot clé TOP d'une instruction SELECT si le compte de lignes a la plus petite valeur.  
  
 L'option SET ROWCOUNT est définie lors de l'exécution, et non pas durant l'analyse.  
  
## <a name="permissions"></a>Autorisations  
 Nécessite l'appartenance au rôle public.  
  
## <a name="examples"></a>Exemples  
 SET ROWCOUNT arrête le traitement après le nombre de lignes spécifié. Dans cet exemple, notez que plus de 500 lignes répondent aux critères de `Quantity` inférieure à `300`. Toutefois, après avoir appliqué SET ROWCOUNT, vous pouvez remarquer que toutes les lignes n'ont pas été retournées.  
  
```sql
USE AdventureWorks2012;  
GO  
SELECT count(*) AS Count  
FROM Production.ProductInventory  
WHERE Quantity < 300;  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 ```
 Count 
 ----------- 
 537 
 
 (1 row(s) affected)
 ```  
  
 Définissez maintenant `ROWCOUNT` sur `4` et retournez toutes les lignes pour démontrer que seules 4 lignes sont retournées.  
  
```sql
SET ROWCOUNT 4;  
SELECT *  
FROM Production.ProductInventory  
WHERE Quantity < 300;  
GO  
  
-- (4 row(s) affected)
```  
  
## <a name="examples-sssdwfull-and-sspdw"></a>Exemples : [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] et [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
 SET ROWCOUNT arrête le traitement après le nombre de lignes spécifié. Dans cet exemple, notez que plus de 20 lignes répondent aux critères de `AccountType = 'Assets'`. Toutefois, après avoir appliqué SET ROWCOUNT, vous pouvez remarquer que toutes les lignes n'ont pas été retournées.  
  
```sql
-- Uses AdventureWorks  
  
SET ROWCOUNT 5;  
SELECT * FROM [dbo].[DimAccount]  
WHERE AccountType = 'Assets';  
```  
  
 Pour retourner toutes les lignes, affectez la valeur 0 à ROWCOUNT.  
  
```sql
-- Uses AdventureWorks  
  
SET ROWCOUNT 0;  
SELECT * FROM [dbo].[DimAccount]  
WHERE AccountType = 'Assets';  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Instructions SET &#40;Transact-SQL&#41;](../../t-sql/statements/set-statements-transact-sql.md)  
  
  

