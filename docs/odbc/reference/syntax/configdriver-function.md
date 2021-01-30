---
description: ConfigDriver, fonction
title: ConfigDriver fonction) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- ConfigDriver
apilocation:
- sqlsrv32.dll
apitype: dllExport
f1_keywords:
- ConfigDriver
helpviewer_keywords:
- ConfigDriver [ODBC]
ms.assetid: 9473f48f-bcae-4784-89c1-7839bad4ed13
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 5abe589b84eff738aeaf41139082781254fb0488
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99174737"
---
# <a name="configdriver-function"></a>ConfigDriver, fonction
**Conformité**  
 Version introduite : ODBC 2,5  
  
 **Résumé**  
 **ConfigDriver** permet à un programme d’installation d’effectuer des fonctions d’installation et de désinstallation sans exiger que le programme appelle **ConfigDSN**. Cette fonction exécute des fonctions spécifiques au pilote, telles que la création d’informations système spécifiques au pilote et l’exécution de conversions de DSN pendant l’installation, ainsi que le nettoyage des modifications d’informations système pendant la désinstallation. Cette fonction est exposée par la DLL d’installation du pilote ou par une DLL d’installation distincte.  
  
## <a name="syntax"></a>Syntaxe  
  
```cpp  
  
BOOL ConfigDriver(  
      HWND    hwndParent,  
      WORD    fRequest,  
      LPCSTR  lpszDriver,  
      LPCSTR  lpszArgs,  
      LPSTR   lpszMsg,  
      WORD    cbMsgMax,  
      WORD *  pcbMsgOut);  
```  
  
## <a name="arguments"></a>Arguments  
 *hwndParent*  
 Entrée Handle de fenêtre parente. La fonction n’affiche pas de boîtes de dialogue si le handle a la valeur null.  
  
 *fRequest*  
 Entrée Type de requête. L’argument *fRequest* doit contenir l’une des valeurs suivantes :  
  
 ODBC_INSTALL_DRIVER : installer un nouveau pilote.  
  
 ODBC_REMOVE_DRIVER : supprimer un pilote.  
  
 Cette option peut également être spécifique au pilote, auquel cas l’argument *fRequest* de la première option doit commencer à partir de ODBC_CONFIG_DRIVER_MAX + 1. L’argument *fRequest* d’une option supplémentaire doit également démarrer à partir d’une valeur supérieure à ODBC_CONFIG_DRIVER_MAX + 1.  
  
 *lpszDriver*  
 Entrée Nom du pilote inscrit dans la clé Odbcinst.ini des informations système.  
  
 *lpszArgs*  
 Entrée Chaîne terminée par le caractère null qui contient des arguments pour un *fRequest* spécifique au pilote.  
  
 *lpszMsg*  
 Sortie Chaîne terminée par le caractère null qui contient un message de sortie de la configuration du pilote.  
  
 *cbMsgMax*  
 Entrée Longueur de *lpszMsg*.  
  
 *pcbMsgOut*  
 Sortie Nombre total d’octets disponibles à retourner dans *lpszMsg*.  
  
 Si le nombre d’octets disponibles à retourner est supérieur ou égal à *cbMsgMax*, le message de sortie dans *lpszMsg* est tronqué à *cbMsgMax* moins le caractère de fin null. L’argument *pcbMsgOut* peut être un pointeur null.  
  
## <a name="returns"></a>Retours  
 La fonction retourne TRUE si elle réussit, FALSe en cas d’échec.  
  
## <a name="diagnostics"></a>Diagnostics  
 Quand **ConfigDriver** retourne false, une valeur *\* pfErrorCode* associée est publiée dans la mémoire tampon d’erreur du programme d’installation par un appel à **SQLPostInstallerError** et peut être obtenue en appelant **SQLInstallerError**. Le tableau suivant répertorie les valeurs *\* pfErrorCode* qui peuvent être retournées par **SQLInstallerError** et les explique dans le contexte de cette fonction.  
  
|*\*pfErrorCode*|Erreur|Description|  
|---------------------|-----------|-----------------|  
|ODBC_ERROR_INVALID_HWND|Handle de fenêtre non valide|L’argument *hwndParent* n’était pas valide.|  
|ODBC_ERROR_INVALID_REQUEST_TYPE|Type de demande non valide|L’argument *fRequest* ne faisait pas partie des éléments suivants :<br /><br /> ODBC_INSTALL_DRIVER ODBC_REMOVE_DRIVER<br /><br /> L’option spécifique au pilote est inférieure ou égale à ODBC_CONFIG_DRIVER_MAX.|  
|ODBC_ERROR_INVALID_NAME|Nom de pilote ou de convertisseur non valide|L’argument *lpszDriver* n’était pas valide. Il est introuvable dans le registre.|  
|ODBC_ERROR_REQUEST_FAILED|Échec de la *requête*|Impossible d’effectuer l’opération demandée par l’argument *fRequest* .|  
|ODBC_ERROR_DRIVER_SPECIFIC|Erreur spécifique au pilote ou au traducteur|Erreur propre au pilote pour laquelle il n’existe aucune erreur de programme d’installation ODBC définie. L’argument *SzError* dans un appel à la fonction **SQLPostInstallerError** doit contenir le message d’erreur spécifique au pilote.|  
  
## <a name="comments"></a>Commentaires  
  
### <a name="driver-specific-options"></a>Options de Driver-Specific  
 Une application peut demander des fonctionnalités spécifiques au pilote exposées par le pilote à l’aide de l’argument *fRequest* . Le *fRequest* pour la première option est ODBC_CONFIG_DRIVER_MAX plus 1, et les options supplémentaires sont incrémentées de 1 à partir de cette valeur. Les arguments requis par le pilote pour cette fonction doivent être fournis dans une chaîne terminée par le caractère null passée dans l’argument *lpszArgs* . Les pilotes fournissant ces fonctionnalités doivent tenir à jour un tableau d’options spécifiques au pilote. Les options doivent être entièrement documentées dans la documentation du pilote. Les créateurs d’applications qui utilisent des options spécifiques au pilote doivent savoir que cela rend l’application moins interopérable.  
  
### <a name="messages"></a>Messages  
 Une routine de configuration de pilote peut envoyer un message texte à une application sous la forme d’une chaîne se terminant par un caractère NULL dans la mémoire tampon *lpszMsg* . Le message sera tronqué en *cbMsgMax* moins le caractère de fin null par la fonction **ConfigDriver** s’il est supérieur ou égal à *cbMsgMax* caractères.
