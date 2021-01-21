---
title: Nouvelle planification partagée (Management Studio) | Microsoft Docs
description: Découvrez comment créer une planification partagée pour exécuter des rapports et des abonnements publiés à l’aide des options de la page Nouvelle planification dans SQL Server Management Studio.
ms.date: 03/14/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: tools
ms.topic: conceptual
f1_keywords:
- sql13.swb.reportserver.newschedule.f1
ms.assetid: b2c69586-d98e-4933-827d-f5e6c15c5203
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 2ec7ca644642125280bdc2f6821faec2b511a45d
ms.sourcegitcommit: d8cdbb719916805037a9167ac4e964abb89c3909
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/20/2021
ms.locfileid: "98596468"
---
# <a name="new-shared-schedule-management-studio"></a>Nouvelle planification partagée (Management Studio)
  Utilisez cette page pour créer une planification partagée pour exécuter les rapports publiés et les abonnements. Les planifications partagées peuvent être utilisées à la place des planifications spécifiques des rapports ou spécifiques des abonnements. Des informations de planification centralisées et la capacité de suspendre et de reprendre les opérations planifiées sont deux fonctionnalités clés qui distinguent les planifications partagées des planifications spécifiques aux éléments.  
  
 Toutes les combinaisons de fréquence ne peuvent pas être prises en charge dans une seule planification. Par exemple, si vous souhaitez exécuter un rapport à 12:00 et 16:00 ous les vendredis, vous devez créer deux planifications quotidiennes qui spécifient le vendredi comme jour d'exécution mais avec une heure de début de 12:00 pour l'une et une heure de début de 16:00 pour la seconde.  
  
 Le traitement de la planification est basé sur l'heure locale du serveur de rapports qui héberge et traite la planification.  
  
 Pour ouvrir cette page, démarrez [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], connectez-vous à un serveur de rapports, cliquez avec le bouton droit sur **Planification partagée** et sélectionnez **Nouvelle planification**. Pour enregistrer la planification, le service Agent SQL Server doit être en cours d'exécution.  
  
> [!NOTE]  
>  Cette fonctionnalité n'est pas disponible dans toutes les éditions de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Pour obtenir la liste des fonctionnalités prises en charge par les éditions de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], consultez [Fonctionnalités prises en charge par les éditions de SQL Server 2012](/previous-versions/sql/sql-server-2012/cc645993(v=sql.110)) (https://go.microsoft.com/fwlink/?linkid=232473).  
  
## <a name="options"></a>Options  
 **Nom**  
 Tapez le nom de la planification partagée. Ce nom apparaît dans les listes déroulantes lorsque les utilisateurs sélectionnent une planification partagée pour les rapports et les abonnements. Veillez à fournir un nom descriptif qui s'ajuste facilement dans une liste et qui distingue aisément une planification partagée d'une autre. Le nom doit contenir un caractère alphanumérique au minimum. Il peut également comporter des espaces et quelques symboles. N'utilisez pas les caractères suivants dans le nom :  
  
 ; ? : \@ & = + , $ / * < >  
  
 " /  
  
 **Commencer l'exécution de cette planification le**  
 Spécifiez la date de début de cette planification.  
  
 **Arrêter cette planification le**  
 Spécifiez la date d'expiration de cette planification.  
  
 **Type**  
 Spécifie si la périodicité est basée essentiellement sur les heures, jours, semaines ou mois.  
  
 **Heure (Modèle de récurrence)**  
 Sélectionnez les options d'exécution d'une opération planifiée par intervalles d'une heure (par exemple, pour exécuter un rapport toutes les 6 heures). Vous pouvez préciser l'intervalle en heures et en minutes.  
  
 **Jour (Modèle de récurrence)**  
 Sélectionnez les options d'exécution d'une opération planifiée par intervalles d'un jour (par exemple, pour exécuter un rapport tous les 2 jours). Vous pouvez spécifier l'intervalle en jours et à l'heure à laquelle vous voulez exécuter la planification.  
  
 **Semaine (Modèle de récurrence)**  
 Sélectionnez les options d'exécution d'une opération planifiée par intervalles d'une semaine ou lorsque la périodicité à répéter est en semaines (par exemple, pour exécuter un rapport une semaine sur deux). Vous pouvez spécifier une planification hebdomadaire au jour et à l'heure où vous voulez l'exécuter.  
  
 **Mois (Modèle de récurrence)**  
 Sélectionnez les options d'exécution d'une opération planifiée par intervalles d'un mois ou lorsque la périodicité à répéter est en mois. Vous pouvez spécifier une planification mensuelle au jour et à l'heure où vous voulez l'exécuter. Vous pouvez ignorer certains mois dans la planification.  
  
 **Une fois**  
 Sélectionnez cette option pour créer une planification exécutée une seule fois, à une date et une heure spécifiques.  
  
## <a name="see-also"></a>Voir aussi  
 [Aide du serveur de rapports dans Management Studio accessible par la touche F1](../../reporting-services/tools/report-server-in-management-studio-f1-help.md)   
 [Se connecter à un serveur de rapports dans Management Studio](../../reporting-services/tools/connect-to-a-report-server-in-management-studio.md)   
 [Create, Modify, and Delete Schedules](../../reporting-services/subscriptions/create-modify-and-delete-schedules.md)   
 [Planifications](../../reporting-services/subscriptions/schedules.md)   
 [Aide du serveur de rapports dans Management Studio via la touche F1](../../reporting-services/tools/report-server-in-management-studio-f1-help.md)  
  
