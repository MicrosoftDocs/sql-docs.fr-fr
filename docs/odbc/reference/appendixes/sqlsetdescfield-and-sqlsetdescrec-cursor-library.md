---
description: SQLSetDescField et SQLSetDescRec (bibliothèque de curseurs)
title: SQLSetDescField et SQLSetDescRec (bibliothèque de curseurs) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- SQLSetDescField function [ODBC], Cursor Library
- SQLSetDescRec function [ODBC], Cursor Library
ms.assetid: 4ccff067-85cd-4bfa-a6cd-7f28051fb5b9
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: c97bbb38b5050461d3ca305d8ac895d693567be3
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99202651"
---
# <a name="sqlsetdescfield-and-sqlsetdescrec-cursor-library"></a>SQLSetDescField et SQLSetDescRec (bibliothèque de curseurs)
> [!IMPORTANT]  
>  Cette fonctionnalité sera supprimée dans une future version de Windows. Évitez d’utiliser cette fonctionnalité dans de nouveaux travaux de développement et prévoyez de modifier les applications qui utilisent actuellement cette fonctionnalité. Microsoft recommande l’utilisation de la fonctionnalité de curseur du pilote.  
  
 Cette rubrique décrit l’utilisation des fonctions **SQLSetDescField** et **SQLSetDescRec** dans la bibliothèque de curseurs. Pour obtenir des informations générales sur ces fonctions, consultez [fonction SQLSetDescField](../../../odbc/reference/syntax/sqlsetdescfield-function.md) et [fonction SQLSetDescRec](../../../odbc/reference/syntax/sqlsetdescrec-function.md).  
  
 La bibliothèque de curseurs exécute **SQLSetDescField** lorsqu’elle est appelée pour retourner la valeur des champs définis pour les colonnes de signets :  
  
 SQL_DESC_DATA_PTR  
  
 SQL_DESC_INDICATOR_PTR  
  
 SQL_DESC_OCTET_LENGTH_PTR  
  
 SQL_DESC_LENGTH  
  
 SQL_DESC_OCTET_LENGTH  
  
 SQL_DESC_DATETIME_INTERVAL_CODE  
  
 SQL_DESC_SCALE  
  
 SQL_DESC_PRECISION  
  
 SQL_DESC_TYPE  
  
 SQL_DESC_NAME  
  
 SQL_DESC_UNNAMED  
  
 SQL_DESC_NULLABLE  
  
 La bibliothèque de curseurs exécute des appels à **SQLSetDescRec** pour une colonne de signets.  
  
 Lorsque vous utilisez un pilote ODBC *2. x* , la bibliothèque de curseurs retourne SQLState HY090 (chaîne ou longueur de mémoire tampon non valide) lorsque **SQLSetDescField** ou **SQLSetDescRec** est appelé pour définir le champ SQL_DESC_OCTET_LENGTH de l’enregistrement de signet d’un ARD sur une valeur qui n’est pas égale à 4. Lorsque vous utilisez un pilote ODBC *3. x* , la bibliothèque de curseurs permet à la mémoire tampon d’être de n’importe quelle taille.  
  
 La bibliothèque de curseurs exécute **SQLSetDescField** lorsqu’elle est appelée pour retourner la valeur du champ SQL_DESC_BIND_OFFSET_PTR, SQL_DESC_BIND_TYPE, SQL_DESC_ROW_ARRAY_SIZE ou SQL_DESC_ROW_STATUS_PTR. Ces champs peuvent être retournés pour n’importe quelle ligne, et pas seulement pour la ligne de signet.  
  
 La bibliothèque de curseurs n’exécute pas **SQLSetDescField** pour modifier un champ de descripteur autre que les champs mentionnés précédemment. Si une application appelle **SQLSetDescField** pour définir un autre champ pendant le chargement de la bibliothèque de curseurs, l’appel est passé au pilote.  
  
 La bibliothèque de curseurs prend en charge la modification dynamique des champs SQL_DESC_DATA_PTR, SQL_DESC_INDICATOR_PTR et SQL_DESC_OCTET_LENGTH_PTR de toute ligne d’un descripteur de ligne d’application (après un appel à **SQLExtendedFetch**, **SQLFetch** ou **SQLFetchScroll**). Le champ SQL_DESC_OCTET_LENGTH_PTR peut être remplacé par un pointeur NULL uniquement pour dissocier la mémoire tampon de longueur pour une colonne.  
  
 La bibliothèque de curseurs ne prend pas en charge la modification du champ SQL_DESC_BIND_TYPE dans APD ou ARD lorsqu’un curseur est ouvert. Le champ SQL_DESC_BIND_TYPE ne peut être modifié qu’après la fermeture du curseur et avant l’ouverture d’un nouveau curseur. Les seuls champs de descripteur que la bibliothèque de curseurs prend en charge en cas de modification lorsqu’un curseur est ouvert sont SQL_DESC_ARRAY_STATUS_PTR, SQL_DESC_BIND_OFFSET_PTR, SQL_DESC_DATA_PTR, SQL_DESC_INDICATOR_PTR, SQL_DESC_OCTET_LENGTH_PTR et SQL_DESC_ROWS_PROCESSED_PTR.  
  
 La bibliothèque de curseurs ne prend pas en charge la modification du champ SQL_DESC_COUNT du ARD après l’appel de **SQLExtendedFetch** ou **SQLFetchScroll** et avant la fermeture du curseur.
