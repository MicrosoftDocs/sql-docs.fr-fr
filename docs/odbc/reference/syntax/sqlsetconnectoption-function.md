---
description: SQLSetConnectOption, fonction
title: SQLSetConnectOption, fonction | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
apiname:
- SQLSetConnectOption
apilocation:
- sqlsrv32.dll
apitype: dllExport
f1_keywords:
- SQLSetConnectOption
helpviewer_keywords:
- SQLSetConnectOption function [ODBC]
ms.assetid: 8cd2c2a2-25c8-4aff-951c-b593bbfc90ad
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: ebe9166ea52971812e8905399da4ea0e6347361a
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/17/2020
ms.locfileid: "88499531"
---
# <a name="sqlsetconnectoption-function"></a>SQLSetConnectOption, fonction
**Conformité**  
 Version introduite : conformité des normes ODBC 1,0 : déconseillé  
  
 **Résumé**  
 Dans ODBC 3 *. x*, la fonction ODBC 2,0 **SQLSetConnectOption** a été remplacée par **SQLSetConnectAttr**. Pour plus d’informations, consultez [SQLSetConnectAttr](../../../odbc/reference/syntax/sqlsetconnectattr-function.md).  
  
> [!NOTE]
>  Pour plus d’informations sur le mappage de cette fonction par le gestionnaire de pilotes lorsqu’une application ODBC 2 *. x* utilise un pilote ODBC 3 *. x* , consultez [mappage des fonctions dépréciées](../../../odbc/reference/appendixes/mapping-deprecated-functions.md).  
  
## <a name="remarks"></a>Notes  
 Consultez [ODBC 64-informations sur ODBC](../../../odbc/reference/odbc-64-bit-information.md), si votre application s’exécute sur un système d’exploitation 64 bits.  
  
> [!NOTE]  
>  L’attribut SQL_ASYNC_DBC_FUNCTION_ENABLE introduit dans ODBC 3,8 n’est pas pris en charge par **SQLSetConnectOption**. Les applications qui utilisent l’opération asynchrone sur le descripteur de connexion doivent utiliser **SQLSetConnectAttr**.  
  
## <a name="see-also"></a>Voir aussi  
 [Informations de référence sur l’API ODBC](../../../odbc/reference/syntax/odbc-api-reference.md)   
 [Fichiers d’en-tête ODBC](../../../odbc/reference/install/odbc-header-files.md)
