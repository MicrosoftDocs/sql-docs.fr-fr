---
description: Purge des membres de version [Master Data Services]
title: Purge des membres de version
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
ms.assetid: adecce2d-46bb-49ff-8be9-0b31b8dd3cb6
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: 4a23a3827cafd22b23144fa8d0862e25f2afe48b
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100340232"
---
# <a name="purge-version-members-master-data-services"></a>Purge des membres de version [Master Data Services]

[!INCLUDE [SQL Server - Windows only ASDBMI  ](../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  Dans [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)], la suppression d’un membre n’a pour effet que de le désactiver (suppression réversible). Les données se trouvent toujours dans la base de données. Cette rubrique explique comment purger (supprimer définitivement) tous les membres supprimés de manière réversible dans une version de modèle.  
  
## <a name="prerequisites"></a>Prérequis  
 Pour effectuer cette procédure :  
  
-   Vous devez avoir l’autorisation d’accéder à la zone fonctionnelle Gestion des versions.  
  
-   Vous devez être administrateur de modèle. Pour plus d’informations, consultez [administrateurs &#40;Master Data Services&#41;](../master-data-services/administrators-master-data-services.md).  
  
## <a name="to-purge-soft-deleted-members"></a>Pour purger les membres supprimés de manière réversible  
  
1.  Dans [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)], cliquez sur **Gestion des versions**.  
  
2.  Sur la page **Gérer les versions** , sélectionnez le modèle associé à la version que vous souhaitez purger. La liste des versions de modèle s’affiche.  
  
3.  Sélectionnez la ligne de la version à purger.  
  
4.  Cliquez sur **Purger les membres**.  
  
5.  À l’invite de confirmation, cliquez sur « OK ».  
  
## <a name="additional-methods-to-purge-members"></a>Autres méthodes de purge  
 Une purge des membres de version a pour effet de supprimer définitivement les membres qui ont été supprimés de manière réversible dans toutes les entités appartenant à la version sélectionnée. Une autre solution consiste à utiliser une mise en lots de la base d’entités pour supprimer définitivement les membres spécifiques d’une entité uniquement. En outre, les administrateurs d’entités ayant une autorisation de fonction Explorateur peuvent purger une version de l’entité sur la page de l’explorateur d’entités.  
  
 Pour plus d’informations, consultez [Table de mise en lots des membres feuilles &#40;Master Data Services&#41;](../master-data-services/leaf-member-staging-table-master-data-services.md).  
  
  
