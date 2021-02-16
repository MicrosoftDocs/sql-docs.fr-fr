---
description: Nom de classement Windows (Transact-SQL)
title: Nom de classement Windows (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
dev_langs:
- TSQL
helpviewer_keywords:
- Windows collations [SQL Server]
- names [SQL Server], collations
- collations [SQL Server], Windows collations
- Collation Designator
ms.assetid: acceef84-2c68-46e2-a021-be019b7ab14e
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 2fa047e5ef018109352972f3ae139c6eff2c1b93
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/01/2021
ms.locfileid: "100349948"
---
# <a name="windows-collation-name-transact-sql"></a>Nom de classement Windows (Transact-SQL)

[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Spécifie le nom de classement Windows dans la clause COLLATE dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Le nom de classement Windows est composé d'un indicateur de classement et de styles de comparaison.

![Icône du lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)

## <a name="syntax"></a>Syntaxe

```syntaxsql
<Windows_collation_name> :: =
<CollationDesignator>_<ComparisonStyle>

<ComparisonStyle> :: =
{ <CaseSensitivity>_<AccentSensitivity> [ _<KanatypeSensitive> ] [ _<WidthSensitive> ] [ _<VariationSelectorSensitive> ] 
}
| { _UTF8 }
| { _BIN | _BIN2 }
```

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments

*CollationDesignator*   
Spécifie les règles de classement de base utilisées par le classement Windows. Les règles de classement de base incluent les éléments suivants :

- Les règles de tri et de comparaison appliquées quand le tri de dictionnaire est spécifié. Les règles de tri sont basées sur l'alphabet ou la langue ;
- La page de codes utilisée pour stocker les données **varchar**.

Quelques exemples :

- Latin1\_General ou Français : les deux utilisent la page de codes 1252.
- Turkish : utilise la page de code 1254.

*CaseSensitivity*  
**CI** ne respecte pas la casse, contrairement à **CS**.

*AccentSensitivity*  
**AI** ne respecte pas les accents, contrairement à **AS**.

*KanatypeSensitive*  
L’oubli de cette option ne suit pas les caractères Kana, alors que **KS** les respecte.

*WidthSensitivity*  
L’oubli de cette option ne tient pas compte des largeurs, alors que **WS** les respecte.

*VariationSelectorSensitivity*  
- **S’applique à** : À compter de [!INCLUDE[sssql17-md](../../includes/sssql17-md.md)] 

- L’oubli de cette option spécifie le non-respect du sélecteur de variation, **VSS** spécifie le respect du sélecteur de variation.

**UTF8**  
- **S’applique à** : À compter de [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)]   

- Spécifie l’encodage UTF-8 à utiliser pour les types de données éligibles. Pour plus d’informations, consultez [Prise en charge d’Unicode et du classement](../../relational-databases/collations/collation-and-unicode-support.md).

**BIN**  
Indique l'ordre de tri binaire et assurant la compatibilité descendante à utiliser.

**BIN2**  
Indique l'ordre de tri binaire utilisant la sémantique de comparaison des points de code.

## <a name="remarks"></a>Notes
Selon la version du classement, certains points de code peuvent ne pas avoir de pondérations de tri et/ou de mappages majuscules/minuscules définis. Par exemple, comparez la sortie de la fonction `LOWER` quand elle reçoit le même caractère, mais dans différentes versions du même classement :

```sql
SELECT NCHAR(504) COLLATE Latin1_General_CI_AS AS [Uppercase],
       NCHAR(505) COLLATE Latin1_General_CI_AS AS [Lowercase];
-- Ǹ    ǹ


SELECT LOWER(NCHAR(504) COLLATE Latin1_General_CI_AS) AS [Version80Collation],
       LOWER(NCHAR(504) COLLATE Latin1_General_100_CI_AS) AS [Version100Collation];
-- Ǹ    ǹ
```

La première instruction montre la forme majuscule et la forme minuscule de ce caractère dans le classement plus ancien (le classement n’affecte pas la disponibilité des caractères lors de l’utilisation de données Unicode). Cependant, la deuxième instruction montre qu’un caractère majuscule est retourné quand le classement est Latin1\_General\_CI\_AS, car il n’y a pas de mappage aux minuscules défini pour ce point de code dans ce classement.

Lors de l'utilisation de certaines langues, il peut être essentiel d'éviter les classements anciens. C'est par exemple le cas pour le télougou.

Dans certains cas, les classements Windows et les classements [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] peuvent générer différents plans de requête pour la même requête.

## <a name="examples"></a>Exemples

Voici quelques exemples de noms de classements Windows :

- **Latin1\_General\_100\_CI\_AS**

  Le classement utilise les mappages et les règles de tri du dictionnaire général Latin1 et établit un mappage à la page de codes 1252. Il s’agit d’un classement de la version \_100, qui ne respecte pas la casse (CI) et respecte les accents (AS).

- **Estonian\_CS\_AS**

  Ce classement utilise les règles de tri du dictionnaire estonien et il est mappé à la page de codes 1257. Il s’agit d’un classement de la version \_80 (découlant de l’absence de numéro de version dans le nom), qui est sensible à la casse (CS) et qui respecte les accents (AS).

- **Japanese\_Bushu\_Kakusu\_140\_BIN2**

  Le classement utilise les règles de tri du point de code binaire et il est mappé à la page de codes 932. Il s’agit d’un classement de la version \_140 et les règles de tri du dictionnaire du japonais Bushu Kakusu sont ignorées.

## <a name="windows-collations"></a>Classements Windows

Pour énumérer les classements Windows pris en charge par votre d'instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], exécutez la requête suivante.

```sql
SELECT * FROM sys.fn_helpcollations() WHERE [name] NOT LIKE N'SQL%';
```

Le tableau suivant répertorie tous les classements Windows pris en charge dans [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)].

