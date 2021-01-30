---
description: SQLRemoveDriverManager, fonction
title: SQLRemoveDriverManager fonction) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLRemoveDriverManager
apilocation:
- sqlsrv32.dll
apitype: dllExport
f1_keywords:
- SQLRemoveDriverManager
helpviewer_keywords:
- SQLRemoveDriverManager function function [ODBC]
ms.assetid: 3a41511f-6603-4b81-a815-7883874023c4
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 92359b7df67535f8bee5a5a3dfb4b7a7348f54f9
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99192497"
---
# <a name="sqlremovedrivermanager-function"></a>SQLRemoveDriverManager, fonction
**Conformité**  
 Version introduite : ODBC 3,0 : déconseillé dans Windows XP Service Pack 2, Windows Server 2003 Service Pack 1 et les systèmes d’exploitation ultérieurs.  
  
 **Résumé**  
 **SQLRemoveDriverManager** modifie ou supprime les informations relatives aux composants ODBC Core de l’entrée Odbcinst.ini dans les informations système.  
  
## <a name="syntax"></a>Syntaxe  
  
```cpp  
  
BOOL SQLRemoveDriverManager(  
     LPDWORD     pdwUsageCount);  
```  
  
## <a name="arguments"></a>Arguments  
 *pdwUsageCount*  
 Sortie Nombre d’utilisations du gestionnaire de pilotes après l’appel de cette fonction.  
  
## <a name="returns"></a>Retours  
 La fonction retourne TRUE si elle réussit, FALSe en cas d’échec. Si aucune entrée n’existe dans les informations système lorsque cette fonction est appelée, la fonction retourne FALSe.  
  
## <a name="diagnostics"></a>Diagnostics  
 Quand **SQLRemoveDriverManager** retourne false, une valeur *\* pfErrorCode* associée peut être obtenue en appelant **SQLInstallerError**. Le tableau suivant répertorie les valeurs *\* pfErrorCode* qui peuvent être retournées par **SQLInstallerError** et les explique dans le contexte de cette fonction.  
  
|*\*pfErrorCode*|Erreur|Description|  
|---------------------|-----------|-----------------|  
|ODBC_ERROR_GENERAL_ERR|Erreur générale du programme d’installation|Une erreur s’est produite pour laquelle aucune erreur d’installation spécifique n’a été rencontrée.|  
|ODBC_ERROR_COMPONENT_NOT_FOUND|Composant introuvable dans le registre|Le programme d’installation n’a pas pu supprimer les informations du gestionnaire de pilotes, car il n’existait pas dans le registre ou est introuvable dans le registre.|  
|ODBC_ERROR_USAGE_UPDATE_FAILED|Impossible d’incrémenter ou de décrémenter le nombre d’utilisations du composant|Le programme d’installation n’a pas pu décrémenter le nombre d’utilisations du gestionnaire de pilotes.|  
|ODBC_ERROR_OUT_OF_MEM|Mémoire insuffisante|Le programme d’installation n’a pas pu exécuter la fonction en raison d’un manque de mémoire.|  
  
## <a name="comments"></a>Commentaires  
 **SQLRemoveDriverManager** complète la fonction **SQLInstallDriverManager** et met à jour le nombre d’utilisations des composants dans les informations système. Cette fonction doit être appelée uniquement à partir d’une application d’installation.  
  
 **SQLRemoveDriverManager** décrémente le nombre d’utilisations du composant principal de 1. Si le nombre d’utilisations des composants atteint 0, les informations du système d’entrée sont supprimées. L’entrée du composant principal se trouve à l’emplacement suivant dans les informations système, sous le titre « ODBC Core » :  
  
 `HKEY_LOCAL_MACHINE`  
  
 `SOFTWARE`  
  
 `ODBC`  
  
 `Odbcinst.ini`  
  
> [!CAUTION]  
>  Une application ne doit pas supprimer physiquement les fichiers du gestionnaire de pilotes lorsque le nombre d’utilisations des composants et le nombre d’utilisations du fichier atteignent zéro.  
  
 **SQLRemoveDriverManager** ne supprime pas réellement les fichiers. Le programme appelant est chargé de supprimer les fichiers et de conserver le nombre d’utilisations des fichiers. Toutefois, les fichiers du gestionnaire de pilotes ne doivent pas être supprimés lorsque le nombre d’utilisations des composants et le nombre d’utilisations des fichiers ont atteint zéro, car ces fichiers peuvent être utilisés par d’autres applications qui n’ont pas incrémenté le nombre d’utilisations des fichiers.  
  
 **SQLRemoveDriverManager** est appelé dans le cadre du processus de désinstallation. Les composants ODBC Core (qui incluent le gestionnaire de pilotes, la bibliothèque de curseurs, le programme d’installation, la bibliothèque de langues, l’administrateur, les fichiers de thunk, etc.) sont désinstallés dans son ensemble. Les fichiers suivants ne sont pas supprimés quand **SQLRemoveDriverManager** est appelé dans le cadre du processus de désinstallation :  

:::row:::
    :::column:::
        ODBC32DLL  
        ODBCCR32.DLL  
        ODBCCU32.DLL  
        ODBCINT.DLL  
        ODBCTRAC.DLL  
        MSVCRT40.DLL  
        ODBCCP32.CPL  
    :::column-end:::
    :::column:::
        ODBCCP32.DLL  
        ODBC16GT.DLL  
        ODBC32GT.DLL  
        DS16GT.DLL  
        DS32GT.DLL  
        ODBCAD32.EXE  
    :::column-end:::
:::row-end:::

 **SQLRemoveDriverManager** est également appelé dans le cadre d’un processus de mise à niveau. Si une application détecte qu’elle doit effectuer une mise à niveau et qu’elle a déjà installé le pilote, le pilote doit être supprimé, puis réinstallé.  
  
 **SQLRemoveDriverManager** doit d’abord être appelé pour décrémenter le nombre d’utilisations du composant. **SQLInstallDriverEx** doit ensuite être appelé pour incrémenter le nombre d’utilisations des composants. Le programme d’installation de l’application doit remplacer les anciens fichiers du composant principal par les nouveaux fichiers. Le nombre d’utilisations de fichiers reste le même, et les autres applications qui utilisent les fichiers des composants de base de la version antérieure utilisent désormais les fichiers de version plus récents.  
  
## <a name="related-functions"></a>Fonctions connexes  
  
|Pour obtenir des informations sur|Consultez|  
|---------------------------|---------|  
|Installation d’un gestionnaire de pilotes|[SQLInstallDriverManager](../../../odbc/reference/syntax/sqlinstalldrivermanager-function.md)|
