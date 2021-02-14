---
title: Propriétés d’élément du serveur de rapports | Microsoft Docs
description: Les propriétés d’élément sont propres aux éléments de la base de données du serveur de rapports, tels que les rapports, les rapports liés, les dossiers, les ressources, les modèles et les sources de données.
ms.date: 03/06/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-server-web-service
ms.topic: reference
helpviewer_keywords:
- report servers [Reporting Services], properties
- item-specific properties [Reporting Services]
- report items [Reporting Services], properties
- items [Reporting Services], properties
ms.assetid: 21edec6d-9897-48fb-8c75-182305b1dbdb
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 4d4db0c4bdef91630cd2bf20b215f7a6b26d714b
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100063657"
---
# <a name="reporting-services-properties---report-server-item-properties"></a>Propriétés de Reporting Services - Propriétés d’élément du serveur de rapports
  Les propriétés d'élément sont des propriétés spécifiques à des éléments dans la base de données du serveur de rapports, notamment des rapports, des rapports liés, des dossiers, des ressources, des modèles et des sources de données.  
  
 Les noms des propriétés d'élément suivants sont réservés. Vous ne pouvez pas créer des propriétés définies par l'utilisateur du même nom. Vous pouvez lire ou modifier un grand nombre de ces propriétés à l'aide des méthodes du service Web Report Server.  
  
## <a name="item-properties"></a>Propriétés d'élément  
 Les propriétés suivantes s'appliquent à tous les éléments dans la base de données du serveur de rapports.  
  
|Propriété|Description|  
|--------------|-----------------|  
|**CreatedBy**|Nom de l'utilisateur ayant initialement ajouté l'élément à la base de données du serveur de rapports.|  
|**CreationDate**|Date et heure auxquelles l'élément a été ajouté à la base de données du serveur de rapports.|  
|**Description**|Description de l'élément.|  
|**Hidden**|Valeur qui indique si l'élément est visible et accessible aux utilisateurs.|  
|**Identifiant**|ID d'un élément dans la base de données du serveur de rapports.|  
|**ModifiedBy**|Nom du dernier utilisateur ayant modifié l'élément dans la base de données du serveur de rapports.|  
|**ModifiedDate**|Date et heure auxquelles le dernier utilisateur a modifié l'élément.|  
|**Nom**|Nom d'un élément dans la base de données du serveur de rapports.|  
|**Chemin d’accès**|Nom de chemin d'accès complet de l'élément. Le chemin d'accès à un élément quelconque dans la base de données du serveur de rapports peut comprendre 260 caractères au maximum.|  
|**Taille**|Taille, en octets, d'un élément dans la base de données du serveur de rapports.|  
|**Type**|Type d'un élément dans la base de données du serveur de rapports.|  
|**VirtualPath**|Chemin d'accès virtuel à un élément dans la base de données du serveur de rapports. La valeur de la propriété <xref:ReportService2010.CatalogItem.VirtualPath%2A> est le chemin d'accès sous lequel un utilisateur s'attend à voir l'élément. Par exemple, le chemin d'accès virtuel d'un rapport nommé report1 se trouvant dans le dossier My Reports personnel de l'utilisateur est /My Reports. Le chemin d'accès réel de l'élément est /Users/nom_utilisateur/My Reports.|  
  
## <a name="folder-properties"></a>Propriétés de dossier  
 Outre les propriétés d'élément répertoriées précédemment, la propriété suivante s'applique aux dossiers dans la base de données du serveur de rapports.  
  
|Propriété|Description|  
|--------------|-----------------|  
|**Reserved**|Valeur retournée par la méthode <xref:ReportService2010.ReportingService2010.GetProperties%2A> pour les dossiers réservés par le serveur de rapports. Les dossiers réservés comprennent Users, My Reports et /. Les dossiers réservés ne peuvent être ni modifiés ni supprimés.|  
  
## <a name="report-properties"></a>Propriétés des états  
 Outre les propriétés d'élément répertoriées précédemment, les propriétés suivantes s'appliquent aux rapports dans la base de données du serveur de rapports.  
  
