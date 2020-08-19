---
description: SQLValidDSN, fonction
title: SQLValidDSN fonction) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
apiname:
- SQLValidDSN
apilocation:
- sqlsrv32.dll
apitype: dllExport
f1_keywords:
- SQLValidDSN
helpviewer_keywords:
- SQLValidDSN [ODBC]
ms.assetid: 930d1d89-337a-4429-85a2-84ee10555ac9
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 4919887a6e0bad4526959d0cd31205019a597a0f
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/17/2020
ms.locfileid: "88421023"
---
# <a name="sqlvaliddsn-function"></a>SQLValidDSN, fonction
**Conformité**  
 Version introduite : ODBC 2,0  
  
 **Résumé**  
 **SQLValidDSN** vérifie la longueur et la validité du nom de la source de données avant que le nom soit ajouté aux informations système.  
  
## <a name="syntax"></a>Syntaxe  
  
```cpp  
  
BOOL SQLValidDSN(  
     LPCSTR    lpszDSN);  
```  
  
## <a name="arguments"></a>Arguments  
 *lpszDSN*  
 Entrée Nom de la source de données à vérifier.  
  
## <a name="returns"></a>Retours  
 La fonction retourne la valeur TRUE si le nom de la source de données est valide. Elle retourne la valeur FALSe si le nom de la source de données n’est pas valide ou si l’appel de fonction a échoué.  
  
## <a name="diagnostics"></a>Diagnostics  
 Quand **SQLValidDSN** retourne false, une valeur * \* pfErrorCode* associée peut être obtenue en appelant **SQLInstallerError**. Un * \* pfErrorCode* est retourné uniquement si l’appel de fonction a échoué, et non si false a été retourné parce que le nom de la source de données n’est pas valide. Le tableau suivant répertorie les valeurs * \* pfErrorCode* qui peuvent être retournées par **SQLInstallerError** et les explique dans le contexte de cette fonction.  
  
|*\*pfErrorCode*|Error|Description|  
|---------------------|-----------|-----------------|  
|ODBC_ERROR_GENERAL_ERR|Erreur générale du programme d’installation|Une erreur s’est produite pour laquelle aucune erreur d’installation spécifique n’a été rencontrée.|  
|ODBC_ERROR_OUT_OF_MEM|Mémoire insuffisante|Le programme d’installation n’a pas pu exécuter la fonction en raison d’un manque de mémoire.|  
  
## <a name="comments"></a>Commentaires  
 **SQLValidDSN** est appelé par le [ConfigDSN](../../../odbc/reference/syntax/configdsn-function.md) d’un pilote pour vérifier la longueur du nom de la source de données et la validité des caractères individuels dans le nom de la source de données. Il vérifie si la longueur du nom est supérieure à SQL_MAX_DSN_LENGTH, comme défini dans Sqlext. h. (La longueur du nom de la source de données est également vérifiée par [SQLWriteDSNToIni](../../../odbc/reference/syntax/sqlwritedsntoini-function.md).) **SQLValidDSN** vérifie si l’un des caractères non valides suivants est inclus dans le nom de la source de données :  
  
 [ ] { } ( ) , ; ? * = ! \@ \  
  
## <a name="related-functions"></a>Fonctions connexes  
  
|Pour obtenir des informations sur|Consultez|  
|---------------------------|---------|  
|Ajout, modification ou suppression d’une source de données|[ConfigDSN](../../../odbc/reference/syntax/configdsn-function.md) (dans la dll d’installation)|  
|Ajout, modification ou suppression d’une source de données|[SQLConfigDataSource](../../../odbc/reference/syntax/sqlconfigdatasource-function.md)|  
|Écriture d’un nom de source de données dans les informations système|[SQLWriteDSNToIni](../../../odbc/reference/syntax/sqlwritedsntoini-function.md)|
