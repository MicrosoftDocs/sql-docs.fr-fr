---
description: Configuration de RDS sur Windows 2000
title: Configuration des services Bureau à distance sur Windows 2000 | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 11/09/2018
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- RDS configuration [ADO], Windows 2000
ms.assetid: ef37e858-c05f-4f52-a65f-3ce6037e0d03
author: rothja
ms.author: jroth
ms.openlocfilehash: caa8fe846651f65e4316c81f29f2f18078628658
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100036569"
---
# <a name="configuring-rds-on-windows-2000"></a>Configuration de RDS sur Windows 2000
Si vous rencontrez des difficultés pour que RDS fonctionne correctement après la mise à niveau vers Windows 2000, procédez comme suit pour résoudre le problème :  
  
1.  Assurez-vous que le service de publication World Wide Web s’exécute en premier en accédant à https://Server à l’aide d’Internet Explorer. Si vous ne pouvez pas accéder au serveur Web de cette manière, ouvrez une invite de commandes et entrez la commande suivante, « NET START W3SVC ».  
  
2.  Dans le menu Démarrer, sélectionnez Exécuter. Tapez msdfmap.ini, puis cliquez sur OK pour ouvrir le fichier msdfmap.ini dans le bloc-notes. Vérifiez la section [CONNECT DEFAULT] et, si le paramètre d’accès a la valeur NoAccess, remplacez-le par READONLY.  
  
3.  À l’aide de l’utilitaire RegEdit, accédez à « HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\DataFactory\HandlerInfo » et assurez-vous que **HandlerRequired** a la valeur 0 et que le **DefaultHandler** est «» (chaîne null).  
  
     **Remarque** Si vous apportez des modifications à cette section du Registre, vous devez arrêter et redémarrer le service de publication World Wide Web en entrant les commandes suivantes à l’invite de commandes : « NET STOP W3SVC » et « NET START W3SVC ».  
  
4.  À l’aide de l’utilitaire RegEdit, accédez au registre « HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\W3SVC\Parameters\ADCLaunch » et vérifiez qu’il existe une clé nommée **RDSServer. DataFactory**. Si ce n’est pas le cas, créez-le.  
  
5.  À l’aide de Gestionnaire des services Internet, recherchez le site Web par défaut et affichez les propriétés de la racine virtuelle MSADC. Examinez les restrictions relatives à la sécurité du répertoire, à l’adresse IP et au nom de domaine. Si l’option « accès refusé » est cochée, sélectionnez « accordé ».  
  
 Veillez à essayer de redémarrer le serveur si les modifications apportées à ne s’affichent pas pour résoudre le problème au préalable.  
  
> [!IMPORTANT]
>  À compter de Windows 8 et de Windows Server 2012, les composants serveur RDS ne sont plus inclus dans le système d’exploitation Windows (pour plus d’informations, consultez le livre de recettes sur la compatibilité avec Windows 8 et [Windows server 2012](https://www.microsoft.com/download/details.aspx?id=27416) ). Les composants clients RDS seront supprimés dans une prochaine version de Windows. Évitez d'utiliser cette fonctionnalité dans de nouveaux travaux de développement, et prévoyez de modifier les applications qui utilisent actuellement cette fonctionnalité. Les applications qui utilisent RDS doivent migrer vers le [service de données WCF](/dotnet/framework/wcf/). À compter de Windows 8 et de Windows Server 2012, les composants serveur RDS ne sont plus inclus dans le système d’exploitation Windows. Migrez les applications qui utilisent RDS vers le [service de données WCF](/dotnet/framework/wcf/).  
  
## <a name="see-also"></a>Voir aussi  
 [Concepts de base de RDS](./rds-fundamentals.md)