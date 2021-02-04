---
description: Méthode getNCharacterStream (int)
title: Méthode getNCharacterStream (int) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
ms.assetid: 6ae704f5-823c-4dfe-8c08-07b547c61a3c
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 2d32325349d63d1dbc4633e97ea7c4aebfd3dc56
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99175433"
---
# <a name="getncharacterstream-method-int"></a>Méthode getNCharacterStream (int)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Récupère la valeur du paramètre désigné sous forme d’objet Reader en fonction de l’index de paramètre.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
public final java.io.Reader getNCharacterStream(int parameterIndex)  
```  
  
#### <a name="parameters"></a>Paramètres  
 *parameterIndex*  
  
 **int** indiquant l’index du paramètre.  
  
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
  
  
