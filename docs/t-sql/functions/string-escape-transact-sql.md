---
description: STRING_ESCAPE (Transact-SQL)
title: STRING_ESCAPE (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 02/25/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- STRING_ESCAPE
- STRING_ESCAPE_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- STRING_ESCAPE function
ms.assetid: 2163bc7a-3816-4304-9c40-8954804f5465
author: julieMSFT
ms.author: jrasnick
monikerRange: = azuresqldb-current||>= sql-server-2016||>= sql-server-linux-2017
ms.openlocfilehash: fb23ecedcaa224d66a6d23a113e696dd05caae05
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99207924"
---
# <a name="string_escape-transact-sql"></a>STRING_ESCAPE (Transact-SQL)


[!INCLUDE [sqlserver2016-asdb-asdbmi](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi.md)]

Échappe les caractères spéciaux dans les textes et retourne le texte avec des caractères d’échappement. **STRING_ESCAPE** est une fonction déterministe, introduite dans SQL Server 2016. 
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql
STRING_ESCAPE( text , type )  
```  

## <a name="arguments"></a>Arguments

 *text*  
 [Expression](../../t-sql/language-elements/expressions-transact-sql.md)**nvarchar** représentant l’objet qui doit être échappé.  
  
 *type*  
 Règles d’échappement qui seront appliquées. La valeur prise en charge actuellement est `'json'`.  
  
## <a name="return-types"></a>Types de retour

 Texte **nvarchar(max)** avec caractères de contrôle et spéciaux d’échappement. Actuellement, **STRING_ESCAPE** peut uniquement échapper les caractères spéciaux JSON répertoriés dans les tableaux suivants.  
  
|Caractère spécial|Séquence codée|  
|-----------------------|----------------------|  
|guillemets (")|\\"|  
|Barre oblique inverse (\\)| \\\\ |  
|Barre oblique (/)|\\/|  
|Retour arrière|\b|  
|Saut de page|\f|  
|Nouvelle ligne|\n|  
|Retour chariot|\r|  
|Tabulation horizontale|\t|  
  
|Caractère de contrôle|Séquence codée|  
|-----------------------|----------------------|  
|CHAR(0)|\u0000|  
|CHAR(1)|\u0001|  
|...|...|  
|CHAR(31)|\u001f|  
  
## <a name="remarks"></a>Notes  
  
## <a name="examples"></a>Exemples  
  
### <a name="a--escape-text-according-to-the-json-formatting-rules"></a>R.  Échapper du texte selon les règles de mise en forme JSON

 La requête suivante échappe les caractères spéciaux à l’aide des règles JSON et retourne le texte avec séquence d’échappement.  
  
```sql
SELECT STRING_ESCAPE('\   /  
\\    "     ', 'json') AS escapedText;  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```
escapedText  
-------------------------------------------------------------  
\\\t\/\n\\\\\t\"\t
```  
  
### <a name="b-format-json-object"></a>B. Mettre en forme un objet JSON

 La requête suivante crée un texte JSON à partir de variables numériques et de chaîne, et échappe les caractères JSON spéciaux dans les variables.  
  
```
SET @json = FORMATMESSAGE('{ "id": %d,"name": "%s", "surname": "%s" }',
    17, STRING_ESCAPE(@name,'json'), STRING_ESCAPE(@surname,'json') );  
```  
  
## <a name="see-also"></a>Voir aussi

- [CONCAT &#40;Transact-SQL&#41;](../../t-sql/functions/concat-transact-sql.md)  
- [CONCAT_WS &#40;Transact-SQL&#41;](../../t-sql/functions/concat-ws-transact-sql.md)  
- [FORMATMESSAGE &#40;Transact-SQL&#41;](../../t-sql/functions/formatmessage-transact-sql.md)  
- [QUOTENAME &#40;Transact-SQL&#41;](../../t-sql/functions/quotename-transact-sql.md)  
- [REPLACE &#40;Transact-SQL&#41;](../../t-sql/functions/replace-transact-sql.md)  
- [REVERSE &#40;Transact-SQL&#41;](../../t-sql/functions/reverse-transact-sql.md)  
- [STRING_AGG &#40;Transact-SQL&#41;](../../t-sql/functions/string-agg-transact-sql.md)  
- [STUFF &#40;Transact-SQL&#41;](../../t-sql/functions/stuff-transact-sql.md)  
- [TRANSLATE &#40;Transact-SQL&#41;](../../t-sql/functions/translate-transact-sql.md)  
- [Fonctions de chaîne &#40;Transact-SQL&#41;](../../t-sql/functions/string-functions-transact-sql.md)
