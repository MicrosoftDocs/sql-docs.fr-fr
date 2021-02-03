---
title: CAST et CONVERT (Transact-SQL) | Microsoft Docs
description: Référence pour les fonctions Transact-SQL CAST et CONVERT. Ces fonctions convertissent des expressions d’un type de données en un autre.
ms.date: 08/23/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- CAST_TSQL
- CONVERT_TSQL
- CAST
- CONVERT
dev_langs:
- TSQL
helpviewer_keywords:
- CAST function
- automatic data type conversion
- varbinary data type
- CONVERT function
- data types [SQL Server], converting
- large value data types
- implicit data type conversions
- image data type, converting
- explicit data type conversions [SQL Server]
- binary [SQL Server], converting
- text data type, converting
- date and time [SQL Server], cast and convert
- truncating conversions
- converting data types [SQL Server], conversion functions
- time zones [SQL Server]
- roundtrip conversions
ms.assetid: a87d0850-c670-4720-9ad5-6f5a22343ea8
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 1572028c2aa575078392c34fca37fa8105e25661
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/01/2021
ms.locfileid: "99236382"
---
# <a name="cast-and-convert-transact-sql"></a>CAST et CONVERT (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Ces fonctions convertissent une expression d’un type de données en un autre.  

## <a name="syntax"></a>Syntaxe  
  
```sqlsyntax 
-- CAST Syntax:  
CAST ( expression AS data_type [ ( length ) ] )  
  
-- CONVERT Syntax:  
CONVERT ( data_type [ ( length ) ] , expression [ , style ] )  
```  

![Icône du lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments
*expression*  
Toute [expression](../../t-sql/language-elements/expressions-transact-sql.md) valide.
  
*data_type*  
Type de données cible. Exemples : **xml**, **bigint** et **sql_variant**. Les types de données alias ne sont pas autorisés.
  
*length*  
Entier facultatif qui spécifie la longueur du type de données cible pour les types de données qui autorisent une longueur spécifiée par l’utilisateur. La valeur par défaut est 30.
  
*style*  
Expression d’entier qui spécifie comment la fonction CONVERT doit traduire *expression*. Si le style est NULL, une valeur NULL est retournée. La plage est déterminée par *data_type*. 
  
## <a name="return-types"></a>Types de retour
Retourne *expression* traduite en *data_type*.
  
## <a name="date-and-time-styles"></a>Styles de date et d’heure  
Quand *expression* est un type de données de date ou d’heure, *style* peut prendre l’une des valeurs indiquées dans le tableau suivant. Les autres valeurs sont traitées comme étant 0. À partir de [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)], les seuls styles pris en charge lors de la conversion des types de date et d’heure en **datetimeoffset** sont 0 ou 1. Tous les autres styles de conversion retournent l'erreur 9809.
  
> [!NOTE]
> [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] prend en charge le format de date dans le style arabe à l’aide de l’algorithme koweitien.
  
|Sans siècle (aa) (<sup>1</sup>)|Avec siècle (aaaa)|Standard|Entrée/sortie (<sup>3</sup>)|  
|---|---|--|---|
|-|**0** ou **100** (<sup>1,</sup><sup>2</sup>)|Valeur par défaut pour datetime et smalldatetime|mois jj aaaa hh:miAM (ou PM)|  
|**1**|**101**|États-Unis|  1 = mm/jj/aa<br /> 101 = mm/jj/aaaa|  
|**2**|**102**|ANSI|  2 = aa.mm.jj<br /> 102 = aaaa.mm.jj|  
|**3**|**103**|Anglais/Français|  3 = jj/mm/aa<br /> 103 = jj/mm/aaaa|  
|**4**|**104**|Allemand|  4 = jj.mm.aa<br /> 104 = jj.mm.aaaa|  
|**5**|**105**|Italien|  5 = jj-mm-aa<br /> 105 = jj-mm-aaaa|  
|**6**|**106** <sup>(1)</sup>|-|  6 = jj mois aa<br /> 106 = jj mois aaaa|  
|**7**|**107** <sup>(1)</sup>|-|  7 = Mois jj, aa<br /> 107 = Mois jj, aaaa|  
|**8** ou **24**|**108**|-|hh:mi:ss|  
|-|**9** ou **109** (<sup>1,</sup><sup>2</sup>)|Valeur par défaut + millièmes de secondes|mois jj aaaa hh:mi:ss:mmmAM (ou PM)|  
|**10**|**110**|États-Unis| 10 = mm-jj-aa<br /> 110 = mm-jj-aaaa|  
|**11**|**111**|JAPON| 11 = aa/mm/jj<br /> 111 = aaaa/mm/jj|  
|**12**|**112**|ISO| 12 = aammjj<br /> 112 = aaaammjj|  
|-|**13** ou **113** (<sup>1,</sup><sup>2</sup>)|Valeur par défaut Europe + millièmes de secondes|jj mois aaaa hh:mi:ss:mmm (24h)|  
|**14**|**114**|-|hh:mi:ss:mmm (24h)|  
|-|**20** ou **120** (<sup>2</sup>)|ODBC canonique|aaaa-mm-jj hh:mi:ss (24h)|  
|-|**21** ou **25** ou **121** (<sup>2</sup>)|Valeur canonique ODBC par défaut (avec millisecondes) pour time, date, datetime2 et datetimeoffset|aaaa-mm-jj hh:mi:ss.mmm (24h)|  
|**22**|-|États-Unis| mm/jj/aa hh:mi:ss AM (ou PM)|
|-|**23**|ISO8601|aaaa-mm-jj|
|-|**126** (<sup>4</sup>)|ISO8601|aaaa-mm-jjThh:mi:ss.mmm (sans espace)<br /><br /> **Remarque :** Pour une valeur en millisecondes (mmm) égale à 0, la valeur de la fraction décimale n’est pas affichée. Par exemple, la valeur « 2012-11-07T18:26:20.000 » est affichée comme « 2012-11-07T18:26:20 ».| 
|-|**127**(<sup>6, 7</sup>)|ISO8601 avec fuseau horaire Z.|aaaa-MM-jjThh:mm:ss.fffZ (sans espace)<br /><br /> **Remarque :** Pour une valeur en millisecondes (mmm) égale à 0, la valeur décimale n’est pas affichée. Par exemple, la valeur « 2012-11-07T18:26:20.000 » est affichée comme « 2012-11-07T18:26:20 ».|  
|-|**130** (<sup>1,</sup><sup>2</sup>)|Hijri (<sup>5</sup>)|jj mois aaaa hh:mi:ss:mmmAM<br /><br /> Dans ce style, **mois** correspond à une représentation Unicode Hijri à plusieurs jetons du nom complet du mois. Cette valeur n’est pas retournée correctement sur les installations en anglais américain par défaut de SSMS.|  
|-|**131** (<sup>2</sup>)|Hijri (<sup>5</sup>)|jj/mm/yyyy hh:mi:ss:mmmAM|  
  