|Propriété|Description|  
|--------------|-----------------|  
|**Langage**|Langue utilisé dans un rapport. La valeur est un code de langue défini dans la spécification RFC1766 du groupe de travail IETF (Internet Engineering Task Force). La première partie est une désignation de deux caractères de la langue de base. La deuxième partie est séparée par un trait d'union et désigne la variation ou le dialecte de la langue. Si aucune valeur n’est spécifiée dans l’élément **Style** associé à l’élément **Body** dans la définition du rapport, la valeur par défaut est la langue du serveur de rapports.|  
|**ReportProcessingTimeout**|Délai d'expiration, en secondes, d'un rapport individuel. Si cette valeur est définie, le serveur de rapports tente d'arrêter le traitement d'un rapport lorsque le délai spécifié est écoulé. Les valeurs valides sont comprises entre **-1** et **2** **147** **483** **647**. Si la valeur est **-1**, le rapport n’est soumis à aucun délai d’expiration au cours du traitement. Si la valeur est **null**, la valeur de la propriété système **ReportProcessingTimeout** est utilisée pour le délai de traitement des rapports. La valeur par défaut est **null**. Pour plus d’informations, consultez [Propriétés système de Report Server](../../../reporting-services/report-server-web-service/net-framework/reporting-services-properties-report-server-system-properties.md).|  
|**ExecutionDate**|Date et heure de dernière création d'un instantané de rapport pour un rapport.|  
|**CanRunUnattended**|Valeur qui indique si un rapport peut être exécuté sans assistance selon une planification. Si cette propriété a la valeur **true**, les valeurs par défaut pour les paramètres de rapport sont définies et les informations d’identification de la source de données sont stockées avec le rapport, ou l’option de récupération des informations d’identification a la valeur **None**. Si cette propriété a la valeur **false**, les conditions préalables pour exécuter un rapport sans assistance ne sont pas satisfaites. Pour plus d’informations, consultez [Configurer le compte d’exécution sans assistance &#40;Gestionnaire de configuration du serveur de rapports&#41;](../../../reporting-services/install-windows/configure-the-unattended-execution-account-ssrs-configuration-manager.md).|  
|**HasParameterDefaultValues**|Valeur qui indique si le rapport a des valeurs par défaut valides définies pour tous les paramètres de rapport. La valeur est également **true** si un rapport ne possède pas de paramètres de rapport. Si cette propriété a la valeur **false**, un ou plusieurs paramètres de rapport n’ont pas de valeur par défaut valide.|  
|**HasDataSourceCredentials**|Valeur qui indique que l’option de récupération des informations d’identification définie pour toutes les sources de données associées au rapport est **None** ou **Store**. Si cette propriété a la valeur **false**, une option de récupération des informations d’identification définie pour l’une des sources de données associées au rapport est **Integrated** ou **Prompt**.|  
|**IsSnapshotExecution**|Valeur qui indique si le rapport est un instantané.|  
|**HasScheduleReadyDataSources**|Valeur qui indique si les sources de données d'un rapport sont configurées pour prendre en charge l'exécution planifiée. Si cette propriété a la valeur **false**, les utilisateurs ne peuvent pas s’abonner au rapport.|  
  
## <a name="resource-properties"></a>Propriétés de ressource  
 Outre les propriétés d'élément répertoriées précédemment, la propriété suivante s'applique aux ressources dans la base de données du serveur de rapports.  
  
|Propriété|Description|  
|--------------|-----------------|  
|**MimeType**|Type MIME d'une ressource dans la base de données du serveur de rapports.|  
  
## <a name="see-also"></a>Voir aussi  
 [Création d’applications à l’aide du service web et du .NET Framework](../../../reporting-services/report-server-web-service/net-framework/building-applications-using-the-web-service-and-the-net-framework.md)   
 [Service web Report Server](../../../reporting-services/report-server-web-service/report-server-web-service.md)   
 [Informations techniques de référence &#40;SSRS&#41;](../../../reporting-services/technical-reference-ssrs.md)  
  
  
