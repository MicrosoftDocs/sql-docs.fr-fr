---
title: SUBSTRING (Transact-SQL) | Microsoft Docs
description: Référence Transact-SQL pour la fonction SUBSTRING. Cette fonction retourne une partie d’une expression de type caractère, binaire, texte ou image spécifiée.
ms.date: 10/21/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- SUBSTRING
- SUBSTRING_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- portion of expression returned [SQL Server]
- part of expression returned [SQL Server]
- SUBSTRING function
- offsets [SQL Server]
- binary [SQL Server], returning part of
- expressions [SQL Server], part returned
- characters [SQL Server], returning part of
ms.assetid: a19c808f-aaf9-4a69-af59-b1a5fc3e5c4c
author: julieMSFT
ms.author: jrasnick
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 2a220d976b1e85dc8559b34d74e8280847afccce
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99207953"
---
# <a name="substring-transact-sql"></a>SUBSTRING (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Retourne une partie d'une expression de type caractère, binaire, texte ou image dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 ![Icône du lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql
SUBSTRING ( expression ,start , length )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments
 *expression*  
 Est une [expression](../../t-sql/language-elements/expressions-transact-sql.md) de type **character**, **binary**, **text**, **ntext** ou **image**.  
  
 *start*  
 Expression entière ou **bigint** qui spécifie où les caractères retournés commencent. (La numérotation est basée sur 1, ce qui signifie que le premier caractère de l’expression est 1). Si *start* est inférieur à 1, l’expression retournée commence au premier caractère spécifié dans *expression*. Dans ce cas, le nombre de caractères retournés correspond à la valeur la plus grande entre la somme de *start* + *length*- 1 et 0. Si *start* est supérieur au nombre de caractères dans l’expression de valeur, une expression de longueur nulle est retournée.  
  
 *length*  
 Expression **bigint** ou entière positive qui spécifie le nombre de caractères d’*expression* à retourner. Si *length* est négatif, une erreur est générée et l’instruction est arrêtée. Si la somme de *start* et *length* est supérieure au nombre de caractères dans *expression*, l’expression de valeur entière qui commence à *start* est retournée.  
  
## <a name="return-types"></a>Types de retour  
 Retourne des données de type caractère si *expression* correspond à l’un des types de données caractères pris en charge. Retourne des données binaires si *expression* correspond à l’un des types de données **binary** pris en charge. La chaîne retournée est du même type que l'expression spécifiée, sauf pour les exceptions énumérées dans le tableau suivant :  
  
|Expression spécifiée|Type de retour|  
|--------------------------|-----------------|  
|**char**/**varchar**/**text**|**varchar**|  
|**nchar**/**nvarchar**/**ntext**|**nvarchar**|  
|**binary**/**varbinary**/**image**|**varbinary**|  
  
## <a name="remarks"></a>Notes  
 Les valeurs de *start* et *length* doivent être spécifiées en nombre de caractères pour les types de données **ntext**, **char** ou **varchar** et en octets pour les types de données **text**, **image**, **binary** ou **varbinary**.  
  
 *expression* doit être de type **varchar(max)** ou **varbinary(max)** lorsque *start* ou *length* contient une valeur supérieure à 2147483647.  
  
## <a name="supplementary-characters-surrogate-pairs"></a>Caractères supplémentaires (paires de substitution)  
 Lors de l’utilisation des classements de caractères supplémentaires (SC), les arguments *start* et *length* comptent chaque paire de substitution dans *expression* comme un caractère unique. Pour plus d’informations, consultez [Prise en charge d’Unicode et du classement](../../relational-databases/collations/collation-and-unicode-support.md).  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-using-substring-with-a-character-string"></a>R. Utilisation de SUBSTRING avec une chaîne de caractères  
 L'exemple suivant illustre la manière de retourner uniquement une partie d'une chaîne de caractères. À partir de la table `sys.databases`, cette requête retourne les noms de base de données système dans la première colonne, la première lettre de la base de données dans la deuxième colonne et les troisième et quatrième caractères dans la colonne finale.  
  
```sql
SELECT name, SUBSTRING(name, 1, 1) AS Initial ,
SUBSTRING(name, 3, 2) AS ThirdAndFourthCharacters
FROM sys.databases  
WHERE database_id < 5;   
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  

|name |Initial |ThirdAndFourthCharacters|
|---|--|--|
|master    |m    |st |
|tempdb    |t    |mp |
|model    |m    |de |
|msdb    |m    |db |


  
 Voici comment afficher les deuxième, troisième et quatrième caractères de la constante de chaîne `abcdef` :  
  
```sql
SELECT x = SUBSTRING('abcdef', 2, 3);  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 ```
x  
----------  
bcd  
  
(1 row(s) affected)
```  
  
### <a name="b-using-substring-with-text-ntext-and-image-data"></a>B. Utilisation de SUBSTRING avec des données text, ntext et image  
  
> [!NOTE]  
>  Pour exécuter les exemples suivants, vous devez installer la base de données **pubs**.  
  
 L’exemple suivant montre comment retourner les 10 premiers caractères de chaque colonne de données **text** et **image** de la table `pub_info` de la base de données `pubs`. Les données **text** sont retournées en tant que données **varchar** et les données **image** sont retournées en tant que **varbinary**.  
  
