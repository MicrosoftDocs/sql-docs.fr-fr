---
description: TRY_PARSE (Transact-SQL)
title: TRY_PARSE (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- TRY_PARSE_TSQL
- TRY_PARSE
dev_langs:
- TSQL
helpviewer_keywords:
- TRY_PARSE function
ms.assetid: 292bac1d-edd8-468c-8ff1-8c7de625bc55
author: julieMSFT
ms.author: jrasnick
monikerRange: = azuresqldb-current||>= sql-server-2016||>= sql-server-linux-2017||=azure-sqldw-latest
ms.openlocfilehash: dfd39a530ca9bac61d2a4ac6036eaae7d5c37152
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99183334"
---
# <a name="try_parse-transact-sql"></a>TRY_PARSE (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa](../../includes/applies-to-version/sql-asdb-asdbmi-asa.md)]

  Retourne le résultat d'une expression, traduit en type de données demandé, ou NULL si la conversion échoue dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Utilisez TRY_PARSE uniquement pour effectuer une conversion d'une chaîne en un type date/heure ou numérique.  
  
 ![Icône du lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql
TRY_PARSE ( string_value AS data_type [ USING culture ] )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments
 *string_value*  
 Valeur **nvarchar(4000)** représentant la valeur mise en forme à analyser dans le type de données spécifié.  
  
 *string_value* doit être une représentation valide du type de données demandé, sinon TRY_PARSE renvoie la valeur Null.  
  
 *data_type*  
 Littéral représentant le type de données demandé pour le résultat.  
  
 *culture*  
 Chaîne facultative qui identifie la culture dans laquelle *string_value* est mise en forme.  
  
 Si l’argument *culture* n’est pas fourni, la langue de la session en cours est utilisée. Cette langue est définie implicitement, ou explicitement à l'aide de l'instruction SET LANGAGE. *culture* accepte n’importe quelle culture prise en charge par le .NET Framework ; elle n’est pas limitée aux langues explicitement prises en charge par [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Si l’argument *culture* n’est pas valide, PARSE génère une erreur.  
  
## <a name="return-types"></a>Types de retour  
 Retourne le résultat de l'expression, traduit en type de données demandé, ou NULL si la conversion échoue.  
  
## <a name="remarks"></a>Remarques  
 Utilisez TRY_PARSE uniquement pour effectuer une conversion d'une chaîne en un type date/heure ou numérique. Pour les conversions de type général, continuez à utiliser CAST ou CONVERT. N'oubliez pas qu'il existe une certaine surcharge des performances lors de l'analyse de la valeur de chaîne.  
  
 TRY_PARSE repose sur la présence du CLR (Common Langage Runtime) .NET Framework.  
  
 Cette fonction ne peut pas être exécutée à distance car elle dépend de la présence du CLR. L'exécution à distance d'une fonction qui nécessite le CLR provoquerait une erreur sur le serveur distant.  
  
 **Informations supplémentaires sur le paramètre data_type**  
  
 Les valeurs du paramètre *data_type* sont limitées aux types répertoriés avec les styles dans le tableau ci-dessous. Les informations sur le style sont fournies pour aider à déterminer les types de modèles autorisés. Pour plus d’informations sur les styles, consultez la documentation du .NET Framework pour les énumérations **System.Globalization.NumberStyles** et **DateTimeStyles**.  
  
|Category|Type|Type .NET|Styles utilisés|  
|--------------|----------|---------------|-----------------|  
|Numérique|bigint|Int64|NumberStyles.Number|  
|Numérique|int|Int32|NumberStyles.Number|  
|Numérique|SMALLINT|Int16|NumberStyles.Number|  
|Numérique|TINYINT|Byte|NumberStyles.Number|  
|Numérique|Décimal|Décimal|NumberStyles.Number|  
|Numérique|numeric|Decimal|NumberStyles.Number|  
|Numérique|float|Double|NumberStyles.Float|  
|Numérique|real|Unique|NumberStyles.Float|  
|Numérique|SMALLMONEY|Decimal|NumberStyles.Currency|  
|Numérique|money|Decimal|NumberStyles.Currency|  
|Date et heure|Date|DateTime|DateTimeStyles.AllowWhiteSpaces &#124; DateTimeStyles.AssumeUniversal|  
|Date et heure|time|TimeSpan|DateTimeStyles.AllowWhiteSpaces &#124; DateTimeStyles.AssumeUniversal|  
|Date et heure|DATETIME|DateTime|DateTimeStyles.AllowWhiteSpaces &#124; DateTimeStyles.AssumeUniversal|  
|Date et heure|smalldatetime|DateTime|DateTimeStyles.AllowWhiteSpaces &#124; DateTimeStyles.AssumeUniversal|  
|Date et heure|datetime2|DateTime|DateTimeStyles.AllowWhiteSpaces &#124; DateTimeStyles.AssumeUniversal|  
|Date et heure|datetimeoffset|DateTimeOffset|DateTimeStyles.AllowWhiteSpaces &#124; DateTimeStyles.AssumeUniversal|  
  
 **Informations supplémentaires sur le paramètre culture**  
  
 Le tableau suivant montre les mappages des langues de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] aux cultures .NET framework.  
  
|Nom complet|Alias|LCID|Culture spécifique|  
|---------------|-----------|----------|----------------------|  
|us_english|Anglais|1033|fr-FR|  
|Deutsch|Allemand|1031|de-DE|  
|Français|Français|1036|fr-FR|  
|日本語|Japonais|1041|ja-JP|  
|Dansk|Danois|1030|da-DK|  
|Español|Espagnol|3082|es-ES|  
|Italiano|Italien|1040|it-IT|  
|Nederlands|Néerlandais|1043|nl-NL|  
|Norsk|Norvégien|2068|nn-NO|  
|Português|Portugais|2070|pt-PT|  
|Suomi|Finnois|1035|fi-FI|  
|Svenska|Suédois|1053|sv-SE|  
|čeština|Tchèque|1029|Cs-CZ|  
|magyar|Hongrois|1038|Hu-HU|  
|polski|Polonais|1045|Pl-PL|  
|română|Roumain|1048|Ro-RO|  
|hrvatski|Croate|1050|hr-HR|  
|slovenčina|Slovaque|1051|Sk-SK|  
|slovenski|Slovène|1060|Sl-SI|  
|ελληνικά|Grec|1032|El-GR|  
|български|Bulgare|1026|bg-BG|  
|русский|Russe|1049|Ru-RU|  
|Türkçe|Turc|1055|Tr-TR|  
|British|British English|2057|en-GB|  
|eesti|Estonien|1061|Et-EE|  
|latviešu|Letton|1062|lv-LV|  
|lietuvių|Lituanien|1063|lt-LT|  
|Português (Brasil)|Brésilien|1046|pt-br|  
|繁體中文|Chinois traditionnel|1028|zh-TW|  
|한국어|Coréen|1042|Ko-KR|  
|简体中文|Chinois simplifié|2052|zh-CN|  
|Arabe|Arabe|1025|ar-SA|  
|ไทย|Thaï|1054|Th-TH|  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-simple-example-of-try_parse"></a>R. Exemple simple de TRY_PARSE  
  
```sql
SELECT TRY_PARSE('Jabberwokkie' AS datetime2 USING 'en-US') AS Result;  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
Result  
---------------  
NULL  
  
(1 row(s) affected)  
```  
  
### <a name="b-detecting-nulls-with-try_parse"></a>B. Détection de valeurs NULL avec TRY_PARSE  
  
```sql
SELECT  
    CASE WHEN TRY_PARSE('Aragorn' AS decimal USING 'sr-Latn-CS') IS NULL  
        THEN 'True'  
        ELSE 'False'  
END  
AS Result;  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
Result  
---------------  
True  
  
(1 row(s) affected)  
```  
  
### <a name="c-using-iif-with-try_parse-and-implicit-culture-setting"></a>C. Utilisation de IIF avec TRY_PARSE et paramètre de culture implicite  
  
```sql
SET LANGUAGE English;  
SELECT IIF(TRY_PARSE('01/01/2011' AS datetime2) IS NULL, 'True', 'False') AS Result;
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
Result  
---------------  
False  
  
(1 row(s) affected)  
```  
  
## <a name="see-also"></a>Voir aussi  
 [PARSE &#40;Transact-SQL&#41;](../../t-sql/functions/parse-transact-sql.md)   
 [Fonctions de conversion &#40;Transact-SQL&#41;](../../t-sql/functions/conversion-functions-transact-sql.md)   
 [TRY_CONVERT &#40;Transact-SQL&#41;](../../t-sql/functions/try-convert-transact-sql.md)   
 [CAST et CONVERT &#40;Transact-SQL&#41;](../../t-sql/functions/cast-and-convert-transact-sql.md)  
  
  
