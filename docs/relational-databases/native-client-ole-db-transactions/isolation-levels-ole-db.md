---
description: Niveaux d’isolement (fournisseur Native Client OLE DB)
title: Niveaux d’isolement (fournisseur Native Client OLE DB) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- OLE DB, transactions
- isolation levels [OLE DB]
- transactions [OLE DB]
- SQL Server Native Client OLE DB provider, transactions
ms.assetid: d70ee72c-6e2a-4bcd-9456-4a697a866361
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 90f33a053e9c1c0ebea0b0248e79660ec7428955
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/17/2020
ms.locfileid: "88498995"
---
# <a name="isolation-levels-native-client-ole-db-provider"></a>Niveaux d’isolement (fournisseur Native Client OLE DB)
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Les clients [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] peuvent contrôler les niveaux d'isolation des transactions pour une connexion. Pour contrôler le niveau d’isolation des transactions, le [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] consommateur du fournisseur OLE DB Native Client utilise :  
  
-   DBPROPSET_SESSION DBPROP_SESS_AUTOCOMMITISOLEVELS de propriété pour le [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] mode de validation automatique par défaut du fournisseur OLE DB Native Client.  
  
     La [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] valeur par défaut du fournisseur de OLE DB Native Client pour le niveau est DBPROPVAL_TI_READCOMMITTED.  
  
-   Le paramètre *isoLevel* de la méthode **ITransactionLocal::StartTransaction** pour les transactions de validation manuelle locales.  
  
-   Le paramètre *isoLevel* de la méthode **ITransactionDispenser::BeginTransaction** pour les transactions distribuées coordonnées par MS DTC.  
  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] autorise l'accès en lecture seule au niveau d'isolation de lecture erronée. Tous les autres niveaux restreignent la concurrence en appliquant des verrous aux objets [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Comme le client a besoin de niveaux d'accès concurrentiel supérieurs, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] applique des restrictions supérieures sur l'accès concurrentiel aux données. Pour maintenir le plus haut niveau d’accès simultané aux données, le [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] consommateur du fournisseur OLE DB Native Client doit contrôler intelligemment ses demandes de niveaux de concurrence spécifiques.  
  
> [!NOTE]  
>  [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] a introduit le niveau d'isolement d'instantané. Pour plus d’informations, consultez [Utilisation du niveau d’isolement d’instantané](../../relational-databases/native-client/features/working-with-snapshot-isolation.md).  
  
## <a name="see-also"></a>Voir aussi  
 [Transactions](../../relational-databases/native-client-ole-db-transactions/transactions.md)  
  
  
