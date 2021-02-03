---
title: CONTAINS (Transact-SQL) | Microsoft Docs
description: Référence Transact-SQL pour l’élément de langage CONTAINS. Permet de rechercher des mots ou des expressions dans une autre expression.
ms.date: 08/23/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- CONTAINS_TSQL
- CONTAINS
dev_langs:
- TSQL
helpviewer_keywords:
- precise or fuzzy (less precise) matches [full-text search]
- CONTAINS predicate (Transact-SQL)
- conditions [SQL Server], CONTAINS
- fuzzy (less precise) word or phrase search [full-text search]
- word weighting values [full-text search]
- word searches [full-text search]
- weighted values [full-text search]
- LANGUAGE option
- word inflectionally generated from another [full-text search]
- NEAR option [full-text search]
- phrase searches [full-text search]
- word near another word search [full-text search]
- full-text search [SQL Server], searching on a property
- proximity searches [full-text search]
- less precise (fuzzy) searches [full-text search]
- property searching [SQL Server], searching on a property
- inflectional forms [full-text search]
- prefix searches [full-text search]
ms.assetid: 996c72fc-b1ab-4c96-bd12-946be9c18f84
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 680cbfab28064f92d446705de0ca2b2f778735df
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99207791"
---
# <a name="contains-transact-sql"></a>CONTAINS (Transact-SQL)

