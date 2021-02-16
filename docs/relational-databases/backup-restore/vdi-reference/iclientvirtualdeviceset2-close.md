---
title: IClientVirtualDeviceSet2::Close
titlesuffix: SQL Server VDI reference
description: Cet article fournit des informations de référence sur la commande IClientVirtualDeviceSet2::Close.
ms.date: 08/30/2019
ms.prod: sql
ms.prod_service: backup-restore
ms.technology: backup-restore
ms.topic: reference
author: cawrites
ms.author: chadam
ms.openlocfilehash: fee9c16399ba091df1878774c38751a2d92880fb
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100347272"
---
# <a name="iclientvirtualdeviceset2close-vdi"></a>IClientVirtualDeviceSet2::Close (VDI)

[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]

La fonction **Close** ferme l’ensemble d’appareils virtuels créé par IClientVirtualDeviceSet2::Create. Cela entraîne la libération de toutes les ressources associées à l’ensemble d’appareils virtuels.

## <a name="syntax"></a>Syntaxe

```c
HRESULT IClientVirtualDeviceSet2::Close ();
```

## <a name="return-value"></a>Valeur de retour

|Valeur de retour | Explication |
|---|---|
| NOERROR | Cette valeur est retournée lorsque l’ensemble d’appareils virtuels a été correctement fermé. |
| VD_E_PROTOCOL | Aucune action n’a été effectuée, car l’ensemble d’appareils virtuels n’était pas ouvert. |
| VD_E_OPEN | Les appareils étaient encore ouverts. |

## <a name="remarks"></a>Notes

L’appel de Close est une déclaration effectuée par le client, indiquant que toutes les ressources utilisées par l’ensemble d’appareils virtuels doivent être libérées. Le client doit s’assurer que toutes les activités impliquant des tampons de données et des appareils virtuels sont terminées avant d’appeler Close. Toutes les interfaces d’appareil virtuel retournées par OpenDevice sont invalidées par Close.

Le client est autorisé à émettre un appel Create sur l’interface de l’ensemble d’appareils virtuels après le retour de l’appel de Close. Un tel appel créerait un nouvel ensemble d’appareils virtuels pour une opération de sauvegarde ou de restauration ultérieure.

Si Close est appelé lorsqu’un ou plusieurs appareils virtuels sont toujours ouverts, VD_E_OPEN est retourné. Dans ce cas, SignalAbort est déclenché en interne, afin de garantir un arrêt correct, si possible. Les ressources VDI sont publiées. Le client doit attendre une indication VD_E_CLOSE sur chaque appareil avant d’appeler IClientVirtualDeviceSet2::Close. Si le client sait que l’ensemble d’appareils virtuels est déjà dans un état Abnormally Terminated (Arrêt anormal), il ne doit pas attendre une indication VD_E_CLOSE de GetCommand et peut appeler IClientVirtualDeviceSet2::Close dès que l’activité sur les mémoires tampons partagées est terminée.

## <a name="next-steps"></a>Étapes suivantes

Pour plus d’informations, consultez [Vue d’ensemble des informations de référence sur l’interface des appareils virtuels SQL Server](reference-virtual-device-interface.md).