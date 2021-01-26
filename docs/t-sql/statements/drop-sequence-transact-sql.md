---
description: DROP SEQUENCE (Transact-SQL)
title: DROP SEQUENCE (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 05/11/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- DROP SEQUENCE
- DROP_SEQUENCE_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- DROP SEQUENCE statement
- sequence number object, dropping
ms.assetid: c25772d3-61af-4aa7-b58b-a6f67a793e3d
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: c68a677a91e0b89b35866070fa7cb605d5195ab8
ms.sourcegitcommit: 713e5a709e45711e18dae1e5ffc190c7918d52e7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/22/2021
ms.locfileid: "98688798"
---
# <a name="drop-sequence-transact-sql"></a>DROP SEQUENCE (Transact-SQL)
[!INCLUDE [SQL Server Azure SQL Database ](../../includes/applies-to-version/sql-asdb.md)]

  Permet de supprimer un objet séquence de la base de données actuelle.  
  
 ![Icône du lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql
DROP SEQUENCE [ IF EXISTS ] { database_name.schema_name.sequence_name | schema_name.sequence_name | sequence_name } [ ,...n ]  
 [ ; ]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments
 *IF EXISTS*  
 **S’applique à**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (de [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] à la [version actuelle](../../sql-server/what-s-new-in-sql-server-2016.md)).  
  
 Supprime, de manière conditionnelle, la séquence uniquement si elle existe déjà.  
  
 *database_name*  
 Nom de la base de données dans laquelle l'objet séquence a été créé.  
  
 *schema_name*  
 Nom du schéma auquel appartient l'objet séquence.  
  
 *sequence_name*  
 Nom de la séquence à supprimer. Le type est **sysname**.  
  
## <a name="remarks"></a>Notes  
 Après avoir généré un nombre, un objet séquence n'a aucune relation continue au nombre qu'il a généré ; par conséquent, l'objet séquence peut être supprimé, bien que le nombre généré soit encore en cours d'utilisation.  
  
 Un objet séquence peut être supprimé alors qu'il est référencé par une procédure stockée ou un déclencheur, car il n'est pas lié au schéma. Un objet séquence ne peut pas être supprimé s'il est référencé en tant que valeur par défaut dans une table. Le message d'erreur indiquera l'objet qui référence la séquence.  
  
 Pour répertorier tous les objets séquences dans la base de données, exécutez l'instruction suivante.  
  
```sql  
SELECT sch.name + '.' + seq.name AS [Sequence schema and name]   
    FROM sys.sequences AS seq  
    JOIN sys.schemas AS sch  
        ON seq.schema_id = sch.schema_id ;  
GO  
```  
  
## <a name="security"></a>Sécurité  
  
### <a name="permissions"></a>Autorisations  
 Requiert l'autorisation ALTER ou CONTROL sur le schéma.  
  
### <a name="audit"></a>Audit  
 Pour auditer **DROP SEQUENCE**, surveillez **SCHEMA_OBJECT_CHANGE_GROUP**.  
  
## <a name="examples"></a>Exemples  
 L'exemple ci-dessous supprime un objet séquence nommé `CountBy1` dans la base de données actuelle.  
  
```sql  
DROP SEQUENCE CountBy1 ;  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [ALTER SEQUENCE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-sequence-transact-sql.md)   
 [CREATE SEQUENCE &#40;Transact-SQL&#41;](../../t-sql/statements/create-sequence-transact-sql.md)   
 [NEXT VALUE FOR &#40;Transact-SQL&#41;](../../t-sql/functions/next-value-for-transact-sql.md)   
 [Numéros de séquence](../../relational-databases/sequence-numbers/sequence-numbers.md)  