[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Recherche des concordances précises ou approximatives (moins précises) avec des mots isolés ou des expressions, des termes situés à une certaine distance les uns des autres ou des concordances pondérées dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. CONTAINS est un prédicat utilisé dans la [clause WHERE](../../t-sql/queries/where-transact-sql.md) d’une instruction SELECT [!INCLUDE[tsql](../../includes/tsql-md.md)] pour effectuer une recherche en texte intégral [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] dans des colonnes indexées en texte intégral qui contiennent des types de données caractères.  
  
 CONTAINS peut rechercher :  
  
-   un mot ou une expression ;  
  
-   le préfixe d'un mot ou d'une expression ;  
  
-   un mot proche d'un autre mot ;  
  
-   un mot dérivant d'un autre mot par inflexion (par exemple, le radical « part » a généré par inflexion les formes dérivées partir, départ, partant ou partie) ;  
  
-   un mot synonyme d'un autre mot d'après un dictionnaire des synonymes (par exemple, le mot « métal » peut avoir des synonymes comme « aluminium » et « acier »).  
  
 Pour plus d’informations sur les sortes de recherches en texte intégral prises en charge par [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], consultez [Exécuter une requête avec une recherche en texte intégral](../../relational-databases/search/query-with-full-text-search.md).  
 
 ![Icône du lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql
CONTAINS (   
     {   
        column_name | ( column_list )   
      | *   
      | PROPERTY ( { column_name }, 'property_name' )    
     }   
     , '<contains_search_condition>'  
     [ , LANGUAGE language_term ]  
   )   
  
<contains_search_condition> ::=   
  {   
      <simple_term>   
    | <prefix_term>   
    | <generation_term>   
    | <generic_proximity_term>   
    | <custom_proximity_term>   
    | <weighted_term>   
    }   
  |   
    { ( <contains_search_condition> )   
        [ { <AND> | <AND NOT> | <OR> } ]   
        <contains_search_condition> [ ...n ]   
  }   
<simple_term> ::=   
     { word | "phrase" }  
  
<prefix term> ::=   
  { "word*" | "phrase*" }  
  
<generation_term> ::=   
  FORMSOF ( { INFLECTIONAL | THESAURUS } , <simple_term> [ ,...n ] )   
  
<generic_proximity_term> ::=   
  { <simple_term> | <prefix_term> } { { { NEAR | ~ }   
     { <simple_term> | <prefix_term> } } [ ...n ] }  
  
<custom_proximity_term> ::=   
  NEAR (   
     {  
        { <simple_term> | <prefix_term> } [ ,...n ]  
     |  
        ( { <simple_term> | <prefix_term> } [ ,...n ] )   
      [, <maximum_distance> [, <match_order> ] ]  
     }  
       )   
  
      <maximum_distance> ::= { integer | MAX }  
      <match_order> ::= { TRUE | FALSE }   
  
<weighted_term> ::=   
  ISABOUT   
   ( {   
        {   
          <simple_term>   
        | <prefix_term>   
        | <generation_term>   
        | <proximity_term>   
        }   
      [ WEIGHT ( weight_value ) ]   
      } [ ,...n ]   
   )   
  
<AND> ::=   
  { AND | & }  
  
<AND NOT> ::=   
  { AND NOT | &! }  
  
<OR> ::=   
  { OR | | }  
  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments
 *column_name*  
 Nom d'une colonne d'index de recherche en texte intégral de la table spécifiée dans la clause FROM. Les colonnes peuvent être de type **char**, **varchar**, **nchar**, **nvarchar**, **text**, **ntext**, **image**, **xml**, **varbinary** ou **varbinary(max)** .  
  
 *column_list*  
 Spécifie deux colonnes ou plus, séparées par des virgules. *column_list* doit être mis entre parenthèses. Une seule et même langue doit être utilisée dans toutes les colonnes de *column_list*, sauf si *language_term* est spécifié.  
  
 \*  
 Spécifie que la requête doit effectuer la recherche dans toutes les colonnes indexées en texte intégral de la table spécifiée dans la clause FROM pour la condition de recherche donnée. Les colonnes de la clause CONTAINS doivent provenir d'une table unique qui possède un index de recherche en texte intégral. Une seule et même langue doit être utilisée dans toutes les colonnes de la table, sauf si *language_term* est spécifié.  
  
 PROPERTY ( *column_name* , '*property_name*')  
**S’applique à** : [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] et versions ultérieures. 
  
 Spécifie une propriété du document sur laquelle rechercher la condition de recherche spécifiée.  
  
> [!IMPORTANT]  
>  Pour que la requête retourne des lignes, *property_name* doit être spécifié dans la liste des propriétés de recherche de l’index de recherche en texte intégral, et cet index doit contenir des entrées spécifiques aux propriétés pour *property_name*. Pour plus d’informations, consultez [Rechercher les propriétés du document à l’aide des listes de propriétés de recherche](../../relational-databases/search/search-document-properties-with-search-property-lists.md).  
  
 LANGUAGE *language_term*  
 Langue à utiliser pour l’analyse lexicale, la recherche de radical, les extensions et remplacements du dictionnaire des synonymes, ainsi que le remplacement des mots parasites (ou [mots vides](../../relational-databases/search/configure-and-manage-stopwords-and-stoplists-for-full-text-search.md)) dans le cadre de la requête. Ce paramètre est facultatif.  
  
 Si des documents de langues différentes sont stockés ensemble en tant qu'objets blob dans une colonne unique, l'identificateur de paramètres régionaux (LCID) d'un document donné détermine la langue à utiliser pour l'indexation de son contenu. Quand une requête est effectuée sur la colonne, la spécification de LANGUAGE *language_term* augmente la probabilité d’une meilleure correspondance.  
  
 *language_term* peut être spécifié sous forme de chaîne, d’entier ou de valeur hexadécimale correspondant à l’identificateur de paramètres régionaux (LCID) de la langue. Si une langue est définie avec *language_term*, elle est appliquée à tous les éléments de la condition de recherche. Si aucune valeur n'est définie, la langue du texte intégral de la colonne est utilisée.  
  
 Quand il est spécifié en tant que chaîne, *language_term* correspond à la valeur de colonne **alias** dans l’affichage de compatibilité [sys.syslanguages &#40;Transact-SQL&#41;](../../relational-databases/system-compatibility-views/sys-syslanguages-transact-sql.md). La chaîne doit être placée entre guillemets simples, comme dans '*language_term*'. Quand il est spécifié sous la forme d’un entier, *language_term* est le LCID qui identifie la langue. Quand il est spécifié sous la forme d’une valeur hexadécimale, *language_term* est 0x suivi de la valeur hexadécimale du LCID. La valeur hexadécimale ne doit pas dépasser huit caractères, y compris les zéros non significatifs.  
  
 Si la valeur est au format DBCS (jeu de caractères codés sur deux octets), [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] la convertit au format Unicode.  
  
 Si la langue spécifiée n'est pas valide ou si aucune ressource correspondant à cette langue n'est installée, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] retourne une erreur. Pour utiliser des ressources linguistiques neutres, indiquez 0x0 pour *language_term*.  
  
 \<*contains_search_condition*>  
 Spécifie le texte à rechercher dans *column_name* et les conditions de correspondance à remplir.  
  
*\<contains_search_condition>* est du type **nvarchar**. Une conversion implicite se produit lorsqu'un autre type de données character est utilisé comme entrée. Les chaînes longues de types varchar(max) et nvarchar(max) ne peuvent pas être utilisées. Dans l'exemple suivant, la variable `@SearchWord`, à laquelle est attribuée la valeur `varchar(30)`, provoque une conversion implicite dans le prédicat `CONTAINS`.
  
```sql  
USE AdventureWorks2012;  
GO  
DECLARE @SearchWord VARCHAR(30)  
SET @SearchWord ='performance'  
SELECT Description   
FROM Production.ProductDescription   
WHERE CONTAINS(Description, @SearchWord);  
```  
  
 Étant donné que la détection des paramètres ne fonctionne pas lors de la conversion, utilisez **nvarchar** pour obtenir de meilleures performances. Dans l’exemple, déclarez `@SearchWord` en tant que `nvarchar(30)`.  
  
```sql  
USE AdventureWorks2012;  
GO  
DECLARE @SearchWord NVARCHAR(30)  
SET @SearchWord = N'performance'  
SELECT Description   
FROM Production.ProductDescription   
WHERE CONTAINS(Description, @SearchWord);  
```  
  
 Vous pouvez également utiliser l'indicateur de requête OPTIMIZE FOR lorsqu'un plan non optimal est généré.  
  
 *word*  
 Chaîne de caractères sans espaces ni ponctuation.  
  
 *phrase*  
 Un ou plusieurs mots séparés par des espaces.  
  
> [!NOTE]  
>  Certaines langues, notamment certaines langues asiatiques, peuvent contenir des expressions composées d'un ou de plusieurs mots non séparés par des espaces.  
  
\<simple_term>  
Spécifie une correspondance pour un mot ou une expression exacts. Exemples de termes simples autorisés : « lieu dit », « lieudit » et « Microsoft SQL Server ». Les expressions doivent être mises entre des guillemets doubles (""). Les mots d’une expression doivent apparaître dans l’ordre spécifié dans *\<contains_search_condition>* , tels qu’ils apparaissent dans la colonne de la base de données. La recherche de caractères dans un mot ou une expression ne respecte pas la casse. Dans des colonnes d’index de recherche en texte intégral, les mots parasites ou [mots vides](../../relational-databases/search/configure-and-manage-stopwords-and-stoplists-for-full-text-search.md) (par exemple « un », « et » ou « le ») ne sont pas stockés dans l’index de recherche en texte intégral. Si un mot parasite est utilisé dans la recherche d'un mot unique, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] retourne un message d'erreur indiquant que la requête contient uniquement des mots parasites. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] contient une liste standard des mots parasites dans le répertoire \Mssql\Binn\FTERef de chaque instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 La ponctuation est ignorée. Par conséquent, `CONTAINS(testing, "computer failure")` retourne la ligne ayant la valeur « Où est mon ordinateur (computer) ? Il est en panne (Failure). » Pour plus d’informations sur le comportement des analyseurs lexicaux, consultez [Configurer et gérer les analyseurs lexicaux et générateurs de formes dérivées pour la recherche](../../relational-databases/search/configure-and-manage-word-breakers-and-stemmers-for-search.md).  
  
 \<prefix_term>  
 Précise une correspondance de mots ou d'expressions commençant par le texte spécifié. Placez un terme préfixe entre guillemets doubles ("") et ajoutez un astérisque (\*) avant les guillemets doubles fermants afin que tout le texte commençant par le terme simple spécifié avant l’astérisque soit pris en compte. La clause doit être définie de la manière suivante : `CONTAINS (column, '"text*"')`. L'astérisque correspond à aucun, à un ou à plusieurs caractères (du ou des mots racine dans le mot ou l'expression). Si le texte et l'astérisque ne sont pas délimités par des guillemets doubles, comme dans `CONTAINS (column, 'text*')`, la recherche en texte intégral considère l'astérisque comme un caractère et recherche les correspondances exactes avec `text*`. Le moteur d’indexation et de recherche en texte intégral ne trouve aucun mot contenant l’astérisque (\*), car les analyseurs lexicaux ignorent en général ces caractères.  
  
 Lorsque *\<prefix_term>* est une expression, chaque mot qui la compose est considéré comme un préfixe séparé. Ainsi, une recherche spécifiant le préfixe « contrôle des mots* » trouvera les lignes contenant le texte « contrôle des mots clés », « contrôleur des mots », etc.  
  
 \<generation_term>  
 Précise une correspondance de mots lorsque les termes simples qui s'y trouvent contiennent des variantes du mot initial à rechercher.  
  
 INFLECTIONAL  
 Spécifie que le générateur de formes dérivées dépendant de la langue doit être utilisé sur le terme simple spécifié. Le comportement du générateur de formes dérivées est défini en fonction des règles de racine de chaque langue spécifique. La langue neutre ne possède pas de générateur de formes dérivées associé. La langue des colonnes interrogées est utilisée pour se reporter au générateur de formes dérivées souhaité. Si *language_term* est spécifié, le générateur de formes dérivées correspondant à cette langue est utilisé.  
  
 Un *\<simple_term>* donné inclus dans un *\<generation_term>* ne peut pas correspondre à la fois aux noms et aux verbes.  
  
 THESAURUS  
 Spécifie l'utilisation du dictionnaire des synonymes correspondant à la langue de texte intégral de la colonne ou à la langue spécifiée dans la requête. Les modèles les plus longs de *\<simple_term>* sont recherchés dans le dictionnaire des synonymes et des termes supplémentaires sont créés pour étendre ou remplacer le modèle d’origine. Si aucune correspondance n’est trouvée pour tout ou partie du *\<simple_term>* , la partie sans correspondance est traitée en tant que *simple_term*. Pour plus d’informations sur le dictionnaire des synonymes pour la recherche en texte intégral, consultez [Configurer et gérer les fichiers de dictionnaire des synonymes pour la recherche en texte intégral](../../relational-databases/search/configure-and-manage-thesaurus-files-for-full-text-search.md).  
  
 \<generic_proximity_term>  
 Spécifie une correspondance de mots ou d'expressions qui doit figurer dans le document faisant l'objet d'une recherche.  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)] Nous vous recommandons d’utiliser \<custom_proximity_term>.  
  
 NEAR | ~  
 Indique que le mot ou l'expression qui se trouve de chaque côté de l'opérateur NEAR ou ~ doit figurer dans un document pour qu'une correspondance soit retournée. Vous devez spécifier deux termes de recherche. Un terme de recherche donné peut être un mot unique ou une expression délimitée par des guillemets doubles ("*expression*").  
  
 Plusieurs termes de proximité peuvent être enchaînés, par exemple `a NEAR b NEAR c` ou `a ~ b ~ c`. Les termes de proximité enchaînés doivent tous figurer dans le document pour qu'une correspondance soit retournée.  
  
 Par exemple, `CONTAINS(*column_name*, 'fox NEAR chicken')` et `CONTAINSTABLE(*table_name*, *column_name*, 'fox ~ chicken')` retournent tous les deux les documents de la colonne spécifiée qui contiennent à la fois « fox » et « chicken ». De plus, CONTAINSTABLE retourne un classement pour chaque document en fonction de la proximité de « renard » et de « poulet ». Par exemple, si un document contient la phrase, « Le renard a mangé le poulet », son classement serait élevé car les termes sont plus proches l'un de l'autre que dans d'autres documents.  
  
 Pour plus d’informations sur les termes de proximité génériques, consultez [Recherche de mots dans le voisinage d’autres mots avec NEAR](../../relational-databases/search/search-for-words-close-to-another-word-with-near.md).  
  
 \<custom_proximity_term>  
**S’applique à** : [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] et versions ultérieures.
  
 Spécifie une correspondance de mots ou d'expressions, et la distance maximale éventuellement autorisée entre des termes de recherche. Vous pouvez également spécifier que les termes de recherche doivent être recherchés dans l’ordre exact dans lequel vous les spécifiez (\<match_order>).  
  
 Un terme de recherche donné peut être un mot unique ou une expression délimitée par des guillemets doubles ("*expression*"). Chaque terme spécifié doit figurer dans le document pour qu'une correspondance soit retournée. Vous devez spécifier au moins deux termes de recherche. Le nombre maximal de termes de recherche est de 64.  
  
 Par défaut, le terme de proximité personnalisée retourne toutes les lignes qui contiennent les termes spécifiés indépendamment de la distance qui les sépare et indépendamment de leur ordre. Par exemple, pour correspondre à la requête suivante, un document devrait simplement contenir `term1` et « `term3 term4` », peu importe leur place et leur ordre dans le document :  
  
```sql  
CONTAINS(column_name, 'NEAR(term1,"term3 term4")')  
```  
  
 Les paramètres facultatifs sont les suivants :  
  
 \<maximum_distance>  
 Spécifie la distance maximale autorisée entre les termes de recherche au début et à la fin d'une chaîne afin que cette chaîne soit considérée comme une correspondance.  
  
 *entier*  
 Spécifie un entier positif compris 0 et 4 294 967 295. Cette valeur contrôle le nombre de termes n'appartenant pas à la recherche pouvant se situer entre les premier et dernier termes de la recherche, à l'exclusion de tout autre terme de recherche spécifié supplémentaire.  
  
 Par exemple, la requête suivante recherche les termes `AA` et `BB` qui peuvent être dans n’importe l'ordre, mais dans une distance maximale de cinq.  
  
```sql  
CONTAINS(column_name, 'NEAR((AA,BB),5)')  
```  
  
 La chaîne `AA one two three four five BB` serait une correspondance. Dans l’exemple suivant, la requête spécifie trois termes de recherche (`AA`, `BB` et `CC`) dans une distance maximale de cinq :  
  
```sql  
CONTAINS(column_name, 'NEAR((AA,BB,CC),5)')  
```  
  
 Cette requête correspondrait à la chaîne suivante, dans laquelle la distance totale est de cinq :  
  
 `BB   one two   CC   three four five A  A`  
  
 Notez que le terme de recherche interne, `CC`, n'est pas compté.  
  
 **MAX**  
 Retourne toutes les lignes qui contiennent les termes spécifiés indépendamment de la distance qui les sépare. Il s’agit de la valeur par défaut.  
  
 \<match_order>  
 Spécifie si les termes doivent apparaître dans l'ordre spécifié pour être retournés par une requête de recherche. Si vous spécifiez \<match_order>, vous devez aussi spécifier \<maximum_distance>.  
  
 \<match_order> prend l’une des valeurs suivantes :  
  
 **TRUE**  
 Impose l'ordre spécifié parmi les termes. Par exemple, `NEAR(A,B)` correspondrait uniquement à `A ... B`.  
  
 **FALSE**  
 Ignore l'ordre spécifié. Par exemple, `NEAR(A,B)` correspondrait à la fois à `A ... B` et à `B ... A`.  
  
 Il s’agit de la valeur par défaut.  
  
 Par exemple, le terme de proximité suivant recherche les mots « `Monday` », « `Tuesday` » et « `Wednesday` » dans l'ordre spécifié, indépendamment de la distance qui les sépare :  
  
```sql  
CONTAINS(column_name, 'NEAR ((Monday, Tuesday, Wednesday), MAX, TRUE)')  
```  
  
 Pour plus d’informations sur l’utilisation des termes de proximité personnalisés, consultez [Recherche de mots dans le voisinage d’autres mots avec NEAR](../../relational-databases/search/search-for-words-close-to-another-word-with-near.md).  
  
 \<weighted_term>  
 Précise que les lignes retournées par la requête correspondent à une liste de mots ou d'expressions auxquels une valeur de pondération peut être affectée.  
  
 ISABOUT  
 Spécifie le mot clé *\<weighted_term>* .  
  
 WEIGHT(*weight_value*)  
 Définit une valeur de pondération qui est un nombre compris entre 0,0 et 1,0. Chaque élément de *\<weighted_term>* peut inclure une valeur *weight_value*. *weight_value* permet de changer l’influence de diverses parties d’une requête sur la valeur de classement assignée à chaque ligne correspondant à la requête. WEIGHT n’a pas d’incidence sur les résultats des requêtes CONTAINS, mais impacte le classement dans les requêtes [CONTAINSTABLE](../../relational-databases/system-functions/containstable-transact-sql.md).  
  
> [!NOTE]  
> Le séparateur de décimale est toujours un point, quels que soient les paramètres régionaux du système d'exploitation.  
  
 { AND \| & } \| { AND NOT \| &! } \| { OR \| \| }  
 Spécifie une opération logique entre deux conditions de recherche de contenu.  
  
 { AND \| & }  
 Indique que les deux conditions de recherche de contenu doivent être satisfaites pour obtenir une correspondance. Vous pouvez utiliser une esperluette (&) à la place du mot clé AND pour représenter l’opérateur AND.  
  
 { AND NOT \| &! }  
 Indique que la deuxième condition de recherche ne doit pas être satisfaite pour obtenir une correspondance. Vous pouvez utiliser une esperluette suivie d’un point d’exclamation (&!) à la place du mot clé AND NOT pour représenter l’opérateur AND NOT.  
  
 { OR \| \| }  
 Indique que l'une des deux conditions de recherche de contenu doit être satisfaite pour obtenir une correspondance. Le symbole de barre verticale (|) peut être utilisé à la place du mot clé OR pour représenter l'opérateur OR.  
  
 Lorsque *\<contains_search_condition>* comporte des groupes placés entre parenthèses, ces groupes sont évalués en premier. Une fois ces groupes évalués, les règles suivantes sont appliquées lorsque ces opérateurs logiques sont utilisés dans les conditions de recherche de contenu :  
  
-   NOT est appliqué avant AND.  
  
-   NOT peut uniquement être utilisé après AND, comme dans AND NOT. L'opérateur OR NOT n'est pas autorisé. NOT ne peut pas être spécifié avant le premier terme. Par exemple, `CONTAINS (mycolumn, 'NOT "phrase_to_search_for" ' )` est incorrect.  
  
-   AND est appliqué avant OR.  
  
-   Les opérateurs booléens de même type (AND, OR) sont associatifs et peuvent donc être utilisés dans un ordre quelconque.  
  
 *n*  
 Espace réservé indiquant que plusieurs conditions de recherche CONTAINS peuvent être spécifiées, de même que plusieurs termes dans celles-ci.  
  
## <a name="general-remarks"></a>Remarques d'ordre général  
 Les prédicats et les fonctions de texte intégral s'appliquent à une table unique, ce qui est implicite dans le prédicat FROM. Pour effectuer des recherches sur plusieurs tables, utilisez une table jointe dans votre clause FROM afin de baser votre recherche sur un jeu de résultats qui est le produit de deux tables ou plus.  
  
 Les prédicats de texte intégral ne sont pas autorisés dans la [clause OUTPUT](../../t-sql/queries/output-clause-transact-sql.md) quand le niveau de compatibilité de la base de données a la valeur 100.  
  
## <a name="querying-remote-servers"></a>Interrogation de serveurs distants  
 Vous pouvez utiliser un nom en quatre parties dans le prédicat CONTAINS ou [FREETEXT](../../t-sql/queries/freetext-transact-sql.md) pour faire des requêtes sur les colonnes indexées en texte intégral dans les tables cibles d’un serveur lié. Pour préparer un serveur distant à recevoir des requêtes de texte intégral, créez un index de recherche en texte intégral sur les tables et colonnes cibles du serveur distant, puis ajoutez le serveur distant comme serveur lié.  
  
## <a name="comparison-of-like-to-full-text-search"></a>Comparaison entre LIKE et la recherche en texte intégral  
 Contrairement à la recherche en texte intégral, le prédicat [LIKE](../../t-sql/language-elements/like-transact-sql.md)[!INCLUDE[tsql](../../includes/tsql-md.md)] fonctionne uniquement sur les modèles de caractères. En outre, vous ne pouvez pas utiliser le prédicat LIKE pour interroger des données binaires mises en forme. De plus, une requête LIKE portant sur un important volume de données de texte non structurées est beaucoup plus lente qu'une requête de texte intégral équivalente exécutée sur les mêmes données. Une requête LIKE portant sur des millions de lignes de données de texte peut prendre plusieurs minutes pour retourner un résultat alors qu'une requête de texte intégral retourne en quelques secondes à peine le même résultat, en fonction du nombre de lignes retournées et de leur taille. Une autre considération à prendre en compte est que LIKE effectue uniquement une analyse de modèle simple d'une table entière. Par opposition, une requête de texte intégral est consciente de la langue et applique des transformations spécifiques au niveau de l'index et de l'heure de requête, telles que le filtrage de mots vides et l'ajout au dictionnaire des synonymes et aux formes fléchies. Ces transformations permettent aux requêtes de texte intégral d'améliorer le rappel et le dernier classement de leurs résultats.  
  
## <a name="querying-multiple-columns-full-text-search"></a>Interrogation de plusieurs colonnes (recherche en texte intégral)  
 Vous pouvez interroger plusieurs colonnes en spécifiant une liste de colonnes à rechercher. Les colonnes doivent être issues de la même table.  
  
 Par exemple, la requête CONTAINS suivante recherche le terme `Red` dans les colonnes `Name` et `Color` de la table `Production.Product` utilisée dans l’exemple de base de données [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)].  
  
```sql  
Use AdventureWorks2012;  
GO  
SELECT Name, Color   
FROM Production.Product  
WHERE CONTAINS((Name, Color), 'Red');  
```  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-using-contains-with-simple_term"></a>R. Utilisation de CONTAINS avec \<simple_term>  
 L'exemple ci-dessous recherche tous les produits qui contiennent le mot `$80.99` et qui coûtent `Mountain`.  
  
```sql  
USE AdventureWorks2012;  
GO  
SELECT Name, ListPrice  
FROM Production.Product  
WHERE ListPrice = 80.99  
   AND CONTAINS(Name, 'Mountain');  
GO  
```  
  
### <a name="b-using-contains-and-phrase-with-simple_term"></a>B. Utilisation de CONTAINS et d’une expression avec \<simple_term>  
 L'exemple ci-dessous retourne tous les produits qui contiennent l'expression `Mountain` ou `Road`.  
  
```sql  
USE AdventureWorks2012;  
GO  
SELECT Name  
FROM Production.Product  
WHERE CONTAINS(Name, ' Mountain OR Road ')  
GO  
```  
  
### <a name="c-using-contains-with-prefix_term"></a>C. Utilisation de CONTAINS avec \<prefix_term>  
 L'exemple ci-dessous retourne tous les noms de produits comportant au moins un mot commençant par la chaîne de préfixe dans la colonne `Name`.  
  
```sql  
USE AdventureWorks2012;  
GO  
SELECT Name  
FROM Production.Product  
WHERE CONTAINS(Name, ' "Chain*" ');  
GO  
```  
  
### <a name="d-using-contains-and-or-with-prefix_term"></a>D. Utilisation de CONTAINS et OR avec \<prefix_term>  
 L'exemple ci-dessous retourne toutes les descriptions de catégorie contenant les chaînes `chain` ou `full`.  
  
```sql  
USE AdventureWorks2012;  
GO  
SELECT Name  
FROM Production.Product  
WHERE CONTAINS(Name, '"chain*" OR "full*"');  
GO  
```  
  
### <a name="e-using-contains-with-proximity_term"></a>E. Utilisation de CONTAINS avec \<proximity_term>  
  
**S’applique à** : [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] et versions ultérieures. 
  
 L’exemple suivant recherche, dans la table `Production.ProductReview`, tous les commentaires qui contiennent le mot `bike` dans une distance de 10 termes du mot `control` et dans l’ordre spécifié (autrement dit, où `bike` précède `control`).  
  
```sql  
USE AdventureWorks2012;  
GO  
SELECT Comments  
FROM Production.ProductReview  
WHERE CONTAINS(Comments , 'NEAR((bike,control), 10, TRUE)');  
GO  
```  
  
### <a name="f-using-contains-with-generation_term"></a>F. Utilisation de CONTAINS avec \<generation_term>  
 L'exemple ci-dessous recherche tous les produits comportant des formes dérivées du mot `ride` : riding, ridden, etc.  
  
```sql  
USE AdventureWorks2012;  
GO  
SELECT Description  
FROM Production.ProductDescription  
WHERE CONTAINS(Description, ' FORMSOF (INFLECTIONAL, ride) ');  
GO  
```  
  
### <a name="g-using-contains-with-weighted_term"></a>G. Utilisation de CONTAINS avec \<weighted_term>  
 L'exemple ci-dessous recherche tous les noms de produits contenant les termes « `performance` », « `comfortable` » ou « `smooth` ». Une pondération différente est affectée à chacun des mots.  
  
```sql  
USE AdventureWorks2012;  
GO  
SELECT Description  
FROM Production.ProductDescription  
WHERE CONTAINS(Description, 'ISABOUT (performance weight (.8),   
comfortable weight (.4), smooth weight (.2) )' );  
GO  
```  
  
### <a name="h-using-contains-with-variables"></a>H. Utilisation de CONTAINS avec des variables  
 L'exemple ci-dessous utilise une variable à la place d'un terme de recherche spécifique.  
  
```sql  
USE AdventureWorks2012;  
GO  
DECLARE @SearchWord NVARCHAR(30)  
SET @SearchWord = N'Performance'  
SELECT Description   
FROM Production.ProductDescription   
WHERE CONTAINS(Description, @SearchWord);  
GO  
```  
  
### <a name="i-using-contains-with-a-logical-operator-and"></a>I. Utilisation de CONTAINS avec un opérateur logique (AND)  
 L'exemple suivant utilise la table ProductDescription de la base de données [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] . La requête utilise le prédicat CONTAINS pour rechercher les descriptions dont l’ID de description n’est pas égal à 5 et qui contiennent les mots `Aluminum` et `spindle`. La condition de recherche utilise l'opérateur booléen AND.  
  
```sql  
USE AdventureWorks2012;  
GO  
SELECT Description  
FROM Production.ProductDescription  
WHERE ProductDescriptionID <> 5 AND  
   CONTAINS(Description, 'Aluminum AND spindle');  
GO  
```  
  
### <a name="j-using-contains-to-verify-a-row-insertion"></a>J. Utilisation de CONTAINS pour vérifier une insertion de ligne  
 L'exemple suivant utilise CONTAINS dans le cadre d'une sous-requête SELECT. À partir de la base de données [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)], la requête obtient la valeur de tous les commentaires de la table ProductReview pour un cycle particulier. La condition de recherche utilise l'opérateur booléen AND.  
  
