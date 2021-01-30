---
description: Codes d’erreur de la bibliothèque de curseurs ODBC
title: Codes d’erreur de la bibliothèque de curseurs ODBC | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- cursor library [ODBC], error codes
- error codes [ODBC], cursor library
- ODBC cursor library [ODBC], error codes
ms.assetid: 9713480e-8744-4f37-a630-20871590d4a1
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 6c9a4d7a68ddaf9f569a04acd8be81b993a636b1
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99193259"
---
# <a name="odbc-cursor-library-error-codes"></a>Codes d’erreur de la bibliothèque de curseurs ODBC
> [!IMPORTANT]  
>  Cette fonctionnalité sera supprimée dans une future version de Microsoft Data Access Component. Évitez d’utiliser cette fonctionnalité dans de nouveaux travaux de développement et prévoyez de modifier les applications qui utilisent actuellement cette fonctionnalité. Utilisez plutôt des curseurs de serveur et de pilote.  
  
 La bibliothèque de curseurs ODBC retourne les valeurs SQLSTATE suivantes en plus de celles figurant dans la référence de l' [API ODBC](../../../odbc/reference/syntax/odbc-api-reference.md).  
  
> [!NOTE]  
>  La bibliothèque de curseurs ne commande pas les enregistrements d’État ; le gestionnaire de pilotes et ODBC 3. les pilotes *x* sont responsables du classement des enregistrements d’État.  
  
|SQLSTATE|Description|Peut être retourné à partir de|  
|--------------|-----------------|--------------------------|  
|01000|Le curseur ne peut pas être mis à jour.|**SQLFetch**<br /><br /> **SQLFetchScroll**|  
|01000|Bibliothèque de curseurs non utilisée. Échec du chargement.|**SQLBrowseConnect**<br /><br /> **SQLConnect**<br /><br /> **SQLDriverConnect**|  
|01000|Bibliothèque de curseurs non utilisée. Prise en charge des pilotes insuffisante.|**SQLBrowseConnect**<br /><br /> **SQLConnect**<br /><br /> **SQLDriverConnect**|  
|01000|Bibliothèque de curseurs non utilisée. Incompatibilité de version avec le gestionnaire de pilotes.|**SQLBrowseConnect**<br /><br /> **SQLConnect**<br /><br /> **SQLDriverConnect**|  
|01000|Le pilote a retourné SQL_SUCCESS_WITH_INFO. Le message d’avertissement a été perdu.|**SQLFetch**<br /><br /> **SQLFetchScroll**|  
|S1000|Erreur générale : impossible de créer le tampon de fichier.|**SQLFetch**<br /><br /> **SQLFetchScroll**<br /><br /> **SQLGetData**|  
|S1000|Erreur générale : impossible de lire à partir de la mémoire tampon du fichier.|**SQLFetch**<br /><br /> **SQLFetchScroll**<br /><br /> **SQLGetData**|  
|S1000|Erreur générale : impossible d’écrire dans le tampon de fichier.|**SQLFetch**<br /><br /> **SQLFetchScroll**<br /><br /> **SQLGetData**|  
|S1000|Erreur générale : impossible de fermer ou de supprimer le tampon de fichier.|**SQLFreeHandle**<br /><br /> **SQLFreeStmt**|  
|SL001|La requête positionnée ne peut pas être effectuée, car aucune colonne pouvant faire l’objet d’une recherche n’est liée.|**SQLExecDirect**<br /><br /> **SQLGetData**<br /><br /> **SQLPrepare**|  
|SL002|La requête positionnée n’a pas pu être exécutée, car le jeu de résultats a été créé par une condition de jointure.|**SQLExecute**<br /><br /> **SQLExecDirect**<br /><br /> **SQLGetData**|  
|SL003|Le tampon lié dépasse la taille maximale du segment.|**SQLFetch**<br /><br /> **SQLFetchScroll**|  
|SL004|Le jeu de résultats n’a pas été généré par une instruction **Select** .|**SQLGetData**|  
|SL005|L’instruction **Select** contient une clause Group by.|**SQLGetData**|  
|SL006|Les tableaux de paramètres ne sont pas pris en charge avec les requêtes positionnées.|**SQLPrepare**<br /><br /> **SQLExecDirect**|  
|SL008|**SQLGetData** n’est pas autorisé sur un curseur avant uniquement (non mis en mémoire tampon).|**SQLGetData**|  
|SL009|Aucune colonne n’a été liée avant l’appel de **SQLFetch** ou **SQLFetchScroll**.|**SQLFetch**<br /><br /> **SQLFetchScroll**|  
|SL010|**SQLBindCol** a retourné SQL_ERROR lors d’une tentative de liaison à une mémoire tampon interne.|**SQLFetch**<br /><br /> **SQLFetchScroll**<br /><br /> **SQLGetData**|  
|SL011|L’option d’instruction est valide uniquement après l’appel de **SQLFetch** ou **SQLFetchScroll**.|**SQLGetStmtAttr**|  
|SL012|Les liaisons d’instruction ne peuvent pas être modifiées lorsqu’un curseur est ouvert.|**SQLBindCol**<br /><br /> **SQLFreeHandle**<br /><br /> **SQLFreeStmt**<br /><br /> **SQLSetStmtAttr**|  
|SL014|Une requête positionnée a été émise et tous les champs de nombre de colonnes n’ont pas été mis en mémoire tampon.|**SQLExecDirect**<br /><br /> **SQLExecute**<br /><br /> **SQLPrepare**|  
|SL015|**SQLFetch** et **SQLFetchScroll** ne peuvent pas être mélangées.|**SQLExtendedFetch**<br /><br /> **SQLFetch**<br /><br /> **SQLFetchScroll**|