<sup>1</sup> Ces valeurs de style retournent des résultats non déterministes. Inclut tous les styles (aa, c'est-à-dire sans siècle) et un sous-ensemble de styles (aaaa, c'est-à-dire avec siècle).
  
<sup>2</sup> Les valeurs par défaut (**0** ou **100**, **9** ou **109**, **13** ou **113**, **20** ou **120**, **23** et **21** ou **25** ou **121**) retournent toujours le siècle (aaaa).

<sup>3</sup> Entrée lors de la conversion en données de type **datetime** ; ou encore, sortie lors de la conversion en données caractères.

<sup>4</sup> Conçue pour le langage XML. Pour la conversion de données **datetime** ou **smalldatetime** en données caractères, le format de sortie est décrit dans le tableau précédent.

<sup>5</sup> Hijri est un système de calendrier avec différentes variantes. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] utilise l'algorithme koweitien.

> [!IMPORTANT]
>  Par défaut, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] interprète les années à deux chiffres par rapport à l'année de coupure 2049. Autrement dit, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] interprète l’année à deux chiffres 49 comme étant 2049 et l’année 50 comme étant 1950. De nombreuses applications clientes, comme celles basées sur les objets Automation, utilisent l’année de coupure 2030. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] fournit l’option de configuration Année de coupure à deux chiffres permettant de modifier l’année de coupure utilisée par [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Cela permet de traiter les dates de manière cohérente. Nous vous recommandons d'utiliser la fonctionnalité d'années exprimées sur quatre chiffres.

<sup>6</sup> Prise en charge uniquement lors de la conversion de données caractères en **datetime** ou **smalldatetime**. Lors de la conversion de données caractères représentant des composants de date ou d’heure uniquement en type de données **datetime** ou **smalldatetime**, le composant d’heure non spécifié est défini sur 00:00:00.000 et le composant de date non spécifié est défini sur 1900-01-01.
  
<sup>7</sup> L’indicateur de fuseau horaire facultatif **Z** facilite le mappage des valeurs **datetime** XML comprenant des informations de fuseau horaire aux valeurs **datetime** [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] dépourvues d’informations de fuseau horaire. Z indique le fuseau horaire UTC-0. Le décalage HH:MM, dans la direction + ou -, indique d’autres fuseaux horaires. Par exemple : `2006-12-12T23:45:12-08:00`.
  
Quand vous convertissez des données **smalldatetime** en données caractères, les styles comportant des secondes ou des millisecondes affichent des zéros à ces positions. Quand vous convertissez des valeurs **datetime** ou **smalldatetime**, utilisez une longueur de type de données **char** ou **varchar** appropriée pour tronquer les parties de date inutiles.
  
Quand vous convertissez des données caractères en données **datetimeoffset** avec un style qui inclut une heure, un décalage de fuseau horaire est ajouté au résultat.
  
## <a name="float-and-real-styles"></a>Styles float et real
Quand *expression* est un type **float** ou **real**, *style* peut prendre l’une des valeurs indiquées dans le tableau suivant. Les autres valeurs sont traitées comme étant 0.
  
|Valeur|Output|  
|---|---|
|**0** (valeur par défaut)|6 chiffres maximum. À utiliser pour la notation scientifique si nécessaire.|  
|**1**|Toujours 8 chiffres. À utiliser obligatoirement pour la notation scientifique.|  
|**2**|Toujours 16 chiffres. À utiliser obligatoirement pour la notation scientifique.|  
|**3**|Toujours 17 chiffres. À utiliser pour la conversion sans perte. Avec ce style, la conversion de chaque valeur float ou real distincte en chaîne de caractères distincte est garantie.<br /><br /> **S’applique à :** [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (À partir de [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)]) et [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].|  
|**126, 128, 129**|Incluses pour des raisons d’héritage et peuvent être dépréciées dans une version ultérieure.|  
  
## <a name="money-and-smallmoney-styles"></a>Styles money et smallmoney
Quand *expression* est un type **money** ou **smallmoney**, *style* peut prendre l’une des valeurs indiquées dans le tableau suivant. Les autres valeurs sont traitées comme étant 0.
  
|Valeur|Output|  
|---|---|
|**0** (valeur par défaut)|Pas de virgule tous les trois chiffres à gauche du point décimal, et deux chiffres à droite de celui-ci.<br /><br />Exemple : 4235.98.|  
|**1**|Insertion d’une virgule tous les trois chiffres à gauche du point décimal, et deux chiffres à droite de celui-ci.<br /><br />Exemple : 3,510.92.|  
|**2**|Pas de virgule tous les trois chiffres à gauche du point décimal, et quatre chiffres à droite de celui-ci.<br /><br />Exemple : 4235.9819.|  
|**126**|Équivalent du style 2 lors de la conversion en char(n) ou varchar(n).|  
  
## <a name="xml-styles"></a>styles xml
Quand *expression* est un type **xml**, *style* peut prendre l’une des valeurs indiquées dans le tableau suivant. Les autres valeurs sont traitées comme étant 0.
  
|Valeur|Output|  
|---|---|
|**0** (valeur par défaut)|Utilisez le comportement d’analyse par défaut permettant de supprimer les espaces non significatifs et n’autorisant pas de sous-ensemble DTD interne.<br /><br />**Remarque :** Quand vous effectuez une conversion vers le type de données **xml**, les espaces non significatifs de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sont gérés différemment de ceux dans XML 1.0. Pour plus d’informations, consultez [Créer des instances de données XML](../../relational-databases/xml/create-instances-of-xml-data.md).|  
|**1**|Maintien des espaces non significatifs. Ce paramètre de style définit la gestion par défaut de **xml:space** pour correspondre au comportement de **xml:space="preserve"** .|  
|**2**|Activation du traitement limité de sous-ensembles DTD internes.<br /><br /> Si cette option est activée, le serveur peut utiliser les informations suivantes fournies dans un sous-ensemble DTD interne afin de procéder à des opérations d’analyse autres que celles de validation des données.<br /><br />   - Les valeurs par défaut concernant les attributs sont alors appliquées<br />   - Les références à des entités internes sont résolues et développées<br />   - Le modèle de contenu DTD fait ensuite l’objet d’une vérification syntaxique<br /><br /> L’analyseur ignore les sous-ensembles DTD externes. Il n’évalue pas non plus la déclaration des balises XML au niveau de l’attribut **standalone** pour vérifier s’il a la valeur **yes** ou **no**. Il analyse cependant l’instance XML comme s’il s’agissait d’un document autonome.|  
|**3**|Maintien des espaces non significatifs et activation du traitement limité de sous-ensembles DTD internes.|  
  
