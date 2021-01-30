---
description: SQLConfigDriver, fonction
title: SQLConfigDriver fonction) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLConfigDriver
apilocation:
- sqlsrv32.dll
apitype: dllExport
f1_keywords:
- SQLConfigDriver
helpviewer_keywords:
- SQLConfigDriver function [ODBC]
ms.assetid: 4f681961-ac9f-4d88-b065-5258ba112642
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: abe28e8d870216cf090e57a93e0c75766db08231
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99165326"
---
# <a name="sqlconfigdriver-function"></a>SQLConfigDriver, fonction
**Conformité**  
 Version introduite : ODBC 2,5  
  
 **Résumé**  
 **SQLConfigDriver** charge la dll d’installation du pilote appropriée et appelle la fonction **ConfigDriver** .  
  
 Vous pouvez également accéder aux fonctionnalités de **SQLConfigDriver** avec [ODBCCONF.EXE](../../../odbc/odbcconf-exe.md).  
  
## <a name="syntax"></a>Syntaxe  
  
```cpp  
  
BOOL SQLConfigDriver(  
     HWND     hwndParent,  
     WORD     fRequest,  
     LPCSTR   lpszDriver,  
     LPCSTR   lpszArgs,  
     LPSTR    lpszMsg,  
     WORD     cbMsgMax,  
     WORD *   pcbMsgOut);  
```  
  
## <a name="arguments"></a>Arguments  
 *hwndParent*  
 Entrée Handle de fenêtre parente. La fonction n’affiche pas de boîtes de dialogue si le handle a la valeur null.  
  
 *fRequest*  
 Entrée Type de requête. *fRequest* doit contenir l’une des valeurs suivantes :  
  
 ODBC_CONFIG_DRIVER : modifie le délai de regroupement des connexions utilisé par le pilote.  
  
 ODBC_INSTALL_DRIVER : installe un nouveau pilote.  
  
 ODBC_REMOVE_DRIVER : supprime un pilote existant.  
  
 Cette option peut également être spécifique au pilote, auquel cas le *fRequest* pour la première option doit commencer à partir de ODBC_CONFIG_DRIVER_MAX + 1. Le *fRequest* pour toute option supplémentaire doit également démarrer à partir d’une valeur supérieure à ODBC_CONFIG_DRIVER_MAX + 1.  
  
 *lpszDriver*  
 Entrée Nom du pilote enregistré dans les informations système.  
  
 *lpszArgs*  
 Entrée Chaîne terminée par le caractère null qui contient des arguments pour un *fRequest* spécifique au pilote.  
  
 *lpszMsg*  
 Sortie Chaîne terminée par le caractère null qui contient un message de sortie de la configuration du pilote.  
  
 *cbMsgMax*  
 Entrée Longueur de *lpszMsg.*  
  
 *pcbMsgOut*  
 Sortie Nombre total d’octets disponibles à retourner dans *lpszMsg*. Si le nombre d’octets disponibles à retourner est supérieur ou égal à *cbMsgMax*, le message de sortie dans *lpszMsg* est tronqué à *cbMsgMax* moins le caractère de fin null. L’argument *pcbMsgOut* peut être un pointeur null.  
  
## <a name="returns"></a>Retours  
 La fonction retourne TRUE si elle réussit, FALSe en cas d’échec.  
  
## <a name="diagnostics"></a>Diagnostics  
 Quand **SQLConfigDriver** retourne false, une valeur *\* pfErrorCode* associée peut être obtenue en appelant **SQLInstallerError**. Le tableau suivant répertorie les valeurs *\* pfErrorCode* qui peuvent être retournées par **SQLInstallerError** et les explique dans le contexte de cette fonction.  
  
