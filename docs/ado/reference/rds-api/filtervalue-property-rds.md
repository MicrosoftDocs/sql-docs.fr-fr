---
description: FilterValue, propriété (RDS)
title: FilterValue, propriété (RDS) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
helpviewer_keywords:
- FilterValue property [ADO]
ms.assetid: 28f17186-b842-4cf9-b320-a9bb941c481b
author: rothja
ms.author: jroth
ms.openlocfilehash: 44e1eca1ac4a949a3d46fe3a1ec58ff0c0b882dc
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100049389"
---
# <a name="filtervalue-property-rds"></a>FilterValue, propriété (RDS)
Indique la valeur avec laquelle filtrer les enregistrements.  
  
> [!IMPORTANT]
>  À compter de Windows 8 et de Windows Server 2012, les composants serveur RDS ne sont plus inclus dans le système d’exploitation Windows (pour plus d’informations, consultez le livre de recettes sur la compatibilité avec Windows 8 et [Windows server 2012](https://www.microsoft.com/download/details.aspx?id=27416) ). Les composants clients RDS seront supprimés dans une prochaine version de Windows. Évitez d'utiliser cette fonctionnalité dans de nouveaux travaux de développement, et prévoyez de modifier les applications qui utilisent actuellement cette fonctionnalité. Les applications qui utilisent RDS doivent migrer vers le [service de données WCF](/dotnet/framework/wcf/).  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
DataControl.FilterValue = String  
```  
  
#### <a name="parameters"></a>Paramètres  
 *DataControl*  
 Variable objet qui représente un objet [RDS. DataControl](./datacontrol-object-rds.md) .  
  
 *Chaîne*  
 Valeur de **chaîne** qui représente une valeur de données avec laquelle filtrer les enregistrements (par exemple, `'Programmer'` ou `125` ).  
  
## <a name="remarks"></a>Notes  
 Les propriétés [SortColumn](./sortcolumn-property-rds.md), [SortDirection](./sortdirection-property-rds.md), **FilterValue**, [FilterCriterion](./filtercriterion-property-rds.md)et [FilterColumn](./filtercolumn-property-rds.md) fournissent des fonctionnalités de tri et de filtrage sur le cache côté client. La fonctionnalité de tri commande les enregistrements par valeurs d’une colonne. La fonctionnalité de filtrage affiche un sous-ensemble d’enregistrements basés sur des critères de recherche, tandis que le [jeu d’enregistrements](../ado-api/recordset-object-ado.md) complet est conservé dans le cache. La méthode de [réinitialisation](./reset-method-rds.md) exécute les critères et remplace le **jeu d’enregistrements** actuel par un **jeu d’enregistrements** pouvant être mis à jour.  
  
 Les valeurs NULL entraînent une erreur d’incompatibilité de type.  
  
## <a name="applies-to"></a>S'applique à  
 [DataControl, objet (RDS)](./datacontrol-object-rds.md)  
  
## <a name="see-also"></a>Voir aussi  
 [FilterColumn, FilterCriterion, FilterValue, SortColumn et SortDirection propriétés et Reset, exemple de méthode (VBScript)](./filter-column-criterion-value-sortcolumn-sortdirection-example-vbscript.md)   
 [FilterColumn, propriété (RDS)](./filtercolumn-property-rds.md)   
 [FilterCriterion, propriété (RDS)](./filtercriterion-property-rds.md)   
 [SortColumn, propriété (RDS)](./sortcolumn-property-rds.md)   
 [SortDirection, propriété (RDS)](./sortdirection-property-rds.md)