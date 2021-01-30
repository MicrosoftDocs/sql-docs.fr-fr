---
description: SQLDriverToDataSource, fonction
title: SQLDriverToDataSource fonction) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLDriverToDataSource
apilocation:
- sqlsrv32.dll
apitype: dllExport
f1_keywords:
- SQLDriverToDataSource
helpviewer_keywords:
- SQLDriverToDataSource function [ODBC]
ms.assetid: 0de28eb5-8aa9-43e4-a87f-7dbcafe800dc
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 2ea0697dcedcf27992e771382b47a4b6713b1535
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99211905"
---
# <a name="sqldrivertodatasource-function"></a>SQLDriverToDataSource, fonction
**SQLDriverToDataSource** prend en charge les traductions pour les pilotes ODBC. Cette fonction n’est pas appelée par les applications compatibles ODBC ; les applications demandent une traduction par le biais de **SQLSetConnectAttr**. Le pilote associé au *ConnectionHandle* spécifié dans **SQLSETCONNECTATTR** appelle la DLL spécifiée pour effectuer des traductions de toutes les données circulant du pilote à la source de données. Une DLL de traduction par défaut peut être spécifiée dans le fichier d’initialisation ODBC.  
  
## <a name="syntax"></a>Syntaxe  
  
```cpp  
  
BOOL SQLDriverToDataSource(  
     UDWORD     fOption,  
     SWORD      fSqlType,  
     PTR        rgbValueIn,  
     SDWORD     cbValueIn,  
     PTR        rgbValueOut,  
     SDWORD     cbValueOutMax,  
     SDWORD *   pcbValueOut,  
     UCHAR *    szErrorMsg,  
     SWORD      cbErrorMsgMax,  
     SWORD *    pcbErrorMsg);  
```  
  
## <a name="arguments"></a>Arguments  
 *fOption*  
 Entrée Valeur de l’option.  
  
 *fSqlType*  
 Entrée Type de données SQL ODBC. Cet argument indique au pilote comment convertir des *rgbValueIn* dans un format acceptable par la source de données. Pour obtenir la liste des types de données SQL valides, consultez [types de données](../../../odbc/reference/appendixes/sql-data-types.md)SQL.  
  
 *rgbValueIn*  
 Entrée Valeur à convertir.  
  
 *cbValueIn*  
 Entrée Longueur de *rgbValueIn*.  
  
 *rgbValueOut*  
 Sortie Résultat de la traduction.  
  
> [!NOTE]  
>  La DLL de traduction ne termine pas cette valeur par un caractère null.  
  
 *cbValueOutMax*  
 Entrée Longueur de *rgbValueOut*.  
  
 *pcbValueOut*  
 Sortie Nombre total d’octets (à l’exclusion de l’octet de fin null) disponibles pour le retour dans *rgbValueOut*.  
  
 Pour les données de type caractère ou binaire, si cette valeur est supérieure ou égale à *cbValueOutMax*, les données de *rgbValueOut* sont tronquées à *cbValueOutMax* octets.  
  
 Pour tous les autres types de données, la valeur de *cbValueOutMax* est ignorée et la dll de traduction suppose que la taille de *rgbValueOut* est la taille du type de données C par défaut du type de données SQL spécifié avec *fSqlType*.  
  
 L’argument *pcbValueOut* peut être un pointeur null.  
  
 *szErrorMsg*  
 Sortie Pointeur vers le stockage pour un message d’erreur. Il s’agit d’une chaîne vide, sauf si la conversion a échoué.  
  
 *cbErrorMsgMax*  
 Entrée Longueur de *szErrorMsg*.  
  
 *pcbErrorMsg*  
 Sortie Pointeur vers le nombre total d’octets (à l’exclusion de l’octet de fin null) disponibles à retourner dans *szErrorMsg*. Si cette valeur est supérieure ou égale à *cbErrorMsg*, les données de *szErrorMsg* sont tronquées à *cbErrorMsgMax* moins le caractère de fin null. L’argument *pcbErrorMsg* peut être un pointeur null.  
  
## <a name="returns"></a>Retours  
 TRUE si la conversion a réussi, FALSe en cas d’échec de la traduction.  
  
## <a name="comments"></a>Commentaires  
 Le pilote appelle **SQLDriverToDataSource** pour convertir toutes les données (instructions SQL, paramètres, etc.) passant du pilote à la source de données. La DLL de traduction peut ne pas traduire certaines données, selon le type de données et l’objectif de la DLL de traduction. Par exemple, une DLL qui traduit les données de type caractère d’une page de codes en une autre ignore toutes les données numériques et binaires.  
  
 La valeur de *fOption* est définie sur la valeur de *vParam* spécifiée en appelant **SQLSetConnectAttr** avec l’attribut SQL_ATTR_TRANSLATE_OPTION. Il s’agit d’une valeur 32 bits qui a une signification spécifique pour une DLL de traduction donnée. Par exemple, il peut spécifier une traduction de jeu de caractères spécifique.  
  
 Si la même mémoire tampon est spécifiée pour *rgbValueIn* et *rgbValueOut*, la traduction des données dans la mémoire tampon est effectuée sur place.  
  
 Bien que *cbValueIn*, *cbValueOutMax* et *pcbValueOut* soient du type SDWORD, **SQLDriverToDataSource** ne prend pas nécessairement en charge les pointeurs énormes.  
  
 Si **SQLDriverToDataSource** retourne la valeur false, la troncation des données s’est peut-être produite lors de la traduction. Si *pcbValueOut* (nombre d’octets disponibles à retourner dans la mémoire tampon de sortie) est supérieur à *cbValueOutMax* (longueur de la mémoire tampon de sortie), la troncation s’est produite. Le pilote doit déterminer si la troncation est acceptable ou non. Si la troncation n’a pas eu lieu, **SQLDriverToDataSource** a retourné false en raison d’une autre erreur. Dans les deux cas, un message d’erreur spécifique est retourné dans *szErrorMsg*.  
  
 Pour plus d’informations sur la traduction de données, consultez [dll de traduction](../../../odbc/reference/develop-app/translation-dlls.md).  
  
## <a name="related-functions"></a>Fonctions connexes  
  
|Pour obtenir des informations sur|Consultez|  
|---------------------------|---------|  
|Traduction des données retournées à partir de la source de données|[SQLDataSourceToDriver](../../../odbc/reference/syntax/sqldatasourcetodriver-function.md)|  
|Renvoi du paramètre d’un attribut de connexion|[SQLGetConnectAttr](../../../odbc/reference/syntax/sqlgetconnectattr-function.md)|  
|Définition d’un attribut de connexion|[SQLSetConnectAttr](../../../odbc/reference/syntax/sqlsetconnectattr-function.md)|
