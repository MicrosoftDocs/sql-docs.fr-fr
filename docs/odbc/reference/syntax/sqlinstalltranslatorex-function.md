---
description: SQLInstallTranslatorEx, fonction
title: SQLInstallTranslatorEx fonction) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLInstallTranslatorEx
apilocation:
- sqlsrv32.dll
apitype: dllExport
f1_keywords:
- SQLInstallTranslatorEx
helpviewer_keywords:
- SQLInstallTranslatorEx function [ODBC]
ms.assetid: a0630602-53c1-4db0-98ce-70d160aedf8d
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: e055f3f3565c6240134bf43b4b4f8951059e2725
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99189649"
---
# <a name="sqlinstalltranslatorex-function"></a>SQLInstallTranslatorEx, fonction
**Conformité**  
 Version introduite : ODBC 3,0  
  
 **Résumé**  
 **SQLInstallTranslatorEx** ajoute des informations à propos d’un traducteur à la section Odbcinst.ini de la clé de Registre HKEY_LOCAL_MACHINE\SOFTWARE\ODBC\ODBCINST.INI \ODBC Translators.  
  
 Vous pouvez également accéder aux fonctionnalités de **SQLInstallTranslatorEx** avec [ODBCCONF.EXE](../../../odbc/odbcconf-exe.md).  
  
## <a name="syntax"></a>Syntaxe  
  
```cpp  
  
BOOL SQLInstallTranslatorEx(  
     LPCSTR    lpszTranslator,  
     LPCSTR    lpszPathIn,  
     LPSTR     lpszPathOut,  
     WORD      cbPathOutMax,  
     WORD *    pcbPathOut,  
     WORD      fRequest,  
     LPDWORD   lpdwUsageCount);  
```  
  
## <a name="arguments"></a>Arguments  
 *lpszTranslator*  
 Entrée Il doit contenir une liste de paires mot clé-valeur se terminant par un caractère null, qui décrivent le traducteur. Pour plus d’informations sur la syntaxe des paires mot clé-valeur, consultez les [sous-clés de spécification du traducteur](../../../odbc/reference/install/translator-specification-subkeys.md).  
  
 Les mots clés **Translator** et **Setup** doivent être inclus dans la chaîne *lpszTranslator* . La DLL de traduction est listée avec le mot clé **Translator** et la dll d’installation du traducteur est listée avec le mot clé **Setup** . Chaque paire se termine par un octet NULL, et la liste entière se termine par un octet NULL. (Autrement dit, deux octets de valeur NULL marquent la fin de la liste.) Le format de *lpszTranslator* est le suivant :  
  
 \0Translator =*translateur-dll-filename*\ 0 [Setup =*Setup-dll-filename*\ 0] \ 0  
  
 *lpszPathIn*  
 Entrée Chemin d’accès complet de l’emplacement où le convertisseur doit être installé ou un pointeur null. Si *lpszPath* est un pointeur null, les traducteurs sont installés dans le répertoire système.  
  
 *lpszPathOut*  
 Sortie Chemin d’accès du répertoire cible où le convertisseur doit être installé. Si le convertisseur n’a jamais été installé, *lpszPathOut* est identique à *lpszPathIn*. S’il existe une installation antérieure du traducteur, *lpszPathOut* est le chemin d’accès de l’installation précédente.  
  
 *cbPathOutMax*  
 Entrée Longueur de *lpszPathOut.*  
  
 *pcbPathOut*  
 Sortie Nombre total d’octets disponibles à retourner dans *lpszPathOut*. Si le nombre d’octets disponibles à retourner est supérieur ou égal à *cbPathOutMax*, le chemin de sortie dans *lpszPathOut* est tronqué à *pcbPathOutMax* moins le caractère de fin null. L’argument *pcbPathOut* peut être un pointeur null.  
  
 *fRequest*  
 Entrée Type de requête. *fRequest* doit contenir l’une des valeurs suivantes :  
  
 ODBC_INSTALL_INQUIRY : vous renseignez sur l’emplacement d’installation d’un convertisseur.  
  
 ODBC_INSTALL_COMPLETE : terminez la demande d’installation.  
  
 *lpdwUsageCount*  
 Sortie Nombre d’utilisations du traducteur après l’appel de cette fonction.  
  
 Les applications ne doivent pas définir le nombre d’utilisations. ODBC conservera ce nombre.  
  
## <a name="returns"></a>Retours  
 La fonction retourne TRUE si elle réussit, FALSe en cas d’échec.  
  
## <a name="diagnostics"></a>Diagnostics  
 Quand **SQLInstallTranslatorEx** retourne false, une valeur *\* pfErrorCode* associée peut être obtenue en appelant **SQLInstallerError**. Le tableau suivant répertorie les valeurs *\* pfErrorCode* qui peuvent être retournées par **SQLInstallerError** et les explique dans le contexte de cette fonction.  
  
