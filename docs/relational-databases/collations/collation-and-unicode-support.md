---
description: Prise en charge d’Unicode et des classements
title: Prise en charge d’Unicode et des classements | Microsoft Docs
ms.custom: ''
ms.date: 12/05/2019
ms.prod: sql
ms.reviewer: ''
ms.technology: ''
ms.topic: conceptual
helpviewer_keywords:
- binary collations [SQL Server]
- expression-level collations [SQL Server]
- Windows collations [SQL Server]
- globalization [SQL Server], about globalization
- supplementary character support
- code pages [SQL Server], about code pages
- collations [SQL Server], about collations
- Unicode [SQL Server], collations
- database-level collations [SQL Server]
- reading order
- column-level collations
- locales [SQL Server], about locales
- locales [SQL Server]
- code pages [SQL Server]
- SQL Server collations
- UTF-8
- UTF-16
- UTF8
- UTF16
- UCS2
- server-level collations [SQL Server]
ms.assetid: 92d34f48-fa2b-47c5-89d3-a4c39b0f39eb
author: pmasl
ms.author: sstein
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: e2a5f205d7182d1ffedf07b885e7411920ca7b27
ms.sourcegitcommit: 38e055eda82d293bf5fe9db14549666cf0d0f3c0
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/02/2021
ms.locfileid: "99251183"
---
# <a name="collation-and-unicode-support"></a>Prise en charge d’Unicode et des classements
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
Les classements dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] fournissent les règles de tri et les propriétés de respect de la casse et des accents pour vos données. Les classements utilisés avec les données de type caractère, tels que **char** et **varchar**, déterminent la page de codes et les caractères correspondants qui peuvent être représentés pour ce type de données. 

Qu’il s’agisse d’installer une nouvelle instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], de restaurer une sauvegarde de base de données ou de connecter un serveur à des bases de données clientes, vous devez bien comprendre les exigences en termes de paramètres régionaux, d’ordre de tri et de respect de la casse et des accents des données que vous utilisez. Pour lister les classements disponibles sur votre instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], consultez [sys.fn_helpcollations (Transact-SQL)](../../relational-databases/system-functions/sys-fn-helpcollations-transact-sql.md).    
    
Quand vous sélectionnez un classement pour votre serveur, base de données, colonne ou expression, vous assignez certaines caractéristiques à vos données. Ces caractéristiques affectent les résultats de nombreuses opérations dans la base de données. Par exemple, quand vous construisez une requête avec `ORDER BY`, l’ordre de tri de votre jeu de résultats peut dépendre du classement qui est appliqué à la base de données ou qui est stipulé dans une clause `COLLATE` au niveau de l’expression de la requête.    
    
Pour exploiter au mieux la prise en charge des classements dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], vous devez comprendre les termes qui sont définis dans cette rubrique et la relation qu’ils entretiennent avec les caractéristiques de vos données.    
    
##  <a name="collation-terms"></a><a name="Terms"></a> Termes de classement    
    
