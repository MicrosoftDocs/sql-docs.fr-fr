---
description: Méthode getSendStringParametersAsUnicode (SQLServerDataSource)
title: Méthode getSendStringParametersAsUnicode (SQLServerDataSource) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
apiname:
- SQLServerDataSource.getSendStringParametersAsUnicode
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: 3836d0ab-c3fb-41ff-bb89-10389594ae51
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 0fc9c5e4ba97f1e48e446768d59d5308604d5b29
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/17/2020
ms.locfileid: "88434581"
---
# <a name="getsendstringparametersasunicode-method-sqlserverdatasource"></a>Méthode getSendStringParametersAsUnicode (SQLServerDataSource)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Retourne une valeur **booléenne** qui indique si l’envoi de paramètres de type chaîne au serveur au format UNICODE est activé.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
public boolean getSendStringParametersAsUnicode()  
```  
  
## <a name="return-value"></a>Valeur de retour  
 Si elle a la valeur **true**, les paramètres de type string sont envoyés au serveur au format UNICODE. Dans le cas contraire, la valeur est **false**.  
  
## <a name="remarks"></a>Notes  
 Si la propriété sendStringParametersAsUnicode a la valeur **true** (valeur par défaut), les paramètres de type chaîne sont envoyés au serveur au format UNICODE. Si la propriété sendStringParametersAsUnicode a la valeur **false**, les paramètres de type chaîne sont envoyés au serveur au format ASCII/MBCS, et non UNICODE. Si sendStringParametersAsUnicode n’est pas défini, getSendStringParametersAsUnicode retourne la valeur par défaut (**true**).  
  
 Pour plus d'informations sur la propriété de connexion sendStringParametersAsUnicode, consultez [Définir les propriétés de connexion](../../../connect/jdbc/setting-the-connection-properties.md).  
  
## <a name="see-also"></a>Voir aussi  
 [SQLServerDataSource, membres](../../../connect/jdbc/reference/sqlserverdatasource-members.md)   
 [SQLServerDataSource, classe](../../../connect/jdbc/reference/sqlserverdatasource-class.md)  
  
  
