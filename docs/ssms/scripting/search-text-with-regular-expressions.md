---
title: Rechercher du texte avec des expressions régulières
description: Découvrez comment utiliser une expression régulière dans le champ « Rechercher » d’une boîte de dialogue Rechercher et remplacer pour spécifier un modèle à mettre en correspondance.
ms.prod: sql
ms.technology: ssms
ms.topic: conceptual
f1_keywords:
- vs.regularexpressionbuilder
helpviewer_keywords:
- regular expressions [SQL Server Management Studio]
- Query Editor [SQL Server Management Studio], regular expression searches
- searches [SQL Server Management Studio], regular expressions
ms.assetid: a057690c-d118-4159-8e4d-2ed5ccfe79d3
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 03/14/2017
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: f09834f72bbcf5932961045a803c57dc9702a242
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100344169"
---
# <a name="search-text-with-regular-expressions"></a>Rechercher du texte avec des expressions régulières

[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Les expressions régulières sont une notation souple et concise pour rechercher et remplacer des modèles de texte. Un ensemble spécifique d'expressions régulières peut être utilisé dans le champ **Rechercher** de la boîte de dialogue **Rechercher et remplacer** de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)].

## <a name="find-using-regular-expressions"></a>Pour effectuer une recherche à l’aide d’expressions régulières
1. Pour pouvoir utiliser des expressions régulières dans le champ **Rechercher** pendant des opérations **Recherche rapide**, **Rechercher dans les fichiers**, **Remplacement rapide** ou **Remplacer dans les fichiers**, sélectionnez l’option **Utiliser** sous **Options de recherche**, puis choisissez **Expressions régulières**.

2. Le bouton triangulaire **Générateur d'expressions** situé en regard du champ **Rechercher** devient alors disponible. Cliquez sur ce bouton pour afficher la liste des expressions régulières les plus couramment utilisées. Lorsque vous choisissez un élément dans le Générateur d'expressions, il est inséré dans la chaîne **Rechercher** .

> [!NOTE]
> Il existe des différences de syntaxe entre les expressions régulières qui peuvent être utilisées dans les chaînes **Rechercher** et celles qui sont valides dans la programmation avec [!INCLUDE[msCoName](../../includes/msconame-md.md)] .NET Framework. Par exemple, dans **Rechercher et remplacer**, les accolades {} sont utilisées pour les expressions avec balises. Ainsi, l'expression « zo{1} » correspond à toutes les occurrences de « zo » suivies de la balise 1, comme dans « Alonzo1 » et « Gonzo1 ». En revanche, dans le .NET Framework, les accolades {} sont utilisées pour les quantificateurs. Ainsi, l'expression « zo{1} » correspond à toutes les occurrences de « z » suivies de la lettre « o », comme dans « zone » (et non « zoo »).

Le tableau ci-dessous décrit les expressions régulières disponibles dans le **Générateur d'expressions**.

|Expression|Syntaxe|Description|
|----------------|------------|-----------------|
|Tout caractère|.|Représente n'importe quel caractère, à l'exception du saut de ligne.|
|Zéro ou plus|*|Représente zéro ou plusieurs occurrences de l'expression précédente, en faisant correspondre toutes les occurrences possibles.|
|Un ou plus|+|Représente au moins une occurrence de l'expression précédente.|
|Début de ligne|^|Ancre la concordance au début d'une ligne.|
|Fin de ligne|$|Ancre la concordance à la fin d'une ligne.|
|Début de mot|\<|Représente la concordance uniquement lorsqu'un mot commence à cette position dans le texte.|
|Fin de mot|>|Représente la concordance uniquement lorsqu'un mot se termine à cette position dans le texte.|
|Saut de ligne|\n|Représente un saut de ligne indépendant de la plateforme. Dans une expression Remplacer, insère un saut de ligne.|
|Tout caractère de l'ensemble|[]|Représente tout caractère figurant entre les crochets []. Pour spécifier une plage de caractères, répertoriez les caractères de début et de fin, en les séparant par un tiret (-) ; par exemple [a-z].|
|Tout caractère hors de l'ensemble|[^...]|Représente tout caractère ne figurant pas dans le jeu de caractères qui suit le signe ^.|
|ou|&#124;|Représente l’expression placée avant ou après le symbole OU (&#124;). Critère surtout utilisé à l'intérieur d'un groupe. Par exemple, compte (courant&#124;rendu) retourne « compte courant » et « compte rendu ».|
|Caractère d'échappement|\|Représente le caractère qui suit la barre oblique inverse (\\) comme littéral. Cette syntaxe vous permet de rechercher les caractères utilisés dans les expressions régulières, tels { et ^. Par exemple, \\^ recherche le caractère ^.|
|Expression avec balises|{}|Représente le texte avec les balises de l'expression.|
|Identificateur C/C++|:i|Représente l'expression ([a-zA-Z_$][a-zA-Z0-9_$]*).|
|Chaîne entre guillemets|:q|Représente l’expression (("[^"]*")&#124;('[^']\*')).|
|Espace ou tabulation|:b|Représente des caractères de tabulation ou d'espace.|
|Integer|:z|Représente l'expression ([0-9]+).|

La liste de toutes les expressions régulières valides dans les opérations **Rechercher et remplacer** est beaucoup trop longue pour être affichée intégralement dans le **Générateur d'expressions**. Vous pouvez également insérer les expressions régulières suivantes dans une chaîne **Rechercher** :

|Expression|Syntaxe|Description|
|----------------|------------|-----------------|
|Zéro ou plusieurs occurrences au minimum|@|Représente zéro ou plusieurs occurrences de l'expression précédente, en faisant correspondre le moins de caractères possible.|
|Une ou plusieurs occurrences au minimum|#|Représente au moins une occurrence de l'expression précédente, en faisant correspondre le moins de caractères possible.|
|Répétition n fois|^n|Représente n occurrences de l'expression précédente. Par exemple, [0-9]^4 représente toute séquence à quatre chiffres.|
|Regroupement|()|Regroupe une sous-expression.|
|Énième texte avec balises|\n|Dans une expression **Rechercher ou Remplacer** , recherche la concordance du texte correspondant à la énième expression avec balises, où n désigne un chiffre compris entre 1 et 9.<br /><br /> Dans une expression **Remplacer** , \0 insère le texte correspondant à l’expression entière.|
|Champ justifié à droite|\\(w,n)|Dans une expression **Remplacer** , aligne à droite la énième expression avec balises dans un champ comportant au moins *w* caractères.|
|Champ justifié à gauche|\\(-w,n)|Dans une expression **Remplacer** , aligne à gauche la énième expression avec balises dans un champ comportant au moins *w* caractères.|
|Empêcher la concordance|~(X)|Empêche la recherche d'une concordance quand le caractère X apparaît à cet endroit dans l'expression. Par exemple, réal~(ité) correspond à « réal » dans « réalisme » et « réalisation », mais pas à « réal » dans « réalité ».|
|Caractère alphanumérique|:a|Représente l'expression ([a-zA-Z0-9]).|
|Caractère alphabétique|:c|Représente l'expression ([a-zA-Z]).|
|Chiffre décimal|:d|Représente l'expression ([0-9]).|
|Chiffre hexadécimal|:h|Représente l'expression ([0-9a-fA-F]+).|
|Nombre rationnel|:n|Représente l’expression (([0-9]+.[0-9]*)&#124;([0-9]\*.[0-9]+)&#124;([0-9]+)).|
|Séquence de caractères alphabétiques|:w|Représente l'expression ([a-zA-Z]+).|
|Caractère d'échappement|\e|Unicode U+001B.|
|Bell|\g|Unicode U+0007.|
|Retour arrière|\h|Unicode U+0008.|
|Onglet|\t|Représente un caractère de tabulation, Unicode U+0009.|
|Caractère Unicode|\x#### ou \u####|Représente un caractère donné selon une valeur Unicode où #### désigne 1 à 4 chiffres hexadécimaux. Vous pouvez spécifier un caractère n'appartenant pas au plan BMP (Basic Multilingual Plane), autrement dit, un substitut à l'aide du point de code ISO 10646 ou de deux points de code Unicode spécifiant les valeurs de la paire de substitution.|

Le tableau suivant répertorie la syntaxe utilisée pour les opérations de recherche à l'aide de propriétés de caractères Unicode standard. L'abréviation de deux lettres est identique à celle figurant dans la base de données de propriétés de caractères Unicode. Ces abréviations peuvent être spécifiées en tant qu'élément d'un jeu de caractères. Ainsi, l'expression [:Nd:Nl:No] représente n'importe quel type de chiffre.

|Expression|Syntaxe|Description|
|----------------|------------|-----------------|
|Lettre majuscule|:Lu|Représente n'importe quelle lettre majuscule. Par exemple, Lues retourne « Les » mais pas « les ».|
|Lettre minuscule|:Ll|Représente n'importe quelle lettre minuscule. Par exemple, :Lles retourne « les » mais pas « Les ».|
|Lettre capitale|:Lt|Représente les caractères associant une lettre majuscule à une lettre minuscule, par exemple Nj et Dz.|
|Lettre modificative|:Lm|Représente des lettres ou de la ponctuation telle que des virgules, des accents croisés, et guillemets doubles qui servent à indiquer les modifications apportées à la lettre précédente.|
|Autre lettre|:Lo|Représente d'autres lettres, telles que la lettre gothique ahsa.|  
|Chiffre décimal|:Nd|Représente des chiffres décimaux comme 0 à 9 et leurs équivalents pleine largeur.|
|Chiffre en lettre|:Nl|Représente des chiffres lettres tels que les chiffres romains et le chiffre zéro idéographique.|
|Autre chiffre|:No|Représente d'autres chiffres tels que l'ancien chiffre un en italiques.|
|Ponctuation initiale|:Ps|Représente une ponctuation initiale comme des parenthèses ou des crochets ouvrants.|
|Ponctuation finale|:Pe|Représente une ponctuation finale comme des parenthèses ou des crochets fermants.|
|Guillemets de ponctuation initiale|:Pi|Représente des guillemets doubles ouvrants.|
|Guillemets de ponctuation finale|:Pf|Représente des guillemets simples et des guillemets doubles fermants.|
|Tiret de ponctuation|:Pd|Représente le tiret.|
|Ponctuation de connexion|:Pc|Représente le trait de soulignement.|
|Autre ponctuation|:Po|Représente les symboles (,), ?, ", !, @, #, %, &, *, \\, (:), (;), ' et /.|
|Espace de séparation|:Zs|Représente les espaces vides.|
|Séparateur de ligne|:Zl|Représente le caractère Unicode U+2028.|
|Séparateur de paragraphe|:Zp|Recherche le caractère Unicode U+2029.|
|Marqueur sans espace|:Mn|Représente des marqueurs sans espace.|
|Marque d'association|:Mc|Représente des marques d'association.|
|Marque de délimitation|:Me|Représente des marques de délimitation.|
|Symbole mathématique|:Sm|Représente +, =, ~, &#124;, \<, and >.|
|Symbole monétaire|:Sc|Représente le symbole $ et autres symboles monétaires.|
|Symbole modificatif|:Sk|Représente des symboles modificateurs tels que l'accent circonflexe, l'accent grave et le trait supérieur.|
|Autre symbole|:So|Représente d'autres symboles tels que les signes du copyright, du paragraphe et du degré.|
|Autre contrôle|:Cc|Représente une fin de ligne.|
|Autre format|:Cf|Caractère de contrôle de mise en forme telle que les caractères de contrôle bidirectionnels.|
|Substitut|:Cs|Représente un élément d'une paire de substitution.|
|Autre usage privé|:Co|Représente n'importe quel caractère de la zone d'utilisation privée.|
|Autre non affecté|:Cn|Représente des caractères qui ne correspondent pas à un caractère Unicode.|
|Spécifie le nombre d'occurrences du caractère ou du groupe précédent. Pour plus d’informations, consultez [Mettre en correspondance exactement n occurrences](/dotnet/standard/base-types/quantifiers-in-regular-expressions#match-exactly-n-times-n).|{n}, où n est le nombre d'occurrences|`x(ab){2}x` correspond à « xababx »<br/>`x(ab){2,3}x` correspond à « xababx » et à « xabababx », mais pas à « xababababx »|

Outre les propriétés de caractères Unicode standard, vous pouvez également spécifier les propriétés suivantes dans un jeu de caractères.

|Expression|Syntaxe|Description|
|----------------|------------|-----------------|
|Alpha|:Al|Représente n'importe quel caractère. Par exemple, :Alar retourne des mots comme « Par », « partie » ou « épargne ».|
|Numérique|:Nu|Représente n'importe quel nombre ou chiffre.|
|Ponctuation|:Pu|Représente n'importe quel signe de ponctuation tel que ?, @, ', etc.|
|Espace blanc|:Wh|Représente n'importe quel type d'espace blanc, y compris les espaces typographiques et idéographiques.|
|Bidirectionnel|:Bi|Représente les caractères d'un script se lisant de droite à gauche (langue arabe ou hébraïque).|
|Hangul|:Ha|Représente le Jamo coréen hangûl et Jamo d'association.|
|Hiragana|:Hi|Représente les caractères hiragana.|
|Katakana|:Ka|Représente les caractères Katakana.|
|Idéographique/Han/Kanji|:Id|Représente des caractères idéographiques tels Han et Kanji.|

## <a name="see-also"></a>Voir aussi  
- [Recherche et remplacement](./search-and-replace.md)
- [Rechercher du texte avec des caractères génériques](./search-text-with-wildcards.md)