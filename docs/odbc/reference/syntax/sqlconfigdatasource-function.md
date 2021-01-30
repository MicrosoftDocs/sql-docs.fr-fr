---
description: Fonction SQLConfigDataSource
title: Fonction SQLConfigDataSource | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLConfigDataSource
apilocation:
- sqlsrv32.dll
apitype: dllExport
f1_keywords:
- SQLConfigDataSource
helpviewer_keywords:
- SQLConfigDataSource function [ODBC]
ms.assetid: f8d6e342-c010-434e-b1cd-f5371fb50a14
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: ddcfac8f1f211884aa3f46db3f56a2762778ed92
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99165336"
---
# <a name="sqlconfigdatasource-function"></a>Fonction SQLConfigDataSource
**Conformité**  
 Version introduite : ODBC 1,0  
  
 **Résumé**  
 **SQLConfigDataSource** ajoute, modifie ou supprime des sources de données.  
  
 Vous pouvez également accéder aux fonctionnalités de **SQLConfigDataSource** avec [ODBCCONF.EXE](../../../odbc/odbcconf-exe.md).  
  
## <a name="syntax"></a>Syntaxe  
  
```cpp  
  
BOOL SQLConfigDataSource(  
     HWND     hwndParent,  
     WORD     fRequest,  
     LPCSTR   lpszDriver,  
     LPCSTR   lpszAttributes);  
```  
  
## <a name="arguments"></a>Arguments  
 *hwndParent*  
 Entrée Handle de fenêtre parente. La fonction n’affiche pas de boîtes de dialogue si le handle a la valeur null.  
  
 *fRequest*  
 Entrée Type de requête. L’argument *fRequest* doit contenir l’une des valeurs suivantes :  
  
 ODBC_ADD_DSN : ajoutez une nouvelle source de données utilisateur.  
  
 ODBC_CONFIG_DSN : configurer (modifier) une source de données utilisateur existante.  
  
 ODBC_REMOVE_DSN : supprimer une source de données utilisateur existante.  
  
 ODBC_ADD_SYS_DSN : ajoutez une nouvelle source de données système.  
  
 ODBC_CONFIG_SYS_DSN : modifier une source de données système existante.  
  
 ODBC_REMOVE_SYS_DSN : supprimer une source de données système existante.  
  
 ODBC_REMOVE_DEFAULT_DSN : supprimez la section de spécification de la source de données par défaut des informations système. (La section Spécification du pilote par défaut est également supprimée de l’entrée Odbcinst.ini dans les informations système. Ce *fRequest* exécute la même fonction que la fonction **SQLRemoveDefaultDataSource** déconseillée.) Quand cette option est spécifiée, tous les autres paramètres dans l’appel à **SQLConfigDataSource** doivent avoir la valeur null ; Si elles n’ont pas la valeur NULL, elles seront ignorées.  
  
 *lpszDriver*  
 Entrée Description du pilote (généralement le nom du SGBD associé) présentée aux utilisateurs au lieu du nom du pilote physique.  
  
 *lpszAttributes*  
 Entrée Liste d’attributs se terminant par un caractère null, sous la forme de paires mot clé-valeur. Pour plus d’informations, consultez [ConfigDSN](../../../odbc/reference/syntax/configdsn-function.md).  
  
## <a name="returns"></a>Retours  
 La fonction retourne TRUE si elle réussit, FALSe en cas d’échec. Si aucune entrée n’existe dans les informations système lorsque cette fonction est appelée, la fonction retourne FALSe.  
  
## <a name="diagnostics"></a>Diagnostics  
 Lorsque **SQLConfigDataSource** retourne false, une valeur *\* pfErrorCode* associée peut être obtenue en appelant **SQLInstallerError**. Le tableau suivant répertorie les valeurs *\* pfErrorCode* qui peuvent être retournées par **SQLInstallerError** et les explique dans le contexte de cette fonction.  
  
