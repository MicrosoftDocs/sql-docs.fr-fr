---
title: Activer le suivi d’événements dans SqlClient
description: Explique comment activer le suivi d’événements dans SqlClient en implémentant un détecteur d’événements et comment accéder aux données des événements.
ms.date: 11/23/2020
dev_langs:
- csharp
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: johnnypham
ms.author: v-jopha
ms.reviewer: ''
ms.openlocfilehash: 1bcf11727a5ccd640846788b936ff5fefe9c63a3
ms.sourcegitcommit: d8cdbb719916805037a9167ac4e964abb89c3909
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/20/2021
ms.locfileid: "98595680"
---
# <a name="enable-event-tracing-in-sqlclient"></a>Activer le suivi d’événements dans SqlClient

[!INCLUDE [appliesto-netfx-netcore-netst-md](../../includes/appliesto-netfx-netcore-netst-md.md)]

[!INCLUDE [Driver_ADONET_Download](../../includes/driver_adonet_download.md)]

Le [Suivi d’événements pour Windows (ETW)](/windows/win32/etw/event-tracing-portal) est une fonctionnalité de traçage efficace au niveau du noyau qui permet de consigner les événements définis par le pilote à des fins de débogage et de test. SqlClient prend en charge la capture des événements ETW à différents niveaux d’information. Pour capturer le suivi d’événements, les applications clientes doivent détecter les événements dans l’implémentation EventSource de SqlClient :

```
Microsoft.Data.SqlClient.EventSource
```

L’implémentation actuelle prend en charge les mots clés d’événements suivants :

| Nom du mot clé | Valeur | Description |
| ------------ | ----- | ----------- |
| ExecutionTrace | 1 | Active la capture des événements de démarrage/arrêt avant et après l’exécution de la commande. |
| Trace | 2 | Active la capture des événements de suivi de flux d’application de base. |
| Étendue | 4 | Active la capture des événements d’entrée et de sortie. |
| NotificationTrace | 8 | Active la capture des événements de suivi `SqlNotification`. |
| NotificationScope | 16 | Active la capture des événements d’entrée et de sortie de portée `SqlNotification`. |
| PoolerTrace | 32 | Active la capture des événements de suivi de flux de regroupement de connexions. |
| PoolerScope | 64 | Active la capture des événements de suivi de portée de regroupement de connexions. |
| AdvancedTrace | 128 | Active la capture des événements de suivi de flux avancés. |
| AdvancedTraceBin  | 256 | Active la capture des événements de suivi de flux avancés avec des informations supplémentaires. |
| CorrelationTrace | 512 | Active la capture des événements de suivi de flux de corrélation. |
| StateDump | 1 024 | Active la capture du vidage de l’état complet de `SqlConnection`. |
| SNITrace | 2 048 | Active la capture des événements de suivi de flux à partir de l’implémentation réseau gérée (s’applique uniquement à .NET Core). |
| SNIScope | 4096 | Active la capture des événements de portée à partir de l’implémentation réseau gérée (s’applique uniquement à .NET Core). |
|||

## <a name="example"></a>Exemple
L’exemple suivant active le suivi d’événements pour une opération de données sur l’exemple de base de données **AdventureWorks** et affiche les événements dans la fenêtre de console.

[!code-csharp [SqlClientEventSource#1](~/../sqlclient/doc/samples/SqlClientEventSource.cs#1)]

## <a name="event-tracing-support-in-native-sni"></a>Support du suivi des événements dans Native SNI

**Microsoft.Data.SqlClient** v2.1.0 étend le support du suivi d’événements dans **Microsoft.Data.SqlClient.SNI** et **Microsoft.Data.SqlClient.SNI.runtime**. En envoyant un EventCommand à `SqlClientEventSource`, les événements du fichier SNI.dll natif peuvent être collectés à l’aide des outils [Xperf](/windows-hardware/test/wpt/) et [PerfView](https://github.com/microsoft/perfview). Les valeurs EventCommand valides sont répertoriées ci-dessous :

```cs
// Enables trace events:
EventSource.SendCommand(eventSource, (EventCommand)8192, null);

// Enables flow events:
EventSource.SendCommand(eventSource, (EventCommand)16384, null);

// Enables both trace and flow events:
EventSource.SendCommand(eventSource, (EventCommand)(8192 | 16384), null);
```

L’exemple suivant active le suivi d’événements dans le fichier SNI.dll natif lorsque l’application cible .NET Framework. 

```cs
// Native SNI tracing example
// .NET Framework application
using System;
using System.Diagnostics.Tracing;
using Microsoft.Data.SqlClient;

public class SqlClientListener : EventListener
{
    protected override void OnEventSourceCreated(EventSource eventSource)
    {
        if (eventSource.Name.Equals("Microsoft.Data.SqlClient.EventSource"))
        {
            // Enables both trace and flow events
            EventSource.SendCommand(eventSource, (EventCommand)(8192 | 16384), null);
        }
    }
}

class Program
{
    static string connectionString = @"Data Source = localhost; Initial Catalog = AdventureWorks;Integrated Security=true;";

    static void Main(string[] args)
    {
        using (SqlClientListener listener = new SqlClientListener())
        using (SqlConnection connection = new SqlConnection(connectionString))
        {
            connection.Open();
        }        
    }
}
```

### <a name="use-xperf-to-collect-trace-log"></a>Utiliser Xperf pour collecter le journal de suivi

1. Démarrez le suivi en utilisant la ligne de commande suivante.

   ```
   xperf -start trace -f myTrace.etl -on *Microsoft.Data.SqlClient.EventSource
   ```
   
2. Exécutez l’exemple de traçage SNI natif pour vous connecter à SQL Server.

3. Arrêtez le suivi en utilisant la ligne de commande suivante.

   ```
   xperf -stop trace
   ```
   
4. Utilisez PerfView pour ouvrir le fichier myTrace.etl spécifié à l’étape 1. Le journal de suivi SNI peut être trouvé avec les noms d’événements `Microsoft.Data.SqlClient.EventSource/SNIScope` et `Microsoft.Data.SqlClient.EventSource/SNITrace`. 

   ![Utiliser PerfView pour afficher le fichier de trace SNI](media/view-event-trace-native-sni.png)


### <a name="use-perfview-to-collect-trace-log"></a>Utiliser PerfView pour collecter le journal de suivi

1. Démarrez PerfView et exécutez `Collect > Collect` à partir de la barre de menus.

2. Configurez le nom du fichier de trace, le chemin de sortie et le nom du fournisseur.

   ![Configurer Prefview avant le regroupement](media/collect-event-trace-native-sni.png)
   
3. Démarrez le regroupement.

4. Exécutez l’exemple de traçage SNI natif pour vous connecter à SQL Server.

5. Arrêtez le regroupement à partir de PerfView. La génération du fichier PerfViewData.etl en fonction de la configuration à l’étape 2 prendra un certain temps.

6. Ouvrez le fichier etl dans PerfView. Le journal de suivi SNI peut être trouvé avec les noms d’événements `Microsoft.Data.SqlClient.EventSource/SNIScope` et `Microsoft.Data.SqlClient.EventSource/SNITrace`. 


## <a name="external-resources"></a>Ressources externes  
Pour plus d'informations, consultez les ressources ci-dessous.  
  
|Ressource|Description|  
|--------------|-----------------|  
|[EventSource Class](/dotnet/api/system.diagnostics.tracing.eventsource)|Offre la possibilité de créer des événements ETW.| 
|[Classe EventListener](/dotnet/api/system.diagnostics.tracing.eventlistener)|Fournit des méthodes permettant d’activer et de désactiver des événements à partir des sources d’événements.|