---
description: Remplacement de la précision du début et de la fin par défaut pour les types de données d’intervalle
title: Remplacer la précision de début et de seconde pour les types de données d’intervalle | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- data types [ODBC], interval data types
- precision [ODBC], interval data types
- seconds precision [ODBC]
- interval data type [ODBC], precision
- interval leading precision [ODBC]
- interval precision [ODBC]
ms.assetid: 3d65493f-dce7-4d29-9f59-c63a4e47918c
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: a79bf67c0a8ccc988d9e171be9760be98c1dc5fa
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99160838"
---
# <a name="overriding-default-leading-and-seconds-precision-for-interval-data-types"></a>Remplacement de la précision du début et de la fin par défaut pour les types de données d’intervalle
Quand le champ SQL_DESC_TYPE d’un ARD est défini sur un type DateTime ou Interval C, en appelant **SQLBindCol** ou **SQLSetDescField**, le champ SQL_DESC_PRECISION (qui contient la précision de l’intervalle en secondes) est défini sur les valeurs par défaut suivantes :  
  
-   6 pour timestamp et tous les types de données Interval avec un deuxième composant.  
  
-   0 pour tous les autres types de données.  
  
 Pour tous les types de données d’intervalle, le champ de descripteur de SQL_DESC_DATETIME_INTERVAL_PRECISION, qui contient la précision de champ d’intervalle, est défini sur une valeur par défaut de 2.  
  
 Lorsque le champ SQL_DESC_TYPE d’un APD est défini sur un type DateTime ou Interval C, en appelant **SQLBindParameter** ou **SQLSetDescField**, les champs SQL_DESC_PRECISION et SQL_DESC_DATETIME_INTERVAL_PRECISION dans le APD sont définis sur la valeur par défaut indiquée précédemment. Cela est vrai pour les paramètres d’entrée, mais pas pour les paramètres d’entrée/sortie ou de sortie.  
  
 Un appel à **SQLSetDescRec** définit la précision de l’intervalle à la valeur par défaut, mais définit la précision de l’intervalle en secondes (dans le champ SQL_DESC_PRECISION) sur la valeur de son argument de *précision* .  
  
 Si l’une des valeurs par défaut fournies précédemment n’est pas acceptable pour une application, l’application doit définir le champ SQL_DESC_PRECISION ou SQL_DESC_DATETIME_INTERVAL_PRECISION en appelant **SQLSetDescField**.  
  
 Si l’application appelle **SQLGetData** pour retourner des données dans un type DateTime ou Interval C, la précision de l’intervalle et la précision de l’intervalle en secondes par défaut sont utilisées. Si l’une des valeurs par défaut n’est pas acceptable, l’application doit appeler **SQLSetDescField** pour définir un champ de descripteur ou **SQLSetDescRec** pour définir SQL_DESC_PRECISION. L’appel à **SQLGetData** doit avoir un *TargetType* de SQL_ARD_TYPE pour utiliser les valeurs dans les champs de descripteur.  
  
 Lorsque **SQLPutData** est appelé, la précision de l’intervalle et la précision de l’intervalle en secondes sont lues à partir des champs de l’enregistrement du descripteur qui correspondent au paramètre ou à la colonne de données en cours d’exécution, qui sont des champs APD pour les appels à **SQLExecute** ou **SQLEXECDIRECT**, ou des champs ARD pour les appels à **SQLBulkOperations** ou **SQLSetPos**.
