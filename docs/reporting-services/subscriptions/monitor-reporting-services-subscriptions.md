---
title: Analyser les abonnements Reportions Services | Microsoft Docs
description: Découvrez comment effectuer le suivi des abonnements Reporting Services à l’aide de l’interface utilisateur, de PowerShell ou des fichiers journaux. Les options de supervision dépendent du mode de serveur de rapports que vous exécutez.
ms.date: 06/12/2019
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: subscriptions
ms.topic: conceptual
helpviewer_keywords:
- subscriptions [Reporting Services], inactive
- subscriptions [Reporting Services], status
- monitoring [Reporting Services], subscriptions
- status information [Reporting Services]
- inactive subscriptions [Reporting Services]
ms.assetid: 054c4a87-60bf-4556-9a8c-8b2d77a534e6
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 6631325809a468aed4e7c7fd00cf71aedecc7ba0
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100042056"
---
# <a name="monitor-reporting-services-subscriptions"></a>Analyser les abonnements Reportions Services
  Vous pouvez surveiller les abonnements [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] à partir de l'interface utilisateur, de Windows PowerShell ou des fichiers journaux. Les options de surveillance à votre disposition dépendent du mode de serveur de rapports que vous exécutez.  
  
**[!INCLUDE[applies](../../includes/applies-md.md)]**

:::row:::
    :::column:::
        [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] en mode natif  
    :::column-end:::
    :::column:::
        [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] Mode SharePoint  
    :::column-end:::
:::row-end:::

 **Dans cet article :**  
  
