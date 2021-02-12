---
title: Paramètres des extensions de remise de Reporting Services | Microsoft Docs
description: Reporting Services propose la remise par e-mail pour l’envoi de rapports aux utilisateurs et la remise par partage de fichiers pour l’envoi de rapports à un partage. Découvrez les paramètres d’extension de remise.
ms.date: 03/03/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-server-web-service
ms.topic: reference
helpviewer_keywords:
- XML Web service [Reporting Services], delivery extension settings
- Report Server Web service, delivery extension settings
- delivery [Reporting Services]
- network share delivery [Reporting Services]
- file share delivery [Reporting Services]
- e-mail [Reporting Services]
- mailing reports [Reporting Services]
- sharing reports
- extensions [Reporting Services], delivery
- mail [Reporting Services]
- Web service [Reporting Services], delivery extension settings
ms.assetid: 68c31a85-261c-4ec4-b8df-1f9842b46f8a
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 6249d9a0edc81dac3cfe384716d425e7ba0f83e8
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100063674"
---
# <a name="reporting-services-delivery-extension-settings"></a>Paramètres des extensions de remise de Reporting Services
  [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] propose une extension de remise par e-mail et une extension de remise par partage de fichiers. La remise par messagerie permet d'envoyer un rapport à des utilisateurs individuels ou à des groupes par le biais de la messagerie électronique. La remise par partage de fichiers permet d'envoyer automatiquement des rapports rendus à un partage de votre réseau. Vous pouvez utiliser l'une ou l'autre de ces extensions de remise avec des abonnements standard ou des abonnements piloté par les données. Vous devez transmettre les paramètres de remise qui sont spécifiques au type d'extension de remise chaque fois que vous appelez les méthodes <xref:ReportService2010.ReportingService2010.CreateSubscription%2A>, <xref:ReportService2010.ReportingService2010.CreateDataDrivenSubscription%2A>, <xref:ReportService2010.ReportingService2010.SetSubscriptionProperties%2A> et <xref:ReportService2010.ReportingService2010.SetDataDrivenSubscriptionProperties%2A>. Pour extraire par programme la liste des paramètres de remise, utilisez la méthode <xref:ReportService2010.ReportingService2010.GetExtensionSettings%2A>.  
  
> [!NOTE]  
>  Les paramètres d'extension de remise respectent la casse.  
  
## <a name="e-mail-delivery-settings"></a>Paramètres de remise par messagerie  
 Le tableau suivant répertorie les paramètres de remise par messagerie pour les abonnements qui utilisent la messagerie électronique du serveur de rapports.  
  
|Paramètre|Valeur|  
|-------------|-----------|  
|**TO**|Adresse e-mail qui s’affiche sur la ligne **À** de l’e-mail. Les adresses de messagerie multiples sont séparées par des points-virgules. Obligatoire.|  
|**CC**|Adresse e-mail qui s’affiche sur la ligne **Cc** de l’e-mail. Les adresses de messagerie multiples sont séparées par des points-virgules. facultatif.|  
|**BCC**|Adresse e-mail qui s’affiche sur la ligne **Cci** de l’e-mail. Les adresses de messagerie multiples sont séparées par des points-virgules. facultatif.|  
|**ReplyTo**|Adresse e-mail qui s’affiche dans l’en-tête **Répondre à** de l’e-mail. Il doit s'agir d'une adresse de messagerie unique. facultatif.|  
|**IncludeReport**|Valeur indiquant si le rapport doit être inclus dans la remise par messagerie. La valeur **true** indique que le rapport est remis dans le corps de l’e-mail.|  
|**RenderFormat**|Nom de l'extension de rendu à utiliser pour générer le rapport rendu. Ce nom doit correspondre à l'une des extensions de rendu visibles installées sur le serveur de rapports. Cette valeur est obligatoire si le paramètre **IncludeReport** est défini sur **true**.|  
|**Priorité**|Priorité avec laquelle le message électronique est envoyé. Les valeurs valides sont **LOW**, **NORMAL** et **HIGH**. La valeur par défaut est **NORMAL**.|  
|**Subject**|Texte figurant dans la ligne Objet du message électronique.|  
|**Commentaire**|Texte inclus dans le corps du message électronique.|  
|**IncludeLink**|Valeur qui indique si un lien vers le rapport doit être inclus dans le corps du message électronique.|  
  
## <a name="file-share-delivery-settings"></a>Paramètres de remise par partage de fichiers  
 Le tableau suivant répertorie les paramètres de remise par partage de fichiers pour les abonnements.  
  
|Paramètre|Valeur|  
|-------------|-----------|  
|**FILENAME**|Nom du fichier qui est enregistré sur le disque.|  
|**FILEEXTN**|Indique si une extension de fichier doit être incluse pour le rapport rendu. La valeur est soit **true**, soit **false**.|  
|**PATH**|Chemin d'accès au dossier ou chemin d'accès au partage de fichiers UNC dans lequel enregistrer le rapport.|  
|**RENDER_FORMAT**|Format du rapport qui est enregistré sur le disque.|  
|**USERNAME**|Nom d'utilisateur requis pour accéder à la ressource réseau ou au disque.|  
|**PASSWORD**|Mot de passe requis pour accéder à la ressource réseau ou au disque.|  
|**WRITEMODE**|Mode écriture à utiliser lors de l'accès au disque. Les valeurs valides sont **Aucun**, **Remplacer** et **Auto-incrément**.|  
  
## <a name="see-also"></a>Voir aussi  
 [Informations techniques de référence &#40;SSRS&#41;](../../../reporting-services/technical-reference-ssrs.md)   
 [Génération d’applications à l’aide du service web et de .NET Framework](../../../reporting-services/report-server-web-service/net-framework/building-applications-using-the-web-service-and-the-net-framework.md)  
  
  
