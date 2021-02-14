---
description: Supprimer une version (Master Data Services)
title: Supprimer une version
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
helpviewer_keywords:
- versions [Master Data Services], deleting
- deleting versions [Master Data Services]
ms.assetid: 2a4eeffe-8379-4744-ad44-c27d8c8ac9a8
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: 5a7b08744e0b1ed8a80859cf81e02079f0e454ac
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100338970"
---
# <a name="delete-a-version-master-data-services"></a>Supprimer une version (Master Data Services)

[!INCLUDE [SQL Server - Windows only ASDBMI  ](../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  Dans [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)], supprimez une version lorsque vous êtes sûr de ne plus avoir besoin des données de référence qui lui sont associées. Après avoir supprimé une version, vous ne pouvez pas récupérer les données de référence associées.  
  
> [!WARNING]  
>  Si un modèle a une seule version et que vous la supprimez, le modèle devient inutilisable.  
  
## <a name="prerequisites"></a>Prérequis  
 Pour effectuer cette procédure :  
  
-   Vous devez avoir l'autorisation d'afficher la vue mdm.viw_SYSTEM_SCHEMA_VERSION et d'exécuter la procédure stockée mds.udpVersionDelete dans la base de données [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] . Pour plus d’informations, consultez [Sécurité de l’objet de base de données &#40;Master Data Services&#41;](../master-data-services/database-object-security-master-data-services.md).  
  
### <a name="to-delete-a-version"></a>Pour supprimer une version  
  
1.  Ouvrez [!INCLUDE[ssManStudioFull](../includes/ssmanstudiofull-md.md)] et connectez-vous à l’instance [!INCLUDE[ssDE](../includes/ssde-md.md)] pour votre base de données [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] .  
  
2.  Ouvrez la vue mdm.viw_SYSTEM_SCHEMA_VERSION.  
  
3.  Recherchez la version du modèle que vous souhaitez supprimer et copiez la valeur dans le champ **ID** .  
  
4.  Créez une requête.  
  
5.  Tapez le texte suivant, en remplaçant *ID_version* par la valeur copiée à l’étape 2.  
  
    ```  
    EXEC [mdm].[udpVersionDelete] @Version_ID='version_ID'  
    ```  
  
6.  Exécuter la requête  
  
    > [!NOTE]  
    >  Vous devrez peut-être attendre quelques minutes avant que l'application Web ne reflète la modification.  
  
## <a name="see-also"></a>Voir aussi  
 [Versions &#40;Master Data Services&#41;](../master-data-services/versions-master-data-services.md)   
 [Copier une version &#40;Master Data Services&#41;](../master-data-services/copy-a-version-master-data-services.md)  
  
  