## <a name="binary-styles"></a>Styles binaires
Quand *expression* est un type **binary(n)** , **char(n)** , **varbinary(n)** ou **varchar(n)** , *style* peut prendre l’une des valeurs indiquées dans le tableau suivant. Les valeurs de style qui ne sont pas répertoriées dans le tableau retournent une erreur.
  
|Valeur|Output|  
|---|---|
|**0** (valeur par défaut)|Traduit des caractères ASCII en octets binaires ou des octets binaires en caractères ASCII. Chaque caractère ou octet est converti 1:1.<br /><br /> Si *data_type* est un type binaire, les caractères 0x sont ajoutés à gauche du résultat.|  
|**1**, **2**|Si *data_type* est un type binaire, l’expression doit être une expression de caractères. *L’expression* doit être composée d’un nombre **pair** de chiffres hexadécimaux (0, 1, 2, 3, 4, 5, 6, 7, 8, 9, A, B, C, D, E, F, a, b, c, d, e, f). Si *style* a la valeur 1, les caractères 0x doivent être les deux premiers caractères de l’expression. Si l’expression contient un nombre impair de caractères ou si l’un des caractères n’est pas valide, une erreur est générée.<br /><br /> Si la longueur de l’expression convertie dépasse la longueur de *data_type*, le résultat est tronqué à droite.<br /><br /> Les *data_type* de longueur fixe qui sont plus longs que le résultat converti se voient ajouter des zéros à droite du résultat.<br /><br /> Un *data_type* de type caractère nécessite une expression binaire. Chaque caractère binaire est converti en deux caractères hexadécimaux. Supposons que la longueur de l’expression convertie dépasse la longueur de *data_type*. Dans ce cas, elle est tronquée.<br /><br /> Pour un *data_type* de type caractère de taille fixe, si la longueur du résultat converti est inférieure à la longueur de *data_type*, des espaces sont ajoutés à droite de l’expression convertie pour conserver un nombre pair de chiffres hexadécimaux.<br /><br /> Les caractères 0x ne sont pas ajoutés à gauche du résultat converti pour le *style* 2.|  
  
## <a name="implicit-conversions"></a>Conversions implicites
Pour les conversions implicites, il n’est pas nécessaire de spécifier la fonction CAST ou CONVERT. En revanche, les conversions explicites requièrent la spécification de la fonction CAST ou CONVERT. L’illustration ci-dessous reprend toutes les conversions de types de données explicites et implicites autorisées pour les types de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] fournis par le système. Ces types sont notamment **bigint**, **sql_variant** et **xml**. Aucune conversion implicite d’attribution de valeur n’est effectuée à partir du type de données **sql_variant**, mais une conversion implicite vers **sql_variant** existe.
  
> [!TIP]  
> Ce graphique est disponible en fichier PNG téléchargeable dans le [Centre de téléchargement Microsoft](https://www.microsoft.com/download/details.aspx?id=35834).  
  
![Table de conversion de type de données](../../t-sql/data-types/media/lrdatahd.png "Table de conversion de type de données")
  
Le graphique ci-dessus illustre toutes les conversions implicites et explicites autorisées dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], mais le type de données résultant de la conversion dépend de l’opération effectuée :

-  Pour les conversions explicites, l’instruction elle-même détermine le type de données résultant.    
-  Pour les conversions implicites, les instructions d’assignation, comme la définition de la valeur d’une variable ou l’insertion d’une valeur dans une colonne, produisent le type de données défini par la déclaration de variable ou la définition de colonne.    
-  Pour les opérateurs de comparaison ou d’autres expressions, le type de données résultant dépend des règles de [priorité des types de données](../../t-sql/data-types/data-type-precedence-transact-sql.md).

> [!TIP]
> Un exemple pratique sur les [effets de la priorité des types de données dans les conversions](#precedence-example) peut être consulté plus loin dans cette section.

Quand vous effectuez une conversion entre **datetimeoffset** et les types de caractères **char**, **nchar**, **nvarchar** et **varchar**, la partie du décalage de fuseau horaire convertie doit toujours comporter des chiffres doubles pour HH et pour MM. Par exemple, -08:00.
  
> [!NOTE]   
> Les données Unicode utilisent toujours un nombre pair d’octets et vous devez par conséquent faire attention lorsque vous convertissez des données de type **binary** ou **varbinary** vers ou à partir des types de données pris en charge par Unicode. Ainsi, la conversion suivante ne retourne pas la valeur hexadécimale 41. Elle retourne la valeur hexadécimale 4100 : `SELECT CAST(CAST(0x41 AS nvarchar) AS varbinary)`. Pour plus d’informations, consultez [Prise en charge d’Unicode et du classement](../../relational-databases/collations/collation-and-unicode-support.md). 
  
## <a name="large-value-data-types"></a>Types de données de valeur élevée
Les types de données correspondant à des valeurs élevées ont le même comportement de conversion, à la fois implicite et explicite, que leurs équivalents de valeurs plus faibles, notamment en ce qui concerne les types **nvarchar**, **varbinary** et **varchar**. Vous devez cependant prendre en compte les recommandations suivantes :
-   Une conversion du type **image** en **varbinary(max)** et vice versa fonctionne comme une conversion implicite et il en va de même pour les conversions entre les types **text** et **varchar(max)** , et **ntext** et **nvarchar(max)** .  
-   La conversion de types de données de valeur élevée, comme c’est le cas pour le type **varchar(max)** , en type équivalent de valeur plus faible, par exemple **varchar**, reste aussi une conversion implicite, mais la valeur trop grande risque d’être tronquée si elle dépasse la longueur indiquée autorisée pour le type de données de valeur plus modeste.  
-   Les conversions de **nvarchar**, **varbinary** ou **varchar** en leurs équivalents de valeur élevée se déroulent de façon implicite.  
-   Enfin, les conversions du type de données **sql_variant** en type plus conséquent correspondent cependant à une conversion explicite.  
-   Il reste à savoir que les types de données de valeur élevée ne peuvent pas être convertis en **sql_variant**.  
  
Pour plus d’informations sur la conversion à partir du type de données **xml**, consultez [Créer des instances de données XML](../../relational-databases/xml/create-instances-of-xml-data.md).
  
## <a name="xml-data-type"></a>type de données xml
Lors de la conversion explicite ou implicite du type de données **xml** en type de données de chaîne ou binaire, le contenu du type de données **xml** est sérialisé par rapport à un ensemble défini de règles. Pour plus d’informations sur ces règles, consultez [Définir la sérialisation des données XML](../../relational-databases/xml/define-the-serialization-of-xml-data.md). Pour plus d’informations sur la conversion des autres types de données vers le type **xml**, consultez [Créer des instances de données XML](../../relational-databases/xml/create-instances-of-xml-data.md).
  
## <a name="text-and-image-data-types"></a>types de données text et image
Les types de données **text** et **image** ne prennent pas en charge la conversion automatique de types de données. Vous pouvez convertir explicitement des données de type **text** en données caractères, et des données de type **image** en données **binary** ou **varbinary**, mais la longueur maximale est de 8 000 octets. Si vous tentez une conversion incorrecte, par exemple une expression de type caractères incluant des lettres et convertie en type **int**, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] retourne un message d’erreur.
  
