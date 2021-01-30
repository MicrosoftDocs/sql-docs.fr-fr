---
description: SQLCreateDataSource, fonction
title: SQLCreateDataSource fonction) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLCreateDataSource
apilocation:
- sqlsrv32.dll
apitype: dllExport
f1_keywords:
- SQLCreateDataSource
helpviewer_keywords:
- SQLCreateDataSource function [ODBC]
ms.assetid: 76ee851a-dca9-40cc-8e9e-eb3f74e560ee
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: feca3e6c135e4dba441a20b1fa458f990b437fde
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99206103"
---
# <a name="sqlcreatedatasource-function"></a>SQLCreateDataSource, fonction
**Conformité**  
 Version introduite : ODBC 2,0  
  
 **Résumé**  
 **SQLCreateDataSource** affiche une boîte de dialogue avec laquelle l’utilisateur peut ajouter une source de données.  
  
## <a name="syntax"></a>Syntaxe  
  
```cpp  
  
BOOL SQLCreateDataSource(  
     HWND    hwnd,  
     LPSTR   lpszDS);  
```  
  
## <a name="arguments"></a>Arguments  
 *HWND*  
 Entrée Handle de fenêtre parente.  
  
 *lpszDS*  
 Entrée Nom de la source de données. *lpszDS* peut être un pointeur null ou une chaîne vide.  
  
## <a name="returns"></a>Retours  
 **SQLCreateDataSource** retourne la valeur true si la source de données est créée. Sinon, elle retourne FALSe.  
  
## <a name="diagnostics"></a>Diagnostics  
 Quand **SQLCreateDataSource** retourne false, une valeur *\* pfErrorCode* associée peut être obtenue en appelant **SQLInstallerError**. Le tableau suivant répertorie les valeurs *\* pfErrorCode* qui peuvent être retournées par **SQLInstallerError** et les explique dans le contexte de cette fonction.  
  
|*\*pfErrorCode*|Erreur|Description|  
|---------------------|-----------|-----------------|  
|ODBC_ERROR_GENERAL_ERR|Erreur générale du programme d’installation|Une erreur s’est produite pour laquelle aucune erreur d’installation spécifique n’a été rencontrée.|  
|ODBC_ERROR_INVALID_HWND|Handle de fenêtre non valide|L’argument *HWND* n’était pas valide ou a la valeur null.|  
|ODBC_ERROR_INVALID_DSN|DSN non valide|L’argument *lpszDS* contient une chaîne non valide pour un DSN.|  
|ODBC_ERROR_REQUEST_FAILED|Échec de la *requête*|Échec de l’appel à **ConfigDSN** avec l’option ODBC_ADD_DSN.|  
|ODBC_ERROR_LOAD_LIBRARY_FAILED|Impossible de charger la bibliothèque d’installation du pilote ou du convertisseur|Impossible de charger la bibliothèque d’installation du pilote.|  
|ODBC_ERROR_USER_CANCELED|Opération annulée par l’utilisateur|L’utilisateur a annulé la création d’une nouvelle source de données.|  
|ODBC_ERROR_CREATE_DSN_FAILED|Impossible de créer le DSN demandé|Impossible de se connecter à la base de données ; l’appel à **SQLDriverConnect** pour un fichier DSN n’a pas retourné une connexion réussie.<br /><br /> Impossible d’écrire dans le fichier.|  
|ODBC_ERROR_OUT_OF_MEM|Mémoire insuffisante|Le programme d’installation n’a pas pu exécuter la fonction en raison d’un manque de mémoire.|  
  
## <a name="comments"></a>Commentaires  
 Si *HWND* a la valeur null, **SQLCreateDataSource** retourne false. Dans le cas contraire, la boîte de dialogue **créer une nouvelle source de données** s’affiche avec une page de l’Assistant qui vous permet de choisir le type de source de données à configurer, comme indiqué dans l’illustration suivante.  
  
 ![Boîte de dialogue Créer une nouvelle source de données : sélection du type](../../../odbc/reference/syntax/media/ch23a.gif "CH23A")  
  
 L’option par défaut est **source de données de fichier**. Quand une source de données a été sélectionnée et que vous avez cliqué sur **suivant** , la page suivante de l’Assistant qui contient la liste des pilotes installés s’affiche.  
  
 ![Boîte de dialogue Créer une nouvelle source de données : sélection du pilote](../../../odbc/reference/syntax/media/ch23b.gif "CH23B")  
  
 Si vous cliquez sur **Annuler** , la boîte de dialogue disparaît et **SQLCreateDataSource** retourne la valeur false avec le code d’erreur de ODBC_ERROR_USER_CANCELED. Si l’option **source** de données utilisateur ou **source de données système** a été sélectionnée, le bouton **avancé** n’est pas disponible.  
  
 Lorsque vous cliquez sur le bouton **suivant** , l’un des éléments suivants se produit, selon le type de source de données sélectionné :  
  
-   Si vous avez sélectionné **source de données fichier** , une page de l’Assistant s’affiche pour que l’utilisateur entre un nom de fichier.  
  
