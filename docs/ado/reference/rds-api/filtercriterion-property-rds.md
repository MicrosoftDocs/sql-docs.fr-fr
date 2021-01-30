---
description: FilterCriterion, propriété (RDS)
title: FilterCriterion, propriété (RDS) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
helpviewer_keywords:
- FilterCriterion property [RDS]
ms.assetid: 24eb03ba-ccfd-4353-b6af-03586b2da6fd
author: rothja
ms.author: jroth
ms.openlocfilehash: 2bb91a09167b10a7b5b1f969e2d0775f6ced31a4
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99168938"
---
# <a name="filtercriterion-property-rds"></a>FilterCriterion, propriété (RDS)
Indique l’opérateur d’évaluation à utiliser dans la valeur de filtre.  
  
> [!IMPORTANT]
>  À compter de Windows 8 et de Windows Server 2012, les composants serveur RDS ne sont plus inclus dans le système d’exploitation Windows (pour plus d’informations, consultez le livre de recettes sur la compatibilité avec Windows 8 et [Windows server 2012](https://www.microsoft.com/download/details.aspx?id=27416) ). Les composants clients RDS seront supprimés dans une prochaine version de Windows. Évitez d'utiliser cette fonctionnalité dans de nouveaux travaux de développement, et prévoyez de modifier les applications qui utilisent actuellement cette fonctionnalité. Les applications qui utilisent RDS doivent migrer vers le [service de données WCF](/dotnet/framework/wcf/).  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
DataControl.FilterCriterion = String  
```  
  
#### <a name="parameters"></a>Paramètres  
 *DataControl*  
 Variable objet qui représente un objet [RDS. DataControl](./datacontrol-object-rds.md) .  
  
 *Chaîne*  
 Valeur de **chaîne** qui spécifie l’opérateur d’évaluation de [FilterValue](./filtervalue-property-rds.md) pour les enregistrements. Il peut s’agir de l’un des éléments suivants : <, \<=, > , >=, = ou <>.  
  
## <a name="remarks"></a>Notes  
 Les propriétés [SortColumn](./sortcolumn-property-rds.md), [SortDirection](./sortdirection-property-rds.md), [FilterValue](./filtervalue-property-rds.md), **FilterCriterion** et [FilterColumn](./filtercolumn-property-rds.md) fournissent des fonctionnalités de tri et de filtrage sur le cache côté client. La fonctionnalité de tri commande les enregistrements par valeurs d’une colonne. La fonctionnalité de filtrage affiche un sous-ensemble d’enregistrements basés sur des critères de recherche, tandis que le [jeu d’enregistrements](../ado-api/recordset-object-ado.md) complet est conservé dans le cache. La méthode de [réinitialisation](./reset-method-rds.md) exécute les critères et remplace le **jeu d’enregistrements** actuel par un **jeu d’enregistrements** pouvant être mis à jour.  
  
 L’opérateur «  ! = » n’est pas valide pour **FilterCriterion**; Utilisez plutôt « <> ».  
  
 Si les propriétés Filter et sort sont toutes les deux définies et que vous appelez la méthode **Reset** , l’ensemble de lignes est tout d’abord filtré, puis trié. Pour les tris croissants, les valeurs NULL sont en haut ; pour les tris décroissants, les valeurs NULL sont en bas (le comportement croissant est le comportement par défaut).  
  
## <a name="applies-to"></a>S'applique à  
 [DataControl, objet (RDS)](./datacontrol-object-rds.md)  
  
## <a name="see-also"></a>Voir aussi  
 [FilterColumn, FilterCriterion, FilterValue, SortColumn et SortDirection propriétés et Reset, exemple de méthode (VBScript)](./filter-column-criterion-value-sortcolumn-sortdirection-example-vbscript.md)   
 [FilterColumn, propriété (RDS)](./filtercolumn-property-rds.md)   
 [FilterValue, propriété (RDS)](./filtervalue-property-rds.md)   
 [SortColumn, propriété (RDS)](./sortcolumn-property-rds.md)   
 [SortDirection, propriété (RDS)](./sortdirection-property-rds.md)