## <a name="output-collation"></a>Classement des résultats  
Quand l’entrée et la sortie de CAST ou CONVERT sont des chaînes de caractères, l’entrée et la sortie présentent les mêmes classement et étiquette de classement. Si l'entrée n'est pas une chaîne de caractères, la sortie présente le classement par défaut de la base de données et une étiquette de classement de contrainte par défaut. Pour plus d’informations, consultez [Priorité de classement &#40;Transact-SQL&#41;](../../t-sql/statements/collation-precedence-transact-sql.md).
  
Pour attribuer un classement différent à la sortie, appliquez la clause COLLATE à l'expression de résultat de la fonction CAST ou CONVERT. Par exemple :
  
`SELECT CAST('abc' AS varchar(5)) COLLATE French_CS_AS`
  
## <a name="truncating-and-rounding-results"></a>Troncation et arrondi des résultats
Lors de la conversion d’une expression de type binaire ou caractères (**binary**, **char**, **nchar**, **nvarchar**, **varbinary** ou **varchar**) en une expression d’un type de données différent, l’opération de conversion peut tronquer les données de sortie, afficher uniquement en partie les données de sortie ou retourner une erreur. Ces situations se produisent si le résultat est trop court pour pouvoir être affiché. Les conversions en **binary**, **char**, **nchar**, **nvarchar**, **varbinary** ou **varchar** sont tronquées, sauf pour celles répertoriées dans le tableau suivant.
  
|Du type de données|Au type de données|Résultats|  
|---|---|---|
|**int**, **smallint** ou **tinyint**|**char**|*|  
||**varchar**|*|  
||**nchar**|E|  
||**nvarchar**|E|  
|**money**, **smallmoney**, **numeric**, **decimal**, **float** ou **real**|**char**|E|  
||**varchar**|E|  
||**nchar**|E|  
||**nvarchar**|E|  
  
\* = Expression résultante trop courte pour être affichée<br /><br />E = Erreur retournée car l'expression résultante est trop courte pour être affichée.
  
[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] garantit que seules des conversions en boucle, c’est-à-dire qui convertissent un type de données en un autre puis à nouveau en son type d’origine, restituent les mêmes valeurs d’une version à l’autre de l’application. L'exemple suivant illustre un tel cas de figure :
  
```sql
DECLARE @myval DECIMAL (5, 2);  
SET @myval = 193.57;  
SELECT CAST(CAST(@myval AS VARBINARY(20)) AS DECIMAL(10,5));  
-- Or, using CONVERT  
SELECT CONVERT(DECIMAL(10,5), CONVERT(VARBINARY(20), @myval));  
```  
  
> [!WARNING]  
> N’essayez pas de construire des valeurs **binary**, puis de les convertir en un type de données de catégorie numérique. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ne garantit pas que le résultat d’une conversion de type de données **decimal** ou **numeric** en **binary** est le même entre les différentes versions de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
L'exemple suivant illustre une expression résultante trop petite pour être affichée.
  
```sql
USE AdventureWorks2012;  
GO  
SELECT p.FirstName, p.LastName, SUBSTRING(p.Title, 1, 25) AS Title,
    CAST(e.SickLeaveHours AS char(1)) AS [Sick Leave]  
FROM HumanResources.Employee e JOIN Person.Person p 
    ON e.BusinessEntityID = p.BusinessEntityID  
WHERE NOT e.BusinessEntityID > 5;  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```   
FirstName   LastName      Title   Sick Leave
---------   ------------- ------- --------`
Ken         Sanchez       NULL   *
Terri       Duffy         NULL   *
Roberto     Tamburello    NULL   *
Rob         Walters       NULL   *
Gail        Erickson      Ms.    *

(5 row(s) affected)  
```
  
Quand vous convertissez des types de données qui diffèrent par le nombre de décimales, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] retourne parfois une valeur résultante tronquée et une valeur arrondie à d’autres moments. Le tableau suivant illustre ce comportement.
  
|À partir|À|Comportement|  
|---|---|---|
|**numeric**|**numeric**|Round|  
|**numeric**|**int**|Tronquer|  
|**numeric**|**money**|Round|  
|**money**|**int**|Round|  
|**money**|**numeric**|Round|  
|**float**|**int**|Tronquer|  
|**float**|**numeric**|Round<br /><br /> La conversion des valeurs **float** qui utilisent la notation scientifique en **decimal** ou en **numeric** est limitée à des valeurs d’une précision de 17 chiffres uniquement. Toute valeur avec une précision plus élevée que 17 sera arrondie à zéro.|  
|**float**|**datetime**|Round|  
|**datetime**|**int**|Round|  
  
Par exemple, les valeurs 10,6496 et -10,6496 peuvent être tronquées ou arrondies pendant la conversion en types **int** ou **numeric** :
  
```sql
SELECT  CAST(10.6496 AS INT) as trunc1,
        CAST(-10.6496 AS INT) as trunc2,
        CAST(10.6496 AS NUMERIC) as round1,
        CAST(-10.6496 AS NUMERIC) as round2;
 ```
Les résultats de la requête sont présentés dans le tableau suivant :
 
|trunc1|trunc2|round1|round2|
|---|---|---|---|
|10|-10|11|-11|
 
Lors de la conversion de types de données où le type de données cible compte moins de chiffres décimaux que le type de données source, la valeur obtenue est arrondie. Par exemple, la conversion suivante retourne `$10.3497` :
  
`SELECT CAST(10.3496847 AS money);`
  
[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] retourne un message d’erreur lors de la conversion de données **char**, **nchar**, **nvarchar** ou **varchar** non numériques en données **decimal**, **float**, **int** ou **numeric**. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] retourne également un message d’erreur lorsqu’une chaîne vide (« ») est convertie en données de type **numeric** ou **decimal**.
  
## <a name="certain-datetime-conversions-are-nondeterministic"></a>Certaines conversions de date/heure sont non déterministes.

Les styles pour lesquels la conversion chaîne-datetime est non déterministe sont les suivants :
  
- Tous les styles inférieurs à 100<sup>1</sup>
- 106  
- 107
- 109
- 113
- 130  
  
<sup>1</sup>  À l’exception des styles 20 et 21

Pour plus d’informations, consultez [Conversion non déterministe de chaînes de date littérale en valeurs DATE](../data-types/nondeterministic-convert-date-literals.md).

