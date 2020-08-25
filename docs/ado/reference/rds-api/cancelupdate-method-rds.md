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
ms.openlocfilehash: 2b0234a240d863f52d33fe1eb230bcfc11eadf06
ms.sourcegitcommit: 7345e4f05d6c06e1bcd73747a4a47873b3f3251f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/24/2020
ms.locfileid: "88768748"
---
# <a name="cancelupdate-method-rds"></a>CancelUpdate, méthode (RDS)
Annule toutes les modifications apportées à la ligne actuelle ou nouvelle d’un objet [Recordset](../ado-api/recordset-object-ado.md) .  
  
> [!IMPORTANT]
>  À compter de Windows 8 et de Windows Server 2012, les composants serveur RDS ne sont plus inclus dans le système d’exploitation Windows (pour plus d’informations, consultez le livre de recettes sur la compatibilité avec Windows 8 et [Windows server 2012](https://www.microsoft.com/download/details.aspx?id=27416) ). Les composants clients RDS seront supprimés dans une prochaine version de Windows. Évitez d'utiliser cette fonctionnalité dans de nouveaux travaux de développement, et prévoyez de modifier les applications qui utilisent actuellement cette fonctionnalité. Les applications qui utilisent RDS doivent migrer vers le [service de données WCF](https://go.microsoft.com/fwlink/?LinkId=199565).  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
DataControl.CancelUpdate  
```  
  
#### <a name="parameters"></a>Paramètres  
 *DataControl*  
 Variable objet qui représente un objet [RDS. DataControl](./datacontrol-object-rds.md) .  
  
## <a name="remarks"></a>Notes  
 Le service de curseur pour OLE DB conserve une copie des valeurs d’origine et un cache de modifications. Lorsque vous appelez **CancelUpdate**, le cache des modifications est réinitialisé à vide et tous les contrôles liés sont actualisés avec les données d’origine.  
  
## <a name="applies-to"></a>S'applique à  
 [DataControl, objet (RDS)](./datacontrol-object-rds.md)  
  
## <a name="see-also"></a>Voir aussi  
 [CancelUpdate, exemple de méthode (VBScript)](./cancelupdate-method-example-vbscript.md)   
 [Boutons de commande du carnet d’adresses](../../guide/remote-data-service/address-book-command-buttons.md)   
 [Cancel, méthode (ADO)](../ado-api/cancel-method-ado.md)   
 [Cancel, méthode (RDS)](./cancel-method-rds.md)   
 [Méthode CancelBatch (ADO)](../ado-api/cancelbatch-method-ado.md)   
 [CancelUpdate, méthode (ADO)](../ado-api/cancelupdate-method-ado.md)   
 [Refresh, méthode (RDS)](./refresh-method-rds.md)   
 [SubmitChanges, méthode (RDS)](./submitchanges-method-rds.md)