-   Si une source de données **utilisateur** ou une **source de données système** a été sélectionnée, une page de l’Assistant affichant le type de la source de données et du pilote s’affiche pour la révision, et une fois que vous avez cliqué sur **Terminer** , la source de données est configurée.  
  
 Si vous cliquez sur **avancé** dans la page créer une nouvelle source de données de l’Assistant, une page de l’Assistant s’affiche pour que l’utilisateur entre des informations spécifiques au pilote. Dans la zone de texte de cette boîte de dialogue, tapez le pilote et les mots clés séparés par Returns, comme indiqué dans l’illustration suivante.  
  
 ![Boîte de dialogue Paramètres avancés de création de sources de données fichier](../../../odbc/reference/syntax/media/ch23c.gif "CH23C")  
  
 Vous trouverez des mots clés supplémentaires spécifiques au pilote sous la description de **SQLDriverConnect**. Toutes les **sources sauf DSN** sont autorisées.  
  
 La valeur par défaut de l’option **vérifier cette connexion** est true. Cette valeur par défaut s’applique si cette page de l’Assistant est activée ou non. Si l’utilisateur clique sur **OK** , la chaîne spécifiée dans la zone de texte et la valeur de l’option **vérifier cette connexion** sont mises en cache. (Si l’utilisateur clique sur le bouton **Fermer** ou sur **Annuler** , toutes les informations propres au pilote qui viennent d’être entrées sont perdues, car la chaîne spécifiée dans la zone de texte et la valeur de l’option **vérifier cette connexion** ne sont pas mises en cache.)  
  
 Si la **source de données fichier** a été sélectionnée dans la première page de l’Assistant, après qu’un pilote a été sélectionné et que les valeurs de mot clé ont été entrées dans la page avancé de l’Assistant, l’utilisateur est invité à entrer un nom de fichier. Cliquez sur **Parcourir** pour rechercher un nom de fichier. dans ce cas, le répertoire par défaut dans **la zone de recherche est** spécifié par une combinaison du chemin d’accès spécifié par CommonFileDir dans HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion et « ODBC\DataSources ». (Si CommonFileDir était « C:\Program Files\Common Files », le répertoire par défaut serait « C:\Program Files\Common Files\ODBC\Data Sources ».)  
  
 Quand un nom de fichier a été entré et que l’utilisateur clique sur **suivant** , le nom de fichier entré est vérifié par rapport aux règles de dénomination de fichier standard du système d’exploitation. Si le nom de fichier n’est pas valide, un message d’erreur indique à l’utilisateur qu’un nom de fichier non valide a été entré. Une fois que l’utilisateur a confirmé la boîte de message, le focus est renvoyé à la page de l’Assistant dans laquelle le nom de fichier est entré. Si le nom de fichier est valide, une page de l’Assistant qui affiche les paires mot clé-valeur sélectionnées s’affiche pour la révision, comme indiqué dans l’illustration suivante.  
  
 ![Boîte de dialogue Créer une nouvelle source de données : vérification](../../../odbc/reference/syntax/media/ch23d.gif "CH23D")  
  
 Si vous avez cliqué sur **Terminer** et que **source de données de fichier** a été sélectionné comme type de source de données, et si l’option **vérifier cette connexion** est vraie, **SQLDriverConnect** est appelé avec les mots clés **SaveFile** et **Driver** . L’argument *DriverCompletion* a la valeur SQL_DRIVER_COMPLETE. Le nom de fichier du mot clé **SaveFile** est le nom qui a été entré ou choisi, et le nom du pilote pour le mot clé **Driver** est le nom choisi. Si une chaîne de connexion spécifique au pilote a été spécifiée dans la page avancé de l’Assistant, cette chaîne est ajoutée après le mot clé **Driver** .  
  
 Si **SQLDriverConnect** retourne SQL_SUCCESS, le gestionnaire de pilotes a créé le nom de source de fichier. **SQLCreateDataSource** retourne la valeur true. Si **SQLDriverConnect** ne retourne pas SQL_SUCCESS, une boîte de message d’avertissement indique qu’une connexion n’a pas pu être établie à la source de données. Un nom de source de données avec des informations de connexion minimales peut encore être créé. Cette boîte de message permet à l’utilisateur d’annuler ou de poursuivre la création du fichier DSN.  
  
 Si l’utilisateur choisit de continuer à créer le DSN, ce processus continue comme si l’option **vérifier cette connexion** était définie sur false. Si l’utilisateur choisit d’annuler, la valeur FALSe est retournée pour **SQLCreateDataSource** avec le code d’erreur ODBC_ERROR_CREATE_DSN_FAILED.  
  
 Si la **source de données de fichier** a été sélectionnée comme type de source de données et que l’option **vérifier cette connexion** a la valeur FALSe, un DSN de fichier est créé avec le mot clé de **pilote** et la chaîne de connexion spécifiée par l’utilisateur (le cas échéant) à partir de la page avancé de l’Assistant. Si la création du fichier a réussi, la valeur TRUE est retournée pour **SQLCreateDataSource**. Si la création du fichier a échoué, une boîte de message d’erreur avertit l’utilisateur avec l’erreur renvoyée par le système d’exploitation. FALSe est retourné pour **SQLCreateDataSource** avec un code d’erreur de ODBC_ERROR_CREATE_DSN_FAILED. Pour plus d’informations sur les sources de données de fichier, consultez [connexion à l’aide de sources de données de fichier](../../../odbc/reference/develop-app/connecting-using-file-data-sources.md)ou voir [SQLDriverConnect](../../../odbc/reference/syntax/sqldriverconnect-function.md).  
  
 Si la **source de données** **utilisateur** ou système a été sélectionnée comme type de source de données, **ConfigDSN** dans la bibliothèque d’installation du pilote est appelé avec le ODBC_ADD_DSN *fRequest*. Pour plus d’informations, consultez [ConfigDSN](../../../odbc/reference/syntax/configdsn-function.md).  
  
## <a name="related-functions"></a>Fonctions connexes  
  
|Pour obtenir des informations sur|Consultez|  
|---------------------------|---------|  
|Gérer les sources de données|[SQLManageDataSources](../../../odbc/reference/syntax/sqlmanagedatasources.md)|
