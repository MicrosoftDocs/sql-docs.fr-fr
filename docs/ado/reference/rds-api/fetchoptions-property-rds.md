---
description: FetchOptions, propriété (RDS)
title: FetchOptions, propriété (RDS) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
helpviewer_keywords:
- FetchOptions property [ADO]
ms.assetid: 7b2e254a-9354-4541-bc98-bb185276388f
author: rothja
ms.author: jroth
ms.openlocfilehash: 2299e0b918206a1eadfa3d3352a01ec0d7320152
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100049409"
---
# <a name="fetchoptions-property-rds"></a>FetchOptions, propriété (RDS)
Indique le type d’extraction asynchrone.  
  
> [!IMPORTANT]
>  À compter de Windows 8 et de Windows Server 2012, les composants serveur RDS ne sont plus inclus dans le système d’exploitation Windows (pour plus d’informations, consultez le livre de recettes sur la compatibilité avec Windows 8 et [Windows server 2012](https://www.microsoft.com/download/details.aspx?id=27416) ). Les composants clients RDS seront supprimés dans une prochaine version de Windows. Évitez d'utiliser cette fonctionnalité dans de nouveaux travaux de développement, et prévoyez de modifier les applications qui utilisent actuellement cette fonctionnalité. Les applications qui utilisent RDS doivent migrer vers le [service de données WCF](/dotnet/framework/wcf/).  
  
## <a name="setting-and-return-values"></a>Définition et valeurs de retour  
 Définit ou retourne l’une des valeurs suivantes.  
  
|Constante|Description|  
|--------------|-----------------|  
|**adcFetchUpFront**|Tous les enregistrements du [Recordset](../ado-api/recordset-object-ado.md) sont récupérés avant que le contrôle soit retourné à l’application. L’intégralité du **Recordset** est extraite pour que l’application soit autorisée à faire quoi que ce soit.|  
|**adcFetchBackground**|Le contrôle peut retourner à l’application dès que le premier lot d’enregistrements a été extrait. Une lecture ultérieure du **Recordset** qui tente d’accéder à un enregistrement qui n’est pas récupéré dans le premier lot est retardée jusqu’à ce que l’enregistrement recherché soit réellement extrait, à partir duquel le contrôle retourne à l’application.|  
|**adcFetchAsync**|Par défaut. Le contrôle retourne immédiatement à l’application tandis que les enregistrements sont extraits en arrière-plan. Si l’application tente de lire un enregistrement qui n’a pas encore été extrait, l’enregistrement le plus proche de l’enregistrement recherché est lu et le contrôle est retourné immédiatement, ce qui indique que la fin actuelle du **Recordset** a été atteinte. Par exemple, un appel à [MoveLast](./movefirst-movelast-movenext-and-moveprevious-methods-rds.md) déplace la position de l’enregistrement actuel vers le dernier enregistrement effectivement extrait, même si un plus grand nombre d’enregistrements continueront à remplir le **Recordset**.|  
  
> [!NOTE]
>  Chaque fichier exécutable côté client qui utilise ces constantes doit fournir des déclarations pour ceux-ci. Vous pouvez couper et coller les déclarations de constantes de votre choix à partir du fichier Adcvbs. Inc, situé dans le dossier d’installation par défaut de la bibliothèque RDS.  
  
## <a name="remarks"></a>Notes  
 Dans une application Web, vous souhaiterez généralement utiliser **adcFetchAsync** (valeur par défaut), car il offre de meilleures performances. Dans une application cliente compilée, vous devez généralement utiliser **adcFetchBackground**.  
  
## <a name="applies-to"></a>S'applique à  
 [DataControl, objet (RDS)](./datacontrol-object-rds.md)  
  
## <a name="see-also"></a>Voir aussi  
 [ExecuteOptions et FetchOptions, exemples de propriétés (VBScript)](./executeoptions-and-fetchoptions-properties-example-vbscript.md)   
 [Cancel, méthode (RDS)](./cancel-method-rds.md)