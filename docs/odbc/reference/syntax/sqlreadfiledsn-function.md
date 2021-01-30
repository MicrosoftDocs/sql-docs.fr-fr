---
description: SQLReadFileDSN, fonction
title: SQLReadFileDSN fonction) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLReadFileDSN
apilocation:
- sqlsrv32.dll
apitype: dllExport
f1_keywords:
- SQLReadFileDSN
helpviewer_keywords:
- SQLReadFileDSN function [ODBC]
ms.assetid: ead464aa-cdc3-47dd-a0c0-997711205d31
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 7a04747258f612a86746642ae11fb06e9b742662
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99192534"
---
# <a name="sqlreadfiledsn-function"></a>SQLReadFileDSN, fonction
**Conformité**  
 Version introduite : ODBC 3,0  
  
 **Résumé**  
 **SQLReadFileDSN** lit les informations à partir d’un fichier DSN.  
  
## <a name="syntax"></a>Syntaxe  
  
```cpp  
  
BOOL SQLReadFileDSN(  
     LPCSTR   lpszFileName,  
     LPCSTR   lpszAppName,  
     LPCSTR   lpszKeyName,  
     LPSTR    lpszString,  
     WORD     cbString,  
     WORD *   pcbString);  
```  
  
## <a name="arguments"></a>Arguments  
 *lpszFileName*  
 Entrée Pointeur vers la mémoire tampon de données contenant le nom du fichier. DSN. Une extension. DSN est ajoutée à tous les noms de fichiers qui n’ont pas déjà une extension. DSN. La valeur de *\* lpszFileName* doit être une chaîne terminée par le caractère null.  
  
 *lpszAppName*  
 Entrée Pointeur vers la mémoire tampon de données contenant le nom de l’application. Il s’agit de « ODBC » pour la section ODBC. La valeur de *\* lpszAppName* doit être une chaîne terminée par le caractère null.  
  
 *lpszKeyName*  
 Entrée Pointeur vers la mémoire tampon de données contenant le nom de la clé à lire. Consultez « Commentaires » pour les mots clés réservés. La valeur de *\* lpszAppName* doit être une chaîne terminée par le caractère null.  
  
 *lpszString*  
 Sortie Pointeur vers la mémoire tampon de données qui contient la chaîne associée à la clé à lire.  
  
 Si *\* lpszFileName* est un nom de fichier. DSN valide mais que l’argument *lpszAppName* est un pointeur null et que l’argument *lpszKeyName* est un pointeur null, *\* lpszString* contient une liste d’applications valides. Si *\* lpszFileName* est un nom de fichier. DSN valide et *\* lpszAppName* est un nom d’application valide, mais que l’argument *lpszKeyName* est un pointeur null, *\* lpszString* contient une liste de mots clés réservés valides dans la section appropriée du fichier DSN, délimitée par des points-virgules. Si *\* lpszFileName* est un nom de fichier. DSN valide mais *\* lpszAppName* est un pointeur null et que l’argument *lpszKeyName* est un pointeur null, *\* lpszString* contient une liste des sections du fichier DSN, séparées par des points-virgules.  
  
 *cbString*  
 Entrée Longueur de la mémoire tampon *\* lpszString* .  
  
 *pcbString*  
 Sortie Nombre total d’octets disponibles à retourner dans *\* lpszString*. Si le nombre d’octets disponibles à retourner est supérieur ou égal à *cbString*, la chaîne de sortie dans *\* lpszString* est tronquée à *cbString* moins le caractère de fin null. L’argument *pcbString* peut être un pointeur null.  
  
## <a name="returns"></a>Retours  
 La fonction retourne TRUE si elle réussit, FALSe en cas d’échec.  
  
## <a name="diagnostics"></a>Diagnostics  
 Quand **SQLReadFileDSN** retourne false, une valeur *\* pfErrorCode* associée peut être obtenue en appelant **SQLInstallerError**. Le tableau suivant répertorie les valeurs *\* pfErrorCode* qui peuvent être retournées par **SQLInstallerError** et les explique dans le contexte de cette fonction.  
  
|*\*pfErrorCode*|Erreur|Description|  
|---------------------|-----------|-----------------|  
|ODBC_ERROR_GENERAL_ERR|Erreur générale du programme d’installation|Une erreur s’est produite pour laquelle aucune erreur d’installation spécifique n’a été rencontrée.|  
|ODBC_ERROR_INVALID_BUFF_LEN|Longueur de la mémoire tampon non valide|L’argument *lpszString* a la valeur null.<br /><br /> L’argument *cbString* est inférieur ou égal à 0.|  
|ODBC_ERROR_INVALID_PATH|Chemin d’installation non valide|Le chemin d’accès au nom de fichier spécifié dans l’argument *lpszFileName* n’est pas valide.|  
|ODBC_ERROR_INVALID_REQUEST_TYPE|Type de demande non valide|L’argument *lpszAppName* était null, tandis que l’argument *lpszKeyName* était valide.|  
|ODBC_ERROR_OUT_OF_MEM|Mémoire insuffisante|Le programme d’installation n’a pas pu exécuter la fonction en raison d’un manque de mémoire.|  
|ODBC_ERROR_OUTPUT_STRING_TRUNCATED|Chaîne de sortie tronquée|La chaîne retournée dans *\* lpszString* a été tronquée, car la valeur de *cbString* était inférieure ou égale à la valeur de *\* pcbString*.|  
|ODBC_ERROR_REQUEST_FAILED|Échec de la demande|Le mot clé n’existait pas dans le nom de source de fichier.|  
  
## <a name="comments"></a>Commentaires  
 ODBC réserve le nom de section [ODBC] dans lequel stocker les informations de connexion. Les mots clés réservés pour cette section sont les mêmes que ceux réservés pour une chaîne de connexion dans **SQLDriverConnect**. (Pour plus d’informations, consultez la description de la fonction [SQLDriverConnect](../../../odbc/reference/syntax/sqldriverconnect-function.md) .)  
  
 Les applications peuvent utiliser ces mots clés réservés pour lire les informations dans un fichier DSN. Si une application souhaite rechercher la chaîne de connexion sans DSN associée à un fichier DSN, elle peut appeler **SQLReadFileDSN** pour l’un des mots clés de chaîne de connexion réservés dans la section [ODBC]. La chaîne de connexion complète transmise dans une connexion sans DSN est une combinaison de tous les mots clés (réservé et spécifique au pilote) dans la section [ODBC].  
  
## <a name="related-functions"></a>Fonctions connexes  
  
|Pour obtenir des informations sur|Consultez|  
|---------------------------|---------|  
|Écriture d’informations dans un fichier DSN|[SQLWriteFileDSN](../../../odbc/reference/syntax/sqlwritefiledsn-function.md)|
