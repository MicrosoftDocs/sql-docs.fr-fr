---
description: Inscription d’un objet métier personnalisé
title: Inscription d’un objet métier personnalisé | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 11/09/2018
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- custom business object in RDS [ADO]
- registering custom business objects in RDS [ADO]
- business objects in RDS [ADO]
ms.assetid: e9032ad8-d14c-42e3-ba13-cb5f00084a79
author: rothja
ms.author: jroth
ms.openlocfilehash: 76f3311a622abbd249aa942e1ebdbda33ad06966
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100036419"
---
# <a name="registering-a-custom-business-object"></a>Inscription d’un objet métier personnalisé
Pour lancer correctement un objet métier personnalisé (. dll ou. exe) par le biais du serveur Web, vous devez entrer le ProgID de l’objet métier dans le registre, comme expliqué dans cette procédure. Cette fonctionnalité RDS protège la sécurité de votre serveur Web en exécutant uniquement les exécutables approuvés.  
  
> [!IMPORTANT]
>  À compter de Windows 8 et de Windows Server 2012, les composants serveur RDS ne sont plus inclus dans le système d’exploitation Windows (pour plus d’informations, consultez le livre de recettes sur la compatibilité avec Windows 8 et [Windows server 2012](https://www.microsoft.com/download/details.aspx?id=27416) ). Les composants clients RDS seront supprimés dans une prochaine version de Windows. Évitez d'utiliser cette fonctionnalité dans de nouveaux travaux de développement, et prévoyez de modifier les applications qui utilisent actuellement cette fonctionnalité. Les applications qui utilisent RDS doivent migrer vers le [service de données WCF](/dotnet/framework/wcf/).  
  
> [!NOTE]
>  Pour MDAC 2,0 et versions ultérieures et Windows DAC, l’objet métier par défaut, [RDSServer. DataFactory](../../reference/rds-api/datafactory-object-rdsserver.md), n’est pas inscrit par défaut au cours de l’installation de MDAC/Windows DAC. Toutefois, si **RDSServer. DataFactory** a été inscrit comme sécurisé pour une exécution sur l’ordinateur avant l’installation, l’entrée de Registre est conservée pour la nouvelle installation.  
  
### <a name="to-register-a-custom-business-object"></a>Pour inscrire un objet métier personnalisé :  
  
1.  Cliquez sur **Démarrer** , puis sur **exécuter**.  
  
2.  Tapez **regedit** et cliquez sur **OK**.  
  
3.  Dans l’éditeur du Registre, accédez à la clé de Registre **HKEY_LOCAL_MACHINE\System\CurrentControlSet\Services\W3SVC\Parameters\ADCLaunch** .  
  
4.  Sélectionnez la clé **ADCLaunch** , puis dans le menu **Edition**, pointez sur **nouveau** , puis cliquez sur **clé**.  
  
5.  Tapez le ProgID de votre objet métier personnalisé, puis cliquez sur **entrée**. Ne renseignez pas l’entrée **valeur** .