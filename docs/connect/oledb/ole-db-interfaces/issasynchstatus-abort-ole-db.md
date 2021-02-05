---
title: ISSAsynchStatus::Abort (pilote OLE DB) | Microsoft Docs
description: Découvrez comment la méthode ISSAsynchStatus::Abort annule une opération s’exécutant de façon asynchrone dans OLE DB Driver pour SQL Server.
ms.custom: ''
ms.date: 06/14/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- ISSAsynchStatus::Abort (OLE DB)
apitype: COM
helpviewer_keywords:
- Abort method
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 0eb1d96d2e3656d046cd738d1a4f38f4f6b7cb9f
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99183745"
---
# <a name="issasynchstatusabort-ole-db"></a>ISSAsynchStatus::Abort (OLE DB)
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

[!INCLUDE[Driver_OLEDB_Download](../../../includes/driver_oledb_download.md)]

  Annule une opération s'exécutant de manière asynchrone.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
HRESULT Abort(  
        HCHAPTER hChapter,  
        DBASYNCHOP eOperation);  
```  
  
## <a name="arguments"></a>Arguments  
 *hChapter*[in]  
 Handle du chapitre pour lequel vous souhaitez abandonner l'opération. Si l’objet appelé n’est pas un objet d’ensemble de lignes ou que l’opération ne s’applique pas à un chapitre, l’appelant doit attribuer la valeur DB_NULL_HCHAPTER à *hChapter*.  
  
 *eOperation*[in]  
 L'opération à abandonner. La valeur suivante doit être utilisée :  
  
 DBASYNCHOP_OPEN : la demande d'annulation s'applique à l'ouverture ou au remplissage asynchrone d'un ensemble de lignes ou à l'initialisation asynchrone d'un objet source de données.  
  
## <a name="return-code-values"></a>Codet de retour  
 S_OK  
 La demande d'annulation de l'opération asynchrone a été traitée. Cela ne garantit pas que l’opération ait été annulée. Pour déterminer si l'opération a bien été annulée, le contrôle serveur consommateur doit appeler [ISSAsynchStatus::GetStatus](../../oledb/ole-db-interfaces/issasynchstatus-getstatus-ole-db.md) et vérifier la présence de DB_E_CANCELED ; toutefois, il est possible qu'il ne soit pas retourné dans l'appel suivant.  
  
 DB_E_CANTCANCEL  
 L'opération asynchrone ne peut pas être annulée.  
  
 DB_E_CANCELED  
 La demande d'abandon de l'opération asynchrone a été annulée au cours de notifications. L'opération est encore exécutée de manière asynchrone.  
  
 E_FAIL  
 Une erreur spécifique au fournisseur s'est produite.  
  
 E_INVALIDARG  
 Le paramètre *hChapter* n’est pas DB_NULL_HCHAPTER, ou *eOperation* n’est pas DBASYNCH_OPEN.  
  
 E_UNEXPECTED  
 `ISSAsynchStatus::Abort` a été appelé sur un objet source de données sur lequel `IDBInitialize::Initialize` n’a pas été appelé ou ne s’est pas terminé.  
  
 `ISSAsynchStatus::Abort` a été appelé sur un objet source de données sur lequel `IDBInitialize::Initialize` a été appelé, puis annulé avant l’initialisation ou dont le délai d’attente a expiré. L'objet source de données n'est pas encore initialisé.  
  
 `ISSAsynchStatus::Abort` a été appelé sur un ensemble de lignes sur lequel `ITransaction::Commit` ou `ITransaction::Abort` a été appelé précédemment, et l’ensemble de lignes n’a pas survécu à la validation ou à l’abandon, et se trouve à l’état zombie.  
  
 `ISSAsynchStatus::Abort` a été appelé sur un ensemble de lignes qui a été annulé de manière asynchrone dans sa phase d'initialisation. L'ensemble de lignes se trouve dans un état zombie.  
  
## <a name="remarks"></a>Notes  
 L'abandon de l'initialisation d'un ensemble de lignes ou d'un objet source de données peut laisser l'ensemble de lignes ou l'objet source de données dans un état zombie, de telle sorte que toutes les méthodes autres que `IUnknown` retournent E_UNEXPECTED. Lorsque cela se produit, la seule action possible pour le consommateur est de libérer l'ensemble de lignes ou l'objet source de données.  
  
 Le fait d'appeler `ISSAsynchStatus::Abort` et de passer une valeur pour *eOperation* autre que DBASYNCHOP_OPEN retourne S_OK. Cette valeur n’implique pas pour autant que l’opération soit terminée ou annulée.  
  
## <a name="see-also"></a>Voir aussi  
 [Exécution d’opérations asynchrones](../../oledb/features/performing-asynchronous-operations.md)  
  
  
