---
description: SQLSetConnectAttrForDbcInfo, fonction
title: SQLSetConnectAttrForDbcInfo fonction) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- SQLSetConnectAttrForDbcInfo function [ODBC]
ms.assetid: a28fadb9-b998-472a-b252-709507e92005
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 2ca64e1309c939f4b39b8b9a020a1debb1cc10b9
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99192441"
---
# <a name="sqlsetconnectattrfordbcinfo-function"></a>SQLSetConnectAttrForDbcInfo, fonction
**Conformité**  
 Version introduite : ODBC 3,81 conforme aux normes : ODBC  
  
 **Résumé**  
 **SQLSetConnectAttrForDbcInfo** est identique à **SQLSetConnectAttr**, mais définit l’attribut sur le jeton d’informations de connexion plutôt que sur le handle de connexion.  
  
## <a name="syntax"></a>Syntaxe  
  
```cpp
  
SQLRETURN  SQLSetConnectAttrForDbcInfo(  
                SQLHDBC_INFO_TOKEN    hDbcInfoToken,  
                SQLINTEGER            Attribute,  
                SQLPOINTER            ValuePtr,  
                SQLINTEGER            StringLength );  
```  
  
## <a name="arguments"></a>Arguments  
 *hDbcInfoToken*  
 Entrée Handle de jeton.  
  
 *Attribut*  
 Entrée Attribut à définir. La liste des attributs valides est spécifique au pilote et est identique à celle de [SQLSetConnectAttr](../../../odbc/reference/syntax/sqlsetconnectattr-function.md).  
  
 *ValuePtr*  
 Entrée Pointeur vers la valeur à associer à l' *attribut*. Selon la valeur de l' *attribut*, *ValuePtr* sera une valeur d’entier non signé 32 bits ou pointera vers une chaîne de caractères terminée par le caractère null. Notez que si l’argument d' *attribut* est une valeur spécifique au pilote, la valeur de *ValuePtr* peut être un entier signé.  
  
 *StringLength*  
 Entrée Si l' *attribut* est un attribut défini par ODBC et que *ValuePtr* pointe vers une chaîne de caractères ou une mémoire tampon binaire, cet argument doit être la longueur de **ValuePtr*. Pour les données de type chaîne de caractères, cet argument doit contenir le nombre d’octets dans la chaîne.  
  
 Si l' *attribut* est un attribut défini par ODBC et que *ValuePtr* est un entier, *StringLength* est ignoré.  
  
 Si l' *attribut* est un attribut défini par le pilote, l’application indique la nature de l’attribut au gestionnaire de pilotes en définissant l’argument *StringLength* . *StringLength* peut avoir les valeurs suivantes :  
  
-   Si *ValuePtr* est un pointeur vers une chaîne de caractères, *StringLength* est la longueur de la chaîne ou SQL_NTS.  
  
-   Si *ValuePtr* est un pointeur vers une mémoire tampon binaire, l’application place le résultat de la macro SQL_LEN_BINARY_ATTR (*longueur*) dans *StringLength*. Cela place une valeur négative dans *StringLength*.  
  
-   Si *ValuePtr* est un pointeur vers une valeur autre qu’une chaîne de caractères ou une chaîne binaire, *StringLength* doit avoir la valeur SQL_IS_POINTER.  
  
-   Si *ValuePtr* contient une valeur de longueur fixe, *StringLength* est soit SQL_IS_INTEGER, soit SQL_IS_UINTEGER, selon le cas.  
  
## <a name="returns"></a>Retours  
 SQL_SUCCESS, SQL_SUCCESS_WITH_INFO, SQL_ERROR ou SQL_INVALID_HANDLE.  
  
## <a name="diagnostics"></a>Diagnostics  
 Identique à [SQLSetConnectAttr](../../../odbc/reference/syntax/sqlsetconnectattr-function.md), sauf que le gestionnaire de pilotes utilisera un **comme HandleType** de SQL_HANDLE_DBC_INFO_TOKEN et un **handle** de *hDbcInfoToken*.  
  
## <a name="remarks"></a>Notes  
 **SQLSetConnectAttrForDbcInfo** est identique à **SQLSetConnectAttr**, mais définit l’attribut sur le jeton d’informations de connexion, plutôt que sur le handle de connexion. Par exemple, si **SQLSetConnectAttr** ne reconnaît pas un attribut, **SQLSetConnectAttrForDbcInfo** doit également retourner SQL_ERROR pour cet attribut.  
  
 Chaque fois que le pilote retourne SQL_ERROR ou SQL_INVALID_HANDLE, le pilote doit ignorer cet attribut pour calculer l’ID du pool. En outre, le gestionnaire de pilotes obtiendra les informations de diagnostic à partir de *hDbcInfoToken* et renverra SQL_SUCCESS_WITH_INFO à l’application dans [SQLConnect](../../../odbc/reference/syntax/sqlconnect-function.md) et [SQLDriverConnect](../../../odbc/reference/syntax/sqldriverconnect-function.md). Par conséquent, une application peut récupérer des détails sur la raison pour laquelle certains attributs ne peuvent pas être définis.  
  
 Les applications ne doivent pas appeler cette fonction directement. Un pilote ODBC qui prend en charge le regroupement de connexions prenant en charge les pilotes doit implémenter cette fonction.  
  
 Incluez sqlspi. h pour le développement du pilote ODBC.  
  
## <a name="see-also"></a>Voir aussi  
 [Développement d’un pilote ODBC](../../../odbc/reference/develop-driver/developing-an-odbc-driver.md)   
 [Regroupement de connexions prenant en charge les pilotes](../../../odbc/reference/develop-app/driver-aware-connection-pooling.md)   
 [Développement de la reconnaissance des pools de connexions dans un pilote ODBC](../../../odbc/reference/develop-driver/developing-connection-pool-awareness-in-an-odbc-driver.md)
