---
description: Fonctions de chaîne
title: Fonctions de chaîne | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- functions [ODBC], string functions
- string functions [ODBC]
ms.assetid: 270f669e-8aab-4db0-95a4-f2b3c69538b3
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: fe6c50c7ad96f1025b27f60e2de74ba31f60698f
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99202517"
---
# <a name="string-functions"></a>Fonctions de chaîne
Le tableau suivant répertorie les fonctions de manipulation de chaînes. Une application peut déterminer les fonctions de chaîne prises en charge par un pilote en appelant **SQLGetInfo** avec un *type d’informations* SQL_STRING_FUNCTIONS.  
  
## <a name="remarks"></a>Notes  
 Les arguments dénotés comme *string_exp* peuvent être le nom d’une colonne, un *littéral de chaîne de caractères* ou le résultat d’une autre fonction scalaire, où le type de données sous-jacent peut être représenté sous la forme SQL_CHAR, SQL_VARCHAR ou SQL_LONGVARCHAR.  
  
 Les arguments dénotés comme *character_exp* sont une chaîne de caractères de longueur variable.  
  
 Les arguments dénotés comme *Start*, *Length* ou *Count* peuvent être un *littéral numérique* ou le résultat d’une autre fonction scalaire, où le type de données sous-jacent peut être représenté sous la forme SQL_TINYINT, SQL_SMALLINT ou SQL_INTEGER.  
  
 Les fonctions de chaîne répertoriées ici sont de base 1 ; autrement dit, le premier caractère de la chaîne est le caractère 1.  
  
 Les fonctions scalaires BIT_LENGTH, CHAR_LENGTH, CHARACTER_LENGTH, OCTET_LENGTH et POSITION ont été ajoutées dans ODBC 3,0 pour s’aligner sur SQL-92.  
  
