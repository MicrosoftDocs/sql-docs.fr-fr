---
description: Fonctions texte et image - TEXTPTR (Transact-SQL)
title: TEXTPTR (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 10/23/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- TEXTPTR_TSQL
- TEXTPTR
dev_langs:
- TSQL
helpviewer_keywords:
- TEXTPTR function
- viewing text pointer values
- text-pointer values
- displaying text pointer values
ms.assetid: 2672b8cb-f747-46f3-9358-9b49b3583b8e
author: julieMSFT
ms.author: jrasnick
ms.openlocfilehash: 39e1c646755c4ea2907b328938cac0f30f6f47d9
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99181449"
---
# <a name="text-and-image-functions---textptr-transact-sql"></a>Fonctions texte et image - TEXTPTR (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Retourne la valeur du pointeur de texte qui correspond à une colonne de type **text**, **ntext** ou **image** au format **varbinary**. La valeur du pointeur de texte obtenue peut être utilisée dans les instructions READTEXT, WRITETEXT et UPDATETEXT.  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)] Aucune fonctionnalité de remplacement n'est disponible.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql
TEXTPTR ( column )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments
 *column*  
 Colonne **text**, **ntext** ou **image** qui sera utilisée.  
  
## <a name="return-types"></a>Types de retour  
 **varbinary**  
  
## <a name="remarks"></a>Notes  
 Pour les tables avec du texte en ligne, TEXTPTR renvoie un descripteur pour le texte à traiter. Vous pouvez obtenir un pointeur de texte valide même lorsque la valeur du texte est nulle.  
  
 Vous ne pouvez pas utiliser la fonction TEXTPTR sur des colonnes de vues. Vous ne pouvez l'utiliser que sur des colonnes de tables. Pour utiliser la fonction TEXTPTR sur une colonne de vue, vous devez spécifier un niveau de compatibilité égal à 80 à l’aide du [niveau de compatibilité ALTER DATABASE](../../t-sql/statements/alter-database-transact-sql-compatibility-level.md). Si la table ne possède pas de texte en ligne et si aucune colonne **text**, **ntext** ni **image** n’a été initialisée par une instruction UPDATETEXT, la fonction TEXTPTR retourne un pointeur null.  
  
 Utilisez TEXTVALID pour vérifier l'existence d'un pointeur de texte. Vous ne pouvez pas utiliser UPDATETEXT, WRITETEXT ou READTEXT sans pointeur de texte valide.  
  
 Ces fonctions et instructions sont également utiles lorsque vous travaillez avec des données **text**, **ntext** et **image**.  
  
|Fonction ou instruction|Description|  
|---------------------------|-----------------|  
|PATINDEX <b>('</b> _%pattern%_ **' ,** _expression_ **)**|Retourne la position d’un caractère dans la chaîne de caractères spécifiée dans les colonnes **text** et **ntext**.|  
|DATALENGTH <b>(</b>_expression_ **)**|Retourne la longueur des données dans les colonnes de type **text**, **ntext** et **image**.|  
|SET TEXTSIZE|Retourne la limite, en octets, des données **text**, **ntext** ou **image** à retourner avec une instruction SELECT.|  
|SUBSTRING <b>(</b>_text_column_, _start_, _length_ **)**|Retourne une chaîne **varchar** spécifiée par le décalage *start* et par la longueur *length*. La longueur doit être inférieure à 8 Ko.|  
  
## <a name="examples"></a>Exemples  
  
> [!NOTE]  
>  Pour exécuter les exemples suivants, vous devez installer la base de données **pubs**.  
  
### <a name="a-using-textptr"></a>R. Utilisation de TEXTPTR  
 L’exemple qui suit utilise la fonction `TEXTPTR` pour rechercher le `logo` de la colonne **image** associé à `New Moon Books` dans la table `pub_info` de la base de données `pubs`. Le pointeur de texte identifie la variable locale `@ptrval.`.  
  
```sql
USE pubs;  
GO  
DECLARE @ptrval VARBINARY(16);  
SELECT @ptrval = TEXTPTR(logo)  
FROM pub_info pr, publishers p  
WHERE p.pub_id = pr.pub_id   
   AND p.pub_name = 'New Moon Books';  
GO  
```  
  
### <a name="b-using-textptr-with-in-row-text"></a>B. Utilisation de TEXTPTR avec du texte dans la ligne  
 Dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], le pointeur de texte dans la ligne doit être utilisé au sein d'une transaction, comme le montre l'exemple suivant.  
  
