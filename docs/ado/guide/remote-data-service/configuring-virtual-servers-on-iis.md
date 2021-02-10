---
description: Configuration de serveurs virtuels sur IIS
title: Configuration de serveurs virtuels sur IIS | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 11/09/2018
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- virtual servers in RDS [ADO]
ms.assetid: 2b4786c6-40c4-4ce1-9ad4-03df436e0aff
author: rothja
ms.author: jroth
ms.openlocfilehash: 686c6b8306486c7ede2cd197190a165460b397ac
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100036519"
---
# <a name="configuring-virtual-servers-on-iis"></a>Configuration de serveurs virtuels sur IIS
Lors de la création de serveurs virtuels dans Internet Information Services 4,0, les deux étapes supplémentaires suivantes sont nécessaires pour configurer le serveur virtuel de façon à ce qu’il fonctionne avec les services Bureau à distance :  
  
1.  Quand vous configurez le serveur, cochez la case « autoriser l’accès en exécution ».  
  
2.  Déplacez msadcs.dll vers *vroot*\MSADC, où *vroot* est le répertoire de départ de votre serveur virtuel.  
  
> [!IMPORTANT]
>  À compter de Windows 8 et de Windows Server 2012, les composants serveur RDS ne sont plus inclus dans le système d’exploitation Windows (pour plus d’informations, consultez le livre de recettes sur la compatibilité avec Windows 8 et [Windows server 2012](https://www.microsoft.com/download/details.aspx?id=27416) ). Les composants clients RDS seront supprimés dans une prochaine version de Windows. Évitez d'utiliser cette fonctionnalité dans de nouveaux travaux de développement, et prévoyez de modifier les applications qui utilisent actuellement cette fonctionnalité. Les applications qui utilisent RDS doivent migrer vers le [service de données WCF](/dotnet/framework/wcf/).  
  
## <a name="see-also"></a>Voir aussi  
 [Concepts de base de RDS](./rds-fundamentals.md)