|*\*pfErrorCode*|Erreur|Description|  
|---------------------|-----------|-----------------|  
|ODBC_ERROR_GENERAL_ERR|Erreur générale du programme d’installation|Une erreur s’est produite pour laquelle aucune erreur d’installation spécifique n’a été rencontrée.|  
|ODBC_ERROR_INVALID_BUFF_LEN|Longueur de la mémoire tampon non valide|L’argument *lpszMsg* n’était pas valide.|  
|ODBC_ERROR_INVALID_HWND|Handle de fenêtre non valide|L’argument *hwndParent* n’était pas valide.|  
|ODBC_ERROR_INVALID_REQUEST_TYPE|Type de demande non valide|L’argument *fRequest* ne faisait pas partie des éléments suivants :<br /><br /> ODBC_INSTALL_DRIVER ODBC_REMOVE_DRIVER<br /><br /> L’argument *fRequest* était une option spécifique au pilote qui était inférieure ou égale à ODBC_CONFIG_DRIVER_MAX.|  
|ODBC_ERROR_INVALID_NAME|Nom de pilote ou de convertisseur non valide|L’argument *lpszDriver* n’était pas valide. Il est introuvable dans le registre.|  
|ODBC_ERROR_INVALID_KEYWORD_VALUE|Paires mot clé/valeur non valides|L’argument *lpszArgs* contient une erreur de syntaxe.|  
|ODBC_ERROR_REQUEST_FAILED|Échec de la *requête*|Le programme d’installation n’a pas pu effectuer l’opération demandée par l’argument *fRequest* . L’appel à **ConfigDriver** a échoué.|  
|ODBC_ERROR_LOAD_LIBRARY_FAILED|Impossible de charger la bibliothèque d’installation du pilote ou du convertisseur|Impossible de charger la bibliothèque d’installation du pilote.|  
|ODBC_ERROR_OUT_OF_MEM|Mémoire insuffisante|Le programme d’installation n’a pas pu exécuter la fonction en raison d’un manque de mémoire.|  
  
## <a name="comments"></a>Commentaires  
 **SQLConfigDriver** permet à une application d’appeler la routine **ConfigDriver** d’un pilote sans avoir à connaître le nom et charger la dll d’installation propre au pilote. Un programme d’installation appelle cette fonction une fois la DLL d’installation du pilote installée. Le programme appelant doit savoir que cette fonction peut ne pas être disponible pour tous les pilotes. Dans ce cas, le programme appelant doit continuer sans erreur.  
  
## <a name="driver-specific-options"></a>Options de Driver-Specific  
 Une application peut demander des fonctionnalités spécifiques au pilote exposées par le pilote à l’aide de l’argument *fRequest* . Le *fRequest* pour la première option est ODBC_CONFIG_DRIVER_MAX + 1, et les options supplémentaires sont incrémentées de 1 à partir de cette valeur. Les arguments requis par le pilote pour cette fonction doivent être fournis dans une chaîne terminée par le caractère null passée dans l’argument *lpszArgs* . Les pilotes fournissant ces fonctionnalités doivent tenir à jour un tableau d’options spécifiques au pilote. Les options doivent être entièrement documentées dans la documentation du pilote. Les créateurs d’applications qui utilisent des options spécifiques au pilote doivent savoir que cette utilisation rend l’application moins interopérable.  
  
## <a name="setting-connection-pooling-timeout"></a>Définition du délai d’attente du regroupement de connexions  
 Les propriétés du délai d’attente du regroupement de connexions peuvent être définies lorsque vous définissez la configuration du pilote. **SQLConfigDriver** est appelé avec un *fRequest* de ODBC_CONFIG_DRIVER et *lpszArgs* défini sur **CPTimeout**. **CPTimeout** détermine la durée pendant laquelle une connexion peut rester dans le pool de connexions sans être utilisée. Lorsque le délai d’attente expire, la connexion est fermée et supprimée du pool. Le délai d’attente par défaut est de 60 secondes.  
  
 Quand **SQLConfigDriver** est appelé avec *fRequest* défini sur ODBC_INSTALL_DRIVER ou ODBC_REMOVE_DRIVER, le gestionnaire de pilotes charge la dll d’installation du pilote appropriée et appelle la fonction **ConfigDriver** . Quand **SQLConfigDriver** est appelé avec un *fRequest* de ODBC_CONFIG_DRIVER, tout le traitement est effectué dans le programme d’installation ODBC, afin de ne pas avoir à charger la dll d’installation du pilote.  
  
## <a name="messages"></a>Messages  
 Une routine de configuration de pilote peut envoyer un message texte à une application en tant que chaînes se terminant par un caractère NULL dans la mémoire tampon *lpszMsg* . Le message sera tronqué en *cbMsgMax* moins le caractère de fin null par la fonction **ConfigDriver** s’il est supérieur ou égal à *cbMsgMax* caractères.  
  
## <a name="related-functions"></a>Fonctions connexes  
  
|Pour obtenir des informations sur|Consultez|  
|---------------------------|---------|  
|Ajout, modification ou suppression d’un pilote|[ConfigDriver](../../../odbc/reference/syntax/configdriver-function.md)(dans la dll d’installation)|  
|Suppression de la source de données par défaut|[SQLRemoveDefaultDataSource](../../../odbc/reference/syntax/sqlremovedefaultdatasource-function.md)|
