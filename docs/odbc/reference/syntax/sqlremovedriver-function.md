---
description: SQLRemoveDriver, fonction
title: SQLRemoveDriver fonction) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLRemoveDriver
apilocation:
- sqlsrv32.dll
apitype: dllExport
f1_keywords:
- SQLRemoveDriver
helpviewer_keywords:
- SQLRemoveDriver function [ODBC]
ms.assetid: 9a3b4f8b-982b-44b9-ade6-754ff026dc90
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 2ee0165f815b874844f072c36325fb7afb8046c2
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99192508"
---
# <a name="sqlremovedriver-function"></a>SQLRemoveDriver, fonction
**Conformité**  
 Version introduite : ODBC 3,0  
  
 **Résumé**  
 **SQLRemoveDriver** modifie ou supprime les informations relatives au pilote de l’entrée Odbcinst.ini dans les informations système.  
  
## <a name="syntax"></a>Syntaxe  
  
```cpp  
  
BOOL SQLRemoveDriver(  
     LPCSTR   lpszDriver,  
     BOOL     fRemoveDSN,  
     LPDWORD  lpdwUsageCount);  
```  
  
## <a name="arguments"></a>Arguments  
 *lpszDriver*  
 Entrée Nom du pilote inscrit dans la clé Odbcinst.ini des informations système.  
  
 *fRemoveDSN*  
 Entrée Les valeurs valides sont les suivantes :  
  
 TRUE : supprimer les DSN associées au pilote spécifié dans *lpszDriver*. FALSe : ne supprimez pas les DSN associés au pilote spécifié dans *lpszDriver*.  
  
 *lpdwUsageCount*  
 Sortie Le nombre d’utilisations du pilote après l’appel de cette fonction.  
  
## <a name="returns"></a>Retours  
 La fonction retourne TRUE si elle réussit, FALSe en cas d’échec. Si aucune entrée n’existe dans les informations système lorsque cette fonction est appelée, la fonction retourne FALSe.  
  
## <a name="diagnostics"></a>Diagnostics  
 Quand **SQLRemoveDriver** retourne false, une valeur *\* pfErrorCode* associée peut être obtenue en appelant **SQLInstallerError**. Le tableau suivant répertorie les valeurs *\* pfErrorCode* qui peuvent être retournées par **SQLInstallerError** et les explique dans le contexte de cette fonction.  
  
|*\*pfErrorCode*|Erreur|Description|  
|---------------------|-----------|-----------------|  
|ODBC_ERROR_GENERAL_ERR|Erreur générale du programme d’installation|Une erreur s’est produite pour laquelle aucune erreur d’installation spécifique n’a été rencontrée.|  
|ODBC_ERROR_COMPONENT_NOT_FOUND|Composant introuvable dans le registre|Le programme d’installation n’a pas pu supprimer les informations du pilote, car il n’existait pas dans le registre ou est introuvable dans le registre.|  
|ODBC_ERROR_INVALID_NAME|Nom de pilote ou de convertisseur non valide|L’argument *lpszDriver* n’était pas valide.|  
|ODBC_ERROR_USAGE_UPDATE_FAILED|Impossible d’incrémenter ou de décrémenter le nombre d’utilisations du composant|Le programme d’installation n’a pas pu décrémenter le nombre d’utilisations du pilote.|  
|ODBC_ERROR_REQUEST_FAILED|Échec de la demande|L’argument *fRemoveDSN* a la valeur true ; Toutefois, un ou plusieurs DSN n’ont pas pu être supprimés. L’appel à **SQLConfigDriver** avec la demande de ODBC_REMOVE_DRIVER a échoué.|  
|ODBC_ERROR_OUT_OF_MEM|Mémoire insuffisante|Le programme d’installation n’a pas pu exécuter la fonction en raison d’un manque de mémoire.|  
  
## <a name="comments"></a>Commentaires  
 **SQLRemoveDriver** complète la fonction [SQLInstallDriverEx](../../../odbc/reference/syntax/sqlinstalldriverex-function.md) et met à jour le nombre d’utilisations des composants dans les informations système. Cette fonction doit être appelée uniquement à partir d’une application d’installation.  
  
 **SQLRemoveDriver** décrémente la valeur du nombre d’utilisations du composant de 1. Si le nombre d’utilisations des composants atteint 0, les éléments suivants se produisent :  
  
1.  La fonction **SQLConfigDriver** avec l’option ODBC_REMOVE_DRIVER est appelée. Si l’option *fRemoveDSN* a la valeur true, la fonction **ConfigDSN** appelle **SQLRemoveDSNFromIni** pour supprimer toutes les sources de données associées au pilote spécifié dans *lpszDriver.* Si l’option *fRemoveDSN* est définie sur false, les sources de données ne seront pas supprimées.  
  
2.  L’entrée du pilote dans les informations système sera supprimée. L’entrée du pilote se trouve à l’emplacement d’informations système suivant, sous le nom du pilote :  
  
     `HKEY_LOCAL_MACHINE`  
  
     `SOFTWARE`  
  
     `ODBC`  
  
     `Odbcinst.ini`  
  
 **SQLRemoveDriver** ne supprime pas réellement les fichiers. Le programme appelant est responsable de la suppression des fichiers et de la gestion du nombre d’utilisations de fichiers. Une fois que le nombre d’utilisations du composant et le nombre d’utilisations du fichier ont atteint zéro, le fichier est supprimé physiquement. Certains fichiers d’un composant peuvent être supprimés, et d’autres non supprimés, selon que les fichiers sont utilisés ou non par d’autres applications qui ont incrémenté le nombre d’utilisations de fichiers.  
  
 **SQLRemoveDriver** est également appelé dans le cadre d’un processus de mise à niveau. Si une application détecte qu’elle doit effectuer une mise à niveau et qu’elle a déjà installé le pilote, le pilote doit être supprimé, puis réinstallé. **SQLRemoveDriver** doit d’abord être appelé pour décrémenter le nombre d’utilisations du composant, puis **SQLInstallDriverEx** doit être appelé pour incrémenter le nombre d’utilisations des composants. Le programme d’installation de l’application doit remplacer les anciens fichiers par les nouveaux fichiers. Le nombre d’utilisations des fichiers reste le même, et les autres applications qui utilisent les fichiers de la version antérieure utilisent désormais la version plus récente.  
  
## <a name="related-functions"></a>Fonctions connexes  
  
|Pour obtenir des informations sur|Consultez|  
|---------------------------|---------|  
|Ajout, modification ou suppression d’un pilote|[ConfigDriver](../../../odbc/reference/syntax/configdriver-function.md) (dans la dll d’installation)|  
|Ajout, modification ou suppression d’un pilote|[SQLConfigDriver](../../../odbc/reference/syntax/sqlconfigdriver-function.md)|  
|Installation d’un pilote|[SQLInstallDriverEx](../../../odbc/reference/syntax/sqlinstalldriverex-function.md)|
