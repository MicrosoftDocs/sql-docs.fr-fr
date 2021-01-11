---
title: Compteurs de performances dans SqlClient
description: Utilisez les compteurs de performances du fournisseur de données Microsoft SqlClient pour SQL Server pour surveiller l’état de votre application et ses ressources de connexion à l’aide de l’analyseur de performances Windows ou par programme.
ms.date: 12/04/2020
dev_langs:
- csharp
ms.assetid: 0b121b71-78f8-4ae2-9aa1-0b2e15778e57
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
ms.reviewer: v-chmalh
ms.openlocfilehash: e9d8c2edb88a9ed50b47c761d3af8aec8016065a
ms.sourcegitcommit: 5b2c47ce88f7e56552fd415c32b319009d043b56
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/29/2020
ms.locfileid: "97804288"
---
# <a name="performance-counters-in-sqlclient"></a>Compteurs de performances dans SqlClient

[!INCLUDE[appliesto-netfx-xxxx-xxxx-md](../../includes/appliesto-netfx-xxxx-xxxx-md.md)]

[!INCLUDE[Driver_ADONET_Download](../../includes/driver_adonet_download.md)]

Vous pouvez utiliser des compteurs de performances <xref:Microsoft.Data.SqlClient> pour surveiller l’état de votre application et les ressources de connexion qu’elle utilise. Vous pouvez surveiller les compteurs de performance à l'aide de l'Analyseur de performances Windows ou accéder à ces derniers par programme à l'aide de la classe <xref:System.Diagnostics.PerformanceCounter> dans l'espace de noms <xref:System.Diagnostics>.

## <a name="available-performance-counters"></a>Compteurs de performances disponibles

Actuellement, il existe 14 compteurs de performances différents disponibles pour <xref:Microsoft.Data.SqlClient>, comme décrit dans le tableau suivant.

