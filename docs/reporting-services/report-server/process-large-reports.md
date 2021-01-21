---
title: Traiter les rapports volumineux | Microsoft Docs
description: Découvrez comment exécuter correctement des rapports volumineux dans le service Report Server en suivant les recommandations relatives à la configuration, au rendu, au déploiement et à la distribution.
ms.date: 06/10/2019
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-server
ms.topic: conceptual
helpviewer_keywords:
- report processing [Reporting Services], large reports
- page breaks [Reporting Services]
- large reports
- size [SQL Server], reports
- distributing reports [Reporting Services], large reports
ms.assetid: c5275a9f-c95b-46d7-bc62-633879a8a291
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 022751a3ec4b47a8e7a3a5f5295306265f659512
ms.sourcegitcommit: d8cdbb719916805037a9167ac4e964abb89c3909
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/20/2021
ms.locfileid: "98594983"
---
# <a name="process-large-reports"></a>Traiter les rapports volumineux
  Les rapports volumineux présentent certains problèmes de traitement. Ils nécessitent un certain nombre de configurations pour garantir leur fonctionnement correct. Ils ne doivent pas être exécutés à la demande, à moins d'être configurés pour prendre en charge la pagination.  
  
> [!NOTE]  
>  Les sauts de page sont activés par défaut. Si vous pensez que le rapport peut contenir une grande quantité de données, ne désactivez pas cette option. Le format de rendu HTML utilisé pour le rendu initial d'un rapport ouvre le fichier dans un navigateur. Si le rapport n'est pas paginé, toutes les données sont contenues dans une seule page, ce qui s'avère impossible à gérer pour bien des navigateurs. Ainsi, il est pratiquement improbable qu'un rapport contenant 5 000 lignes de données puisse être affiché en une seule page dans un navigateur.  
  
 Si vous travaillez sur un rapport volumineux, vous devez choisir les options d'exécution, de rendu et de remise convenant aux documents de grande taille. La taille des rapports est en grande partie définie par l'ensemble des lignes renvoyées par la requête et l'extension de rendu utilisé pour présenter le rapport.  
  
 La taille des rapports contenant des données volatiles peut changer considérablement d'une exécution à l'autre. Dans ce cas, vous devez analyser la source des données pour déterminer de quelle façon la volatilité des données agit sur ces rapports, afin de savoir si vous devez suivre les recommandations prodiguées dans la présente rubrique.  
  
 Pour plus d’informations et de conseils sur le diagnostic des erreurs de délai d’attente et des erreurs d’insuffisance de mémoire, consultez l’article [Comment diagnostiquer des problèmes lors de l'exécution de rapports sur le serveur de rapports](/archive/blogs/lukaszp/) sur blogs.msdn.microsoft.com.  
  
## <a name="configuration-recommendations"></a>Recommandations relatives à la configuration  
 Les recommandations concernant l'accès, l'exécution et le rendu des rapports sont les suivantes :  
  
-   Concevez le rapport pour la prise en charge de la pagination. Le serveur de rapports renvoie un rapport, une page à la fois. Si le rapport contient une pagination, vous pouvez contrôler la quantité de données transmises au navigateur. Pour plus d’informations, consultez [Précharger le cache (SSRS)](../../reporting-services/report-server/preload-the-cache-report-manager.md).  
  
-   Configurez le rapport pour qu'il s'exécute en tant qu'instantané de rapport planifié et ainsi empêcher son exécution à la demande. Ne définissez pas de délai d'expiration pour l'exécution du rapport. Exécutez le rapport durant les heures creuses.  
  
-   Configurez le rapport de telle sorte qu'il utilise une source de données partagée si vous voulez contrôler l'activation de son traitement. L'intérêt d'utiliser une source de données partagée est de pouvoir la désactiver. Le fait de désactiver la source de données empêche le traitement du rapport.  
  
-   Désactivez l'historique de rapport pour économiser de l'espace sur le disque. Pour désactiver l'historique de rapport, désactivez toutes les cases à cocher dans la page des propriétés de l'historique.  
  
-   Limitez l'accès au rapport. Configurez le rapport pour utiliser la sécurité au niveau de l'élément et remplacez les attributions de rôles par défaut par des rôles nouveaux autorisant l'accès exclusivement aux utilisateurs qui en ont besoin.  
  
     Par défaut, les utilisateurs peuvent ouvrir tous les rapports qu'ils peuvent afficher dans l'arborescence des dossiers. Même si vous configurez un rapport pour qu'il s'exécute en tant qu'instantané, les utilisateurs qui peuvent voir ce rapport dans un dossier peuvent l'ouvrir. Si le rapport est très volumineux, il peut provoquer le blocage du navigateur lorsqu'il est ouvert par un utilisateur dans le portail web.  
  
## <a name="rendering-recommendations"></a>Recommandations relatives au rendu  
 Avant de configurer la distribution d'un rapport, il est important de savoir quels sont les clients de rendu qui acceptent les documents volumineux. L'extension de rendu HTML par défaut à saut de page automatique est le format recommandé, mais vous pouvez choisir n'importe quel autre format prenant en charge la pagination.  
  
 Les performances du système et l'utilisation de la mémoire varient d'un format de rendu à l'autre. Un même rapport sera rendu à des vitesses et des quantités de mémoire différentes selon le format que vous sélectionnez. Les formats les plus rapides et les moins consommateurs en mémoire sont : CSV, XML et HTML. Les formats PDF et Excel affichent les performances les plus lentes, mais pour des raisons différentes. Le format PDF consomme une grande quantité de ressources de l'UC tandis que le format Excel préfère la mémoire vive. Le rendu d'image se situe entre ces deux tendances. Vous pouvez spécifier ce format lorsque vous définissez le mode de distribution du rapport.  
  
## <a name="deployment-and-distribution-recommendations"></a>Recommandations relatives au déploiement et à la distribution  
 En utilisant des sauts de page pour contrôler le rendu des rapports, vous pouvez déployer un rapport volumineux de la même façon que n'importe quel autre rapport. Prévoyez un accès au rapport via le portail web, un composant WebPart SharePoint ou une URL que vous ajoutez à un portail ou un site Web. Toutes ces options de déploiement gèrent aussi bien l'accès à la demande qu'un instantané de rapport précédemment exécutée.  
  
 Une autre stratégie de déploiement consiste à distribuer les rapports à des utilisateurs individuels. Il est possible de distribuer des rapports volumineux par le biais d'abonnements, en étant très prudent dans la manière de configurer les options de remise. Vous pouvez utiliser un abonnement standard ou un abonnement piloté par les données pour remettre un rapport. Les recommandations relatives aux abonnements et à la remise des rapports sont les suivantes :  
  
-   Configurez un abonnement pour utiliser des fichiers d'archive Web (MHTML), PDF ou Excel.  
  
-   Configurez un abonnement pour utiliser la remise dans le partage de fichiers si vous choisissez les formats PDF ou Excel. Une fois le rapport remis, utilisez une application bureautique pour travailler sur le rapport. Vous devez définir des autorisations sur le partage de fichiers pour déterminer quels utilisateurs seront autorisés à afficher le rapport.  
  
     Remarque : une fois qu'un rapport se trouve sur un partage de fichiers, il ne fait plus l'objet d'aucun contrôle par [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]et n'est plus sécurisé. Si vous souhaitez recevoir une notification lorsqu'un rapport est mis à jour, créez un second abonnement qui utilise la remise par messagerie avec envoi de notification uniquement.  
  
 Si vous voulez utiliser la remise de rapport par courrier électronique, configurez l'abonnement pour inclure un lien. Évitez d'envoyer le rapport sous forme de pièce jointe.  
  
## <a name="see-also"></a>Voir aussi  
 [Abonnements et remise &#40;Reporting Services&#41;](../../reporting-services/subscriptions/subscriptions-and-delivery-reporting-services.md)   
 [Définir les propriétés de traitement d’un rapport](../../reporting-services/report-server/set-report-processing-properties.md)   
 [Spécifier des informations d'identification et de connexion pour les sources de données de rapport](../../reporting-services/report-data/specify-credential-and-connection-information-for-report-data-sources.md)   
 [Gestion du contenu du serveur de rapports &#40;SSRS en mode natif&#41;](../../reporting-services/report-server/report-server-content-management-ssrs-native-mode.md)   
 [Précharger le cache (SSRS)](../../reporting-services/report-server/preload-the-cache-report-manager.md)