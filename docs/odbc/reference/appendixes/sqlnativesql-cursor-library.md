---
description: SQLNativeSql (bibliothèque de curseurs)
title: SQLNativeSql (bibliothèque de curseurs) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- SQLNativeSql function [ODBC], Cursor Library
ms.assetid: c4459092-1177-4b2a-b7f5-e0083d3bf2b2
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 06e5d6ceffad6025c2184f60ab98457756c36901
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99202702"
---
# <a name="sqlnativesql-cursor-library"></a>SQLNativeSql (bibliothèque de curseurs)
> [!IMPORTANT]  
>  Cette fonctionnalité sera supprimée dans une future version de Windows. Évitez d’utiliser cette fonctionnalité dans de nouveaux travaux de développement et prévoyez de modifier les applications qui utilisent actuellement cette fonctionnalité. Microsoft recommande l’utilisation de la fonctionnalité de curseur du pilote.  
  
 Cette rubrique traite de l’utilisation de la fonction **SQLNativeSql** dans la bibliothèque de curseurs. Pour obtenir des informations générales sur **SQLNativeSql**, consultez [fonction SQLNativeSql](../../../odbc/reference/syntax/sqlnativesql-function.md).  
  
 Si le pilote prend en charge cette fonction, la bibliothèque de curseurs appelle **SQLNativeSql** dans le pilote et lui transmet l’instruction SQL. Pour les instructions Update positionnée, Deleted Delete et **Select for Update** , la bibliothèque de curseurs modifie l’instruction avant de la transmettre au pilote.  
  
> [!NOTE]  
>  La bibliothèque de curseurs retourne incorrectement SQLSTATE 34000 (nom de curseur non valide) si le nom de curseur n’est pas valide dans une instruction UPDATE ou DELETE positionnée qui est passée dans l’argument *InStatementText* de **SQLNativeSql**. **SQLNativeSql** n’est pas destiné à retourner des erreurs de syntaxe, qui sont retournées uniquement lors de la préparation ou de l’exécution d’une instruction.
