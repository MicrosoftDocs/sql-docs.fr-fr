---
description: Séquences d’échappement de GUID
title: Séquences d’échappement GUID | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
helpviewer_keywords:
- ODBC escape sequences [ODBC], GUID
- escape sequences [ODBC], guid
- guid escape sequence [ODBC]
ms.assetid: 71d43ef9-4a31-493e-b9e0-f864e9ef3ce6
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: d7babe2d26c0e5f2b311f8df5bbd1763622f3042
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/17/2020
ms.locfileid: "88466155"
---
# <a name="guid-escape-sequences"></a>Séquences d’échappement de GUID
ODBC utilise des séquences d’échappement pour les littéraux GUID. La syntaxe de cette séquence d’échappement est la suivante :  
  
```  
{guid 'nnnnnnnn-nnnn-nnnn-nnnn-nnnnnnnnnnnn'}  
```  
  
## <a name="remarks"></a>Notes  
 Dans la notation BNF, la syntaxe est la suivante :  
  
 *ODBC-GUID-Escape* :: =  
     *ODBC-ESC-GUID de l’initiateur* '*Guid-value*' *ODBC-ESC-terminateur*  
  
 *ODBC-Echap-Initiator* :: = {  
  
 *ODBC-ESC-terminateur* :: =}  
  
 Guid *-value* :: = *GUID de valeur basse-valeur de séparateur-horloge de point médian-GUID de valeur de séparateur-horloge-valeur maximale GUID-point de séparation-SEQ-valeur Guid-nœud séparateur-valeur*  
  
 *GUID-Separator* :: =-  
  
 *Clock-Low-value* :: = *hex_digit hex_digit hex_digit hex_digit* hex_digit hex_digit hex_digit hex_digit  
  
 *Clock-Middle-value* :: = *hex_digit hex_digit hex_digit hex_digit*  
  
 *Clock-High-Value* :: = *hex_digit hex_digit hex_digit hex_digit*  
  
 *Clock-SEQ-value* :: = *hex_digit hex_digit hex_digit hex_digit*  
  
 *Clock-node-value* :: = *hex_digit hex_digit hex_digit hex_digit hex_digit* hex_digit hex_digit hex_digit hex_digit hex_digit hex_digit hex_digit  
  
 *hex_digit* :: = 0 &#124; 1 &#124; 2 &#124; 3 &#124; 4 &#124; 5 &#124; 6 &#124; 7 &#124; 8 &#124; 9 &#124; A &#124; B &#124; C &#124; D &#124; E &#124; F  
  
 La séquence d’échappement littérale GUID est prise en charge si le type de données GUID est pris en charge par la source de données. Une application doit appeler **SQLGetTypeInfo** pour déterminer si ce type de données est pris en charge.