-   [Interface utilisateur en mode natif](#bkmk_native_mode)  
  
-   [Mode SharePoint](#bkmk_sharepoint_mode)  
  
-   [Utiliser PowerShell pour surveiller les abonnements](#bkmk_use_powershell)  
  
-   [Gestion des abonnements inactifs](#bkmk_manage_inactive)  
  
##  <a name="native-mode-user-interface"></a><a name="bkmk_native_mode"></a> Interface utilisateur en mode natif  
 Chaque utilisateur [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] peut surveiller l'état d'un abonnement dans la page **Mes abonnements** ou sous l'onglet **Abonnements** du portail web. Les pages d'abonnement présentent des colonnes qui indiquent l'état de l'abonnement et à quel moment l'abonnement a été exécuté pour la dernière fois. Les messages d'état sont mis à jour à chaque traitement planifié de l'abonnement. Si le déclencheur ne se produit jamais (par exemple, si un instantané d'exécution de rapport n'est jamais actualisée ou si une planification n'est jamais exécutée), le message d'état ne sera pas mis à jour.  
  
 Le tableau suivant répertorie les valeurs possibles de la colonne **État** .  
  
|Statut|Description|  
|------------|-----------------|  
|Nouvel abonnement|Apparaît la première fois que vous créez l'abonnement.|  
|Inactif|Apparaît lorsqu'un abonnement ne peut pas être traité. Pour plus d'informations, consultez « Gestion des abonnements inactifs », plus loin dans cet article.|  
|Effectué : \<*number*> traités sur un total de \<*number*> ; \<*number*> erreurs.|Indique l'état de l'exécution d'un abonnement piloté par les données. Ce message provient du processeur de planification et de livraison.|  
|\<*number*> traités|Nombre de notifications que le processeur de planification et de livraison a réussi à remettre ou n'essaie plus de remettre. Lorsqu'une remise pilotée par les données est terminée, le nombre de notifications traitées doit être égal au nombre total de notifications générées.|  
|Total de \<*number*>|Nombre total de notifications générées pour la dernière remise de l'abonnement.|  
|\<*number*> erreurs|Nombre de notifications que le processeur de planification et de livraison n'a pas réussi à remettre ou n'essaie plus de remettre.|  
|Échec de l'envoi du message électronique : le transport a échoué dans sa connexion au serveur.|Indique que le serveur de rapports ne s'est pas connecté au serveur de messagerie ; ce message provient de l'extension de remise par messagerie électronique.|  
|Le fichier \<*filename*> a été écrit dans \<path>.|Indique que la remise à l'emplacement du partage de fichiers a réussi ; ce message provient de l'extension de remise dans un partage de fichiers.|  
|Une erreur inconnue s'est produite lors de l'écriture du fichier.|Indique que la remise à l'emplacement du partage de fichiers a échoué ; ce message provient de l'extension de remise dans un partage de fichiers.|  
|Échec lors de la connexion au dossier de destination, \<path>. Assurez-vous que le dossier de destination ou le partage de fichiers existe.|Indique que le dossier que vous avez spécifié n'a pas pu être trouvé ; ce message provient de l'extension de remise dans un partage de fichiers.|  
|impossible d’écrire le fichier \<filename> sur \<path>. Nouvelle tentative d'écriture en cours.|Indique que le fichier n'a pas pu être mis à jour avec une version plus récente ; ce message provient de l'extension de remise dans un partage de fichiers.|  
|Échec d'écriture du fichier \<filename> : \<message>|Indique que la remise à l'emplacement du partage de fichiers a échoué ; ce message provient de l'extension de remise dans un partage de fichiers.|  
|\<custom status messages>|Messages d'état indiquant le succès ou l'échec de la remise. Ces messages proviennent des extensions de remise. Si vous utilisez une extension de remise tierce ou personnalisée, d'autres messages d'état peuvent apparaître.|  
  
 Les administrateurs de serveur de rapports peuvent également surveiller les abonnements standard en cours de traitement. Les abonnements pilotés par les données ne peuvent pas être analysés. Pour plus d’informations, consultez [Gérer un processus en cours d’exécution](../../reporting-services/subscriptions/manage-a-running-process.md).  
  
 En cas d'échec de remise d'un abonnement (par exemple, si le serveur de messagerie est indisponible), l'extension de remise réessaye la remise. Un paramètre de configuration spécifie le nombre de tentatives qui seront effectuées. Par défaut, il n'y en a aucune. Dans certains cas, le rapport peut avoir été traité sans données (par exemple, si la source de données est hors connexion), auquel cas le texte à cet effet est fourni dans le corps du message.  
  
### <a name="native-mode-log-files"></a>Fichiers journaux en mode natif  
 Si une erreur se produit au cours de la remise, une entrée est inscrite dans le journal de trace du serveur de rapports.  
  
 Les administrateurs de serveur de rapports peuvent passer en revue les fichiers **ReportServerService_*.log** pour déterminer l’état de remise des abonnements. Pour la remise par messagerie électronique, les fichiers journaux du serveur de rapports contiennent un enregistrement du traitement et des remises effectuées sur les comptes de messagerie spécifiques. Voici l'emplacement par défaut des fichiers journaux :  
  
 `C:\Program Files\Microsoft SQL Server Reporting Services\SSRS\LogFiles`  
  
 Voici un exemple de nom de fichier journal :  
  
 `ReportServerService__05_21_2019_00_05_07.log`  
  
 Voici un exemple de message d'erreur de fichier journal de trace lié aux abonnements :  
  
-   library!WindowsService_7!b60!05/20/2019-22:34:36 i INFO: Initializing EnableExecutionLogging to 'True'  as specified in Server system properties.emailextension!WindowsService_7!b60!05/20/2019-22:34:41 ERROR: **Error sending email**. Exception : System.Net.Mail.SmtpException: Le serveur SMTP requiert une connexion sécurisée ou le client n'était pas authentifié. La réponse du serveur était : 5.7.1 Le client n'était pas authentifié sur System.Net.Mail.MailCommand.CheckResponse(SmtpStatusCode statusCode, String response)  
  
 Le fichier journal ne contient aucune information indiquant si le rapport a été ouvert ou si la remise a réussi. Une remise réussie signifie qu'aucune erreur n'a été générée par le processeur de planification et de livraison et que le serveur de rapports s'est connecté au serveur de messagerie. Si le message électronique a entraîné l'envoi d'un message d'erreur de non-remise dans la boîte aux lettres de l'utilisateur, cette information ne figurera pas dans le fichier journal. Pour plus d’informations sur les fichiers journaux, consultez [Fichiers journaux et sources de Reporting Services](../../reporting-services/report-server/reporting-services-log-files-and-sources.md).  
  
##  <a name="sharepoint-mode"></a><a name="bkmk_sharepoint_mode"></a> Mode SharePoint  
 Pour surveiller un abonnement en mode SharePoint : l'état de l'abonnement peut être surveillé à partir de la page **Gérer les abonnements** .  
  
1.  Accédez à la bibliothèque de documents qui contient le rapport.  
  
2.  Ouvrez le menu contextuel du rapport ( **...** ).  
  
3.  Sélectionnez l’option de menu développé ( **...** ).  
  
4.  Sélectionnez **Gérer les abonnements**  
  
### <a name="sharepoint-uls-log-files"></a>Fichiers journaux ULS SharePoint  
 Les informations associées aux abonnements sont écrites dans le journal ULS SharePoint. Pour plus d’informations sur la configuration d’événements [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] pour le journal ULS, consultez [Activer des événements Reporting Services pour le journal des traces SharePoint &#40;ULS&#41;](../../reporting-services/report-server/turn-on-reporting-services-events-for-the-sharepoint-trace-log-uls.md).  Voici un exemple d'entrée de journal ULS relative aux abonnements [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] .  
  
|Date|Process|Domaine|Category|Level|Correlation|Message|  
|-|-|-|-|-|-|-|  
|5/21/2019 14:34:06:15|Pool d'applications : a0ba039332294f40bc4a81544afde01d|SQL Server Reporting Services|Extension du courrier électronique service Web Report Server|Inattendu.|(empty)|**Erreur d'envoi de courrier électronique.** Exception : System.Net.Mail.SmtpException: Boîte aux lettres non disponible. La réponse du serveur était : 5.7.1 Le client n'est pas autorisé à envoyer en tant que cet expéditeur sur System.Net.Mail.DataStopCommand.CheckResponse(SmtpStatusCode statusCode, String serverResponse) sur System.Net.Mail.DataStopCommand.Send(SmtpConnection conn) sur System.Net.Mail.SmtpClient.Send(MailMessage message) sur Microsoft.ReportingServices.EmailDeliveryProvider.EmailProvider.Deliver(Notification notification)|  
  
##  <a name="use-powershell-to-monitor-subscriptions"></a><a name="bkmk_use_powershell"></a> Utiliser PowerShell pour surveiller les abonnements  
 Pour obtenir des exemples de scripts PowerShell permettant de vérifier l'état des abonnements en mode natif ou SharePoint, voir [Gérer les propriétaires d’abonnements et exécuter un abonnement - PowerShell](../../reporting-services/subscriptions/manage-subscription-owners-and-run-subscription-powershell.md).  
  
##  <a name="managing-inactive-subscriptions"></a><a name="bkmk_manage_inactive"></a> Gestion des abonnements inactifs  
 Lorsqu'un abonnement devient inactif, vous devez soit le supprimer, soit le réactiver en résolvant les conditions sous-jacentes qui empêchent son traitement. Les abonnements peuvent devenir inactifs si certaines conditions empêchant leur traitement se produisent. Ces conditions sont les suivantes :  
  
-   Suppression ou désinstallation de l'extension de remise spécifiée dans l'abonnement.  
  
-   Changement des paramètres d'informations d'identification stockées en valeurs intégrées ou en valeurs demandées par invite.  
  
-   Changement d'un nom de paramètre ou d'un type de données dans la définition de rapport, puis republication d'un rapport. Si un abonnement comprend un paramètre qui n'est plus valide, l'abonnement devient inactif.  
  
-   Modification du mode d'exécution d'un rapport (par exemple, modification d'un rapport à la demande de sorte qu'il s'exécute comme un instantané d'exécution de rapport). Pour plus d’informations, consultez [Définir les propriétés de traitement d’un rapport](../../reporting-services/report-server/set-report-processing-properties.md).  
  
 Un abonnement inactif est indiqué par un message dans l'abonnement lui-même. Le message contient des informations sur la cause de l'inactivité et indique les étapes à suivre pour réactiver l'abonnement.  
  
 Lorsque certaines conditions entraînent l'inactivité de l'abonnement, celui-ci manifeste son état lorsque le serveur de rapports exécute l'abonnement. Si un abonnement doit remettre un rapport chaque vendredi à 02:00 et que l'extension de remise qu'il utilise a été désinstallée le lundi à 09:00, l'abonnement ne manifestera pas son état d'inactivité avant le vendredi à 02:00.  
  
## <a name="see-also"></a>Voir aussi  
 [Créer et gérer des abonnements pour les serveurs de rapports en mode natif](../../reporting-services/subscriptions/create-and-manage-subscriptions-for-native-mode-report-servers.md)   
 [Abonnements et remise &#40;Reporting Services&#41;](../../reporting-services/subscriptions/subscriptions-and-delivery-reporting-services.md)  
  
  
