---
description: SQLGetInfo, prise en charge
title: Prise en charge de SQLGetInfo | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- compatibility [ODBC], SQLGetInfo
- backward compatibility [ODBC], SQLGetInfo
- SQLGetInfo function [ODBC], support
ms.assetid: 57326f57-daba-46b6-b0be-6c97213b9ef1
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 1dc92c6bd409829da13395b1286a5762e4504ced
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99202744"
---
# <a name="sqlgetinfo-support"></a>SQLGetInfo, prise en charge
Quand ODBC 2. *x* appelle **SQLGetInfo** sur un pilote ODBC 3 *. x* , les arguments d' *infotype* dans le tableau suivant doivent être pris en charge.  
  
|*TypeInfo*|Retours|  
|----------------|-------------|  
|SQL_ALTER_TABLE (ODBC 2,0) **Remarque :**  ce type d’information n’est pas déconseillé ; les masques de la colonne à droite sont déconseillés.|Un masque de SQLINTEGER destinée qui énumère les clauses de l’instruction **ALTER TABLE** prise en charge par la source de données.<br /><br /> Les masques de détours suivants sont utilisés pour déterminer quelles clauses sont prises en charge :<br /><br /> SQL_AT_DROP_COLUMN = la possibilité de supprimer des colonnes est prise en charge. La définition de ce résultat en cascade ou en mode restreint est définie par le pilote. (ODBC 2,0)<br /><br /> SQL_AT_ADD_COLUMN = la possibilité d’ajouter plusieurs colonnes dans une seule instruction ALTER TABLE est prise en charge. Ce bit n’est pas combiné avec d’autres bits de SQL_AT_ADD_COLUMN_XXX ou SQL_AT_CONSTRAINT_XXX bits. (ODBC 2,0)|  
|SQL_FETCH_DIRECTION (ODBC 1,0)<br /><br /> Le type d’informations a été introduit dans ODBC 1,0 ; Chaque masque de masque est étiqueté avec la version dans laquelle il a été introduit.|Masque de SQLINTEGER destinée qui énumère les options de direction de récupération prises en charge.<br /><br /> Les masques de transparence suivants sont utilisés conjointement avec l’indicateur pour déterminer les options prises en charge :<br /><br /> SQL_FD_FETCH_NEXT (ODBC 1,0) SQL_FD_FETCH_FIRST (ODBC 1,0) SQL_FD_FETCH_LAST (ODBC 1,0) SQL_FD_FETCH_PRIOR (ODBC 1,0) SQL_FD_FETCH_ABSOLUTE (ODBC 1,0) SQL_FD_FETCH_RELATIVE (ODBC 1,0) SQL_FD_FETCH_BOOKMARK (ODBC 2,0)|  
|SQL_LOCK_TYPES (ODBC 2,0)|Un masque de SQLINTEGER destinée qui énumère les types de verrous pris en charge pour l’argument de *troupeau* dans **SQLSetPos**.<br /><br /> Les masques de transparence suivants sont utilisés conjointement avec l’indicateur pour déterminer les types de verrous pris en charge :<br /><br /> SQL_LCK_NO_CHANGE SQL_LCK_EXCLUSIVE SQL_LCK_UNLOCK|  
|SQL_ODBC_API_CONFORMANCE (ODBC 1,0)|Valeur SQLSMALLINT indiquant le niveau de conformité ODBC.<br /><br /> SQL_OAC_NONE = aucun<br /><br /> SQL_OAC_LEVEL1 = niveau 1 pris en charge<br /><br /> SQL_OAC_LEVEL2 = niveau 2 pris en charge|  
|SQL_ODBC_SQL_CONFORMANCE (ODBC 1,0)|Valeur SQLSMALLINT indiquant la grammaire SQL prise en charge par le pilote. Consultez l' [annexe C : grammaire SQL](../../../odbc/reference/appendixes/appendix-c-sql-grammar.md) pour une définition des niveaux de conformité SQL.<br /><br /> SQL_OSC_MINIMUM = grammaire minimale prise en charge<br /><br /> SQL_OSC_CORE = grammaire principale prise en charge<br /><br /> SQL_OSC_EXTENDED = syntaxe étendue prise en charge|  
|SQL_POS_OPERATIONS (ODBC 2,0)|Masque de SQLINTEGER destinée qui énumère les opérations prises en charge dans **SQLSetPos**.<br /><br /> Les masques de transparence suivants sont utilisés conjointement avec l’indicateur pour déterminer les options prises en charge :<br /><br /> SQL_POS_POSITION (ODBC 2,0) SQL_POS_REFRESH (ODBC 2,0) SQL_POS_UPDATE (ODBC 2,0) SQL_POS_DELETE (ODBC 2,0) SQL_POS_ADD (ODBC 2,0)|  
|SQL_POSITIONED_STATEMENTS (ODBC 2,0)|Masque de SQLINTEGER destinée qui énumère les instructions SQL prises en charge.<br /><br /> Les masques de détours suivants sont utilisés pour déterminer les instructions prises en charge :<br /><br /> SQL_PS_POSITIONED_DELETE SQL_PS_POSITIONED_UPDATE SQL_PS_SELECT_FOR_UPDATE|  
|SQL_SCROLL_CONCURRENCY (ODBC 1,0)|Masque de SQLINTEGER destinée qui énumère les options de contrôle d’accès concurrentiel prises en charge pour le curseur.<br /><br /> Les masques de détours suivants sont utilisés pour déterminer les options prises en charge :<br /><br /> SQL_SCCO_READ_ONLY = le curseur est en lecture seule. Aucune mise à jour n’est autorisée.<br /><br /> SQL_SCCO_LOCK = Cursor utilise le niveau de verrouillage le plus bas pour s’assurer que la ligne peut être mise à jour.<br /><br /> SQL_SCCO_OPT_ROWVER = Cursor utilise le contrôle d’accès concurrentiel optimiste, en comparant les versions de ligne, telles que SQLBase ROWID ou Sybase TIMESTAMP.<br /><br /> SQL_SCCO_OPT_VALUES = Cursor utilise le contrôle d’accès concurrentiel optimiste, en comparant les valeurs.|  
|SQL_STATIC_SENSITIVITY (ODBC 2,0)|Masque de bits SQLINTEGER destinée énumérant si les modifications apportées par une application à un curseur statique ou de jeu de clés via **SQLSetPos** ou des instructions de mise à jour ou de suppression positionnées peuvent être détectées par cette application.<br /><br /> SQL_SS_ADDITIONS = les lignes ajoutées sont visibles par le curseur. le curseur peut défiler jusqu’à ces lignes. L’emplacement où ces lignes sont ajoutées au curseur dépend du pilote.<br /><br /> SQL_SS_DELETIONS = les lignes supprimées ne sont plus disponibles pour le curseur et ne laissent pas de « trou » dans le jeu de résultats ; une fois que le curseur a fait défiler une ligne supprimée, il ne peut pas revenir à cette ligne.<br /><br /> SQL_SS_UPDATES = les mises à jour des lignes sont visibles par le curseur. Si le curseur défile et retourne à une ligne mise à jour, les données retournées par le curseur sont les données mises à jour, et non les données d’origine. Cette option s’applique uniquement aux curseurs statiques ou aux mises à jour sur les curseurs de jeu de clés qui ne mettent pas à jour la clé. Cette option ne s’applique pas à un curseur dynamique ou dans le cas où une clé est modifiée dans un curseur mixte.<br /><br /> Si une application peut détecter des modifications apportées au jeu de résultats par d’autres utilisateurs, y compris d’autres curseurs dans la même application, dépend du type de curseur.|  
  
 Une application ODBC 3 *. x* qui utilise un pilote ODBC 3 *. x* ne doit pas **appeler SQLGetInfo** avec les arguments *infotype* décrits dans le tableau précédent, mais doit utiliser les arguments d' *infotype* ODBC 3 *. x* répertoriés dans le paragraphe suivant. Il n’existe pas de correspondance un-à-un entre les arguments *infotype* utilisés dans ODBC 2. *x* et ceux utilisés dans ODBC 3 *. x*. Une application ODBC 3 *. x* fonctionnant avec ODBC 2. *x* , en revanche, doit utiliser les arguments *infotype* décrits précédemment.  
  
 Certains types d’informations du tableau précédent sont déconseillés en faveur des types d’informations d’attributs de curseur. Ces types d’informations déconseillées sont SQL_FETCH_DIRECTION, SQL_LOCK_TYPES, SQL_POS_OPERATIONS, SQL_POSITIONED_STATEMENTS, SQL_SCROLL_CONCURRENCY et SQL_STATIC_SENSITIVITY. Les nouveaux types d’attributs de curseur sont SQL_XXX_CURSOR_ATTRIBUTES1and SQL_XXX_CURSOR_ATTRIBUTES2, où XXX est égal à dynamique, FORWARD_ONLY, KEYSET_DRIVEN ou statique. Chacun des nouveaux types indique les capacités du pilote pour un type de curseur unique. Pour plus d’informations sur ces options, consultez la description de la fonction [SQLGetInfo](../../../odbc/reference/syntax/sqlgetinfo-function.md) .
