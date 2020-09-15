---
title: rsModelGenerationError - Erreur Reporting Services | Microsoft Docs
description: "Dans cette page de référence d’erreur, découvrez l’ID d’événement « rsModelGenerationError » : Une erreur s'est produite lors de la génération du modèle."
ms.date: 03/14/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: troubleshooting
ms.topic: conceptual
helpviewer_keywords:
- rsModelGenerationError
ms.assetid: 3a0ad63f-87f9-4ca1-b0c2-c85fa991634a
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: d49619a5005b542478f5be971b5206f492b66027
ms.sourcegitcommit: df1f0f2dfb9452f16471e740273cd1478ff3100c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/29/2020
ms.locfileid: "87396738"
---
# <a name="rsmodelgenerationerror---reporting-services-error"></a>rsModelGenerationError - Erreur Reporting Services
    
## <a name="details"></a>Détails  
  
|Category|Valeur|  
|-|-|  
|Nom du produit|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]|  
|ID de l’événement|rsRenderingError|  
|Source de l’événement|Microsoft.ReportingServices.Diagnostics.Utilities.ErrorStrings|  
|Composant|[!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]|  
|Texte du message|Une erreur s'est produite lors de la génération du modèle. (rsModelGenerationError) (ReportingServicesLibrary) %1|  
  
## <a name="explanation"></a>Explication  
 Le modèle de rapport n'a pas pu être généré. Dans [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]2005 SP1 et les versions antérieures, cette erreur s'affiche, en règle générale, si l'objet System.Data.DataSet ne peut pas gérer une table ou une relation au sein du schéma de la base de données, par exemple lorsque deux clés étrangères sont définies sur la même colonne d'une table.  
  
## <a name="user-action"></a>Action de l'utilisateur  
 Pour déterminer précisément la raison pour laquelle vous avez reçu ce message, consultez les fichiers journaux du serveur de rapports qui se trouvent dans le dossier \Microsoft SQL Server\\<Instance SQL Server\>\Reporting Services\LogFiles.  
  
## <a name="internal-only"></a>Interne uniquement  
  
