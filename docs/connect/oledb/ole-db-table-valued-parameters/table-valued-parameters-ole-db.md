---
title: Paramètres table (OLE DB) | Microsoft Docs
description: Paramètres table (OLE DB)
ms.custom: ''
ms.date: 06/14/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- OLE DB, table-valued parameters
- table-valued parameters (OLE DB)
author: pmasl
ms.author: pelopes
ms.openlocfilehash: 8b93503bbf538cd5cb58488b5df7de9190dd3c78
ms.sourcegitcommit: f3321ed29d6d8725ba6378d207277a57cb5fe8c2
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/06/2020
ms.locfileid: "85998924"
---
# <a name="table-valued-parameters-ole-db"></a>Paramètres table (OLE DB)
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

[!INCLUDE[Driver_OLEDB_Download](../../../includes/driver_oledb_download.md)]

  Cette section décrit la prise en charge des paramètres table dans le fournisseur OLE DB Driver pour SQL Server. Pour plus d’informations, consultez [Paramètres table &#40;OLE DB Driver pour SQL Server&#41;](../../oledb/features/table-valued-parameters-oledb-driver-for-sql-server.md). Pour obtenir un exemple, consultez [Utiliser les paramètres table &#40;OLE DB&#41;](../../oledb/ole-db-how-to/use-table-valued-parameters-ole-db.md).  
  
## <a name="remarks"></a>Notes  
 Vous pouvez actuellement envoyer des données à lignes multiples au serveur en tant que paramètres à une procédure, avec des jeux de paramètres (le paramètre DBPARAMS dans **ICommand::Execute**). Avec des jeux de paramètres, chaque élément du jeu doit être envoyé dans une demande d'appel de procédure distante séparée au serveur. Les paramètres table fournissent des fonctionnalités semblables, mais l'intégration avec le serveur est meilleure. Cela réduit le nombre de demandes d’appel de procédure distante et autorise des opérations basées sur des jeux sur le serveur.  
  
 Les paramètres table sont pris en charge dans OLE DB Driver pour SQL Server en tant qu’objets **Ensemble de lignes** OLE DB. Tout objet **Rowset** peut être fourni par le consommateur (autrement dit, l’application cliente utilisant OLE DB Driver for SQL Server) en tant qu’espace réservé pour les paramètres de paramètre table. Les paramètres table sont traités comme tout autre type de paramètre [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Le fournisseur OLE DB Driver pour SQL Server fournit des interfaces de création, de découverte, de spécification, de liaison et de schéma.  
  
## <a name="in-this-section"></a>Dans cette section  
  
-   [Création d’un ensemble de lignes de paramètres table](../../oledb/ole-db-table-valued-parameters/table-valued-parameter-rowset-creation.md)  
  
-   [Détection des types de paramètres table](../../oledb/ole-db-table-valued-parameters/table-valued-parameter-type-discovery.md)  
  
-   [Exécution de commandes contenant des paramètres table](../../oledb/ole-db-table-valued-parameters/executing-commands-containing-table-valued-parameters.md)  
  
-   [Insertion de données dans des paramètres table](../../oledb/ole-db-table-valued-parameters/inserting-data-into-table-valued-parameters.md)  
  
-   [Ensembles de lignes de schéma modifiés pour les paramètres table OLE DB](../../oledb/ole-db-table-valued-parameters/schema-rowsets-changed-for-ole-db-table-valued-parameters.md)  
  
-   [Prise en charge des types de paramètre table OLE DB](../../oledb/ole-db-table-valued-parameters/ole-db-table-valued-parameter-type-support.md)  
  
-   [Prise en charge des types de paramètre table OLE DB &#40;méthodes&#41;](../../oledb/ole-db-table-valued-parameters/ole-db-table-valued-parameter-type-support-methods.md)  
  
-   [Prise en charge des types de paramètre table OLE DB &#40;propriétés&#41;](../../oledb/ole-db-table-valued-parameters/ole-db-table-valued-parameter-type-support-properties.md)  
  
## <a name="see-also"></a>Voir aussi  
 [Programmation du pilote OLE DB pour SQL Server](../../oledb/ole-db/oledb-driver-for-sql-server-programming.md)   
 [Utiliser les paramètres table &#40;OLE DB&#41;](../../oledb/ole-db-how-to/use-table-valued-parameters-ole-db.md)  
  
  