```sql  
USE AdventureWorks2012;  
GO  
INSERT INTO Production.ProductReview   
  (ProductID, ReviewerName, EmailAddress, Rating, Comments)   
VALUES  
  (780, 'John Smith', 'john@fourthcoffee.com', 5,   
'The Mountain-200 Silver from AdventureWorks2008 Cycles meets and exceeds expectations. I enjoyed the smooth ride down the roads of Redmond');  
  
-- Given the full-text catalog for these tables is Adv_ft_ctlg,   
-- with change_tracking on so that the full-text indexes are updated automatically.  
WAITFOR DELAY '00:00:30';     
-- Wait 30 seconds to make sure that the full-text index gets updated.  
  
SELECT r.Comments, p.Name  
FROM Production.ProductReview AS r  
JOIN Production.Product AS p   
    ON r.ProductID = p.ProductID  
    AND r.ProductID = (SELECT ProductID  
FROM Production.ProductReview  
WHERE CONTAINS (Comments,   
    ' AdventureWorks2008 AND   
    Redmond AND   
    "Mountain-200 Silver" '));  
GO  
```  
  
### <a name="k-querying-on-a-document-property"></a>K. Interrogation sur une propriété de document  
  
**S’applique à** : [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] et versions ultérieures. 
  
 La requête suivante effectue une recherche sur une propriété indexée, `Title`, dans la colonne `Document` de la table `Production.Document`. La requête retourne uniquement les documents dont la propriété `Title` contient la chaîne `Maintenance` ou `Repair`.  
  
