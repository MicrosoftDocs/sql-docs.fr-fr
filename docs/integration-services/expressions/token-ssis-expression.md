---
description: TOKEN  (expression SSIS)
title: TOKEN (expression SSIS) | Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
ms.assetid: 9fdd06bf-5bc9-445c-95bf-709e0ca5989b
author: chugugrace
ms.author: chugu
ms.openlocfilehash: cc99e599596658019c073b67dbf39b6dd7e573ce
ms.sourcegitcommit: 1f826eb3f73bd4d94bc9638b9cdd60991a2e2fa0
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/12/2021
ms.locfileid: "98125600"
---
# <a name="token--ssis-expression"></a>TOKEN  (expression SSIS)

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  Retourne un jeton (sous-chaîne) à partir d'une chaîne en fonction des délimiteurs spécifiés qui séparent les jetons de la chaîne et du numéro de jeton qui indique le jeton à retourner.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
TOKEN(character_expression, delimiter_string, occurrence)  
```  
  
## <a name="arguments"></a>Arguments  
 *expression_caractère*  
 Chaîne qui contient des jetons séparés par des délimiteurs.  
  
 *delimiter_string*  
 Chaîne qui contient les caractères de délimitation. Par exemple, « ; , » contient trois caractères de délimitation : le point-virgule, un espace vide et une virgule.  
  
 *occurrence*  
 Entier signé ou non signé qui spécifie le jeton à retourner. Par exemple, si vous spécifiez 3 comme valeur pour ce paramètre, le troisième jeton de la chaîne est retourné.  
  
## <a name="result-types"></a>Types des résultats  
 DT_WSTR  
  
## <a name="remarks"></a>Notes  
 Cette fonction fractionne la chaîne <character_expression> en un ensemble de jetons séparés par les délimiteurs spécifiés dans la chaîne <delimiter_string>, puis retourne le N-ième jeton, où N est le nombre d’occurrences du jeton spécifié par le paramètre \<occurrence>. Consultez la section Exemples pour obtenir des exemples d'utilisation de cette fonction.  
  
 Les remarques suivantes s'appliquent à la fonction TOKEN :  
  
-   La chaîne de délimitation peut contenir un ou plusieurs caractères de délimitation.  
  
-   Si la valeur du paramètre \<occurrence> est supérieure au nombre total de jetons présents dans la chaîne, la fonction retourne NULL.  
  
-   Les délimiteurs de début sont ignorés.  
  
-   La fonction TOKEN fonctionne seulement avec le type de données DT_WSTR. Un argument *character_expression* représentant un littéral de chaîne ou une colonne de données du type de données DT_STR est implicitement converti dans le type de données DT_WSTR avant que la fonction TOKEN ne soit exécutée. Les autres types de données doivent être explicitement convertis vers le type de données DT_WSTR.  
  
-   La fonction TOKEN retourne un résultat Null si l'argument character_expression est Null.  
  
-   Vous pouvez utiliser des variables et des colonnes comme valeurs de tous les arguments de l'expression.  
  
## <a name="expression-examples"></a>Exemples d'expressions  
 Dans l’exemple suivant, la fonction TOKEN retourne « a ». La chaîne « a little white dog » comprend 4 jetons (« a », « little », « white », « dog ») séparés par le délimiteur «   » (espace). Le deuxième argument, une chaîne de délimitation, spécifie un seul délimiteur, l'espace, à utiliser dans le fractionnement de la chaîne d'entrée en jetons. Le dernier argument, 1, indique le premier jeton à retourner. Le premier jeton est « a » dans cet exemple de chaîne.  
  
```  
TOKEN("a little white dog"," ",1)  
```  
  
 Dans l'exemple suivant, la fonction TOKEN retourne « dog ». La chaîne de délimitation de cet exemple contient 5 délimiteurs. La chaîne d’entrée contient 4 jetons : « a », « little », « white », « dog ».  
  
```  
TOKEN("a:little|white dog","| ,.:",4)  
```  
  
 Dans l'exemple suivant, la fonction TOKEN retourne «   » (une chaîne vide), car il n'y a pas 99 jetons dans la chaîne.  
  
```  
TOKEN("a little white dog"," ",99)  
```  
  
 Dans l'exemple suivant, la fonction TOKEN retourne la chaîne complète. La fonction analyse la chaîne d'entrée à la recherche de délimiteurs et comme il n'y en a pas dans la chaîne, elle ajoute simplement la chaîne entière en tant que premier jeton.  
  
```  
TOKEN("a little white dog","|",1)  
```  
  
 Dans l’exemple suivant, la fonction TOKEN retourne « a ». Elle ignore tous les espaces de début.  
  
```  
TOKEN("        a little white dog", " ", 1)  
```  
  
 Dans l'exemple suivant, la fonction TOKEN retourne l'année spécifiée dans une chaîne de date.  
  
```  
TOKEN("2009/01/01", "/", 1)
```  
  
 Dans l'exemple suivant, la fonction TOKEN retourne le nom de fichier figurant dans le chemin d'accès spécifié. Par exemple, si la valeur de User::Path est « c:\program files\data\myfile.txt », la fonction TOKEN retourne « myfile.txt ». La fonction TOKENCOUNT retourne 4 et la fonction TOKEN retourne le 4ème jeton, « myfile.txt ».  
  
```  
TOKEN(@[User::Path], "\\", TOKENCOUNT(@[User::Path], "\\"))  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Fonctions &#40;expression SSIS&#41;](../../integration-services/expressions/functions-ssis-expression.md)  
  
  
