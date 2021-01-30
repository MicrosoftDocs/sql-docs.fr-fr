---
description: SQLRemoveDSNFromIni, fonction
title: SQLRemoveDSNFromIni fonction) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLRemoveDSNFromIni
apilocation:
- sqlsrv32.dll
apitype: dllExport
f1_keywords:
- SQLRemoveDSNFromIni
helpviewer_keywords:
- SQLRemoveDSNFromIni function [ODBC]
ms.assetid: bb2e8273-7b61-4113-bfc8-f7ccc607c811
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 26bbcf3bf68dda172220323cb7d9b0dc39e3178d
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99192491"
---
# <a name="sqlremovedsnfromini-function"></a>SQLRemoveDSNFromIni, fonction
**Conformité**  
 Version introduite : ODBC 1,0  
  
 **Résumé**  
 **SQLRemoveDSNFromIni** supprime une source de données des informations système.  
  
## <a name="syntax"></a>Syntaxe  
  
```cpp  
  
BOOL SQLRemoveDSNFromIni(  
     LPCSTR   lpszDSN);  
```  
  
## <a name="arguments"></a>Arguments  
 *lpszDSN*  
 Entrée Nom de la source de données à supprimer.  
  
## <a name="returns"></a>Retours  
 La fonction retourne la valeur TRUE si elle supprime la source de données ou si la source de données n’était pas dans le fichier Odbc.ini. Elle retourne FALSe s’il ne parvient pas à supprimer la source de données.  
  
## <a name="diagnostics"></a>Diagnostics  
 Quand **SQLRemoveDSNFromIni** retourne false, une valeur *\* pfErrorCode* associée peut être obtenue en appelant **SQLInstallerError**. Le tableau suivant répertorie les valeurs *\* pfErrorCode* qui peuvent être retournées par **SQLInstallerError** et les explique dans le contexte de cette fonction.  
  
|*\*pfErrorCode*|Erreur|Description|  
|---------------------|-----------|-----------------|  
|ODBC_ERROR_GENERAL_ERR|Erreur générale du programme d’installation|Une erreur s’est produite pour laquelle aucune erreur d’installation spécifique n’a été rencontrée.|  
|ODBC_ERROR_INVALID_DSN|DSN non valide|L’argument *lpszDSN* n’était pas valide.|  
|ODBC_ERROR_REQUEST_FAILED|Échec de la demande|Le programme d’installation n’a pas pu supprimer les informations du DSN du Registre.|  
|ODBC_ERROR_OUT_OF_MEM|Mémoire insuffisante|Le programme d’installation n’a pas pu exécuter la fonction en raison d’un manque de mémoire.|  
  
## <a name="comments"></a>Commentaires  
 **SQLRemoveDSNFromIni** supprime le nom de la source de données de la section [sources de données ODBC] des informations système. Elle supprime également la section Spécification de la source de données des informations système.  
  
 Cette fonction doit être appelée uniquement à partir d’une bibliothèque de configuration de pilote.  
  
## <a name="related-functions"></a>Fonctions connexes  
  
|Pour obtenir des informations sur|Consultez|  
|---------------------------|---------|  
|Ajout, modification ou suppression d’une source de données|[ConfigDSN](../../../odbc/reference/syntax/configdsn-function.md)|  
|Ajout, modification ou suppression d’une source de données|[SQLConfigDataSource](../../../odbc/reference/syntax/sqlconfigdatasource-function.md)|  
|Suppression de la source de données par défaut|[SQLRemoveDefaultDataSource](../../../odbc/reference/syntax/sqlremovedefaultdatasource-function.md)|  
|Ajout d’un nom de source de données aux informations système|[SQLWriteDSNToIni](../../../odbc/reference/syntax/sqlwritedsntoini-function.md)|