## <a name="supplementary-characters-surrogate-pairs"></a>Caractères supplémentaires (paires de substitution)
Depuis [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)], si vous utilisez les classements de caractères supplémentaires (SC), une opération de conversion CAST du type **nchar** ou **nvarchar** en type **nchar** ou **nvarchar** de plus petite longueur n’est pas tronquée à l’intérieur d’une paire de substitution. Au lieu de cela, elle est tronquée avant le caractère supplémentaire. Par exemple, le fragment de code suivant laisse `@x` avec seulement `'ab'`. Il n'y a pas assez d'espace pour conserver le caractère supplémentaire.
  
```sql
DECLARE @x NVARCHAR(10) = 'ab' + NCHAR(0x10000);  
SELECT CAST (@x AS NVARCHAR(3));  
```  
  
Lors de l’utilisation de classements SC, le comportement de `CONVERT` est analogue à celui de `CAST`. Pour plus d’informations, consultez [Prise en charge d’Unicode et du classement, Caractères supplémentaires](../../relational-databases/collations/collation-and-unicode-support.md#Supplementary_Characters).
  
## <a name="compatibility-support"></a>Prise en charge de la compatibilité
Dans les versions antérieures de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], le style par défaut pour les opérations CAST et CONVERT sur les types de données **time** et **datetime2** est 121, sauf si l’un des types est utilisé dans une expression de colonne calculée. Pour les colonnes calculées, le style par défaut est 0. Ce comportement influe sur les colonnes calculées lorsqu'elles sont créées, utilisées dans des requêtes impliquant le paramétrage automatique, ou utilisées dans des définitions de contraintes.
  
