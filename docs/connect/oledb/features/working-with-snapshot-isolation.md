---
title: Utilisation de l’isolement de capture instantanée | Microsoft Docs
description: Découvrez comment les améliorations apportées à OLE DB Driver pour SQL Server utilisent l’isolement de capture instantanée, qui améliore la concurrence pour les applications de traitement transactionnel en ligne.
ms.custom: ''
ms.date: 06/12/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- data access [OLE DB Driver for SQL Server], snapshot isolation
- MSOLEDBSQL, snapshot isolation
- isolation levels [SQL Server], snapshot
- DBPROPSET_SESSION property set
- DBDROPSET_DATASOURCEINFO property set
- snapshot isolation [OLE DB Driver for SQL Server]
- OLE DB Driver for SQL Server, snapshot isolation
- SQLGetInfo function
- concurrency [OLE DB Driver for SQL Server]
- SQLSetConnectAttr function
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 700ce06359188f05c7f3e152003dfa9dde06da76
ms.sourcegitcommit: c95f3ef5734dec753de09e07752a5d15884125e2
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/25/2020
ms.locfileid: "88861423"
---
# <a name="working-with-snapshot-isolation"></a>Utilisation de l’isolement de capture instantanée
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

[!INCLUDE[Driver_OLEDB_Download](../../../includes/driver_oledb_download.md)]

  Dans [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)], un nouveau niveau d'isolement d'« instantané » a été introduit pour améliorer la concurrence pour les applications de traitement transactionnel en ligne (OLTP). Dans les versions antérieures à [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], la concurrence était uniquement basée sur le verrouillage, ce qui pouvait provoquer des problèmes de blocage pour certaines applications. Le niveau d'isolement d'instantané, qui dépend des améliorations apportées au contrôle de version de ligne, est conçu pour améliorer les performances en évitant les scénarios de blocage du lecteur/enregistreur.  
  
 Les transactions qui démarrent avec un niveau d'isolement d'instantané lisent un instantané de base de données au moment du démarrage de la transaction. Quand vous ouvrez des curseurs côté serveur statiques, dynamiques et de jeu de clés dans un contexte de transaction d’instantané, ceux-ci se comportent de manière comparable à des curseurs statiques ouverts dans des transactions sérialisables. Toutefois, lorsque les curseurs sont ouverts avec un niveau d'isolement d'instantané, les verrous ne sont pas pris, ce qui peut réduire le blocage sur le serveur.  
  
## <a name="ole-db-driver-for-sql-server"></a>OLE DB Driver pour SQL Server  
 OLE DB Driver pour SQL Server offre des améliorations qui tirent parti de l'isolement d'instantané apparu dans [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)]. Ces améliorations incluent des modifications aux jeux de propriétés DBPROPSET_DATASOURCEINFO et DBPROPSET_SESSION.  
  
### <a name="dbpropset_datasourceinfo"></a>DBPROPSET_DATASOURCEINFO  
 Le jeu de propriétés DBPROPSET_DATASOURCEINFO a été modifié pour indiquer que le niveau d'isolement d'instantané est pris en charge par l'ajout de la valeur DBPROPVAL_TI_SNAPSHOT utilisée dans la propriété DBPROP_SUPPORTEDTXNISOLEVELS. Cette nouvelle valeur indique que le niveau d'isolement d'instantané est pris en charge, que le contrôle de version ait été activé sur la base de données ou non. Le tableau suivant répertorie les valeurs DBPROP_SUPPORTEDTXNISOLEVELS :  
  
|ID de propriété|Description|  
|-----------------|-----------------|  
|DBPROP_SUPPORTEDTXNISOLEVELS|Type : VT_I4<br /><br /> Lecture/écriture : lecture seule<br /><br /> Description : masque de bits spécifiant les niveaux d'isolation de la transaction pris en charge. Combinaison de zéro ou plusieurs des éléments suivants :<br /><br /> DBPROPVAL_TI_CHAOS<br /><br /> DBPROPVAL_TI_READUNCOMMITTED<br /><br /> DBPROPVAL_TI_BROWSE<br /><br /> DBPROPVAL_TI_CURSORSTABILITY<br /><br /> DBPROPVAL_TI_READCOMMITTED<br /><br /> DBPROPVAL_TI_REPEATABLEREAD<br /><br /> DBPROPVAL_TI_SERIALIZABLE<br /><br /> DBPROPVAL_TI_ISOLATED<br /><br /> DBPROPVAL_TI_SNAPSHOT|  
  
### <a name="dbpropset_session"></a>DBPROPSET_SESSION  
 Le jeu de propriétés DBPROPSET_SESSION a été modifié pour indiquer que le niveau d'isolement d'instantané est pris en charge par l'ajout de la valeur DBPROPVAL_TI_SNAPSHOT utilisée dans la propriété DBPROP_SESS_AUTOCOMMITISOLEVELS. Cette nouvelle valeur indique que le niveau d'isolement d'instantané est pris en charge, que le contrôle de version ait été activé sur la base de données ou non. Le tableau suivant répertorie les valeurs DBPROP_SESS_AUTOCOMMITISOLEVELS :
  
|ID de propriété|Description|  
|-----------------|-----------------|  
|DBPROP_SESS_AUTOCOMMITISOLEVELS|Type : VT_I4<br /><br /> Lecture/écriture : lecture seule<br /><br /> Description : spécifie un masque de bits qui indique le niveau d'isolation de la transaction lorsqu'elle se trouve en mode de validation automatique. Les valeurs pouvant être définies dans ce masque de bits sont les mêmes que celles qui peuvent être définies pour DBPROP_SUPPORTEDTXNISOLEVELS.|  
  
> [!NOTE]  
>  Les erreurs DB_S_ERRORSOCCURRED ou DB_E_ERRORSOCCURRED se produisent si DBPROPVAL_TI_SNAPSHOT est défini dans le cadre de l'utilisation de versions de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] antérieures à [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)].  
  
 Pour plus d'informations sur la prise en charge du niveau d'isolement d'instantané dans les transactions, consultez [Prise en charge des transactions locales](../../oledb/ole-db-transactions/supporting-local-transactions.md).  

  
## <a name="see-also"></a>Voir aussi  
 [Fonctionnalités OLE DB Driver pour SQL Server](../../oledb/features/oledb-driver-for-sql-server-features.md)    
 [Propriétés et comportements des ensembles de lignes](../../oledb/ole-db-rowsets/rowset-properties-and-behaviors.md)  
  
  