|Paramètres régionaux Windows|Classement version 100|Classement version 90|
|--------------------|---------------------------|--------------------------|
|Alsacien (France)|Latin1_General_100_|Non disponible|
|Amharique (Éthiopie)|Latin1_General_100_|Non disponible|
|Arménien (Arménie)|Cyrillic_General_100_|Non disponible|
|Assamais (Inde)|Assamese_100_ <sup>1</sup>|Non disponible|
|Bengali (Bangladesh)|Bengali_100_<sup>1</sup>|Non disponible|
|Bachkir (Russie)|Bashkir_100_|Non disponible|
|Basque (Basque)|Latin1_General_100_|Non disponible|
|Bengali (India)|Bengali_100_<sup>1</sup>|Non disponible|
|Bosniaque (Bosnie-Herzégovine, cyrillique)|Bosnian_Cyrillic_100_|Non disponible|
|Bosniaque (Bosnie-Herzégovine, latin)|Bosnian_Latin_100_|Non disponible|
|Breton (France)|Breton_100_|Non disponible|
|Chinese (Macao (R.A.S.))|Chinese_Traditional_Pinyin_100_|Non disponible|
|Chinese (Macao (R.A.S.))|Chinese_Traditional_Stroke_Order_100_|Non disponible|
|Chinese (Singapore)|Chinese_Simplified_Stroke_Order_100_|Non disponible|
|Corse (France)|Corsican_100_|Non disponible|
|Croate (Bosnie-Herzégovine, latin)|Croatian_100_|Non disponible|
|Dari (Afghanistan)|Dari_100_|Non disponible|
|Anglais (Inde)|Latin1_General_100_|Non disponible|
|Anglais (Malaisie)|Latin1_General_100_|Non disponible|
|Anglais (Singapour)|Latin1_General_100_|Non disponible|
|Filipino (Philippines)|Latin1_General_100_|Non disponible|
|Frison (Pays-Bas)|Frisian_100_|Non disponible|
|Géorgien (Géorgie)|Cyrillic_General_100_|Non disponible|
|Groenlandais (Groenland)|Danish_Greenlandic_100_|Non disponible|
|Goudjrati (Inde)|Indic_General_100_<sup>1</sup>|Indic_General_90_|
|Haoussa (Nigeria, latin)|Latin1_General_100_|Non disponible|
|Hindi (Inde)|Indic_General_100_<sup>1</sup>|Indic_General_90_|
|Igbo (Nigeria)|Latin1_General_100_|Non disponible|
|Inuktitut (Canada, latin)|Latin1_General_100_|Non disponible|
|Inuktitut (syllabique, Canada)|Latin1_General_100_|Non disponible|
|Irlandais (Irlande)|Latin1_General_100_|Non disponible|
|Japonais (Japon XJIS)|Japanese_XJIS_100_|Japanese_90_, Japanese_|
|Japonais (Japon)|Japanese_Bushu_Kakusu_100_|Non disponible|
|Kannada (Inde)|Indic_General_100_<sup>1</sup>|Indic_General_90_|
|Khmer (Cambodge)|Khmer_100_<sup>1</sup>|Non disponible|
|Quiché (Guatemala)|Modern_Spanish_100_|Non disponible|
|Kinyarwanda (Rwanda)|Latin1_General_100_|Non disponible|
|Konkani (Inde)|Indic_General_100_<sup>1</sup>|Indic_General_90_|
|Lao (RDP Lao)|Lao_100_<sup>1</sup>|Non disponible|
|Bas-sorabe (Allemagne)|Latin1_General_100_|Non disponible|
|Luxembourgeois (Luxembourg)|Latin1_General_100_|Non disponible|
|Malayalam (Inde)|Indic_General_100_<sup>1</sup>|Non disponible|
|Maltais (Malte)|Maltese_100_|Non disponible|
|Maori (Nouvelle-Zélande)|Maori_100_|Non disponible|
|Mapuche (Chili)|Mapudungan_100_|Non disponible|
|Marathi (Inde)|Indic_General_100_<sup>1</sup>|Indic_General_90_|
|Mohawk (Canada)|Mohawk_100_|Non disponible|
|Mongol (République populaire de Chine)|Cyrillic_General_100_|Non disponible|
|Népalais (Népal)|Nepali_100_<sup>1</sup>|Non disponible|
|Norvégien (bokmål, Norvège)|Norwegian_100_|Non disponible|
|Norvégien (Nynorsk, Norvège)|Norwegian_100_|Non disponible|
|Occitan (France)|French_100_|Non disponible|
|Odia (Inde)|Indic_General_100_<sup>1</sup>|Non disponible|
|Pachtou (Afghanistan)|Pashto_100_<sup>1</sup>|Non disponible|
|Persan (Iran)|Persian_100_|Non disponible|
|Pendjabi (Inde)|Indic_General_100_<sup>1</sup>|Indic_General_90_|
|Quechua (Bolivie)|Latin1_General_100_|Non disponible|
|Quechua (Équateur)|Latin1_General_100_|Non disponible|
|Quechua (Pérou)|Latin1_General_100_|Non disponible|
|Romanche (Suisse)|Romansh_100_|Non disponible|
|Same d'Inari (Finlande)|Sami_Sweden_Finland_100_|Non disponible|
|Sami (Lule, Norvège)|Sami_Norway_100_|Non disponible|
|Same de Lule (Suède)|Sami_Sweden_Finland_100_|Non disponible|
|Same du nord (Finlande)|Sami_Sweden_Finland_100_|Non disponible|
|Same du nord (Norvège)|Sami_Norway_100_|Non disponible|
|Same du nord (Suède)|Sami_Sweden_Finland_100_|Non disponible|
|Same de Skolt (Finlande)|Sami_Sweden_Finland_100_|Non disponible|
|Same du sud (Norvège)|Sami_Norway_100_|Non disponible|
|Same du sud (Suède)|Sami_Sweden_Finland_100_|Non disponible|
|Sanskrit (Inde)|Indic_General_100_<sup>1</sup>|Indic_General_90_|
|Serbe (Bosnie-Herzégovine, cyrillique)|Serbian_Cyrillic_100_|Non disponible|
|Serbe (Bosnie-Herzégovine, latin)|Serbian_Latin_100_|Non disponible|
|Serbe (Serbie, cyrillique)|Serbian_Cyrillic_100_|Non disponible|
|Serbe (latin, Serbie)|Serbian_Latin_100_|Non disponible|
|Sesotho sa Leboa/Sotho du Nord (Afrique du Sud)|Latin1_General_100_|Non disponible|
|Setswana/Tswana (Afrique du Sud)|Latin1_General_100_|Non disponible|
|Cingalais (Sri Lanka)|Indic_General_100_<sup>1</sup>|Non disponible|
|Swahili (Kenya)|Latin1_General_100_|Non disponible|
|Syriaque (Syrie)|Syriac_100_<sup>1</sup>|Syriac_90_|
|Tadjik (Tadjikistan)|Cyrillic_General_100_|Non disponible|
|Tamazight (Algérie, latin)|Tamazight_100_|Non disponible|
|Tamoul (Inde)|Indic_General_100_<sup>1</sup>|Indic_General_90_|
|Télougou (Inde)|Indic_General_100_<sup>1</sup>|Indic_General_90_|
|Tibétain (RPC)|Tibetan_100_<sup>1</sup>|Non disponible|
|Turkmène (Turkménistan)|Turkmen_100_|Non disponible|
|Ouïgour (RPC)|Uighur_100_|Non disponible|
|Haut-sorabe (Allemagne)|Upper_Sorbian_100_|Non disponible|
|Ourdou (Pakistan)|Urdu_100_|Non disponible|
|Gallois (Royaume-Uni)|Welsh_100_|Non disponible|
|Wolof (Sénégal)|French_100_|Non disponible|
|Xhosa (Afrique du Sud)|Latin1_General_100_|Non disponible|
|Iakoute (Russie)|Yakut_100_|Non disponible|
|Yi (RPC)|Latin1_General_100_|Non disponible|
|Yoruba (Nigeria)|Latin1_General_100_|Non disponible|
|Zoulou (Afrique du Sud)|Latin1_General_100_|Non disponible|
|Déconseillé, non disponible au niveau serveur dans [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] ou les versions ultérieures|Hindi|Hindi|
|Déconseillé, non disponible au niveau serveur dans [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] ou les versions ultérieures|Korean_Wansung_Unicode|Korean_Wansung_Unicode|
|Déconseillé, non disponible au niveau serveur dans [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] ou les versions ultérieures|Lithuanian_Classic|Lithuanian_Classic|
|Déconseillé, non disponible au niveau serveur dans [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] ou les versions ultérieures|Macédonien|Macédonien|
||||

<sup>1</sup> Les classements Windows Unicode seulement ne peuvent être appliqués qu’à des données de niveau colonne ou de niveau expression. Ils ne peuvent pas être utilisés en tant que classements de serveur ou de base de données.

<sup>2</sup>Comme le classement chinois (Taiwan), le chinois (Macao (R.A.S.)) utilise les règles du chinois simplifié ; contrairement au chinois (Taiwan), il utilise la page de codes 950.

## <a name="see-also"></a>Voir aussi

- [Prise en charge d'Unicode et du classement](../../relational-databases/collations/collation-and-unicode-support.md)
- [ALTER TABLE](../../t-sql/statements/alter-table-transact-sql.md)
- [Constantes](../../t-sql/data-types/constants-transact-sql.md)
- [CREATE DATABASE](../../t-sql/statements/create-database-transact-sql.md)
- [CREATE TABLE](../../t-sql/statements/create-table-transact-sql.md)
- [DECLARE @local_variable](../../t-sql/language-elements/declare-local-variable-transact-sql.md)
- [table](../../t-sql/data-types/table-transact-sql.md)
- [sys.fn_helpcollations](../../relational-databases/system-functions/sys-fn-helpcollations-transact-sql.md)
