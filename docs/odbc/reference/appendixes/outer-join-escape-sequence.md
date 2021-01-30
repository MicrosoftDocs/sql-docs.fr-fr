---
description: Séquence d’échappement pour les jointures externes
title: Séquence d’échappement de jointure externe | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- outer join escape sequence [ODBC]
- escape sequences [ODBC], outer join
- ODBC escape sequences [ODBC], outer join
ms.assetid: 2cfd1525-6677-4d36-9b9e-730496853750
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: e25cde1a4da326d06efe23bc2f7c642e61589ff3
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99160854"
---
# <a name="outer-join-escape-sequence"></a>Séquence d’échappement pour les jointures externes
ODBC utilise des séquences d’échappement pour les jointures externes. La syntaxe de cette séquence d’échappement est la suivante :  
  
```  
{oj outer-join}  
```  
  
## <a name="remarks"></a>Notes  
 Dans la notation BNF, la syntaxe est la suivante :  
  
 *ODBC-externe-jointure-Escape* :: =  
  
 *ODBC-ESC-initiateur* JO *Outer-joindre ODBC-ESC-terminateur*  
  
 *OUTER-JOIN* :: = *table-Name* [*corrélation-Name*] {Left &#124; Right &#124; complet}  
  
 JOINTURE externe {*table-Name* [*Correlation-* Name] &#124; *jointure externe*} sur  
  
 *recherche*  
  
 *condition*  
  
 *Correlation-Name* :: = *nom défini par l’utilisateur*  
  
 *ODBC-Echap-Initiator* :: = {  
  
 *ODBC-ESC-terminateur* :: =}  
  
 Pour déterminer quelles parties de cette instruction sont prises en charge, une application appelle **SQLGetInfo** avec le type d’informations SQL_OJ_CAPABILITIES. Pour les jointures externes, la *condition de recherche* doit contenir uniquement la condition de jointure entre les *noms de table* spécifiés.
