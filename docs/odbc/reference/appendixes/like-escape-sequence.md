---
description: Séquence d’échappement pour l’opérateur LIKE
title: COMME une séquence d’échappement | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- ODBC escape sequences [ODBC], LIKE
- LIKE escape sequence [ODBC]
- escape sequences [ODBC], LIKE
ms.assetid: 798d75ea-be9d-4bef-b297-318bc327f1ca
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: d32b0470cb2adf0960e23dac17d8cb4942f296c0
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99184390"
---
# <a name="like-escape-sequence"></a>Séquence d’échappement pour l’opérateur LIKE
ODBC utilise des séquences d’échappement pour la clause LIKE. La syntaxe de cette séquence d’échappement est la suivante :  
  
```  
{'escape-character'}  
```  
  
## <a name="remarks"></a>Notes  
 Dans la notation BNF, la syntaxe est la suivante :  
  
 *ODBC-like-Escape* :: =  
  
 *ODBC-ESC-initiateur* Echap'*Escape-caractère*' *ODBC-ESC-terminateur*  
  
 *caractère d’échappement* :: = *caractère*  
  
 *ODBC-Echap-Initiator* :: = {  
  
 *ODBC-ESC-terminateur* :: =}  
  
 Pour déterminer si le pilote prend en charge la séquence d’échappement LIKE, une application peut appeler **SQLGetInfo** avec le type d’informations SQL_LIKE_ESCAPE_CLAUSE.
