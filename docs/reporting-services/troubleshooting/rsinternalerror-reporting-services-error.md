---
title: rsInternalError - Erreur Reporting Services | Microsoft Docs
description: "Dans cette page de référence d’erreur, découvrez l’ID d’événement « rsInternalError » : Une erreur interne s'est produite sur le serveur de rapports. Consultez le journal des erreurs pour plus d'informations."
ms.date: 03/14/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: troubleshooting
ms.topic: conceptual
helpviewer_keywords:
- rsInternalError
ms.assetid: 52613d52-fc78-4870-93f0-7d393ab9c335
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: d8df18bf9487d1d8797eff4e60f5ec5eefb87c4f
ms.sourcegitcommit: df1f0f2dfb9452f16471e740273cd1478ff3100c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/29/2020
ms.locfileid: "87394720"
---
# <a name="rsinternalerror---reporting-services-error"></a>rsInternalError - Erreur Reporting Services
    
## <a name="details"></a>Détails  
  
|Category|Valeur|  
|-|-|  
|Nom du produit|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]|  
|ID de l’événement|rsInternalError|  
|Source de l’événement|Microsoft.ReportingServices.Diagnostics.Utilities.ErrorStrings|  
|Composant|[!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]|  
|Texte du message|Une erreur interne s'est produite sur le serveur de rapports. Consultez le journal des erreurs pour plus d'informations.|  
  
## <a name="explanation"></a>Explication  
 Il s'agit d'un message d'erreur générique qui est souvent suivi par un autre message plus descriptif fournissant des informations détaillées.  
  
 Les erreurs internes sont rares. Si vous recevez ce message, des informations supplémentaires sont disponibles dans les journaux des traces du serveur de rapports. En outre, si vous êtes connecté en tant qu'administrateur local sur l'ordinateur sur lequel l'erreur s'est produite, vous pouvez afficher la pile des appels pour avoir accès à des informations supplémentaires.  
  
## <a name="user-action"></a>Action de l'utilisateur  
 Pour déterminer la cause spécifique de ce message, consultez les fichiers journaux du serveur de rapports qui se trouvent dans le dossier \Microsoft SQL Server\MSRS12.\<instancename >\Reporting Services\LogFiles. Pour plus d’informations, consultez [Fichiers journaux et sources de Reporting Services](../../reporting-services/report-server/reporting-services-log-files-and-sources.md).  
  
 Pour afficher la pile des appels, cliquez avec le bouton droit sur la page sur laquelle l’erreur s’est produite et pointez sur **Afficher la source**. La consultation de la pile des appels requiert des autorisations d'administrateur sur l'ordinateur où l'erreur s'est produite.  
  
 Si aucune information supplémentaire n'est disponible, vous pouvez réessayer votre demande.  
  
## <a name="internal-only"></a>Interne uniquement  
  
## <a name="see-also"></a>Voir aussi  
 [Démarrer et arrêter le service du serveur de rapports](../../reporting-services/report-server/start-and-stop-the-report-server-service.md)  
  
  
