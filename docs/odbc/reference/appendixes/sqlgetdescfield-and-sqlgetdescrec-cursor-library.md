---
description: SQLGetDescField et SQLGetDescRec (bibliothèque de curseurs)
title: SQLGetDescField et SQLGetDescRec (bibliothèque de curseurs) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- SQLGetDescField function [ODBC], Cursor Library
- SQLGetDescRec function [ODBC], Cursor Library
ms.assetid: 1a801f22-6fea-48aa-a723-3187a2ad852b
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: ef395940419d593571dc2da6df6323437f2812eb
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99202774"
---
# <a name="sqlgetdescfield-and-sqlgetdescrec-cursor-library"></a>SQLGetDescField et SQLGetDescRec (bibliothèque de curseurs)
> [!IMPORTANT]  
>  Cette fonctionnalité sera supprimée dans une future version de Windows. Évitez d’utiliser cette fonctionnalité dans de nouveaux travaux de développement et prévoyez de modifier les applications qui utilisent actuellement cette fonctionnalité. Microsoft recommande l’utilisation de la fonctionnalité de curseur du pilote.  
  
 Cette rubrique décrit l’utilisation des fonctions **SQLGetDescField** et **SQLGetDescRec** dans la bibliothèque de curseurs. Pour obtenir des informations générales sur ces fonctions, consultez [fonction SQLGetDescField](../../../odbc/reference/syntax/sqlgetdescfield-function.md) et [fonction SQLGetDescRec](../../../odbc/reference/syntax/sqlgetdescrec-function.md).  
  
 La bibliothèque de curseurs exécute **SQLGetDescRec** pour retourner les métadonnées des colonnes de signets. La bibliothèque de curseurs exécute **SQLGetDescField** pour retourner les mêmes champs que ceux retournés par **SQLGetDescRec**, qui sont SQL_DESC_NAME, SQL_DESC_TYPE, SQL_DESC_DATETIME_INTERVAL_CODE, SQL_DESC_OCTET_LENGTH, SQL_DESC_PRECISION, SQL_DESC_SCALE et SQL_DESC_NULLABLE. Pour des fins de cohérence, **SQLGetDescField** retourne également SQL_DESC_UNNAMED.  
  
 La bibliothèque de curseurs exécute **SQLGetDescField** lorsqu’elle est appelée pour retourner la valeur des champs suivants qui sont définis pour les colonnes de signet de liaison : SQL_DESC_DATA_PTR, SQL_DESC_INDICATOR_PTR, SQL_DESC_OCTET_LENGTH_PTR et SQL_DESC_LENGTH.  
  
 La bibliothèque de curseurs exécute **SQLGetDescField** lorsqu’elle est appelée pour retourner la valeur du champ SQL_DESC_BIND_OFFSET_PTR, SQL_DESC_BIND_TYPE, SQL_DESC_ROW_ARRAY_SIZE ou SQL_DESC_ROW_STATUS_PTR. Ces champs peuvent être retournés pour n’importe quelle ligne, et pas seulement pour la ligne de signet.  
  
 Si une application appelle **SQLGetDescField** pour retourner la valeur d’un champ autre que ceux mentionnés précédemment, la bibliothèque de curseurs passe l’appel au pilote.
