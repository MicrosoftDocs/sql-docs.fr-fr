---
title: Créer et gérer des abonnements pour des serveurs de rapports en mode SharePoint | Microsoft Docs
description: Découvrez comment créer un abonnement Reporting Services pour remettre des rapports à partir d’une application web SharePoint qui est intégrée à un serveur de rapports en mode SharePoint.
ms.date: 03/07/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: subscriptions
ms.topic: conceptual
helpviewer_keywords:
- subscriptions [Reporting Services], creating
- subscriptions [Reporting Services], deleting
- subscriptions [Reporting Services], managing
ms.assetid: 44be7ee2-33ce-46e4-9d1a-a20aaf43a227
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: bf68742926ef0f26c5a12a5d001ea9b14f9b199c
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100080091"
---
# <a name="create-and-manage-subscriptions-for-sharepoint-mode-report-servers"></a>Créer et gérer des abonnements pour des serveurs de rapports en mode SharePoint
  Vous pouvez créer des abonnements [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] pour remettre les rapports à partir d’une application web SharePoint qui est intégrée à un serveur de rapports en mode SharePoint. Les abonnements peuvent remettre des rapports dans une bibliothèque de documents, un dossier de fichiers ou sous forme de courrier électronique. Cette rubrique résume les conditions requises et les étapes de création d’un abonnement [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] .  
  
||  
|-|  
|**[!INCLUDE[applies](../../includes/applies-md.md)]**<br /><br /> [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] Mode SharePoint &#124; SharePoint 2010 et SharePoint 2013|  
  
 Lorsque vous créez un abonnement, il existe trois méthodes pour spécifier sa remise :  
  
-   **Bibliothèque de documents**  : vous pouvez créer un abonnement qui remet un document basé sur le rapport d’origine dans une bibliothèque située sur le même site SharePoint que le rapport d’origine. Vous ne pouvez pas remettre le document dans une bibliothèque sur un autre serveur ou un autre site au sein de la même collection de sites. Pour remettre le document, vous devez être autorisé à ajouter des éléments dans la bibliothèque à laquelle le rapport est remis.  
  
-   **Dossier de fichiers :** vous pouvez remettre un document basé sur le rapport d’origine dans un dossier partagé du système de fichiers. Vous devez sélectionner un dossier existant accessible via une connexion réseau.  
  
-   **E-mail :** si le serveur de rapports est configuré pour utiliser l’extension de remise par messagerie du serveur de rapports, vous pouvez créer un abonnement qui envoie un rapport ou un fichier de rapport exporté (enregistré dans un format de sortie) vers votre boîte de réception. Pour recevoir simplement la notification sans le rapport ou l'URL du rapport, désactivez les cases à cocher **Inclure un lien dans le rapport** et **Afficher le rapport dans le message** .  
  
 **Dans cette rubrique :**  
  
