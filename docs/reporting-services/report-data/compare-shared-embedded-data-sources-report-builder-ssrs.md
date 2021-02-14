---
description: Comparer des sources de données partagées et incorporées - Générateur de rapports et Reporting Services (SSRS)
title: Comparer des sources de données partagées et incorporées - Générateur de rapports et Reporting Services | Microsoft Docs
ms.date: 11/18/2019
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-data
ms.topic: conceptual
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 5e2c72ad95392be7571c05e1dddcee1ed35c3d6a
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100042219"
---
# <a name="compare-shared-and-embedded-data-sources---report-builder--reporting-services-ssrs"></a>Comparer des sources de données partagées et incorporées - Générateur de rapports et Reporting Services (SSRS)

[!INCLUDE [ssrs-appliesto](../../includes/ssrs-appliesto.md)] [!INCLUDE [ssrs-appliesto-2016-and-later](../../includes/ssrs-appliesto-2016-and-later.md)] [!INCLUDE[ssrs-appliesto-pbirsi](../../includes/ssrs-appliesto-pbirs.md)] [!INCLUDE[ssrs-appliesto-sharepoint-2013-2016i](../../includes/ssrs-appliesto-sharepoint-2013-2016.md)]
 
Vous pouvez vous connecter à des données à l’aide d’une source de données partagée ou incorporée. Une *source de données partagée* est définie indépendamment d’un rapport. Vous pouvez l’utiliser dans plusieurs rapports d’un serveur de rapports ou du site SharePoint. Une *source de données incorporée* est définie dans un rapport. Vous ne pouvez l’utiliser que dans ce rapport. 

 Les sources de données partagées sont utiles lorsque vous disposez de sources de données que vous utilisez souvent. La création et le partage de sources de données partagées sont recommandés dans la mesure du possible. Celles-ci permettent de gérer plus facilement les rapports et l'accès aux rapports, et de sécuriser davantage les rapports et les sources de données auxquelles ils accèdent. Si vous avez besoin d'une source de données partagée, demandez à votre administrateur système d'en créer une pour vous.  
  
 Une source de données incorporée, également appelée *source de données spécifique au rapport* est une connexion de données enregistrée dans la définition de rapport. Les informations de connexion des sources de données incorporées peuvent être utilisées uniquement par le rapport dans lequel elles sont incorporées. Pour définir et gérer des sources de données incorporées, utilisez la boîte de dialogue **Propriétés de la source de données** .  
  
 La différence entre des sources de données incorporées réside dans leur mode de création, de stockage et de gestion.  
  
-   Dans le Concepteur de rapports, créez des sources de données incorporées ou partagées dans le cadre d'un projet [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)] . Vous pouvez choisir de les utiliser localement pour l'aperçu ou de les déployer dans le cadre du projet sur un serveur de rapports ou sur un site SharePoint. Vous pouvez utiliser les extensions de données personnalisées qui ont été installées sur votre ordinateur et sur le serveur de rapports ou le site SharePoint sur lequel vous déployez vos rapports.  
  
     Les administrateurs système peuvent installer et configurer des extensions supplémentaires pour le traitement des données, ainsi que des fournisseurs de données .NET Framework. Pour plus d’informations, consultez [Extensions pour le traitement des données et fournisseurs de données .NET Framework &#40;SSRS&#41;](../../reporting-services/report-data/data-processing-extensions-and-net-framework-data-providers-ssrs.md).  
  
     Les développeurs peuvent utiliser l’API <xref:Microsoft.ReportingServices.DataProcessing> pour créer des extensions pour le traitement des données permettant de prendre en charge d’autres types de sources de données.  
  
-   Dans le [!INCLUDE[ssRBnoversion](../../includes/ssrbnoversion.md)], accédez à un serveur de rapports ou à un site SharePoint et sélectionnez les sources de données partagées ou créez des sources de données incorporées dans le rapport. Vous pouvez créer une source de données partagée dans [!INCLUDE[ssRBnoversion](../../includes/ssrbnoversion.md)]. Vous ne pouvez pas utiliser des extensions de données personnalisées dans [!INCLUDE[ssRBnoversion](../../includes/ssrbnoversion.md)].  

## <a name="summary-of-differences"></a>Récapitulatif des différences
  
 Le tableau ci-après récapitule les différences entre les sources de données incorporées et partagées :  
  
|Description|Embedded<br /><br /> source de données|Partagé<br /><br /> source de données|  
|-----------------|------------------------------|----------------------------|  
|La connexion de données est incorporée dans la définition de rapport.|![Disponible](../../reporting-services/report-data/media/greencheck.gif "Disponible")||  
|Le pointeur vers la connexion de données sur le serveur de rapports est incorporé dans la définition de rapport.||![Disponible](../../reporting-services/report-data/media/greencheck.gif "Disponible")|  
|Gestion sur le serveur de rapports|![Disponible](../../reporting-services/report-data/media/greencheck.gif "Disponible")|![Disponible](../../reporting-services/report-data/media/greencheck.gif "Disponible")|  
|Obligatoire pour les datasets partagés||![Disponible](../../reporting-services/report-data/media/greencheck.gif "Disponible")|  
|Obligatoire pour les composants||![Disponible](../../reporting-services/report-data/media/greencheck.gif "Disponible")|  

## <a name="next-steps"></a>Étapes suivantes

[Créer et gérer des sources de données partagées](../../reporting-services/report-data/create-modify-and-delete-shared-data-sources-ssrs.md)   
[Créer et modifier des sources de données incorporées](../../reporting-services/report-data/create-and-modify-embedded-data-sources.md)   
[Définir des propriétés de déploiement](../../reporting-services/tools/set-deployment-properties-reporting-services.md)   
[Spécifier des informations d'identification et de connexion pour les sources de données de rapports](../../reporting-services/report-data/specify-credential-and-connection-information-for-report-data-sources.md)  

D’autres questions ? [Essayez de poser une question dans le forum Reporting Services](https://go.microsoft.com/fwlink/?LinkId=620231)
