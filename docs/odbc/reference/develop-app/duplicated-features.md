---
description: Fonctionnalités en doublon
title: Fonctionnalités dupliquées | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
helpviewer_keywords:
- duplicated functions [ODBC]
- compatibility [ODBC], duplicated functions
- ODBC drivers [ODBC], backward compatibility
- functions [ODBC], duplicated functions
- backward compatibility [ODBC], duplicated functions
ms.assetid: 641b16bc-f791-46d8-b093-31736473fe3d
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 0aa03482efbadc8dac2fa887deb0f01d9b167214
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/17/2020
ms.locfileid: "88483022"
---
# <a name="duplicated-features"></a>Fonctionnalités en doublon
Les fonctions ODBC *2. x* suivantes ont été dupliquées par les fonctions ODBC *3. x* . Par conséquent, les fonctions ODBC *2. x* sont déconseillées dans ODBC *3. x*. Les fonctions ODBC *3. x* sont appelées fonctions de remplacement.  
  
 Quand une application utilise une fonction ODBC *2. x* déconseillée et que le pilote sous-jacent est un pilote ODBC *3. x* , le gestionnaire de pilotes mappe l’appel de fonction à la fonction de remplacement correspondante. La seule exception à cette règle est **SQLExtendedFetch**. (Consultez la note de bas de page à la fin du tableau ci-dessous.) Pour plus d’informations sur ces mappages, consultez [mappage des fonctions déconseillées](../../../odbc/reference/appendixes/mapping-deprecated-functions.md) dans l’annexe G : instructions relatives aux pilotes pour la compatibilité descendante.  
  
 Quand une application utilise une fonction de remplacement et que le pilote sous-jacent est un pilote ODBC *2. x* , le gestionnaire de pilotes mappe l’appel de fonction à la fonction déconseillée correspondante.  
  
|ODBC *2. x,* fonction|ODBC *3. x,* fonction|  
|-------------------------|-------------------------|  
|**SQLAllocConnect**|**SQLAllocHandle**|  
|**SQLAllocEnv**|**SQLAllocHandle**|  
|**SQLAllocStmt,**|**SQLAllocHandle**|  
|**SQLColAttributes**|**SQLColAttribute**|  
|**SQLError**|**SQLGetDiagRec**|  
|**SQLExtendedFetch**[1]|**SQLFetchScroll**|  
|**Sqlfreeconnect,**|**SQLFreeHandle**|  
|**Sqlfreeenv,**|**SQLFreeHandle**|  
|**Sqlgetconnectoption,**|**SQLGetConnectAttr**|  
|**SQLGetStmtOption**|**SQLGetStmtAttr**|  
|**SQLParamOptions,**|**SQLSetStmtAttr**, **SQLGetStmtAttr**|  
|**SQLSetConnectOption**|**SQLSetConnectAttr**|  
|**SQLSetParam,**|**SQLBindParameter**|  
|**SQLSetStmtOption**|**SQLSetStmtAttr**|  
|**SQLTransact**|**SQLEndTran**|  
  
 [1] la fonction **SQLExtendedFetch** est une fonctionnalité dupliquée ; **SQLFetchScroll** offre les mêmes fonctionnalités dans ODBC *3. x*. Toutefois, le gestionnaire de pilotes ne mappe pas **SQLExtendedFetch** à **SQLFetchScroll** quand il passe à un pilote ODBC *3. x* . Pour plus d’informations, voir [ce que fait le gestionnaire de pilotes](../../../odbc/reference/appendixes/what-the-driver-manager-does.md) dans annexe G : instructions relatives aux pilotes pour la compatibilité descendante. Le gestionnaire de pilotes mappe **SQLFetchScroll** à **SQLExtendedFetch** quand il passe à un pilote ODBC *2. x* .  
  
> [!NOTE]
>  La fonction **SQLBindParam** est un cas spécial. **SQLBindParam** est une fonctionnalité dupliquée. Il ne s’agit pas d’une fonction ODBC *2. x* , mais d’une fonction qui est présente dans les normes Open Group et ISO. La fonctionnalité fournie par cette fonction est entièrement incluse par celle de **SQLBindParameter**. Par conséquent, le gestionnaire de pilotes mappe un appel à **SQLBindParam** sur **SQLBindParameter** lorsque le pilote sous-jacent est un pilote ODBC *3. x* . Toutefois, lorsque le pilote sous-jacent est un pilote ODBC *2. x* , le gestionnaire de pilotes n’effectue pas ce mappage.