-   [Classement](#Collation_Defn) 
    - [Ensembles de classements](#Collation_sets)
    - [Niveaux de classement](#Collation_levels)
-   [Paramètres régionaux](#Locale_Defn)    
-   [Page de codes](#Code_Page_Defn)    
-   [Ordre de tri](#Sort_Order_Defn)    
    
###  <a name="collation"></a><a name="Collation_Defn"></a> Classement    
Un classement désigne les modèles binaires qui représentent chaque caractère dans un jeu de données. Les classements déterminent également les règles de tri et de comparaison des données. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] prend en charge le stockage d’objets ayant des classements différents dans une même base de données. Pour les colonnes non-Unicode, le paramètre de classement spécifie la page de codes pour les données et les caractères qui peuvent être représentés. Les données que vous déplacez entre des colonnes non-Unicode doivent être converties de la page de codes source vers la page de codes de destination.    
    
Le résultat d'une instruction[!INCLUDE[tsql](../../includes/tsql-md.md)] peut varier lorsque cette dernière est exécutée dans un contexte réunissant plusieurs bases de données dont chacune a un paramètre de classement différent. Dans la mesure du possible, choisissez un classement normalisé pour votre organisation. De cette manière, vous n’avez pas à spécifier le classement dans chaque caractère ou expression Unicode. Si vous devez utiliser des objets qui ont des paramètres de classement et de page de codes différents, codez vos requêtes conformément aux règles de priorité des classements. Pour plus d’informations, consultez [Priorité de classement (Transact-SQL)](../../t-sql/statements/collation-precedence-transact-sql.md).    
    
Les options associées à un classement sont le respect de la casse, le respect des accents, le respect du jeu de caractères Kana, le respect de la largeur et le respect du sélecteur de variante. [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)] introduit une option supplémentaire pour l’encodage [UTF-8](https://www.wikipedia.org/wiki/UTF-8). 

Vous pouvez spécifier ces options en les ajoutant au nom du classement. Par exemple, le classement **Japanese_Bushu_Kakusu_100_CS_AS_KS_WS_UTF8** respecte la casse, les accents, le jeu de caractères Kana et la largeur, et il est encodé en UTF-8. Autre exemple : le classement **Japanese_Bushu_Kakusu_140_CI_AI_KS_WS_VSS** ne respecte pas la casse, ne respecte pas les accents, respecte le jeu de caractères Kana, respecte la largeur, respecte le sélecteur de variante et utilise un encodage non-Unicode. 

Le comportement associé à ces différentes options est décrit dans le tableau suivant :    
    
|Option|Description|    
|------------|-----------------|    
|Respecter la casse (\_CS)|Fait la distinction entre les majuscules et les minuscules. Si cette option est sélectionnée, les minuscules sont triées avant leurs équivalents majuscules. Si cette option n’est pas sélectionnée, le classement ne respecte pas la casse. Dans ce cas, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] considère que les versions en majuscules et en minuscules des lettres sont identiques dans les opérations de tri. Pour sélectionner explicitement le non-respect de la casse, spécifiez \_CI.|   
|Respecter les accents (\_AS)|Fait la distinction entre les caractères accentués et non accentués. Par exemple, « a » n’est pas équivalent à « ấ ». Si cette option n’est pas sélectionnée, le classement ne respecte pas les accents. Dans ce cas, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] considère que la version accentuée et la version non accentuée d’une même lettre sont identiques dans les opérations de tri. Pour sélectionner explicitement le non-respect des accents, spécifiez \_AI.|    
|Respecter le jeu de caractères Kana (\_KS)|Fait la distinction entre les deux types de caractères japonais Kana : Hiragana et Katakana. Si cette option n’est pas sélectionnée, le classement ne respecte pas les caractères Kana. Dans ce cas, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] considère que les caractères Hiragana et Katakana sont identiques dans les opérations de tri. L’omission de cette option est le seul moyen de spécifier le non-respect du jeu de caractères Kana.|   
|Respecter la largeur (\_WS)|Fait la différence entre les caractères pleine largeur et demi-largeur. Si cette option n’est pas sélectionnée, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] considère que la représentation pleine largeur et demi-largeur d’un même caractère sont identiques dans les opérations de tri. L'omission de cette option est le seul moyen de spécifier le non-respect de la largeur.|  
|Respecter le sélecteur de variante (\_VSS)|Fait la distinction entre différents sélecteurs de variante idéographiques dans les classements japonais **Japanese_Bushu_Kakusu_140** et **Japanese_XJIS_140**, qui sont introduits dans [!INCLUDE [sssql17-md](../../includes/sssql17-md.md)]. Une séquence de variantes se compose d’un caractère de base et d’un sélecteur de variante supplémentaire. Si l’option \_VSS n’est pas sélectionnée, le classement ne respecte pas le sélecteur de variante, qui n’est pas non plus pris en compte dans la comparaison. Autrement dit, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] considère comme identiques pour les tris les caractères basés sur le même caractère de base avec différents sélecteurs de variante. Pour plus d’informations, consultez [Unicode Ideographic Variation Database](https://www.unicode.org/reports/tr37/).<br/><br/> Les classements qui respectent le sélecteur de variante (\_VSS) ne sont pas pris en charge par les index de recherche en texte intégral, qui ne gèrent que les options Respecter les accents (\_AS), Respecter le jeu de caractères Kana (\_KS) et Respecter la largeur (\_WS). Les moteurs XML et CLR de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ne prennent pas en charge les sélecteurs de variante (\_VSS).|      
|Binaire (\_BIN)<sup>1</sup>|Trie et compare les données dans les tables [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en fonction des modèles de bits définis pour chaque caractère. L’ordre de tri binaire respecte la casse et les accents. Il s'agit aussi de l'ordre de tri le plus rapide. Pour plus d’informations, consultez la section [Classements binaires](#Binary-collations) de cet article.|      
|Point de code binaire (\_BIN2)<sup>1</sup> | Trie et compare les données des tables [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en fonction des points de code Unicode pour les données Unicode. Pour les données non-Unicode, le point de code binaire utilise des comparaisons identiques à celles utilisées pour les tris binaires.<br/><br/> L’utilisation d’un ordre de tri de point de code binaire présente l’avantage de ne devoir retrier les données dans les applications qui comparent les données triées de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Par conséquent, un ordre de tri de point de code binaire simplifie le développement des applications et permet d’améliorer les performances. Pour plus d’informations, consultez la section [Classements binaires](#Binary-collations) de cet article.|
|UTF-8 (\_UTF8)|Permet le stockage des données encodées en UTF-8 dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Si cette option n’est pas sélectionnée, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] utilise le format d’encodage non-Unicode par défaut pour les types de données applicables. Pour plus d’informations, consultez la section [Prise en charge d’UTF-8](#utf8) de cet article.| 

<sup>1</sup> Si Binaire ou Point de code binaire est sélectionné, les options Respecter la casse (\_CS), Respecter les accents (\_AS), Respecter les caractères Kana (\_KS) et Respecter la largeur (\_WS) ne sont pas disponibles.      

#### <a name="examples-of-collation-options"></a>Exemples d’options de classement
Chaque classement se présente comme une série de suffixes permettant de définir le respect de la casse, des accents, de la largeur ou des caractères Kana. Les exemples suivants décrivent le comportement de l’ordre de tri selon différentes combinaisons de suffixes.

|Suffixe de classement Windows|Description de l'ordre de tri|
|------------|-----------------| 
|\_BIN<sup>1</sup>|Tri binaire|
|\_BIN2<sup>1, 2</sup>|Ordre de tri de point de code binaire|
|\_CI_AI<sup>2</sup>|Non-respect de la casse, non-respect des accents, non-respect des caractères Kana, non-respect de la largeur|
|\_CI_AI_KS<sup>2</sup>|Non-respect de la casse, non-respect des accents, respect des caractères Kana, non-respect de la largeur|
|\_CI_AI_KS_WS<sup>2</sup>|Non-respect de la casse, non-respect des accents, respect des caractères Kana, respect de la largeur|
|\_CI_AI_WS<sup>2</sup>|Non-respect de la casse, non-respect des accents, non-respect des caractères Kana, respect de la largeur|
|\_CI_AS<sup>2</sup>|Non-respect de la casse, respect des accents, non-respect des caractères Kana, non-respect de la largeur|
|\_CI_AS_KS<sup>2</sup>|Non-respect de la casse, respect des accents, respect des caractères Kana, non-respect de la largeur|
|\_CI_AS_KS_WS<sup>2</sup>|Non-respect de la casse, respect des accents, respect des caractères Kana, respect de la largeur|
|\_CI_AS_WS<sup>2</sup>|Non-respect de la casse, respect des accents, non-respect des caractères Kana, respect de la largeur|
|\_CS_AI<sup>2</sup>|Respect de la casse, non-respect des accents, non-respect des caractères Kana, non-respect de la largeur|
|\_CS_AI_KS<sup>2</sup>|Respect de la casse, non-respect des accents, respect des caractères Kana, non-respect de la largeur|
|\_CS_AI_KS_WS<sup>2</sup>|Respect de la casse, non-respect des accents, respect des caractères Kana, respect de la largeur|
|\_CS_AI_WS<sup>2</sup>|Respect de la casse, non-respect des accents, non-respect des caractères Kana, respect de la largeur|
|\_CS_AS<sup>2</sup>|Respect de la casse, respect des accents, non-respect des caractères Kana, non-respect de la largeur|
|\_CS_AS_KS<sup>2</sup>|Respect de la casse, respect des accents, respect des caractères Kana, non-respect de la largeur|
|\_CS_AS_KS_WS<sup>2</sup>|Respect de la casse, respect des accents, respect des caractères Kana, respect de la largeur|
|\_CS_AS_WS<sup>2</sup>|Respect de la casse, respect des accents, non-respect des caractères Kana, respect de la largeur|

<sup>1</sup> Si Binaire ou Point de code binaire est sélectionné, les options Respecter la casse (\_CS), Respecter les accents (\_AS), Respecter les caractères Kana (\_KS) et Respecter la largeur (\_WS) ne sont pas disponibles.    

<sup>2</sup> L’ajout de l’option UTF-8 (\_UTF8) vous permet d’encoder les données Unicode avec UTF-8. Pour plus d’informations, consultez la section [Prise en charge d’UTF-8](#utf8) de cet article. 

### <a name="collation-sets"></a><a name="Collation_sets"></a> Ensembles de classements

[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] prend en charge les ensembles de classement suivants :    

-  [Classements Windows](#Windows-collations)
-  [Classements binaires](#Binary-collations)
-  [Classements SQL Server](#SQL-collations)
    
#### <a name="windows-collations"></a><a name="Windows-collations"></a> Classements Windows    
Les classements Windows définissent les règles de stockage des données de type caractère selon les paramètres régionaux système Windows associés. Pour un classement Windows, vous pouvez implémenter une comparaison de données non-Unicode via le même algorithme que pour les données Unicode. Les règles de classement Windows de base spécifient l’alphabet ou la langue utilisée pour le tri du dictionnaire. Les règles spécifient également la page de codes utilisée pour stocker les données de type caractère non-Unicode. Les tris Unicode et non-Unicode sont compatibles avec les comparaisons de chaînes dans une version particulière de Windows. Les types de données sont ainsi cohérents dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], ce qui permet aux développeurs de trier les chaînes dans leurs applications en appliquant les mêmes règles que celles utilisées par [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Pour plus d’informations, consultez [Nom de classement Windows (Transact-SQL)](../../t-sql/statements/windows-collation-name-transact-sql.md).    
    
#### <a name="binary-collations"></a><a name="Binary-collations"></a> Classements binaires    
Les classements binaires trient les données en fonction de la séquence des valeurs codées qui sont définies par les paramètres régionaux et le type de données. Ils respectent la casse. Un classement binaire dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] définit les paramètres régionaux et la page de codes ANSI à utiliser. Cela applique un ordre de tri binaire. Parce qu’ils sont relativement simples, les classements binaires aident à améliorer les performances de l’application. Pour les types de données non-Unicode, les comparaisons de données sont basées sur les points de code qui sont définis dans la page de codes ANSI. Pour les données de type Unicode, les comparaisons de données se basent sur les points de code Unicode. Pour le classement binaire des types de données Unicode, les paramètres régionaux (la langue) ne sont pas pris en compte dans les tris de données. Par exemple, **Latin_1_General_BIN** et **Japanese_BIN** produisent des résultats de tri identiques quand ils sont utilisés avec des données Unicode. Pour plus d’informations, consultez [Nom de classement Windows (Transact-SQL)](../../t-sql/statements/windows-collation-name-transact-sql.md).   
    
Il existe deux types de classements binaires dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] :

-  Les classements **BIN** précédents, qui effectuaient une comparaison incomplète de point de code à point de code pour les données Unicode. Ces classements binaires hérités comparaient le premier caractère comme WCHAR, suivi d’une comparaison octet par octet. Dans un classement BIN, seul le premier caractère est trié selon le point de code. Les autres caractères sont triés en fonction de leurs valeurs d’octet.

-  Les classements **BIN2** plus récents, qui implémentent une comparaison de point de code pure. Dans un classement BIN2, tous les caractères sont triés en fonction de leurs points de code. En raison de l’architecture little endian de la plateforme Intel, les caractères de code Unicode sont toujours stockés avec les octets inversés.     
    
#### <a name="sql-server-collations"></a><a name="SQL-collations"></a> Classements SQL Server    
Les classements de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (SQL_\*) offrent la compatibilité des ordres de tri avec les versions antérieures de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Les règles de tri du dictionnaire pour les données non-Unicode ne sont pas compatibles avec les routines de tri fournies par les systèmes d’exploitation Windows. Toutefois, le tri de données Unicode est compatible avec une version particulière de règles de tri Windows. Comme les classements [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] utilisent des règles de comparaison différentes pour les données Unicode et non-Unicode, vous pouvez obtenir des résultats différents pour des comparaisons des mêmes données, selon le type de données sous-jacent. Pour plus d’informations, consultez [Nom du classement SQL Server (Transact-SQL)](../../t-sql/statements/sql-server-collation-name-transact-sql.md). 

Lors de l’installation de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], le classement par défaut est déterminé par les paramètres régionaux du système d’exploitation. Vous pouvez modifier le classement au niveau du serveur pendant l’installation ou en modifiant les paramètres régionaux du système d’exploitation avant l’installation. Pour garantir la compatibilité ascendante, le classement par défaut est défini d’après la version disponible la plus ancienne associée à chaque ensemble de paramètres régionaux spécifiques. Par conséquent, il ne s’agit pas toujours du classement recommandé. Pour tirer pleinement parti des fonctionnalités de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], modifiez les paramètres d’installation par défaut de façon à utiliser les classements Windows. Par exemple, pour les paramètres régionaux du système d’exploitation « Anglais (États-Unis) » (page de codes 1252), le classement par défaut lors de l’installation est **SQL_Latin1_General_CP1_CI_AS** et il peut être remplacé par le classement Windows le plus proche **Latin1_General_100_CI_AS_SC** équivalent.
    
> [!NOTE]    
> Quand vous mettez à niveau une instance en anglais de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], vous pouvez spécifier les classements [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (SQL_\*) pour la compatibilité avec les instances [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] existantes. Le classement par défaut d’une instance [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] étant défini au cours de la procédure d’installation, assurez-vous de spécifier soigneusement les paramètres de classement dans les cas suivants :    
>     
> -   Votre code d'application dépend du comportement des classements [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] précédents.    
> -   Vous devez stocker des données de caractères de plusieurs langues.    
    
### <a name="collation-levels"></a><a name="Collation_levels"></a> Niveaux de classement
Les paramétrages des classements sont pris en charge aux niveaux suivants d'une instance [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]:    

-  [Classements au niveau du serveur](#Server-level-collations)
-  [Classements au niveau de la base de données](#Database-level-collations)
-  [Classements au niveau de la colonne](#Column-level-collations)
-  [Classements au niveau de l’expression](#Expression-level-collations)

#### <a name="server-level-collations"></a><a name="Server-level-collations"></a> Classements au niveau du serveur   
Le classement par défaut du serveur est défini lors de l’installation de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], et il devient le classement par défaut des bases de données système et de toutes les bases de données utilisateur. 

Le tableau suivant montre les désignations de classement par défaut, telles qu’elles sont déterminées par les paramètres régionaux du système d’exploitation, avec leurs identificateurs de code de langue (LCID) Windows et SQL :

|Paramètres régionaux Windows|LCID Windows|LCID SQL|Classement par défaut|
|---------------|---------|---------|---------------|
|Afrikaans (Afrique du Sud)|0x0436|0x0409|Latin1_General_CI_AS|
|Albanais (Albanie)|0x041c|0x041c|Albanian_CI_AS|
|Alsacien (France)|0x0484|0x0409|Latin1_General_CI_AS|
|Amharique (Éthiopie)|0x045e|0x0409|Latin1_General_CI_AS|
|Arabe (Algérie)|0x1401|0x0401|Arabic_CI_AS|
|Arabe (Bahreïn)|0x3c01|0x0401|Arabic_CI_AS|
|Arabe (Égypte)|0x0c01|0x0401|Arabic_CI_AS|
|Arabe (Irak)|0x0801|0x0401|Arabic_CI_AS|
|Arabe (Jordanie)|0x2c01|0x0401|Arabic_CI_AS|
|Arabe (Koweït)|0x3401|0x0401|Arabic_CI_AS|
|Arabe (Liban)|0x3001|0x0401|Arabic_CI_AS|
|Arabe (Libye)|0x1001|0x0401|Arabic_CI_AS|
|Arabe (Maroc)|0x1801|0x0401|Arabic_CI_AS|
|Arabe (Oman)|0x2001|0x0401|Arabic_CI_AS|
|Arabe (Qatar)|0x4001|0x0401|Arabic_CI_AS|
|Arabe (Arabie saoudite)|0x0401|0x0401|Arabic_CI_AS|
|Arabe (Syrie)|0x2801|0x0401|Arabic_CI_AS|
|Arabe (Tunisie)|0x1c01|0x0401|Arabic_CI_AS|
|Arabe (E.A.U.)|0x3801|0x0401|Arabic_CI_AS|
|Arabe (Yémen)|0x2401|0x0401|Arabic_CI_AS|
|Arménien (Arménie)|0x042b|0x0419|Latin1_General_CI_AS|
|Assamais (Inde)|0x044d|0x044d|Non disponible au niveau du serveur|
|Azerbaïdjanais (Azerbaïdjan, cyrillique)|0x082c|0x082c|Déprécié, non disponible au niveau serveur|
|Azerbaïdjanais (Azerbaïdjan, latin)|0x042c|0x042c|Déprécié, non disponible au niveau serveur|
|Bachkir (Russie)|0x046d|0x046d|Latin1_General_CI_AI|
|Basque (Basque)|0x042d|0x0409|Latin1_General_CI_AS|
|Biélorusse (Bélarus)|0x0423|0x0419|Cyrillic_General_CI_AS|
|Bengali (Bangladesh)|0x0845|0x0445|Non disponible au niveau du serveur|
|Bengali (India)|0x0445|0x0439|Non disponible au niveau du serveur|
|Bosniaque (Bosnie-Herzégovine, cyrillique)|0x201a|0x201a|Latin1_General_CI_AI|
|Bosniaque (Bosnie-Herzégovine, latin)|0x141a|0x141a|Latin1_General_CI_AI|
|Breton (France)|0x047e|0x047e|Latin1_General_CI_AI|
|Bulgare (Bulgarie)|0x0402|0x0419|Cyrillic_General_CI_AS|
|Catalan (Catalogne)|0x0403|0x0409|Latin1_General_CI_AS|
|Chinois (Hong Kong R.A.S., RPC)|0x0c04|0x0404|Chinese_Taiwan_Stroke_CI_AS|
|Chinese (Macao (R.A.S.))|0x1404|0x1404|Latin1_General_CI_AI|
|Chinois (Macao, R.A.S.)|0x21404|0x21404|Latin1_General_CI_AI|
|Chinois (RPC)|0x0804|0x0804|Chinese_PRC_CI_AS|
|Chinois (RPC)|0x20804|0x20804|Chinese_PRC_Stroke_CI_AS|
|Chinese (Singapore)|0x1004|0x0804|Chinese_PRC_CI_AS|
|Chinese (Singapore)|0x21004|0x20804|Chinese_PRC_Stroke_CI_AS|
|Chinois (Taïwan)|0x30404|0x30404|Chinese_Taiwan_Bopomofo_CI_AS|
|Chinois (Taïwan)|0x0404|0x0404|Chinese_Taiwan_Stroke_CI_AS|
|Corse (France)|0x0483|0x0483|Latin1_General_CI_AI|
|Croate (Bosnie-Herzégovine, latin)|0x101a|0x041a|Croatian_CI_AS|
|Croate (Croatie)|0x041a|0x041a|Croatian_CI_AS|
|Tchèque (République tchèque)|0x0405|0x0405|Czech_CI_AS|
|Danois (Danemark)|0x0406|0x0406|Danish_Norwegian_CI_AS|
|Dari (Afghanistan)|0x048c|0x048c|Latin1_General_CI_AI|
|Maldivien (Maldives)|0x0465|0x0465|Non disponible au niveau du serveur|
|Néerlandais (Belgique)|0x0813|0x0409|Latin1_General_CI_AS|
|Néerlandais (Pays-Bas)|0x0413|0x0409|Latin1_General_CI_AS|
|Anglais (Australie)|0x0c09|0x0409|Latin1_General_CI_AS|
|Anglais (Belize)|0x2809|0x0409|Latin1_General_CI_AS|
|Anglais (Canada)|0x1009|0x0409|Latin1_General_CI_AS|
|Anglais (Caraïbes)|0x2409|0x0409|Latin1_General_CI_AS|
|Anglais (Inde)|0x4009|0x0409|Latin1_General_CI_AS|
|Anglais (Irlande)|0x1809|0x0409|Latin1_General_CI_AS|
|Anglais (Jamaïque)|0x2009|0x0409|Latin1_General_CI_AS|
|Anglais (Malaisie)|0x4409|0x0409|Latin1_General_CI_AS|
|Anglais (Nouvelle-Zélande)|0x1409|0x0409|Latin1_General_CI_AS|
|Anglais (Philippines)|0x3409|0x0409|Latin1_General_CI_AS|
|Anglais (Singapour)|0x4809|0x0409|Latin1_General_CI_AS|
|Anglais (Afrique du Sud)|0x1c09|0x0409|Latin1_General_CI_AS|
|Anglais (Trinidad-et-Tobago)|0x2c09|0x0409|Latin1_General_CI_AS|
|Anglais (Royaume-Uni)|0x0809|0x0409|Latin1_General_CI_AS|
|Anglais (États-Unis)|0x0409|0x0409|SQL_Latin1_General_CP1_CI_AS|
|Anglais (Zimbabwe)|0x3009|0x0409|Latin1_General_CI_AS|
|Estonien (Estonie)|0x0425|0x0425|Estonian_CI_AS|
|Féroïen (Îles Féroé)|0x0438|0x0409|Latin1_General_CI_AS|
|Filipino (Philippines)|0x0464|0x0409|Latin1_General_CI_AS|
|Finnois (Finlande)|0x040b|0x040b|Finnish_Swedish_CI_AS|
|Français (Belgique)|0x080c|0x040c|French_CI_AS|
|Français (Canada)|0x0c0c|0x040c|French_CI_AS|
|Français (France)|0x040c|0x040c|French_CI_AS|
|Français (Luxembourg)|0x140c|0x040c|French_CI_AS|
|Français (Monaco)|0x180c|0x040c|French_CI_AS|
|Français (Suisse)|0x100c|0x040c|French_CI_AS|
|Frison (Pays-Bas)|0x0462|0x0462|Latin1_General_CI_AI|
|Galicien|0x0456|0x0409|Latin1_General_CI_AS|
|Géorgien (Géorgie)|0x10437|0x10437|Georgian_Modern_Sort_CI_AS|
|Géorgien (Géorgie)|0x0437|0x0419|Latin1_General_CI_AS|
|Allemand - Annuaire téléphonique (DIN)|0x10407|0x10407|German_PhoneBook_CI_AS|
|Allemand (Autriche)|0x0c07|0x0409|Latin1_General_CI_AS|
|Allemand (Allemagne)|0x0407|0x0409|Latin1_General_CI_AS|
|Allemand (Liechtenstein)|0x1407|0x0409|Latin1_General_CI_AS|
|Allemand (Luxembourg)|0x1007|0x0409|Latin1_General_CI_AS|
|Allemand (Suisse)|0x0807|0x0409|Latin1_General_CI_AS|
|Grec (Grèce)|0x0408|0x0408|Greek_CI_AS|
|Groenlandais (Groenland)|0x046f|0x0406|Danish_Norwegian_CI_AS|
|Goudjrati (Inde)|0x0447|0x0439|Non disponible au niveau du serveur|
|Haoussa (Nigeria, latin)|0x0468|0x0409|Latin1_General_CI_AS|
|Hébreu (Israël)|0x040d|0x040d|Hebrew_CI_AS|
|Hindi (Inde)|0x0439|0x0439|Non disponible au niveau du serveur|
|Hongrois (Hongrie)|0x040e|0x040e|Hungarian_CI_AS|
|Hongrois - Technique|0x1040e|0x1040e|Hungarian_Technical_CI_AS|
|Islandais (Islande)|0x040f|0x040f|Icelandic_CI_AS|
|Igbo (Nigeria)|0x0470|0x0409|Latin1_General_CI_AS|
|Indonésien (Indonésie)|0x0421|0x0409|Latin1_General_CI_AS|
|Inuktitut (Canada, latin)|0x085d|0x0409|Latin1_General_CI_AS|
|Inuktitut (syllabique, Canada)|0x045d|0x045d|Latin1_General_CI_AI|
|Irlandais (Irlande)|0x083c|0x0409|Latin1_General_CI_AS|
|Italien (Italie)|0x0410|0x0409|Latin1_General_CI_AS|
|Italien (Suisse)|0x0810|0x0409|Latin1_General_CI_AS|
|Japonais (Japon XJIS)|0x0411|0x0411|Japanese_CI_AS|
|Japonais (Japon)|0x040411|0x40411|Latin1_General_CI_AI|
|Kannada (Inde)|0x044b|0x0439|Non disponible au niveau du serveur|
|Kazakh (Kazakhstan)|0x043f|0x043f|Kazakh_90_CI_AS|
|Khmer (Cambodge)|0x0453|0x0453|Non disponible au niveau du serveur|
|Quiché (Guatemala)|0x0486|0x0c0a|Modern_Spanish_CI_AS|
|Kinyarwanda (Rwanda)|0x0487|0x0409|Latin1_General_CI_AS|
|Konkani (Inde)|0x0457|0x0439|Non disponible au niveau du serveur|
|Coréen (Corée - Dictionnaire)|0x0412|0x0412|Korean_Wansung_CI_AS|
|Kirghize (Kirghizistan)|0x0440|0x0419|Cyrillic_General_CI_AS|
|Lao (RDP Lao)|0x0454|0x0454|Non disponible au niveau du serveur|
|Letton (Lettonie)|0x0426|0x0426|Latvian_CI_AS|
|Lituanien (Lituanie)|0x0427|0x0427|Lithuanian_CI_AS|
|Bas-sorabe (Allemagne)|0x082e|0x0409|Latin1_General_CI_AS|
|Luxembourgeois (Luxembourg)|0x046e|0x0409|Latin1_General_CI_AS|
|Macédonien (Macédoine du Nord, ARYM)|0x042f|0x042f|Macedonian_FYROM_90_CI_AS|
|Malais (Brunéi Darussalam)|0x083e|0x0409|Latin1_General_CI_AS|
|Malais (Malaisie)|0x043e|0x0409|Latin1_General_CI_AS|
|Malayalam (Inde)|0x044c|0x0439|Non disponible au niveau du serveur|
|Maltais (Malte)|0x043a|0x043a|Latin1_General_CI_AI|
|Maori (Nouvelle-Zélande)|0x0481|0x0481|Latin1_General_CI_AI|
|Mapuche (Chili)|0x047a|0x047a|Latin1_General_CI_AI|
|Marathi (Inde)|0x044e|0x0439|Non disponible au niveau du serveur|
|Mohawk (Canada)|0x047c|0x047c|Latin1_General_CI_AI|
|Mongol (Mongolie)|0x0450|0x0419|Cyrillic_General_CI_AS|
|Mongol (République populaire de Chine)|0x0850|0x0419|Cyrillic_General_CI_AS|
|Népalais (Népal)|0x0461|0x0461|Non disponible au niveau du serveur|
|Norvégien (bokmål, Norvège)|0x0414|0x0414|Latin1_General_CI_AI|
|Norvégien (Nynorsk, Norvège)|0x0814|0x0414|Latin1_General_CI_AI|
|Occitan (France)|0x0482|0x040c|French_CI_AS|
|Odia (Inde)|0x0448|0x0439|Non disponible au niveau du serveur|
|Pachtou (Afghanistan)|0x0463|0x0463|Non disponible au niveau du serveur|
|Persan (Iran)|0x0429|0x0429|Latin1_General_CI_AI|
|Polonais (Pologne)|0x0415|0x0415|Polish_CI_AS|
|Portugais (Brésil)|0x0416|0x0409|Latin1_General_CI_AS|
|Portugais (Portugal)|0x0816|0x0409|Latin1_General_CI_AS|
|Pendjabi (Inde)|0x0446|0x0439|Non disponible au niveau du serveur|
|Quechua (Bolivie)|0x046b|0x0409|Latin1_General_CI_AS|
|Quechua (Équateur)|0x086b|0x0409|Latin1_General_CI_AS|
|Quechua (Pérou)|0x0c6b|0x0409|Latin1_General_CI_AS|
|Roumain (Roumanie)|0x0418|0x0418|Romanian_CI_AS|
|Romanche (Suisse)|0x0417|0x0417|Latin1_General_CI_AI|
|Russe (Russie)|0x0419|0x0419|Cyrillic_General_CI_AS|
|Sahka (Russie)|0x0485|0x0485|Latin1_General_CI_AI|
|Same d'Inari (Finlande)|0x243b|0x083b|Latin1_General_CI_AI|
|Sami (Lule, Norvège)|0x103b|0x043b|Latin1_General_CI_AI|
|Same de Lule (Suède)|0x143b|0x083b|Latin1_General_CI_AI|
|Same du nord (Finlande)|0x0c3b|0x083b|Latin1_General_CI_AI|
|Same du nord (Norvège)|0x043b|0x043b|Latin1_General_CI_AI|
|Same du nord (Suède)|0x083b|0x083b|Latin1_General_CI_AI|
|Same de Skolt (Finlande)|0x203b|0x083b|Latin1_General_CI_AI|
|Same du sud (Norvège)|0x183b|0x043b|Latin1_General_CI_AI|
|Same du sud (Suède)|0x1c3b|0x083b|Latin1_General_CI_AI|
|Sanskrit (Inde)|0x044f|0x0439|Non disponible au niveau du serveur|
|Serbe (Bosnie-Herzégovine, cyrillique)|0x1c1a|0x0c1a|Latin1_General_CI_AI|
|Serbe (Bosnie-Herzégovine, latin)|0x181a|0x081a|Latin1_General_CI_AI|
|Serbe (Serbie, cyrillique)|0x0c1a|0x0c1a|Latin1_General_CI_AI|
|Serbe (latin, Serbie)|0x081a|0x081a|Latin1_General_CI_AI|
|Sesotho sa Leboa/Sotho du Nord (Afrique du Sud)|0x046c|0x0409|Latin1_General_CI_AS|
|Setswana/Tswana (Afrique du Sud)|0x0432|0x0409|Latin1_General_CI_AS|
|Cingalais (Sri Lanka)|0x045b|0x0439|Non disponible au niveau du serveur|
|Slovaque (Slovaquie)|0x041b|0x041b|Slovak_CI_AS|
|Slovène (Slovénie)|0x0424|0x0424|Slovenian_CI_AS|
|Espagnol (Argentine)|0x2c0a|0x0c0a|Modern_Spanish_CI_AS|
|Espagnol (Bolivie)|0x400a|0x0c0a|Modern_Spanish_CI_AS|
|Espagnol (Chili)|0x340a|0x0c0a|Modern_Spanish_CI_AS|
|Espagnol (Colombie)|0x240a|0x0c0a|Modern_Spanish_CI_AS|
|Espagnol (Costa Rica)|0x140a|0x0c0a|Modern_Spanish_CI_AS|
|Espagnol (République dominicaine)|0x1c0a|0x0c0a|Modern_Spanish_CI_AS|
|Espagnol (Équateur)|0x300a|0x0c0a|Modern_Spanish_CI_AS|
|Espagnol (Salvador)|0x440a|0x0c0a|Modern_Spanish_CI_AS|
|Espagnol (Guatemala)|0x100a|0x0c0a|Modern_Spanish_CI_AS|
|Espagnol (Honduras)|0x480a|0x0c0a|Modern_Spanish_CI_AS|
|Espagnol (Mexique)|0x080a|0x0c0a|Modern_Spanish_CI_AS|
|Espagnol (Nicaragua)|0x4c0a|0x0c0a|Modern_Spanish_CI_AS|
|Espagnol (Panama)|0x180a|0x0c0a|Modern_Spanish_CI_AS|
|Espagnol (Paraguay)|0x3c0a|0x0c0a|Modern_Spanish_CI_AS|
|Espagnol (Pérou)|0x280a|0x0c0a|Modern_Spanish_CI_AS|
|Espagnol (Porto Rico)|0x500a|0x0c0a|Modern_Spanish_CI_AS|
|Espagnol (Espagne)|0x0c0a|0x0c0a|Modern_Spanish_CI_AS|
|Espagnol (Espagne, traditionnel)|0x040a|0x040a|Traditional_Spanish_CI_AS|
|Espagnol (États-Unis)|0x540a|0x0409|Latin1_General_CI_AS|
|Espagnol (Uruguay)|0x380a|0x0c0a|Modern_Spanish_CI_AS|
|Espagnol (Venezuela)|0x200a|0x0c0a|Modern_Spanish_CI_AS|
|Swahili (Kenya)|0x0441|0x0409|Latin1_General_CI_AS|
|Suédois (Finlande)|0x081d|0x040b|Finnish_Swedish_CI_AS|
|Suédois (Suède)|0x041d|0x040b|Finnish_Swedish_CI_AS|
|Syriaque (Syrie)|0x045a|0x045a|Non disponible au niveau du serveur|
|Tadjik (Tadjikistan)|0x0428|0x0419|Cyrillic_General_CI_AS|
|Tamazight (Algérie, latin)|0x085f|0x085f|Latin1_General_CI_AI|
|Tamoul (Inde)|0x0449|0x0439|Non disponible au niveau du serveur|
|Tatar (Russie)|0x0444|0x0444|Cyrillic_General_CI_AS|
|Télougou (Inde)|0x044a|0x0439|Non disponible au niveau du serveur|
|Thaï (Thaïlande)|0x041e|0x041e|Thai_CI_AS|
|Tibétain (RPC)|0x0451|0x0451|Non disponible au niveau du serveur|
|Turc (Turquie)|0x041f|0x041f|Turkish_CI_AS|
|Turkmène (Turkménistan)|0x0442|0x0442|Latin1_General_CI_AI|
|Ouïgour (RPC)|0x0480|0x0480|Latin1_General_CI_AI|
|Ukrainien (Ukraine)|0x0422|0x0422|Ukrainian_CI_AS|
|Haut-sorabe (Allemagne)|0x042e|0x042e|Latin1_General_CI_AI|
|Ourdou (Pakistan)|0x0420|0x0420|Latin1_General_CI_AI|
|Ouzbek (Ouzbékistan, cyrillique)|0x0843|0x0419|Cyrillic_General_CI_AS|
|Ouzbek (Ouzbékistan, latin)|0x0443|0x0443|Uzbek_Latin_90_CI_AS|
|Vietnamien (Vietnam)|0x042a|0x042a|Vietnamese_CI_AS|
|Gallois (Royaume-Uni)|0x0452|0x0452|Latin1_General_CI_AI|
|Wolof (Sénégal)|0x0488|0x040c|French_CI_AS|
|Xhosa (Afrique du Sud)|0x0434|0x0409|Latin1_General_CI_AS|
|Yi (RPC)|0x0478|0x0409|Latin1_General_CI_AS|
|Yoruba (Nigeria)|0x046a|0x0409|Latin1_General_CI_AS|
|Zoulou (Afrique du Sud)|0x0435|0x0409|Latin1_General_CI_AS|

Une fois que vous avez affecté un classement au serveur, vous pouvez le modifier uniquement en exportant la totalité des objets et des données de la base de données, en reconstruisant la base de données *master* et en important la totalité des objets et des données de la base de données. Au lieu de modifier le classement par défaut d’une instance [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], vous pouvez spécifier le classement désiré quand vous créez une nouvelle base de données ou colonne de base de données.    

Pour demander le classement du serveur pour une instance [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], utilisez la fonction `SERVERPROPERTY` :

```sql
SELECT CONVERT(varchar, SERVERPROPERTY('collation'));
```

Pour demander au serveur tous les classements disponibles, utilisez la fonction intégrée `fn_helpcollations()` suivante :

```sql
SELECT * FROM sys.fn_helpcollations();
```
    
#### <a name="database-level-collations"></a><a name="Database-level-collations"></a> Classements au niveau de la base de données    
Lorsque vous créez ou modifiez une base de données, vous pouvez utiliser la clause `COLLATE` de l’instruction `CREATE DATABASE` ou `ALTER DATABASE` pour spécifier le classement par défaut de la base de données. Si aucun classement n'est spécifié, le classement du serveur est affecté à la base de données.    
    
Vous ne pouvez pas modifier le classement des bases de données système, à moins de modifier le classement du serveur.
    
Le classement de la base de données est utilisé pour toutes les métadonnées de la base de données. Il constitue le classement par défaut pour l’ensemble des colonnes de chaîne, des objets temporaires, des noms de variable et des autres chaînes utilisées dans la base de données. Quand vous modifiez le classement d'une base de données utilisateur, des conflits de classement risquent de se produire quand des requêtes de la base de données accèdent à des tables temporaires. Les tables temporaires sont toujours stockées dans la base de données système *tempdb*, qui utilise le classement de l’instance. Les requêtes qui comparent les données caractères entre la base de données utilisateur et *tempdb* risquent d’échouer si les classements génèrent un conflit lors de l’évaluation des données caractères. Vous pouvez résoudre ce problème en spécifiant la clause `COLLATE` dans la requête. Pour plus d’informations, consultez [COLLATE (Transact-SQL)](~/t-sql/statements/collations.md).    

> [!NOTE]
> Vous ne pouvez pas modifier le classement une fois que la base de données a été créée dans [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].

Vous pouvez modifier le classement d’une base de données utilisateur avec une instruction `ALTER DATABASE` similaire à celle-ci :

```sql
ALTER DATABASE myDB COLLATE Greek_CS_AI;
```

> [!IMPORTANT]
> Le fait de changer le classement au niveau de la base de données n’affecte pas les classements au niveau des expressions ou des colonnes.

Vous pouvez récupérer le classement actuel d’une base de données à l’aide d’une instruction semblable à la suivante :

```sql
SELECT CONVERT (VARCHAR(50), DATABASEPROPERTYEX('database_name','collation'));
```

#### <a name="column-level-collations"></a><a name="Column-level-collations"></a> Classements au niveau des colonnes    
Lorsque vous créez ou modifiez une table, vous pouvez spécifier des classements pour chaque colonne de chaîne de caractères à l’aide de la clause `COLLATE`. Si vous ne spécifiez pas de classement, le classement par défaut de la base de données est appliqué à la colonne.    

Vous pouvez modifier le classement d’une colonne avec une instruction `ALTER TABLE` similaire à celle-ci :

```sql
ALTER TABLE myTable ALTER COLUMN mycol NVARCHAR(10) COLLATE Greek_CS_AI;
```
    
#### <a name="expression-level-collations"></a><a name="Expression-level-collations"></a> Classements au niveau de l’expression    
Les classements au niveau de l'expression sont définis lors de l'exécution d'une instruction et ils affectent la façon dont l'ensemble de résultats est retourné. Cela permet aux résultats de tri `ORDER BY` d’être spécifiques aux paramètres régionaux. Pour implémenter les classements au niveau de l’expression, utilisez une clause `COLLATE` telle que la suivante :    
    
```sql    
SELECT name FROM customer ORDER BY name COLLATE Latin1_General_CS_AI;    
```    
    
###  <a name="locale"></a><a name="Locale_Defn"></a> Paramètres régionaux    
Les paramètres régionaux sont un ensemble d’informations associées à un emplacement ou à une culture. Il peut s’agir du nom et de l’identificateur de la langue parlée, du script utilisé pour écrire la langue et des conventions culturelles. Les classements peuvent être associés à un ou plusieurs ensembles de paramètres régionaux. Pour plus d'informations, consultez [Locale IDs Assigned by Microsoft (en anglais)](/openspecs/windows_protocols/ms-lcid/a9eac961-e77d-41a6-90a5-ce1a8b0cdb9c).    
    
###  <a name="code-page"></a><a name="Code_Page_Defn"></a> Page de codes    
Une page de codes est le jeu ordonné de caractères d'un script donné dans lequel un index numérique (ou une valeur de point de code) est associé à chaque caractère. Une page de codes Windows est généralement appelée *jeu de caractères* ou *charset*. Les pages de codes permettent d'assurer la prise en charge des jeux de caractères et des dispositions du clavier utilisés par différents paramètres régionaux système Windows.     
 
###  <a name="sort-order"></a><a name="Sort_Order_Defn"></a> Ordre de tri    
L'ordre de tri spécifie comment sont triées les valeurs de données. Il affecte les résultats de comparaison de données. Les données sont triées en utilisant les classements et peuvent être optimisées à l'aide des index.    
    
##  <a name="unicode-support"></a><a name="Unicode_Defn"></a> Prise en charge d’Unicode    
Unicode est un standard en matière de correspondance de points de code avec des caractères. Comme il est conçu pour couvrir tous les caractères de toutes les langues du monde, vous n’avez pas besoin de pages de codes différentes pour gérer des jeux de caractères différents.

### <a name="unicode-basics"></a>Concepts de base d’Unicode
Le stockage de données en plusieurs langues dans une même base de données est délicat à gérer quand vous utilisez seulement des données de type caractère et des pages de codes. Il est également difficile de trouver une page de codes capable de stocker tous les caractères nécessaires spécifiques aux différentes langues. De plus, il est difficile de garantir que les caractères spéciaux sont lus ou mis à jour correctement par différents clients exécutant des pages de codes différentes. Les bases de données qui prennent en charge des clients internationaux doivent toujours utiliser les types de données Unicode au lieu de types de données non-Unicode.

Prenons par exemple une base de données de clients en Amérique du Nord qui doit prendre en charge trois langues principales :

-  des noms et des adresses en espagnol pour le Mexique
-  des noms et des adresses en français pour le Québec
-  des noms et des adresses en anglais pour les autres régions du Canada et les États-Unis

Quand vous utilisez seulement des colonnes de caractères et des pages de codes, vous devez veiller à ce que la base de données soit installée avec une page de codes capable de gérer les caractères des trois langues. Vous devez aussi faire en sorte de garantir la traduction correcte des caractères d’une des langues quand ils sont lus par des clients exécutant une page de codes pour une autre langue.
   
> [!NOTE]
> Les pages de codes utilisées par un client sont déterminées par les paramètres du système d’exploitation. Pour définir les pages de codes du client sur le système d'exploitation Windows, utilisez **Paramètres régionaux** dans le Panneau de configuration.    

Il est difficile de sélectionner une page de codes pour des types de données caractères qui va prendre en charge tous les caractères requis pour une audience internationale. Le moyen le plus simple de gérer les données de type caractère dans les bases de données internationales est de toujours utiliser un type de données qui prend en charge Unicode. 

### <a name="unicode-data-types"></a>Types de données Unicode
Si vous stockez des données caractères qui reflètent plusieurs langues dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] et versions ultérieures ), utilisez des types de données Unicode (**nchar**, **nvarchar** et **ntext**) au lieu de types de données non-Unicode (**char**, **varchar** et **text**). 

> [!NOTE]
> Pour les types de données Unicode, le [!INCLUDE[ssde_md](../../includes/ssde_md.md)] peut représenter jusqu'à 65 535 caractères à l’aide de UCS-2 ou la plage Unicode complète (1 114 111 caractères) si les caractères supplémentaires sont utilisés. Pour plus d’informations sur l’activation de caractères supplémentaires, consultez [Caractères supplémentaires](#Supplementary_Characters).

D’autre part, à compter de [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)], si un classement compatible UTF-8 (\_UTF8) est utilisé, les types de données non Unicode (**char** et **varchar**) deviennent des types de données Unicode basés sur l’encodage UTF-8. [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)] ne change pas le comportement des types de données Unicode existants (**nchar**, **nvarchar** et **ntext**), qui continuent d’utiliser l’encodage UCS-2 ou UTF-16. Pour plus d’informations, consultez [Différences de stockage entre UTF-8 et UTF-16](#storage_differences).

### <a name="unicode-considerations"></a>Considérations relatives à Unicode
Des limitations significatives sont associées aux types de données non-Unicode. C’est parce qu’un ordinateur non-Unicode ne peut utiliser qu’une seule page de codes. Vous pouvez bénéficier de gains de performances en utilisant Unicode, car il requiert moins de conversions de page de codes. Les classements Unicode doivent être sélectionnés individuellement au niveau de la base de données, de la colonne ou de l’expression parce qu’ils ne sont pas pris en charge au niveau du serveur.    

Lorsque vous déplacez des données d'un serveur vers un client, votre classement du serveur peut ne pas être reconnu par les pilotes de clients plus anciens. Cela peut se produire lorsque vous déplacez des données d'un serveur Unicode vers un client non-Unicode. La meilleure solution peut alors consister à mettre à niveau le système d'exploitation du client afin de mettre aussi à jour les classements du système sous-jacent. Si le client est équipé du logiciel client de base de données, vous pouvez envisager de lui appliquer une mise à jour du service.    
    
> [!TIP]
> Vous pouvez également essayer d'utiliser un autre classement pour les données du serveur. Choisissez un classement qui établit un mappage à une page de codes du client.    
>
> Pour utiliser les classements UTF-16 disponibles dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] et versions ultérieures) afin d’améliorer la recherche et le tri de certains caractères Unicode (classements Windows uniquement), vous pouvez sélectionner un des classements (\_SC) de caractères supplémentaires ou un des classements de la version 140.    
 
Pour utiliser les classements UTF-8 disponibles dans [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)] et améliorer la recherche et le tri de certains caractères Unicode (classements Windows uniquement), vous devez sélectionner des classements compatibles avec l’encodage UTF-8 (\_UTF8).
 
-   L’indicateur UTF8 peut être appliqué aux éléments suivants :    
    -   Classements linguistiques qui prennent déjà en charge les caractères supplémentaires (\_SC) ou le sélecteur de variante (\_VSS)
    -   BIN2<sup>1</sup> classement binaire
    
-   L’indicateur UTF8 ne peut pas être appliqué aux éléments suivants :    
    -   Classements linguistiques qui ne prennent pas en charge les caractères supplémentaires (\_SC) ou le sélecteur de variante (\_VSS)
    -   Classements binaires BIN ou BIN2<sup>2</sup>
    -   Classements SQL\_*  
    
<sup>1</sup> À compter de [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)] CTP 2.3. [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)] CTP 3.0 a remplacé le classement **UTF8_BIN2** par **Latin1_General_100_BIN2_UTF8**.        
<sup>2</sup> Jusqu’à [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)] CTP 2.3.    
    
