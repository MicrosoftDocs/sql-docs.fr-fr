---
title: Méthodes de gestion des espaces de noms du serveur de rapports | Microsoft Docs
description: Le service web de gestion des serveurs de rapports contient des méthodes que vous pouvez utiliser pour gérer des rapports, des dossiers et des ressources dans la base de données du serveur de rapports.
ms.date: 03/06/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-server-web-service
ms.topic: reference
helpviewer_keywords:
- reports [Reporting Services], managing
- management methods [Reporting Services]
- methods [Reporting Services], about methods
- methods [Reporting Services]
ms.assetid: 2aa43ce9-f51e-408a-8ce0-b40d3dd62561
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: fab863f46d1b5707359c64577e6cffde4a6e2ea2
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100063335"
---
# <a name="report-server-namespace-management-methods"></a>Méthodes de gestion des espaces de noms du serveur de rapports
  Le service web de gestion des serveurs de rapports contient des méthodes que vous pouvez utiliser pour gérer des rapports, des dossiers et des ressources dans la base de données du serveur de rapports.  
  
|Méthode|Action|  
|------------|------------|  
|<xref:ReportService2010.ReportingService2010.CancelJob%2A>|Annule l’exécution d’un travail.|  
|<xref:ReportService2010.ReportingService2010.CreateFolder%2A>|Ajoute un dossier à la base de données du serveur de rapports ou à la bibliothèque SharePoint.|  
|<xref:ReportService2010.ReportingService2010.CreateCatalogItem%2A>|Ajoute un nouvel élément à une base de données du serveur de rapports ou à la bibliothèque SharePoint. Cette méthode s’applique aux types d’éléments **Rapport**, **Modèle**, **Dataset**, **Composant**, **Ressource** et **DataSource**.|  
|M:ReportService2010.ReportingService2010.CreateReportEditSession(System.String, System.String,System.Byte[],ReportService2010.Warning[]@)|Crée une session d'édition de rapport.|  
|<xref:ReportService2010.ReportingService2010.DeleteItem%2A>|Supprime un élément de la base de données du serveur de rapports ou de la bibliothèque SharePoint.|  
|<xref:ReportService2010.ReportingService2010.FindItems%2A>|Retourne les éléments de la base de données du serveur de rapports ou de la bibliothèque SharePoint qui correspondent aux critères de recherche spécifiés.|  
|<xref:ReportService2010.ReportingService2010.FireEvent%2A>|Déclenche un événement en fonction des paramètres fournis.|  
|<xref:ReportService2010.ReportingService2010.GetExtensionSettings%2A>|Retourne une liste de paramètres pour une extension donnée.|  
|<xref:ReportService2010.ReportingService2010.GetItemType%2A>|Récupère le type d'un élément dans la base de données du serveur de rapports ou la bibliothèque SharePoint (si l'élément existe).|  
|<xref:ReportService2010.ReportingService2010.GetProperties%2A>|Retourne les valeurs d'une ou plusieurs propriétés d'un élément dans la base de données du serveur de rapports ou la bibliothèque SharePoint.|  
|<xref:ReportService2010.ReportingService2010.GetItemDefinition%2A>|Récupère la définition ou le contenu d'un élément. Cette méthode s’applique aux types d’éléments **Rapport**, **Modèle**, **Dataset**, **Composant**, **Ressource** et **DataSource**.|  
|<xref:ReportService2010.ReportingService2010.GetItemReferences%2A>|Retourne une liste de références d'éléments de catalogue associées à un élément.|  
|<xref:ReportService2010.ReportingService2010.GetReportServerConfigInfo%2A>|Retourne des informations sur l'instance du serveur de rapports connectée ou sur toutes les instances du serveur de rapports dans un déploiement avec montée en puissance parallèle.|  
|<xref:ReportService2010.ReportingService2010.GetSystemProperties%2A>|Retourne une ou plusieurs propriétés système.|  
|<xref:ReportService2010.ReportingService2010.ListChildren%2A>|Obtient la liste des enfants d’un dossier spécifié.|  
|<xref:ReportService2010.ReportingService2010.ListDatabaseCredentialRetrievalOptions%2A>|Retourne une liste d'options de récupération des informations d'identification prises en charge.|  
|<xref:ReportService2010.ReportingService2010.ListEvents%2A>|Retourne une liste d'extensions d'événement telles qu'elles apparaissent dans le fichier de configuration du serveur de rapports.|  
|<xref:ReportService2010.ReportingService2010.ListJobs%2A>|Retourne la liste des travaux en cours d’exécution sur le serveur de rapports.|  
|<xref:ReportService2010.ReportingService2010.ListExtensions%2A>|Retourne la liste des extensions configurées pour un type d'extension donné.|  
|<xref:ReportService2010.ReportingService2010.ListExtensionTypes%2A>|Retourne une liste de types d'extension pris en charge.|  
|<xref:ReportService2010.ReportingService2010.ListItemTypes%2A>|Retourne une liste de types d'élément de catalogue pris en charge.|  
|<xref:ReportService2010.ReportingService2010.ListJobActions%2A>|Retourne une liste d'actions de travail prises en charge.|  
|<xref:ReportService2010.ReportingService2010.ListJobStates%2A>|Retourne une liste d'états de travail pris en charge.|  
|<xref:ReportService2010.ReportingService2010.ListJobTypes%2A>|Retourne une liste de types de travail pris en charge.|  
|<xref:ReportService2010.ReportingService2010.ListParents%2A>|Récupère des éléments parents pour l'élément donné.|  
|<xref:ReportService2010.ReportingService2010.ListSecurityScopes%2A>|Retourne une liste de portées de la sécurité prises en charge.|  
|<xref:ReportService2010.ReportingService2010.Logoff%2A>|Déconnecte l'utilisateur actuel faisant des demandes de service Web. Cette méthode ne s'applique qu'au mode natif.|  
|<xref:ReportService2010.ReportingService2010.LogonUser%2A>|Connecte un utilisateur et authentifie sa demande de service Web Report Server. Cette méthode ne s'applique qu'au mode natif.|  
|<xref:ReportService2010.ReportingService2010.SetItemReferences%2A>|Définit les éléments de catalogue associés à un élément.|  
|<xref:ReportService2010.ReportingService2010.MoveItem%2A>|Déplace et/ou renomme un élément.|  
|<xref:ReportService2010.ReportingService2010.SetProperties%2A>|Définit une ou plusieurs propriétés d'un élément.|  
|<xref:ReportService2010.ReportingService2010.SetItemDefinition%2A>|Définit la définition ou le contenu d'un élément spécifié. Cette méthode s’applique aux types d’éléments **Rapport**, **Modèle**, **Dataset**, **Composant**, **Ressource** et **DataSource**.|  
|<xref:ReportService2010.ReportingService2010.SetSystemProperties%2A>|Définit une ou plusieurs propriétés système dans le serveur de rapports ou la batterie de serveurs SharePoint.|  
|<xref:ReportService2010.ReportingService2010.ValidateExtensionSettings%2A>|Valide les paramètres d'extension des [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)].|  
  
## <a name="see-also"></a>Voir aussi  
 [Création d’applications à l’aide du service web et du .NET Framework](../../../reporting-services/report-server-web-service/net-framework/building-applications-using-the-web-service-and-the-net-framework.md)   
 [Service web Report Server](../../../reporting-services/report-server-web-service/report-server-web-service.md)   
 [Méthodes du service web Report Server](../../../reporting-services/report-server-web-service/methods/report-server-web-service-methods.md)   
 [Informations techniques de référence &#40;SSRS&#41;](../../../reporting-services/technical-reference-ssrs.md)  
  
  
