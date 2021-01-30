---
description: 'Annexe B : Tableaux des transitions d’état ODBC'
title: 'Annexe B : tables de transition d’État ODBC | Microsoft Docs'
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- state transitions [ODBC]
- transitioning states [ODBC], about state transitions
- state transitions [ODBC], about state transitions
ms.assetid: 15088dbe-896f-4296-b397-02bb3d0ac0fb
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 67c14205590ccdf9d20a30f44c13aa2da5abbe8d
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99212571"
---
# <a name="appendix-b-odbc-state-transition-tables"></a>Annexe B : Tableaux des transitions d’état ODBC
Les tableaux de cette annexe montrent comment les fonctions ODBC entraînent des transitions entre les États de l’environnement, de la connexion, de l’instruction et du descripteur. L’état de l’environnement, de la connexion, de l’instruction ou du descripteur dicte généralement le moment où les fonctions qui utilisent le type de handle correspondant (environnement, connexion, instruction ou descripteur) peuvent être appelées. Les États de l’environnement, de la connexion, de l’instruction et du descripteur se chevauchent à peu près comme indiqué dans les illustrations suivantes. Par exemple, le chevauchement exact des États de connexion C5 et C6 et des États d’instruction S1 à S12 est dépendant de la source de données, car les transactions commencent à différents moments sur des sources de données différentes, et l’état du descripteur D1i (descripteur alloué de manière implicite) dépend de l’état de l’instruction à laquelle le descripteur est associé. Pour obtenir une description de chaque État, consultez [transitions d’environnement](../../../odbc/reference/appendixes/environment-transitions.md), [transitions de connexion](../../../odbc/reference/appendixes/connection-transitions.md), [transitions d’instructions](../../../odbc/reference/appendixes/statement-transitions.md)et [transitions de descripteur](../../../odbc/reference/appendixes/descriptor-transitions.md), plus loin dans cette annexe.  
  
 L’environnement et les États de connexion se chevauchent comme suit :  
  
 ![Chevauchement des états d'environnement et de connexion](../../../odbc/reference/appendixes/media/app01.gif "app01")  
  
 Les États de connexion et d’instruction se chevauchent comme suit :  
  
 ![Chevauchement des états de connexion et d'instruction](../../../odbc/reference/appendixes/media/app02.gif "app02")  
  
 Les États de l’instruction et du descripteur se chevauchent comme suit :  
  
 ![Chevauchement des états d'instruction et de descripteur](../../../odbc/reference/appendixes/media/app03.gif "app03")  
  
 Les États de connexion et de descripteur se chevauchent comme suit :  
  
 ![Chevauchement des états de connexion et de descripteur](../../../odbc/reference/appendixes/media/app04.gif "app04")  
  
 Chaque entrée d’une table de transition peut prendre l’une des valeurs suivantes :  
  
-   **--** -L’État est inchangé après l’exécution de la fonction.  
  
-   **E**  

     **_n_** , **C_n_**, **S_n_** ou **D_n_** -l’état de l’environnement, de la connexion, de l’instruction ou du descripteur passe à l’état spécifié.  
 
-   **(** Au-dessus de)-un handle non valide a été passé à la fonction. Si le handle était un handle null ou était un handle valide du mauvais type, par exemple, un descripteur de connexion était passé lorsqu’un descripteur d’instruction était requis : la fonction retourne SQL_INVALID_HANDLE ; dans le cas contraire, le comportement est indéfini et probablement fatal. Cette erreur s’affiche uniquement lorsqu’il s’agit du seul résultat possible de l’appel de la fonction dans l’état spécifié. Cette erreur ne modifie pas l’État et est toujours détectée par le gestionnaire de pilotes, comme indiqué par les parenthèses.  
  
-   État de **NS** -Next. La transition d’instruction est la même que si l’instruction n’avait pas dépassé les États asynchrones. Par exemple, supposons qu’une instruction qui crée un jeu de résultats passe à l’État S11 à partir de l’état S1, car **SQLExecDirect** retournait SQL_STILL_EXECUTING. La notation NS dans l’État S11 signifie que les transitions de l’instruction sont identiques à celles d’une instruction dans l’état S1 qui crée un jeu de résultats. Si **SQLExecDirect** retourne une erreur, l’instruction reste dans l’état S1 ; Si elle est réussie, l’instruction passe à l’État S5 ; Si des données sont nécessaires, l’instruction passe à l’État S8 ; et s’il est toujours en cours d’exécution, il reste dans l’État S11.  

-   **_Xxxxx_**  ou **(*xxxxx*)** : SQLSTATE qui est lié à la table de transition ; Les SQLSTATEs détectées par le gestionnaire de pilotes sont mises entre parenthèses. La fonction a retourné SQL_ERROR et la SQLSTATE spécifiée, mais l’État ne change pas. Par exemple, si **SQLExecute** est appelé avant **SQLPrepare**, il retourne SQLState HY010 (erreur de séquence de fonction).  

> [!NOTE]  
>  Les tables n’affichent pas les erreurs qui ne sont pas liées aux tables de transition qui ne changent pas d’État. Par exemple, lorsque **SQLAllocHandle** est appelé dans l’état d’environnement E1 et retourne SQLState HY001 (erreur d’allocation de mémoire), l’environnement reste dans l’État E1. Cela n’est pas indiqué dans la table de transition de l’environnement pour **SQLAllocHandle**.  
  
 Si l’environnement, la connexion, l’instruction ou le descripteur peut passer à plusieurs États, chaque état possible est affiché et une ou plusieurs notes de bas de page décrivent les conditions dans lesquelles chaque transition est effectuée. Les notes de bas de page suivantes peuvent apparaître dans n’importe quel tableau.  
  
|Appel|Signification|  
|--------------|-------------|  
|b|Avant ou après. Le curseur a été placé avant le début du jeu de résultats ou après la fin du jeu de résultats.|  
|c|Fonction active. La fonction active s’exécutait de manière asynchrone.|  
|d|Données requises. La fonction a retourné SQL_NEED_DATA.|  
|e|Erreur. La fonction a retourné SQL_ERROR.|  
|i|Ligne non valide. Le curseur a été positionné sur une ligne dans le jeu de résultats et soit la ligne a été supprimée, soit une erreur s’est produite dans une opération sur la ligne. Si le tableau d’état de ligne existait, la valeur dans le tableau d’état de ligne pour la ligne était SQL_ROW_DELETED ou SQL_ROW_ERROR. (Le tableau d’état de ligne est désigné par l’attribut d’instruction SQL_ATTR_ROW_STATUS_PTR.)|  
|nf|Introuvable. La fonction a retourné SQL_NO_DATA. Cela ne s’applique pas lorsque **SQLExecDirect**, **SQLExecute** ou **SQLParamData** retourne SQL_NO_DATA après l’exécution d’une instruction UPDATE ou DELETE recherchée.|  
|np|Non préparé. L’instruction n’a pas été préparée.|  
|nr|Aucun résultat. L’instruction ne crée pas ou n’a pas créé de jeu de résultats.|  
|o|Autre fonction. Une autre fonction s’exécutait de manière asynchrone.|  
|p|Coïncid. L’instruction a été préparée.|  
|r|Résultats. L’instruction crée ou crée un jeu de résultats (éventuellement vide).|  
|s|Réussite. La fonction a retourné SQL_SUCCESS_WITH_INFO ou SQL_SUCCESS.|  
|v|Ligne valide. Le curseur a été placé sur une ligne dans le jeu de résultats, et la ligne a été correctement insérée, mise à jour avec succès ou une autre opération sur la ligne a été correctement effectuée. Si le tableau d’état de ligne existait, la valeur dans le tableau d’état de ligne pour la ligne était SQL_ROW_ADDED, SQL_ROW_SUCCESS ou SQL_ROW_UPDATED. (Le tableau d’état de ligne est désigné par l’attribut d’instruction SQL_ATTR_ROW_STATUS_PTR.)|  
|x|En cours d'exécution. La fonction a retourné SQL_STILL_EXECUTING.|  
  
## <a name="sqlfreehandle"></a>SQLFreeHandle  
 Dans cet exemple, la ligne de la table de transition d’état de l’environnement pour **SQLFreeHandle** quand *comme HandleType* est SQL_HANDLE_ENV est la suivante.  
  
|E0<br /><br /> Non alloué|E1<br /><br /> Allocated|E2<br /><br /> Connexion|  
|------------------------|----------------------|-----------------------|  
|IH|E0|HY010|  
  
 Si **SQLFreeHandle** est appelé dans l’état d’environnement E0 avec *comme handletype* défini sur SQL_HANDLE_ENV, le gestionnaire de pilotes retourne SQL_INVALID_HANDLE. S’il est appelé dans l’État E1 où *comme HandleType* a la valeur SQL_HANDLE_ENV, l’environnement passe à l’État E0 si la fonction réussit et reste dans l’État E1 si la fonction échoue. S’il est appelé dans l’État E2 avec *comme HandleType* défini sur SQL_HANDLE_ENV, le gestionnaire de pilotes retourne toujours SQL_ERROR et SQLSTATE HY010 (erreur de séquence de fonction) et l’environnement reste dans l’État E2.  
  
 Pour comprendre les tables de transition d’État, il est nécessaire de comprendre l’élément (environnement, connexion, instruction ou descripteur) auquel ils font référence. Supposons qu’une fonction accepte le descripteur d’un élément de type X. La table de transition d’État X pour cette fonction décrit comment l’appel de la fonction, avec le handle d’un élément de type X, affecte cet élément. Par exemple, **SQLDisconnect** accepte un handle de connexion. La table de transition d’état de connexion pour **SQLDisconnect** décrit comment **SQLDisconnect** affecte l’état de la connexion pour laquelle elle est appelée.  
  
 Supposons qu’une fonction accepte le descripteur d’un élément de type Y, où Y n’est pas égal à X. La table de transition d’État X pour cette fonction décrit comment l’appel de la fonction, avec un handle de type X associé à l’élément de type Y, affecte l’élément de type Y. Par exemple, la table de transition d’état d’instruction pour **SQLDisconnect** décrit comment **SQLDisconnect** affecte l’état d’une instruction quand elle est appelée avec le handle de la connexion à laquelle l’instruction est associée.  
  
 Cette annexe contient les rubriques suivantes.  
  
-   [Transitions d’environnement](../../../odbc/reference/appendixes/environment-transitions.md)  
  
-   [Transitions de connexion](../../../odbc/reference/appendixes/connection-transitions.md)  
  
-   [Transitions d’instruction](../../../odbc/reference/appendixes/statement-transitions.md)  
  
-   [Transitions de descripteur](../../../odbc/reference/appendixes/descriptor-transitions.md)
