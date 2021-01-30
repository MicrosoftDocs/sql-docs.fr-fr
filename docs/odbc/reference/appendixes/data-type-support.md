---
description: Prise en charge du type de données
title: Prise en charge des types de données | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- minimum SQL syntax supported [ODBC]
- ODBC drivers [ODBC], minimum SQL syntax supported
- data types [ODBC], ODBC drivers
- ODBC drivers [ODBC], data types
ms.assetid: 782b4490-372b-4366-aad7-a486fb8a07c8
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 5b9249539c87e727b293c73ee12ed0fa734e08fa
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99179540"
---
# <a name="data-type-support"></a>Prise en charge du type de données
Les pilotes ODBC doivent prendre en charge au moins l’un des SQL_CHAR et SQL_VARCHAR. La prise en charge d’autres types de données est déterminée par le niveau de conformité de SQL-92 de la source de données ou du pilote. Une application doit appeler **SQLGetTypeInfo** pour déterminer les types de données pris en charge par le pilote.  
  
 Pour plus d’informations sur les types de données, consultez l' [annexe D : types de données](../../../odbc/reference/appendixes/appendix-d-data-types.md).
