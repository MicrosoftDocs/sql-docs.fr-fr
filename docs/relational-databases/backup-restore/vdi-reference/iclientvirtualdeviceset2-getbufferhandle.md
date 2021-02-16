---
title: IClientVirtualDeviceSet2::GetBufferHandle
titlesuffix: SQL Server VDI reference
description: Cet article fournit des informations de référence sur la commande IClientVirtualDeviceSet2::GetBufferHandle.
ms.date: 08/30/2019
ms.prod: sql
ms.prod_service: backup-restore
ms.technology: backup-restore
ms.topic: reference
author: cawrites
ms.author: chadam
ms.openlocfilehash: 3751af7f6d784d58fce46dfe24605260f530a99e
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100347284"
---
# <a name="iclientvirtualdeviceset2getbufferhandle-vdi"></a>IClientVirtualDeviceSet2::GetBufferHandle (VDI)

[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]

Certaines applications peuvent nécessiter plusieurs processus pour fonctionner sur les mémoires tampons retournées par **IClientVirtualDevice2::GetCommand**. Dans ce cas, le processus qui reçoit la commande peut utiliser **GetBufferHandle** pour obtenir un descripteur indépendant du processus qui identifie la mémoire tampon. Ce descripteur peut ensuite être communiqué à tout autre processus qui a également le même ensemble d’appareils virtuels ouvert. Ce processus utilise ensuite IClientVirtualDeviceSet2::MapBufferHandle pour obtenir l’adresse de la mémoire tampon. L’adresse sera probablement différente de celle de son partenaire, car chaque processus peut mapper des mémoires tampons à des adresses différentes.

## <a name="syntax"></a>Syntaxe

```c
HRESULT IClientVirtualDeviceSet2::GetBufferHandle (
   BYTE*         pBuffer,
   DWORD*      pBufferHandle
);
```

## <a name="parameters"></a>Paramètres

*pBuffer* : l’adresse d’une mémoire tampon obtenue à partir d’une commande Read ou Write.

*pBufferHandle* : un identificateur unique pour la mémoire tampon est retourné.

## <a name="return-value"></a>Valeur de retour

|Valeur de retour | Explication |
|---|---|
| NOERROR | La fonction a réussi. |
| VD_E_PROTOCOL | L’ensemble d’appareils virtuels n’est pas actuellement ouvert. |
| VD_E_INVALID | Le pBuffer n’est pas une adresse valide. |

## <a name="remarks"></a>Notes

Le processus qui appelle la fonction GetBufferHandle est responsable de l’appel de IClientVirtualDevice2::CompleteCommand quand le transfert de données est terminé.

## <a name="next-steps"></a>Étapes suivantes

Pour plus d’informations, consultez [Vue d’ensemble des informations de référence sur l’interface des appareils virtuels SQL Server](reference-virtual-device-interface.md).