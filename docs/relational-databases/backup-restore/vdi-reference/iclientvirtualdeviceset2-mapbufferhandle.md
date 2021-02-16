---
title: IClientVirtualDeviceSet2::MapBufferHandle
titlesuffix: SQL Server VDI reference
description: Cet article fournit des informations de référence sur la commande IClientVirtualDeviceSet2::MapBufferHandle.
ms.date: 08/30/2019
ms.prod: sql
ms.prod_service: backup-restore
ms.technology: backup-restore
ms.topic: reference
author: cawrites
ms.author: chadam
ms.openlocfilehash: 28ce82433b718bc98c6a27e66385b97996fe65a0
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100347223"
---
# <a name="iclientvirtualdeviceset2mapbufferhandle-vdi"></a>IClientVirtualDeviceSet2::MapBufferHandle (VDI)

[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]

La fonction **MapBufferHandle** est utilisée pour obtenir une adresse de mémoire tampon valide à partir d’un descripteur de mémoire tampon obtenu auprès d’un autre processus.

## <a name="syntax"></a>Syntaxe

```c
HRESULT IClientVirtualDeviceSet2::MapBufferHandle (
   DWORD      dwBuffer,
   BYTE**      ppBuffer
);
```

## <a name="parameters"></a>Paramètres

*dwBuffer* : le descripteur retourné par IClientVirtualDeviceSet2::GetBufferHandle.

*ppBuffer* : l’adresse de la mémoire tampon qui est valide dans le processus actuel.

## <a name="return-value"></a>Valeur de retour

|Valeur de retour | Explication |
|---|---|
| NOERROR | La fonction a réussi. |
| VD_E_PROTOCOL | L’ensemble d’appareils virtuels n’est pas actuellement ouvert. |
| VD_E_INVALID | Le ppBuffer est un descripteur non valide. |

## <a name="remarks"></a>Notes

Veillez à communiquer les descripteurs correctement. Les descripteurs sont locaux à un seul ensemble d’appareils virtuels. Les processus partenaires qui partagent un descripteur doivent s’assurer que les descripteurs de la mémoire tampon sont utilisés uniquement dans l’étendue de l’ensemble d’appareils virtuels à partir duquel la mémoire tampon a été obtenue à la base.

## <a name="next-steps"></a>Étapes suivantes

Pour plus d’informations, consultez [Vue d’ensemble des informations de référence sur l’interface des appareils virtuels SQL Server](reference-virtual-device-interface.md).