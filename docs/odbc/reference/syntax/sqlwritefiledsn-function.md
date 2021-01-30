---
description: SQLWriteFileDSN, fonction
title: SQLWriteFileDSN fonction) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLWriteFileDSN
apilocation:
- sqlsrv32.dll
apitype: dllExport
f1_keywords:
- SQLWriteFileDSN
helpviewer_keywords:
- SQLWriteFileDSN [ODBC]
ms.assetid: 9e18f56f-1061-416b-83d4-ffeec42ab5a9
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: f778c2669a55e3f63c1946d2b4323ae64242c533
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99191664"
---
# <a name="sqlwritefiledsn-function"></a>SQLWriteFileDSN, fonction
**Conformité**  
 Version introduite : ODBC 3,0  
  
 **Résumé**  
 **SQLWriteFileDSN** écrit les informations dans un fichier DSN.  
  
## <a name="syntax"></a>Syntaxe  
  
```cpp  
  
BOOL SQLWriteFileDSN(  
     LPCSTR     lpszFileName,  
     LPCSTR     lpszAppName,  
     LPCSTR     lpszKeyName,  
     LPCSTR     lpszString);  
```  
  
## <a name="arguments"></a>Arguments  
 *lpszFileName*  
 Entrée Pointeur vers le nom du fichier DSN. Une extension de DSN est ajoutée à tous les noms de fichiers qui n’ont pas déjà une extension DSN.  
  
 *lpszAppName*  
 Entrée Pointeur vers le nom de l’application. Il s’agit de « ODBC » pour la section ODBC.  
  
 *lpszKeyName*  
 Entrée Pointeur vers le nom de la clé à lire. Consultez « Commentaires » pour les mots clés réservés.  
  
 *lpszString*  
 Sortie Pointe vers la chaîne associée à la clé à écrire. La longueur maximale de la chaîne vers laquelle pointe cet argument est de 32 767 octets.  
  
## <a name="returns"></a>Retours  
 La fonction retourne TRUE si elle réussit, FALSe en cas d’échec.  
  
## <a name="diagnostics"></a>Diagnostics  
 Quand **SQLWriteFileDSN** retourne false, une valeur *\* pfErrorCode* associée peut être obtenue en appelant **SQLInstallerError**. Le tableau suivant répertorie les valeurs *\* pfErrorCode* qui peuvent être retournées par **SQLInstallerError** et les explique dans le contexte de cette fonction.  
  
|*\*pfErrorCode*|Erreur|Description|  
|---------------------|-----------|-----------------|  
|ODBC_ERROR_GENERAL_ERR|Erreur générale du programme d’installation|Une erreur s’est produite pour laquelle aucune erreur d’installation spécifique n’a été rencontrée.|  
|ODBC_ERROR_INVALID_PATH|Chemin d’installation non valide|Le chemin d’accès au nom de fichier spécifié dans l’argument *lpszFileName* n’est pas valide.|  
|ODBC_ERROR_INVALID_REQUEST_TYPE|Type de demande non valide|L’argument *lpszAppName*, *lpszKeyName* ou *lpszString* a la valeur null.|  
  
## <a name="comments"></a>Commentaires  
 ODBC réserve le nom de section [ODBC] dans lequel stocker les informations de connexion. Les mots clés réservés pour cette section sont les mêmes que ceux réservés pour une chaîne de connexion dans **SQLDriverConnect**. (Pour plus d’informations, consultez la description de la fonction [SQLDriverConnect](../../../odbc/reference/syntax/sqldriverconnect-function.md) .)  
  
 Les applications peuvent utiliser ces mots clés réservés pour écrire des informations directement dans un fichier DSN. Si une application souhaite créer ou modifier la chaîne de connexion sans DSN associée à un fichier DSN, elle peut appeler **SQLWriteFileDSN** pour n’importe quel mot clé de chaîne de connexion réservée dans la section [ODBC].  
  
 Si l’argument *lpszString* est un pointeur null, le mot clé pointé par l’argument *lpszKeyName* sera supprimé du fichier. DSN. Si les arguments *lpszString* et *lpszKeyName* sont tous deux des pointeurs null, la section vers laquelle pointe l’argument *lpszAppName* sera supprimée du fichier. DSN.  
  
## <a name="related-functions"></a>Fonctions connexes  
  
|Pour obtenir des informations sur|Consultez|  
|---------------------------|---------|  
|Lecture d’informations à partir de sources de données de fichier|[SQLReadFileDSN](../../../odbc/reference/syntax/sqlreadfiledsn-function.md)|