Pour déterminer les problèmes qui sont liés à l'utilisation des types de données Unicode ou non-Unicode, testez votre scénario pour mesurer les écarts de performances dans votre environnement. Il est recommandé de normaliser le classement utilisé sur les systèmes de votre organisation et de déployer des serveurs et clients Unicode partout où vous le pouvez.    
    
Dans de nombreuses situations, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] interagit avec d’autres serveurs ou clients, et votre organisation peut utiliser plusieurs normes d’accès aux données entre les applications et les instances de serveur. Les clients[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sont l'un des deux types principaux :    
    
-   Les **clients Unicode** qui utilisent OLE DB et ODBC (Open Database Connectivity) version 3.7 ou ultérieure.    
-   Les **clients non-Unicode** qui utilisent DB-Library et ODBC version 3.6 ou antérieure.    
    
Le tableau suivant présente des informations sur l’utilisation des données multilingues avec diverses combinaisons de serveurs Unicode et non-Unicode :    
    
|Serveur|Client|Avantages ou restrictions|    
|------------|------------|-----------------------------|    
|Unicode|Unicode|Comme les données Unicode sont utilisées dans tout le système, ce scénario fournit les meilleures performances et la meilleure protection contre l’altération des données récupérées. C’est le cas avec ActiveX Data Objects (ADO), OLE DB et ODBC version 3.7 ou ultérieure.|    
|Unicode|Non-Unicode|Dans ce scénario, et surtout avec les connexions entre un serveur exécutant un système d’exploitation récent et un client exécutant une version antérieure de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], ou un système d’exploitation plus ancien, il peut y avoir des limitations ou des erreurs lorsque vous déplacez des données vers un ordinateur client. Les données Unicode sur le serveur tentent d’établir un mappage à une page de codes correspondante sur le client non-Unicode afin de convertir les données.|    
|Non-Unicode|Unicode|Cette configuration n’est pas idéale pour l’utilisation de données multilingues. Vous ne pouvez pas écrire des données Unicode sur le serveur non-Unicode. Des problèmes peuvent survenir lorsque les données sont envoyées à des serveurs qui sont en dehors de la page de codes du serveur.|    
|Non-Unicode|Non-Unicode|Cette configuration est la plus limitée pour des données multilingues. Vous pouvez utiliser uniquement une seule page de codes.|    
    
