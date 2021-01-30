---
description: Séquence d’échappement d’appel de procédure
title: Séquence d’échappement d’appel de procédure | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- escape sequences [ODBC], procedure call
- procedure call escape sequence [ODBC]
- ODBC escape sequences [ODBC], procedure call
ms.assetid: 269fbab0-e5f2-4a98-86c0-2d7b647acaae
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: a3177c134009ed6f8979637980e96f39ab9badd2
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99205328"
---
# <a name="procedure-call-escape-sequence"></a>Séquence d’échappement d’appel de procédure
ODBC utilise des séquences d’échappement pour les appels de procédure. La syntaxe de cette séquence d’échappement est la suivante :  
  
 **{**[ ? =]**Call** *procedure-Name*[**(**[*paramètre*] [, [*paramètre*]]... **)**]**}**  
  
 Dans la notation BNF, la syntaxe est la suivante :  
  
 *ODBC-procédure-Escape* :: =  
  
 &#124; *ODBC-Échap-Initiator* [ ? =] appeler la *procédure ODBC-ESC-terminateur*  
  
 *procédure* :: = *procédure-nom* &#124; *procédure-nom* (*procédure-paramètre-liste*)  
  
 *procedure-identifier* :: = *nom défini par l’utilisateur*  
  
 *procedure-Name* :: = *procédure-identifier*  
  
 &#124; *owner-name*. *identificateur de procédure*  
  
 Catalogue de &#124; nom-procédure de *séparateur* - *identificateur*  
  
 Catalogue-nom du catalogue *de* &#124; [*owner-name*]. *identificateur de procédure*  
  
 (La troisième syntaxe est valide uniquement si la source de données ne prend pas en charge les propriétaires.)  
  
 *owner-name* :: = *nom défini par l’utilisateur*  
  
 *Catalog-Name* :: = *nom défini par l’utilisateur*  
  
 *catalogue-Separator* :: = {*Implementation-defined*}  
  
 (Le séparateur de catalogue est retourné via **SQLGetInfo** avec l’option d’informations SQL_CATALOG_NAME_SEPARATOR.)  
  
 *procédure-parameter-list* :: = *procédure-paramètre*  
  
 *Paramètre de procédure*&#124;, *procédure-paramètre-list*  
  
 *procédure-paramètre* :: = *paramètre dynamique* &#124; *littéral* &#124; *chaîne vide*  
  
 *Empty-String* :: =  
  
 *ODBC-Echap-Initiator* :: = {  
  
 *ODBC-ESC-terminateur* :: =}  
  
 (Si un paramètre de procédure est une chaîne vide, la procédure utilise la valeur par défaut pour ce paramètre.)  
  
 Pour déterminer si la source de données prend en charge les procédures et que le pilote prend en charge la syntaxe d’appel de procédure ODBC, une application peut appeler **SQLGetInfo** avec le type d’informations SQL_PROCEDURES.
