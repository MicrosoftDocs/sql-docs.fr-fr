---
description: COMPRESS (Transact-SQL)
title: COMPRESS (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 10/11/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- COMPRESS
- COMPRESS_TSQL
helpviewer_keywords:
- COMPRESS function
ms.assetid: c2bfe9b8-57a4-48b4-b028-e1a3ed5ece88
author: markingmyname
ms.author: maghan
ms.openlocfilehash: ce9659824a46615da6056928c81e6a8cc4b98b36
ms.sourcegitcommit: 192f6a99e19e66f0f817fdb1977f564b2aaa133b
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/25/2020
ms.locfileid: "96128551"
---
# <a name="compress-transact-sql"></a>COMPRESS (Transact-SQL)
[!INCLUDE [sqlserver2016-asdb-asdbmi-asa](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi-asa.md)]

Cette fonction compresse l’expression d’entrée à l’aide de l’algorithme GZIP. La fonction retourne un tableau d’octets de type **varbinary(max)**.
  
![Icône du lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql
COMPRESS ( expression )  
```  
  
## <a name="arguments"></a>Arguments
*expression*  
A

* **binary(** _n_*_)_*
* **char(** _n_*_)_*
* **nchar(** _n_*_)_*
* **nvarchar(max)**
* **nvarchar(** _n_*_)_*
* **varbinary(max)**
* **varbinary(** _n_*_)_*
* **varchar(max)**

ou

* **varchar(** _n_*_)_*

expression. Pour plus d’informations, consultez [Expressions &#40;Transact-SQL&#41;](../../t-sql/language-elements/expressions-transact-sql.md).
  
## <a name="return-types"></a>Types de retour
**varbinary(max)** représentant le contenu compressé de l’entrée.
  
## <a name="remarks"></a>Remarques  
Les données compressées ne peuvent pas être indexées.
  
La fonction `COMPRESS` compresse les données d’expression d’entrée. Vous devez appeler cette fonction pour chaque section de données à compresser. Consultez [Compression des données](../../relational-databases/data-compression/data-compression.md) pour plus d’informations sur la compression automatique des données pendant le stockage au niveau des lignes ou des pages.
  
## <a name="examples"></a>Exemples  
  
### <a name="a-compress-data-during-the-table-insert"></a>R. Compresser les données durant l’insertion dans la table  
Cet exemple montre comment compresser les données insérées dans une table :
  
```sql
INSERT INTO player (name, surname, info )  
VALUES (N'Ovidiu', N'Cracium',   
        COMPRESS(N'{"sport":"Tennis","age": 28,"rank":1,"points":15258, turn":17}'));  
  
INSERT INTO player (name, surname, info )  
VALUES (N'Michael', N'Raheem', compress(@info));  
```  
  
### <a name="b-archive-compressed-version-of-deleted-rows"></a>B. Archiver la version compressée des lignes supprimées  
Cette instruction commence par supprimer les anciens enregistrements de lecteur de la table `player`. Pour gagner de l’espace, elle stocke les enregistrements dans la table `inactivePlayer`, dans un format compressé.
  
```sql
DELETE FROM player  
OUTPUT deleted.id, deleted.name, deleted.surname, deleted.datemodifier, COMPRESS(deleted.info)   
INTO dbo.inactivePlayers
WHERE datemodified < @startOfYear; 
```  
  
## <a name="see-also"></a>Voir aussi
[Fonctions de chaîne &#40;Transact-SQL&#41;](../../t-sql/functions/string-functions-transact-sql.md)  
[DECOMPRESS &#40;Transact-SQL&#41;](../../t-sql/functions/decompress-transact-sql.md)
  
  
