---
description: SQLGetConfigMode, fonction
title: SQLGetConfigMode fonction) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLGetConfigMode
apilocation:
- sqlsrv32.dll
apitype: dllExport
f1_keywords:
- SQLGetConfigMode
helpviewer_keywords:
- SQLGetConfigMode function [ODBC]
ms.assetid: b96ab3b8-08d5-4fea-9ffe-e03043efbf2d
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 0ccd703f6fd8c7b80dec575d85e44f2c955ea669
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99203464"
---
# <a name="sqlgetconfigmode-function"></a>SQLGetConfigMode, fonction
**Conformité**  
 Version introduite : ODBC 3,0  
  
 **Résumé**  
 **SQLGetConfigMode** récupère le mode de configuration qui indique où l’entrée de Odbc.ini répertoriant les valeurs DSN se trouve dans les informations système.  
  
## <a name="syntax"></a>Syntaxe  
  
```cpp  
  
BOOL SQLGetConfigMode(  
     UWORD *   pwConfigMode);  
```  
  
## <a name="arguments"></a>Arguments  
 *pwConfigMode*  
 Sortie Pointeur vers la mémoire tampon qui contient le mode de configuration. (Voir « commentaires ».) La valeur de *\* pwConfigMode* peut être :  
  
 ODBC_USER_DSN  
  
 ODBC_SYSTEM_DSN  
  
 ODBC_BOTH_DSN  
  
## <a name="returns"></a>Retours  
 La fonction retourne TRUE si elle réussit, FALSe en cas d’échec.  
  
## <a name="diagnostics"></a>Diagnostics  
 Quand **SQLGetConfigMode** retourne false, une valeur *\* pfErrorCode* associée peut être obtenue en appelant **SQLInstallerError**. Le tableau suivant répertorie les valeurs *\* pfErrorCode* qui peuvent être retournées par **SQLInstallerError** et les explique dans le contexte de cette fonction.  
  
|*\*pfErrorCode*|Erreur|Description|  
|---------------------|-----------|-----------------|  
|ODBC_ERROR_OUT_OF_MEM|Mémoire insuffisante|Le programme d’installation n’a pas pu exécuter la fonction en raison d’un manque de mémoire.|  
  
## <a name="comments"></a>Commentaires  
 Cette fonction est utilisée pour déterminer où l’entrée de Odbc.ini répertoriant les valeurs DSN se trouve dans les informations système. Si *\* pwConfigMode* est ODBC_USER_DSN, le DSN est un DSN utilisateur et la fonction lit à partir de l’entrée de Odbc.ini dans HKEY_CURRENT_USER. S’il est ODBC_SYSTEM_DSN, le DSN est un DSN système et la fonction lit à partir de l’entrée de Odbc.ini dans HKEY_LOCAL_MACHINE. S’il est ODBC_BOTH_DSN, HKEY_CURRENT_USER est essayé et, en cas d’échec, HKEY_LOCAL_MACHINE est utilisé.  
  
 Par défaut, **SQLGetConfigMode** retourne ODBC_BOTH_DSN. Lorsqu’un DSN utilisateur ou un DSN système est créé par un appel à **SQLConfigDataSource**, la fonction définit le mode de configuration sur ODBC_USER_DSN ou ODBC_SYSTEM_DSN pour distinguer les noms DSN utilisateur et système lors de la modification d’un DSN. Avant de retourner, **SQLConfigDataSource** réinitialise le mode de configuration à ODBC_BOTH_DSN.  
  
## <a name="related-functions"></a>Fonctions connexes  
  
|Pour obtenir des informations sur|Consultez|  
|---------------------------|---------|  
|Définition du mode de configuration|[SQLSetConfigMode](../../../odbc/reference/syntax/sqlsetconfigmode-function.md)|
