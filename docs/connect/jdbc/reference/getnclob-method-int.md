---
description: Méthode getNClob (int)
title: Méthode getNClob (int) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
ms.assetid: 10dfa251-9408-469e-ae2a-1acf3917cf47
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 4cfd906b2b109d2785c016e82971f289622c1bec
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99175400"
---
# <a name="getnclob-method-int"></a>Méthode getNClob (int)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Récupère la valeur du paramètre **NCLOB** JDBC désigné sous forme d’objet NClob dans le langage de programmation Java.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
public java.sql.NClob getNClob(int parameterIndex)  
```  
  
#### <a name="parameters"></a>Paramètres  
 *parameterIndex*  
  
 **int** indiquant l’index du paramètre.  
  
## <a name="return-value"></a>Valeur de retour  
 Objet NClob.  
  
## <a name="exceptions"></a>Exceptions  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Notes  
 Cette méthode getNClob est spécifiée par la méthode getNClob de l’interface java.sql.CallableStatement.  
  
 Cette méthode ne permet de récupérer que des paramètres **NCHAR**, **NVARCHAR**, **NTEXT** et **XML**. L'appel de ces méthodes sur d'autres paramètres de type de données entraîne une exception.  
  
## <a name="see-also"></a>Voir aussi  
 [getNClob, méthode &#40;SQLServerCallableStatement&#41;](../../../connect/jdbc/reference/getnclob-method-sqlservercallablestatement.md)   
 [Membres de SQLServerCallableStatement](../../../connect/jdbc/reference/sqlservercallablestatement-members.md)  
  
  
