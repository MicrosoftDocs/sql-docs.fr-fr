---
title: IClientVirtualDevice::CompleteCommand
titlesuffix: SQL Server VDI reference
description: Cet article fournit des informations de référence sur la commande IClientVirtualDevice::CompleteCommand.
ms.date: 08/30/2019
ms.prod: sql
ms.prod_service: backup-restore
ms.technology: backup-restore
ms.topic: reference
author: cawrites
ms.author: chadam
ms.openlocfilehash: bac15898440ec2afad21b6487888290eeee8df8e
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100347292"
---
# <a name="iclientvirtualdevicecompletecommand-vdi"></a>IClientVirtualDevice::CompleteCommand (VDI)

[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]

La fonction **CompleteCommand** est utilisée pour avertir SQL Server qu’une commande est terminée. Les informations d’achèvement appropriées pour la commande doivent être retournées.

## <a name="syntax"></a>Syntaxe

```c
HRESULT IClientVirtualDevice::CompleteCommand (
   VDC_Command*         const pCmd,
   UINT32               dwCompletionCode,
   UINT32               dwBytesTransferred,
   UINT64               dwlPosition
);
```

## <a name="parameters"></a>Paramètres

*pCmd* : l’adresse d’une commande précédemment retournée à partir de IClientVirtualDevice::GetCommand.

*dwCompletionCode* : un code d’état WIN32 qui indique l’état d’achèvement. Ce paramètre doit être retourné pour toutes les commandes. Le code retourné doit être approprié pour la commande en cours d’exécution. ERROR_SUCCESS est utilisé dans tous les cas pour indiquer une commande exécutée avec succès. Pour obtenir la liste complète des codes possibles, consultez le fichier Winerror.h. Une liste de codes d’état courants pour chaque commande apparaît dans la section « Commandes ».

*dwBytesTransferred* : le nombre d’octets transférés avec succès. Il est retourné uniquement pour les commandes de transfert de données en lecture et en écriture.

*dwlPosition* : une réponse à la commande GetPosition uniquement.

## <a name="return-value"></a>Valeur de retour

|Valeur de retour | Explication |
|---|---|
| NOERROR | La saisie semi-automatique a été correctement notée. |
| VD_E_INVALID | pCmd n’était pas une commande active. |
| VD_E_ABORT | Abort a été signalé. |
| VD_E_PROTOCOL | L'appareil n'est pas ouvert. |

## <a name="next-steps"></a>Étapes suivantes

Pour plus d’informations, consultez [Vue d’ensemble des informations de référence sur l’interface des appareils virtuels SQL Server](reference-virtual-device-interface.md).
