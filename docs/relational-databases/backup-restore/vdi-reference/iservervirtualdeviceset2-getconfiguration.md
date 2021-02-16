---
title: IServerVirtualDeviceSet2::GetConfiguration
titlesuffix: SQL Server VDI reference
description: Cet article fournit des informations de référence sur la commande IServerVirtualDeviceSet2::GetConfiguration.
ms.date: 08/30/2019
ms.prod: sql
ms.prod_service: backup-restore
ms.technology: backup-restore
ms.topic: reference
author: cawrites
ms.author: chadam
ms.openlocfilehash: d0b2b89a9ac7415c80927e4cedd28a2530c7c5bc
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100347116"
---
# <a name="iservervirtualdeviceset2getconfiguration-vdi"></a>IServerVirtualDeviceSet2::GetConfiguration (VDI)

[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]

La fonction **GetConfiguration** obtient la configuration demandée par le client.

## <a name="syntax"></a>Syntaxe

```c
HRESULT IServerVirtualDeviceSet2::GetConfiguration (
   VDConfig*   pCfg
);
```

## <a name="parameters"></a>Paramètres

*pCfg* : la configuration spécifiée par le client avec IClientVirtualDeviceSet2::Create.

## <a name="return-value"></a>Valeur de retour

Retourne un paramètre *HRESULT* qui indique si l'appel de la méthode a réussi ou a échoué. Une valeur NOERROR indique que l’appel de la méthode a réussi. Une valeur différente de zéro indique qu'une erreur s'est produite.

## <a name="remarks"></a>Notes

Le serveur est censé inspecter les paramètres fournis par le client et y répondre. Pour plus d’informations, consultez Configuration. Le serveur peut utiliser SignalAbort s’il détermine qu’il ne peut pas fonctionner correctement avec la configuration fournie.

## <a name="next-steps"></a>Étapes suivantes

Pour plus d’informations, consultez [Vue d’ensemble des informations de référence sur l’interface des appareils virtuels SQL Server](reference-virtual-device-interface.md).