---
description: Propriété ProtocolDLL (classe ClientNetworkProtocol)
title: Propriété ProtocolDLL (ClientNetworkProtocol)
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: wmi
ms.topic: reference
apiname:
- ProtocolDLL Property (ClientNetworkProtocol Class)
apilocation:
- sqlmgmproviderxpsp2up.mof
apitype: MOFDef
helpviewer_keywords:
- ProtocolDLL property
ms.assetid: fe8650d5-7b9d-46f8-bf74-baf1d9d2a06a
author: CarlRabeler
ms.author: carlrab
ms.openlocfilehash: 17bde6ee4793e80a7dc3c3b536ba3337ac5de97c
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/17/2020
ms.locfileid: "88446311"
---
# <a name="protocoldll-property-clientnetworkprotocol-class"></a>Propriété ProtocolDLL (classe ClientNetworkProtocol)
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  Obtient le nom du fichier .dll requis par le protocole réseau spécifié par [Configurer des protocoles clients](https://technet.microsoft.com/library/ms181035.aspx).  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
object.ProtocolDLL [= value]  
```  
  
## <a name="parts"></a>Éléments  
 *object*  
 A [classe ClientNetworkProtocol](../../../relational-databases/wmi-provider-configuration-classes/clientnetworkprotocol-class/clientnetworkprotocol-class.md) qui représente le protocole réseau utilisé par le client [!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] .  
  
## <a name="property-valuereturn-value"></a>Valeur de propriété/valeur de retour  
 Valeur de chaîne qui spécifie le fichier .dll de protocole requis par le protocole réseau client.  
  
## <a name="remarks"></a>Notes  
  
## <a name="see-also"></a>Voir aussi  
 [Configuration des bibliothèques réseau et des protocoles réseau clients](https://technet.microsoft.com/library/ms181035.aspx)  
  
  
