---
title: Paramètres globaux (boîtes de dialogue) (SybaseToSQL) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: e11452b7-ba94-4367-a745-5ccf1764acec
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: 4dc7bf9bf6b8f2e20fb8254a3a27ca2e293a0174
ms.sourcegitcommit: e8f6c51d4702c0046aec1394109bc0503ca182f0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87931460"
---
# <a name="global-settings-dialogs--sybasetosql"></a>Paramètres globaux (boîtes de dialogue) (SybaseToSQL)
Utilisez la page boîtes de dialogue de la boîte de dialogue **paramètres globaux** pour spécifier les paramètres d’action et d’avertissement de l’utilisateur par défaut pour SSMA.  
  
Pour accéder aux paramètres de la boîte de dialogue dans le menu **Outils** , sélectionnez **paramètres globaux**, cliquez sur **GUI** en bas du volet gauche, puis sélectionnez **boîtes de dialogue**.  
  
## <a name="options"></a>Options  
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
  
