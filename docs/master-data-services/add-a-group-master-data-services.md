---
description: Ajouter un groupe (Master Data Services)
title: Ajouter un groupe
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
helpviewer_keywords:
- groups [Master Data Services], adding
- adding groups [Master Data Services]
ms.assetid: c7a88381-3b2c-4af7-9cf7-3a930c1abdee
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: 8ddc803c87e5c899f482a123f3217c8c13bc0b3b
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100272910"
---
# <a name="add-a-group-master-data-services"></a>Ajouter un groupe (Master Data Services)

[!INCLUDE [SQL Server - Windows only ASDBMI  ](../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  Ajoutez un groupe à la liste **Groupes** dans [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)] pour commencer la procédure d’affectation d’une autorisation d’accès à l’application web. Pour qu’un utilisateur du groupe puisse accéder à [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)], vous devez accorder au groupe une autorisation d’accès à un ou plusieurs objets de modèle et zones fonctionnelles.  
  
## <a name="prerequisites"></a>Prérequis  
 Pour effectuer cette procédure :  
  
-   Vous devez avoir l'autorisation d'accéder à la zone fonctionnelle **Autorisations d'accès** .  
  
### <a name="to-add-a-group"></a>Pour ajouter un groupe  
  
1.  Dans [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)], cliquez sur **Autorisations d'accès**.  
  
2.  Dans la page **Utilisateurs** , dans la barre de menus, cliquez sur **Gérer les groupes**.  
  
3.  Cliquez sur **Ajouter des groupes**.  
  
4.  Tapez le nom du groupe précédé du nom de domaine Active Directory ou du nom du serveur, comme dans *domaine\nom_groupe* ou *ordinateur\nom_groupe*.  
  
5.  Cliquez éventuellement sur **Vérifier les noms**.  
  
6.  Cliquez sur **OK**.  
  
    > [!NOTE]  
    >  Quand l’utilisateur accède pour la première fois à [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)], son nom est ajouté à la liste des utilisateurs de [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)] .  
  
## <a name="next-steps"></a>Étapes suivantes  
  
-   [Affecter des autorisations de zone fonctionnelle &#40;Master Data Services&#41;](../master-data-services/assign-functional-area-permissions-master-data-services.md)  
  
## <a name="see-also"></a>Voir aussi  
 [Sécurité &#40;Master Data Services&#41;](../master-data-services/security-master-data-services.md)  
  
  
