---
description: SQLWriteDSNToIni, fonction
title: SQLWriteDSNToIni fonction) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLWriteDSNToIni
apilocation:
- sqlsrv32.dll
apitype: dllExport
f1_keywords:
- SQLWriteDSNToIni
helpviewer_keywords:
- SQLWriteDSNToIni [ODBC]
ms.assetid: dc7018b2-18d4-4657-96d0-086479a47474
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 58e09c90ef785d1c851a1b95ef9a861277f47db7
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99191697"
---
# <a name="sqlwritedsntoini-function"></a>SQLWriteDSNToIni, fonction
**Conformité**  
 Version introduite : ODBC 1,0  
  
 **Résumé**  
 **SQLWriteDSNToIni** ajoute une source de données aux informations système.  
  
## <a name="syntax"></a>Syntaxe  
  
```cpp  
  
BOOL SQLWriteDSNToIni(  
     LPCSTR   lpszDSN,  
     LPCSTR   lpszDriver);  
```  
  
## <a name="arguments"></a>Arguments  
 *lpszDSN*  
 Entrée Nom de la source de données à ajouter.  
  
 *lpszDriver*  
 Entrée Description du pilote (généralement le nom du SGBD associé) présentée aux utilisateurs au lieu du nom du pilote physique.  
  
## <a name="returns"></a>Retours  
 La fonction retourne TRUE si elle réussit, FALSe en cas d’échec.  
  
## <a name="diagnostics"></a>Diagnostics  
 Quand **SQLWriteDSNToIni** retourne false, une valeur *\* pfErrorCode* associée peut être obtenue en appelant **SQLInstallerError**. Le tableau suivant répertorie les valeurs *\* pfErrorCode* qui peuvent être retournées par **SQLInstallerError** et les explique dans le contexte de cette fonction.  
  
|*\*pfErrorCode*|Erreur|Description|  
|---------------------|-----------|-----------------|  
|ODBC_ERROR_GENERAL_ERR|Erreur générale du programme d’installation|Une erreur s’est produite pour laquelle aucune erreur d’installation spécifique n’a été rencontrée.|  
|ODBC_ERROR_INVALID_DSN|DSN non valide|L’argument *lpszDSN* contient une chaîne non valide pour un DSN.|  
|ODBC_ERROR_INVALID_NAME|Nom de pilote ou de convertisseur non valide|L’argument *lpszDriver* n’était pas valide.|  
|ODBC_ERROR_REQUEST_FAILED|Échec de la demande|Le programme d’installation n’a pas pu créer un DSN dans le registre.|  
|ODBC_ERROR_OUT_OF_MEM|Mémoire insuffisante|Le programme d’installation n’a pas pu exécuter la fonction en raison d’un manque de mémoire.|  
  
## <a name="comments"></a>Commentaires  
 **SQLWriteDSNToIni** ajoute la source de données à la section [sources de données ODBC] des informations système. Il crée ensuite une section de spécification pour la source de données et ajoute un mot clé unique (**pilote**) avec le nom de la dll du pilote comme valeur. Si la section de spécification de la source de données existe déjà, **SQLWriteDSNToIni** supprime l’ancienne section avant d’en créer une nouvelle.  
  
 L’appelant de cette fonction doit ajouter des mots clés et des valeurs spécifiques au pilote à la section Spécification de la source de données des informations système.  
  
 Si le nom de la source de données est par défaut, **SQLWriteDSNToIni** crée également la section Spécification du pilote par défaut dans les informations système.  
  
 Cette fonction doit être appelée uniquement à partir d’une DLL d’installation.  
  
## <a name="related-functions"></a>Fonctions connexes  
  
|Pour obtenir des informations sur|Consultez|  
|---------------------------|---------|  
|Ajout, modification ou suppression d’une source de données|[ConfigDSN](../../../odbc/reference/syntax/configdsn-function.md)(dans la dll d’installation)|  
|Ajout, modification ou suppression d’une source de données|[SQLConfigDataSource](../../../odbc/reference/syntax/sqlconfigdatasource-function.md)|  
|Suppression d’un nom de source de données des informations système|[SQLRemoveDSNFromIni](../../../odbc/reference/syntax/sqlremovedsnfromini-function.md)|
