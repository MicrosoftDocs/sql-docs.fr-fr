---
description: CancelUpdate, méthode (RDS)
title: CancelUpdate, méthode (RDS) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
apitype: COM
helpviewer_keywords:
- CancelUpdate method [RDS]
ms.assetid: 76d8a6e9-bc6c-4ea0-8e7a-2bae5ed06650
author: rothja
ms.author: jroth
ms.openlocfilehash: 7e3f7180b812aab47f9388e25a64d9445e9b5c9e
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/17/2020
ms.locfileid: "88439231"
---
# <a name="cancelupdate-method-rds"></a>CancelUpdate, méthode (RDS)
Annule toutes les modifications apportées à la ligne actuelle ou nouvelle d’un objet [Recordset](../../../ado/reference/ado-api/recordset-object-ado.md) .  
  
> [!IMPORTANT]
>  À compter de Windows 8 et de Windows Server 2012, les composants serveur RDS ne sont plus inclus dans le système d’exploitation Windows (pour plus d’informations, consultez le livre de recettes sur la compatibilité avec Windows 8 et [Windows server 2012](https://www.microsoft.com/download/details.aspx?id=27416) ). Les composants clients RDS seront supprimés dans une prochaine version de Windows. Évitez d'utiliser cette fonctionnalité dans de nouveaux travaux de développement, et prévoyez de modifier les applications qui utilisent actuellement cette fonctionnalité. Les applications qui utilisent RDS doivent migrer vers le [service de données WCF](https://go.microsoft.com/fwlink/?LinkId=199565).  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
DataControl.CancelUpdate  
```  
  
#### <a name="parameters"></a>Paramètres  
 *DataControl*  
 Variable objet qui représente un objet [RDS. DataControl](../../../ado/reference/rds-api/datacontrol-object-rds.md) .  
  
## <a name="remarks"></a>Notes  
 Le service de curseur pour OLE DB conserve une copie des valeurs d’origine et un cache de modifications. Lorsque vous appelez **CancelUpdate**, le cache des modifications est réinitialisé à vide et tous les contrôles liés sont actualisés avec les données d’origine.  
  
## <a name="applies-to"></a>S'applique à  
 [DataControl, objet (RDS)](../../../ado/reference/rds-api/datacontrol-object-rds.md)  
  
## <a name="see-also"></a>Voir aussi  
 [CancelUpdate, exemple de méthode (VBScript)](../../../ado/reference/rds-api/cancelupdate-method-example-vbscript.md)   
 [Boutons de commande du carnet d’adresses](../../../ado/guide/remote-data-service/address-book-command-buttons.md)   
 [Cancel, méthode (ADO)](../../../ado/reference/ado-api/cancel-method-ado.md)   
 [Cancel, méthode (RDS)](../../../ado/reference/rds-api/cancel-method-rds.md)   
 [Méthode CancelBatch (ADO)](../../../ado/reference/ado-api/cancelbatch-method-ado.md)   
 [CancelUpdate, méthode (ADO)](../../../ado/reference/ado-api/cancelupdate-method-ado.md)   
 [Refresh, méthode (RDS)](../../../ado/reference/rds-api/refresh-method-rds.md)   
 [SubmitChanges, méthode (RDS)](../../../ado/reference/rds-api/submitchanges-method-rds.md)


