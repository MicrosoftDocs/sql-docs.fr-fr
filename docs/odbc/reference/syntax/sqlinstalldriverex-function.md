---
description: SQLInstallDriverEx, fonction
title: SQLInstallDriverEx fonction) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLInstallDriverEx
apilocation:
- sqlsrv32.dll
apitype: dllExport
f1_keywords:
- SQLInstallDriverEx
helpviewer_keywords:
- SQLInstallDriverEx function [ODBC]
ms.assetid: 1dd74544-f4e9-46e1-9b5f-c11d84fdab4c
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 3ebdfa732ba9c5a4a539009600749563daff4a35
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99189709"
---
# <a name="sqlinstalldriverex-function"></a>SQLInstallDriverEx, fonction
**Conformité**  
 Version introduite : ODBC 3,0  
  
 **Résumé**  
 **SQLInstallDriverEx** ajoute des informations sur le pilote à l’entrée Odbcinst.ini dans les informations système et incrémente de 1 le *UsageCount* du pilote. Toutefois, si une version du pilote existe déjà mais que la valeur *UsageCount* pour le pilote n’existe pas, la nouvelle valeur *UsageCount* est définie sur 2.  
  
 Cette fonction ne copie en fait pas de fichiers. Il incombe au programme appelant de copier correctement les fichiers du pilote dans le répertoire cible.  
  
 Vous pouvez également accéder aux fonctionnalités de **SQLInstallDriverEx** avec [ODBCCONF.EXE](../../../odbc/odbcconf-exe.md).  
  
## <a name="syntax"></a>Syntaxe  
  
```cpp  
  
BOOL SQLInstallDriverEx(  
     LPCSTR    lpszDriver,  
     LPCSTR    lpszPathIn,  
     LPSTR     lpszPathOut,  
     WORD      cbPathOutMax,  
     WORD *    pcbPathOut,  
     WORD      fRequest,  
     LPDWORD   lpdwUsageCount);  
```  
  
## <a name="arguments"></a>Arguments  
 *lpszDriver*  
 Entrée Description du pilote (généralement le nom du SGBD associé) présentée aux utilisateurs au lieu du nom du pilote physique. L’argument *lpszDriver* doit contenir une liste de paires mot clé-valeur se terminant par un caractère null, qui décrivent le pilote. Pour plus d’informations sur les paires mot clé-valeur, consultez [sous-clés de spécification de pilote](../../../odbc/reference/install/driver-specification-subkeys.md). Pour plus d’informations sur la chaîne double terminée par un caractère null, consultez [fonction ConfigDSN](../../../odbc/reference/syntax/configdsn-function.md).  
  
 *lpszPathIn*  
 Entrée Chemin d’accès complet du répertoire cible de l’installation ou pointeur null. Si *lpszPathIn* est un pointeur null, les pilotes sont installés dans le répertoire système.  
  
 *lpszPathOut*  
 Sortie Chemin d’accès du répertoire cible où le pilote doit être installé. Si le pilote n’a pas encore été installé, *lpszPathOut* doit être identique à *lpszPathIn*. Si le pilote a été installé précédemment, *lpszPathOut* est le chemin d’accès de l’installation précédente.  
  
 *cbPathOutMax*  
 Entrée Longueur de *lpszPathOut*.  
  
 *pcbPathOut*  
 Sortie Nombre total d’octets (à l’exclusion du caractère de fin null) pouvant être renvoyés dans *lpszPathOut*. Si le nombre d’octets disponibles à retourner est supérieur ou égal à *cbPathOutMax*, le chemin de sortie dans *lpszPathOut* est tronqué à *cbPathOutMax* moins le caractère de fin null. L’argument *pcbPathOut* peut être un pointeur null.  
  
 *fRequest*  
 Entrée Type de requête. L’argument *fRequest* doit contenir l’une des valeurs suivantes :  
  
 ODBC_INSTALL_INQUIRY : pour savoir où un pilote peut être installé.  
  
 ODBC_INSTALL_COMPLETE : terminez la demande d’installation.  
  
 *lpdwUsageCount*  
 Sortie Le nombre d’utilisations du pilote après l’appel de cette fonction.  
  
 Les applications ne doivent pas définir le nombre d’utilisations. ODBC conservera ce nombre.  
  
## <a name="returns"></a>Retours  
 La fonction retourne TRUE si elle réussit, FALSe en cas d’échec.  
  
