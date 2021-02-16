---
title: IClientVirtualDeviceSet2::GetConfiguration
titlesuffix: SQL Server VDI reference
description: Cet article fournit des informations de référence sur la commande IClientVirtualDeviceSet2::GetConfiguration.
ms.date: 08/30/2019
ms.prod: sql
ms.prod_service: backup-restore
ms.technology: backup-restore
ms.topic: reference
author: cawrites
ms.author: chadam
ms.openlocfilehash: 4ee3d601a463e5ee3d9c766f41180c867955ed5d
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100347230"
---
# <a name="iclientvirtualdeviceset2getconfiguration-vdi"></a>IClientVirtualDeviceSet2::GetConfiguration (VDI)

[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]

La fonction **GetConfiguration** est utilisée pour attendre que le serveur configure l’ensemble d’appareils virtuels.

## <a name="syntax"></a>Syntaxe

```c
HRESULT IClientVirtualDeviceSet2::GetConfiguration (
   DWORD         dwTimeOut,
   VDConfig*      pCfg
);
```

## <a name="parameters"></a>Paramètres

*DwTimeOut* : le délai d’attente en millisecondes. Utilisez INFINITE pour empêcher l’expiration du délai d’attente.

*pCfg* : une fois l’exécution réussie, ceci contient la configuration sélectionnée par le serveur. Pour plus d’informations, consultez Configuration.

## <a name="return-value"></a>Valeur de retour

|Valeur de retour | Explication |
|---|---|
| NOERROR | La configuration a été retournée. |
| VD_E_ABORT | SignalAbort a été appelé. |
| VD_E_TIMEOUT | Expiration du délai d’attente de la fonction. |

## <a name="remarks"></a>Notes

Cette fonction se bloque dans un état Alertable. Une fois l’appel réussi, les appareils de l’ensemble d’appareils virtuels peuvent être ouverts.

## <a name="next-steps"></a>Étapes suivantes

Pour plus d’informations, consultez [Vue d’ensemble des informations de référence sur l’interface des appareils virtuels SQL Server](reference-virtual-device-interface.md).