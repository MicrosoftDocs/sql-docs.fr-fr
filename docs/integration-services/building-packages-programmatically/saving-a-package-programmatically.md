---
description: Enregistrement d'un package par programme
title: Enregistrement d’un package par programmation | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: reference
helpviewer_keywords:
- programmatically saving a package
- saving a package programmatically
ms.assetid: 4204f817-d5df-475a-9338-d7f01305d566
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 4ce0606f8d63015be37b5230a47cb66c82cf1a20
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/26/2020
ms.locfileid: "88352105"
---
# <a name="saving-a-package-programmatically"></a>Enregistrement d'un package par programme

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  Après avoir généré un package par programme, ou modifié un package existant, vous souhaitez généralement enregistrer vos modifications.  
  
 Toutes les méthodes de cette rubrique qui permettent d'enregistrer des packages requièrent une référence à l'assembly **Microsoft.SqlServer.ManagedDTS** . Après avoir ajouté la référence dans un nouveau projet, importez l’espace de noms <xref:Microsoft.SqlServer.Dts.Runtime> à l’aide d’une instruction **using** ou **Imports**.  
  
## <a name="saving-a-package-programmatically"></a>Enregistrement d'un package par programme  
 Pour enregistrer un package programmatiquement, appelez l’une des méthodes suivantes de la classe [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]  <xref:Microsoft.SqlServer.Dts.Runtime.Application> :  
  
|Emplacement de stockage|Méthode à appeler|  
|----------------------|--------------------|  
|Fichier|<xref:Microsoft.SqlServer.Dts.Runtime.Application.SaveToXml%2A>|  
|Magasin de packages SSIS|<xref:Microsoft.SqlServer.Dts.Runtime.Application.SaveToDtsServer%2A>|  
|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]|<xref:Microsoft.SqlServer.Dts.Runtime.Application.SaveToSqlServer%2A><br /><br /> ou<br /><br /> <xref:Microsoft.SqlServer.Dts.Runtime.Application.SaveToSqlServerAs%2A>|  
  
> [!IMPORTANT]  
>  Les méthodes de la classe <xref:Microsoft.SqlServer.Dts.Runtime.Application> qui permettent d'utiliser le magasin de packages SSIS prennent uniquement en charge « . » ou le nom du serveur local. Vous ne pouvez pas utiliser « (local) » ou « localhost ».  
  
## <a name="see-also"></a>Voir aussi  
 [Enregistrer des packages](../../integration-services/save-packages.md)  
  
  
