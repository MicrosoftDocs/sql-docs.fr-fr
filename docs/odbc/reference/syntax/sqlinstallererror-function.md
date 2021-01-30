---
description: SQLInstallerError, fonction
title: SQLInstallerError fonction) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLInstallerError
apilocation:
- sqlsrv32.dll
apitype: dllExport
f1_keywords:
- SQLInstallerError
helpviewer_keywords:
- SQLInstallerError [ODBC]
ms.assetid: e6474b79-4d55-458f-81ce-abfafe357f83
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: dcafadc902504ddfd32fa8b7369780bd5c296a0d
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99189673"
---
# <a name="sqlinstallererror-function"></a>SQLInstallerError, fonction
**Conformité**  
 Version introduite : ODBC 3,0  
  
 **Résumé**  
 **SQLInstallerError** retourne des informations d’erreur ou d’État pour les fonctions du programme d’installation ODBC.  
  
## <a name="syntax"></a>Syntaxe  
  
```cpp  
  
RETCODE SQLInstallerError(  
     WORD      iError,  
     DWORD *   pfErrorCode,  
     LPSTR     lpszErrorMsg,  
     WORD      cbErrorMsgMax,  
     WORD *    pcbErrorMsg);  
```  
  
## <a name="arguments"></a>Arguments  
 *iError*  
 Entrée Numéro d’enregistrement d’erreur. Les nombres valides sont compris entre 1 et 8.  
  
 *pfErrorCode*  
 Sortie Code d’erreur du programme d’installation. (Pour plus d’informations, consultez « commentaires ».)  
  
 *lpszErrorMsg*  
 Sortie Pointeur vers le stockage pour le texte du message d’erreur.  
  
 *cbErrorMsgMax*  
 Entrée Longueur maximale de la mémoire tampon *szErrorMsg* . Celui-ci doit être inférieur ou égal à SQL_MAX_MESSAGE_LENGTH moins le caractère de fin null.  
  
 *cbErrorMsgMax*  
 Entrée Longueur maximale de la mémoire tampon *szErrorMsg* . Celui-ci doit être inférieur ou égal à SQL_MAX_MESSAGE_LENGTH moins le caractère de fin null.  
  
 *pcbErrorMsg*  
 Sortie Pointeur vers le nombre total d’octets (à l’exclusion du caractère de fin null) pouvant être retourné dans *lpszErrorMsg*. Si le nombre d’octets disponibles à retourner est supérieur ou égal à *cbErrorMsgMax*, le texte du message d’erreur dans *lpszErrorMsg* est tronqué à *cbErrorMsgMax* moins les octets de fin de caractère null. L’argument *pcbErrorMsg* peut être un pointeur null.  
  
## <a name="returns"></a>Retours  
 SQL_SUCCESS, SQL_SUCCESS_WITH_INFO, SQL_NO_DATA ou SQL_ERROR.  
  
## <a name="diagnostics"></a>Diagnostics  
 **SQLInstallerError** ne publie pas de valeurs d’erreur pour lui-même. **SQLInstallerError** retourne SQL_NO_DATA lorsqu’il ne peut pas récupérer d’informations sur l’erreur (auquel cas *pfErrorCode* n’est pas défini). Si **SQLInstallerError** ne peut pas accéder aux valeurs d’erreur pour une raison qui retournerait normalement SQL_ERROR, **SQLInstallerError** retourne SQL_ERROR mais ne publie pas de valeurs d’erreur. Si vous ne connaissez pas la longueur de la chaîne d’avertissement (*lpszErrorMsg*), vous pouvez affecter à *lpszErrorMsg* la valeur null et appeler **SQLInstallerError**. **SQLInstallerError** retourne alors la longueur de la chaîne d’avertissement dans *cbErrorMsgMax*. Si la mémoire tampon du message d’erreur est trop petite, **SQLInstallerError** retourne SQL_SUCCESS_WITH_INFO et retourne la valeur *PfErrorCode* correcte pour **SQLInstallerError**.  
  
 Pour déterminer si une troncation s’est produite dans le message d’erreur, une application peut comparer la valeur de l’argument *cbErrorMsgMax* à la longueur réelle du texte du message écrit dans l’argument *pcbErrorMsg* . Si la troncation se produit, la longueur de mémoire tampon correcte doit être allouée pour *lpszErrorMsg* et **SQLInstallerError** doit être appelé à nouveau avec l’enregistrement *iError* correspondant.  
  
## <a name="comments"></a>Commentaires  
 Une application appelle **SQLInstallerError** lorsqu’un appel précédent à la fonction du programme d’installation ODBC retourne la valeur false. Les fonctions du programme d’installation ODBC et du pilote ou du convertisseur publient zéro ou plusieurs erreurs uniquement lorsque la fonction échoue (retourne FALSe); par conséquent, une application appelle **SQLInstallerError** uniquement après l’échec d’une fonction de programme d’installation ODBC.  
  
 La file d’attente des erreurs du programme d’installation ODBC est vidée chaque fois qu’une nouvelle fonction d’installation est appelée. Par conséquent, une application ne peut pas s’attendre à récupérer des erreurs pour les fonctions autres que celles du dernier appel de la fonction du programme d’installation.  
  
 Pour récupérer plusieurs erreurs pour un appel de fonction, une application appelle **SQLInstallerError** plusieurs fois.  
  
 Lorsqu’il n’y a pas d’informations supplémentaires, **SQLInstallerError** retourne SQL_NO_DATA, l’argument *pfErrorCode* n’est pas défini, l’argument *pcbErrorMsg* est égal à 0 et l’argument *lpszErrorMsg* contient un caractère de fin null unique (sauf si l’argument *cbErrorMsgMax* est égal à 0).
