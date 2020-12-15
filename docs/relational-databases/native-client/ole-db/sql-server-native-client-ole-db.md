---
title: OLE DB
description: Le fournisseur SQL Server Native Client OLE DB est une API COM pour l’accès aux données, utilisée pour les outils, les utilitaires ou les composants de bas niveau qui nécessitent des performances élevées.
ms.custom: ''
ms.date: 03/17/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- SQL Server Native Client OLE DB provider, about SQL Server Native Client OLE DB provider
- OLE DB, SQL Server Native Client OLE DB provider
- data access [SQL Server Native Client], OLE DB
- SQLNCLI, OLE DB
- OLE DB
- SQL Server Native Client OLE DB provider
- SQL Server Native Client, OLE DB
ms.assetid: da846da4-ec19-4a4f-81fb-7d5a2b2bf80a
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 133f9a53d6de89b6d8720781606301b939a32bf3
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/14/2020
ms.locfileid: "97467550"
---
# <a name="sql-server-native-client-ole-db"></a>SQL Server Native Client (OLE DB)
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Le [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] fournisseur de OLE DB Native Client (SQLNCLI) est une API COM de bas niveau utilisée pour accéder aux données. Le fournisseur OLE DB [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client est recommandé pour développer des outils, des utilitaires ou des composants de bas niveau qui ont besoin de performances élevées. Le fournisseur OLE DB [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client est un fournisseur natif de hautes performances qui accède directement au protocole TDS (Tabular Data Stream) [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].  
  
 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client fournit la prise en charge OLE DB aux applications qui se connectent à [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].  
  
 Le [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] fournisseur OLE DB Native Client est un fournisseur conforme à la version 2,0 de OLE DB.  
 
> [!IMPORTANT]
> Le [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] OLE DB Native Client (SQLNCLI) reste déconseillé et il n’est pas recommandé de l’utiliser pour un nouveau travail de développement. Au lieu de cela, utilisez le nouveau [Microsoft OLE DB Driver pour SQL Server](../../../connect/oledb/oledb-driver-for-sql-server.md) (MSOLEDBSQL), qui sera mis à jour avec les fonctionnalités serveur les plus récentes.
  
## <a name="in-this-section"></a>Dans cette section  
  
-   [Création d'une application de fournisseur OLE DB de SQL Server Native Client](../../../relational-databases/native-client-ole-db-provider/creating-a-sql-server-native-client-ole-db-provider-application.md)  
  
-   [Objets source de données &#40;OLE DB&#41;](../../../relational-databases/native-client-ole-db-data-source-objects/data-source-objects-ole-db.md)  
  
-   [Commandes](../../../relational-databases/native-client-ole-db-commands/commands.md)  
  
-   [Ensembles de lignes](../../../relational-databases/native-client-ole-db-rowsets/rowsets.md)  
  
-   [Procédures stockées](../../../relational-databases/native-client/ole-db/stored-procedures.md)  
  
-   [Objets BLOB et OLE](../../../relational-databases/native-client-ole-db-blobs/blobs-and-ole-objects.md)  
  
-   [Tables et index](../../../relational-databases/native-client-ole-db-tables-indexes/tables-and-indexes.md)  
  
-   [Types de données &#40;OLE DB&#41;](../../../relational-databases/native-client-ole-db-data-types/data-types-ole-db.md)  
  
-   [Prise en charge des ensembles de lignes de schéma &#40;OLE DB&#41;](../../../relational-databases/native-client/ole-db/schema-rowset-support-ole-db.md)  
  
-   [Paramètres table &#40;OLE DB&#41;](../../../relational-databases/native-client-ole-db-table-valued-parameters/table-valued-parameters-ole-db.md)  
  
-   [Améliorations des types de données de date et d’heure &#40;OLE DB&#41;](../../../relational-databases/native-client-ole-db-date-time/date-and-time-improvements-ole-db.md)  
  
-   [Types CLR de grande taille définis par l’utilisateur &#40;OLE DB&#41;](../../../relational-databases/native-client/ole-db/large-clr-user-defined-types-ole-db.md)  
  
-   [Prise en charge FILESTREAM &#40;OLE DB&#41;](../../../relational-databases/native-client/ole-db/filestream-support-ole-db.md)  
  
-   [Transactions](../../../relational-databases/native-client-ole-db-transactions/transactions.md)  
  
-   [Erreurs](../../../relational-databases/native-client-ole-db-errors/errors.md)  
  
-   [Noms de principal du service &#40;SPN&#41; dans les connexions clientes &#40;OLE DB&#41;](../../../relational-databases/native-client/ole-db/service-principal-names-spns-in-client-connections-ole-db.md)  
  
-   [Prise en charge des colonnes éparses &#40;OLE DB&#41;](../../../relational-databases/native-client/ole-db/sparse-columns-support-ole-db.md)  
  
-   [SQL Server Native Client &#40;OLE DB référence de&#41;](../../../relational-databases/native-client-ole-db-interfaces/sql-server-native-client-ole-db-interfaces.md)  
  
-   [Rubriques de procédures liées à OLE DB](../../../relational-databases/native-client-ole-db-how-to/ole-db-how-to-topics.md)  
  
## <a name="see-also"></a>Voir aussi  
 [Programmation de SQL Server Native Client](../../../relational-databases/native-client/sql-server-native-client-programming.md)  
  
  