## <a name="diagnostics"></a>Diagnostics  
 Quand **SQLInstallDriverEx** retourne false, une valeur *\* pfErrorCode* associée peut être obtenue en appelant **SQLInstallerError**. Le tableau suivant répertorie les valeurs *\* pfErrorCode* qui peuvent être retournées par **SQLInstallerError** et les explique dans le contexte de cette fonction.  
  
|*\*pfErrorCode*|Erreur|Description|  
|---------------------|-----------|-----------------|  
|ODBC_ERROR_GENERAL_ERR|Erreur générale du programme d’installation|Une erreur s’est produite pour laquelle aucune erreur d’installation spécifique n’a été rencontrée.|  
|ODBC_ERROR_INVALID_BUFF_LEN|Longueur de la mémoire tampon non valide|L’argument *lpszPathOut* n’était pas suffisamment grand pour contenir le chemin de sortie. La mémoire tampon contient le chemin d’accès tronqué.<br /><br /> L’argument *cbPathOutMax* était 0 et *fRequest* était ODBC_INSTALL_COMPLETE.|  
|ODBC_ERROR_INVALID_REQUEST_TYPE|Type de demande non valide|L’argument *fRequest* ne faisait pas partie des éléments suivants :<br /><br /> ODBC_INSTALL_INQUIRY ODBC_INSTALL_COMPLETE|  
|ODBC_ERROR_INVALID_KEYWORD_VALUE|Paires mot clé/valeur non valides|L’argument *lpszDriver* contient une erreur de syntaxe.|  
|ODBC_ERROR_INVALID_PATH|Chemin d’installation non valide|L’argument *lpszPathIn* contient un chemin d’accès non valide.|  
|ODBC_ERROR_LOAD_LIBRARY_FAILED|Impossible de charger la bibliothèque d’installation du pilote ou du convertisseur|Impossible de charger la bibliothèque d’installation du pilote.|  
|ODBC_ERROR_INVALID_PARAM_SEQUENCE|Séquence de paramètres non valide|L’argument *lpszDriver* ne contient pas de liste de paires mot clé-valeur.|  
|ODBC_ERROR_USAGE_UPDATE_FAILED|Impossible d’incrémenter ou de décrémenter le nombre d’utilisations du composant|Le programme d’installation n’a pas pu incrémenter le nombre d’utilisations du pilote.|  
  
## <a name="comments"></a>Commentaires  
 L’argument *lpszDriver* est une liste d’attributs sous la forme de paires mot clé-valeur. Chaque paire se termine par un octet NULL, et la liste entière se termine par un octet NULL. (Autrement dit, deux octets de valeur null marquent la fin de la liste.) Le format de cette liste est le suivant :  
  
 _pilote-DESC_ **\\** **=**_pilote 0DRIVER-dll-nom de fichier_ **\\** 0 [programme d’installation **=** _-dll-nom de fichier_ <b>\\</b> 0]  
  
 [_Driver-attr-keyword1_ **=** _valeur1_ <b>\\</b> 0] [_Driver-attr-keyword2_ **=** _value2_ <b>\\</b> 0]... <b>\\</b> entre  
  
 où \ 0 est un octet NULL et *Driver-attr-Keyword* est un mot clé d’attribut Driver. Les mots clés doivent apparaître dans l’ordre spécifié. Par exemple, supposons qu’un pilote pour les fichiers texte mis en forme possède des dll de pilote et d’installation distinctes et peut utiliser des fichiers avec les extensions. txt et. csv. L’argument *lpszDriver* pour ce pilote peut être le suivant :  
  
```  
Text\0Driver=TEXT.DLL\0Setup=TXTSETUP.DLL\0FileUsage=1\0  
FileExtns=*.txt,*.csv\0\0  
```  
  
 Supposons qu’un pilote pour SQL Server n’a pas de DLL d’installation distincte et n’a pas de mot clé d’attribut de pilote. L’argument *lpszDriver* pour ce pilote peut être le suivant :  
  
