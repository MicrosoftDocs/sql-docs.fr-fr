---
description: Méthode updateNCharacterStream (java.lang.String, java.io.Reader)
title: updateNCharacterStream, méthode (java.lang.String, java.io.Reader) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
ms.assetid: 504d7d06-0227-45e1-8b01-899c3e6006e8
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 50cb102543f9fe482e8e5e4afc07c90dac05cd8e
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99193308"
---
# <a name="updatencharacterstream-method-javalangstring-javaioreader"></a>Méthode updateNCharacterStream (java.lang.String, java.io.Reader)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Met à jour la colonne désignée avec une valeur de flux de caractères.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
public void updateNCharacterStream(java.lang.String columnLabel,  
                                  java.io.Reader reader)  
```  
  
#### <a name="parameters"></a>Paramètres  
 *columnLabel*  
  
 **String** contenant l’étiquette de colonne.  
  
 *reader*  
  
 Objet Reader.  
  
## <a name="exceptions"></a>Exceptions  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Notes  
 Cette méthode updateNCharacterStream est spécifiée par la méthode updateNCharacterStream de l’interface java.sql.ResultSet.  
  
 Cette méthode transmet les caractères Unicode d’un objet Reader aux colonnes **nchar**, **nvarchar(max)** , **ntext** et **xml** sélectionnées. L'utilisation de cette méthode sur d'autres colonnes de type de données lève une exception.  
  
## <a name="see-also"></a>Voir aussi  
 [updateNCharacterStream, méthode &#40;SQLServerResultSet&#41;](../../../connect/jdbc/reference/updatencharacterstream-method-sqlserverresultset.md)   
 [SQLServerResultSet, membres](../../../connect/jdbc/reference/sqlserverresultset-members.md)   
 [SQLServerResultSet, classe](../../../connect/jdbc/reference/sqlserverresultset-class.md)  
  
  