|*\*pfErrorCode*|Erreur|Description|  
|---------------------|-----------|-----------------|  
|ODBC_ERROR_GENERAL_ERR|Erreur générale du programme d’installation|Une erreur s’est produite pour laquelle aucune erreur d’installation spécifique n’a été rencontrée.|  
|ODBC_ERROR_INVALID_HWND|Handle de fenêtre non valide|L’argument *hwndParent* n’est pas valide ou a la valeur null.|  
|ODBC_ERROR_INVALID_REQUEST_TYPE|Type de demande non valide|L’argument *fRequest* ne faisait pas partie des éléments suivants :<br /><br /> ODBC_ADD_DSN ODBC_CONFIG_DSN ODBC_REMOVE_DSN ODBC_ADD_SYS_DSN ODBC_CONFIG_SYS_DSN ODBC_REMOVE_SYS_DSN ODBC_REMOVE_DEFAULT_DSN|  
|ODBC_ERROR_INVALID_NAME|Nom de pilote ou de convertisseur non valide|L’argument *lpszDriver* n’était pas valide. Il est introuvable dans le registre.|  
|ODBC_ERROR_INVALID_KEYWORD_VALUE|Paires mot clé/valeur non valides|L’argument *lpszAttributes* contient une erreur de syntaxe.|  
|ODBC_ERROR_REQUEST_FAILED|Échec de la *requête*|Le programme d’installation n’a pas pu effectuer l’opération demandée par l’argument *fRequest* . L’appel à **ConfigDSN** a échoué.|  
|ODBC_ERROR_LOAD_LIBRARY_FAILED|Impossible de charger la bibliothèque d’installation du pilote ou du convertisseur|Impossible de charger la bibliothèque d’installation du pilote.|  
|ODBC_ERROR_OUT_OF_MEM|Mémoire insuffisante|Le programme d’installation n’a pas pu exécuter la fonction en raison d’un manque de mémoire.|  
  
## <a name="comments"></a>Commentaires  
 **SQLConfigDataSource** utilise la valeur de *lpszDriver* pour lire le chemin d’accès complet de la dll d’installation du pilote à partir des informations système. Il charge la DLL et appelle **ConfigDSN** avec les mêmes arguments que ceux qui lui ont été transmis.  
  
 **SQLConfigDataSource** retourne false s’il ne parvient pas à trouver ou à charger la dll d’installation ou si l’utilisateur annule la boîte de dialogue. Dans le cas contraire, elle retourne l’État qu’elle a reçu de **ConfigDSN**.  
  
 **SQLConfigDataSource** mappe les *FRequest* DSN du système aux *fRequest* de DSN utilisateur (ODBC_ADD_SYS_DSN à ODBC_ADD_DSN, ODBC_CONFIG_SYS_DSN à ODBC_CONFIG_DSN et ODBC_REMOVE_SYS_DSN à ODBC_REMOVE_DSN). Pour distinguer les DSN utilisateur et système, **SQLConfigDataSource** définit le mode de configuration du programme d’installation conformément au tableau suivant. Avant de retourner, **SQLConfigDataSource** réinitialise le mode de configuration sur BOTHDSN. **ConfigDSN** (implémenté par les pilotes) doit appeler **SQLWriteDSNToIni** et **SQLWritePrivateProfileString** pour prendre en charge un DSN système. Pour plus d’informations, consultez [fonction ConfigDSN](../../../odbc/reference/syntax/configdsn-function.md).  
  
|*fRequest*|Mode de configuration|  
|----------------|------------------------|  
|ODBC_ADD_DSN|USERDSN_ONLY|  
|ODBC_CONFIG_DSN|USERDSN_ONLY|  
|ODBC_REMOVE_DSN|USERDSN_ONLY|  
|ODBC_ADD_SYS_DSN|SYSTEMDSN_ONLY|  
|ODBC_CONFIG_SYS_DSN|SYSTEMDSN_ONLY|  
|ODBC_REMOVE_SYS_DSN|SYSTEMDSN_ONLY|  
  
## <a name="related-functions"></a>Fonctions connexes  
  
|Pour obtenir des informations sur|Consultez|  
|---------------------------|---------|  
|Ajout, modification ou suppression d’une source de données|[ConfigDSN](../../../odbc/reference/syntax/configdsn-function.md) (dans la dll d’installation)|  
|Suppression d’un nom de source de données des informations système|[SQLRemoveDSNFromIni](../../../odbc/reference/syntax/sqlremovedsnfromini-function.md)|  
|Ajout d’un nom de source de données aux informations système|[SQLWriteDSNToIni](../../../odbc/reference/syntax/sqlwritedsntoini-function.md)|
