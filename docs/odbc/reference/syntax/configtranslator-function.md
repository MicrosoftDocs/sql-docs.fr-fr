---
description: ConfigTranslator, fonction
title: ConfigTranslator fonction) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
apiname:
- ConfigTranslator
apilocation:
- sqlsrv32.dll
apitype: dllExport
f1_keywords:
- ConfigTranslator
helpviewer_keywords:
- ConfigTranslator [ODBC]
ms.assetid: 7c22f07e-36de-425b-aa67-e32a84afae92
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 9b99628b801199c7e2d7fd033e1b0728f1538932
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/17/2020
ms.locfileid: "88461271"
---
# <a name="configtranslator-function"></a>ConfigTranslator, fonction
**Conformité**  
 Version introduite : ODBC 2,0  
  
 **Résumé**  
 **ConfigTranslator** retourne une option de traduction par défaut pour un traducteur. Il peut se trouver dans la DLL du traducteur ou dans une DLL d’installation distincte.  
  
## <a name="syntax"></a>Syntaxe  
  
```cpp  
  
BOOL ConfigTranslator(  
     HWND     hwndParent,  
     DWORD *  pvOption);  
```  
  
## <a name="arguments"></a>Arguments  
 *hwndParent*  
 Entrée Handle de fenêtre parente. La fonction n’affiche pas de boîtes de dialogue si le handle a la valeur null.  
  
 *pvOption*  
 Sortie Option de traduction 32 bits.  
  
## <a name="returns"></a>Retours  
 La fonction retourne TRUE si elle réussit, FALSe en cas d’échec.  
  
## <a name="diagnostics"></a>Diagnostics  
 Quand **ConfigTranslator** retourne false, une valeur * \* pfErrorCode* associée est publiée dans la mémoire tampon d’erreur du programme d’installation par un appel à **SQLPostInstallerError** et peut être obtenue en appelant **SQLInstallerError**. Le tableau suivant répertorie les valeurs * \* pfErrorCode* qui peuvent être retournées par **SQLInstallerError** et les explique dans le contexte de cette fonction.  
  
|*\*pfErrorCode*|Error|Description|  
|---------------------|-----------|-----------------|  
|ODBC_ERROR_INVALID_HWND|Handle de fenêtre non valide|L’argument *hwndParent* n’est pas valide ou a la valeur null.|  
|ODBC_ERROR_DRIVER_SPECIFIC|Erreur spécifique au pilote ou au traducteur|Erreur propre au pilote pour laquelle il n’existe aucune erreur de programme d’installation ODBC définie. L’argument *SzError* dans un appel à la fonction **SQLPostInstallerError** doit contenir le message d’erreur spécifique au pilote.|  
|ODBC_ERROR_INVALID_OPTION|Option de traduction non valide|L’argument *pvOption* contient une valeur non valide.|  
  
## <a name="comments"></a>Commentaires  
 Si le traducteur ne prend en charge qu’une seule option de traduction, **ConfigTranslator** retourne true et définit *pvOption* sur l’option 32 bits. Dans le cas contraire, il détermine l’option de traduction par défaut à utiliser. **ConfigTranslator** peut afficher une boîte de dialogue permettant à un utilisateur de sélectionner une option de traduction par défaut.  
  
## <a name="related-functions"></a>Fonctions connexes  
  
|Pour obtenir des informations sur|Consultez|  
|---------------------------|---------|  
|Obtention d’une option de traduction|[SQLGetConnectAttr](../../../odbc/reference/syntax/sqlgetconnectattr-function.md)|  
|Sélection d’un convertisseur|[SQLGetTranslator](../../../odbc/reference/syntax/sqlgettranslator-function.md)|  
|Définition d’une option de traduction|[SQLSetConnectAttr](../../../odbc/reference/syntax/sqlsetconnectattr-function.md)|
