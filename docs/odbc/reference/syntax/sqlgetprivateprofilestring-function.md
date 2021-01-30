---
description: SQLGetPrivateProfileString, fonction
title: SQLGetPrivateProfileString fonction) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLGetPrivateProfileString
apilocation:
- sqlsrv32.dll
apitype: dllExport
f1_keywords:
- SQLGetPrivateProfileString
helpviewer_keywords:
- SQLGetPrivateProfileString function [ODBC]
ms.assetid: b72ca065-4d67-48df-baac-e18379a8320a
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 6ec5dfa03f04b004f704ad707fb7916d40e69716
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99186193"
---
# <a name="sqlgetprivateprofilestring-function"></a>SQLGetPrivateProfileString, fonction
**Conformité**  
 Version introduite : ODBC 2,0  
  
 **Résumé**  
 **SQLGetPrivateProfileString** obtient une liste de noms de valeurs ou de données correspondant à une valeur des informations système.  
  
## <a name="syntax"></a>Syntaxe  
  
```cpp  
  
int SQLGetPrivateProfileString(  
     LPCSTR   lpszSection,  
     LPCSTR   lpszEntry,  
     LPCSTR   lpszDefault,  
     LPCSTR   RetBuffer,  
     INT      cbRetBuffer,  
     LPCSTR   lpszFilename);  
```  
  
## <a name="arguments"></a>Arguments  
 *lpszSection*  
 Entrée Pointe vers une chaîne se terminant par un caractère null qui spécifie la section contenant le nom de la clé. Si cet argument a la valeur NULL, la fonction copie tous les noms de sections du fichier dans la mémoire tampon fournie.  
  
 *lpszEntry*  
 Entrée Pointe vers la chaîne terminée par le caractère null qui contient le nom de la clé dont la chaîne associée doit être récupérée. Si cet argument a la valeur NULL, tous les noms de clés de la section spécifiée par l’argument *lpszSection* sont copiés dans la mémoire tampon spécifiée par l’argument *RetBuffer* .  
  
 *lpszDefault*  
 Entrée Pointe vers une chaîne se terminant par un caractère null qui spécifie la valeur par défaut de la clé donnée si la clé est introuvable dans le fichier d’initialisation. Cet argument ne peut pas avoir la valeur null.  
  
 *RetBuffer*  
 Sortie Pointe vers la mémoire tampon qui reçoit la chaîne récupérée.  
  
 *cbRetBuffer*  
 Entrée Spécifie la taille, en caractères, de la mémoire tampon vers laquelle pointe l’argument *RetBuffer* .  
  
 *lpszFilename*  
 Entrée Pointe vers une chaîne se terminant par un caractère null qui nomme le fichier d’initialisation. Si cet argument ne contient pas de chemin d’accès complet au fichier, la recherche s’effectue dans le répertoire par défaut.  
  
## <a name="returns"></a>Retours  
 **SQLGetPrivateProfileString** retourne une valeur entière qui indique le nombre de caractères lus.  
  
## <a name="diagnostics"></a>Diagnostics  
 Lorsqu’un appel à **SQLGetPrivateProfileString** échoue, une valeur *\* pfErrorCode* associée peut être obtenue en appelant **SQLInstallerError**. Le tableau suivant répertorie les valeurs *\* pfErrorCode* qui peuvent être retournées par **SQLInstallerError** et les explique dans le contexte de cette fonction.  
  
|*\*pfErrorCode*|Erreur|Description|  
|---------------------|-----------|-----------------|  
|ODBC_ERROR_GENERAL_ERR|Erreur générale du programme d’installation|Une erreur s’est produite pour laquelle aucune erreur d’installation spécifique n’a été rencontrée.|  
|ODBC_ERROR_OUT_OF_MEM|Mémoire insuffisante|Le programme d’installation n’a pas pu exécuter la fonction en raison d’un manque de mémoire.|  
  
## <a name="comments"></a>Commentaires  
 **SQLGetPrivateProfileString** est fourni comme un moyen simple de porter des pilotes et des dll de configuration de pilote de Microsoft® Windows® à Microsoft windows NT®/Windows 2000. Les appels à **GetPrivateProfileString** qui récupèrent une chaîne de profil à partir du fichier Odbc.ini doivent être remplacés par des appels à **SQLGetPrivateProfileString**. **SQLGetPrivateProfileString** appelle des fonctions dans l’API de® Win32 pour récupérer les noms demandés de valeurs ou de données correspondant à une valeur de la sous-clé Odbc.ini des informations système.  
  
 Le mode de configuration (tel que défini par **SQLSetConfigMode**) indique où l’entrée de Odbc.ini répertoriant les valeurs DSN se trouve dans les informations système. Si le DSN est un DSN utilisateur (le mode de configuration est USERDSN_ONLY), la fonction lit à partir de l’entrée de Odbc.ini dans HKEY_CURRENT_USER. Si le DSN est un DSN système (SYSTEMDSN_ONLY), la fonction lit à partir de l’entrée de Odbc.ini dans HKEY_LOCAL_MACHINE. Si le mode de configuration est BOTHDSN, HKEY_CURRENT_USER est essayé et, en cas d’échec, HKEY_LOCAL_MACHINE est utilisé.  
  
## <a name="related-functions"></a>Fonctions connexes  
  
|Pour obtenir des informations sur|Consultez|  
|---------------------------|---------|  
|Écriture d’une valeur dans les informations système|[SQLWritePrivateProfileString](../../../odbc/reference/syntax/sqlwriteprivateprofilestring-function.md)|
