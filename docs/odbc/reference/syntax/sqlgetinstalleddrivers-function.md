---
description: SQLGetInstalledDrivers, fonction
title: SQLGetInstalledDrivers fonction) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLGetInstalledDrivers
apilocation:
- sqlsrv32.dll
apitype: dllExport
f1_keywords:
- SQLGetInstalledDrivers
helpviewer_keywords:
- SQLGetInstalledDrivers function [ODBC]
ms.assetid: a1983a2e-0edf-422e-bd1b-ec5db40a34bc
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 5f770254874b09158632ebaee071964c00265a46
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99180955"
---
# <a name="sqlgetinstalleddrivers-function"></a>SQLGetInstalledDrivers, fonction
**Conformité**  
 Version introduite : ODBC 1,0  
  
 **Résumé**  
 **SQLGetInstalledDrivers** lit la section [ODBC Drivers] des informations système et renvoie une liste des descriptions des pilotes installés.  
  
## <a name="syntax"></a>Syntaxe  
  
```cpp  
  
BOOL SQLGetInstalledDrivers(  
     LPSTR   lpszBuf,  
     WORD    cbBufMax,  
     WORD *  pcbBufOut);  
```  
  
## <a name="arguments"></a>Arguments  
 *lpszBuf*  
 Sortie Liste des descriptions des pilotes installés. Pour plus d’informations sur la structure de la liste, consultez « comments ».  
  
 *cbBufMax*  
 Entrée Longueur de *lpszBuf*.  
  
 *pcbBufOut*  
 Sortie Nombre total d’octets (à l’exception de l’octet de fin null) retournés dans *lpszBuf*. Si le nombre d’octets disponibles à retourner est supérieur ou égal à *cbBufMax*, la liste des descriptions des pilotes dans *lpszBuf* est tronquée à *cbBufMax* moins le caractère de fin null. L’argument *pcbBufOut* peut être un pointeur null.  
  
## <a name="returns"></a>Retours  
 La fonction retourne TRUE si elle réussit, FALSe en cas d’échec.  
  
## <a name="diagnostics"></a>Diagnostics  
 Quand **SQLGetInstalledDrivers** retourne false, une valeur *\* pfErrorCode* associée peut être obtenue en appelant **SQLInstallerError**. Le tableau suivant répertorie les valeurs *\* pfErrorCode* qui peuvent être retournées par **SQLInstallerError** et les explique dans le contexte de cette fonction.  
  
|*\*pfErrorCode*|Erreur|Description|  
|---------------------|-----------|-----------------|  
|ODBC_ERROR_GENERAL_ERR|Erreur générale du programme d’installation|Une erreur s’est produite pour laquelle aucune erreur d’installation spécifique n’a été rencontrée.|  
|ODBC_ERROR_INVALID_BUFF_LEN|Longueur de la mémoire tampon non valide|L’argument *lpszBuf* a la valeur null ou n’est pas valide, ou l’argument *cbBufMax* était inférieur ou égal à 0.|  
|ODBC_ERROR_COMPONENT_NOT_FOUND|Composant introuvable dans le registre|Le programme d’installation n’a pas pu trouver la section [ODBC Drivers] dans le registre.|  
|ODBC_ERROR_OUT_OF_MEM|Mémoire insuffisante|Le programme d’installation n’a pas pu exécuter la fonction en raison d’un manque de mémoire.|  
  
## <a name="comments"></a>Commentaires  
 Chaque description de pilote se termine par un octet NULL, et la liste entière se termine par un octet NULL. (Autrement dit, deux octets de valeur null marquent la fin de la liste.) Si la mémoire tampon allouée n’est pas assez grande pour contenir la totalité de la liste, la liste est tronquée sans erreur. Une erreur est retournée si un pointeur null est passé en tant que *lpszBuf*.  
  
## <a name="related-functions"></a>Fonctions connexes  
  
|Pour obtenir des informations sur|Consultez|  
|---------------------------|---------|  
|Renvoi des descriptions et des attributs des pilotes|[SQLDrivers](../../../odbc/reference/syntax/sqldrivers-function.md)|
