---
description: Méthode toString (DateTimeOffset)
title: Méthode toString (DateTimeOffset) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
ms.assetid: e77b9be3-1a02-4769-8acf-ac71d48d6a76
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 4599b8152bbb7041565cc4858199ec88d1bb0510
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99158828"
---
# <a name="tostring-method-datetimeoffset"></a>Méthode toString (DateTimeOffset)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Retourne une représentation de chaîne de l’objet **DateTimeOffset**.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
public String toString()  
```  
  
## <a name="return-value"></a>Valeur de retour  
 Représentation de chaîne de l’objet **DateTimeOffset**.  
  
## <a name="remarks"></a>Notes  
 La chaîne respecte le format `YYYY-MM-DD HH:mm:ss[.fffffff] [+|-]HH:mm`.  
  
 Les fractions de seconde de la chaîne renvoyée sont remplies avec des zéros jusqu'à la précision déclarée. Par exemple, **datetimeoffset(6)** avec la valeur « 2010-03-10 12:34:56.78 -08:00 » sera mis en forme comme « 2010-03-10 12:34:56.780000 -08:00 » par DateTimeOffset.toString.  
  
## <a name="see-also"></a>Voir aussi  
 [DateTimeOffset, classe](../../../connect/jdbc/reference/datetimeoffset-class.md)   
 [Membres de DateTimeOffset](../../../connect/jdbc/reference/datetimeoffset-members.md)  
  
  
