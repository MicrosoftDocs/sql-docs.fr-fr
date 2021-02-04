---
description: Méthode getNCharacterStream (java.lang.String)
title: Méthode getNCharacterStream (java.lang.String) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
ms.assetid: 45d2695b-0727-419d-8921-a51d6feef0aa
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 7fbf13e4757ccf709d6b6d3151b454bc87b037a3
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99175427"
---
# <a name="getncharacterstream-method-javalangstring"></a>Méthode getNCharacterStream (java.lang.String)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Récupère la valeur du paramètre désigné sous forme d’objet Reader en fonction du nom de paramètre.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
public final java.io.Reader getNCharacterStream(java.lang.String columnLabel)  
```  
  
#### <a name="parameters"></a>Paramètres  
 *columnLabel*  
  
 **String** contenant l’étiquette de colonne.  
  
## <a name="return-value"></a>Valeur de retour  
 Objet Reader.  
  
## <a name="exceptions"></a>Exceptions  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Notes  
 Cette méthode doit être utilisée lors de l’accès aux paramètres **NCHAR**, **NVARCHAR** et **LONGNVARCHAR**.  
  
 Cette méthode getNCharacterStream est spécifiée par la méthode getNCharacterStream de l’interface java.sql.CallableStatement.  
  
## <a name="see-also"></a>Voir aussi  
 [getNCharacterStream, méthode &#40;SQLServerCallableStatement&#41;](../../../connect/jdbc/reference/getncharacterstream-method-sqlservercallablestatement.md)   
 [Membres de SQLServerCallableStatement](../../../connect/jdbc/reference/sqlservercallablestatement-members.md)  
  
  