```  
SQL Server\0Driver=SQLSRVR.DLL\0\0  
```  
  
 Une fois que **SQLInstallDriverEx** a récupéré des informations sur le pilote à partir de l’argument *lpszDriver* , il ajoute la description du pilote à la section [ODBC Drivers] de l’entrée Odbcinst.ini dans les informations système. Il crée ensuite une section intitulée avec la description du pilote et ajoute les chemins d’accès complets de la DLL du pilote et la DLL d’installation. Enfin, elle retourne le chemin d’accès du répertoire cible de l’installation, mais ne copie pas les fichiers du pilote vers celle-ci. Le programme appelant doit réellement copier les fichiers de pilote dans le répertoire cible.  
  
 **SQLInstallDriverEx** incrémente de 1 le nombre d’utilisations des composants pour le pilote installé. Si une version du pilote existe déjà mais que le nombre d’utilisations des composants pour le pilote n’existe pas, la valeur du nouveau nombre d’utilisations du composant est définie sur 2.  
  
 Le programme d’installation de l’application est responsable de la copie physique du fichier de pilote et de la gestion du nombre d’utilisations de fichiers. Si le fichier de pilote n’a pas déjà été installé, le programme d’installation de l’application doit copier le fichier dans le chemin d’accès *lpszPathIn* et créer le nombre d’utilisations de fichiers. Si le fichier a déjà été installé, le programme d’installation incrémente simplement le nombre d’utilisations de fichiers et retourne le chemin d’accès de l’installation antérieure dans l’argument *lpszPathOut* .  
  
> [!NOTE]  
>  Pour plus d’informations sur le nombre d’utilisations des composants et le nombre d’utilisations des fichiers, consultez [comptage de l’utilisation](../../../odbc/reference/install/usage-counting.md).  
  
 Si une version antérieure du fichier de pilote était précédemment installée par l’application, le pilote doit être désinstallé, puis réinstallé, afin que le nombre d’utilisations du composant de pilote soit valide. **SQLConfigDriver** (avec un *fRequest* de ODBC_REMOVE_DRIVER) doit d’abord être appelé, puis **SQLRemoveDriver** doit être appelé pour décrémenter le nombre d’utilisations du composant. **SQLInstallDriverEx** doit ensuite être appelé pour réinstaller le pilote, ce qui incrémente le nombre d’utilisations des composants. Le programme d’installation de l’application doit remplacer l’ancien fichier par le nouveau fichier. Le nombre d’utilisations de fichiers reste le même et toute autre application qui utilisait le fichier de version antérieure utilisera désormais la version plus récente.  
  
> [!NOTE]  
>  Si le pilote a été précédemment installé et que **SQLInstallDriverEx** est appelé pour installer le pilote dans un répertoire différent, la fonction retourne la valeur true, mais *lpszPathOut* inclut le répertoire dans lequel le pilote a déjà été installé. Elle n’inclut pas le répertoire entré dans l’argument *lpszDriver* .  
  
 La longueur du chemin d’accès dans *lpszPathOut* dans **SQLInstallDriverEx** permet un processus d’installation en deux phases, de sorte qu’une application peut déterminer ce que *CbPathOutMax* doit être en appelant **SQLInstallDriverEx** avec un *fRequest* du mode ODBC_INSTALL_INQUIRY. Cette opération renvoie le nombre total d’octets disponibles dans la mémoire tampon *pcbPathOut* . **SQLInstallDriverEx** peut ensuite être appelé avec un *fRequest* de ODBC_INSTALL_COMPLETE et l’argument *cbPathOutMax* défini sur la valeur de la mémoire tampon *pcbPathOut* , plus le caractère de fin null.  
  
 Si vous choisissez de ne pas utiliser le modèle en deux phases pour **SQLInstallDriverEx**, vous devez définir *cbPathOutMax*, qui définit la taille du stockage pour le chemin d’accès du répertoire cible, sur la valeur _MAX_PATH, comme défini dans stdlib. h, pour empêcher la troncation.  
  
 Lorsque *fRequest* est ODBC_INSTALL_COMPLETE, **SQLInstallDriverEx** n’autorise pas la valeur de *lpszPathOut* null (ou *cbPathOutMax* à 0). Si *fRequest* est ODBC_INSTALL_COMPLETE, false est retourné lorsque le nombre d’octets disponibles à retourner est supérieur ou égal à *cbPathOutMax*, avec pour résultat que la troncation se produit.  
  
 Une fois que **SQLInstallDriverEx** a été appelé et que le programme d’installation de l’application a copié le fichier de pilote (si nécessaire), la dll d’installation du pilote doit appeler **SQLConfigDriver** pour définir la configuration du pilote.  
  
## <a name="related-functions"></a>Fonctions connexes  
  
|Pour obtenir des informations sur|Consultez|  
|---------------------------|---------|  
|Installation du Gestionnaire de pilotes|[SQLInstallDriverManager](../../../odbc/reference/syntax/sqlinstalldrivermanager-function.md)|