Quand le [niveau de compatibilité](../../t-sql/statements/alter-database-transact-sql-compatibility-level.md#compatibility-levels-and-database-engine-upgrades) a la valeur 110 et supérieure, le style par défaut pour les opérations CAST et CONVERT sur les types de données **time** et **datetime2** est toujours 121. Si une requête repose sur l’ancien comportement, utilisez un niveau de compatibilité inférieur à 110, ou spécifiez explicitement le style 0 dans la requête affectée.

|Valeur de [niveau de compatibilité](../../t-sql/statements/alter-database-transact-sql-compatibility-level.md#compatibility-levels-and-database-engine-upgrades)|Style par défaut pour CAST et CONVERT<sup>1</sup>|Style par défaut de la colonne calculée|  
|------------|------------|------------|
|< **110**|121|0|  
|> = **110**|121|121|  

<sup>1</sup> Sauf pour les colonnes calculées

La mise à niveau de la base de données vers le niveau de compatibilité 110 et supérieur ne modifie pas les données utilisateur stockées sur le disque. Vous devez corriger manuellement ces données comme il convient. Par exemple, si vous avez utilisé SELECT INTO pour créer une table à partir d’une source contenant une expression de colonne calculée décrite ci-dessus, les données (utilisant le style 0) sont stockées à la place de la définition de colonne calculée. Vous devez mettre à jour manuellement ces données pour qu’elles correspondent au style 121.
  
## <a name="examples"></a><a name="BKMK_examples"></a> Exemples  
  
### <a name="a-using-both-cast-and-convert"></a>R. Utilisation simultanée de CAST et CONVERT  
Ces exemples récupèrent le nom de chaque produit possédant un `3` comme premier chiffre de son prix et convertit sa valeur `ListPrice` en type `int`.
  
```sql
-- Use CAST  
USE AdventureWorks2012;  
GO  
SELECT SUBSTRING(Name, 1, 30) AS ProductName, ListPrice  
FROM Production.Product  
WHERE CAST(ListPrice AS int) LIKE '33%';  
GO  
  
-- Use CONVERT.  
USE AdventureWorks2012;  
GO  
SELECT SUBSTRING(Name, 1, 30) AS ProductName, ListPrice  
FROM Production.Product  
WHERE CONVERT(int, ListPrice) LIKE '33%';  
GO  
```  

[!INCLUDE[ssResult](../../includes/ssresult-md.md)] L’exemple de jeu de résultats est le même pour CAST et CONVERT. 

```
ProductName                    ListPrice
------------------------------ ---------------------
LL Road Frame - Black, 58      337.22
LL Road Frame - Black, 60      337.22
LL Road Frame - Black, 62      337.22
LL Road Frame - Red, 44        337.22
LL Road Frame - Red, 48        337.22
LL Road Frame - Red, 52        337.22
LL Road Frame - Red, 58        337.22
LL Road Frame - Red, 60        337.22
LL Road Frame - Red, 62        337.22
LL Road Frame - Black, 44      337.22
LL Road Frame - Black, 48      337.22
LL Road Frame - Black, 52      337.22
Mountain-100 Black, 38         3374.99
Mountain-100 Black, 42         3374.99
Mountain-100 Black, 44         3374.99
Mountain-100 Black, 48         3374.99
HL Road Front Wheel            330.06
LL Touring Frame - Yellow, 62  333.42
LL Touring Frame - Blue, 50    333.42
LL Touring Frame - Blue, 54    333.42
LL Touring Frame - Blue, 58    333.42
LL Touring Frame - Blue, 62    333.42
LL Touring Frame - Yellow, 44  333.42
LL Touring Frame - Yellow, 50  333.42
LL Touring Frame - Yellow, 54  333.42
LL Touring Frame - Yellow, 58  333.42
LL Touring Frame - Blue, 44    333.42
HL Road Tire                   32.60

(28 rows affected)
```
  
### <a name="b-using-cast-with-arithmetic-operators"></a>B. Utilisation de CAST avec des opérateurs arithmétiques  
Cet exemple illustre le calcul effectué sur une colonne unique (intitulée `Computed`) où le total des ventes de l’année en cours (`SalesYTD`) est divisé par le pourcentage de commission (dont la valeur se trouve dans `CommissionPCT`). Cette valeur est arrondie au nombre entier le plus proche, puis convertie en type de données `int`.
  
```sql
USE AdventureWorks2012;  
GO  
SELECT CAST(ROUND(SalesYTD/CommissionPCT, 0) AS INT) AS Computed  
FROM Sales.SalesPerson   
WHERE CommissionPCT != 0;  
GO  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
```  
Computed
------
379753754
346698349
257144242
176493899
281101272
0  
301872549
212623750
298948202
250784119
239246890
101664220
124511336
97688107

(14 row(s) affected)  
```  
  
### <a name="c-using-cast-to-concatenate"></a>C. Utilisation de CAST pour la concaténation d'expressions  
Cet exemple illustre la concaténation d’expressions de type non-caractères par le biais de CAST. Il utilise la base de données AdventureWorksDW.
  
```sql
SELECT 'The list price is ' + CAST(ListPrice AS VARCHAR(12)) AS ListPrice  
FROM dbo.DimProduct  
WHERE ListPrice BETWEEN 350.00 AND 400.00;  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```  
ListPrice
------------------------
The list price is 357.06
The list price is 364.09
The list price is 364.09
The list price is 364.09
The list price is 364.09  
```  
  
### <a name="d-using-cast-to-produce-more-readable-text"></a>D. Utilisation de CAST pour faciliter la lecture des résultats  
Cet exemple s’appuie sur CAST dans la liste SELECT pour convertir la colonne `Name` en colonne de type **char(10)** . Il utilise la base de données AdventureWorksDW.
  
```sql
SELECT DISTINCT CAST(EnglishProductName AS CHAR(10)) AS Name, ListPrice  
FROM dbo.DimProduct  
WHERE EnglishProductName LIKE 'Long-Sleeve Logo Jersey, M';  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```  
Name        ListPrice
----------  ---------
Long-Sleev  31.2437
Long-Sleev  32.4935
Long-Sleev  49.99  
```  
  
### <a name="e-using-cast-with-the-like-clause"></a>E. Utilisation de CAST avec la clause LIKE  
Cet exemple convertit les valeurs `SalesYTD` de la colonne `money` en type de données `int`, puis en type de données `char(20)` pour que la clause `LIKE` puisse les utiliser.
  
```sql
USE AdventureWorks2012;  
GO  
SELECT p.FirstName, p.LastName, s.SalesYTD, s.BusinessEntityID  
FROM Person.Person AS p   
JOIN Sales.SalesPerson AS s   
    ON p.BusinessEntityID = s.BusinessEntityID  
WHERE CAST(CAST(s.SalesYTD AS INT) AS char(20)) LIKE '2%';  
GO  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```  
FirstName        LastName            SalesYTD         BusinessEntityID
---------------- ------------------- ---------------- -------------
Tsvi             Reiter              2811012.7151      279
Syed             Abbas               219088.8836       288
Rachel           Valdez              2241204.0424      289

(3 row(s) affected)  
```
  
### <a name="f-using-convert-or-cast-with-typed-xml"></a>F. Utilisation de CONVERT ou de CAST avec des données au format XML typé  
Les exemples suivants illustrent l’utilisation de la fonction CONVERT pour convertir des données en XML typé avec les [type et colonnes de données XML &#40;SQL Server&#41;](../../relational-databases/xml/xml-data-type-and-columns-sql-server.md).
  
Cet exemple convertit une chaîne incluant des espaces, du texte et des balises en XML typé, puis supprime tous les espaces non significatifs (correspondant aux espaces délimitant les nœuds) :
  
```sql
SELECT CONVERT(XML, '<root><child/></root>')  
```  
  
Cet exemple convertit une chaîne similaire incluant des espaces, du texte et des balises en XML typé, mais conserve les espaces non significatifs (correspondant aux espaces délimitant les nœuds) :
  
```sql
SELECT CONVERT(XML, '<root>          <child/>         </root>', 1)  
```  
  
Enfin, cet exemple convertit une chaîne similaire incluant des espaces, du texte et des balises en XML typé :
  
```sql
SELECT CAST('<Name><FName>Carol</FName><LName>Elliot</LName></Name>'  AS XML)  
```  
  
Pour obtenir plus d’exemples, consultez [Créer des instances de données XML](../../relational-databases/xml/create-instances-of-xml-data.md).
  
### <a name="g-using-cast-and-convert-with-datetime-data"></a>G. Utilisation de CAST et de CONVERT avec des données de type datetime  
À compter des valeurs `GETDATE()`, cet exemple affiche la date et l’heure actuelles, utilise `CAST` pour convertir la date et l’heure actuelles en type de données caractères, puis utilise `CONVERT` pour afficher la date et l’heure au format `ISO 8601`.
  
```sql
SELECT   
   GETDATE() AS UnconvertedDateTime,  
   CAST(GETDATE() AS NVARCHAR(30)) AS UsingCast,  
   CONVERT(nvarchar(30), GETDATE(), 126) AS UsingConvertTo_ISO8601  ;  
GO  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```  
UnconvertedDateTime     UsingCast              UsingConvertTo_ISO8601
----------------------- ---------------------- ------------------------------
2006-04-18 09:58:04.570 Apr 18 2006  9:58AM    2006-04-18T09:58:04.570

(1 row(s) affected)  
```
  
Cet exemple est plus ou moins l’inverse de l’exemple précédent. Cet exemple affiche une date et une heure sous forme de données caractères, utilise `CAST` pour convertir les données caractères en type de données `datetime`, puis utilise `CONVERT` pour convertir le type de données caractères en type de données `datetime`.
  
```sql
SELECT   
   '2006-04-25T15:50:59.997' AS UnconvertedText,  
   CAST('2006-04-25T15:50:59.997' AS datetime) AS UsingCast,  
   CONVERT(datetime, '2006-04-25T15:50:59.997', 126) AS UsingConvertFrom_ISO8601 ;  
GO  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```  
UnconvertedText         UsingCast               UsingConvertFrom_ISO8601
----------------------- ----------------------- ------------------------
2006-04-25T15:50:59.997 2006-04-25 15:50:59.997 2006-04-25 15:50:59.997

(1 row(s) affected)  
```
  
### <a name="h-using-convert-with-binary-and-character-data"></a>H. Utilisation de CONVERT avec des données binaires et caractères  
Ces exemples montrent les résultats de la conversion de données binaires et caractères en utilisant des styles différents.
  
```sql
--Convert the binary value 0x4E616d65 to a character value.  
SELECT CONVERT(CHAR(8), 0x4E616d65, 0) AS [Style 0, binary to character];  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```  
Style 0, binary to character
----------------------------
Name  

(1 row(s) affected)  
```
 
Cet exemple montre que Style 1 peut forcer la troncation de résultat. Les caractères 0x dans le jeu de résultats forcent la troncation.  
```sql  
SELECT CONVERT(CHAR(8), 0x4E616d65, 1) AS [Style 1, binary to character];  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```  
Style 1, binary to character
------------------------------
0x4E616D

(1 row(s) affected)  
```  
 
Cet exemple montre que Style 2 ne tronque pas le résultat, car les caractères 0x ne sont pas inclus dans le résultat.  
```sql  
SELECT CONVERT(CHAR(8), 0x4E616d65, 2) AS [Style 2, binary to character];  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```  
Style 2, binary to character
------------------------------
4E616D65

(1 row(s) affected)  
```
  
Convertit la valeur de caractère « Name » en valeur binaire.  
```sql
SELECT CONVERT(BINARY(8), 'Name', 0) AS [Style 0, character to binary];  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```  
Style 0, character to binary
----------------------------
0x4E616D6500000000

(1 row(s) affected)  
```
  
```sql
SELECT CONVERT(BINARY(4), '0x4E616D65', 1) AS [Style 1, character to binary];  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```  
Style 1, character to binary
---------------------------- 
0x4E616D65

(1 row(s) affected)  
```  

```sql
SELECT CONVERT(BINARY(4), '4E616D65', 2) AS [Style 2, character to binary];  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```  
Style 2, character to binary  
----------------------------------  
0x4E616D65

(1 row(s) affected)  
```  
  
### <a name="i-converting-date-and-time-data-types"></a>I. Conversion des types de données de date et d'heure  
Cet exemple illustre la conversion des types de données date, time et datetime.
  
```sql
DECLARE @d1 DATE, @t1 TIME, @dt1 DATETIME;  
SET @d1 = GETDATE();  
SET @t1 = GETDATE();  
SET @dt1 = GETDATE();  
SET @d1 = GETDATE();  
-- When converting date to datetime the minutes portion becomes zero.  
SELECT @d1 AS [DATE], CAST (@d1 AS DATETIME) AS [date as datetime];  
-- When converting time to datetime the date portion becomes zero   
-- which converts to January 1, 1900.  
SELECT @t1 AS [TIME], CAST (@t1 AS DATETIME) AS [time as datetime];  
-- When converting datetime to date or time non-applicable portion is dropped.  
SELECT @dt1 AS [DATETIME], CAST (@dt1 AS DATE) AS [datetime as date], 
   CAST (@dt1 AS TIME) AS [datetime as time];  
```  

### <a name="j-using-convert-with-datetime-data-in-different-formats"></a>J. Utilisation de CONVERT avec des données datetime dans différents formats  
À partir des valeurs `GETDATE()`, cet exemple utilise `CONVERT` pour afficher tous les styles de date et d’heure dans la section [Styles de date et d’heure](#date-and-time-styles) de cet article.

|Numéro de format|Exemple de requête|Exemple de résultat|
|--------|------------------------------------|------------------|
|0|`SELECT CONVERT(NVARCHAR, GETDATE(), 0)`|23 août 2019  1:39PM|
|1|`SELECT CONVERT(NVARCHAR, GETDATE(), 1)`|08/23/19|
|2|`SELECT CONVERT(NVARCHAR, GETDATE(), 2)`|19.08.23|
|3|`SELECT CONVERT(NVARCHAR, GETDATE(), 3)`|23/08/19|
|4|`SELECT CONVERT(NVARCHAR, GETDATE(), 4)`|23.08.19|
|5|`SELECT CONVERT(NVARCHAR, GETDATE(), 5)`|23-08-19|
|6|`SELECT CONVERT(NVARCHAR, GETDATE(), 6)`|23 août 19|
|7|`SELECT CONVERT(NVARCHAR, GETDATE(), 7)`|23 août, 19|
|8 ou 24 ou 108|`SELECT CONVERT(NVARCHAR, GETDATE(), 8)`|13:39:17|
|9 ou 109|`SELECT CONVERT(NVARCHAR, GETDATE(), 9)`|23 août 2019  1:39:17:090PM|
|10|`SELECT CONVERT(NVARCHAR, GETDATE(), 10)`|08-23-19|
|11|`SELECT CONVERT(NVARCHAR, GETDATE(), 11)`|19/08/23|
|12|`SELECT CONVERT(NVARCHAR, GETDATE(), 12)`|190823|
|13 ou 113|`SELECT CONVERT(NVARCHAR, GETDATE(), 13)`|23 août 2019 13:39:17:090|
|14 ou 114|`SELECT CONVERT(NVARCHAR, GETDATE(), 14)`|13:39:17:090|
|20 ou 120|`SELECT CONVERT(NVARCHAR, GETDATE(), 20)`|2019-08-23 13:39:17|
|21 ou 25 ou 121|`SELECT CONVERT(NVARCHAR, GETDATE(), 21)`|2019-08-23 13:39:17.090|
|22|`SELECT CONVERT(NVARCHAR, GETDATE(), 22)`|08/23/19  1:39:17 PM|
|23|`SELECT CONVERT(NVARCHAR, GETDATE(), 23)`|2019-08-23|
|101|`SELECT CONVERT(NVARCHAR, GETDATE(), 101)`|08/23/2019|
|102|`SELECT CONVERT(NVARCHAR, GETDATE(), 102)`|2019.08.23|
|103|`SELECT CONVERT(NVARCHAR, GETDATE(), 103)`|23/08/2019|
|104|`SELECT CONVERT(NVARCHAR, GETDATE(), 104)`|23.08.2019|
|105|`SELECT CONVERT(NVARCHAR, GETDATE(), 105)`|23-08-2019|
|106|`SELECT CONVERT(NVARCHAR, GETDATE(), 106)`|23 août 2019|
|107|`SELECT CONVERT(NVARCHAR, GETDATE(), 107)`|23 août 2019|
|110|`SELECT CONVERT(NVARCHAR, GETDATE(), 110)`|08-23-2019|
|111|`SELECT CONVERT(NVARCHAR, GETDATE(), 111)`|2019/08/23|
|112|`SELECT CONVERT(NVARCHAR, GETDATE(), 112)`|20190823|
|113|`SELECT CONVERT(NVARCHAR, GETDATE(), 113)`|23 août 2019 13:39:17.090|
|120|`SELECT CONVERT(NVARCHAR, GETDATE(), 120)`|2019-08-23 13:39:17|
|121|`SELECT CONVERT(NVARCHAR, GETDATE(), 121)`|2019-08-23 13:39:17.090|
|126|`SELECT CONVERT(NVARCHAR, GETDATE(), 126)`|2019-08-23T13:39:17.090|
|127|`SELECT CONVERT(NVARCHAR, GETDATE(), 127)`|2019-08-23T13:39:17.090|
|130|`SELECT CONVERT(NVARCHAR, GETDATE(), 130)`|22 ذو الحجة 1440  1:39:17.090P|
|131|`SELECT CONVERT(NVARCHAR, GETDATE(), 131)`|22/12/1440  1:39:17.090PM|

### <a name="k-effects-of-data-type-precedence-in-allowed-conversions"></a><a name="precedence-example"></a> K. Effets de la priorité des types de données dans les conversions autorisées  
L’exemple suivant définit une variable de type VARCHAR, assigne une valeur de type entier à la variable, puis sélectionne une concaténation de la variable avec une chaîne.

```sql
DECLARE @string VARCHAR(10);
SET @string = 1;
SELECT @string + ' is a string.' AS Result
```

[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```  
Result
-----------------------
1 is a string.
```  

La valeur int 1 a été convertie en VARCHAR.

Cet exemple montre une requête similaire, avec une variable int à la place :

```sql
DECLARE @notastring INT;
SET @notastring = '1';
SELECT @notastring + ' is not a string.' AS Result
```

Dans ce cas, l’instruction SELECT génère l’erreur suivante :

```
Msg 245, Level 16, State 1, Line 3
Conversion failed when converting the varchar value ' is not a string.' to data type int.
```

Afin d’évaluer l’expression `@notastring + ' is not a string.'`, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] doit suivre les règles de priorité des types de données pour effectuer la conversion implicite avant le calcul du résultat de l’expression. Étant donné que le type int a une priorité plus élevée que VARCHAR, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] tente de convertir la chaîne en entier et échoue parce que cette chaîne ne peut pas être convertie en entier. 

Si nous fournissons une chaîne pouvant être convertie, l’instruction réussit, comme dans l’exemple suivant :

```SQL
DECLARE @notastring INT;
SET @notastring = '1';
SELECT @notastring + '1'
```

La chaîne `'1'` pouvant être convertie en valeur entière 1 dans ce cas, l’instruction SELECT retourne la valeur 2. Quand les types de données fournis sont des entiers, l’opérateur + devient l’opérateur mathématique d’addition, plutôt qu’une concaténation de chaînes.

## <a name="examples-sssdwfull-and-sspdw"></a>Exemples : [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] et [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="l-using-cast-and-convert"></a>L. Utilisation de CAST et CONVERT  
Cet exemple récupère le nom de chaque produit possédant un `3` au premier chiffre de son prix et convertit sa valeur `ListPrice` en type **int**. Il utilise la base de données `AdventureWorksDW2016`.
  
```sql
SELECT EnglishProductName AS ProductName, ListPrice  
FROM dbo.DimProduct  
WHERE CAST(ListPrice AS int) LIKE '3%';  
```  
  
Cet exemple montre la même requête, qui utilise CONVERT au lieu de CAST. Il utilise la base de données `AdventureWorksDW2016`.
  
```sql
SELECT EnglishProductName AS ProductName, ListPrice  
FROM dbo.DimProduct  
WHERE CONVERT(INT, ListPrice) LIKE '3%';  
```  
  
### <a name="m-using-cast-with-arithmetic-operators"></a>M. Utilisation de CAST avec des opérateurs arithmétiques  
Cet exemple effectue un calcul de valeur de colonne unique en divisant le prix unitaire du produit (`UnitPrice`) par le pourcentage de remise (`UnitPriceDiscountPct`). Ce résultat est ensuite arrondi au nombre entier le plus proche, puis converti en type de données `int`. Cet exemple utilise la base de données `AdventureWorksDW2016`.
  
```sql
SELECT ProductKey, UnitPrice,UnitPriceDiscountPct,  
       CAST(ROUND (UnitPrice*UnitPriceDiscountPct,0) AS int) AS DiscountPrice  
FROM dbo.FactResellerSales  
WHERE SalesOrderNumber = 'SO47355'   
      AND UnitPriceDiscountPct > .02;  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```  
ProductKey  UnitPrice  UnitPriceDiscountPct  DiscountPrice
----------  ---------  --------------------  -------------
323         430.6445   0.05                  22
213         18.5043    0.05                  1
456         37.4950    0.10                  4
456         37.4950    0.10                  4
216         18.5043    0.05                  1  
```  
  
### <a name="n-using-cast-with-the-like-clause"></a>N. Utilisation de CAST avec la clause LIKE  
Cet exemple convertit la valeur `ListPrice` de la colonne **money** en type **int**, puis en type **char(20)** pour que la clause LIKE puisse l’utiliser. Cet exemple utilise la base de données `AdventureWorksDW2016`.  
  
```sql
SELECT EnglishProductName AS Name, ListPrice  
FROM dbo.DimProduct  
WHERE CAST(CAST(ListPrice AS INT) AS CHAR(20)) LIKE '2%';  
```  
  
### <a name="o-using-cast-and-convert-with-datetime-data"></a>O. Utilisation de CAST et de CONVERT avec des données de type datetime  
Cet exemple affiche la date et l’heure actuelles, utilise CAST pour les convertir en type de données caractères, puis utilise CONVERT pour les afficher au format ISO 8601. Cet exemple utilise la base de données `AdventureWorksDW2016`.
  
```sql
SELECT TOP(1)  
   SYSDATETIME() AS UnconvertedDateTime,  
   CAST(SYSDATETIME() AS NVARCHAR(30)) AS UsingCast,  
   CONVERT(NVARCHAR(30), SYSDATETIME(), 126) AS UsingConvertTo_ISO8601  
FROM dbo.DimCustomer;  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```  
UnconvertedDateTime     UsingCast                     UsingConvertTo_ISO8601  
---------------------   ---------------------------   ---------------------------  
07/20/2010 1:44:31 PM   2010-07-20 13:44:31.5879025   2010-07-20T13:44:31.5879025  
```  
  
Cet exemple est plus ou moins l’inverse de l’exemple précédent. Cet exemple affiche une date et une heure sous forme de données caractères, utilise CAST pour convertir les données caractères en type de données **datetime**, puis utilise CONVERT pour convertir le type de données caractères en type de données **datetime**. Cet exemple utilise la base de données `AdventureWorksDW2016`.
  
```sql
SELECT TOP(1)   
   '2010-07-25T13:50:38.544' AS UnconvertedText,  
CAST('2010-07-25T13:50:38.544' AS DATETIME) AS UsingCast,  
   CONVERT(DATETIME, '2010-07-25T13:50:38.544', 126) AS UsingConvertFrom_ISO8601  
FROM dbo.DimCustomer;  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```  
UnconvertedText         UsingCast               UsingConvertFrom_ISO8601
----------------------- ----------------------- ------------------------
2010-07-25T13:50:38.544 07/25/2010 1:50:38 PM   07/25/2010 1:50:38 PM  
```  

## <a name="see-also"></a>Voir aussi
[Priorité des types de données &#40;Transact-SQL&#41;](../../t-sql/data-types/data-type-precedence-transact-sql.md)       
[Conversion de type de données &#40;moteur de base de données&#41;](../../t-sql/data-types/data-type-conversion-database-engine.md)     
[FORMAT &#40;Transact-SQL&#41;](../../t-sql/functions/format-transact-sql.md)      
[STR &#40;Transact-SQL&#41;](../../t-sql/functions/str-transact-sql.md)     
[SELECT &#40;Transact-SQL&#41;](../../t-sql/queries/select-transact-sql.md)      
[Fonctions système &#40;Transact-SQL&#41;](../../relational-databases/system-functions/system-functions-category-transact-sql.md)      
[Prise en charge d'Unicode et du classement](../../relational-databases/collations/collation-and-unicode-support.md)      
[Rédiger des instructions Transact-SQL internationales](../../relational-databases/collations/write-international-transact-sql-statements.md)       
  
