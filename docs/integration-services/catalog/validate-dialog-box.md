---
description: Boîte de dialogue Valider
title: Boîte de dialogue Valider | Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- sql13.ssis.ssms.isprojectvalidate.f1
- sql13.ssis.ssms.ispackagevalidate.f1
ms.assetid: 134e14ce-4f8d-4a20-889a-918014c841d8
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 2826ed7de2ffe70267d8c0627dd3ec12b6d07c97
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100339254"
---
# <a name="validate-dialog-box"></a>Boîte de dialogue Valider

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  Utilisez la boîte de dialogue **Valider** pour examiner les problèmes usuels d'un projet ou package [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] .  
  
 En cas de problème, un message s'affiche en haut de la boîte de dialogue. Sinon, le terme Prêt s'affiche en haut.  
  
 **Que voulez-vous faire ?**  
  
-   [Ouvrir la boîte de dialogue Valider](#open_dialog)  
  
-   [Définir les options sur la page Général](#general)  
  
##  <a name="open-the-validate-dialog-box"></a><a name="open_dialog"></a> Ouvrir la boîte de dialogue Valider  
  
1.  Dans [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], connectez-vous au serveur [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] .  
  
     Vous vous connectez à l’instance du [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] qui héberge la base de données SSISDB.  
  
2.  Dans l'Explorateur d'objets, développez l'arborescence pour afficher le nœud **Integration Services Catalogues** .  
  
3.  Développez le nœud **SSISDB** .  
  
4.  Développez le dossier qui contient le projet ou package à valider.  
  
5.  Cliquez avec le bouton droit sur le projet ou le package, puis cliquez sur **Valider**.  
  
##  <a name="set-the-options-on-the-general-page"></a><a name="general"></a> Définir les options sur la page Général  
 **Environment**  
 Sélectionnez l'environnement à utiliser pour valider le projet ou le package.  
  
 **Runtime 32 bits**  
 Sélectionnez cette option afin d'utiliser un runtime 32 bits pour exécuter un package.  
  
 L'onglet **Paramètres** indique les valeurs de paramètres que vous utilisez pour valider le projet ou le package. Voici les options de l'onglet Paramètres.  
  
 **Conteneur**  
 Indique l'objet qui contient le paramètre.  
  
 **Paramètre**  
 Indique le nom des paramètres.  
  
 **Valeur**  
 Indique la valeur du paramètre.  
  
 L'onglet **Gestionnaires de connexions** indique les valeurs de propriétés du gestionnaire de connexions que vous utilisez pour valider le projet ou le package.  
  
 Voici les options de l'onglet **Gestionnaires de connexions** .  
  
 **Conteneur**  
 Indique l'objet qui contient le gestionnaire de connexions.  
  
 **Nom**  
 Indique le nom du gestionnaire de connexions.  
  
 **Nom de la propriété**  
 Indique le nom de la propriété du gestionnaire de connexions.  
  
 **Valeur**  
 Indique la valeur affectée à la propriété du gestionnaire de connexions.  
  
  
