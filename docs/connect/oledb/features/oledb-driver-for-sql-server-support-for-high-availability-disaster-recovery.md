---
title: Prise en charge de la récupération d’urgence et de la haute disponibilité par OLE DB Driver pour SQL Server
description: En savoir plus sur la prise en charge d’OLE DB Driver pour SQL Server pour la haute disponibilité et la récupération d’urgence lors de la connexion à des bases de données avec ces fonctionnalités configurées.
ms.custom: ''
ms.date: 06/12/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: d5adce5bcda14dd275791f055359654bc120e79d
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100347905"
---
# <a name="ole-db-driver-for-sql-server-support-for-high-availability-disaster-recovery"></a>Prise en charge de la récupération d’urgence et de la haute disponibilité par OLE DB Driver pour SQL Server
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

[!INCLUDE[Driver_OLEDB_Download](../../../includes/driver_oledb_download.md)]

  Cet article évoque la prise en charge par *OLE DB Driver pour SQL Server* de [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)]. Pour plus d’informations sur [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)], consultez [Écouteurs de groupe de disponibilité, connectivité client et basculement d’application &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/listeners-client-connectivity-application-failover.md), [Création et configuration des groupes de disponibilité &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/creation-and-configuration-of-availability-groups-sql-server.md), [Clustering de basculement et groupes de disponibilité AlwaysOn &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/failover-clustering-and-always-on-availability-groups-sql-server.md), et [Secondaires actifs : réplicas secondaires lisibles actifs &#40;groupes de disponibilité AlwaysOn&#41;](../../../database-engine/availability-groups/windows/active-secondaries-readable-secondary-replicas-always-on-availability-groups.md).  
  
 Vous pouvez spécifier l’écouteur d’un groupe de disponibilité donné dans la chaîne de connexion. Si une application OLE DB Driver for SQL Server est connectée à une base de données dans un groupe de disponibilité qui bascule, la connexion d’origine est rompue et l’application doit ouvrir une nouvelle connexion pour reprendre le travail après le basculement.  
  
 Si vous ne vous connectez pas à un écouteur du groupe de disponibilité et si plusieurs adresses IP sont associées à un nom d’hôte, OLE DB Driver for SQL Server effectue une itération séquentielle parmi toutes les adresses IP associées à l’entrée DNS. Cette opération peut prendre du temps si la première adresse IP retournée par le serveur DNS n'est liée à aucune carte d'interface réseau (NIC). Lors de la connexion à un écouteur de groupe de disponibilité, OLE DB Driver for SQL Server tente d’établir une connexion à toutes les adresses IP en parallèle. Quand une tentative de connexion réussit, le pilote abandonne toutes les tentatives de connexion en attente.  
  
> [!NOTE]  
> L'augmentation du délai de connexion et l'implémentation de la logique de tentative de connexion augmente la probabilité qu'une application se connecte à un groupe de disponibilité. En raison du risque d'échec de connexion en cas de basculement d'un groupe de disponibilité, il est également nécessaire d'implémenter la logique de déclenchement de nouvelles tentatives de connexion, afin de multiplier les tentatives jusqu'à ce qu'une connexion soit établie.  
  
## <a name="connecting-with-multisubnetfailover"></a>Connexion à MultiSubnetFailover  
 Spécifiez toujours **MultiSubnetFailover=Yes** lors de la connexion à un écouteur du groupe de disponibilité SQL Server AlwaysOn ou à une instance de cluster de basculement [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. **MultiSubnetFailover** permet un basculement plus rapide pour tous les groupes de disponibilité AlwaysOn et les instances de cluster de basculement dans [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], et réduit considérablement le temps de basculement pour les topologies AlwaysOn uniques et de plusieurs sous-réseaux. Lors d'un basculement de sous-réseaux multiples, le client tente les connexions en parallèle. Pendant un basculement de sous-réseau, OLE DB Driver pour SQL Server retente la connexion TCP.  
  
 La propriété de connexion **MultiSubnetFailover** indique que l’application est déployée sur un groupe de disponibilité ou une instance de cluster de basculement et que OLE DB Driver for SQL Server tente de se connecter à la base de données sur l’instance principale de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] en essayant toutes les adresses IP. Quand **MultiSubnetFailover=Yes** est spécifié dans le cadre d’une connexion, le client retente d’établir une connexion TCP plus rapidement que les intervalles de retransmission TCP par défaut du système d’exploitation. Ceci permet une reconnexion plus rapide après le basculement d’un groupe de disponibilité AlwaysOn ou d’une instance de cluster de basculement, et s’applique aux groupes de disponibilité et aux instances de cluster de basculement uniques et à plusieurs sous-réseaux.  
  
 Pour plus d’informations sur les mots clés de chaîne de connexion, consultez [Utilisation de mots clés de chaîne de connexion avec OLE DB Driver for SQL Server](../../oledb/applications/using-connection-string-keywords-with-oledb-driver-for-sql-server.md).  
  
 La spécification de **MultiSubnetFailover=Yes** quand la connexion n’est pas établie avec un écouteur de groupe de disponibilité ou une instance de cluster de basculement peut avoir un impact négatif sur les performances et n’est pas prise en charge.  
  
 Utilisez les instructions suivantes pour la connexion à un serveur dans un groupe de disponibilité ou dans une instance de cluster de basculement :  
  
-   Utilisez la propriété de connexion **MultiSubnetFailover** quand vous vous connectez à un sous-réseau unique ou à des sous-réseaux multiples pour améliorer leurs performances.  
  
-   Pour vous connecter à un groupe de disponibilité, spécifiez l'écouteur du groupe de disponibilité en tant que serveur dans votre chaîne de connexion.  
  
-   La connexion à une instance [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] configurée avec plus de 64 adresses IP provoque un échec de connexion.  
  
-   Le comportement d’une application qui utilise la propriété de connexion **MultiSubnetFailover** peut ne pas être affecté en fonction du type d’authentification : authentification [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], authentification Kerberos ou authentification Windows.  
  
-   Vous pouvez augmenter la valeur de **loginTimeout** pour tenir compte du temps de basculement et réduire les nouvelles tentatives de connexion de l’application.  
  
-   Les transactions distribuées ne sont pas prises en charge.  
  
Si le routage en lecture seule n'est pas appliqué, la connexion à un emplacement de réplica secondaire dans un groupe de disponibilité échoue dans les situations suivantes :  
  
1.  Si l'emplacement du réplica secondaire n'est pas configuré pour accepter des connexions.  
  
2.  Si une application utilise **ApplicationIntent=ReadWrite**(voir ci-dessous) et si l’emplacement de réplica secondaire est configuré pour un accès en lecture seule.  
  
Une connexion échoue si un réplica principal est configuré pour rejeter des charges de travail en lecture seule et si la chaîne de connexion contient **ApplicationIntent=ReadOnly**.  
  
## <a name="upgrading-to-use-multi-subnet-clusters-from-database-mirroring"></a>Mise à niveau pour utiliser des clusters de sous-réseaux multiples à partir de la mise en miroir de bases de données  
Une erreur de connexion se produit si les mots clés de connexion **MultiSubnetFailover** et **Failover_Partner** sont présents dans la chaîne de connexion. Une erreur se produit également si **MultiSubnetFailover** est utilisé et si [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] retourne une réponse de partenaire de basculement qui indique qu’il fait partie d’une paire de mise en miroir de bases de données.  
  
Si vous mettez à niveau une application OLE DB Driver for SQL Server qui utilise actuellement la mise en miroir de bases de données pour un scénario de sous-réseaux multiples, vous devez supprimer la propriété de connexion **Failover_Partner** et la remplacer par **MultiSubnetFailover** définie sur **Yes**, et remplacer le nom du serveur dans la chaîne de connexion par un écouteur du groupe de disponibilité. Si une chaîne de connexion utilise **Failover_Partner** et **MultiSubnetFailover=Yes**, le pilote génère une erreur. Toutefois, si une chaîne de connexion utilise **Failover_Partner** et **MultiSubnetFailover=No** (ou **ApplicationIntent=ReadWrite**), l’application utilise la mise en miroir de bases de données.  
  
Le pilote retournera une erreur si la mise en miroir de bases de données est utilisée pour la base de données principale au sein du groupe de disponibilité, et si **MultiSubnetFailover=Yes** est utilisé dans la chaîne de connexion qui établit une connexion avec une base de données principale au lieu d’un écouteur de groupe de disponibilité.  


[!INCLUDE[specify-application-intent_read-only-routing](~/includes/paragraph-content/specify-application-intent-read-only-routing.md)]


## <a name="ole-db"></a>OLE DB  
OLE DB Driver pour SQL Server prend en charge les mots clés **ApplicationIntent** et **MultiSubnetFailover**.   
  
Les deux mots clés de chaîne de connexion OLE DB ont été ajoutés pour prendre en charge [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] dans OLE DB Driver pour SQL Server :  
  
-   **ApplicationIntent** 
-   **MultiSubnetFailover**  
  
 Pour plus d’informations sur les mots clés de chaîne de connexion dans le pilote OLE DB pour SQL Server, voir [Utiliser des mots clés de chaîne de connexion avec le pilote OLE DB pour SQL Server](../../oledb/applications/using-connection-string-keywords-with-oledb-driver-for-sql-server.md).  

### <a name="application-intent"></a>Intention de l'application 

Les propriétés de connexion équivalentes sont :  
  
-   **SSPROP_INIT_APPLICATIONINTENT**  
  
-   **DBPROP_INIT_PROVIDERSTRING**  
  
Une application OLE DB Driver pour SQL Server peut utiliser l'une de ces méthodes pour spécifier l'intention de l'application :  
  
 -   **IDBInitialize::Initialize**  
 **IDBInitialize::Initialize** utilise le jeu de propriétés configuré précédemment pour initialiser la source de données et créer l’objet source de données. Spécifiez l'intention de l'application en tant que propriété de fournisseur ou dans le cadre de la chaîne de propriétés étendues.  
  
 -   **IDataInitialize::GetDataSource**  
 **IDataInitialize::GetDataSource** accepte une chaîne de connexion d’entrée qui peut contenir le mot clé **Application Intent**.  
  
 -   **IDBProperties::SetProperties**  
 Pour définir la valeur de propriété **ApplicationIntent**, appelez **IDBProperties::SetProperties** qui passe la propriété **SSPROP_INIT_APPLICATIONINTENT** avec la valeur « **ReadWrite** » ou « **ReadOnly** » ou **DBPROP_INIT_PROVIDERSTRING** avec la valeur qui contient « **ApplicationIntent=ReadOnly** » ou « **ApplicationIntent=ReadWrite** ».  
  
Vous pouvez spécifier l’intention de l’application dans le champ Propriétés de l’intention de l’application de l’onglet Tous de la boîte de dialogue **Propriétés de liaison de données**.  
  
Lorsque des connexions implicites sont établies, la connexion implicite utilise le paramètre d'intention de l'application de la connexion parente. De la même façon, plusieurs sessions créées à partir de la même source de données héritent du paramètre d'intention de l'application de la source de données.  
  
### <a name="multisubnetfailover"></a>MultiSubnetFailover

Les propriétés de connexion équivalentes sont :  
  
-   **SSPROP_INIT_MULTISUBNETFAILOVER**  
  
-   **DBPROP_INIT_PROVIDERSTRING**  

Une application OLE DB Driver pour SQL Server peut utiliser l’une des méthodes suivantes pour définir l’option MultiSubnetFailover :  

 -   **IDBInitialize::Initialize**  
 **IDBInitialize::Initialize** utilise le jeu de propriétés configuré précédemment pour initialiser la source de données et créer l’objet source de données. Spécifiez l'intention de l'application en tant que propriété de fournisseur ou dans le cadre de la chaîne de propriétés étendues.  
  
 -   **IDataInitialize::GetDataSource**  
 **IDataInitialize::GetDataSource** accepte en entrée une chaîne de connexion qui peut contenir le mot clé **Application Intent**.  

-   **IDBProperties::SetProperties**  
Pour définir la valeur de la propriété **MultiSubnetFailover**, appelez **IDBProperties::SetProperties** en passant la propriété **SSPROP_INIT_MULTISUBNETFAILOVER** avec la valeur **VARIANT_TRUE** ou **VARIANT_FALSE**, ou la propriété **DBPROP_INIT_PROVIDERSTRING** avec la valeur contenant « **MultiSubnetFailover = Yes** » ou « **MultiSubnetFailover = no** ».

#### <a name="example"></a>Exemple

```
DBPROP rgPropMultisubnet;

rgPropMultisubnet.dwPropertyID = SSPROP_INIT_MULTISUBNETFAILOVER;
rgPropMultisubnet.dwOptions = DBPROPOPTIONS_REQUIRED;
rgPropMultisubnet.dwStatus = DBPROPSTATUS_OK;
rgPropMultisubnet.colid = DB_NULLID;
V_VT(&(rgPropMultisubnet.vValue)) = VT_BOOL;
V_BOOL(&(rgPropMultisubnet.vValue)) = VARIANT_TRUE;

DBPROPSET PropSet;

PropSet.rgProperties = &rgPropMultisubnet;
PropSet.cProperties = 1;
PropSet.guidPropertySet = DBPROPSET_SQLSERVERDBINIT;
IDBProperties* pIDBProperties = NULL;
hr = pIDBInitialize->QueryInterface(IID_IDBProperties, (void **)&pIDBProperties);
pIDBProperties->SetProperties(1, &PropSet);
```

## <a name="see-also"></a>Voir aussi  
 [Fonctionnalités OLE DB Driver pour SQL Server](../../oledb/features/oledb-driver-for-sql-server-features.md)    
 [Utilisation de mots clés de chaîne de connexion avec OLE DB Driver pour SQL Server](../../oledb/applications/using-connection-string-keywords-with-oledb-driver-for-sql-server.md)  
  
  
