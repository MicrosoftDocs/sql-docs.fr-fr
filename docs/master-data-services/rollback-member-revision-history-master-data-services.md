---
description: Restauration de l’historique de révision de membre (Master Data Services)
title: Restauration de l’historique de révision de membre
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
ms.assetid: d39d3474-20e7-429f-9c8d-fcc4eb0854fc
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: ec99fc4d464037b4051da46e293efb4b2fb6969a
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100355285"
---
# <a name="rollback-member-revision-history-master-data-services"></a>Restauration de l’historique de révision de membre (Master Data Services)

[!INCLUDE [SQL Server - Windows only ASDBMI  ](../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  Un historique de révision de membre est enregistré chaque fois qu’un membre fait l’objet d’une modification. Vous pouvez restaurer la version précédente d’un historique de révision de membre.  
  
## <a name="prerequisites"></a>Prérequis  
  
-   Vous devez disposer des autorisations requises pour mettre à jour au moins l’un des attributs du membre sélectionné. Lorsque vous restaurez un historique de révision, toutes les valeurs d’attribut qui peuvent être mises à jour sont restaurées à celles de la version précédente.  
  
-   L’historique de révision est disponible uniquement lorsque le type de journal des transactions de l’entité est un membre.  
  
 **Pour restaurer un historique de révision de membre**  
  
1.  Dans Master Data Manager, cliquez sur Explorateur  
  
2.  Choisissez l’entité et le membre à restaurer.  
  
3.  Cliquez sur **Afficher l’historique**.  
  
4.  Choisissez la révision à restaurer, puis cliquez sur **Restaurer**.  
  
## <a name="see-also"></a>Voir aussi  
 [Historique de révision de membre &#40;Master Data Services&#41;](../master-data-services/member-revision-history-master-data-services.md)   
 [Changer le type du journal des transactions de l’entité &#40;Master Data Services&#41;](../master-data-services/change-the-entity-transaction-log-type-master-data-services.md)  
  
  
