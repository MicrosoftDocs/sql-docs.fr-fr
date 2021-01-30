---
description: SQLBindParameter (bibliothèque de curseurs)
title: SQLBindParameter (bibliothèque de curseurs) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- SQLBindParameter function [ODBC], Cursor Library
ms.assetid: 04c53e4c-cd1d-40b2-9997-684ebe43499f
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 3dfc87c9dc17aeb7dc2cf0a3aa1bfa8e466d2ed7
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99202923"
---
# <a name="sqlbindparameter-cursor-library"></a>SQLBindParameter (bibliothèque de curseurs)
> [!IMPORTANT]  
>  Cette fonctionnalité sera supprimée dans une future version de Windows. Évitez d’utiliser cette fonctionnalité dans de nouveaux travaux de développement et prévoyez de modifier les applications qui utilisent actuellement cette fonctionnalité. Microsoft recommande l’utilisation de la fonctionnalité de curseur du pilote.  
  
 Cette rubrique traite de l’utilisation de la fonction **SQLBindParameter** dans la bibliothèque de curseurs. Pour obtenir des informations générales sur **SQLBindParameter**, consultez [fonction SQLBindParameter](../../../odbc/reference/syntax/sqlbindparameter-function.md).  
  
 Une application peut appeler **SQLBindParameter** pour relier des paramètres, à condition que le type de données C, la taille de colonne et les chiffres décimaux de la colonne liée restent les mêmes.  
  
 La bibliothèque de curseurs prend en charge la définition de l’attribut d’instruction SQL_ATTR_ROW_BIND_OFFSET_PTR pour utiliser des décalages de liaison. (**SQLBindParameter** n’a pas besoin d’être appelé pour que cette reliaison se produise.)  
  
 La bibliothèque de curseurs prend en charge la liaison des paramètres de données en cours d’exécution.
