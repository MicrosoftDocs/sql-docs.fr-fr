---
description: RDS retourne une &quot; erreur d’Inlecture du flux &quot;
title: RDS retourne une &quot; erreur de lecture de flux non lu &quot; | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 11/09/2018
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- stream not read error in RDS [ADO]
ms.assetid: cb5a68f8-dba4-41da-bafd-04efe53706b7
author: rothja
ms.author: jroth
ms.openlocfilehash: 23de3604b57f2b424a2166cc25f79bcb4ccbde09
ms.sourcegitcommit: 18a98ea6a30d448aa6195e10ea2413be7e837e94
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/27/2020
ms.locfileid: "88977900"
---
# <a name="rds-returns-quotstream-not-readquot-error"></a>RDS retourne une &quot; erreur d’Inlecture du flux &quot;
«L’objet de flux n’a pas pu être lu, car il est vide ou la position actuelle se trouve à la fin du flux. Pour les flux non vides, définissez la position actuelle avec la propriété position. Pour déterminer si un flux est vide, vérifiez la propriété Size.  
  
 Si vous voyez ce message d’erreur, vous avez peut-être tenté d’utiliser une requête hiérarchique paramétrée sur http. Les services Bureau à distance ne vous permettent pas d’utiliser des hiérarchies paramétrées à distance.  
  
> [!IMPORTANT]
>  À compter de Windows 8 et de Windows Server 2012, les composants serveur RDS ne sont plus inclus dans le système d’exploitation Windows (pour plus d’informations, consultez le livre de recettes sur la compatibilité avec Windows 8 et [Windows server 2012](https://www.microsoft.com/download/details.aspx?id=27416) ). Les composants clients RDS seront supprimés dans une prochaine version de Windows. Évitez d'utiliser cette fonctionnalité dans de nouveaux travaux de développement, et prévoyez de modifier les applications qui utilisent actuellement cette fonctionnalité. Les applications qui utilisent RDS doivent migrer vers le [service de données WCF](https://go.microsoft.com/fwlink/?LinkId=199565).  
  
## <a name="see-also"></a>Voir aussi  
 [Concepts de base de RDS](./rds-fundamentals.md)