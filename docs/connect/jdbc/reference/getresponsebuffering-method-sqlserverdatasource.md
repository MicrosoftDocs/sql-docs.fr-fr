---
description: Méthode getResponseBuffering (SQLServerDataSource)
title: Méthode getResponseBuffering (SQLServerDataSource) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
apiname:
- SQLServerDataSource.getResponseBuffering()
apilocation:
- SQLServerDataSource.getResponseBuffering()
apitype: Assembly
ms.assetid: 19585a93-88a4-415e-a20e-12ba58cddeaa
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 387f5442224f26095b0803891725ab8af81996ef
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/17/2020
ms.locfileid: "88434801"
---
# <a name="getresponsebuffering-method-sqlserverdatasource"></a>Méthode getResponseBuffering (SQLServerDataSource)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Retourne le mode de mise en mémoire tampon des réponses pour cet objet [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md).  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
public java.lang.String getResponseBuffering()  
```  
  
## <a name="return-value"></a>Valeur de retour  
 **Chaîne** contenant une minuscule, **full** ou **adaptative**.  
  
## <a name="remarks"></a>Notes  
 La valeur **full** consiste à lire l’intégralité du résultat du serveur à l’exécution.  
  
 La valeur **adaptive** consiste à mettre en mémoire tampon le moins de données possible si nécessaire. La valeur **adaptive** correspond au mode de mise en mémoire tampon par défaut.  
  
 Pour plus d’informations sur le mode de mise en mémoire tampon des réponses, consultez [Utiliser la mise en mémoire tampon adaptative](../../../connect/jdbc/using-adaptive-buffering.md).  
  
## <a name="see-also"></a>Voir aussi  
 [setResponseBuffering, méthode &#40;SQLServerDataSource&#41;](../../../connect/jdbc/reference/setresponsebuffering-method-sqlserverdatasource.md)   
 [SQLServerDataSource, membres](../../../connect/jdbc/reference/sqlserverdatasource-members.md)   
 [SQLServerDataSource, classe](../../../connect/jdbc/reference/sqlserverdatasource-class.md)  
  
  
