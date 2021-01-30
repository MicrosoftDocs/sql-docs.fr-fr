---
description: Retour de SQL_NO_DATA
title: Retour de SQL_NO_DATA | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- SQL_NO_DATA [ODBC]
- backward compatibility [ODBC], SQL_NO_DATA
- compatibility [ODBC], SQL_NO_DATA
ms.assetid: deed0163-9d1a-4e9b-9342-3f82e64477d2
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: f61d676897bf7b5633a0c9d37c12e44ae9de1cbe
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99187167"
---
# <a name="returning-sql_no_data"></a>Retour de SQL_NO_DATA
Quand une application *ODBC 2. x* avec un pilote *ODBC 3. x* appelle **SQLExecDirect**, **SQLExecute** ou **SQLParamData** et qu’une instruction UPDATE ou DELETE recherchée a été exécutée mais n’a affecté aucune ligne au niveau de la source de données, le pilote ODBC *3. x* doit retourner SQL_SUCCESS. Quand une application *ODBC 3. x* qui utilise un pilote *ODBC 3. x* appelle **SQLExecDirect**, **SQLExecute** ou **SQLParamData** avec le même résultat, le pilote ODBC *3. x* doit retourner SQL_NO_DATA.  
  
 Si une instruction UPDATE ou DELETE recherchée dans un lot d’instructions n’affecte aucune ligne de la source de données, **SQLMoreResults** retourne SQL_SUCCESS. Elle ne peut pas retourner SQL_NO_DATA, car cela signifie qu’il n’y a plus de résultats, et non qu’il y a un résultat d’une mise à jour/suppression recherchée qui n’a affecté aucune ligne.
