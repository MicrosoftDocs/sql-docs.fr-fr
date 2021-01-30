---
description: SQLManageDataSources
title: SQLManageDataSources | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLManageDataSources
apilocation:
- sqlsrv32.dll
apitype: dllExport
f1_keywords:
- SQLManageDataSources
helpviewer_keywords:
- SQLManageDataSources [ODBC]
ms.assetid: ac6d186f-b394-406c-94c4-c6331d1ca468
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 4fbb34d2daafc4a135cbf51c2b0382d3965f9dfc
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99160808"
---
# <a name="sqlmanagedatasources"></a>SQLManageDataSources
**Conformité**  
 Version introduite : ODBC 2,0  
  
 **Résumé**  
 **SQLManageDataSources** affiche une boîte de dialogue dans laquelle les utilisateurs peuvent configurer, ajouter et supprimer des sources de données dans les informations système.  
  
## <a name="syntax"></a>Syntaxe  
  
```cpp  
  
BOOL SQLManageDataSources(  
     HWND     hwnd);  
```  
  
## <a name="arguments"></a>Arguments  
 *HWND*  
 Entrée Handle de fenêtre parente.  
  
## <a name="returns"></a>Retours  
 **SQLManageDataSources** retourne la valeur false si *HWND* n’est pas un handle de fenêtre valide. Sinon, elle retourne TRUE.  
  
## <a name="diagnostics"></a>Diagnostics  
 Quand **SQLManageDataSources** retourne false, une valeur *\* pfErrorCode* associée peut être obtenue en appelant **SQLInstallerError**. Le tableau suivant répertorie les valeurs *\* pfErrorCode* qui peuvent être retournées par **SQLInstallerError** et les explique dans le contexte de cette fonction.  
  
|*\*pfErrorCode*|Erreur|Description|  
|---------------------|-----------|-----------------|  
|ODBC_ERROR_GENERAL_ERR|Erreur générale du programme d’installation|Une erreur s’est produite pour laquelle aucune erreur d’installation spécifique n’a été rencontrée.|  
|ODBC_ERROR_REQUEST_FAILED|Échec de la *requête*|L’appel à **ConfigDSN** a échoué.|  
|ODBC_ERROR_INVALID__HWND|Handle de fenêtre non valide|L’argument *HWND* n’était pas valide ou a la valeur null.|  
|ODBC_ERROR_OUT_OF_MEM|Mémoire insuffisante|Le programme d’installation n’a pas pu exécuter la fonction en raison d’un manque de mémoire.|  
  
## <a name="managing-data-sources"></a>Gestion des sources de données  
 **SQLManageDataSources** affiche initialement la boîte de dialogue administrateur de la **source de données ODBC** , comme indiqué dans l’illustration suivante.  
  
 ![Boîte de dialogue Administrateur de sources de données ODBC](../../../odbc/reference/syntax/media/ch23e.gif "CH23E")  
  
 La boîte de dialogue affiche les sources de données listées dans les informations système sous trois onglets : nom de source de données **utilisateur**, **nom de source** de données système et **fichier DSN**. Si l’utilisateur double-clique sur une source de données ou sélectionne une source de données et clique sur **configurer**, **SQLManageDataSources** appelle **ConfigDSN** dans la DLL d’installation avec l’option ODBC_CONFIG_DSN.  
  
 Si l’utilisateur clique sur **Ajouter**, **SQLManageDataSources** affiche la boîte de dialogue **créer une nouvelle source de données** , comme indiqué dans l’illustration suivante.  
  
 ![Boîte de dialogue Créer une nouvelle source de données](../../../odbc/reference/syntax/media/ch23f.gif "CH23F")  
  
 La boîte de dialogue affiche la liste des pilotes installés. Si l’utilisateur double-clique sur un pilote ou sélectionne un pilote et clique sur **OK**, **SQLManageDataSources** appelle **ConfigDSN** dans la dll d’installation et lui transmet l’option ODBC_ADD_DSN.  
  
 Si l’utilisateur sélectionne une source de données et clique sur **supprimer**, **SQLManageDataSources** vous demande si l’utilisateur souhaite supprimer la source de données. Si l’utilisateur clique sur **Oui**, **SQLManageDataSources** appelle **ConfigDSN** dans la DLL d’installation avec l’option ODBC_REMOVE_DSN.  
  
 La boîte de dialogue **créer une nouvelle source de données** est utilisée pour ajouter ou supprimer une source de données utilisateur, une source de données système ou une source de données de fichier.  
  
## <a name="user-dsns"></a>DSN utilisateur  
 Les DSN créés pour des utilisateurs individuels sont appelés DSN utilisateur pour les distinguer des sources de noms système. Les noms de sources de données utilisateur sont enregistrés comme suit dans les informations système :  
  
 `HKEY_CURRENT_USERS`  
  
 `SOFTWARE`  
  
 `ODBC`  
  
 `Odbc.ini`  
  
