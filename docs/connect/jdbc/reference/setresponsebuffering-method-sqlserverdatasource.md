---
description: Méthode setResponseBuffering (SQLServerDataSource)
title: Méthode setResponseBuffering (SQLServerDataSource) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerDataSource.setResponseBuffering(String responseBufferingValue)
apilocation:
- SQLServerDataSource.setResponseBuffering(String responseBufferingValue)
apitype: Assembly
ms.assetid: c9e43ff2-8117-4dca-982d-83c863d0c8e1
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: a62919c7efbefd41505f0ac3e6c021229f3a74d2
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99173167"
---
# <a name="setresponsebuffering-method-sqlserverdatasource"></a>Méthode setResponseBuffering (SQLServerDataSource)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Définit le mode de mise en mémoire tampon des réponses pour les connexions créées avec cet objet [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md).  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
public void setResponseBuffering(java.lang.String value)  
```  
  
#### <a name="parameters"></a>Paramètres  
 *value*  
  
 **Chaîne** contenant le mode de mise en mémoire tampon et de diffusion en continu. Le mode valide peut être l’une des chaînes non sensibles à la casse suivantes : **full** ou **adaptive**.  
  
## <a name="remarks"></a>Notes  
 La valeur **full** consiste à lire l’intégralité du résultat du serveur à l’exécution.  
  
 La valeur **adaptive** consiste à mettre en mémoire tampon le moins de données possible si nécessaire. La valeur **adaptive** correspond au mode de mise en mémoire tampon par défaut.  
  
 Pour plus d’informations sur le mode de mise en mémoire tampon des réponses, consultez [Utiliser la mise en mémoire tampon adaptative](../../../connect/jdbc/using-adaptive-buffering.md).  
  
## <a name="see-also"></a>Voir aussi  
 [SQLServerDataSource, membres](../../../connect/jdbc/reference/sqlserverdatasource-members.md)   
 [SQLServerDataSource, classe](../../../connect/jdbc/reference/sqlserverdatasource-class.md)  
  
  