##  <a name="supplementary-characters"></a><a name="Supplementary_Characters"></a> Caractères supplémentaires    
Le Consortium Unicode alloue à chaque caractère un point de code unique, qui est une valeur comprise entre 000000 et 10FFFF. Les caractères les plus fréquemment utilisés ont des valeurs de point de code dans la plage de 000000 à 00FFFF (65 535 caractères) qui correspondent à un mot de 8 ou 16 bits en mémoire et sur le disque. Cette plage est généralement désignée en tant que Plan multilingue de base (BMP). 

Mais le Consortium Unicode a établi des 16 « plans » de caractères supplémentaires, chacun ayant la même taille que le BMP. Cette définition accorde à Unicode le potentiel de représenter 1 114 112 caractères (autrement dit, 2<sup>16</sup> * 17 caractères) au sein de la plage de point de code de 000000 à 10FFFF. Les caractères dont les valeurs de code de caractère supérieures à 00FFFF requièrent entre deux et quatre mots de 8 bits consécutifs (UTF-8) ou deux mots de 16 bits consécutifs (UTF-16). Ces caractères situés au-delà du BMP sont appelés *caractères supplémentaires* et les deux mots de 8 ou 16 bits consécutifs supplémentaires sont appelés *paires de substitution*. Pour plus d’informations sur les caractères supplémentaires, les substitutions et les paires de substitution, reportez-vous à [la norme Unicode](http://www.unicode.org/standard/standard.html).    

[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] fournit des types de données tels que **nchar** et **nvarchar** pour stocker les données Unicode dans la plage BMP (de 000000 à 00FFFF), ce que [!INCLUDE[ssde_md](../../includes/ssde_md.md)] encode à l’aide d’UCS-2. 

[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] a introduit une nouvelle famille de classements de caractères supplémentaires (\_SC) pouvant être utilisée avec les types de données **nchar**, **nvarchar** et **sql_variant** pour représenter la plage de caractères Unicode complète (de 000000 à 10FFFF). Par exemple : **Latin1_General_100_CI_AS_SC** ou, si vous utilisez un classement japonais, **Japanese_Bushu_Kakusu_100_CI_AS_SC**. 
 
[!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)] étend la prise en charge des caractères supplémentaires aux types de données **char** et **varchar** avec les nouveaux classements prenant en charge UTF-8 ([\_UTF8](#utf8)). Ces types de données sont également capables de représenter la plage de caractères Unicode complète.   

> [!NOTE]
> À compter de [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)], tous les nouveaux classements \_140 prennent en charge automatiquement les caractères supplémentaires.

Si vous utilisez des caractères supplémentaires :    
    
-   Les caractères supplémentaires peuvent être utilisés dans des opérations de tri et de comparaison dans les versions de classement 90 ou versions supérieures.    
-   Tous les classements version 100 prennent en charge le tri linguistique avec les caractères supplémentaires.    
-   Les caractères supplémentaires ne sont pas utilisables dans les métadonnées, telles que les noms d’objets de base de données.    
-   L’indicateur SC peut s’appliquer aux éléments suivants :    
    -   Classements version 90    
    -   Classements version 100    
    
-   L’indicateur SC ne peut pas s’appliquer aux éléments suivants :    
    -   Classements Windows version 80 et sans version    
    -   Classements binaires BIN ou BIN2    
    -   Classements SQL\*    
    -   Classements version 140 (ces derniers n’ont pas besoin de l’indicateur SC, car ils prennent déjà en charge les caractères supplémentaires)    
    
Le tableau suivant compare le comportement de quelques fonctions de chaîne et opérateurs de chaîne quand ils utilisent des caractères supplémentaires avec et sans classement sensible aux caractères supplémentaires :    
    
|Fonction de chaîne ou opérateur|Avec un classement sensible aux caractères supplémentaires|Sans classement sensible aux caractères supplémentaires|    
|---------------------------------|--------------------------|-----------------------------|    
|[CHARINDEX](../../t-sql/functions/charindex-transact-sql.md)<br /><br /> [LEN](../../t-sql/functions/len-transact-sql.md)<br /><br /> [PATINDEX](../../t-sql/functions/patindex-transact-sql.md)|La paire de substitution UTF-16 est comptée comme un point de code unique.|La paire de substitution UTF-16 est comptée comme deux points de code.|    
|[LEFT](../../t-sql/functions/left-transact-sql.md)<br /><br /> [REPLACE](../../t-sql/functions/replace-transact-sql.md)<br /><br /> [REVERSE](../../t-sql/functions/reverse-transact-sql.md)<br /><br /> [RIGHT](../../t-sql/functions/right-transact-sql.md)<br /><br /> [SUBSTRING](../../t-sql/functions/substring-transact-sql.md)<br /><br /> [STUFF](../../t-sql/functions/stuff-transact-sql.md)|Ces fonctions traitent chaque paire de substitution comme un point de code unique et fonctionnent comme prévu.|Ces fonctions peuvent fractionner toutes les paires de substitution et conduire à des résultats inattendus.|    
|[NCHAR](../../t-sql/functions/nchar-transact-sql.md)|Retourne le caractère qui correspond à la valeur de point de code Unicode spécifiée dans la plage de 0 à 0x10FFFF. Si la valeur spécifiée figure dans la plage de 0 à 0xFFFF, un seul caractère est retourné. Pour les valeurs plus élevées, la paire de substitution correspondante est retournée.|Une valeur supérieure à 0xFFFF retourne NULL au lieu du substitut correspondant.|    
|[UNICODE](../../t-sql/functions/unicode-transact-sql.md)|Retourne un point de code UTF-16 dans la plage de 0 à 0x10FFFF.|Retourne un point de code UCS-2 dans la plage de 0 à 0xFFFF.|    
|[Recherche de correspondance d’un seul caractère générique](../../t-sql/language-elements/wildcard-match-one-character-transact-sql.md)<br /><br /> [Caractère générique - Caractères à ne pas faire correspondre](../../t-sql/language-elements/wildcard-character-s-not-to-match-transact-sql.md)|Les caractères supplémentaires sont pris en charge pour toutes les opérations génériques.|Les caractères supplémentaires ne sont pas pris en charge pour ces opérations génériques. D'autres opérateurs génériques sont pris en charge.|    
    
## <a name="gb18030-support"></a><a name="GB18030"></a> Prise en charge de la norme GB18030    
GB18030 est une norme distincte utilisée en République populaire de Chine pour l’encodage des caractères chinois. Dans la norme GB18030, les caractères peuvent être encodés sur 1, 2 ou 4 octets de longueur. Pour prendre en charge les caractères encodés selon la norme GB18030,[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] les reconnaît lorsqu'ils entrent dans le serveur en provenance d'une application côté client, puis les convertit et les stocke en mode natif en tant que caractères Unicode. Une fois stockés dans le serveur, ils sont traités en tant que caractères Unicode dans toutes les opérations suivantes. 

Vous pouvez utiliser n'importe quel classement chinois, de préférence la version 100 la plus récente. Tous les classements de niveau \_100 prennent en charge le tri linguistique avec les caractères GB18030. Si les données incluent des caractères supplémentaires (paires de substitution), vous pouvez utiliser les classements SC disponibles dans [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] pour améliorer la recherche et le tri.    

> [!NOTE]
> Vérifiez que vos outils clients, comme [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], utilisent la police Dengxian pour afficher correctement les chaînes qui contiennent des caractères encodés en GB18030.
    
## <a name="complex-script-support"></a><a name="Complex_script"></a> Prise en charge des scripts complexes    
[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] peut prendre en charge l'entrée, le stockage, la modification et l'affichage de scripts complexes. Les scripts complexes sont notamment les suivants :    
    
-   Scripts qui associent l'utilisation de textes écrits de droite à gauche et de gauche à droite, par exemple les textes écrits en arabe et en anglais.    
-   Scripts dont les caractères changent de forme en fonction de leur position ou lorsqu'ils sont associés à d'autres caractères, par exemple les caractères arabes, indiens et thaï.    
-   Des langues, telles que le thaï, nécessitent des dictionnaires internes pour la reconnaissance des mots, car ces derniers ne sont pas séparés.    
    
Les applications de base de données qui interagissent avec [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] doivent utiliser des contrôles qui prennent en charge les scripts complexes. Les contrôles Windows Form standard créés dans du code managé peuvent prendre en charge les scripts complexes.    

## <a name="japanese-collations-added-in--sssql17-md"></a><a name="Japanese_Collations"></a> Classements japonais ajoutés dans  [!INCLUDE [sssql17-md](../../includes/sssql17-md.md)]
 
À compter de [!INCLUDE [sssql17-md](../../includes/sssql17-md.md)], de nouvelles familles de classement du japonais sont prises en charge, avec les permutations de différentes options (\_CS, \_AS, \_KS, \_WS et \_VSS). 

Pour lister ces classements, vous pouvez interroger le [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] :      

```sql 
SELECT Name, Description FROM fn_helpcollations()  
WHERE Name LIKE 'Japanese_Bushu_Kakusu_140%' OR Name LIKE 'Japanese_XJIS_140%'
``` 

Tous les nouveaux classements disposent d’une prise en charge intégrée des caractères supplémentaires, de sorte qu’aucun des nouveaux classements **\_140** n’a (ni ne requiert) l’indicateur SC.

Ces classements sont pris en charge dans les index, les tables optimisées en mémoire, les index columnstore et les modules compilés en mode natif [!INCLUDE[ssde_md](../../includes/ssde_md.md)].

<a name="ctp23"></a>

## <a name="utf-8-support"></a><a name="utf8"></a> Support UTF-8
[!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)] introduit le complet support du codage de caractères UTF-8 largement utilisé en tant qu’encodage d’importation ou d’exportation et en tant que classement au niveau des base de données et au niveau des colonnes pour les données de chaîne. UTF-8 est autorisé dans les types de données **char** et **varchar**, et il est activé quand vous créez ou modifiez le classement d’un objet en un classement avec un suffixe *UTF8*. La modification de **LATIN1_GENERAL_100_CI_AS_SC** en **LATIN1_GENERAL_100_CI_AS_SC_UTF8** en est un exemple. 