|Fonction|Description|  
|--------------|-----------------|  
|**ASCII (** _string_exp_ **)**  (ODBC 1,0)|Retourne la valeur de code ASCII du caractère le plus à gauche de *string_exp* sous la forme d’un entier.|  
|**BIT_LENGTH (** _string_exp_ **)**  (ODBC 3,0)|Retourne la longueur en bits de l'expression de chaîne.<br /><br /> Ne fonctionne pas uniquement pour les types de données de type chaîne. par conséquent, ne convertit pas implicitement *string_exp* en chaîne, mais retourne à la place la taille (interne) du type de données qu’il reçoit.|  
|**Char (** _code_ **)**  (ODBC 1,0)|Retourne le caractère qui a la valeur de code ASCII spécifiée par le *code*. La valeur du *code* doit être comprise entre 0 et 255 ; dans le cas contraire, la valeur de retour est dépendante de la source de données.|  
|**CHAR_LENGTH (** _string_exp_ **)**  (ODBC 3,0)|Retourne la longueur en caractères de l’expression de chaîne, si l’expression de chaîne est de type de données character ; sinon, retourne la longueur en octets de l’expression de chaîne (le plus petit entier qui n’est pas inférieur au nombre de bits divisé par 8). (Cette fonction est la même que la fonction CHARACTER_LENGTH.)|  
|**CHARACTER_LENGTH (** _string_exp_ **)**  (ODBC 3,0)|Retourne la longueur en caractères de l’expression de chaîne, si l’expression de chaîne est de type de données character ; sinon, retourne la longueur en octets de l’expression de chaîne (le plus petit entier qui n’est pas inférieur au nombre de bits divisé par 8). (Cette fonction est la même que la fonction CHAR_LENGTH.)|  
|**Concat (** _string_exp1_,_string_exp2_**)**  (ODBC 1,0)|Retourne une chaîne de caractères qui est le résultat de la concaténation de *string_exp2* à *string_exp1*. La chaîne résultante dépend de SGBD. Par exemple, si la colonne représentée par *string_exp1* contenait une valeur null, DB2 retournerait null, mais SQL Server retournerait la chaîne non null.|  
|**Difference (** _string_exp1_,_string_exp2_**)**  (ODBC 2,0)|Retourne une valeur entière qui indique la différence entre les valeurs retournées par la fonction SOUNDEX pour *string_exp1* et *string_exp2*.|  
|**Insert (** _string_exp1_, *Start*, *Length*, _string_exp2_**)**  (ODBC 1,0)|Retourne une chaîne de caractères où les caractères de *longueur* ont été supprimés de *string_exp1*, à partir de *Start*, et où *string_exp2* a été inséré dans *string_exp,* en commençant au *début*.|  
|**LCase (** _string_exp_ **)** (ODBC 1,0)|Retourne une chaîne égale à celle de *string_exp*, avec tous les caractères majuscules convertis en minuscules.|  
|**Left (** _string_exp_, _Count_**)** (ODBC 1,0)|Retourne le *nombre* de caractères le plus à gauche de *string_exp*.|  
|**Longueur (** _string_exp_ **)** (ODBC 1,0)|Retourne le nombre de caractères dans *string_exp,* à l’exception des espaces blancs de fin.<br /><br /> La **longueur** accepte uniquement les chaînes. Par conséquent, convertit implicitement *string_exp* en chaîne et retourne la longueur de cette chaîne (et non la taille interne du type de données).|  
|**Locate (** _string_exp1_, *string_exp2*[, *Start*]**)** (ODBC 1,0)|Retourne la position de départ de la première occurrence de *string_exp1* dans *string_exp2*. La recherche de la première occurrence de *string_exp1* commence par la position du premier caractère dans *string_exp2* , sauf si l’argument facultatif, *Start*, est spécifié. Si *Start* est spécifié, la recherche commence par la position de caractère indiquée par la valeur de *Start*. La première position de caractère dans *string_exp2* est indiquée par la valeur 1. Si *string_exp1* est introuvable dans *string_exp2*, la valeur 0 est retournée.<br /><br /> Si une application peut appeler la fonction LOCATe Scala avec les arguments *string_exp1*, *string_exp2* et *start* , le pilote retourne SQL_FN_STR_LOCATE lorsque **SQLGetInfo** est appelé avec une *option* de SQL_STRING_FUNCTIONS. Si l’application peut appeler la fonction LOCATe Scala avec uniquement les arguments *string_exp1* et *string_exp2* , le pilote retourne SQL_FN_STR_LOCATE_2 lorsque **SQLGetInfo** est appelé avec une *option* de SQL_STRING_FUNCTIONS. Les pilotes qui prennent en charge l’appel de la fonction LOCATe avec deux ou trois arguments retournent à la fois SQL_FN_STR_LOCATE et SQL_FN_STR_LOCATE_2.|  
|**LTRIM (** _string_exp_ **)** (ODBC 1,0)|Retourne les caractères de *string_exp*, avec les espaces de début supprimés.|  
|**OCTET_LENGTH (** _string_exp_ **)** (ODBC 3,0)|Retourne la longueur en octets de l'expression de chaîne. Le résultat est le plus petit entier qui n'est pas inférieur au nombre de bits divisé par 8.<br /><br /> Ne fonctionne pas uniquement pour les types de données de type chaîne. par conséquent, ne convertit pas implicitement *string_exp* en chaîne, mais retourne à la place la taille (interne) du type de données qu’il reçoit.|  
|**Position (** _character_exp_ **dans** _character_exp_**)** (ODBC 3,0)|Retourne la position de la première expression de caractères dans la deuxième expression de caractères. Le résultat est une valeur numérique exacte avec une précision définie par l’implémentation et une échelle de 0.|  
|**REPEAT (** _string_exp,_ _Count_**)** (ODBC 1,0)|Retourne une chaîne de caractères composée de *string_exp* fois les *nombres* répétés.|  
|**Replace (** _string_exp1_, *string_exp2*, _string_exp3_**)** (ODBC 1,0)|Recherchez *string_exp1* foroccurrences de *string_exp2* et remplacez par *string_exp3*.|  
|**Right (** _string_exp_, _Count_**)** (ODBC 1,0)|Retourne le *nombre* de caractères le plus à droite de *string_exp*.|  
|**Rtrim (** _string_exp_ **)** (ODBC 1,0)|Retourne les caractères de *string_exp* avec suppression des espaces à droite.|  
|**SOUNDEX (** _string_exp_ **)** (ODBC 2,0)|Retourne une chaîne de caractères dépendante de la source de données représentant le son des mots dans *string_exp*. Par exemple, SQL Server retourne un code SOUNDEX à 4 chiffres ; Oracle retourne une représentation phonétique de chaque mot.|  
|**Espace (** _nombre_ **)** (ODBC 2,0)|Retourne une chaîne de caractères composée d’espaces de *nombre* .|  
|**Substring (** _string_exp_, *début*, longueur **)** (ODBC 1,0)|Retourne une chaîne de caractères dérivée de *string_exp*, en commençant à la position de caractère spécifiée par *Start* pour les caractères de *longueur* .|  
|**UCase (** _string_exp_ **)** (ODBC 1,0)|Retourne une chaîne égale à celle de *string_exp*, avec tous les caractères minuscules convertis en majuscules.|
