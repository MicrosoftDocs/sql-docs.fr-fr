---
title: Utilisation de la classe Setting pour une extension de remise | Microsoft Docs
description: Découvrez comment les extensions de remise peuvent utiliser la classe Paramètre qui stocke les informations dont l’extension de remise a besoin pour remettre des notifications et des rapports.
ms.date: 03/06/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: extensions
ms.topic: reference
helpviewer_keywords:
- delivery extensions [Reporting Services], settings
- Setting class
ms.assetid: 50746639-da7c-46a5-ac7b-e87dd6b91880
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: d90728a83821575bd71c3fe93fb1a090beaf8bdb
ms.sourcegitcommit: 2f166e139f637d6edfb5731510d632a13205eb25
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/08/2020
ms.locfileid: "84529095"
---
# <a name="using-the-setting-class-for-a-delivery-extension"></a>Utilisation de la classe Setting pour une extension de remise
  La classe <xref:Microsoft.ReportingServices.Interfaces.Setting> se trouve dans l'espace de noms <xref:Microsoft.ReportingServices.Interfaces> et représente les informations relatives aux paramètres d'une extension de remise. La classe <xref:Microsoft.ReportingServices.Interfaces.Setting> fournit l'infrastructure de stockage des informations sur les paramètres nécessaires au bon fonctionnement d'une extension de remise. Par exemple, dans l'extension de remise par messagerie du serveur de rapports, un utilisateur est tenu de fournir des paramètres spécifiques à la remise par messagerie, tels que l'adresse du destinataire, l'adresse de l'expéditeur ou la ligne d'objet du message électronique. Vos fournisseurs de remise personnalisés demanderont sans aucun doute à l'utilisateur de fournir des paramètres spécifiques afin que l'extension de remise puisse remettre des notifications et des rapports.  
  
 La classe <xref:Microsoft.ReportingServices.Interfaces.Setting> est utilisée lors de l'implémentation de la propriété <xref:Microsoft.ReportingServices.Interfaces.IDeliveryExtension.ExtensionSettings%2A> de l'interface <xref:Microsoft.ReportingServices.Interfaces.IDeliveryExtension>. La classe <xref:Microsoft.ReportingServices.Interfaces.Setting> est également utilisée pour traiter les données de paramètre d'extension fournies par un utilisateur lorsqu'un abonnement ou la notification est créé(e).  
  
 Pour un exemple d’utilisation de la classe <xref:Microsoft.ReportingServices.Interfaces.Setting>, consultez [SQL Server Reporting Services Product Samples](https://go.microsoft.com/fwlink/?LinkId=177889) (Exemples Reporting Services pour le produit SQL Server).  
  
## <a name="see-also"></a>Voir aussi  
 [Implémentation d’une extension de remise](../../../reporting-services/extensions/delivery-extension/implementing-a-delivery-extension.md)   
 [Bibliothèque d'extensions Reporting Services](../../../reporting-services/extensions/reporting-services-extension-library.md)  
  
  
