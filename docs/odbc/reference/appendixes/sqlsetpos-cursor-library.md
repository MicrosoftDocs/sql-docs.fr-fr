---
description: SQLSetPos (bibliothèque de curseurs)
title: SQLSetPos (bibliothèque de curseurs) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- SQLSetPos function [ODBC], Cursor Library
ms.assetid: 574399c3-2bb2-4d19-829c-7c77bd82858d
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: bf97529f4fd411581738459accffc805a444a8ea
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99202616"
---
# <a name="sqlsetpos-cursor-library"></a>SQLSetPos (bibliothèque de curseurs)
> [!IMPORTANT]  
>  Cette fonctionnalité sera supprimée dans une future version de Windows. Évitez d’utiliser cette fonctionnalité dans de nouveaux travaux de développement et prévoyez de modifier les applications qui utilisent actuellement cette fonctionnalité. Microsoft recommande l’utilisation de la fonctionnalité de curseur du pilote.  
  
 Cette rubrique traite de l’utilisation de la fonction **SQLSetPos** dans la bibliothèque de curseurs. Pour obtenir des informations générales sur **SQLSetPos**, consultez [fonction SQLSetPos](../../../odbc/reference/syntax/sqlsetpos-function.md).  
  
 La bibliothèque de curseurs prend en charge l’opération SQL_POSITION uniquement pour l’argument *operation* dans **SQLSetPos**. Elle prend en charge la valeur SQL_LOCK_NO_CHANGE uniquement pour l’argument *LockType* .  
  
 Si le pilote ne prend pas en charge les opérations en bloc, la bibliothèque de curseurs retourne SQLSTATE HYC00 (pilote non compatible) quand **SQLSetPos** est appelé avec *RowNumber* égal à 0. Ce comportement de pilote n’est pas recommandé.  
  
 La bibliothèque de curseurs ne prend pas en charge les opérations SQL_UPDATE et SQL_DELETE dans un appel à **SQLSetPos**. La bibliothèque de curseurs implémente une instruction SQL UPDATE ou DELETE positionnée en créant une instruction UPDATE ou DELETE recherchée avec une clause WHERE qui énumère les valeurs stockées dans son cache pour chaque colonne liée. Pour plus d’informations, consultez [traitement des instructions Update et DELETE positionnées](../../../odbc/reference/appendixes/processing-positioned-update-and-delete-statements.md).  
  
 Si le pilote ne prend pas en charge les curseurs statiques, une application qui utilise la bibliothèque de curseurs doit appeler **SQLSetPos** uniquement sur un ensemble de lignes extrait par **SQLExtendedFetch** ou **SQLFetchScroll**, et non par **SQLFetch**. La bibliothèque de curseurs implémente **SQLExtendedFetch** et **SQLFetchScroll** en effectuant des appels répétés de **SQLFetch** (avec une taille d’ensemble de lignes de 1) dans le pilote. La bibliothèque de curseurs transmet les appels à **SQLFetch**, en revanche, au pilote. Si **SQLSetPos** est appelé sur un ensemble de lignes multiligne extrait par **SQLFetch** alors que le pilote ne prend pas en charge les curseurs statiques, l’appel échoue car **SQLSetPos** ne fonctionne pas avec les curseurs avant uniquement. Cela se produit même si une application a correctement appelé **SQLSetStmtAttr** pour définir SQL_ATTR_CURSOR_TYPE sur SQL_CURSOR_STATIC, que la bibliothèque de curseurs prend en charge même si le pilote ne prend pas en charge les curseurs statiques.
