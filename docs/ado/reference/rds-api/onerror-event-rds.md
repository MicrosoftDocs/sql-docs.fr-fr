---
description: onError, événement (RDS)
title: onError, événement (RDS) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
helpviewer_keywords:
- onError event [ADO]
ms.assetid: b01cbc62-fbd7-4068-b16c-8b0f80a05887
author: rothja
ms.author: jroth
ms.openlocfilehash: 2596f0ff667a682a16ccf1a9c5678e678e83342e
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99163721"
---
# <a name="onerror-event-rds"></a>onError, événement (RDS)
L’événement **OnError** est appelé chaque fois qu’une erreur se produit pendant une opération.  
  
> [!IMPORTANT]
>  À compter de Windows 8 et de Windows Server 2012, les composants serveur RDS ne sont plus inclus dans le système d’exploitation Windows (pour plus d’informations, consultez le livre de recettes sur la compatibilité avec Windows 8 et [Windows server 2012](https://www.microsoft.com/download/details.aspx?id=27416) ). Les composants clients RDS seront supprimés dans une prochaine version de Windows. Évitez d'utiliser cette fonctionnalité dans de nouveaux travaux de développement, et prévoyez de modifier les applications qui utilisent actuellement cette fonctionnalité. Les applications qui utilisent RDS doivent migrer vers le [service de données WCF](/dotnet/framework/wcf/).  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
onError SCode, Description, Source, CancelDisplay  
```  
  
#### <a name="parameters"></a>Paramètres  
 *SCode*  
 Entier qui indique le code d’état de l’erreur.  
  
 *Description*  
 **Chaîne** qui indique une description de l’erreur.  
  
 *Source*  
 **Chaîne** qui indique la requête ou la commande qui a provoqué l’erreur.  
  
 *CancelDisplay*  
 Valeur **booléenne** qui, si elle est définie sur **true**, empêche l’affichage de l’erreur dans une boîte de dialogue.  
  
## <a name="applies-to"></a>S'applique à  
 [DataControl, objet (RDS)](./datacontrol-object-rds.md)  
  
## <a name="see-also"></a>Voir aussi  
 [Exemple de modèle d’événements ADO (VC + +)](../ado-api/ado-events-model-example-vc.md)   
 [Présentation rapide du gestionnaire d’événements ADO](../../guide/data/ado-event-handler-summary.md)