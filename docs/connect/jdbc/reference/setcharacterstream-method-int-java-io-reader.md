---
description: Méthode setCharacterStream (int, java.io.Reader)
title: setCharacterStream, méthode (int, java.io.Reader) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
ms.assetid: b8d4e1f7-14fc-4590-af98-1eda30d2ca6d
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 550a8792f1baea96aa6fd8b4ff8c59d7a75ce2c0
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99173753"
---
# <a name="setcharacterstream-method-int-javaioreader"></a>Méthode setCharacterStream (int, java.io.Reader)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Définit le paramètre désigné sur la valeur de l’objet java.io.Reader spécifiée.  
  
> [!NOTE]
>  Cette fonctionnalité a été introduite avec la version 2.0 du pilote [!INCLUDE[msCoName](../../../includes/msconame_md.md)][!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] JDBC.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
public final void setCharacterStream(int parameterIndex,  
                              java.io.Reader reader)  
```  
  
#### <a name="parameters"></a>Paramètres  
 *parameterIndex*  
  
 **int** indiquant le numéro du paramètre.  
  
 *reader*  
  
 L’objet java.io.Reader contenant les données Unicode.  
  
## <a name="exceptions"></a>Exceptions  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Notes  
 Cette méthode setCharacterStream est spécifiée par la méthode setCharacterStream de l’interface java.sql.PreparedStatement.  
  
## <a name="see-also"></a>Voir aussi  
 [setCharacterStream, méthode &#40;SQLServerPreparedStatement&#41;](../../../connect/jdbc/reference/setcharacterstream-method-sqlserverpreparedstatement.md)   
 [Membres de SQLServerPreparedStatement](../../../connect/jdbc/reference/sqlserverpreparedstatement-members.md)  
  
  
