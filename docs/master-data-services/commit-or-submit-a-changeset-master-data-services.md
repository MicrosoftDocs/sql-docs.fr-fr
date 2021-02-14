---
description: Valider ou envoyer un ensemble de modifications (Master Data Services)
title: Valider ou envoyer un ensemble de modifications
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
ms.assetid: d323bbac-c8d4-4d2f-a7d2-a597e8b53e2d
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: 4e9e7076497208bb5113312c7305eab21655e1ca
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100272780"
---
# <a name="commit-or-submit-a-changeset-master-data-services"></a>Valider ou envoyer un ensemble de modifications (Master Data Services)

[!INCLUDE [SQL Server - Windows only ASDBMI  ](../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  Un ensemble de modifications regroupe des modifications en attente, qui portent sur les données de référence. Si les modifications apportées à une entité ne nécessitent pas l’approbation de l’administrateur, vous pouvez valider l’ensemble de modifications. Si les modifications apportées à une entité nécessitent l’approbation de l’administrateur, vous pouvez envoyer l’ensemble de modifications pour approbation.  
  
## <a name="prerequisites"></a>Prérequis  
  
-   Vous devez avoir l'autorisation d'accéder à la zone fonctionnelle **Explorateur** . Pour plus d’informations, consultez [autorisations de zone fonctionnelle &#40;Master Data Services&#41;](../master-data-services/functional-area-permissions-master-data-services.md)  
  
-   Si les modifications apportées à une entité ne nécessitent pas l’approbation de l’administrateur, vous pouvez valider l’ensemble de modifications uniquement si vous possédez l’ensemble de modifications et si l’état de l’ensemble de modifications est ouvert.  
  
-   Si les modifications apportées à une entité nécessitent l’approbation de l’administrateur, vous pouvez envoyer l’ensemble de modifications pour approbation uniquement si vous possédez l’ensemble de modifications et si l’état de l’ensemble de modifications est ouvert ou rejeté.  
  
## <a name="to-commit-a-local-changeset"></a>Pour valider un ensemble de modifications local  
 L’option de validation n’est disponible pour les ensembles de modifications locaux que sur les entités où l’administrateur d’entité n’a pas activé la nécessité d’approbation.  
  
1.  Sur la [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)] page d’hébergement, sélectionnez le modèle et la version, puis cliquez sur **Explorateur**.  
  
2.  Dans le menu **Entités** , cliquez sur une entité.  
  
3.  Dans le volet droit, sélectionnez **Ensembles de modifications** , puis double-cliquez sur l’ensemble de modifications à valider.  
  
4.  Cliquez sur **Valider**.  
  
## <a name="to-submit-a-changeset"></a>Pour envoyer un ensemble de modifications  
 L’option d’envoi n’est disponible pour les ensembles de modifications que sur les entités où l’administrateur d’entité a activé la nécessité d’approbation.  
  
1.  Sur la [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)] page d’hébergement, sélectionnez le modèle et la version, puis cliquez sur **Explorateur**.  
  
2.  Dans le menu **Entités** , cliquez sur une entité.  
  
3.  Dans le volet droit, sélectionnez **Ensembles de modifications** , puis double-cliquez sur l’ensemble de modifications à envoyer.  
  
4.  Cliquez sur **Envoyer**.  
  
## <a name="next-steps"></a>Étapes suivantes  
 [Approuver ou rejeter un ensemble de modifications &#40;Master Data Services&#41;](../master-data-services/approve-or-reject-a-changeset-master-data-services.md)  
  
## <a name="see-also"></a>Voir aussi  
 [Créer un ensemble de modifications &#40;Master Data Services&#41;](../master-data-services/create-a-changeset-master-data-services.md)   
 [Appliquer et mettre à jour un ensemble de modifications &#40;Master Data Services&#41;](../master-data-services/apply-and-update-a-changeset-master-data-services.md)   
 [Approuver ou rejeter un ensemble de modifications &#40;Master Data Services&#41;](../master-data-services/approve-or-reject-a-changeset-master-data-services.md)  
  
  