|Compteur de performances|Description|  
|-------------------------|-----------------|  
|`HardConnectsPerSecond`|Nombre de connexions par seconde qui sont établies à un serveur de base de données.|  
|`HardDisconnectsPerSecond`|Nombre de déconnexions par seconde qui sont effectuées à un serveur de base de données.|  
|`NumberOfActiveConnectionPoolGroups`|Nombre de groupes du pool de connexion unique qui sont actifs. Ce compteur est contrôlé par le nombre de chaînes de connexion uniques qui se trouvent dans AppDomain.|  
|`NumberOfActiveConnectionPools`|Nombre total de regroupements de connexions.|  
|`NumberOfActiveConnections`|Nombre de connexions actives en cours d'utilisation. **Remarque :**  Ce compteur de performance n’est pas activé par défaut. Pour activer ce compteur de performance, voir [Activation des compteurs désactivés par défaut](#ActivatingOffByDefault).|  
|`NumberOfFreeConnections`|Nombre de connexions disponibles pour une utilisation dans les regroupements de connexions. **Remarque :**  Ce compteur de performance n’est pas activé par défaut. Pour activer ce compteur de performance, voir [Activation des compteurs désactivés par défaut](#ActivatingOffByDefault).|  
|`NumberOfInactiveConnectionPoolGroups`|Nombre de groupes du regroupement de connexions unique qui sont marqués pour le nettoyage. Ce compteur est contrôlé par le nombre de chaînes de connexion uniques qui se trouvent dans AppDomain.|  
|`NumberOfInactiveConnectionPools`|Nombre de regroupements de connexions inactifs sans activité récente et en attente de suppression.|  
|`NumberOfNonPooledConnections`|Nombre de connexions actives qui n'ont pas été regroupées.|  
|`NumberOfPooledConnections`|Nombre de connexions actives qui sont gérées par l'infrastructure de regroupement de connexions.|  
|`NumberOfReclaimedConnections`|Nombre de connexions ayant été récupérées par le biais du garbage collection dans lequel `Close` ou `Dispose` n'a pas été appelé par l'application. **Remarque** La fermeture ou la suppression explicite des connexions nuit aux performances.|  
|`NumberOfStasisConnections`|Nombre de connexions en attente de l'achèvement d'une action et par conséquent non disponibles pour une utilisation par votre application.|  
|`SoftConnectsPerSecond`|Nombre de connexions actives en cours d'extraction du regroupement de connexions. **Remarque :**  Ce compteur de performance n’est pas activé par défaut. Pour activer ce compteur de performance, voir [Activation des compteurs désactivés par défaut](#ActivatingOffByDefault).|  
|`SoftDisconnectsPerSecond`|Nombre de connexions actives retournées au regroupement de connexions. **Remarque :**  Ce compteur de performance n’est pas activé par défaut. Pour activer ce compteur de performance, voir [Activation des compteurs désactivés par défaut](#ActivatingOffByDefault).|  

### <a name="connection-pool-groups-and-connection-pools"></a>Groupes de pools de connexions et pools de connexions

Lorsque vous utilisez l'authentification Windows (sécurité intégrée), vous devez surveiller les deux compteurs de performance `NumberOfActiveConnectionPoolGroups` et `NumberOfActiveConnectionPools`. En effet, les groupes du regroupement de connexions sont mappés à des chaînes de connexion uniques. Les regroupements de connexions sont mappés aux chaînes de connexion et créent des regroupements séparés pour des identités Windows individuelles, lors de l'utilisation de la sécurité intégrée. Par exemple, si Fred et Julie, qui appartiennent au même AppDomain, utilisent tous les deux la chaîne de connexion `"Data Source=MySqlServer;Integrated Security=true"`, un groupe de regroupement de connexions est créé pour la chaîne de connexion, et deux autres regroupements sont créés, un pour Fred et un pour Julie. Si Jean et Martha utilisent une chaîne de connexion avec une connexion SQL Server identique, `"Data Source=MySqlServer;User Id=<myUserID>;Password=<myPassword>"`, un regroupement unique est alors créé pour l’identité **<myUserID>** .

<a name="ActivatingOffByDefault"></a>

### <a name="activate-off-by-default-counters"></a>Activer les compteurs désactivés par défaut

Les compteurs de performance `NumberOfFreeConnections`, `NumberOfActiveConnections`, `SoftDisconnectsPerSecond` et `SoftConnectsPerSecond` sont désactivés par défaut. Ajoutez les informations suivantes dans le fichier de configuration de l'application pour les activer :

```xml  
<system.diagnostics>  
  <switches>  
    <add name="ConnectionPoolPerformanceCounterDetail"  
         value="4"/>  
  </switches>  
</system.diagnostics>  
```  

## <a name="retrieve-performance-counter-values"></a>Récupérer les valeurs des compteurs de performances

L'application console suivante montre comment récupérer les valeurs de compteur de performance dans votre application. Les connexions doivent être ouvertes et actives pour que les informations soient retournées pour l’ensemble des compteurs de performances du fournisseur de données Microsoft SqlClient pour SQL Server.

> [!NOTE]
> Cet exemple utilise [la base de données d’exemple **AdventureWorks**](../../samples/adventureworks-install-configure.md). Les chaînes de connexion fournies dans l’exemple de code supposent que la base de données est installée et disponible sur l’ordinateur local, et que vous avez créé des connexions qui correspondent à celles fournies dans les chaînes de connexion. Vous devrez peut-être activer les connexions SQL Server si votre serveur est configuré à l'aide des paramètres de sécurité par défaut qui autorisent uniquement l'authentification Windows. Modifiez les chaînes de connexion en fonction de votre environnement.

### <a name="example"></a>Exemple

[!code-csharp[SqlClient_PerformanceCounter#1](~/../sqlclient/doc/samples/SqlClient_PerformanceCounter.cs#1)]

## <a name="see-also"></a>Voir aussi

- [Connexion à une source de données](connecting-to-data-source.md)
- [Profilage de runtime](/dotnet/framework/debug-trace-profile/runtime-profiling)
- [Présentation de l’analyse des seuils de performance](/previous-versions/visualstudio/visual-studio-2008/bd20x32d(v=vs.90))
- [Microsoft ADO.NET pour SQL Server](microsoft-ado-net-sql-server.md)
