---
title: Commandes générant des résultats à plusieurs ensembles de lignes (Native Client OLE DB fournisseur) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- multiple rowsets
- rowsets [OLE DB], multiple
- SQL Server Native Client OLE DB provider, commands
- SQL Server Native Client OLE DB provider, multiple rowsets
- commands [OLE DB]
- multiple-rowset results
ms.assetid: 4567668d-35fd-4162-b61f-f7536862cdcb
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: d9febce09a93e92ff2a344b091fd3cd85d6bdc2b
ms.sourcegitcommit: 216f377451e53874718ae1645a2611cdb198808a
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/28/2020
ms.locfileid: "87247043"
---
# <a name="sql-server-native-client-commands-generating-multiple-rowset-results"></a>SQL Server Native Client des commandes générant des résultats à plusieurs ensembles de lignes
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Le [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] fournisseur OLE DB Native Client peut retourner plusieurs ensembles de lignes à partir d' [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] instructions. Les instructions [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] retournent des résultats dans plusieurs ensembles de lignes dans les conditions suivantes :  
  
-   des instructions SQL groupées sont soumises en tant que commande unique ;  
  
-   des procédures stockées implémentent un lot d'instructions SQL ;  
  
## <a name="batches"></a>Lots  
 Le [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] fournisseur de OLE DB Native Client reconnaît le point-virgule comme délimiteur de lot pour les instructions SQL :  
  
```  
WCHAR*       wSQLString = L"SELECT * FROM Categories; "  
                          L"SELECT * FROM Products";  
```  
  
 Il est plus efficace d'envoyer plusieurs instructions SQL dans un lot que d'exécuter chaque instruction SQL séparément. L'envoi d'un lot réduit les allers-retours sur le réseau entre le client et le serveur.  
  
## <a name="stored-procedures"></a>Procédures stockées  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] retourne un jeu de résultats pour chaque instruction dans une procédure stockée ; ainsi, la plupart des procédures stockées [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] retournent plusieurs jeux de résultats.  
  
## <a name="in-this-section"></a>Dans cette section  
  
-   [Utilisation d'IMultipleResults pour traiter plusieurs jeux de résultats](../../relational-databases/native-client-ole-db-commands/using-imultipleresults-to-process-multiple-result-sets.md)  
  
## <a name="see-also"></a>Voir aussi  
 [Commandes](../../relational-databases/native-client-ole-db-commands/commands.md)  
  
  
