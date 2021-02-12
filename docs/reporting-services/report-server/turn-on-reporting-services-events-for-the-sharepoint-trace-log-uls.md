---
title: Activer des événements Reporting Services pour le journal des traces SharePoint (ULS) | Microsoft Docs
description: Découvrez comment activer l’écriture d’événements Reporting Services dans le journal de suivi ULS SharePoint pour les serveurs Reporting Services en mode SharePoint.
ms.date: 05/30/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-server
ms.topic: conceptual
ms.assetid: 81110ef6-4289-405c-a931-e7e9f49e69ba
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 73a806d2f86cc155c0aa9d70c4fa0f9e01aa2b59
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100023344"
---
# <a name="turn-on-reporting-services-events-for-the-sharepoint-trace-log-uls"></a>Activer des événements Reporting Services pour le journal des traces SharePoint (ULS)

  À partir de [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)], les serveurs d' [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] en mode SharePoint peuvent écrire des événements [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] dans le journal des traces su service de journalisation unifiée SharePoint (ULS). [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] sont disponibles sur la page de supervision de l'Administration centrale de SharePoint.  
  
 Dans cette rubrique :  
  
-   [Recommandations générales du journal ULS](#bkmk_general)  
  
-   [Pour activer et désactiver des événements Reporting Services dans la catégorie Reporting Services](#bkmk_turnon)  
  
-   [Configuration recommandée](#bkmk_recommended)  
  
-   [Lecture des entrées de journal](#bkmk_readentries)  
  
-   [Liste des événements SQL Server Reporting Services](#bkmk_list)  
  
-   [Afficher un journal avec PowerShell](#bkmk_powershell)  
  
-   [Emplacement du journal des traces](#bkmk_trace)  
  
##  <a name="general-uls-log-recommendations"></a><a name="bkmk_general"></a> Recommandations générales du journal ULS  
 Le tableau suivant répertorie les catégories et niveaux d'événements recommandés pour l'analyse d'un environnement [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] . Lorsqu'un événement est journalisé, chaque entrée inclut l'heure à laquelle l'événement a été enregistré, le nom du processus et l'ID du thread.  
  
|Category|Level|Description|  
|--------------|-----------|-----------------|  
|Base de données|Commentaires|Journalise les événements ayant trait à l'accès aux bases de données.|  
|Général|Commentaires|Journalise les événements ayant trait à l'accès aux éléments suivants :<br /><br /> [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] pages Web<br /><br /> Gestionnaire HTTP de la visionneuse de rapports<br /><br /> Accès aux rapports (fichiers .rdl)<br /><br /> Sources de données (fichiers .rsds)<br /><br /> URL sur le site SharePoint (fichiers .smdl)|  
|Généralités sur Office Server|Exception|Journalise les échecs d'ouverture de session.|  
|Topologie|Commentaires|Journalise des informations sur l'utilisateur actuel.|  
|composants WebPart|Commentaires|Journalise les événements ayant trait à l'accès au composant WebPart Visionneuse de rapports.|  
  
##  <a name="to-turn-on-and-off-reporting-services-events-in-the-reporting-services-category"></a><a name="bkmk_turnon"></a> Pour activer et désactiver des événements Reporting Services dans la catégorie Reporting Services  
  
1.  Dans l'Administration centrale de SharePoint  
  
2.  Cliquez sur **Supervision**.  
  
3.  Cliquez sur **Configurer la journalisation des diagnostics** dans le groupe **Création de rapports** .  
  
4.  Recherchez **SQL Server Reporting Services** dans la liste des catégories.  
  
5.  Cliquez sur le signe plus (+) pour développer les sous-catégories de **SQL Server Reporting Services**.  
  
6.  Sélectionnez les sous-catégories à ajouter au journal des traces.  
  
7.  À la fin de la liste des catégories, sélectionnez un niveau d'événement pour **Événement le moins critique à enregistrer dans le journal de suivi**. Sélectionnez **Aucun** pour désactiver le suivi.  
  
> [!NOTE]  
>  L'option **Événement le moins critique à enregistrer dans le journal d'événements** n'est pas prise en charge par [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]. L'option est ignorée.  
  
##  <a name="recommended-configuration"></a><a name="bkmk_recommended"></a> Configuration recommandée  
 Les options de journalisation suivantes sont recommandées comme configuration standard :  
  
-   **Redirecteur HTTP**  
  
-   **Proxy de client SOAP**  
  
-   Si vous rencontrez des problèmes avec la configuration, ajoutez **Pages de configuration**.  
  
 Vous pouvez examiner tous les paramètres actuels du journal de diagnostics de la batterie de serveurs existante avec l'applet de commande PowerShell suivant :  
  
```  
Get-SPDiagnosticConfig  
```  
  
##  <a name="reading-the-logs-entries"></a><a name="bkmk_readentries"></a> Lecture des entrées de journal  
 Les entrées de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] dans le journal sont mises en forme de la manière suivante.  
  
1.  **Produit : SQL Server Reporting Services**  
  
2.  **Catégorie :** Les événements associés au serveur ont les caractères « Report Server », au début du nom. Par exemple les événements « Report Server Alerting Runtime » sont aussi enregistrés dans les fichiers journaux du serveur de rapports.  
  
3.  **Catégorie :** Les événements liés à ou communiqués à partir d'un composant web front-end ne contiennent pas « Report Server ». Par exemple « Service Application Proxy Report Server Alerting Runtime ». Les entrées WFE contiennent un CorrelationID contrairement aux entrées de serveur.  
  
##  <a name="list-of-sql-server-reporting-services-events"></a><a name="bkmk_list"></a> Liste des événements SQL Server Reporting Services  
 Le tableau suivant contient la liste des événements de la catégorie SQL Server Reporting Services :  
  
|Nom de zone|Entrées de description ou témoin|  
|---------------|-----------------------------------|  
|Pages de configuration||  
|Redirecteur HTTP||  
|Traitement en mode local||  
|Rendu en mode local||  
|Proxy de client SOAP||  
|Pages d'interface utilisateur||  
|Power View|Entrées de journal écrites dans l'API **LogClientTraceEvents** . Ces entrées proviennent des applications clientes, notamment Power View, une fonctionnalité du complément SQL Server Reporting Services.<br /><br /> Toutes les entrées de journal provenant de l’API LogClientTraceEvents sont enregistrées sous la **catégorie** « SQL Server Reporting Services » et la **zone** « Power View ».<br /><br /> Le contenu des entrées enregistrées avec la zone « Power View » est déterminé par l’application cliente.|  
|Runtime d'alerte de Report Server||  
|Gestionnaire d'applications de domaine du service Web Report Server||  
|Réponse mise en mémoire tampon service Web Report Server||  
|Cache service Web Report Server||  
|Catalogue service Web Report Server||  
|Segment service Web Report Server||  
|Nettoyage service Web Report Server||  
|Gestionnaire de configuration service Web Report Server|Entrées témoin :<br /><br /> URL interne du serveur de rapports MediumUsing `https://localhost:80/ReportServer`.<br /><br /> Paramètre UnexpectedMissing ou Invalid ExtendedProtectionLevel|  
|Chiffrement service Web Report Server||  
|Extension de données service Web Report Server||  
|Interrogation de la base de données service Web Report Server||  
|Paramètres par défaut service Web Report Server||  
|Extension du courrier électronique service Web Report Server||  
|Convertisseur Excel service Web Report Server||  
|Fabrique d'extension service Web Report Server||  
|Exécution HTTP service Web Report Server||  
|Convertisseur d'image service Web Report Server||  
|Surveillance de la mémoire service Web Report Server||  
|Notification service Web Report Server||  
|Traitement service Web Report Server||  
|Fournisseur du serveurs de rapports||  
|Rendu service Web Report Server||  
|Aperçu des rapports service Web Report Server||  
|Utilitaire des ressources service Web Report Server|Entrées témoin :<br /><br /> Référence SKU de démarrage des services MediumReporting : Évaluation<br /><br /> Copie de MediumEvaluation : 180 jours restants|  
|Travaux d'exécution service Web Report Server||  
|Demandes d'exécution service Web Report Server||  
|Planification service Web Report Server||  
|Sécurité service Web Report Server||  
|Contrôleur de services service Web Report Server||  
|Session service Web Report Server||  
|Abonnement service Web Report Server||  
|Runtime WCF de Report Server||  
|Serveur Web Report Server||  
|Proxy d'application de service||  
|Service partagé|Entrées témoin :<br /><br /> MediumUpdating ReportingWebServiceApplication<br /><br /> Accès de MediumGranting aux bases de données de contenu.<br /><br /> Instances de MediumProvisioning pour ReportingWebServiceApplication<br /><br /> Modification du compte de service MediumProcessing pour ReportingWebServiceApplication<br /><br /> Autorisations de base de données de MediumSetting.|  
  
##  <a name="view-a-log-file-with-powershell"></a><a name="bkmk_powershell"></a> Afficher un journal avec PowerShell  
 ![Contenu relatif à PowerShell](/analysis-services/analysis-services/instances/install-windows/media/rs-powershellicon.jpg "Contenu relatif à PowerShell") Vous pouvez utiliser PowerShell pour retourner la liste d’événements connexes [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] à partir d’un fichier journal ULS. Tapez la commande suivante à partir de SharePoint 2010 Management Shell pour obtenir la liste filtrée des lignes du fichier journal ULS nommé UESQL11SPOINT-20110606-1530.log, qui contient « **sql server reporting services** » :  
  
```  
Get-content -path "C:\Program Files\Common Files\Microsoft Shared\Web Server Extensions\14\LOGS\UESQL11SPOINT-20110606-1530.log" | select-string "sql server reporting services"  
```  
  
 Il existe également des outils que vous pouvez télécharger pour lire des journaux ULS. Par exemple, [SharePoint LogViewer](https://github.com/hasankhan/SharePointLogViewer), disponible sur GitHub. 
  
 Pour plus d’informations sur l’utilisation de PowerShell pour afficher les données du journal, consultez [Afficher les journaux de diagnostic (SharePoint Server 2010)](/SharePoint/administration/view-diagnostic-logs).  
  
##  <a name="trace-log-location"></a><a name="bkmk_trace"></a> Emplacement du journal des traces  
 Les fichiers journaux des traces se trouvent généralement dans le dossier **c:\Program Files\Common files\Microsoft Shared\Web Server Extensions\14\logs** , mais vous pouvez vérifier ou modifier le chemin d’accès à partir de la page **Journalisation des diagnostics** dans l’Administration centrale de SharePoint.  
  
 Pour plus d’informations et pour connaître les étapes permettant de configurer la journalisation des diagnostics sur un serveur SharePoint dans l’Administration centrale de SharePoint 2010, consultez [Configurer les paramètres de journalisation des diagnostics (Windows SharePoint Services)](/previous-versions/office/sharepoint-2007-products-and-technologies/cc288649(v=office.12)).  

D’autres questions ? [Essayez de poser une question dans le forum Reporting Services](https://go.microsoft.com/fwlink/?LinkId=620231)