|*\*pfErrorCode*|Erreur|Description|  
|---------------------|-----------|-----------------|  
|ODBC_ERROR_GENERAL_ERR|Erreur générale du programme d’installation|Une erreur s’est produite pour laquelle aucune erreur d’installation spécifique n’a été rencontrée.|  
|ODBC_ERROR_INVALID_BUFF_LEN|Longueur de la mémoire tampon non valide|L’argument *lpszPathOut* n’était pas suffisamment grand pour contenir le chemin de sortie. La mémoire tampon contient le chemin d’accès tronqué.<br /><br /> L’argument *cbPathOutMax* était 0 et l’argument *fRequest* était ODBC_INSTALL_COMPLETE.|  
|ODBC_ERROR_INVALID_REQUEST_TYPE|Type de demande non valide|L’argument *fRequest* ne faisait pas partie des éléments suivants :<br /><br /> ODBC_INSTALL_INQUIRY ODBC_INSTALL_COMPLETE|  
|ODBC_ERROR_INVALID_KEYWORD_VALUE|Paires mot clé/valeur non valides|L’argument *lpszTranslator* contient une erreur de syntaxe.|  
|ODBC_ERROR_INVALID_PATH|Chemin d’installation non valide|L’argument *lpszPathIn* contient un chemin d’accès non valide.|  
|ODBC_ERROR_INVALID_PARAM_SEQUENCE|Séquence de paramètres non valide|L’argument *lpszTranslator* ne contient pas de liste de paires mot clé-valeur.|  
|ODBC_ERROR_USAGE_UPDATE_FAILED|Impossible d’incrémenter ou de décrémenter le nombre d’utilisations des composants du Registre|Le programme d’installation n’a pas pu incrémenter le nombre d’utilisations du traducteur.|  
  
## <a name="comments"></a>Commentaires  
 **SQLInstallTranslatorEx** fournit un mécanisme permettant d’installer uniquement le traducteur. Cette fonction ne copie en fait pas de fichiers. Le programme appelant est chargé de copier les fichiers de traducteur.  
  
 **SQLInstallTranslatorEx** incrémente de 1 le nombre d’utilisations du composant pour le traducteur installé. Si une version du traducteur existe déjà mais que le nombre d’utilisations du composant pour le traducteur n’existe pas, la valeur du nouveau nombre d’utilisations du composant est définie sur 2.  
  
 Le programme d’installation de l’application est chargé de copier physiquement le fichier du traducteur et de conserver le nombre d’utilisations des fichiers. Si le fichier de traduction n’a pas encore été installé, le programme d’installation de l’application doit copier le ou les fichiers et créer le nombre d’utilisations du fichier ou des fichiers. Si le fichier a déjà été installé, le programme d’installation incrémente simplement le nombre d’utilisations de fichiers.  
  
 Si une version antérieure du convertisseur était précédemment installée par l’application, le convertisseur doit être désinstallé, puis réinstallé, afin que le nombre d’utilisations du composant de traduction soit valide. **SQLRemoveTranslator** doit être appelé pour décrémenter le nombre d’utilisations du composant, puis **SQLInstallTranslatorEx** doit être appelé pour incrémenter le nombre d’utilisations du composant. Le programme d’installation de l’application doit remplacer les anciens fichiers ou fichiers par le nouveau fichier. Le nombre d’utilisations de fichiers reste le même, et les autres applications qui utilisaient le fichier de version antérieure utiliseront désormais la version plus récente.  
  
 La longueur du chemin d’accès dans *lpszPathOut* dans **SQLInstallTranslatorEx** permet un processus d’installation en deux phases, de sorte qu’une application peut déterminer ce que *CbPathOutMax* doit être en appelant **SQLInstallTranslatorEx** avec un *fRequest* du mode ODBC_INSTALL_INQUIRY. Cette opération renvoie le nombre total d’octets disponibles dans la mémoire tampon *pcbPathOut* . **SQLInstallTranslatorEx** peut ensuite être appelé avec un *fRequest* de ODBC_INSTALL_COMPLETE et l’argument *cbPathOutMax* défini sur la valeur de la mémoire tampon *pcbPathOut* , plus le caractère de fin null.  
  
 Si vous choisissez de ne pas utiliser le modèle en deux phases pour **SQLInstallTranslatorEx**, vous devez définir *cbPathOutMax*, qui définit la taille du stockage pour le chemin d’accès du répertoire cible, sur la valeur _MAX_PATH, comme défini dans stdlib. h, pour empêcher la troncation.  
  
 Lorsque *fRequest* est ODBC_INSTALL_COMPLETE, **SQLInstallTranslatorEx** n’autorise pas la valeur de *lpszPathOut* null (ou *cbPathOutMax* à 0). Si *fRequest* est ODBC_INSTALL_COMPLETE, false est retourné lorsque le nombre d’octets disponibles à retourner est supérieur ou égal à *cbPathOutMax*, avec pour résultat que la troncation se produit.  
  
## <a name="related-functions"></a>Fonctions connexes  
  
|Pour obtenir des informations sur|Consultez|  
|---------------------------|---------|  
|Retour d’une option de traduction par défaut|[ConfigTranslator](../../../odbc/reference/syntax/configtranslator-function.md)|  
|Sélection de traducteurs|[SQLGetTranslator](../../../odbc/reference/syntax/sqlgettranslator-function.md)|  
|Suppression de convertisseurs|[SQLRemoveTranslator](../../../odbc/reference/syntax/sqlremovetranslator-function.md)|
