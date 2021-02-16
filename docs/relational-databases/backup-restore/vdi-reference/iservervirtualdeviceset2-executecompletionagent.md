---
title: IServerVirtualDeviceSet2::ExecuteCompletionAgent
titlesuffix: SQL Server VDI reference
description: Cet article fournit des informations de référence sur la commande IServerVirtualDeviceSet2::ExecuteCompletionAgent.
ms.date: 08/30/2019
ms.prod: sql
ms.prod_service: backup-restore
ms.technology: backup-restore
ms.topic: reference
author: cawrites
ms.author: chadam
ms.openlocfilehash: 75b09df68be51f74cbb62dac5ddf039259ad79f1
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100347104"
---
# <a name="iservervirtualdeviceset2executecompletionagent-vdi"></a>IServerVirtualDeviceSet2::ExecuteCompletionAgent (VDI)

[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]

La fonction **ExecuteCompletionAgent** est utilisée pour implémenter la boucle principale de l’agent d’achèvement.

## <a name="syntax"></a>Syntaxe

```c
HRESULT IServerVirtualDeviceSet2::ExecuteCompletionAgent ();
```

## <a name="return-value"></a>Valeur de retour

Retourne un paramètre *HRESULT* qui indique si l'appel de la méthode a réussi ou a échoué. Une valeur NOERROR indique que l’appel de la méthode a réussi. Une valeur différente de zéro indique qu'une erreur s'est produite.

## <a name="remarks"></a>Notes

L’agent d’achèvement fournit un mécanisme par lequel SQL Server peut se synchroniser lui-même avec l’achèvement des commandes des appareils virtuels. Il doit être actif avant que des commandes puissent être émises et il doit rester actif jusqu’à la fermeture de tous les appareils.

Comme SQL Server peut devoir effectuer une initialisation de thread spéciale, cette interface ne démarre pas un nouveau thread de contrôle. Au lieu de cela, SQL Server configure un thread, puis passe le contrôle à cette routine. Le thread doit être blocable sur des mécanismes de communication interprocessus (IPC) de Windows NT et être en mesure d’appeler une des fonctions de rappel fournies avec les commandes envoyées via IServerVirtualDevice::SendCommand.

Cette fonction ne retourne pas tant que IServerVirtualDeviceSet2::Close ou SignalAbort n’est pas appelé.

## <a name="next-steps"></a>Étapes suivantes

Pour plus d’informations, consultez [Vue d’ensemble des informations de référence sur l’interface des appareils virtuels SQL Server](reference-virtual-device-interface.md).