---
description: ConfigDSN, fonction
title: ConfigDSN fonction) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- ConfigDSN
apilocation:
- sqlsrv32.dll
apitype: dllExport
f1_keywords:
- ConfigDSN
helpviewer_keywords:
- ConfigDSN [ODBC]
ms.assetid: 01ced74e-c575-4a25-83f5-bd7d918123f8
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 42e85b0c77ccc71946836e3103953fc85206b6e2
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99174725"
---
# <a name="configdsn-function"></a>ConfigDSN, fonction
**Conformité**  
 Version introduite : ODBC 1,0  
  
 **Résumé**  
 **ConfigDSN** ajoute, modifie ou supprime des sources de données à partir des informations système. Il peut inviter l’utilisateur à fournir des informations de connexion. Il peut se trouver dans la DLL du pilote ou dans une DLL d’installation distincte.  
  
## <a name="syntax"></a>Syntaxe  
  
```cpp  
  
BOOL ConfigDSN(  
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
  
 ODBC_ADD_DSN : ajoutez une nouvelle source de données.  
  
 ODBC_CONFIG_DSN : configurer (modifier) une source de données existante.  
  
 ODBC_REMOVE_DSN : supprimer une source de données existante.  
  
 *lpszDriver*  
 Entrée Description du pilote (généralement le nom du SGBD associé) présentée aux utilisateurs au lieu du nom du pilote physique.  
  
 *lpszAttributes*  
 Entrée Liste d’attributs se terminant par un caractère null, sous la forme de paires mot clé-valeur. Pour plus d’informations, consultez la section « commentaires ».  
  
## <a name="returns"></a>Retours  
 La fonction retourne TRUE si elle réussit, FALSe en cas d’échec.  
  
## <a name="diagnostics"></a>Diagnostics  
 Quand **ConfigDSN** retourne false, une valeur *\* pfErrorCode* associée est publiée dans la mémoire tampon d’erreur du programme d’installation par un appel à **SQLPostInstallerError** et peut être obtenue en appelant **SQLInstallerError**. Le tableau suivant répertorie les valeurs *\* pfErrorCode* qui peuvent être retournées par **SQLInstallerError** et les explique dans le contexte de cette fonction.  
  
|*\*pfErrorCode*|Erreur|Description|  
|---------------------|-----------|-----------------|  
|ODBC_ERROR_INVALID_HWND|Handle de fenêtre non valide|L’argument *hwndParent* n’était pas valide.|  
|ODBC_ERROR_INVALID_KEYWORD_VALUE|Paires mot clé/valeur non valides|L’argument *lpszAttributes* contient une erreur de syntaxe.|  
|ODBC_ERROR_INVALID_NAME|Nom de pilote ou de convertisseur non valide|L’argument *lpszDriver* n’était pas valide. Il est introuvable dans le registre.|  
|ODBC_ERROR_INVALID_REQUEST_TYPE|Type de demande non valide|L’argument *fRequest* ne faisait pas partie des éléments suivants :<br /><br /> ODBC_ADD_DSN ODBC_CONFIG_DSN ODBC_REMOVE_DSN|  
|ODBC_ERROR_REQUEST_FAILED|Échec de la *requête*|Impossible d’effectuer l’opération demandée par l’argument *fRequest* .|  
|ODBC_ERROR_DRIVER_SPECIFIC|Erreur spécifique au pilote ou au traducteur|Erreur propre au pilote pour laquelle il n’existe aucune erreur de programme d’installation ODBC définie. L’argument *SzError* dans un appel à la fonction **SQLPostInstallerError** doit contenir le message d’erreur spécifique au pilote.|  
  
## <a name="comments"></a>Commentaires  
 **ConfigDSN** reçoit les informations de connexion de la dll du programme d’installation sous la forme d’une liste d’attributs sous la forme de paires mot clé-valeur. Chaque paire se termine par un octet NULL, et la liste entière se termine par un octet NULL. (Autrement dit, deux octets de valeur null marquent la fin de la liste.) Les espaces ne sont pas autorisés autour du signe égal dans la paire mot clé-valeur. **ConfigDSN** peut accepter des mots clés qui ne sont pas des mots clés valides pour **SQLBrowseConnect** et **SQLDriverConnect**. **ConfigDSN** ne prend pas nécessairement en charge tous les mots clés qui sont des mots clés valides pour **SQLBrowseConnect** et **SQLDriverConnect**. (**ConfigDSN** n’accepte pas le mot clé **Driver** .) Les mots clés utilisés par la fonction **ConfigDSN** doivent prendre en charge toutes les options nécessaires à la recréation de la source de données à l’aide de la fonctionnalité de configuration automatique du programme d’installation. Lorsque les utilisations des valeurs **ConfigDSN** et des valeurs de chaîne de connexion sont identiques, les mêmes mots clés doivent être utilisés.  
  
 Comme dans **SQLBrowseConnect** et **SQLDriverConnect**, les mots clés et leurs valeurs ne doivent pas contenir les **[] {} (),;? \* = ! @** caractères, et la valeur du mot clé **DSN** ne peut pas contenir uniquement des espaces. En raison de la grammaire du Registre, les mots clés et les noms de sources de données ne peuvent pas contenir de barre oblique inverse ( \\ ).  
  
 **ConfigDSN** doit appeler **SQLValidDSN** pour vérifier la longueur du nom de la source de données et vérifier qu’aucun caractère non valide n’est inclus dans le nom. Si le nom de la source de données est plus long que SQL_MAX_DSN_LENGTH ou contient des caractères non valides, **SQLValidDSN** retourne une erreur et **ConfigDSN** retourne une erreur. La longueur du nom de la source de données est également vérifiée par **SQLWriteDSNToIni**.  
  
 Par exemple, pour configurer une source de données qui requiert un ID d’utilisateur, un mot de passe et un nom de base de données, une application de configuration peut passer les paires mot clé/valeur suivantes :  
  
```  
DSN=Personnel Data\0UID=Smith\0PWD=Sesame\0DATABASE=Personnel\0\0  
```  
  
 Pour plus d’informations sur ces mots clés, consultez [SQLDriverConnect](../../../odbc/reference/syntax/sqldriverconnect-function.md) et la documentation de chaque pilote.  
  
 Pour afficher une boîte de dialogue, *hwndParent* ne doit pas avoir la valeur null.  
  
## <a name="adding-a-data-source"></a>Ajout d'une source de données  
 Si un nom de source de données est transmis à **ConfigDSN** dans *lpszAttributes*, **ConfigDSN** vérifie que le nom est valide. Si le nom de la source de données correspond à un nom de source de données existant et que *hwndParent* a la valeur null, **ConfigDSN** remplace le nom existant. S’il correspond à un nom existant et que *hwndParent* n’est pas null, **ConfigDSN** invite l’utilisateur à remplacer le nom existant.  
  
 Si *lpszAttributes* contient suffisamment d’informations pour se connecter à une source de données, **ConfigDSN** peut ajouter la source de données ou afficher une boîte de dialogue permettant à l’utilisateur de modifier les informations de connexion. Si *lpszAttributes* ne contient pas suffisamment d’informations pour se connecter à une source de données, **ConfigDSN** doit déterminer les informations nécessaires ; Si *hwndParent* n’a pas la valeur null, il affiche une boîte de dialogue pour récupérer les informations de l’utilisateur.  
  
 Si **ConfigDSN** affiche une boîte de dialogue, elle doit afficher toutes les informations de connexion qui lui sont passées dans *lpszAttributes*. En particulier, si un nom de source de données lui a été passé, **ConfigDSN** affiche ce nom, mais ne permet pas à l’utilisateur de le modifier. **ConfigDSN** peut fournir des valeurs par défaut pour les informations de connexion qui ne lui sont pas transmises dans *lpszAttributes*.  
  
 Si **ConfigDSN** ne parvient pas à obtenir les informations de connexion complètes pour une source de données, elle retourne false.  
  
 Si **ConfigDSN** peut obtenir des informations de connexion complètes pour une source de données, il appelle **SQLWRITEDSNTOINI** dans la dll du programme d’installation pour ajouter la nouvelle spécification de la source de données au fichier Odbc.ini (ou au registre). **SQLWriteDSNToIni** ajoute le nom de la source de données à la section [sources de données ODBC], crée la section de spécification de la source de données et ajoute le mot clé **Driver** avec la description du pilote comme valeur. **ConfigDSN** appelle **SQLWRITEPRIVATEPROFILESTRING** dans la dll du programme d’installation pour ajouter tous les mots clés et valeurs supplémentaires utilisés par le pilote.  
  
## <a name="modifying-a-data-source"></a>Modification d’une source de données  
 Pour modifier une source de données, un nom de source de données doit être passé à **ConfigDSN** dans *lpszAttributes*. **ConfigDSN** vérifie que le nom de la source de données se trouve dans le fichier Odbc.ini (ou dans le registre).  
  
 Si *hwndParent* a la valeur null, **ConfigDSN** utilise les informations dans *lpszAttributes* pour modifier les informations dans le fichier de Odbc.ini (ou le registre). Si *hwndParent* n’a pas la valeur null, **ConfigDSN** affiche une boîte de dialogue à l’aide des informations contenues dans *lpszAttributes*; pour les informations qui ne se trouvent pas dans *lpszAttributes*, il utilise les informations des informations système. L’utilisateur peut modifier les informations avant que **ConfigDSN** ne les stocke dans les informations système.  
  
 Si le nom de la source de données a été modifié, **ConfigDSN** appelle d’abord **SQLREMOVEDSNFROMINI** dans la dll du programme d’installation pour supprimer la spécification de la source de données existante du fichier Odbc.ini (ou du registre). Il suit ensuite les étapes de la section précédente pour ajouter la nouvelle spécification de source de données. Si le nom de la source de données n’a pas été modifié, **ConfigDSN** appelle **SQLWRITEPRIVATEPROFILESTRING** dans la dll du programme d’installation pour apporter d’autres modifications. **ConfigDSN** ne peut pas supprimer ou modifier la valeur du mot clé **Driver** .  
  
## <a name="deleting-a-data-source"></a>Suppression d’une source de données  
 Pour supprimer une source de données, un nom de source de données doit être passé à **ConfigDSN** dans *lpszAttributes*. **ConfigDSN** vérifie que le nom de la source de données se trouve dans le fichier Odbc.ini (ou dans le registre). Il appelle ensuite **SQLRemoveDSNFromIni** dans la dll du programme d’installation pour supprimer la source de données.  
  
## <a name="note"></a>Notes
 Si vous écrivez une version Unicode de cette routine, elle doit être appelée **ConfigDSNW**, avec des arguments LPCWSTR au lieu de LPCSTR.
  
## <a name="related-functions"></a>Fonctions connexes  
  
|Pour obtenir des informations sur|Consultez|  
|---------------------------|---------|  
|Ajout, modification ou suppression d’une source de données|[SQLConfigDataSource](../../../odbc/reference/syntax/sqlconfigdatasource-function.md)|  
|Obtention d’une valeur à partir du fichier Odbc.ini ou du Registre|[SQLGetPrivateProfileString](../../../odbc/reference/syntax/sqlgetprivateprofilestring-function.md)|  
|Suppression de la source de données par défaut|[SQLRemoveDefaultDataSource](../../../odbc/reference/syntax/sqlremovedefaultdatasource-function.md)|  
|Suppression d’un nom de source de données Odbc.ini (ou du registre)|[SQLRemoveDSNFromIni](../../../odbc/reference/syntax/sqlremovedsnfromini-function.md)|  
|Ajout d’un nom de source de données à Odbc.ini (ou au registre)|[SQLWriteDSNToIni](../../../odbc/reference/syntax/sqlwritedsntoini-function.md)|  
|Écriture d’une valeur dans le fichier Odbc.ini ou le registre|[SQLWritePrivateProfileString](../../../odbc/reference/syntax/sqlwriteprivateprofilestring-function.md)|
