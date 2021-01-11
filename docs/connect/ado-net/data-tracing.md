---
title: Suivi de données dans SqlClient
description: Décrit comment le fournisseur de données Microsoft SqlClient pour SQL Server fournit des fonctionnalités de traçage de données intégrées.
ms.date: 12/04/2020
ms.assetid: a6a752a5-d2a9-4335-a382-b58690ccb79f
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
ms.reviewer: v-chmalh
ms.openlocfilehash: fc8b5a7ca06af3c3e3ea83fcb747f79517e5ef7a
ms.sourcegitcommit: 4419e99d77ee2c73f9da1559c7944f7702f2de30
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/23/2020
ms.locfileid: "97744383"
---
# <a name="data-tracing-in-sqlclient"></a>Suivi de données dans SqlClient

[!INCLUDE[appliesto-netfx-netcore-netst-md](../../includes/appliesto-netfx-netcore-netst-md.md)]

[!INCLUDE[Driver_ADONET_Download](../../includes/driver_adonet_download.md)]

.NET intègre une fonctionnalité de traçage de données qui est prise en charge par le fournisseur de données Microsoft SqlClient pour SQL Server et les protocoles réseau SQL Server.

Le traçage d'appels API d'accès aux données peut aider à diagnostiquer les problèmes suivants :

- discordance de schéma entre le programme client et la base de données ;

- problèmes d'indisponibilité de base de données ou de bibliothèque réseau ;

- SQL incorrect qu'il soit en codage dur ou généré par une application ;

- logique de programmation incorrecte ;

- Problèmes résultant de l’interaction entre le fournisseur de données Microsoft SqlClient pour SQL Server et vos propres composants.

Pour prendre en charge plusieurs technologies de traçage, le traçage est extensible, de sorte qu'un développeur peut tracer un problème à tout niveau de la pile d'applications. Le fournisseur de données Microsoft SqlClient pour SQL Server tire parti des API de suivi et d’instrumentation généralisées.

Pour plus d’informations sur la définition et la configuration du traçage managé dans .NET, consultez [Tracing Data Access](/previous-versions/sql/sql-server-2012/hh880086(v=msdn.10)) (en anglais).

## <a name="access-diagnostic-information-in-the-extended-events-log"></a>Accès aux informations de diagnostic dans le journal des événements étendus

Dans le fournisseur de données Microsoft SqlClient Data Provider pour SQL Server, le [suivi d’accès aux données](/previous-versions/sql/sql-server-2012/hh880086(v=msdn.10)) facilite la mise en corrélation des événements clients avec les informations de diagnostic, telles que les échecs de connexion, les informations de performance des applications et de la mémoire tampon en anneau de connectivité du serveur dans le journal des événements étendus. Pour plus d’informations sur la lecture du journal des événements étendus, consultez [Afficher des données de session d’événements](/previous-versions/sql/sql-server-2012/hh710068(v=sql.110)).

Pour les opérations de connexion, le fournisseur de données Microsoft SqlClient pour SQL Server enverra un ID de connexion client. Si la connexion échoue, vous pouvez accéder à la mémoire tampon de l’anneau de connectivité ([Résolution des problèmes de connectivité dans SQL Server 2008 avec la mémoire tampon de l’anneau de connectivité](/archive/blogs/sql_protocols/connectivity-troubleshooting-in-sql-server-2008-with-the-connectivity-ring-buffer)) et consulter le champ `ClientConnectionID` pour obtenir les informations de diagnostic sur l’échec de connexion. Les ID de connexion du client sont enregistrés dans la mémoire tampon en anneau uniquement en cas d'erreur. Si une connexion échoue avant d’envoyer le paquet de préconnexion, un ID de connexion client ne sera pas généré. L'ID de connexion client est un GUID à 16 octets. Vous pouvez également rechercher l'ID de connexion client dans la sortie cible d'événements étendus, si l'action `client_connection_id` est ajoutée aux événements dans une session d'événements étendus. Vous pouvez activer le traçage d'accès aux données, réexécuter la commande de connexion et observer le champ `ClientConnectionID` dans la trace d'accès aux données, si vous avez besoin d'obtenir une aide supplémentaire pour le diagnostic du pilote client.

Vous pouvez obtenir l'ID de connexion client par programme à l'aide de la propriété `SqlConnection.ClientConnectionID`.

> [!NOTE]
> Le fournisseur de données Microsoft SqlClient pour SQL Server prend en charge l’ID de processus serveur depuis la version 2.1.0. Vous pouvez l’utiliser par programmation à l’aide de la propriété `SqlConnection.ServerProcessId`.

`ClientConnectionID` et `ServerProcessId` sont disponibles pour un objet <xref:Microsoft.Data.SqlClient.SqlConnection> qui établit une connexion avec succès. Si une tentative de connexion échoue, `ClientConnectionID` peut être disponible via `SqlException.ToString`.

Le fournisseur de données Microsoft SqlClient pour SQL Server envoie également un ID d’activité spécifique au thread. L’ID d’activité est capturé dans les sessions d’événements étendus si les sessions sont démarrées alors que l’option TRACK_CAUSALITY est activée. En cas de problèmes de performances avec une connexion active, vous pouvez obtenir l'ID d'activité de la trace d'accès aux données du client (champ `ActivityID`), puis rechercher l'ID d'activité dans la sortie des événements étendus. L’ID d’activité dans les événements étendus est un GUID sur 16 octets (différent du GUID de l’ID de connexion cliente) suivi d’un numéro séquentiel sur 4 octets. Le numéro séquentiel représente l'ordre d'une demande dans un thread et indique l'ordre relatif du traitement par lot et des instructions RPC pour le thread. `ActivityID` est actuellement éventuellement envoyé pour les instructions par lots SQL et les demandes RPC lorsque le suivi de l'accès aux données est activé et le 18e bit dans le mot de configuration de suivi d'accès aux données est activé.

Voici un exemple d’instruction SQL qui utilise Transact-SQL pour démarrer une session d’événements étendus qui sera stockée dans une mémoire tampon en anneau et enregistrera l’ID d’activité envoyé d’un client sur des opérations de traitement par lot et RPC.

```sql
create event session MySession on server
add event connectivity_ring_buffer_recorded,
add event sql_statement_starting (action (client_connection_id)),
add event sql_statement_completed (action (client_connection_id)),
add event rpc_starting (action (client_connection_id)),
add event rpc_completed (action (client_connection_id))
add target ring_buffer with (track_causality=on)
```

## <a name="see-also"></a>Voir aussi
- [Microsoft ADO.NET pour SQL Server](microsoft-ado-net-sql-server.md)
