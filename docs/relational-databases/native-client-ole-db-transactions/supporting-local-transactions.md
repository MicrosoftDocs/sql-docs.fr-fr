---
description: Prise en charge des transactions locales dans SQL Server Native Client
title: Prise en charge des transactions locales (fournisseur Native Client OLE DB)
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- OLE DB, transactions
- autocommit mode
- transactions [OLE DB]
- ITransactionLocal interface
- SQL Server Native Client OLE DB provider, transactions
- local transactions [OLE DB]
ms.assetid: 78f2e5fc-b6fb-4eda-9f71-991a4d6c4902
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 70db41bdf982201269b5274436ccafd3602861e3
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/14/2020
ms.locfileid: "97462120"
---
# <a name="supporting-local-transactions-in-sql-server-native-client"></a>Prise en charge des transactions locales dans SQL Server Native Client
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Une session délimite l’étendue de la transaction pour une [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] transaction locale du fournisseur OLE DB Native Client. Quand, à la direction d’un consommateur, le [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] fournisseur de OLE DB Native Client soumet une requête à une instance connectée de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , la demande constitue une unité de travail pour le [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] fournisseur de OLE DB Native Client. Les transactions locales encapsulent toujours une ou plusieurs unités de travail sur une seule [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] session Native Client OLE DB fournisseur.  
  
 En utilisant le [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] mode de validation automatique du fournisseur OLE DB Native Client par défaut, une unité de travail unique est traitée comme l’étendue d’une transaction locale. Une seule unité participe à la transaction locale. Lorsqu’une session est créée, le [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] fournisseur de OLE DB Native Client commence une transaction pour la session. Une fois que l'unité de travail a été exécutée avec succès, le travail est validé. En cas d'échec, tout travail commencé est annulé et l'erreur est signalée au consommateur. Dans les deux cas, le [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] fournisseur de OLE DB Native Client commence une nouvelle transaction locale pour la session afin que tout le travail soit effectué dans une transaction.  
  
 Le [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] consommateur du fournisseur OLE DB Native Client peut diriger un contrôle plus précis sur l’étendue de la transaction locale à l’aide de l’interface **ITransactionLocal** . Lorsqu’une session de consommateur démarre une transaction, toutes les unités de travail de la session entre le point de départ de la transaction et les appels éventuels de la méthode **Commit** ou **Abort** sont traités comme une unité atomique. Le [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] fournisseur OLE DB Native Client commence implicitement une transaction lorsqu’il y est invité par le consommateur. Si le consommateur ne demande pas la rétention, la session revient au comportement parent du niveau de la transaction, soit le plus généralement le mode de validation automatique.  
  
 Le [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] fournisseur OLE DB Native Client prend en charge les paramètres **ITransactionLocal :: StartTransaction** comme suit.  
  
|Paramètre|Description|  
|---------------|-----------------|  
|*isoLevel*[in]|Niveau d'isolation à utiliser avec cette transaction. Dans les transactions locales, le [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] fournisseur de OLE DB Native Client prend en charge les éléments suivants :<br /><br /> **ISOLATIONLEVEL_UNSPECIFIED**<br /><br /> **ISOLATIONLEVEL_CHAOS**<br /><br /> **ISOLATIONLEVEL_READUNCOMMITTED**<br /><br /> **ISOLATIONLEVEL_READCOMMITTED**<br /><br /> **ISOLATIONLEVEL_REPEATABLEREAD**<br /><br /> **ISOLATIONLEVEL_CURSORSTABILITY**<br /><br /> **ISOLATIONLEVEL_REPEATABLEREAD**<br /><br /> **ISOLATIONLEVEL_SERIALIZABLE**<br /><br /> **ISOLATIONLEVEL_ISOLATED**<br /><br /> **ISOLATIONLEVEL_SNAPSHOT**<br /><br /> <br /><br /> Remarque : Dans [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] et versions ultérieures, ISOLATIONLEVEL_SNAPSHOT est valide pour l’argument *isoLevel*, que le suivi des versions soit activé ou non pour la base de données. Cependant, une erreur se produit si l'utilisateur essaie d'exécuter une instruction et que le suivi des versions n'est pas activé et/ou que la base de données n'est pas en lecture seule. De plus, l’erreur XACT_E_ISOLATIONLEVEL se produit si ISOLATIONLEVEL_SNAPSHOT est spécifié comme *isoLevel* lors de la connexion à une version de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] antérieure à [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)].|  
|*isoFlags*[in]|Le [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] fournisseur de OLE DB Native Client retourne une erreur pour toute valeur autre que zéro.|  
|*pOtherOptions*[in]|Si la valeur n’est pas NULL, le [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] fournisseur de OLE DB Native Client demande l’objet d’options à partir de l’interface. Le [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] fournisseur de OLE DB Native Client retourne XACT_E_NOTIMEOUT si le membre *ulTimeout* de l’objet d’options n’est pas égal à zéro. Le [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] fournisseur de OLE DB Native Client ignore la valeur du membre *szDescription* .|  
|*pulTransactionLevel*[out]|Si la valeur n’est pas NULL, le [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] fournisseur de OLE DB Native Client retourne le niveau imbriqué de la transaction.|  
  
 Pour les transactions locales, le [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] fournisseur de OLE DB Native Client implémente les paramètres **ITransaction :: Abort** comme suit.  
  
