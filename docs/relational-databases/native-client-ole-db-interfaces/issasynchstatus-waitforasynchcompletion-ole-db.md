---
description: 'ISSAsynchStatus :: WaitForAsynchCompletion en SQL Server Native Client (OLE DB)'
title: 'ISSAsynchStatus :: WaitForAsynchCompletion (fournisseur Native Client OLE DB) | Microsoft Docs'
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
apiname:
- ISSAsynchStatus::WaitForAsynchCompletion (OLE DB)
apitype: COM
helpviewer_keywords:
- WaitForAsynchCompletion method
ms.assetid: 9f65e9e7-eb93-47a1-bc42-acd4649fbd0e
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 8496a19a7ce6d532fbcf740368bbe35571789d0e
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/14/2020
ms.locfileid: "97469310"
---
# <a name="issasynchstatuswaitforasynchcompletion-in-sql-server-native-client-ole-db"></a>ISSAsynchStatus :: WaitForAsynchCompletion en SQL Server Native Client (OLE DB)
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Attend que l'opération s'exécutant de façon asynchrone se termine ou qu'un délai d'expiration soit dépassé.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
HRESULT WaitForAsynchCompletion(   
        DWORD dwMillisecTimeOut);  
```  
  
## <a name="arguments"></a>Arguments  
 *dwMillisecTimeOut*[in]  
 Délai en millisecondes.  
  
## <a name="return-code-values"></a>Codet de retour  
 S_OK  
 S_OK  
  
 E_UNEXPECTED  
 Un ensemble de lignes est dans un état inutilisé, car **ITransaction::Commit** ou **ITransaction::Abort** a été appelé, ou l'ensemble de lignes a été annulé pendant sa phase d'initialisation.  
  
 DB_E_CANCELED  
 Le traitement asynchrone a été annulé pendant le remplissage de l'ensemble de lignes ou l'initialisation de l'objet source de données.  
  
 DB_S_ASYNCHRONOUS  
 L'opération n'est pas encore terminée même si le délai d'expiration spécifié a été atteint.  
  
> [!NOTE]  
>  Outre les valeurs de code de retour répertoriées ci-dessus, la méthode **ISSAsynchStatus::WaitForAsynchCompletion** prend également en charge les valeurs de code de retour retournées par les méthodes OLEDB **ICommand::Execute** et **IDBInitialize::Initialize** principales.  
  
## <a name="remarks"></a>Notes  
 La méthode **ISSAsynchStatus::WaitForAsynchCompletion** n'est pas retournée tant que la valeur du délai d'attente (en millisecondes) n'est pas passée ou que l'opération en attente n'est pas terminée. L'objet **Command** a une propriété **CommandTimeout** qui contrôle le nombre de secondes pendant lesquelles une requête doit s'exécuter avant l'expiration du délai imparti. La propriété **CommandTimeout** est ignorée si elle est utilisée conjointement avec la méthode **ISSAsynchStatus::WaitForAsynchCompletion**.  
  
 La propriété relative au délai d'expiration est ignorée pour les opérations asynchrones. Le paramètre de délai d'expiration de **ISSAsynchStatus::WaitForAsynchCompletion** spécifie la durée maximale qui doit s'écouler avant que le contrôle ne soit retourné à l'appelant. Si ce délai expire, DB_S_ASYNCHRONOUS est retourné. L'expiration des délais d'attente n'annule jamais les opérations asynchrones. Si l'application doit annuler une opération asynchrone qui ne se termine pas dans un délai imparti, elle doit attendre l'expiration du délai, puis annuler explicitement l'opération si DB_S_ASYNCHRONOUS est retourné.  
  
> [!NOTE]  
>  Quand les composants du service OLE DB sont utilisés, S_OK peut être retourné quand DB_S_ASYNCHRONOUS est attendu ; ainsi, les applications doivent appeler [ISSAsynchStatus::GetStatus](../../relational-databases/native-client-ole-db-interfaces/issasynchstatus-getstatus-ole-db.md) pour vérifier l’état d’achèvement quand S_OK ou DB_S_ASYNCHRONOUS est retourné.  
  
 Si la valeur de *dwMillisecTimeOut* est INFINITE, la méthode **ISSAsynchStatus::WaitForAsynchCompletion** se bloque jusqu'à ce que l'opération soit terminée. Si la valeur de *dwMillisecTimeOut* est 0, la méthode est retournée immédiatement avec l'état de l'opération en attente. Si le délai d'attente expire avant que l'opération ne soit terminée, DB_S_ASYNCHRONOUS est retourné.  
  
 Si l'opération se termine avant l'expiration du délai d'attente, le HRESULT retourné correspond au HRESULT retourné par l'opération (HRESULT retourné si l'opération était effectuée de façon synchrone).  
  
 De plus, la propriété SSPROP_ISSAsynchStatus a été ajoutée au jeu de propriétés DBPROPSET_SQLSERVERROWSET. Les fournisseurs qui prennent en charge l’interface [ISSAsynchStatus](../../relational-databases/native-client-ole-db-interfaces/issasynchstatus-ole-db.md) doivent implémenter cette propriété avec la valeur VARIANT_TRUE.  
  
## <a name="see-also"></a>Voir aussi  
 [Exécution d’opérations asynchrones](../../relational-databases/native-client/features/performing-asynchronous-operations.md)   
 [ISSAsynchStatus &#40;OLE DB&#41;](../../relational-databases/native-client-ole-db-interfaces/issasynchstatus-ole-db.md)  
  
  