UTF-8 est disponible uniquement pour les classements Windows qui prennent en charge les caractères supplémentaires, comme introduit dans [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]. Les types de données **nchar** et **nvarchar** autorisent l’encodage UCS-2 ou UTF-16 uniquement et restent inchangés.

### <a name="storage-differences-between-utf-8-and-utf-16"></a><a name="storage_differences"></a> Différences de stockage entre UTF-8 et UTF-16
Le Consortium Unicode alloue à chaque caractère un point de code unique, qui est une valeur comprise entre 000000 et 10FFFF. Avec [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)], les encodages UTF-8 et UTF-16 sont disponibles pour représenter la plage complète :    
-  Avec l’encodage UTF-8, les caractères figurant dans la plage ASCII (de 000000 à 00007F) utilisent 1 octet, les points de code de 000080 à 0007FF nécessitent 2 octets, les points de code de 000800 à 00FFFF nécessitent 3 octets et les points de code de 0010000 à 0010FFFF nécessitent 4 octets. 
-  Avec l’encodage UTF-16, les points de code de 000000 à 00FFFF nécessitent 2 octets et les points de code de 0010000 à 0010FFFF nécessitent 4 octets. 

La table suivante liste les octets de stockage d’encodage pour chaque plage de caractères et type d’encodage :

