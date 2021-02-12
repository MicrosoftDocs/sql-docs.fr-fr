---
description: Paramètres globaux (boîtes de dialogue) (AccessToSQL)
title: Paramètres globaux (boîtes de dialogue) (AccessToSQL) | Microsoft Docs
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: 6c2204f2-d49e-49ba-9c0f-f14cf07fa561
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: 15dd9656a263640b598389de650b779704b8dc41
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100078890"
---
# <a name="global-settings-dialogs-accesstosql"></a>Paramètres globaux (boîtes de dialogue) (AccessToSQL)
Utilisez la page boîtes de dialogue de la boîte de dialogue **paramètres globaux** pour spécifier les paramètres d’action et d’avertissement de l’utilisateur par défaut pour SSMA.  
  
Pour accéder aux paramètres de la boîte de dialogue dans le menu **Outils** , sélectionnez **paramètres globaux**, cliquez sur **GUI** en bas du volet gauche, puis sélectionnez **boîtes de dialogue**.  
  
## <a name="options"></a>Options  
**Afficher l’Assistant Migration au démarrage**  
Dans SSMA pour Access, vous avez la possibilité d’activer ou de désactiver l' **Assistant Migration** au démarrage de l’application SSMA. Par défaut, cette option a la **valeur true**.  
  
-   Si l’option a la valeur **true**, la boîte de dialogue de l’Assistant Migration s’affiche initialement lorsque vous ouvrez l’application SSMA for Access.  
  
-   Si l’option a la valeur **false**, l’Assistant Migration ne s’affiche pas et vous devez y accéder manuellement à partir du menu **fichier** , si nécessaire.  
  
**Avertir avant de remplacer les objets**  
Lorsque SSMA convertit des objets en SQL Server, certains objets existent peut-être déjà dans les métadonnées de SQL Server du projet. Ces objets ont peut-être déjà été convertis, ou les objets peuvent simplement avoir le même nom dans le schéma cible que les objets que vous allez convertir.  
  
Utilisez cette option pour spécifier si SSMA doit vous demander de remplacer les définitions d’objets en double :  
  
-   Si vous sélectionnez **true**, SSMA affiche une boîte de dialogue d’avertissement lorsqu’il rencontre un objet dupliqué. Dans cette boîte de dialogue, vous pouvez choisir de remplacer des objets individuels ou tous les objets dupliqués, ou d’ignorer des objets individuels ou tous les objets dupliqués.  
  
-   Si vous sélectionnez **false**, l’option d' **action remplacer l’objet par défaut** s’affiche, dans laquelle vous spécifiez l’action par défaut.  
  
**Action par défaut de remplacement d’objet**  
Cette option apparaît si vous sélectionnez **false** pour l’option **avertir avant de remplacer les objets** .  
  
Utilisez cette option pour spécifier le comportement de remplacement de l’objet par défaut :  
  
-   Si vous sélectionnez **true**, SSMA remplacera automatiquement les objets dans les métadonnées du projet SQL Server qui ont le même nom et qui se trouvent dans le même schéma cible que l’objet à convertir.  
  
-   Si vous sélectionnez **false**, SSMA ne remplace pas les métadonnées de l’objet lors de la conversion.  
  