> [!NOTE]  
>  Pour qu'une recherche de propriété retourne des lignes, le ou les filtres qui analysent la colonne pendant l'indexation doivent extraire la propriété spécifiée. De même, l'index de recherche en texte intégral de la table spécifiée a dû être configuré afin d'inclure la propriété. Pour plus d’informations, consultez [Rechercher les propriétés du document à l’aide des listes de propriétés de recherche](../../relational-databases/search/search-document-properties-with-search-property-lists.md).  
  
```sql  
Use AdventureWorks2012;  
GO  
SELECT Document 
FROM Production.Document  
WHERE CONTAINS(PROPERTY(Document,'Title'), 'Maintenance OR Repair');  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Commencer à utiliser la recherche en texte intégral](../../relational-databases/search/get-started-with-full-text-search.md)   
 [Créer et gérer des catalogues de texte intégral](../../relational-databases/search/create-and-manage-full-text-catalogs.md)   
 [CREATE FULLTEXT CATALOG &#40;Transact-SQL&#41;](../../t-sql/statements/create-fulltext-catalog-transact-sql.md)   
 [CREATE FULLTEXT INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/create-fulltext-index-transact-sql.md)   
 [Créer et gérer des index de recherche en texte intégral](../../relational-databases/search/create-and-manage-full-text-indexes.md)   
 [Exécuter une requête avec une recherche en texte intégral](../../relational-databases/search/query-with-full-text-search.md)   
 [CONTAINSTABLE &#40;Transact-SQL&#41;](../../relational-databases/system-functions/containstable-transact-sql.md)   
 [FREETEXT &#40;Transact-SQL&#41;](../../t-sql/queries/freetext-transact-sql.md)   
 [FREETEXTTABLE &#40;Transact-SQL&#41;](../../relational-databases/system-functions/freetexttable-transact-sql.md)   
 [Exécuter une requête avec une recherche en texte intégral](../../relational-databases/search/query-with-full-text-search.md)   
 [Recherche en texte intégral](../../relational-databases/search/full-text-search.md)   
 [Créer des requêtes de recherche en texte intégral &#40;Visual Database Tools&#41;](../../ssms/visual-db-tools/create-full-text-search-queries-visual-database-tools.md)   
 [WHERE &#40;Transact-SQL&#41;](../../t-sql/queries/where-transact-sql.md)   
 [Rechercher les propriétés du document à l’aide des listes des propriétés de recherche](../../relational-databases/search/search-document-properties-with-search-property-lists.md)  
  
