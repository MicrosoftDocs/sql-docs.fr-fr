---
description: SQLWritePrivateProfileString, fonction
title: SQLWritePrivateProfileString fonction) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLWritePrivateProfileString
apilocation:
- sqlsrv32.dll
apitype: dllExport
f1_keywords:
- SQLWritePrivateProfileString
helpviewer_keywords:
- SQLWritePrivateProfileString [ODBC]
ms.assetid: 526f36a4-92ed-4874-9725-82d27c0b86f9
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 8f001e3a4ad2b6e3e4aad3f34e8a87be1c7b2b12
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99191646"
---
# <a name="sqlwriteprivateprofilestring-function"></a>SQLWritePrivateProfileString, fonction
**Conformité**  
 Version introduite : ODBC 2,0  
  
 **Résumé**  
 **SQLWritePrivateProfileString** écrit un nom de valeur et des données dans la sous-clé Odbc.ini des informations système.  
  
## <a name="syntax"></a>Syntaxe  
  
```cpp  
  
BOOL SQLWritePrivateProfileString(  
     LPCSTR     lpszSection,  
     LPCSTR     lpszEntry,  
     LPCSTR     lpszString,  
     LPCSTR     lpszFilename);  
```  
  
## <a name="arguments"></a>Arguments  
 *lpszSection*  
 Entrée Pointe vers une chaîne se terminant par un caractère null qui contient le nom de la section dans laquelle la chaîne sera copiée. Si la section n’existe pas, elle est créée. Le nom de la section est indépendant de la casse ; la chaîne peut être n’importe quelle combinaison de lettres majuscules et minuscules.  
  
 *lpszEntry*  
 Entrée Pointe vers une chaîne se terminant par un caractère null qui contient le nom de la clé à associer à une chaîne. Si la clé n’existe pas dans la section spécifiée, elle est créée. Si cet argument a la valeur NULL, la section entière, y compris toutes les entrées de la section, est supprimée.  
  
 *lpszString*  
 Entrée Pointe vers une chaîne se terminant par un caractère null à écrire dans le fichier. Si cet argument a la valeur NULL, la clé vers laquelle pointe l’argument *lpszEntry* est supprimée.  
  
 *lpszFilename*  
 Sortie Pointe vers une chaîne se terminant par un caractère null qui nomme le fichier d’initialisation.  
  
## <a name="returns"></a>Retours  
 La fonction retourne TRUE si elle réussit, FALSe en cas d’échec.  
  
## <a name="diagnostics"></a>Diagnostics  
 Quand **SQLWritePrivateProfileString** retourne false, une valeur *\* pfErrorCode* associée peut être obtenue en appelant **SQLInstallerError**. Le tableau suivant répertorie les valeurs *\* pfErrorCode* qui peuvent être retournées par **SQLInstallerError** et les explique dans le contexte de cette fonction.  
  
|*\*pfErrorCode*|Erreur|Description|  
|---------------------|-----------|-----------------|  
|ODBC_ERROR_GENERAL_ERR|Erreur générale du programme d’installation|Une erreur s’est produite pour laquelle aucune erreur d’installation spécifique n’a été rencontrée.|  
|ODBC_ERROR_REQUEST_FAILED|Échec de la demande|Les informations système demandées n’ont pas pu être écrites.|  
|ODBC_ERROR_OUT_OF_MEM|Mémoire insuffisante|Le programme d’installation n’a pas pu exécuter la fonction en raison d’un manque de mémoire.|  
  
## <a name="comments"></a>Commentaires  
 **SQLWritePrivateProfileString** est fourni comme un moyen simple de porter des pilotes et des dll de configuration de pilote de Microsoft® Windows® à Microsoft windows NT®/Windows 2000. Les appels à **WritePrivateProfileString** qui écrivent une chaîne de profil dans le fichier Odbc.ini doivent être remplacés par des appels à **SQLWritePrivateProfileString**. **SQLWritePrivateProfileString** appelle des fonctions dans l’API de® Win32 pour ajouter le nom de valeur et les données spécifiés à la sous-clé Odbc.ini des informations système.  
  
 Un mode de configuration indique où l’entrée de Odbc.ini répertoriant les valeurs DSN se trouve dans les informations système. Si le DSN est un DSN utilisateur (la variable d’État est USERDSN_ONLY), la fonction écrit dans l’entrée de Odbc.ini dans HKEY_CURRENT_USER. Si le DSN est un DSN système (SYSTEMDSN_ONLY), la fonction écrit dans l’entrée de Odbc.ini dans HKEY_LOCAL_MACHINE. Si la variable d’État est BOTHDSN, HKEY_CURRENT_USER est tentée et, en cas d’échec, HKEY_LOCAL_MACHINE est utilisé.  
  
## <a name="related-functions"></a>Fonctions connexes  
  
|Pour obtenir des informations sur|Consultez|  
|---------------------------|---------|  
|Obtention d’une valeur à partir des informations système|[SQLGetPrivateProfileString](../../../odbc/reference/syntax/sqlgetprivateprofilestring-function.md)|