```sql
CREATE TABLE t1 (c1 INT, c2 TEXT);  
EXEC sp_tableoption 't1', 'text in row', 'on';  
INSERT t1 VALUES ('1', 'This is text.');  
GO  
BEGIN TRAN;  
   DECLARE @ptrval VARBINARY(16);  
   SELECT @ptrval = TEXTPTR(c2)  
   FROM t1  
   WHERE c1 = 1;  
   READTEXT t1.c2 @ptrval 0 1;  
COMMIT;  
```  
  
### <a name="c-returning-text-data"></a>C. Renvoi des données texte  
 Cet exemple sélectionne la colonne `pub_id` et le pointeur de texte sur 16 octets de la colonne `pr_info` dans la table `pub_info`.  
  
```sql
USE pubs;  
GO  
SELECT pub_id, TEXTPTR(pr_info)  
FROM pub_info  
ORDER BY pub_id;  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
pub_id                                      
------ ----------------------------------   
0736   0x6c0000000000feffb801000001000100   
0877   0x6d0000000000feffb801000001000300   
1389   0x6e0000000000feffb801000001000500   
1622   0x700000000000feffb801000001000900   
1756   0x710000000000feffb801000001000b00   
9901   0x720000000000feffb801000001000d00   
9952   0x6f0000000000feffb801000001000700   
9999   0x730000000000feffb801000001000f00   
  
(8 row(s) affected)  
```  
  
 Cet exemple indique comment retourner les `8000` premiers octets de texte sans utiliser TEXTPTR.  
  
```sql
USE pubs;  
GO  
SET TEXTSIZE 8000;  
SELECT pub_id, pr_info  
FROM pub_info  
ORDER BY pub_id;  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
pub_id pr_info                                                                                                                                                                                                                                                           
------ -----------------------------------------------------------------  
0736   New Moon Books (NMB) has just released another top ten publication. With the latest publication this makes NMB the hottest new publisher of the year!                                                                                                             
0877   This is sample text data for Binnet & Hardley, publisher 0877 in the pubs database. Binnet & Hardley is located in Washington, D.C.  
  
This is sample text data for Binnet & Hardley, publisher 0877 in the pubs database. Binnet & Hardley is located in Washi   
1389   This is sample text data for Algodata Infosystems, publisher 1389 in the pubs database. Algodata Infosystems is located in Berkeley, California.  
  
9999   This is sample text data for Lucerne Publishing, publisher 9999 in the pubs database. Lucerne publishing is located in Paris, France.  
  
This is sample text data for Lucerne Publishing, publisher 9999 in the pubs database. Lucerne publishing is located in   
  
(8 row(s) affected)  
```  
  
### <a name="d-returning-specific-text-data"></a>D. Renvoi des données texte spécifiques  
 L’exemple qui suit localise la colonne `text` (`pr_info`) associée à `pub_id``0736` dans la table `pub_info` de la base de données `pubs`. Elle commence par déclarer la variable locale `@val`. Le pointeur de texte (une chaîne binaire de type long) est ensuite placé dans `@val`, puis passé comme paramètre à l'instruction `READTEXT`. Cette opération renvoie 10 octets à partir du cinquième octet (décalage de 4 octets).  
  
```sql
USE pubs;  
GO  
DECLARE @val VARBINARY(16);  
SELECT @val = TEXTPTR(pr_info)   
FROM pub_info  
WHERE pub_id = '0736';  
READTEXT pub_info.pr_info @val 4 10;  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
pr_info                                                                                                                                                                                                                                                           
-----------------------------------------------------------------------  
 is sample  
(1 row(s) affected)  
```  
  
## <a name="see-also"></a>Voir aussi  
 [DATALENGTH &#40;Transact-SQL&#41;](../../t-sql/functions/datalength-transact-sql.md)   
 [PATINDEX &#40;Transact-SQL&#41;](../../t-sql/functions/patindex-transact-sql.md)   
 [READTEXT &#40;Transact-SQL&#41;](../../t-sql/queries/readtext-transact-sql.md)   
 [SET TEXTSIZE &#40;Transact-SQL&#41;](../../t-sql/statements/set-textsize-transact-sql.md)   
 [Fonctions texte et image &#40;Transact-SQL&#41;]()   
 [UPDATETEXT &#40;Transact-SQL&#41;](../../t-sql/queries/updatetext-transact-sql.md)   
 [WRITETEXT &#40;Transact-SQL&#41;](../../t-sql/queries/writetext-transact-sql.md)  
  