## <a name="system-dsns"></a>DSN système  
 La boîte de dialogue **créer une nouvelle source de données** vous permet d’ajouter une source de données système à votre ordinateur local ou de supprimer une source de données, ou de définir la configuration d’une source de données système.  
  
 Une source de données configurée avec un nom de source de données (DSN) système peut être utilisée par plusieurs utilisateurs sur le même ordinateur. Il peut également être utilisé par un service du système, qui peut alors accéder à la source de données même si aucun utilisateur n’a ouvert de session sur l’ordinateur.  
  
 Un nom de source de données système est inscrit dans l’entrée HKEY_LOCAL_MACHINE dans les informations système plutôt que dans l’entrée HKEY_CURRENT_USER. Elle n’est pas liée à un utilisateur qui se connecte avec son nom d’utilisateur et son mot de passe particuliers, mais elle peut être utilisée par n’importe quel utilisateur de cet ordinateur ou par un service de système automatique. Le DSN système est, toutefois, lié à un ordinateur. Il ne prend pas en charge la possibilité d’utiliser des noms d’accès distant entre les ordinateurs. Les noms de sources de données système sont enregistrés comme suit dans les informations système :  
  
 Odbc.ini ODBC DES LOGICIELS HKEY_LOCAL_MACHINE  
  
## <a name="file-dsns"></a>Fichiers DSN  
 Une source de données de fichier n’a pas de nom de source de données, comme la source de données d’une machine, et n’est pas inscrite auprès d’un utilisateur ou d’un ordinateur. Les informations de connexion pour cette source de données sont contenues dans un fichier. DSN qui peut être copié sur n’importe quel ordinateur. Une source de données de fichier peut être partagée, auquel cas le fichier. DSN réside sur un réseau et peut être utilisé simultanément par plusieurs utilisateurs sur le réseau tant que le pilote approprié est installé sur l’utilisateur. Il est également possible de ne pas partager une source de données de fichier, auquel cas elle peut être utilisée uniquement sur un seul ordinateur.  
  
 Pour plus d’informations sur les sources de données de fichier, consultez [connexion à l’aide de sources de données de fichier](../../../odbc/reference/develop-app/connecting-using-file-data-sources.md)ou voir [SQLDriverConnect](../../../odbc/reference/syntax/sqldriverconnect-function.md).  
  
## <a name="managing-drivers"></a>Gestion des pilotes  
 Si l’utilisateur clique sur l’onglet **pilotes** de la boîte de dialogue **administrateur de sources de données ODBC** , **SQLManageDataSources** affiche la liste des pilotes ODBC installés sur le système, ainsi que des informations sur les pilotes. La date affichée correspond à la date de création du pilote, comme indiqué dans l’illustration suivante.  
  
 ![Onglet Pilotes de la boîte de dialogue Administrateur de sources de données ODBC](../../../odbc/reference/syntax/media/ch23g.gif "ch23g")  
  
## <a name="tracing-options"></a>Options de suivi  
 Si l’utilisateur clique sur l’onglet **suivi** de la boîte de dialogue **administrateur de sources de données ODBC** , **SQLManageDataSources** affiche les options de suivi, comme indiqué dans l’illustration suivante.  
  
 ![Onglet Traçage de la boîte de dialogue Administrateur de sources de données ODBC](../../../odbc/reference/syntax/media/ch23h.gif "Ch23h")  
  
 Si l’utilisateur clique sur **Démarrer le suivi maintenant** et clique sur **OK**, **SQLManageDataSources** active le traçage manuellement pour toutes les applications en cours d’exécution sur l’ordinateur.  
  
 Si l’utilisateur spécifie le nom d’un fichier de trace dans la zone de texte **chemin d’accès du fichier journal** , puis clique sur **OK**, **SQLManageDataSources** définit le mot clé **tracefile** dans la section [ODBC] des informations système sur le nom spécifié.  
  
> [!IMPORTANT]  
>  La prise en charge de Visual Studio Analyzer a été supprimée depuis Windows 8 (Visual Studio Analyzer n’était inclus que dans les versions antérieures de Visual Studio). Pour un autre mécanisme de résolution des problèmes, utilisez le suivi des enchères.  
  
 Si l’utilisateur clique sur **démarrer Visual Studio Analyzer** puis clique sur **OK**, Visual Studio Analyzer est activé. Elle reste activée jusqu’à ce que vous cliquiez sur **arrêter le Visual Studio Analyzer** .  
  
 Pour plus d’informations sur le suivi, consultez [traçage](../../../odbc/reference/develop-app/tracing.md). Pour plus d’informations sur les mots clés **trace** et **tracefile** , consultez la rubrique relative à la [sous-clé ODBC](../../../odbc/reference/install/odbc-subkey.md).  
  
## <a name="related-functions"></a>Fonctions connexes  
  
|Pour obtenir des informations sur|Consultez|  
|---------------------------|---------|  
|Création de sources de données|[SQLCreateDataSource](../../../odbc/reference/syntax/sqlcreatedatasource-function.md)|
