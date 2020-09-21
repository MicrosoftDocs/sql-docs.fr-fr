---
title: Accès aux informations de diagnostic dans le journal des événements étendus | Microsoft Docs
description: Suivi OLE DB Driver pour SQL Server et accès aux informations de diagnostic dans le journal des événements étendus
ms.custom: ''
ms.date: 06/12/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 28b07fa4befbae597fd3356d5abe83cf1631763e
ms.sourcegitcommit: 827ad02375793090fa8fee63cc372d130f11393f
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/04/2020
ms.locfileid: "89480937"
---
# <a name="accessing-diagnostic-information-in-the-extended-events-log"></a>Accès aux informations de diagnostic dans le journal des événements étendus
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

[!INCLUDE[Driver_OLEDB_Download](../../../includes/driver_oledb_download.md)]

  À compter de [!INCLUDE[ssSQL11](../../../includes/sssql11-md.md)], OLE DB Driver pour SQL Server et le traçage de l’accès aux données ([Traçage de l’accès aux données](https://go.microsoft.com/fwlink/?LinkId=125805)) ont été mis à jour pour faciliter l’obtention d’informations de diagnostic sur les échecs de connexion à partir de la mémoire tampon en anneau de connectivité, ainsi que d’informations de performance d’application depuis le journal des événements étendus.  
  
 Pour plus d’informations sur la lecture du journal des événements étendus, consultez [Afficher des données de session d’événements](../../../relational-databases/extended-events/advanced-viewing-of-target-data-from-extended-events-in-sql-server.md). 

  
> [!NOTE]  
>  Cette fonctionnalité n'est conçue qu'à des fins de dépannage et de diagnostic et peut ne pas convenir à des fins d'audit ou de sécurité.  
  
## <a name="remarks"></a>Notes  
 Pour les opérations de connexion, OLE DB Driver pour SQL Server envoie un ID de connexion client. En cas d’échec de la connexion, vous pouvez accéder à la mémoire tampon en anneau de connectivité ([Résolution des problèmes de connectivité dans SQL Server 2008 avec la mémoire tampon en anneau de connectivité](https://docs.microsoft.com/archive/blogs/sql_protocols/connectivity-troubleshooting-in-sql-server-2008-with-the-connectivity-ring-buffer)) et rechercher le champ **ClientConnectionID** pour obtenir les informations de diagnostic sur l’échec de la connexion. Les ID de connexion du client sont enregistrés dans la mémoire tampon en anneau uniquement en cas d'erreur. (Si une connexion échoue avant d'envoyer le paquet de préconnexion, un ID de connexion client ne sera pas généré.) L'ID de connexion client est un GUID à 16 octets. Vous pouvez également rechercher l’ID de connexion client dans la cible de sortie d’événements étendus, si l’action **client_connection_id** est ajoutée aux événements dans une session d’événements étendus. Vous pouvez activer le traçage de l’accès aux données et réexécuter la commande de connexion, puis observer le champ **ClientConnectionID** dans la trace d’accès aux données pour une opération ayant échoué, si vous avez besoin d’une aide supplémentaire pour le diagnostic.  
   
  
 OLE DB Driver pour SQL Server envoie également un ID d'activité spécifique au thread. L'ID d'activité est capturé dans les sessions d'événements étendus si les sessions sont démarrées alors que l'option TRACK_CAUSAILITY est activée. En cas de problèmes de performance avec une connexion active, vous pouvez obtenir l’ID d’activité de la trace d’accès aux données du client (champ **ActivityID**), puis le localiser dans la sortie d’événements étendus. L'ID d'activité dans les événements étendus est un GUID à 16 octets (différent du GUID de l'ID de connexion client) ajouté avec un numéro de séquence de quatre octets. Le numéro séquentiel représente l'ordre d'une demande dans un thread et indique l'ordre relatif du traitement par lot et des instructions RPC pour le thread. **ActivityID** est éventuellement envoyé pour les instructions par lots SQL et les demandes RPC quand le traçage de l’accès aux données est activé et que le 18ème bit dans le mot de configuration de traçage est activé.  
  
 Voici un exemple qui utilise [!INCLUDE[tsql](../../../includes/tsql-md.md)] pour démarrer une session d'événements étendus qui sera stockée dans une mémoire tampon en anneau et enregistrera l'ID d'activité envoyé d'un client sur des opérations de traitement par lot et RPC.  
  
```  
create event session MySession on server   
add event connectivity_ring_buffer_recorded,   
add event sql_statement_starting (action (client_connection_id)),   
add event sql_statement_completed (action (client_connection_id)),   
add event rpc_starting (action (client_connection_id)),   
add event rpc_completed (action (client_connection_id))  
add target ring_buffer with (track_causality=on)  
  
```  
  
## <a name="control-file"></a>Fichier de contrôle  
 Le contenu du fichier de contrôle OLE DB Driver pour SQL Server (ctrl.guid) est :  
  
```  
{8B98D3F2-3CC6-0B9C-6651-9649CCE5C752}  0x630ff  0   MSDADIAG.ETW
{EE7FB59C-D3E8-9684-AEAC-B214EFD91B31}  0x630ff  0   MSOLEDBSQL.1  
```  
  
## <a name="mof-file"></a>Fichier MOF  
 Le contenu du fichier MOF OLE DB Driver pour SQL Server est :  
  
```  
#pragma classflags("forceupdate")  
#pragma namespace ("\\\\.\\Root\\WMI")  
  
/////////////////////////////////////////////////////////////////////////////  
//  
//  MSDADIAG.ETW  
  
[  
 dynamic: ToInstance,  
 Description("MSDADIAG.ETW"),  
 Guid("{8B98D3F2-3CC6-0B9C-6651-9649CCE5C752}"),  
 locale("MS\\0x409")  
]  
class Bid2Etw_MSDADIAG_ETW : EventTrace  
{  
};  
  
[  
 dynamic: ToInstance,  
 Description("MSDADIAG.ETW"),  
 Guid("{8B98D3F3-3CC6-0B9C-6651-9649CCE5C752}"),  
 DisplayName("msdadiag"),  
 locale("MS\\0x409")  
]  
class Bid2Etw_MSDADIAG_ETW_Trace : Bid2Etw_MSDADIAG_ETW  
{  
};  
  
[  
 dynamic: ToInstance,  
 Description("MSDADIAG.ETW formatted output (A)"),  
 EventType(17),  
 EventTypeName("TextA"),  
 locale("MS\\0x409")  
]  
class Bid2Etw_MSDADIAG_ETW_Trace_TextA : Bid2Etw_MSDADIAG_ETW_Trace  
{  
    [  
     WmiDataId(1),  
     Description("Module ID"),  
     read  
    ]  
    uint32 ModID;  
  
    [  
     WmiDataId(2),  
     Description("Text StringA"),  
     extension("RString"),  
     read  
    ]  
    object msgStr;  
};  
  
[  
 dynamic: ToInstance,  
 Description("MSDADIAG.ETW formatted output (W)"),  
 EventType(18),  
 EventTypeName("TextW"),  
 locale("MS\\0x409")  
]  
class Bid2Etw_MSDADIAG_ETW_Trace_TextW : Bid2Etw_MSDADIAG_ETW_Trace  
{  
    [  
     WmiDataId(1),  
     Description("Module ID"),  
     read  
    ]  
    uint32 ModID;  
  
    [  
     WmiDataId(2),  
     Description("Text StringW"),  
     extension("RWString"),  
     read  
    ]  
    object msgStr;  
};  
  
/////////////////////////////////////////////////////////////////////////////  
//  
//  MSOLEDBSQL.1  
  
[  
 dynamic: ToInstance,  
 Description("MSOLEDBSQL.1"),  
 Guid("{EE7FB59C-D3E8-9684-AEAC-B214EFD91B31}"),  
 locale("MS\\0x409")  
]  
class Bid2Etw_MSOLEDBSQL_1 : EventTrace  
{  
};  
  
[  
 dynamic: ToInstance,  
 Description("MSOLEDBSQL.1"),  
 Guid("{EE7FB59D-D3E8-9684-AEAC-B214EFD91B31}"),  
 DisplayName("MSOLEDBSQL.1"),  
 locale("MS\\0x409")  
]  
class Bid2Etw_MSOLEDBSQL_1_Trace : Bid2Etw_MSOLEDBSQL_1  
{  
};  
  
[  
 dynamic: ToInstance,  
 Description("MSOLEDBSQL.1 formatted output (A)"),  
 EventType(17),  
 EventTypeName("TextA"),  
 locale("MS\\0x409")  
]  
class Bid2Etw_MSOLEDBSQL_1_Trace_TextA : Bid2Etw_MSOLEDBSQL_1_Trace  
{  
    [  
     WmiDataId(1),  
     Description("Module ID"),  
     read  
    ]  
    uint32 ModID;  
  
    [  
     WmiDataId(2),  
     Description("Text StringA"),  
     extension("RString"),  
     read  
    ]  
    object msgStr;  
};  
  
[  
 dynamic: ToInstance,  
 Description("MSOLEDBSQL.1 formatted output (W)"),  
 EventType(18),  
 EventTypeName("TextW"),  
 locale("MS\\0x409")  
]  
class Bid2Etw_MSOLEDBSQL_1_Trace_TextW : Bid2Etw_MSOLEDBSQL_1_Trace  
{  
    [  
     WmiDataId(1),  
     Description("Module ID"),  
     read  
    ]  
    uint32 ModID;  
  
    [  
     WmiDataId(2),  
     Description("Text StringW"),  
     extension("RWString"),  
     read  
    ]  
    object msgStr;  
};  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Gestion des erreurs](../../oledb/ole-db-errors/errors.md)  
  
  
