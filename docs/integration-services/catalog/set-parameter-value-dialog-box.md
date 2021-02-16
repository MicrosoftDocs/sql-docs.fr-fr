---
description: Boîte de dialogue Définir la valeur du paramètre
title: Boîte de dialogue Définir la valeur du paramètre | Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
ms.assetid: ce9c2201-4e9a-4495-948f-b68deeaa7955
author: chugugrace
ms.author: chugu
ms.openlocfilehash: f462d8b1f94b201a004272d65d08fc4902e33492
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100350969"
---
# <a name="set-parameter-value-dialog-box"></a>Boîte de dialogue Définir la valeur du paramètre

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  Utilisez la boîte de dialogue **Définir la valeur du paramètre** pour définir les valeurs des paramètres et les propriétés des gestionnaires de connexions, pour les projets et packages.  
  
 **Que voulez-vous faire ?**  
  
-   [Ouvrir la boîte de dialogue Définir la valeur du paramètre](#open_dialog)  
  
-   [Configurer les options](#option)  
  
##  <a name="open-the-set-parameter-value-dialog-box"></a><a name="open_dialog"></a> Ouvrir la boîte de dialogue Définir la valeur du paramètre  
  
1.  Dans [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], connectez-vous au serveur [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] .  
  
     Vous vous connectez à l’instance du [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] qui héberge la base de données SSISDB.  
  
2.  Dans l'Explorateur d'objets, développez l'arborescence pour afficher le nœud **Integration Services Catalogues** .  
  
3.  Développez le nœud **SSISDB** .  
  
4.  Cliquez avec le bouton droit sur un package ou un projet, cliquez sur **Configurer**, puis cliquez sur le bouton de sélection sous l’onglet **Paramètres** ou l’onglet **Gestionnaires de connexions** .  
  
##  <a name="configure-the-options"></a><a name="option"></a> Configurer les options  
 **Paramètre**  
 Indique le nom du paramètre.  
  
 **Type**  
 Indique le type de données de la valeur de paramètre.  
  
 **Description**  
 Affiche une description facultative du paramètre.  
  
 **Modifier la valeur**  
 Sélectionnez cette option pour modifier la valeur du paramètre.  
  
 **Utiliser la valeur par défaut du package**  
 Sélectionnez cette option afin d'utiliser le paramètre par défaut stocké dans le package.  
  
 **Utiliser la variable d'environnement**  
 Sélectionnez cette option pour utiliser une valeur de variable stockée dans l'environnement, qui est référencé par le projet ou le package. Pour ajouter une référence d'environnement à un projet ou package, utilisez la boîte de dialogue **Configurer** . Pour plus d'informations, consultez [Configure Dialog Box](../../integration-services/catalog/configure-dialog-box.md).  
  
  