```sql
USE pubs;  
SELECT pub_id, SUBSTRING(logo, 1, 10) AS logo,   
   SUBSTRING(pr_info, 1, 10) AS pr_info  
FROM pub_info  
WHERE pub_id = '1756';  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 ```
 pub_id logo    pr_info
------ ---------------------- ----------
1756   0x474946383961E3002500 This is sa

(1 row(s) affected)
```  
  
 L’exemple suivant illustre l’effet de SUBSTRING sur les données **text** et **ntext**. D'abord, cet exemple crée, dans la base de données `pubs`, une nouvelle table nommée `npub_info`. Ensuite, l'exemple crée la colonne `pr_info` dans la table `npub_info` à partir des 80 premiers caractères de la colonne `pub_info.pr_info` puis ajoute le caractère `ü` en guise de premier caractère. Pour finir, une opération `INNER JOIN` extrait tous les numéros d’identification de serveur de publication et le `SUBSTRING` des deux colonnes d’informations de serveur de publication **text** et **ntext**.  
  
```sql
IF EXISTS (SELECT table_name FROM INFORMATION_SCHEMA.TABLES   
      WHERE table_name = 'npub_info')  
   DROP TABLE npub_info;  
GO  
-- Create npub_info table in pubs database. Borrowed from instpubs.sql.  
USE pubs;  
GO  
CREATE TABLE npub_info  
(  
 pub_id CHAR(4) NOT NULL  
    REFERENCES publishers(pub_id)  
    CONSTRAINT UPKCL_npubinfo PRIMARY KEY CLUSTERED,  
pr_info ntext NULL  
);  
  
GO  
  
-- Fill the pr_info column in npub_info with international data.  
RAISERROR('Now at the inserts to pub_info...',0,1);  
  
GO  
  
INSERT npub_info VALUES('0736', N'üThis is sample text data for New Moon Books, publisher 0736 in the pubs database')  
,('0877', N'üThis is sample text data for Binnet & Hardley, publisher 0877 in the pubs databa')  
,('1389', N'üThis is sample text data for Algodata Infosystems, publisher 1389 in the pubs da')  
,('9952', N'üThis is sample text data for Scootney Books, publisher 9952 in the pubs database')  
,('1622', N'üThis is sample text data for Five Lakes Publishing, publisher 1622 in the pubs d')  
,('1756', N'üThis is sample text data for Ramona Publishers, publisher 1756 in the pubs datab')  
,('9901', N'üThis is sample text data for GGG&G, publisher 9901 in the pubs database. GGG&G i')  
,('9999', N'üThis is sample text data for Lucerne Publishing, publisher 9999 in the pubs data');  
GO  
-- Join between npub_info and pub_info on pub_id.  
SELECT pr.pub_id, SUBSTRING(pr.pr_info, 1, 35) AS pr_info,  
   SUBSTRING(npr.pr_info, 1, 35) AS npr_info  
FROM pub_info pr INNER JOIN npub_info npr  
   ON pr.pub_id = npr.pub_id  
ORDER BY pr.pub_id ASC;  
```  
  
## <a name="examples-sssdwfull-and-sspdw"></a>Exemples : [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] et [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="c-using-substring-with-a-character-string"></a>C. Utilisation de SUBSTRING avec une chaîne de caractères  
 L'exemple suivant illustre la manière de retourner uniquement une partie d'une chaîne de caractères. À partir de la table `dbo.DimEmployee`, cette requête retourne les noms de famille dans une colonne avec seulement la première initiale des prénoms dans la seconde colonne.  
  
```sql
-- Uses AdventureWorks  
  
SELECT LastName, SUBSTRING(FirstName, 1, 1) AS Initial  
FROM dbo.DimEmployee  
WHERE LastName LIKE 'Bar%'  
ORDER BY LastName;  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 ```
LastName             Initial
-------------------- -------
Barbariol            A
Barber               D
Barreto de Mattos    P
```  
  
 L’exemple suivant montre comment retourner les deuxième, troisième et quatrième caractères de la constante chaîne `abcdef`.  
  
```sql
USE ssawPDW;  
  
SELECT TOP 1 SUBSTRING('abcdef', 2, 3) AS x FROM dbo.DimCustomer;  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 ```
x
-----
bcd
```  
  
## <a name="see-also"></a>Voir aussi  
 [LEFT &#40;Transact-SQL&#41;](../../t-sql/functions/left-transact-sql.md)  
 [LTRIM &#40;Transact-SQL&#41;](../../t-sql/functions/ltrim-transact-sql.md)  
 [RIGHT &#40;Transact-SQL&#41;](../../t-sql/functions/right-transact-sql.md)  
 [RTRIM &#40;Transact-SQL&#41;](../../t-sql/functions/rtrim-transact-sql.md)  
 [STRING_SPLIT &#40;Transact-SQL&#41;](../../t-sql/functions/string-split-transact-sql.md)  
 [TRIM &#40;Transact-SQL&#41;](../../t-sql/functions/trim-transact-sql.md)  
 [Fonctions de chaîne &#40;Transact-SQL&#41;](../../t-sql/functions/string-functions-transact-sql.md)  
  
  


