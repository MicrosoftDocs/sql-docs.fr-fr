---
title: IServerVirtualDeviceSet2::Close
titlesuffix: SQL Server VDI reference
description: Cet article fournit des informations de référence sur la commande IServerVirtualDeviceSet2::Close.
ms.date: 08/30/2019
ms.prod: sql
ms.prod_service: backup-restore
ms.technology: backup-restore
ms.topic: reference
author: cawrites
ms.author: chadam
ms.openlocfilehash: 2bc520d4cb4ded130c6d44a2fef70ddd4b5ff5d4
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100347140"
---
# <a name="iservervirtualdeviceset2close-vdi"></a>IServerVirtualDeviceSet2::Close (VDI)

[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]

La fonction **Close** ferme un ensemble d’appareils virtuels ouvert par IServerVirtualDeviceSet2::Open. Ceci entraîne la libération de toutes les ressources associées à l’ensemble d’appareils virtuels. Le descripteur IServerVirtualDeviceSet2 n’est pas utile après le retour de cette fonction et doit être retourné à COM.

## <a name="syntax"></a>Syntaxe

```c
HRESULT IServerVirtualDeviceSet2::Close ();
```

## <a name="return-value"></a>Valeur de retour

|Valeur de retour | Explication |
|---|---|
| VD_E_PROTOCOL | Les appareils étaient encore ouverts. |

## <a name="remarks"></a>Notes

Une fermeture de l’ensemble d’appareils virtuels avant la fermeture des appareils ne doit pas être effectuée. Si cette situation se produit, VD_E_PROTOCOL est retourné. Cette action fait que Close libère immédiatement son mappage de la mémoire partagée. Le serveur est l’objet de violations d’accès s’il continue à attendre la propriété des ressources retournées depuis l’interface de l’appareil virtuel. L’interface effectue le traitement SignalAbort.

L’agent d’achèvement, s’il est en cours d’exécution, termine toutes les commandes en suspens avant de retourner à son appelant. Toutes les commandes en suspens sont exécutées avec ERROR_OPERATION_ABORTED. Autrement dit, la fonction de rappel est appelée pour chaque commande de ce type.

Dans tous les cas, y compris quand des erreurs sont retournées, Close libère toutes les ressources pour l’interface de l’appareil virtuel. Les mémoires tampons et les autres pointeurs d’interface retournés depuis l’interface VDI deviennent non valides.

Il est important de vérifier que l’agent d’achèvement a été arrêté avant le déchargement de la bibliothèque COM. Si la bibliothèque est déchargée avant que l’agent d’achèvement retourne à son appelant, le processus peut provoquer une violation d’instruction.

## <a name="next-steps"></a>Étapes suivantes

Pour plus d’informations, consultez [Vue d’ensemble des informations de référence sur l’interface des appareils virtuels SQL Server](reference-virtual-device-interface.md).