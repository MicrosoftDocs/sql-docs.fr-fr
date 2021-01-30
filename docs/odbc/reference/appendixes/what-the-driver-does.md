---
description: Ce que fait le pilote
title: Ce que fait le pilote | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- scrollable cursors [ODBC]
- cursors [ODBC], backward compatibility
- compatibility [ODBC], cursors
- backward compatibility [ODBC], cursors
- block cursors [ODBC]
ms.assetid: 75dcdea6-ff6b-4ac8-aa11-a1f9edbeb8e6
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 37c6c84b74e6b89dc9b31a342e9c47c8a63eb030
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99158178"
---
# <a name="what-the-driver-does"></a>Ce que fait le pilote
Le tableau suivant récapitule les fonctions et les attributs d’instruction qu’un pilote ODBC *3. x* doit implémenter pour les curseurs de bloc et de défilement.  
  
|Function ou<br /><br /> attribut d'instruction|Commentaires|  
|-----------------------------------------|--------------|  
|SQL_ATTR_ROW_STATUS_PTR|Définit l’adresse du tableau d’état de ligne rempli par **SQLFetch** et **SQLFetchScroll**. Ce tableau est également rempli par **SQLSetPos** si **SQLSetPos** est appelé dans l’état d’instruction S6. Si **SQLSetPos** est appelé dans l’État S7, ce tableau n’est pas rempli, mais le tableau désigné par l’argument *RowStatusArray* de **SQLExtendedFetch** est rempli. Pour plus d’informations, consultez [transitions d’instructions](../../../odbc/reference/appendixes/statement-transitions.md) dans annexe B : tables de transition d’État ODBC.|  
|SQL_ATTR_ROWS_FETCHED_PTR|Définit l’adresse de la mémoire tampon dans laquelle **SQLFetch** et **SQLFetchScroll** retournent le nombre de lignes extraites. Si **SQLExtendedFetch** est appelé, cette mémoire tampon n’est pas remplie, mais l’argument *RowCountPtr* pointe vers le nombre de lignes extraites.|  
|SQL_ATTR_ROW_ARRAY_SIZE|Définit la taille de l’ensemble de lignes utilisée par **SQLFetch** et **SQLFetchScroll**.|  
|SQL_ROWSET_SIZE|Définit la taille de l’ensemble de lignes utilisée par **SQLExtendedFetch**. Les pilotes ODBC *3. x* implémentent cela s’ils souhaitent travailler avec des applications ODBC *2. x* qui appellent **SQLExtendedFetch** ou **SQLSetPos**.|  
|**SQLBulkOperations**|Si un pilote *ODBC 3. x* doit fonctionner avec des applications ODBC *2. x* qui utilisent **sqlsetpos** avec une *opération* de SQL_ADD, le pilote doit prendre en charge **SQLSetPos** avec une opération de SQL_ADD en plus de **SQLBulkOperations** avec une *opération* de SQL_ADD. |  
|**SQLExtendedFetch**|Retourne l’ensemble de lignes spécifié. Les pilotes ODBC *3. x* implémentent cela s’ils souhaitent travailler avec des applications ODBC *2. x* qui appellent **SQLExtendedFetch** ou **SQLSetPos**. Voici les détails de l’implémentation :<br /><br /> -Le pilote récupère la taille de l’ensemble de lignes à partir de la valeur de l’attribut d’instruction SQL_ROWSET_SIZE.<br />-Le pilote récupère l’adresse du tableau d’état de ligne à partir de l’argument *RowStatusArray* , et non de l’attribut d’instruction SQL_ATTR_ROW_STATUS_PTR. L’argument *RowStatusArray* dans un appel à **SQLExtendedFetch** ne doit pas être un pointeur null. (Notez que dans ODBC *3. x*, l’attribut d’instruction SQL_ATTR_ROW_STATUS_PTR peut être un pointeur null.)<br />-Le pilote récupère l’adresse du tampon de lignes extraites à partir de l’argument *RowCountPtr* , et non de l’attribut d’instruction SQL_ATTR_ROWS_FETCHED_PTR.<br />-Le pilote retourne SQLSTATE 01S01 (erreur dans la ligne) pour indiquer qu’une erreur s’est produite lors de l’extraction des lignes par un appel à **SQLExtendedFetch**. Un pilote ODBC *3. x* doit retourner SQLSTATE 01S01 (erreur dans la ligne) uniquement quand **SQLExtendedFetch** est appelé, et non quand **SQLFetch** ou **SQLFetchScroll** est appelé. Pour préserver la compatibilité descendante, lorsque SQLSTATE 01S01 (erreur dans la ligne) est retourné par **SQLExtendedFetch**, le gestionnaire de pilotes ne commande pas les enregistrements d’État dans la file d’attente d’erreurs conformément aux règles indiquées dans la section « séquence d’enregistrements d’état » de [SQLGetDiagField](../../../odbc/reference/syntax/sqlgetdiagfield-function.md).|  
|**SQLFetch**|Retourne l’ensemble de lignes suivant. Voici les détails de l’implémentation :<br /><br /> -Le pilote récupère la taille de l’ensemble de lignes à partir de la valeur de l’attribut d’instruction SQL_ATTR_ROW_ARRAY_SIZE.<br />-Le pilote récupère l’adresse du tableau d’état de ligne à partir de l’attribut d’instruction SQL_ATTR_ROW_STATUS_PTR.<br />-Le pilote récupère l’adresse du tampon de lignes extraites à partir de l’attribut d’instruction SQL_ATTR_ROWS_FETCHED_PTR.<br />-L’application peut mélanger les appels entre **SQLFetchScroll** et **SQLFetch**.<br />-   **SQLFetch** retourne des signets si la colonne 0 est liée.<br />-   **SQLFetch** peut être appelée pour retourner plusieurs lignes.<br />-Le pilote ne retourne pas SQLSTATE 01S01 (erreur de ligne) pour indiquer qu’une erreur s’est produite lors de l’extraction de lignes par un appel à **SQLFetch**.|  
|**SQLFetchScroll**|Retourne l’ensemble de lignes spécifié. Voici les détails de l’implémentation :<br /><br /> -Le pilote récupère la taille de l’ensemble de lignes à partir de l’attribut d’instruction SQL_ATTR_ROW_ARRAY_SIZE.<br />-Le pilote récupère l’adresse du tableau d’état de ligne à partir de l’attribut d’instruction SQL_ATTR_ROW_STATUS_PTR.<br />-Le pilote récupère l’adresse du tampon de lignes extraites à partir de l’attribut d’instruction SQL_ATTR_ROWS_FETCHED_PTR.<br />-L’application peut mélanger les appels entre **SQLFetchScroll** et **SQLFetch**.<br />-Le pilote ne retourne pas SQLSTATE 01S01 (erreur de ligne) pour indiquer qu’une erreur s’est produite lors de l’extraction de lignes par un appel à **SQLFetchScroll**.|  
|**SQLSetPos**|Effectue plusieurs opérations positionnées. Voici les détails de l’implémentation :<br /><br /> : Cette instruction peut être appelée dans les États S6 ou S7. Pour plus d’informations, consultez [transitions d’instructions](../../../odbc/reference/appendixes/statement-transitions.md) dans annexe B : tables de transition d’État ODBC.<br />-Si cette méthode est appelée dans état d’instruction S5 ou S6, le pilote récupère la taille de l’ensemble de lignes à partir de l’attribut d’instruction SQL_ATTR_ROWS_FETCHED_PTR et l’adresse du tableau d’état de la ligne à partir de l’attribut d’instruction SQL_ATTR_ROW_STATUS_PTR.<br />-Si cette méthode est appelée dans état d’instruction S7, le pilote récupère la taille de l’ensemble de lignes à partir de l’attribut d’instruction SQL_ROWSET_SIZE et l’adresse du tableau d’état de ligne à partir de l’argument *RowStatusArray* de **SQLExtendedFetch**.<br />-Le pilote retourne SQLSTATE 01S01 (erreur dans la ligne) uniquement pour indiquer qu’une erreur s’est produite lors de l’extraction des lignes par un appel à **SQLSetPos** pour effectuer une opération en bloc lorsque la fonction est appelée dans l’État S7. Pour préserver la compatibilité descendante, si SQLSTATE 01S01 (erreur dans la ligne) est retourné par **SQLSetPos**, le gestionnaire de pilotes ne commande pas les enregistrements d’État dans la file d’attente d’erreurs conformément aux règles indiquées dans la section « séquence d’enregistrements d’état » de [SQLGetDiagField](../../../odbc/reference/syntax/sqlgetdiagfield-function.md).<br />-Si le pilote doit fonctionner avec des applications ODBC *2. x* qui appellent **SQLSetPos** avec un argument *operation* de SQL_ADD, le pilote doit prendre en charge **SQLSetPos** avec un argument *operation* de SQL_ADD.|
