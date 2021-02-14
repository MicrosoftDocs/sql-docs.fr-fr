---
description: Création automatique de code (Master Data Services)
title: Création automatique de code
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
ms.assetid: 9adbd5e1-f28c-4fb5-afa7-082de2831f3e
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: ccd738b1ff39138db4d3e6017a041a383aa72511
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100339521"
---
# <a name="automatic-code-creation-master-data-services"></a>Création automatique de code (Master Data Services)

[!INCLUDE [SQL Server - Windows only ASDBMI  ](../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  Dans [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)], vous pouvez générer automatiquement des valeurs numériques pour l'attribut Code, ou pour toute autre attribut numérique. Lorsque vous générez les codes automatiquement, rien n'empêche de saisir d'autres valeurs pour les codes ; sinon, une valeur initiale est définie automatiquement.  
  
## <a name="generating-code-values"></a>Génération de valeurs de code  
 Les administrateurs peuvent configurer des valeurs générées automatiquement pour l’attribut Code en modifiant les propriétés de l’entité associée. Ils peuvent spécifier une valeur initiale, et chaque valeur suivante est augmentée d'une unité.  
  
 Lorsque vous entrez des valeurs Code dans MDS, dans l'un des outils ou à l'aide du processus de site, vous pouvez laisser la valeur Code vide pour qu'elle soit générée automatiquement. Ou vous pouvez spécifier une valeur de code de votre choix.  
  
## <a name="generating-other-attribute-values"></a>Génération d'autres valeurs d'attribut  
 Les administrateurs peuvent générer automatiquement des valeurs pour les attributs autres que le code en créant des règles d'entreprise. Ils peuvent spécifier une valeur initiale, puis le nombre avec lequel chaque valeur suivante est incrémentée.  
  
 Lorsque vous entrez des valeurs d'attribut dans MDS, dans l'un des outils ou à l'aide du processus de site, vous pouvez laisser les valeurs d'attribut vides. Lorsque les règles d'entreprise sont appliquées, les valeurs seront incrémentées en fonction de la valeur existante la plus élevée. Par exemple, si votre règle est « Attribut par défaut avec une valeur générée qui commence à 1 et est incrémentée par 4 » et que la valeur courante la plus élevée pour l’attribut est 700, la valeur du membre suivant ajouté sera 704.  
  
## <a name="related-tasks"></a>Tâches associées  
  
|Description de la tâche|Rubrique|  
|----------------------|-----------|  
|Générez automatiquement les valeurs de l'attribut code.|[Générer automatiquement les valeurs de l’attribut Code &#40;Master Data Services&#41;](../master-data-services/automatically-generate-code-attribute-values-master-data-services.md)|  
|Générez automatiquement les valeurs des autres attributs.|[Générer automatiquement les valeurs des attributs autres que Code &#40;Master Data Services&#41;](../master-data-services/automatically-generate-attribute-values-other-than-code-master-data-services.md)|  
  
## <a name="related-content"></a>Contenu associé  
  
-   [Vue d’ensemble de Master Data Services &#40;MDS&#41;](../master-data-services/master-data-services-overview-mds.md)  
  
-   [Règles d’entreprise &#40;Master Data Services&#41;](../master-data-services/business-rules-master-data-services.md)  
  
-   [Entités &#40;Master Data Services&#41;](../master-data-services/entities-master-data-services.md)  
  
  
