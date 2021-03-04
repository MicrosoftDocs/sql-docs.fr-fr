---
title: Commutateurs AppContext dans SqlClient
description: Explique comment utiliser les commutateurs AppContext disponibles dans SqlClient.
ms.date: 06/15/2020
dev_langs:
- csharp
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: johnnypham
ms.author: v-jopha
ms.reviewer: v-daenge
ms.openlocfilehash: aa6de0ff33b0d6a70ef14a42f5def3f3c6a20482
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/04/2021
ms.locfileid: "101836003"
---
# <a name="appcontext-switches-in-sqlclient"></a>Commutateurs AppContext dans SqlClient

[!INCLUDE [Driver_ADONET_Download](../../includes/driver_adonet_download.md)]

La classe AppContext permet à SqlClient de fournir de nouvelles fonctionnalités tout en continuant à prendre en charge les appelants qui dépendent du comportement précédent. Les utilisateurs peuvent refuser un changement de comportement en définissant des commutateurs AppContext spécifiques.

## <a name="enabling-decimal-truncation-behavior"></a>Activation du comportement de troncation décimale

[!INCLUDE [appliesto-netfx-netcore-netst-md](../../includes/appliesto-netfx-netcore-netst-md.md)]

À compter de la version 2.0 de Microsoft.Data.SqlClient, les données décimales sont arrondies par défaut, comme le fait SQL Server. Pour activer le comportement de troncation précédent, vous pouvez définir le commutateur AppContext **Switch.Microsoft.Data.SqlClient.TruncateScaledDecimal** sur `true` au démarrage de l’application :

```csharp
AppContext.SetSwitch("Switch.Microsoft.Data.SqlClient.TruncateScaledDecimal", true);
```

## <a name="enabling-managed-networking-on-windows"></a>Activation de la mise en réseau gérée sur Windows

[!INCLUDE [appliesto-xxxx-netcore-netst-md](../../includes/appliesto-xxxx-netcore-netst-md.md)]

Sur Windows, SqlClient utilise une implémentation native de l’interface réseau SNI par défaut. Pour permettre l’utilisation d’une implémentation SNI gérée, vous pouvez définir le commutateur AppContext **Switch.Microsoft.Data.SqlClient.UseManagedNetworkingOnWindows** sur `true` au démarrage de l’application :

```csharp
AppContext.SetSwitch("Switch.Microsoft.Data.SqlClient.UseManagedNetworkingOnWindows", true);
```

Ce commutateur modifie le comportement du pilote de façon à utiliser une implémentation réseau gérée dans les projets .NET Core 2.1 (et versions ultérieures) et .NET Standard 2.0 (et versions ultérieures) sur Windows, éliminant ainsi toutes les dépendances vis-à-vis de bibliothèques natives pour la bibliothèque Microsoft.Data.SqlClient. Il est uniquement destiné aux tests et au débogage.

> [!NOTE]
> Il existe des différences connues par rapport à l’implémentation native. Par exemple, l’implémentation gérée ne prend pas en charge l’Authentification Windows sans domaine.

## <a name="disabling-transparent-network-ip-resolution"></a>Désactivation de la résolution d’adresses IP réseau transparente

[!INCLUDE [appliesto-netfx-xxxx-xxxx-md](../../includes/appliesto-netfx-xxxx-xxxx-md.md)]

La résolution d’adresses IP réseau transparente (TNIR) est une révision de la fonctionnalité MultiSubnetFailover existante. Elle affecte la séquence de connexion du pilote dans le cas où la première adresse IP résolue du nom d’hôte ne répond pas et qu’il existe plusieurs adresses IP associées au nom d’hôte. Elle interagit avec MultiSubnetFailover pour fournir les trois séquences de connexion suivantes :<br />
* 0 : Une adresse IP est tentée, suivie de toutes les adresses IP en parallèle.
* 1 : Toutes les adresses IP sont tentées en parallèle.
* 2 : Toutes les adresses IP sont tentées l’une après l’autre.

|TransparentNetworkIPResolution|MultiSubnetFailover|Comportement|
|--------|--------|--------|
|True|True|1|
|Vrai|False|0|
|False|True|1|
|False|False|2|

TransparentNetworkIPResolution est activé par défaut. MultiSubnetFailover est désactivé par défaut. Pour désactiver la résolution d’adresses IP réseau transparente, vous pouvez définir le commutateur AppContext **Switch.Microsoft.Data.SqlClient.DisableTNIRByDefaultInConnectionString** sur `true` au démarrage de l’application :

```csharp
AppContext.SetSwitch("Switch.Microsoft.Data.SqlClient.DisableTNIRByDefaultInConnectionString", true);
```

Pour plus d’informations sur la définition de ces propriétés, consultez la documentation relative à la [propriété SqlConnection.ConnectionString](/dotnet/api/microsoft.data.sqlclient.sqlconnection.connectionstring). 

## <a name="enable-a-minimum-timeout-during-login"></a>Activation d’un délai d’expiration minimal pendant la connexion

[!INCLUDE [appliesto-netfx-xxxx-xxxx-md](../../includes/appliesto-netfx-xxxx-xxxx-md.md)]

Pour empêcher une tentative de connexion d’attendre indéfiniment, vous pouvez définir le commutateur AppContext **Switch.Microsoft.Data.SqlClient.UseOneSecFloorInTimeoutCalculationDuringLogin** sur `true` au démarrage de l’application :

```csharp
AppContext.SetSwitch("Switch.Microsoft.Data.SqlClient.UseOneSecFloorInTimeoutCalculationDuringLogin", false);
```

## <a name="disable-blocking-behavior-of-readasync"></a>Désactivation du comportement de blocage de ReadAsync

[!INCLUDE [appliesto-netfx-xxxx-xxxx-md](../../includes/appliesto-netfx-xxxx-xxxx-md.md)]

Par défaut, ReadAsync s’exécute de façon synchrone et bloque le thread appelant sur .NET Framework. Pour désactiver ce comportement de blocage, vous pouvez définir le commutateur AppContext **Switch.Microsoft.Data.SqlClient.MakeReadAsyncBlocking** sur `false` au démarrage de l’application :

```csharp
AppContext.SetSwitch("Switch.Microsoft.Data.SqlClient.MakeReadAsyncBlocking", false);
```

## <a name="see-also"></a>Voir aussi

[Classe AppContext](/dotnet/api/system.appcontext?view=netcore-3.1&preserve-view=true)