-   [Exigences générales pour les abonnements](#bkmk_subscription_requirements)  
  
-   [Pour créer un abonnement afin de livrer un rapport à une bibliothèque SharePoint.](#bkmk_tosharepoint_library)  
  
-   [Pour créer un abonnement afin de livrer un rapport à une bibliothèque SharePoint.](#bkmk_tosharepoint_library)  
  
-   [Pour créer un abonnement pour une remise par messagerie du serveur de rapports](#bkmk_subscription_for_email)  
  
-   [Pour afficher ou modifier un abonnement](#bkmk_to_modify_subscription)  
  
-   [Pour supprimer un abonnement](#bkmk_to_delete_subscription)  
  
##  <a name="general-requirements-for-subscriptions"></a><a name="bkmk_subscription_requirements"></a> Exigences générales pour les abonnements  
 Pour créer un abonnement, le rapport doit utiliser des informations d'identification stockées et vous devez être autorisé à afficher le rapport et à créer des alertes.  
  
 Lorsque vous créez un abonnement, vous pouvez sélectionner un format de fichier de sortie. Tous les rapports ne fonctionnent pas de manière optimale dans tous les formats. Avant de sélectionner un format dans un abonnement, ouvrez le rapport et exportez-le vers différents formats afin de vous assurer qu'il s'affiche comme vous le souhaitez.  
  
 Les utilisateurs ont besoin de l'autorisation de liste **Modifier des éléments** dans SharePoint s'ils souhaitent pouvoir créer des abonnements [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] . Pour plus d'informations, consultez [Article de référence sur les autorisations de site SharePoint et de listes pour les éléments de serveur de rapports](../../reporting-services/security/sharepoint-site-and-list-permission-reference-for-report-server-items.md)  
  
> [!IMPORTANT]  
>  Un abonnement qui remet un rapport dans une bibliothèque ou dans un dossier partagé crée un nouveau fichier statique basé sur le rapport d'origine, mais ce n'est pas une véritable définition de rapport qui s'exécute dans le composant WebPart de visionneuse de rapports. Si le rapport d'origine dispose de fonctionnalités interactives (par exemple de liens d'extraction) ou d'un contenu dynamique, ces fonctionnalités ne sont pas disponibles dans le fichier statique remis à l'emplacement cible. Si vous sélectionnez une « page Web », vous pouvez préserver une certaine interactivité, mais comme le document n'est pas un fichier .rdl qui s'exécute dans la visionneuse de rapports, cliquer dans un rapport crée de nouvelles pages dans la session du navigateur que vous devez parcourir pour retourner au site.  
  
 Vous ne pouvez pas renommer l'extension du nom de fichier d'un rapport exporté au format .rdl et l'exécuter dans le composant WebPart Visionneuse de rapports. Si vous souhaitez créer un abonnement qui propose un véritable rapport, utilisez l'extension de remise par messagerie du serveur de rapports et définissez les options appropriées afin d'inclure un lien vers le rapport.  
  
 Les paramètres de version de la bibliothèque qui contient le document remis déterminent si une nouvelle version du document est créée à chaque remise. Par défaut, les paramètres de version sont activés pour chaque bibliothèque. Sauf si vous avez choisi **Pas de version** spécifiquement, une nouvelle version principale du document est créée à la remise. Seules les versions principales du document sont créées ; les versions mineures ne sont jamais créées suite à une remise des abonnements, même si vous sélectionnez une option de version autorisant les versions mineures. Si vous limitez le nombre de versions principales retenues, les anciennes remises seront remplacées par des nouvelles une fois atteinte la limite maximale.  
  
 Les formats de sortie que vous sélectionnez pour un abonnement sont basés sur les extensions de rendu installées sur le serveur de rapports. Vous pouvez uniquement sélectionner les formats de sortie qui sont pris en charge par les extensions de rendu sur le serveur de rapports.  
  
###  <a name="to-create-a-subscription-to-deliver-a-report-to-a-sharepoint-library"></a><a name="bkmk_tosharepoint_library"></a> Pour créer un abonnement afin de livrer un rapport à une bibliothèque SharePoint.  
  
1.  Accédez à une bibliothèque SharePoint qui contient votre rapport.  
  
2.  Cliquez sur la flèche orientée vers le bas en regard du rapport, puis sélectionnez **Gérer les abonnements**.  
  
3.  Cliquez sur **Ajouter un abonnement**.  
  
4.  Dans **Extension de remise**, sélectionnez l'option **Bibliothèque de documents SharePoint**.  
  
5.  Dans **Bibliothèque de documents**, sélectionnez une bibliothèque du même site.  
  
6.  Dans **Options de fichier**, spécifiez le nom de fichier et le titre du document qui sera créé par l'abonnement.  
  
7.  Dans **Format de sortie**, sélectionnez le format d'application.  
  
     Archive Web (MHTML) est le format par défaut, car il produit un fichier HTML autonome ; toutefois, il ne conserve pas les fonctionnalités interactives éventuellement présentes dans le rapport d'origine.  
  
8.  Dans **Options de remplacement**, spécifiez l'option qui détermine si les remises ultérieures doivent entraîner le remplacement du fichier. Si vous souhaitez conserver les remises antérieures, sélectionnez **Créer un fichier avec un nom unique**. Un nombre est ajouté aux nouveaux fichiers pour permettre la création d'un nom de fichier unique.  
  
9. Dans **Événement de remise**, spécifiez la planification ou l'événement qui doit déclencher l'exécution de l'abonnement. Vous pouvez créer une planification personnalisée, sélectionner une planification partagée (le cas échéant) ou exécuter l'abonnement lors de l'actualisation des données pour un rapport s'exécutant avec des données d'instantanés. Pour plus d’informations sur les planifications et le traitement des données, consultez [Définir les options de traitement &#40;Reporting Services en mode intégré SharePoint&#41;](../../reporting-services/report-server-sharepoint/set-processing-options-reporting-services-in-sharepoint-integrated-mode.md).  
  
10. Dans **Paramètres**, si vous créez un abonnement à un rapport paramétrable, spécifiez les valeurs à utiliser avec le rapport lors du traitement de l'abonnement. La section de paramètres n'est pas visible dans cette page si le rapport que vous sélectionnez ne contient pas de paramètres. Pour plus d’informations sur les paramètres, consultez [Définir les paramètres sur un rapport publié &#40;Reporting Services en mode intégré SharePoint&#41;](../../reporting-services/report-design/set-parameters-on-a-published-report-sharepoint-integrated-mode.md).  
  
###  <a name="to-create-a-subscription-for-shared-folder-delivery"></a><a name="bkmk_subscription_for_sharedfolder"></a> Pour créer un abonnement pour une remise par dossier partagé  
  
1.  Accédez à une bibliothèque SharePoint qui contient votre rapport.  
  
2.  Cliquez sur la flèche orientée vers le bas en regard du rapport, puis sélectionnez **Gérer les abonnements**.  
  
3.  Cliquez sur **Ajouter un abonnement**.  
  
4.  Dans **Extension de remise**, sélectionnez **Partage de fichiers Windows**.  
  
5.  Dans **Nom de fichier**, entrez le nom du fichier qui sera créé dans le dossier partagé.  
  
6.  Dans **Chemin d’accès**, entrez le chemin du dossier au format UNC (Uniform Naming Convention) en incluant le nom réseau de l’ordinateur. N'incluez pas de barre oblique inverse à la fin du chemin d'accès au dossier. Voici un exemple de chemin d'accès `\\ComputerName01\Public\MyReports`, où Public et MyReports sont des dossiers partagés.  
  
7.  Dans **Format du rendu**, sélectionnez le format d'application du rapport.  
  
8.  Dans **Mode écriture**, choisissez entre **Aucun**, **Auto-incrément** et **Remplacer**. Ces options permettent de déterminer si les remises ultérieures doivent remplacer un fichier. Si vous souhaitez conserver les remises antérieures, choisissez **Autoincrément**. Un nombre est ajouté aux nouveaux fichiers pour permettre la création d'un nom de fichier unique. Si vous choisissez **Aucun**, il n'y aura pas de remise si un fichier portant le même nom existe déjà à l'emplacement cible.  
  
9. Dans **Extension de fichier**, choisissez **Vrai** pour ajouter une extension de nom de fichier correspondant au format du fichier d'application ou Faux pour créer le fichier sans extension.  
  
10. Dans **Nom d'utilisateur** et **Mot de passe**, entrez les informations d'identification permettant de bénéficier d'autorisations d'accès en écriture sur le dossier partagé.  
  
11. Dans **Événement de remise**, spécifiez la planification ou l'événement qui doit déclencher l'exécution de l'abonnement. Vous pouvez créer une planification personnalisée, sélectionner une planification partagée (le cas échéant) ou exécuter l'abonnement lors de l'actualisation des données pour un rapport s'exécutant avec des données d'instantanés. Pour plus d’informations sur les planifications et le traitement des données, consultez [Définir les options de traitement &#40;Reporting Services en mode intégré SharePoint&#41;](../../reporting-services/report-server-sharepoint/set-processing-options-reporting-services-in-sharepoint-integrated-mode.md).  
  
12. Dans **Paramètres**, si vous créez un abonnement à un rapport paramétrable, spécifiez les valeurs à utiliser avec le rapport lors du traitement de l'abonnement. Pour plus d’informations sur les paramètres, consultez [Définir les paramètres sur un rapport publié &#40;Reporting Services en mode intégré SharePoint&#41;](../../reporting-services/report-design/set-parameters-on-a-published-report-sharepoint-integrated-mode.md).  
  
###  <a name="to-create-a-subscription-for-report-server-e-mail-delivery"></a><a name="bkmk_subscription_for_email"></a> Pour créer un abonnement pour une remise par messagerie du serveur de rapports  
  
1.  Accédez à une bibliothèque SharePoint qui contient votre rapport.  
  
2.  Cliquez sur la flèche orientée vers le bas en regard du rapport, puis sélectionnez **Gérer les abonnements**.  
  
3.  Cliquez sur **Ajouter un abonnement**.  
  
4.  Dans **Extension de remise**, sélectionnez **Messagerie électronique**.  
  
5.  Dans **Options de remise**, spécifiez l’adresse e-mail à laquelle le rapport doit être envoyé.  
  
6.  Si vous le souhaitez, vous pouvez modifier la ligne Objet. La ligne Objet utilise des paramètres intégrés qui capturent le nom du rapport et l'heure à laquelle il a été traité. Il s'agit des seuls paramètres intégrés qui peuvent être utilisés. Les paramètres sont des espaces réservés qui personnalisent le texte affiché dans la ligne Objet, mais vous pouvez les remplacer par du texte statique.  
  
7.  Choisissez l'option **Inclure un lien dans le rapport** pour incorporer une URL de rapport dans le corps du message.  
  
8.  Dans **Contenu du rapport**, spécifiez si vous souhaitez incorporer le rapport réel dans le corps du message.  
  
     Le format de rendu et le navigateur déterminent si le rapport est incorporé ou joint. Si votre navigateur prend en charge les formats HTML 4.0 et MHTML et si vous sélectionnez le format de rendu Archive Web, le rapport est incorporé au message. Tous les autres formats de rendu (CSV, PDF, etc.) remettent les rapports sous forme de pièces jointes. [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] ne vérifie pas la taille de la pièce jointe ou du message avant d'envoyer le rapport. Si la pièce jointe ou le message dépasse la limite maximale autorisée par votre serveur de messagerie, le rapport ne sera pas remis. Choisissez une des autres options de remise (URL ou notification) pour les rapports volumineux.  
  
9. Dans **Événement de remise**, spécifiez la planification ou l'événement qui doit déclencher l'exécution de l'abonnement. Vous pouvez créer une planification personnalisée, sélectionner une planification partagée (le cas échéant) ou exécuter l'abonnement lors de l'actualisation des données pour un rapport s'exécutant avec des données d'instantanés. Pour plus d’informations sur les planifications et le traitement des données, consultez [Définir les options de traitement &#40;Reporting Services en mode intégré SharePoint&#41;](../../reporting-services/report-server-sharepoint/set-processing-options-reporting-services-in-sharepoint-integrated-mode.md).  
  
10. Dans **Paramètres**, si vous créez un abonnement à un rapport paramétrable, spécifiez les valeurs à utiliser avec le rapport lors du traitement de l'abonnement. Pour plus d’informations sur les paramètres, consultez [Définir les paramètres sur un rapport publié &#40;Reporting Services en mode intégré SharePoint&#41;](../../reporting-services/report-design/set-parameters-on-a-published-report-sharepoint-integrated-mode.md).  
  
###  <a name="to-view-or-modify-a-subscription"></a><a name="bkmk_to_modify_subscription"></a> Pour afficher ou modifier un abonnement  
  
1.  Accédez à une bibliothèque SharePoint qui contient votre rapport.  
  
2.  Cliquez sur la flèche orientée vers le bas en regard du rapport, puis cliquez sur **Gérer les abonnements**.  
  
3.  Chaque abonnement est identifié par le type de remise. Cliquez sur le type d'abonnement pour afficher et modifier les propriétés existantes.  
  
###  <a name="to-delete-a-subscription"></a><a name="bkmk_to_delete_subscription"></a> Pour supprimer un abonnement  
  
1.  Accédez à une bibliothèque SharePoint qui contient votre rapport.  
  
2.  Cliquez sur la flèche orientée vers le bas en regard du rapport, puis cliquez sur **Gérer les abonnements**.  
  
3.  Activez la case à cocher en regard de l'abonnement, puis cliquez sur **Supprimer**.  
  
## <a name="see-also"></a>Voir aussi  
 [Abonnements et remise &#40;Reporting Services&#41;](../../reporting-services/subscriptions/subscriptions-and-delivery-reporting-services.md)   
 [Remise par e-mail dans Reporting Services](../../reporting-services/subscriptions/e-mail-delivery-in-reporting-services.md)   
 [Remise par partage de fichiers dans Reporting Services](../../reporting-services/subscriptions/file-share-delivery-in-reporting-services.md)   
 [Remise à une bibliothèque SharePoint dans Reporting Services](../../reporting-services/subscriptions/sharepoint-library-delivery-in-reporting-services.md)   
 [Configurer un serveur de rapports pour la remise par messagerie (Gestionnaire de configuration du serveur de rapports)](../install-windows/configure-the-report-server-service-account-ssrs-configuration-manager.md)  
  
  
