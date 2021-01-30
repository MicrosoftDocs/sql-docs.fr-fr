---
description: SortColumn, propriété (RDS)
title: SortColumn, propriété (RDS) | Microsoft Docs
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.prod: sql
ms.prod_service: connectivity
ms.topic: reference
apitype: COM
helpviewer_keywords:
- SortColumn property [RDS]
ms.assetid: f6f80f67-f0fb-4e63-a5f5-8fdf312aac63
author: rothja
ms.author: jroth
ms.openlocfilehash: edbfa8f6c4dbb1296e9bf86aed43148bb7b12ec2
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99168779"
---
# <a name="sortcolumn-property-rds"></a>SortColumn, propriété (RDS)
Indique la colonne dans laquelle trier les enregistrements.  
  
> [!IMPORTANT]
>  À compter de Windows 8 et de Windows Server 2012, les composants serveur RDS ne sont plus inclus dans le système d’exploitation Windows (pour plus d’informations, consultez le livre de recettes sur la compatibilité avec Windows 8 et [Windows server 2012](https://www.microsoft.com/download/details.aspx?id=27416) ). Les composants clients RDS seront supprimés dans une prochaine version de Windows. Évitez d'utiliser cette fonctionnalité dans de nouveaux travaux de développement, et prévoyez de modifier les applications qui utilisent actuellement cette fonctionnalité. Les applications qui utilisent RDS doivent migrer vers le [service de données WCF](/dotnet/framework/wcf/).  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
DataControl.SortColumn = String  
```  
  
#### <a name="parameters"></a>Paramètres  
 *DataControl*  
 Variable objet qui représente un objet [RDS. DataControl](./datacontrol-object-rds.md) .  
  
 *Chaîne*  
 Valeur de **chaîne** qui représente le nom ou l’alias de la colonne selon laquelle trier les enregistrements.  
  
## <a name="remarks"></a>Notes  
 Les propriétés **SortColumn**, [SortDirection](./sortdirection-property-rds.md), [FilterValue](./filtervalue-property-rds.md), [FilterCriterion](./filtercriterion-property-rds.md)et [FilterColumn](./filtercolumn-property-rds.md) fournissent des fonctionnalités de tri et de filtrage sur le cache côté client. La fonctionnalité de tri commande les enregistrements par valeurs d’une colonne. La fonctionnalité de filtrage affiche un sous-ensemble d’enregistrements basés sur des critères de recherche, tandis que le [jeu d’enregistrements](../ado-api/recordset-object-ado.md) complet est conservé dans le cache. La méthode de [réinitialisation](./reset-method-rds.md) exécute les critères et remplace le **jeu d’enregistrements** actuel par un **jeu d’enregistrements** pouvant être mis à jour.  
  
 Pour effectuer un tri sur un **jeu d’enregistrements**, vous devez d’abord enregistrer toutes les modifications en attente. Si vous utilisez le **RDS. DataControl**, vous pouvez utiliser la méthode [SubmitChanges](./submitchanges-method-rds.md) . Par exemple, si votre **objet RDS. DataControl** est nommé ADC1, votre code est `ADC1.SubmitChanges` . Si vous utilisez un **jeu d’enregistrements** ADO, vous pouvez utiliser sa méthode [UpdateBatch](../ado-api/updatebatch-method.md) . L’utilisation de **UpdateBatch** est la méthode recommandée pour les objets **Recordset** créés avec la méthode [CreateRecordset](./createrecordset-method-rds.md) . Par exemple, votre code peut être `myRS.UpdateBatch` ou `ADC1.Recordset.UpdateBatch` .  
  
## <a name="applies-to"></a>S'applique à  
 [DataControl, objet (RDS)](./datacontrol-object-rds.md)  
  
## <a name="see-also"></a>Voir aussi  
 [FilterColumn, FilterCriterion, FilterValue, SortColumn et SortDirection propriétés et Reset, exemple de méthode (VBScript)](./filter-column-criterion-value-sortcolumn-sortdirection-example-vbscript.md)   
 [Sort, propriété](../ado-api/sort-property.md)   
 [SortDirection, propriété (RDS)](./sortdirection-property-rds.md)