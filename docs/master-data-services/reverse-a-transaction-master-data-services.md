---
description: Inverser une transaction (Master Data Services)
title: Inverser une transaction
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
helpviewer_keywords:
- transactions [Master Data Services], reversing
ms.assetid: 6f7c3f07-0f64-4283-8c9c-93facd00a046
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: 72598e6b0dd5c86cb5f231c4ac01ffd1d499b98b
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100340238"
---
# <a name="reverse-a-transaction-master-data-services"></a>Inverser une transaction (Master Data Services)

[!INCLUDE [SQL Server - Windows only ASDBMI  ](../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  Dans [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)], les administrateurs peuvent inverser une transaction lorsqu'une action doit être annulée. Les exemples de transactions sont les suivants : modifications apportées aux valeurs d'attribut, déplacements de hiérarchie ou suppressions de membres. Cette rubrique s’applique seulement aux transactions des entités ayant le type de journal des transactions « Attribut ». Accédez à la page de l’explorateur d’entités pour afficher l’historique de transaction des entités avec le type de journal des transactions « Membre ».  
  
## <a name="prerequisites"></a>Prérequis  
  
-   Vous devez avoir l’autorisation d’accéder à la zone fonctionnelle **Gestion des versions** .  
  
-   Vous devez être administrateur de modèle. Pour plus d’informations, consultez [administrateurs &#40;Master Data Services&#41;](../master-data-services/administrators-master-data-services.md).  
  
### <a name="to-reverse-a-transaction"></a>Pour inverser une transaction  
  
1.  Dans la page d’accueil de [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)] , cliquez sur **Gestion des versions**.  
  
2.  Dans la barre de menus, cliquez sur **Transactions**.  
  
3.  Dans la page **Transactions** , dans la liste **Modèle** , sélectionnez un modèle.  
  
4.  Dans la liste **Version** , sélectionnez une version.  
  
5.  Cliquez dans la grille sur la ligne correspondant à la transaction que vous souhaitez inverser.  
  
6.  Cliquez sur **Inverser la transaction**.  
  
7.  Dans la boîte de dialogue de confirmation, cliquez sur **OK**. Une autre transaction est ajoutée à la grille pour enregistrer la transaction inversée.  
  
## <a name="see-also"></a>Voir aussi  
 [Transactions &#40;Master Data Services&#41;](../master-data-services/transactions-master-data-services.md)   
 [Réactiver un membre ou une collection &#40;Master Data Services&#41;](../master-data-services/reactivate-a-member-or-collection-master-data-services.md)  
 [Restauration de l’historique de révision de membre](../master-data-services/rollback-member-revision-history-master-data-services.md)
  
  
