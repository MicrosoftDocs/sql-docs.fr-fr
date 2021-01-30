---
description: Compatibilité descendante des types de données C
title: Compatibilité descendante des types de données C | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- backward compatibility [ODBC], C data types
- compatibility [ODBC], C data types
- data types [ODBC], backward compatibility
- C data types [ODBC], backward compatibility
ms.assetid: b1453a65-ae03-4061-b0cf-a8434d8bc40b
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 75ea0c53091faaece23f9d7316c9a6f242677a41
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99212507"
---
# <a name="backward-compatibility-of-c-data-types"></a>Compatibilité descendante des types de données C
SQL_C_SHORT, SQL_C_LONG et SQL_C_TINYINT ont été remplacés dans ODBC par des types signés et non signés : SQL_C_SSHORT et SQL_C_USHORT, SQL_C_SLONG et SQL_C_ULONG, et SQL_C_STINYINT et SQL_C_UTINYINT. Un pilote ODBC *3. x* qui doit fonctionner avec les applications ODBC *2. x* doit prendre en charge SQL_C_SHORT, SQL_C_LONG et SQL_C_TINYINT, car lorsqu’ils sont appelés, le gestionnaire de pilotes les transmet au pilote.
