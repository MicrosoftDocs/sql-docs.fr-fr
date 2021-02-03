---
description: READTEXT (Transact-SQL)
title: READTEXT (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 10/24/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- READTEXT_TSQL
- READTEXT
dev_langs:
- TSQL
helpviewer_keywords:
- column reading [SQL Server]
- READTEXT statement
- reading columns
ms.assetid: 91b69853-1381-4306-8343-afdb73105738
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: e312bff216746345fc028b1ff581bdee56a12eca
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99207617"
---
# <a name="readtext-transact-sql"></a>READTEXT (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Lit les valeurs **text**, **ntext** ou **image** d’une colonne **text**, **ntext** ou **image**. Commence la lecture à la position de décalage spécifiée et lit le nombre d’octets indiqué.  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)] À la place, utilisez la fonction [SUBSTRING](../../t-sql/functions/substring-transact-sql.md).  
  
![Icône du lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql
READTEXT { table.column text_ptr offset size } [ HOLDLOCK ]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments
_table_ **.** _column_  
Nom de la table et de la colonne dont le contenu doit être lu. Les noms de la table et de la colonne doivent suivre les règles applicables aux [identificateurs](../../relational-databases/databases/database-identifiers.md). Les noms de la table et de la colonne sont obligatoires ; ceux de la base de données et du propriétaire sont facultatifs.  
  
_text\_ptr_  
Pointeur de texte valide. _text\_ptr_ doit être de type **binary(16)** .  
  
_offset_  
Est le nombre d’octets lorsque les types de données **text** ou **image** sont utilisés. Cela peut aussi être le nombre d’octets des caractères, lorsque le type de données **ntext** est utilisé, à ignorer avant de commencer la lecture des données **text**, **image** ou **ntext**.  
  
_size_ Est le nombre d’octets lorsque les types de données **text** ou **image** sont utilisés. C’est également le nombre d’octets des caractères lorsque le type de données **ntext** est utilisé pour les données à lire. Si _size_ a pour valeur 0, 4 Ko de données sont lues.  
  
HOLDLOCK  
Demande de verrouiller la valeur texte en lecture jusqu'à la fin de la transaction. Les autres utilisateurs peuvent lire la valeur, mais pas la modifier.  
  
## <a name="remarks"></a>Notes   
Utilisez la fonction [TEXTPTR](../../t-sql/functions/text-and-image-functions-textptr-transact-sql.md) pour obtenir une valeur _text\_ptr_ valide. TEXTPTR retourne un pointeur vers la colonne **text**, **ntext** ou **image** dans la ligne spécifiée. TEXTPRT peut également retourner un pointeur vers la colonne **text**, **ntext** ou **image** dans la dernière ligne que la requête retourne si celle-ci retourne plusieurs lignes. Comme la fonction TEXTPTR retourne une chaîne binaire de 16 octets, il est conseillé de déclarer une variable locale pour stocker le pointeur de texte et utiliser cette variable dans READTEXT. Pour plus d’informations sur la déclaration d’une variable locale, consultez [DECLARE @local_variable &#40;Transact-SQL&#41;](../../t-sql/language-elements/declare-local-variable-transact-sql.md).  
  
Dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], il peut exister des pointeurs de texte dans la ligne non valides. Pour plus d’informations sur l’option **text in row**, consultez [sp_tableoption &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-tableoption-transact-sql.md). Pour plus d’informations sur l’invalidation des pointeurs de texte, consultez [sp_invalidate_textptr &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-invalidate-textptr-transact-sql.md).  
  
La valeur de la fonction @@TEXTSIZE remplace la taille indiquée dans READTEXT si elle est inférieure à cette dernière. La fonction @@TEXTSIZE spécifie la limite du nombre d’octets de données à retourner, défini à l’aide de l’instruction SET TEXTSIZE. Pour plus d’informations sur la définition du paramètre de session pour TEXTSIZE, consultez [SET TEXTSIZE &#40;Transact-SQL&#41;](../../t-sql/statements/set-textsize-transact-sql.md).  
  
## <a name="permissions"></a>Autorisations  
Les autorisations READTEXT reviennent par défaut aux utilisateurs ayant des autorisations SELECT sur la table indiquée. Ces autorisations peuvent être transférées lorsque les autorisations SELECT sont transférées.  
  
## <a name="examples"></a>Exemples  
L’exemple suivant lit les données du deuxième au vingt-sixième caractère de la colonne `pr_info` de la table `pub_info`.  
  
> [!NOTE]  
>  Pour exécuter cet exemple, vous devez installer l’exemple de base de données [**pubs**](https://github.com/microsoft/sql-server-samples/tree/master/samples/databases).  
  
```sql
USE pubs;  
GO  
DECLARE @ptrval VARBINARY(16);  
SELECT @ptrval = TEXTPTR(pr_info)   
   FROM pub_info pr INNER JOIN publishers p  
      ON pr.pub_id = p.pub_id   
      AND p.pub_name = 'New Moon Books'  
READTEXT pub_info.pr_info @ptrval 1 25;  
GO  
```  
  
## <a name="see-also"></a> Voir aussi  
[@@TEXTSIZE &#40;Transact-SQL&#41;](../../t-sql/functions/textsize-transact-sql.md)   
[UPDATETEXT &#40;Transact-SQL&#41;](../../t-sql/queries/updatetext-transact-sql.md)   
[WRITETEXT &#40;Transact-SQL&#41;](../../t-sql/queries/writetext-transact-sql.md)  
  
  
