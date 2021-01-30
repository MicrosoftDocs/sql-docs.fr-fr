---
description: SQLRemoveTranslator, fonction
title: SQLRemoveTranslator fonction) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLRemoveTranslator
apilocation:
- sqlsrv32.dll
apitype: dllExport
f1_keywords:
- SQLRemoveTranslator
helpviewer_keywords:
- SQLRemoveTranslator function [ODBC]
ms.assetid: c6feda49-0359-4224-8de9-77125cf2397b
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: cc8d1c7340d548276f462512bf2b52a3deb37b79
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99192482"
---
# <a name="sqlremovetranslator-function"></a>SQLRemoveTranslator, fonction
**Conformité**  
 Version introduite : ODBC 3,0  
  
 **Résumé**  
 **SQLRemoveTranslator** supprime les informations relatives à un traducteur de la section Odbcinst.ini des informations système et décrémente le nombre d’utilisations de composants du traducteur de 1.  
  
## <a name="syntax"></a>Syntaxe  
  
```cpp  
  
BOOL SQLRemoveTranslator(  
     LPCSTR    lpszTranslator,  
     LPDWORD   lpdwUsageCount);  
```  
  
## <a name="arguments"></a>Arguments  
 *lpszTranslator*  
 Entrée Nom du traducteur inscrit dans la clé de Odbcinst.ini des informations système.  
  
 *lpdwUsageCount*  
 Sortie Nombre d’utilisations du traducteur après l’appel de cette fonction.  
  
## <a name="returns"></a>Retours  
 La fonction retourne TRUE si elle réussit, FALSe en cas d’échec. Si aucune entrée n’existe dans les informations système lorsque cette fonction est appelée, la fonction retourne FALSe.  
  
## <a name="diagnostics"></a>Diagnostics  
 Quand **SQLRemoveTranslator** retourne false, une valeur *\* pfErrorCode* associée peut être obtenue en appelant **SQLInstallerError**. Le tableau suivant répertorie les valeurs *\* pfErrorCode* qui peuvent être retournées par **SQLInstallerError** et les explique dans le contexte de cette fonction.  
  
|*\*pfErrorCode*|Erreur|Description|  
|---------------------|-----------|-----------------|  
|ODBC_ERROR_GENERAL_ERR|Erreur générale du programme d’installation|Une erreur s’est produite pour laquelle aucune erreur d’installation spécifique n’a été rencontrée.|  
|ODBC_ERROR_COMPONENT_NOT_FOUND|Composant introuvable dans le registre|Le programme d’installation n’a pas pu supprimer les informations du traducteur, car il n’existait pas dans le registre ou est introuvable dans le registre.|  
|ODBC_ERROR_INVALID_NAME|Nom de pilote ou de convertisseur non valide|L’argument *lpszTranslator* n’était pas valide.|  
|ODBC_ERROR_USAGE_UPDATE_FAILED|Impossible d’incrémenter ou de décrémenter le nombre d’utilisations du composant|Le programme d’installation n’a pas pu décrémenter le nombre d’utilisations du pilote.|  
|ODBC_ERROR_OUT_OF_MEM|Mémoire insuffisante|Le programme d’installation n’a pas pu exécuter la fonction en raison d’un manque de mémoire.|  
  
## <a name="comments"></a>Commentaires  
 **SQLRemoveTranslator** complète la fonction [SQLInstallTranslatorEx](../../../odbc/reference/syntax/sqlinstalltranslatorex-function.md) et met à jour le nombre d’utilisations des composants dans les informations système. Cette fonction doit être appelée uniquement à partir d’une application d’installation.  
  
 **SQLRemoveTranslator** décrémente le nombre d’utilisations du composant de 1. Si le nombre d’utilisations des composants atteint 0, l’entrée du traducteur dans les informations système sera supprimée. L’entrée Translator se trouve à l’emplacement suivant dans les informations système, sous le nom du traducteur :  
  
 `HKEY_LOCAL_MACHINE`  
  
 `SOFTWARE`  
  
 `ODBC`  
  
 `Odbcinst.ini`  
  
 **SQLRemoveTranslator** ne supprime pas réellement les fichiers. Le programme appelant est responsable de la suppression des fichiers et de la gestion du nombre d’utilisations de fichiers. Une fois que le nombre d’utilisations du composant et le nombre d’utilisations du fichier ont atteint zéro, le fichier est supprimé physiquement. Certains fichiers d’un composant peuvent être supprimés, et d’autres non supprimés, selon que les fichiers sont utilisés ou non par d’autres applications qui ont incrémenté le nombre d’utilisations de fichiers.  
  
 **SQLRemoveTranslator** est également appelé dans le cadre d’un processus de mise à niveau. Si une application détecte qu’elle doit effectuer une mise à niveau et qu’elle a déjà installé le pilote, le pilote doit être supprimé, puis réinstallé. **SQLRemoveTranslator** doit d’abord être appelé pour décrémenter le nombre d’utilisations du composant, puis **SQLInstallTranslatorEx** doit être appelé pour incrémenter le nombre d’utilisations des composants. Le programme d’installation de l’application doit remplacer physiquement les anciens fichiers par les nouveaux fichiers. Le nombre d’utilisations des fichiers reste le même, et les autres applications qui utilisent les fichiers de la version antérieure utilisent désormais la version plus récente.  
  
## <a name="related-functions"></a>Fonctions connexes  
  
|Pour obtenir des informations sur|Consultez|  
|---------------------------|---------|  
|Installation d’un convertisseur|[SQLInstallTranslatorEx](../../../odbc/reference/syntax/sqlinstalltranslatorex-function.md)|
