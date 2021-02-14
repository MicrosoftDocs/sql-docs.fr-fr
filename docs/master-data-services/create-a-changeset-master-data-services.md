---
description: Créer un ensemble de modifications (Master Data Services)
title: Créer un ensemble de modifications
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
ms.assetid: cfad6f1c-9125-4896-b5f5-a4b9f9593cc4
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: 7aa771612709a47f5dce24038af6307dc41d95e7
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100272730"
---
# <a name="create-a-changeset-master-data-services"></a>Créer un ensemble de modifications (Master Data Services)

[!INCLUDE [SQL Server - Windows only ASDBMI  ](../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  Un ensemble de modifications regroupe des modifications en attente, qui portent sur les données de référence. Si l’entité requiert l’approbation des modifications, les modifications en attente doivent être enregistrées dans un ensemble de modifications, puis soumises à l’administrateur à des fins d’approbation.  
  
## <a name="prerequisites"></a>Prérequis  
  
-   Vous devez être autorisé à accéder à la zone fonctionnelle Explorateur. Pour plus d’informations, consultez [autorisations de zone fonctionnelle &#40;Master Data Services&#41;](../master-data-services/functional-area-permissions-master-data-services.md)  
  
-   Vous devez disposer au minimum d’un accès en lecture à l’entité ou à l’un de ses attributs.  
  
## <a name="to-create-a-local-changeset"></a>Pour créer un ensemble de modifications local  
  
1.  Sur la page d’accueil de [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)] , sélectionnez le modèle et la version, puis cliquez sur **Explorateur**.  
  
2.  Dans le menu **Entités** , cliquez sur une entité.  
  
3.  Dans le volet de droite, sélectionnez **Ensembles de modifications** et cliquez sur **Créer**.  
  
4.  Donnez un nom au fichier, puis cliquez sur **Enregistrer**.  
  
     Le nom de l’ensemble de modifications doit être unique au sein d’un modèle.  
  
## <a name="to-create-a-changeset-for-approval"></a>Pour créer un ensemble de modifications pour approbation  
  
1.  Sur la page d’accueil de [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)] , sélectionnez le modèle et la version, puis cliquez sur **Explorateur**.  
  
2.  Dans le menu **Entités** , cliquez sur une entité.  
  
3.  Apportez des modifications à l’entité, puis cliquez sur **OK**.  
  
4.  **Choisir un ensemble de modifications** s’affiche.  
  
5.  Cliquez sur **Nouveau**, donnez un nom à l’ensemble de modifications et cliquez sur **Enregistrer**. Le nom de l’ensemble de modifications doit être unique dans le modèle.  
  
6.  Pour utiliser un ensemble de modifications existant, cliquez sur **Existant** et choisissez l’ensemble de modifications dans la liste. Seuls les ensembles de modifications qui se trouvent à l’état ouvert ou rejeté sont disponibles.  
  
## <a name="next-steps"></a>Étapes suivantes  
 [Appliquer et mettre à jour un ensemble de modifications &#40;Master Data Services&#41;](../master-data-services/apply-and-update-a-changeset-master-data-services.md)  
  
## <a name="see-also"></a>Voir aussi  
 [Valider ou envoyer un ensemble de modifications &#40;Master Data Services&#41;](../master-data-services/commit-or-submit-a-changeset-master-data-services.md)   
 [Approuver ou rejeter un ensemble de modifications &#40;Master Data Services&#41;](../master-data-services/approve-or-reject-a-changeset-master-data-services.md)  
  
  