|Paramètre|Description|  
|---------------|-----------------|  
|*pboidReason*[in]|Ignoré s'il est défini. Peut être égal à NULL en toute sécurité.|  
|*fRetaining*[in]|Lorsque la valeur est TRUE, une nouvelle transaction est commencée implicitement pour la session. La transaction doit être validée ou terminée par le consommateur. Quand la valeur est FALSe, le [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] fournisseur de OLE DB Native Client revient en mode de validation automatique pour la session.|  
|*fAsync*[in]|L’abandon asynchrone n’est pas pris en charge par le [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] fournisseur de OLE DB Native Client. Le [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] fournisseur OLE DB Native Client retourne XACT_E_NOTSUPPORTED si la valeur n’est pas false.|  
  
 Pour les transactions locales, le [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] fournisseur de OLE DB Native Client implémente les paramètres **ITransaction :: Commit** comme suit.  
  
|Paramètre|Description|  
|---------------|-----------------|  
|*fRetaining*[in]|Lorsque la valeur est TRUE, une nouvelle transaction est commencée implicitement pour la session. La transaction doit être validée ou terminée par le consommateur. Quand la valeur est FALSe, le [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] fournisseur de OLE DB Native Client revient en mode de validation automatique pour la session.|  
|*grfTC*[in]|Les retours asynchrones et de phase 1 ne sont pas pris en charge par le [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] fournisseur de OLE DB Native Client. Le [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] fournisseur de OLE DB Native Client retourne XACT_E_NOTSUPPORTED pour toute valeur autre que XACTTC_SYNC.|  
|*grfRM*[in]|Doit être égal à 0.|  
  
 Les [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ensembles de lignes du fournisseur Native Client OLE DB sur la session sont conservés lors d’une opération de validation ou d’abandon locale basée sur les valeurs des propriétés de l’ensemble de lignes DBPROP_ABORTPRESERVE et DBPROP_COMMITPRESERVE. Par défaut, ces propriétés sont VARIANT_FALSE et tous les [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ensembles de lignes de fournisseur OLE DB Native Client de la session sont perdus après une opération d’abandon ou de validation.  
  
 Le [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] fournisseur OLE DB Native Client n’implémente pas l’interface **ITransactionObject** . La tentative d'un consommateur pour extraire une référence sur l'interface retourne E_NOINTERFACE.  
  
 L’exemple suivant utilise **ITransactionLocal**.  
  
```  
// Interfaces used in the example.  
IDBCreateSession*   pIDBCreateSession   = NULL;  
ITransaction*       pITransaction       = NULL;  
IDBCreateCommand*   pIDBCreateCommand   = NULL;  
IRowset*            pIRowset            = NULL;  
  
HRESULT             hr;  
  
// Get the command creation and local transaction interfaces for the  
// session.  
if (FAILED(hr = pIDBCreateSession->CreateSession(NULL,  
     IID_IDBCreateCommand, (IUnknown**) &pIDBCreateCommand)))  
    {  
    // Process error from session creation. Release any references and  
    // return.  
    }  
  
if (FAILED(hr = pIDBCreateCommand->QueryInterface(IID_ITransactionLocal,  
    (void**) &pITransaction)))  
    {  
    // Process error. Release any references and return.  
    }  
  
// Start the local transaction.  
if (FAILED(hr = ((ITransactionLocal*) pITransaction)->StartTransaction(  
    ISOLATIONLEVEL_REPEATABLEREAD, 0, NULL, NULL)))  
    {  
    // Process error from StartTransaction. Release any references and  
    // return.  
    }  
  
// Get data into a rowset, then update the data. Functions are not  
// illustrated in this example.  
if (FAILED(hr = ExecuteCommand(pIDBCreateCommand, &pIRowset)))  
    {  
    // Release any references and return.  
    }  
  
// If rowset data update fails, then terminate the transaction, else  
// commit. The example doesn't retain the rowset.  
if (FAILED(hr = UpdateDataInRowset(pIRowset, bDelayedUpdate)))  
    {  
    // Get error from update, then terminate.  
    pITransaction->Abort(NULL, FALSE, FALSE);  
    }  
else  
    {  
    if (FAILED(hr = pITransaction->Commit(FALSE, XACTTC_SYNC, 0)))  
        {  
        // Get error from failed commit.  
        }  
    }  
  
if (FAILED(hr))  
    {  
    // Update of data or commit failed. Release any references and  
    // return.  
    }  
  
// Release any references and continue.  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Transactions](../../relational-databases/native-client-ole-db-transactions/transactions.md)   
 [Utilisation du niveau d’isolement de capture instantanée](../../relational-databases/native-client/features/working-with-snapshot-isolation.md)  
  
  
