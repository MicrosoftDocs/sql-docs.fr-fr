---
description: Structures d’entiers 64 bits
title: Structures d’entiers 64 bits | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- C data types [ODBC], 64-bit integer structures
- data types [ODBC], C data types
- 64-bit integer structures [ODBC]
ms.assetid: ac80c798-d9b2-4430-85ed-bd2461db0ac7
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: cda6c3bd7e1ad9e850d6c9bbb0c444e5bf3e5f5f
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99198171"
---
# <a name="64-bit-integer-structures"></a>Structures d’entiers 64 bits
Le type C pour les identificateurs de type de données SQL_C_SBIGINT et SQL_C_UBIGINT sur les compilateurs Microsoft C est _int64. Lorsqu’un compilateur autre qu’un compilateur Microsoft® C est utilisé, le type C peut être différent. Si le compilateur prend en charge les entiers 64 bits en mode natif, le pilote ou l’application doit définir ODBCINT64 comme type d’entier 64 bits natif. Si le compilateur ne prend pas en charge les entiers 64 bits en mode natif, une application ou un pilote peut définir les structures suivantes pour s’assurer qu’il a accès à ces données :  
  
```  
typedef struct{  
SQLUINTEGER dwLowWord;  
SQLUINTEGER dwHighWord;  
} SQLUBIGINT  
  
typedef struct{  
SQLUINTEGER dwLowWord;  
SQLINTEGER sdwHighWord;  
} SQLBIGINT  
```  
  
 Ces structures doivent être alignées sur une limite de 8 octets, car un entier 64 bits est aligné sur la limite de 8 octets.
