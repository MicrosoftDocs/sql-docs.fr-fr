---
description: 'Erreur du serveur Internet : accès refusé'
title: 'Erreur du serveur Internet : accès refusé | Microsoft Docs'
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 11/09/2018
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- access denied error in RDS [ADO]
ms.assetid: e5b43cfa-da8d-430d-a2ab-5443dda47a16
author: rothja
ms.author: jroth
ms.openlocfilehash: f0fa2a2e3ee7d28bab239980710440cbd375d52a
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100036479"
---
# <a name="internet-server-error-access-denied"></a>Erreur du serveur Internet : accès refusé
Si vous recevez cette erreur, cela signifie généralement que Microsoft Internet Information Services (IIS) a retourné l’état suivant :  
  
 HTTP_STATUS_DENIED 401  
  
 Assurez-vous que les répertoires accessibles par IIS disposent des autorisations appropriées. Les services Bureau à distance peuvent communiquer avec un serveur Web IIS exécutant dans l’un des trois modes d’authentification de mot de passe : anonyme, de base ou stimulation/réponse NT (appelée authentification Windows intégrée dans Windows 2000). En outre, le serveur Web doit avoir des autorisations sur l’ordinateur source de données s’il s’agit d’un ordinateur Windows NT/Windows 2000.  
  
> [!IMPORTANT]
>  À compter de Windows 8 et de Windows Server 2012, les composants serveur RDS ne sont plus inclus dans le système d’exploitation Windows (pour plus d’informations, consultez le livre de recettes sur la compatibilité avec Windows 8 et [Windows server 2012](https://www.microsoft.com/download/details.aspx?id=27416) ). Les composants clients RDS seront supprimés dans une prochaine version de Windows. Évitez d'utiliser cette fonctionnalité dans de nouveaux travaux de développement, et prévoyez de modifier les applications qui utilisent actuellement cette fonctionnalité. Les applications qui utilisent RDS doivent migrer vers le [service de données WCF](/dotnet/framework/wcf/).  
  
## <a name="see-also"></a>Voir aussi  
 [Concepts de base de RDS](./rds-fundamentals.md)