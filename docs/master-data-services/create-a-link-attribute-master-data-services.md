---
description: Créer un attribut de lien (Master Data Services)
title: Créer un attribut de lien
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
helpviewer_keywords:
- attributes [Master Data Services], creating link attributes
- creating link attributes [Master Data Services]
ms.assetid: e6658e9c-5b08-4b8d-b556-17ec2dd041d2
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: 621aa0093affb93167703655686b9ee1ec631d53
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100272660"
---
# <a name="create-a-link-attribute-master-data-services"></a>Créer un attribut de lien (Master Data Services)

[!INCLUDE [SQL Server - Windows only ASDBMI  ](../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  Dans [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)], créez un attribut de lien quand vous souhaitez que les utilisateurs entrent un lien hypertexte comme valeur d’attribut, par exemple, `https://www.contoso.com`.  
  
> [!NOTE]  
>  Quand les utilisateurs entrent une valeur pour un attribut de lien, la chaîne doit commencer par **https://**, sinon une erreur est affichée.  
  
## <a name="prerequisites"></a>Prérequis  
 Pour effectuer cette procédure :  
  
-   Vous devez avoir l'autorisation d'accéder à la zone fonctionnelle **Administration de système** .  
  
-   Vous devez être administrateur de modèle. Pour plus d’informations, consultez [administrateurs &#40;Master Data Services&#41;](../master-data-services/administrators-master-data-services.md).  
  
-   Une entité doit exister pour créer l'attribut qui lui est destiné. Pour plus d’informations, consultez [créer une entité &#40;Master Data Services&#41;](../master-data-services/create-an-entity-master-data-services.md).  
  
## <a name="attribute-information"></a>Informations sur les attributs  
 Pour chaque attribut créé, une ligne comportant sept colonnes est ajoutée à la grille. Le tableau suivant décrit ces colonnes.  
  
|Colonne|Description|  
|------------|-----------------|  
|Statut|État de l’attribut.<br /><br /> Lorsque vous cliquez sur enregistrer, l’image ![icône d’état de mise à jour](../master-data-services/media/mds-statusicon-updating.png "Icône de mise à jour de l’État") s’affiche, indiquant que l’attribut est en état de mise à jour.<br /><br /> Si des erreurs se produisent lors de la création ou de la modification d’un attribut, l’image ![icône d’état d’erreur](../master-data-services/media/mds-statusicon-error.png "Icône d’état d’erreur") s’affiche.<br /><br /> Dans le cas contraire, l’État est OK et l’image ![icône d’état OK](../master-data-services/media/mds-statusicon-ok.png "Icône d’état OK") s’affiche.|  
|Name|Nom de l'attribut.|  
|Nom d’affichage|Nom de l’attribut.|  
|Description|Description de l’attribut.|  
|Largeur d’affichage en pixels|Largeur de l’attribut.|  
|Types et propriétés|Informations sur le type et le type de données de l’attribut.|  
|Activer le suivi des modifications|Spécifie si le suivi des modifications est activé sur l’attribut et indique le numéro du groupe entre parenthèses.|  
  
 Quand vous cliquez sur un attribut, les informations suivantes s’affichent.  
  
-   **Créé par**: nom de l’utilisateur qui a créé l’attribut.  
  
-   **Le**: date et heure de création de l’attribut.  
  
-   **Mis à jour par**: nom du dernier utilisateur qui a mis à jour l’attribut.  
  
-   **Le**: date et heure de la dernière mise à jour de l’attribut.  
  
### <a name="to-create-a-link-attribute"></a>Pour créer un attribut de lien  
  
1.  Dans [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)], cliquez sur **Administration de système**.  
  
2.  Sur la page **gérer le modèle** , sélectionnez un modèle dans la grille, puis cliquez sur **entités**.  
  
3.  Dans la page **Gérer l’entité** , sélectionnez la ligne de l’entité pour laquelle vous souhaitez créer un attribut.  
  
4.  Cliquez sur **Attributs**.  
  
5.  Dans la page **Gérer les attributs** , effectuez l’une des opérations suivantes, puis cliquez sur **Ajouter**.  
  
    -   Si l’attribut concerne les membres feuille, sélectionnez **Feuille** dans la zone de liste **Types de membre** .  
  
    -   Si l’attribut concerne les membres consolidés, sélectionnez **Consolidé** dans la zone de liste **Types de membre** .  
  
    -   Si l’attribut concerne les collections, sélectionnez **Collection** dans la zone de liste **Types de membre** .  
  
6.  Dans la zone **Nom** , tapez un nom pour l'attribut. Pour obtenir la liste des mots qui ne doivent pas être utilisés comme noms d’attribut, consultez la section [Mots réservés &#40;Master Data Services&#41;](../master-data-services/reserved-words-master-data-services.md).  
  
7.  Si vous le souhaitez, tapez le nom complet et une description de l’attribut dans le champ **Description** .  
  
8.  Dans la zone **Largeur d'affichage en pixels** , tapez la largeur de la colonne d'attribut à afficher dans la grille **Explorateur** .  
  
9. Dans la liste **Type d’attribut** , sélectionnez **Forme libre**.  
  
10. Dans la liste **Type de données** , sélectionnez **Lien**.  
  
11. Dans la zone **Longueur** , tapez le nombre maximal de caractères autorisé.  
  
12. Sélectionnez éventuellement **Activer le suivi des modifications** pour effectuer le suivi des modifications apportées aux groupes d'attributs. Pour plus d’informations, consultez [Ajouter des attributs à un groupe de suivi des modifications &#40;Master Data Services&#41;](../master-data-services/add-attributes-to-a-change-tracking-group-master-data-services.md).  
  
13. Cliquez sur **Enregistrer**.  
  
## <a name="see-also"></a>Voir aussi  
 [Attributs &#40;Master Data Services&#41;](../master-data-services/attributes-master-data-services.md)   
 [Modifiez le nom d’un attribut et le type de données &#40;Master Data Services&#41;](../master-data-services/change-an-attribute-name-and-data-type-master-data-services.md)   
 [Créer un attribut Domain-Based &#40;Master Data Services&#41;](../master-data-services/create-a-domain-based-attribute-master-data-services.md)   
 [Créer un attribut de fichier &#40;Master Data Services&#41;](../master-data-services/create-a-file-attribute-master-data-services.md)  
  
  
