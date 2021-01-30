---
description: SQLGetTranslator, fonction
title: SQLGetTranslator fonction) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLGetTranslator
apilocation:
- sqlsrv32.dll
apitype: dllExport
f1_keywords:
- SQLGetTranslator
helpviewer_keywords:
- SQLGetTranslator function [ODBC]
ms.assetid: 33879db3-5ef9-4585-9be5-69376157e017
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 5ec960312c652c625a6fb27388bd9c21a09f3206
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99189731"
---
# <a name="sqlgettranslator-function"></a>SQLGetTranslator, fonction
**Conformité**  
 Version introduite : ODBC 2,0  
  
 **Résumé**  
 **SQLGetTranslator** affiche une boîte de dialogue à partir de laquelle un utilisateur peut sélectionner un traducteur.  
  
## <a name="syntax"></a>Syntaxe  
  
```cpp  
  
BOOL SQLGetTranslator(  
     HWND      hwndParent,  
     LPSTR     lpszName,  
     WORD      cbNameMax,  
     WORD *    pcbNameOut,  
     LPSTR     lpszPath,  
     WORD      cbPathMax,  
     WORD *    pcbPathOut,  
     DWORD *   pvOption);  
```  
  
## <a name="arguments"></a>Arguments  
 *hwndParent*  
 Entrée Handle de fenêtre parente.  
  
 *lpszName*  
 [Entrée/sortie] Nom du traducteur à partir des informations système.  
  
 *cbNameMax*  
 Entrée Longueur maximale de la mémoire tampon *lpszName* .  
  
 *pcbNameOut*  
 [Entrée/sortie] Nombre total d’octets (à l’exclusion de l’octet de fin null) passés ou retournés dans *lpszName*. Si le nombre d’octets disponibles à retourner est supérieur ou égal à *cbNameMax*, le nom du traducteur dans *lpszName* est tronqué en *cbNameMax* moins le caractère de fin null. L’argument *pcbNameOut* peut être un pointeur null.  
  
 *lpszPath*  
 Sortie Chemin d’accès complet de la DLL de traduction.  
  
 *cbPathMax*  
 Entrée Longueur maximale de la mémoire tampon *lpszPath* .  
  
 *pcbPathOut*  
 Sortie Nombre total d’octets (à l’exception de l’octet de fin null) retournés dans *lpszPath*. Si le nombre d’octets disponibles à retourner est supérieur ou égal à *cbPathMax*, le chemin d’accès à la dll de traduction dans *lpszPath* est tronqué à *cbPathMax* moins le caractère de fin null. L’argument *pcbPathOut* peut être un pointeur null.  
  
 *pvOption*  
 [Sortie] option de traduction 32 bits.  
  
## <a name="returns"></a>Retours  
 La fonction retourne TRUE si elle réussit, FALSe si elle échoue ou si l’utilisateur annule la boîte de dialogue.  
  
## <a name="diagnostics"></a>Diagnostics  
 Quand **SQLGetTranslator** retourne false, une valeur *\* pfErrorCode* associée peut être obtenue en appelant **SQLInstallerError**. Le tableau suivant répertorie les valeurs *\* pfErrorCode* qui peuvent être retournées par **SQLInstallerError** et les explique dans le contexte de cette fonction.  
  
|*\*pfErrorCode*|Erreur|Description|  
|---------------------|-----------|-----------------|  
|ODBC_ERROR_GENERAL_ERR|Erreur générale du programme d’installation|Une erreur s’est produite pour laquelle aucune erreur d’installation spécifique n’a été rencontrée.|  
|ODBC_ERROR_INVALID_BUFF_LEN|Longueur de la mémoire tampon non valide|L’argument *cbNameMax* ou *cbPathMax* était inférieur ou égal à 0.|  
|ODBC_ERROR_INVALID_HWND|Handle de fenêtre non valide|L’argument *hwndParent* n’est pas valide ou a la valeur null.|  
|ODBC_ERROR_INVALID_NAME|Nom de pilote ou de convertisseur non valide|L’argument *lpszName* n’était pas valide. Il est introuvable dans le registre.|  
|ODBC_ERROR_LOAD_LIBRARY_FAILED|Impossible de charger la bibliothèque d’installation du pilote ou du convertisseur|Impossible de charger la bibliothèque de traducteurs.|  
|ODBC_ERROR_INVALID_OPTION|Option de transaction non valide|L’argument *pvOption* contient une valeur non valide.|  
|ODBC_ERROR_OUT_OF_MEM|Mémoire insuffisante|Le programme d’installation n’a pas pu exécuter la fonction en raison d’un manque de mémoire.|  
  
## <a name="comments"></a>Commentaires  
 Si *hwndParent* a la valeur null ou si *lpszName*, *lpszPath* ou *pvOption* est un pointeur null, **SQLGetTranslator** retourne false. Dans le cas contraire, elle affiche la liste des convertisseurs installés dans la boîte de dialogue suivante.  
  
 ![Boîte de dialogue Sélectionner le convertisseur](../../../odbc/reference/syntax/media/ch23j.gif "CH23J")  
  
 Si *lpszName* contient un nom de traducteur valide, il est sélectionné. Sinon, \<No Translator> est sélectionné.  
  
 Si l’utilisateur choisit \<No Translator> , le contenu de *lpszName*, *lpszPath* et *pvOption* ne sont pas affectés. **SQLGetTranslator** définit *pcbNameOut* et *PCBPATHOUT* sur 0 et retourne true.  
  
 Si l’utilisateur choisit un traducteur, **SQLGetTranslator** appelle **CONFIGTRANSLATOR** dans la dll d’installation du traducteur. Si **ConfigTranslator** retourne la valeur false, **SQLGetTranslator** retourne à sa boîte de dialogue. Si **ConfigTranslator** retourne la valeur true, **SQLGetTranslator** retourne la valeur true, ainsi que le nom du traducteur, le chemin d’accès et l’option de traduction sélectionnés.  
  
## <a name="related-functions"></a>Fonctions connexes  
  
|Pour obtenir des informations sur|Consultez|  
|---------------------------|---------|  
|Configuration d’un convertisseur|[ConfigTranslator](../../../odbc/reference/syntax/configtranslator-function.md)|  
|Obtention d’un attribut de traduction|[SQLGetConnectAttr](../../../odbc/reference/syntax/sqlgetconnectattr-function.md)|  
|Définition d’un attribut de traduction|[SQLSetConnectAttr](../../../odbc/reference/syntax/sqlsetconnectattr-function.md)|
