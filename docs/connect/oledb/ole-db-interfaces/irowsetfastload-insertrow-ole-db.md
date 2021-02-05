---
title: IRowsetFastLoad::InsertRow (pilote OLE DB) | Microsoft Docs
description: Découvrez comment la méthode IRowsetFastLoad::InsertRow ajoute une ligne à l’ensemble de lignes de copie en bloc dans OLE DB Driver pour SQL Server.
ms.custom: ''
ms.date: 06/14/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- IRowsetFastLoad::InsertRow (OLE DB)
apitype: COM
helpviewer_keywords:
- InsertRow method
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 650694069bb8019067f44c1a7aac593c27f048f9
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99183783"
---
# <a name="irowsetfastloadinsertrow-ole-db"></a>IRowsetFastLoad::InsertRow (OLE DB)
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

[!INCLUDE[Driver_OLEDB_Download](../../../includes/driver_oledb_download.md)]

  Ajoute une ligne à l'ensemble de lignes de copie en bloc. Pour obtenir des exemples, consultez [Copier des données en bloc avec IRowsetFastLoad &#40;OLE DB&#41;](../../oledb/ole-db-how-to/bulk-copy-data-using-irowsetfastload-ole-db.md) et [Envoyer des données BLOB vers SQL Server en utilisant IROWSETFASTLOAD et ISEQUENTIALSTREAM &#40;OLE DB&#41;](../../oledb/ole-db-how-to/send-blob-data-to-sql-server-using-irowsetfastload-and-isequentialstream-ole-db.md).  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
HRESULT InsertRow(  
      HACCESSOR hAccessor,  
      void* pData);  
```  
  
## <a name="arguments"></a>Arguments  
 *hAccessor*[in]  
 Handle de l'accesseur définissant les données de ligne pour la copie en bloc. L'accesseur référencé est un accesseur de ligne, liant la mémoire dont le consommateur est propriétaire et qui contient les valeurs des données.  
  
 *pData*[in]  
 Pointeur vers la mémoire dont le consommateur est propriétaire et qui contient les valeurs des données. Pour plus d'informations, consultez [Structures DBBINDING](/previous-versions/windows/desktop/ms716845(v=vs.85)).  
  
## <a name="return-code-values"></a>Codet de retour  
 S_OK  
 S_OK Les valeurs d'état liées de toutes les colonnes ont la valeur DBSTATUS_S_OK ou DBSTATUS_S_NULL.  
  
 E_FAIL  
 Une erreur est survenue. Les informations sur l'erreur sont disponibles depuis les interfaces d'erreur de l'ensemble de lignes.  
  
 E_INVALIDARG  
 L'argument *pData* a été défini sur un pointeur NULL.  
  
 E_OUTOFMEMORY  
 MSOLEDBSQL n’a pas pu allouer une mémoire suffisante pour traiter la requête.  
  
 E_UNEXPECTED  
 La méthode a été appelée sur un ensemble de lignes de copie en bloc précédemment invalidé par la méthode [IRowsetFastLoad::Commit](../../oledb/ole-db-interfaces/irowsetfastload-commit-ole-db.md).  
  
 DB_E_BADACCESSORHANDLE  
 L'argument *hAccessor* fourni par le consommateur n'était pas valide.  
  
 DB_E_BADACCESSORTYPE  
 L'accesseur spécifié n'était pas un accesseur de ligne ou ne spécifiait pas une mémoire dont le consommateur était propriétaire.  
  
## <a name="remarks"></a>Notes  
 Une erreur lors de la conversion des données du consommateur vers le type de données [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] pour une colonne provoque un retour E_FAIL du pilote OLE DB pour SQL Server. Les données peuvent être transmises à [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] sur n’importe quelle méthode **InsertRow** ou seulement sur la méthode **Commit**. L'application du consommateur peut appeler la méthode **InsertRow** plusieurs fois avec des données erronées avant de recevoir le message qu'une erreur de conversion de type de données s'est produite. Comme la méthode **Commit** garantit que toutes les données sont spécifiées correctement par le consommateur, celui-ci peut utiliser la méthode **Commit** de façon appropriée pour valider les données comme nécessaire.  
  
 Les ensembles de lignes de copie en bloc OLE DB Driver pour SQL Server sont en écriture seule. Le fournisseur OLE DB Driver pour SQL Server n'expose aucune méthode qui autorise l'interrogation de l'ensemble de lignes par le consommateur. Pour terminer le traitement, le consommateur peut libérer sa référence sur l’interface [IRowsetFastLoad](../../oledb/ole-db-interfaces/irowsetfastload-ole-db.md) sans appeler la méthode **Commit**. Il n'existe pas d'utilitaires permettant d'accéder à une ligne insérée par le consommateur dans l'ensemble de lignes et de modifier ses valeurs, ou de la supprimer individuellement de l'ensemble de lignes.  
  
 Les lignes copiées en bloc sont mises en forme sur le serveur pour [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Le format de ligne est affecté par les options qui ont pu être définies pour la connexion ou la session, telles que ANSI_PADDING. Cette option est activée par défaut pour toute connexion établie à travers le fournisseur OLE DB Driver pour SQL Server.  
  
## <a name="see-also"></a>Voir aussi  
 [IRowsetFastLoad &#40;OLE DB&#41;](../../oledb/ole-db-interfaces/irowsetfastload-ole-db.md)  
  
