---
title: IBCPSession::BCPColumns (pilote OLE DB) | Microsoft Docs
description: Découvrez comment la méthode IBCPSession::BCPColumns définit le nombre de champs à lier aux colonnes d’une table SQL Server dans OLE DB Driver pour SQL Server.
ms.custom: ''
ms.date: 06/14/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.technology: connectivity
ms.topic: reference
apiname:
- IBCPSession::BCPColumns (OLE DB)
apitype: COM
helpviewer_keywords:
- BCPColumns method
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: fd5ba895a531c180862bf25f88a6693b324d24bf
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99186265"
---
# <a name="ibcpsessionbcpcolumns-ole-db"></a>IBCPSession::BCPColumns (OLE DB)
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

[!INCLUDE[Driver_OLEDB_Download](../../../includes/driver_oledb_download.md)]

  Définit le nombre de champs qui doivent être liés aux colonnes dans une table [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] .  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
HRESULT BCPColumns(   
      DBCOUNTITEM nColumns);  
```  
  
## <a name="remarks"></a>Notes  
 En interne, il appelle [IBCPSession::BCPColFmt](../../oledb/ole-db-interfaces/ibcpsession-bcpcolfmt-ole-db.md) afin de définir les valeurs par défaut pour les données du champ. Ces valeurs par défaut sont obtenues à partir des informations de colonne SQL Server que le fournisseur récupère en interne lorsque le nom de la table est spécifié par le biais de [IBCPSession::BCPInit](../../oledb/ole-db-interfaces/ibcpsession-bcpinit-ole-db.md).  
  
> [!NOTE]  
>  Cette méthode peut être appelée uniquement après que **BCPInit** a été appelé avec un nom de fichier valide.  
  
 Vous devez appeler cette méthode uniquement si vous envisagez d'utiliser un format de fichier utilisateur qui diffère du format par défaut. Pour plus d'informations sur une description du format de fichier utilisateur par défaut, consultez la méthode **BCPInit** .  
  
 Après avoir appelé la méthode **BCPColumns** , vous devez appeler la méthode **BCPColFmt** pour chaque colonne dans le fichier utilisateur afin de définir complètement un format de fichier personnalisé.  
  
## <a name="arguments"></a>Arguments  
 *nColumns*[in]  
 Nombre total de champs dans le fichier utilisateur. Même si vous vous préparez à copier en bloc les données provenant du fichier utilisateur dans une table SQL Server et n'envisagez pas de copier tous les champs dans le fichier utilisateur, vous devez définir l'argument *nColumns* en spécifiant le nombre total de champs de fichier utilisateur. Les champs omis peuvent alors être spécifiés par le biais de **BCPColFmt**.  
  
## <a name="return-code-values"></a>Codet de retour  
 S_OK  
 S_OK  
  
 E_FAIL  
 Une erreur spécifique au fournisseur s’est produite. Pour obtenir des informations détaillées, utilisez l’interface [ISQLServerErrorInfo](./isqlservererrorinfo-geterrorinfo-ole-db.md).  
  
 E_UNEXPECTED  
 L'appel à la méthode était inattendu. Par exemple, la méthode **BCPInit** n'a pas été appelée avant cette méthode. Cela se produit également lorsque cette méthode est appelée plus d'une fois pour une opération de copie en bloc.  
  
 E_OUTOFMEMORY  
 Erreur de mémoire insuffisante.  
  
## <a name="see-also"></a>Voir aussi  
 [IBCPSession &#40;OLE DB&#41;](../../oledb/ole-db-interfaces/ibcpsession-ole-db.md)   
 [Exécution d'opérations de copie en bloc](../../oledb/features/performing-bulk-copy-operations.md)  
  
