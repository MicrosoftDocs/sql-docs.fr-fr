---
title: Accès aux informations de diagnostic dans le journal des événements étendus
description: Découvrez comment accéder sur le serveur à des événements étendus liés à des événements de Microsoft JDBC Driver pour SQL Server.
ms.custom: ''
ms.date: 05/06/2020
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
ms.assetid: a79e9468-2257-4536-91f1-73b008c376c3
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 6d9b75ea8c722ca753e831811226b8128df15266
ms.sourcegitcommit: c7f40918dc3ecdb0ed2ef5c237a3996cb4cd268d
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/05/2020
ms.locfileid: "91725505"
---
# <a name="accessing-diagnostic-information-in-the-extended-events-log"></a>Accès aux informations de diagnostic dans le journal des événements étendus
[!INCLUDE[Driver_JDBC_Download](../../includes/driver_jdbc_download.md)]

  Dans [!INCLUDE[jdbc_40](../../includes/jdbc_40_md.md)], le suivi ([Suivi du fonctionnement du pilote](../../connect/jdbc/tracing-driver-operation.md)) a été mis à jour pour faciliter la corrélation des événements clients avec les informations de diagnostic, comme les échecs de connexion, à partir de la mémoire tampon en anneau de connectivité du serveur et des informations relatives aux performances dans le journal des événements étendus. Pour plus d'informations sur la lecture du journal des événements étendus, consultez [Événements étendus](../../relational-databases/extended-events/extended-events.md).  
  
## <a name="details"></a>Détails  
 Pour les opérations de connexion, [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)] envoie un ID de connexion cliente. Si la connexion échoue, vous pouvez accéder à la mémoire tampon en anneau de connectivité ([Résolution des problèmes de connectivité dans SQL Server 2008 avec la mémoire tampon en anneau de connectivité](/archive/blogs/sql_protocols/connectivity-troubleshooting-in-sql-server-2008-with-the-connectivity-ring-buffer)) et rechercher le champ **ClientConnectionID** pour obtenir les informations de diagnostic sur l’échec de connexion. Les ID de connexion du client sont enregistrés dans la mémoire tampon en anneau uniquement en cas d'erreur. (Si une connexion échoue avant d’envoyer le paquet de préconnexion, un ID de connexion client n’est pas généré.) L'ID de connexion client est un GUID à 16 octets. Vous pouvez également rechercher l’ID de connexion cliente dans la sortie de la cible des événements étendus, si l’action **client_connection_id** est ajoutée aux événements dans une session d’événements étendus. Si vous avez besoin d’une assistance supplémentaire pour diagnostiquer le pilote client, vous pouvez activer le suivi et réexécuter la commande de connexion, puis observer le champ **ClientConnectionID** dans la trace.  
  
 Vous pouvez obtenir l'ID de connexion du client programmatiquement avec l’[interface ISQLServerConnection](../../connect/jdbc/reference/isqlserverconnection-interface.md). L'ID de connexion sera également présent dans toutes les exceptions liées à la connexion.  
  
 En cas d’erreur de connexion, l’ID de connexion client dans les informations de trace des diagnostics intégrés du serveur et dans la mémoire tampon en anneau de connectivité peut aider à corréler les connexions clientes aux connexions sur le serveur. Pour plus d’informations sur les traces des diagnostics intégrés, consultez [Traçage de l’accès aux données](/previous-versions/sql/sql-server-2008/cc765421(v=sql.100)). Notez que cet article contient également des informations sur la trace des accès aux données, qui ne s'applique pas à [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)] ; consultez [Suivi du fonctionnement du pilote](../../connect/jdbc/tracing-driver-operation.md) pour plus d'informations sur la création d'une trace d'accès aux données avec [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)].  
  
 Le pilote JDBC envoie également un ID d'activité spécifique au thread. L'ID d'activité est capturé dans les sessions d'événements étendus si les sessions sont démarrées alors que l'option TRACK_CAUSAILITY est activée. En cas de problèmes de performance avec une connexion active, vous pouvez obtenir l'ID d'activité de la trace du client (champ ActivityID), puis le localiser dans la sortie d'événements étendus. L’ID d’activité dans les événements étendus est un GUID sur 16 octets (différent du GUID de l’ID de connexion cliente) suivi d’un numéro séquentiel sur 4 octets. Le numéro séquentiel représente l'ordre d'une demande dans un thread. L'ActivityId est envoyé pour les instructions de lot SQL et les requêtes RPC. Pour activer l'envoi d'ActivityId au serveur, vous devez d'abord spécifier la paire de valeur/clé suivante dans le fichier Logging.Properties :  
  
```
com.microsoft.sqlserver.jdbc.traceactivity = on  
```  
  
 Toute valeur autre que `on` (valeur respectant la casse) désactive l’envoi de l’ActivityId.  
  
 Pour plus d'informations, consultez [Suivi du fonctionnement du pilote](../../connect/jdbc/tracing-driver-operation.md). L'indicateur de trace est utilisé avec les enregistreurs d'objets JDBC correspondants pour décider s'il faut tracer et envoyer l'ActivityId dans le pilote JDBC. En plus de la mise à jour du fichier Logging.Properties, l'enregistreur com.microsoft.sqlserver.jdbc doit être activé avec le niveau FINER ou plus élevé. Si vous voulez envoyer ActivityId au serveur pour des requêtes effectuées par une classe donnée, l’enregistreur de classe correspondant doit être activé avec le niveau FINER ou FINEST. Par exemple, si la classe est SQLServerStatement, activez l'enregistreur com.microsoft.sqlserver.jdbc.SQLServerStatement.  
  
 L’exemple suivant utilise [!INCLUDE[tsql](../../includes/tsql-md.md)] pour démarrer une session d’événements étendus qui est stockée dans une mémoire tampon en anneau et enregistre l’ID d’activité envoyé par un client lors des opérations par lot et RPC :  
  
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

[Diagnostic de problèmes avec le pilote JDBC](../../connect/jdbc/diagnosing-problems-with-the-jdbc-driver.md)