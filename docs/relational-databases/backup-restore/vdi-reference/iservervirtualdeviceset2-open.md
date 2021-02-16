---
title: IServerVirtualDeviceSet2::Open
titlesuffix: SQL Server VDI reference
description: Cet article fournit des informations de référence sur la commande IServerVirtualDeviceSet2::Open.
ms.date: 08/30/2019
ms.prod: sql
ms.prod_service: backup-restore
ms.technology: backup-restore
ms.topic: reference
author: cawrites
ms.author: chadam
ms.openlocfilehash: bb5341a03ee64b3b9d1d23508e1e59048f843b35
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100347086"
---
# <a name="iservervirtualdeviceset2open-vdi"></a>IServerVirtualDeviceSet2::Open (VDI)

[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]

La fonction **Open** ouvre l’ensemble d’appareils virtuels sur le serveur, et il doit s’agir du premier appel effectué avec le descripteur d’interface fourni par COM.

## <a name="syntax"></a>Syntaxe

```c
HRESULT IServerVirtualDeviceSet2::Open (
   LPCWSTR      lpInstanceName,
   LPCWSTR      lpName
);
```

## <a name="parameters"></a>Paramètres

*lpInstanceName* : cette chaîne identifie l’instance SQL Server à laquelle la commande SQL sera envoyée. La valeur NULL peut être passée pour identifier l’instance par défaut sur la machine actuelle.

*lpName* : fourni à partir de la première clause VIRTUAL_DEVICE = de la commande BACKUP ou RESTORE. Ce nom est utilisé comme clé pour obtenir l’accès à l’ensemble d’appareils virtuels créé par le client.

## <a name="return-value"></a>Valeur de retour

|Valeur de retour | Explication |
|---|---|
| NOERROR | La fonction a réussi. |
| VD_E_INVALID | Le nom fourni n’a pas identifié un ensemble d’appareils virtuels accessible au serveur. |

## <a name="remarks"></a>Notes

Une fois que cette fonction a été appelée avec succès, le serveur peut procéder à la configuration de l’ensemble d’appareils virtuels avec GetConfiguration et SetConfiguration.

## <a name="next-steps"></a>Étapes suivantes

Pour plus d’informations, consultez [Vue d’ensemble des informations de référence sur l’interface des appareils virtuels SQL Server](reference-virtual-device-interface.md).