|Plage de codes (hexadécimal)|Plage de codes (décimal)|Octets de stockage<sup>1</sup> avec UTF-8|Octets de stockage<sup>1</sup> avec UTF-16|    
|---------------------------------|---------------------------------|--------------------------|-----------------------------|   
|000000–00007F|0–127|1|2|
|000080–00009F<br />0000A0–0003FF<br />000400–0007FF|128–159<br />160–1 023<br />1 024–2 047|2|2|
|000800–003FFF<br />004000–00FFFF|2 048–16 383<br />16 384–65 535|3|2|
|010000–03FFFF<sup>2</sup><br /><br />040000–10FFFF<sup>2</sup>|65 536–262 143<sup>2</sup><br /><br />262 144–1 114 111<sup>2</sup>|4|4|

<sup>1</sup> Les *octets de stockage* font référence à la longueur d’octets encodée, et non à la taille de stockage sur disque des types de données. Pour plus d’informations sur les tailles de stockage sur disque, consultez [nchar et nvarchar](../../t-sql/data-types/nchar-and-nvarchar-transact-sql.md) et [char et varchar](../../t-sql/data-types/char-and-varchar-transact-sql.md).

<sup>2</sup> Plage de points de code pour des [caractères supplémentaires](#Supplementary_Characters).

> [!TIP]   
> Il est courant de penser en [CHAR(*n*) et en VARCHAR(*n*)](../../t-sql/data-types/char-and-varchar-transact-sql.md), ou en [NCHAR(*n*) et en NVARCHAR(*n*)](../../t-sql/data-types/nchar-and-nvarchar-transact-sql.md), que le *n* définit le nombre de caractères. En effet, dans l’exemple d’une colonne CHAR(10), 10 caractères ASCII de la plage de 0 à 127 peuvent être stockés à l’aide d’un classement tel que **Latin1_General_100_CI_AI**, car chaque caractère de cette plage utilise uniquement 1 octet.
>    
> Toutefois, dans [CHAR(*n*) et VARCHAR(*n*)](../../t-sql/data-types/char-and-varchar-transact-sql.md), le *n* définit la taille de la chaîne en *octets* (de 0 à 8 000), et dans [NCHAR(*n*) et NVARCHAR(*n*)](../../t-sql/data-types/nchar-and-nvarchar-transact-sql.md), le *n* définit la taille de la chaîne en *paires d’octets* (de 0 à 4 000). *n* ne définit jamais des nombres de caractères pouvant être stockés.

Comme vous venez de le voir, le choix de l’encodage Unicode et du type de données appropriés peut fournir des gains de stockage significatifs ou augmenter votre encombrement de stockage actuel, en fonction du jeu de caractères utilisé. Par exemple, lorsque vous utilisez un classement Latin prenant en charge UTF-8, tel que **Latin1_General_100_CI_AI_SC_UTF8**, une colonne `CHAR(10)` stocke 10 octets et peut contenir 10 caractères ASCII dans la plage de 0 à 127. Toutefois, elle ne peut contenir que 5 caractères dans la plage de 128 à 2 047 et seulement 3 caractères dans la plage de 2 048 à 65 535. En comparaison, comme une colonne `NCHAR(10)` stocke 10 paires d’octets (20 octets), elle peut contenir 10 caractères dans la plage de 0 à 65 535.  

Avant de choisir s’il faut utiliser l’encodage UTF-8 ou UTF-16 pour une base de données ou une colonne, prenez en compte la distribution des données de chaîne qui seront stockées :
-  Si elle est principalement dans la plage ASCII de 0 à 127 (comme l’anglais), chaque caractère nécessite 1 octet avec UTF-8 et 2 octets avec UTF-16. L’utilisation UFT-8 offre des avantages du stockage. Le fait de transformer un type de données de colonne existant comportant des caractères ASCII de la plage de 0 à 127 de `NCHAR(10)` en `CHAR(10)` et d’utiliser un classement prenant en charge UTF-8 se traduit par une réduction de 50 % des besoins en stockage. Cette réduction correspond au fait que `NCHAR(10)` nécessite 20 octets pour le stockage, tandis que `CHAR(10)` nécessite 10 octets pour la même représentation de chaîne Unicode.    
-  Au-dessus de la plage ASCII, presque tout l’alphabet latin et grec, cyrillique, copte, arménien, hébreu, arabe, syriaque, Tāna et n’ko nécessite 2 octets par caractère dans les encodages UTF-8 et UTF-16. Dans ces cas, il n’y a pas de différences significatives de stockage pour des types de données comparables (par exemple, entre **char** et **nchar**).
-  S’il s’agit principalement d’un script d’Asie de l'Est (par exemple, coréen, chinois ou japonais), chaque caractère nécessite 3 octets en UTF-8 et 2 octets en UTF-16. L’utilisation UFT-16 offre des avantages du stockage. 
-  Les caractères figurant dans la plage de 010000 à 10FFFF nécessitent 4 octets en UTF-8 et UTF-16. Dans ces cas, il n’y a pas de différences de stockage pour des types de données comparables (par exemple entre **char** et **nchar**).

Pour d’autres considérations, consultez [Écrire des instructions Transact-SQL internationales](../../relational-databases/collations/write-international-transact-sql-statements.md).

### <a name="converting-to-utf-8"></a><a name="converting"></a> Conversion au format UTF-8
Dans la mesure où dans [CHAR(*n*) et VARCHAR(*n*)](../../t-sql/data-types/char-and-varchar-transact-sql.md) ou dans [NCHAR(*n*) et NVARCHAR(*n*)](../../t-sql/data-types/nchar-and-nvarchar-transact-sql.md), le *n* définit la taille de stockage des octets, et non le nombre de caractères qui peuvent être stockés, il est important de déterminer la taille du type de données à convertir pour éviter la troncation des données. 

Par exemple, considérons une colonne définie en tant que **NVARCHAR(100)** , qui stocke 180 octets de caractères japonais. Dans cet exemple, les données de colonne sont encodées à l’aide de UCS-2 ou UTF-16, qui utilise 2 octets par caractère. La conversion du type de colonne en **VARCHAR(200)** n’est pas suffisante pour empêcher la troncation des données, car le nouveau type de données peut stocker uniquement 200 octets, mais les caractères japonais nécessitent 3 octets quand ils sont encodés en UTF-8. La colonne doit donc être définie en tant que **VARCHAR(270)** pour éviter toute perte de données par troncation de données.

Il est donc nécessaire de savoir à l’avance quelle est la taille en octets projetée pour la définition de colonne avant de convertir les données existantes en UTF-8, et d’ajuster la nouvelle taille de type de données de manière appropriée. Consultez le script [!INCLUDE[tsql](../../includes/tsql-md.md)] ou le notebook SQL dans les [exemples de données sur GitHub ](https://github.com/microsoft/sql-server-samples/blob/master/samples/features/unicode), qui utilisent la fonction [DATALENGTH](../../t-sql/functions/datalength-transact-sql.md) et l’instruction [COLLATE](../../t-sql/statements/collations.md) afin de déterminer les exigences de longueur de données appropriées pour les opérations de conversion UTF-8 dans une base de données existante.

Pour changer le classement des colonnes et le type de données dans une table existante, utilisez l’une des méthodes décrites dans [Définir ou changer le classement des colonnes](../../relational-databases/collations/set-or-change-the-column-collation.md).

Pour changer le classement de la base de données, en permettant aux nouveaux objets d’hériter du classement de base de données par défaut, ou pour changer le classement du serveur, en permettant aux nouvelles bases de données d’hériter du classement système par défaut, consultez la section [Tâches associées](#Related_Tasks) de cet article. 

##  <a name="related-tasks"></a><a name="Related_Tasks"></a> Related tasks    
    
|Tâche|Rubrique|    
|----------|-----------|    
|Explique comment définir ou modifier le classement de l'instance de SQL Server. Notez que le changement du classement du serveur ne change pas le classement des bases de données existantes.|[Définir ou modifier le classement du serveur](../../relational-databases/collations/set-or-change-the-server-collation.md)|    
|Explique comment définir ou modifier le classement d'une base de données utilisateur. Notez que le changement d’un classement de base de données ne change pas le classement des colonnes de table existantes.|[Définir ou modifier le classement de la base de données](../../relational-databases/collations/set-or-change-the-database-collation.md)|    
|Explique comment définir ou modifier le classement d’une colonne dans la base de données|[Définir ou modifier le classement des colonnes](../../relational-databases/collations/set-or-change-the-column-collation.md)|    
|Explique comment retourner des informations de classement au niveau du serveur, de la base de données ou de la colonne|[Afficher des informations de classement](../../relational-databases/collations/view-collation-information.md)|    
|Explique comment écrire des instructions Transact-SQL qui sont plus portables d’une langue à une autre ou qui prennent en charge plusieurs langues plus facilement|[Rédiger des instructions Transact-SQL internationales](../../relational-databases/collations/write-international-transact-sql-statements.md)|    
|Explique comment modifier la langue des messages d’erreur et des paramètres relatifs à l’utilisation et l’affichage de la date, de l’heure et des devises|[Définir une langue de session](../../relational-databases/collations/set-a-session-language.md)|    
    
##  <a name="related-content"></a><a name="Related_Content"></a> Related content    
Pour plus d’informations, consultez le contenu connexe suivante :
* [SQL Server Best Practices Collation Change (Bonnes pratiques relatives au changement de classement dans SQL Server)](https://go.microsoft.com/fwlink/?LinkId=113891)  
* [Utiliser le format caractère Unicode pour importer ou exporter des données (SQL Server)](../../relational-databases/import-export/use-unicode-character-format-to-import-or-export-data-sql-server.md)
* [Rédiger des instructions Transact-SQL internationales](../../relational-databases/collations/write-international-transact-sql-statements.md)  
* [SQL Server Best Practices Migration to Unicode](https://go.microsoft.com/fwlink/?LinkId=113890) (plus de support)   
* [Consortium Unicode](https://go.microsoft.com/fwlink/?LinkId=48619)  
* [Norme Unicode](http://www.unicode.org/standard/standard.html)  
* [Prise en charge d’UTF-8 dans OLE DB Driver pour SQL Server](../../connect/oledb/features/utf-8-support-in-oledb-driver-for-sql-server.md)  
* [Nom du classement SQL Server (Transact-SQL)](../../t-sql/statements/sql-server-collation-name-transact-sql.md)  
* [Nom de classement Windows (Transact-SQL)](../../t-sql/statements/windows-collation-name-transact-sql.md)  
* [Introducing UTF-8 support for SQL Server](https://techcommunity.microsoft.com/t5/SQL-Server/Introducing-UTF-8-support-for-SQL-Server/ba-p/734928)      
* [COLLATE (Transact-SQL)](../../t-sql/statements/collations.md)      
* [Priorité de classement](../../t-sql/statements/collation-precedence-transact-sql.md)    
    
## <a name="see-also"></a>Voir aussi    
[Classements de base de données autonome](../../relational-databases/databases/contained-database-collations.md)     
[Choisir une langue lors de la création d’un index de recherche en texte intégral](../../relational-databases/search/choose-a-language-when-creating-a-full-text-index.md)     
[sys.fn_helpcollations (Transact-SQL)](../../relational-databases/system-functions/sys-fn-helpcollations-transact-sql.md)       
[Jeux de caractères codés sur un octet et multioctets](/cpp/c-runtime-library/single-byte-and-multibyte-character-sets)