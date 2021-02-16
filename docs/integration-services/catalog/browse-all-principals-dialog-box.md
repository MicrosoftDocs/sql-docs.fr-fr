---
description: Parcourir tous les principaux, boîte de dialogue
title: Boîte de dialogue Parcourir tous les principaux | Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- sql13.ssis.ssms.browseprincipals.f1
ms.assetid: f11d2c5e-ee05-45f3-8dc2-0feb99b2f76f
author: chugugrace
ms.author: chugu
ms.openlocfilehash: e8179f9f21d875ab5e622d9e4afe03772d83ea5a
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100354994"
---
# <a name="browse-all-principals-dialog-box"></a>Parcourir tous les principaux, boîte de dialogue

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  Utilisez la boîte de dialogue **Parcourir tous les principaux** pour sélectionner un principal de base de données et modifier les autorisations du principal sur le projet sélectionné ou sur tous les projets contenus dans un dossier sélectionné.  
  
 **Que voulez-vous faire ?**  
  
-   [Ouvrir la boîte de dialogue Parcourir tous les principaux](#open_dialog)  
  
-   [Configurer les options](#options)  
  
##  <a name="open-the-browse-all-principals-dialog-box"></a><a name="open_dialog"></a> Ouvrir la boîte de dialogue Parcourir tous les principaux  
  
1.  Dans [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], connectez-vous au serveur [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] .  
  
     Vous devez vous connecter à l’instance du [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] qui héberge le catalogue SSISDB.  
  
2.  Dans l'Explorateur d'objets, développez l'arborescence pour afficher le nœud **Integration Services Catalogues** .  
  
3.  Développez le nœud **SSISDB** .  
  
4.  Pour changer les autorisations du principal sur tous les projets contenus dans un dossier sélectionné, cliquez avec le bouton droit sur le dossier, puis cliquez sur **Propriétés**.  
  
     Pour changer les autorisations du principal sur un projet sélectionné, développez le dossier qui contient le projet, cliquez avec le bouton droit sur le projet, puis cliquez sur **Propriétés**.  
  
5.  Sélectionnez la page **Autorisations** , puis cliquez sur **Parcourir**.  
  
##  <a name="configure-the-options"></a><a name="options"></a> Configurer les options  
 Cette page affiche les principaux de la vue catalogue, sys.database_principals, de la base de données SSISDB.  
  
 La sélection de principaux les ajoute à la liste **Connexions ou rôles** dans la page **Autorisations** de la boîte de dialogue parent lorsque vous cliquez sur **OK** et fermez la boîte de dialogue **Parcourir tous les principaux** . Après avoir ajouté des principaux à la liste **Connexions ou rôles** , vous pouvez modifier leurs autorisations sur le projet sélectionné.  
  
 **Sélection de colonne**  
 Sélectionnez cette option pour ajouter le principal à la liste **Connexions ou rôles** dans la page **Autorisations** de la boîte de dialogue parent.  
  
 **Colonne Icône**  
 Icône qui correspond au **Type** du principal.  
  
 **Nom**  
 Nom du principal.  
  
 **Type**  
 Type du principal. Les types courants sont :  
  
-   Utilisateur SQL  
  
-   Utilisateur Windows  
  
-   Rôle de base de données  
  
  
