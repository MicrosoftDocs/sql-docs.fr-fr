---
description: SQLInstallDriverManager, fonction
title: SQLInstallDriverManager fonction) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLInstallDriverManager
apilocation:
- sqlsrv32.dll
apitype: dllExport
f1_keywords:
- SQLInstallDriverManager
helpviewer_keywords:
- SQLInstallDriverManager function [ODBC]
ms.assetid: aebc439b-fffd-4d98-907a-0163f79aee8d
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: b85af0323de46972ec6d6c4bf88359209b0015d5
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99189694"
---
# <a name="sqlinstalldrivermanager-function"></a>SQLInstallDriverManager, fonction
**Conformité**  
 Version introduite : ODBC 1,0 : déconseillé dans Windows XP Service Pack 2, Windows Server 2003 Service Pack 1 et les systèmes d’exploitation ultérieurs  
  
 **Résumé**  
 **SQLInstallDriverManager** retourne le chemin d’accès du répertoire cible pour l’installation des composants ODBC Core. Le programme appelant doit réellement copier les fichiers du gestionnaire de pilotes dans le répertoire cible.  
  
## <a name="syntax"></a>Syntaxe  
  
```cpp  
  
BOOL SQLInstallDriverManager(  
     LPSTR    lpszPath,  
     WORD     cbPathMax,  
     WORD *   pcbPathOut);  
```  
  
## <a name="arguments"></a>Arguments  
 *lpszPath*  
 Sortie Chemin d’accès du répertoire cible de l’installation.  
  
 *cbPathMax*  
 Entrée Longueur de *lpszPath*. Il doit s’agir d’au moins _MAX_PATH octets.  
  
 *pcbPathOut*  
 Sortie Nombre total d’octets (à l’exception de l’octet de fin null) retournés dans *lpszPath*. Si le nombre d’octets disponibles à retourner est supérieur ou égal à *cbPathMax*, le chemin d’accès dans *lpszPath* est tronqué à *cbPathMax* moins le caractère de fin null. L’argument *pcbPathOut* peut être un pointeur null.  
  
## <a name="returns"></a>Retours  
 La fonction retourne TRUE si elle réussit, FALSe en cas d’échec.  
  
## <a name="diagnostics"></a>Diagnostics  
 Quand **SQLInstallDriverManager** retourne false, une valeur *\* pfErrorCode* associée peut être obtenue en appelant **SQLInstallerError**. Le tableau suivant répertorie les valeurs *\* pfErrorCode* qui peuvent être retournées par **SQLInstallerError** et les explique dans le contexte de cette fonction.  
  
|*\*pfErrorCode*|Erreur|Description|  
|---------------------|-----------|-----------------|  
|ODBC_ERROR_GENERAL_ERR|Erreur générale du programme d’installation|Une erreur s’est produite pour laquelle aucune erreur d’installation spécifique n’a été rencontrée.|  
|ODBC_ERROR_INVALID_BUFF_LEN|Longueur de la mémoire tampon non valide|L’argument *lpszPath* n’était pas suffisamment grand pour contenir le chemin de sortie. La mémoire tampon contient le chemin d’accès tronqué.<br /><br /> L’argument *cbPathMax* est inférieur à _MAX_PATH.|  
|ODBC_ERROR_USAGE_UPDATE_FAILED|Impossible d’incrémenter ou de décrémenter le nombre d’utilisations du composant|Le programme d’installation n’a pas pu incrémenter le nombre d’utilisations du composant ODBC Core.|  
|ODBC_ERROR_OUT_OF_MEM|Mémoire insuffisante|Le programme d’installation n’a pas pu exécuter la fonction en raison d’un manque de mémoire.|  
  
## <a name="comments"></a>Commentaires  
 **SQLInstallDriverManager** est appelé pour retourner le chemin d’accès des composants principaux ODBC et incrémenter le nombre d’utilisations des composants dans les informations système. Si une version du gestionnaire de pilotes existe déjà mais que le nombre d’utilisations des composants pour le pilote n’existe pas, la valeur du nouveau nombre d’utilisations du composant est définie sur 2.  
  
 Le programme d’installation de l’application est chargé de copier physiquement les fichiers de composants de base et de conserver le nombre d’utilisations des fichiers. Si un fichier de composant principal n’a pas encore été installé, le programme d’installation de l’application doit copier le fichier et créer le nombre d’utilisations de fichiers. Si le fichier a déjà été installé, le programme d’installation incrémente simplement le nombre d’utilisations de fichiers.  
  
 Si une version antérieure du gestionnaire de pilotes était précédemment installée par le programme d’installation de l’application, les composants de base doivent être désinstallés puis réinstallés, afin que le nombre d’utilisations du composant principal soit valide. **SQLRemoveDriverManager** doit d’abord être appelé pour décrémenter le nombre d’utilisations du composant. **SQLInstallDriverManager** doit ensuite être appelé pour incrémenter le nombre d’utilisations des composants. Le programme d’installation de l’application doit remplacer les anciens fichiers du composant principal par les nouveaux fichiers. Le nombre d’utilisations de fichiers reste le même, et les autres applications qui utilisaient les fichiers des composants de base de la version antérieure utilisent désormais les fichiers de version plus récents.  
  
 Dans une nouvelle installation des composants ODBC principaux, des pilotes et des convertisseurs, le programme d’installation de l’application doit appeler les fonctions suivantes dans l’ordre : **SQLInstallDriverManager**, **SQLInstallDriverEx**, **SQLConfigDriver** (avec un *fRequest* de ODBC_INSTALL_DRIVER), puis **SQLInstallTranslatorEx**. Lors de la désinstallation des composants de base, des pilotes et des traducteurs, le programme d’installation de l’application doit appeler les fonctions suivantes dans l’ordre : **SQLRemoveTranslator**, **SQLRemoveDriver**, puis **SQLRemoveDriverManager**. Ces fonctions doivent être appelées dans cette séquence. Dans une mise à niveau de tous les composants, toutes les fonctions de désinstallation doivent être appelées dans l’ordre, puis toutes les fonctions d’installation doivent être appelées dans l’ordre.  
  
## <a name="related-functions"></a>Fonctions connexes  
  
|Pour obtenir des informations sur|Consultez|  
|---------------------------|---------|  
|Ajout, modification ou suppression d’un pilote|[SQLConfigDriver](../../../odbc/reference/syntax/sqlconfigdriver-function.md)|  
|Installation d’un pilote|[SQLInstallDriverEx](../../../odbc/reference/syntax/sqlinstalldriverex-function.md)|  
|Installation d’un convertisseur|[SQLInstallTranslatorEx](../../../odbc/reference/syntax/sqlinstalltranslatorex-function.md)|  
|Suppression d’un pilote|[SQLRemoveDriver](../../../odbc/reference/syntax/sqlremovedriver-function.md)|  
|Suppression du gestionnaire de pilotes|[SQLRemoveDriverManager](../../../odbc/reference/syntax/sqlremovedrivermanager-function.md)|  
|Suppression d’un convertisseur|[SQLRemoveTranslator](../../../odbc/reference/syntax/sqlremovetranslator-function.md)|
