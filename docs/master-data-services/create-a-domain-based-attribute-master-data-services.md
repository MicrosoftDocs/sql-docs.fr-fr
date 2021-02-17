---
description: Créer un attribut basé sur un domaine (Master Data Services)
title: Créer un attribut basé sur un domaine
ms.custom: ''
ms.date: 07/25/2017
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
helpviewer_keywords:
- domain-based attributes [Master Data Services], creating
- creating domain-based attributes [Master Data Services]
- attributes [Master Data Services], creating domain-based attributes
ms.assetid: 11c31c9f-e6cc-47b7-b76a-d691f84c93c6
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: ec761b64a614af99a4eddfbaa60c37d93d30bf1b
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100272690"
---
# <a name="create-a-domain-based-attribute-master-data-services"></a>Créer un attribut basé sur un domaine (Master Data Services)

[!INCLUDE [SQL Server - Windows only ASDBMI  ](../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  Dans [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)], créez un attribut basé sur un domaine pour remplir les valeurs d'un attribut avec les membres d'une entité.

## <a name="prerequisites"></a>Prérequis
 Pour effectuer cette procédure :

-   Vous devez avoir l'autorisation d'accéder à la zone fonctionnelle **Administration de système** .

-   Vous devez être administrateur de modèle. Pour plus d’informations, consultez [administrateurs &#40;Master Data Services&#41;](../master-data-services/administrators-master-data-services.md).

-   Une entité doit exister pour servir de source de valeurs d'attribut. Par exemple, vous devez créer l'entité Color avant de créer un attribut basé sur un domaine et sur l'entité Color. Pour plus d’informations, consultez [créer une entité &#40;Master Data Services&#41;](../master-data-services/create-an-entity-master-data-services.md).

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

### <a name="to-create-a-domain-based-attribute"></a>Pour créer un attribut basé sur un domaine

1.  Dans [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)], cliquez sur **Administration de système**.

2.  Sur la page **Gérer les modèles** , sélectionnez un modèle, puis cliquez sur **Entités**.

3.  Sur la page **Gérer les entités** , sélectionnez la ligne de l’entité pour laquelle vous souhaitez créer un attribut.

4.  Cliquez sur **Attributs**.

5.  Dans la page **Gérer les attributs** , effectuez l’une des opérations suivantes, puis cliquez sur **Ajouter**.

    -   Si l’attribut concerne les membres feuille, sélectionnez **Feuille** dans la zone de liste **Types de membre** .

    -   Si l’attribut concerne les membres consolidés, sélectionnez **Consolidé** dans la zone de liste **Types de membre** .

    -   Si l’attribut concerne les collections, sélectionnez **Collection** dans la zone de liste **Types de membre** .

6.  Dans la zone **Nom** , tapez un nom pour l'attribut. Pour obtenir la liste des mots qui ne doivent pas être utilisés comme noms d’attribut, consultez [Mots réservés &#40;Master Data Services&#41;](../master-data-services/reserved-words-master-data-services.md).

7.  Si vous le souhaitez, entrez le nom complet et une description dans le champ **Description** .

8.  Dans la zone **Largeur d'affichage en pixels** , tapez la largeur de la colonne d'attribut à afficher dans la grille **Explorateur** .

9. Dans la liste **Type d’attribut** , sélectionnez **Basé sur un domaine**.

10. Dans la liste **Entité de domaine** , sélectionnez l’entité à utiliser pour renseigner les valeurs d’attribut. 

11. **Facultatif, pour les attributs basés sur un domaine des membres feuille.** Sélectionnez un attribut parent de filtre utilisé pour limiter les valeurs autorisées pour l’attribut basé sur un domaine.

     L’attribut parent du filtre doit être un autre attribut basé sur un domaine d’un membre feuille attaché à la même entité. Ceci suppose d’instaurer une hiérarchie dérivée contenant un niveau qui définit la relation parent-enfant entre les entités de domaine des deux attributs.

     Pour plus d’informations sur la restriction des valeurs autorisées, consultez le billet [How to filter Domain Based Attribute drop down lists](https://blogs.msdn.microsoft.com/mds/2015/12/03/in-sql-server-2016-master-data-services-how-to-filter-domain-based-attribute-drop-down-lists/)(Filtrage des listes déroulantes d’attributs basés sur un domaine) sur le blog Master Data Services.

12. **Facultatif.** Sélectionnez **Activer le suivi des modifications** pour effectuer le suivi des modifications apportées aux groupes d'attributs. Pour plus d’informations, consultez [Ajouter des attributs à un groupe de suivi des modifications &#40;Master Data Services&#41;](../master-data-services/add-attributes-to-a-change-tracking-group-master-data-services.md).

13. Cliquez sur **Enregistrer**.

## <a name="see-also"></a>Voir aussi
 [Les attributs basés sur un domaine &#40;Master Data Services&#41;](../master-data-services/domain-based-attributes-master-data-services.md) [créer une hiérarchie dérivée &#40;Master Data Services](../master-data-services/create-a-derived-hierarchy-master-data-services.md)&#41;[modifier un nom d’attribut et un type de données &#40;](../master-data-services/change-an-attribute-name-and-data-type-master-data-services.md) Master Data Services [supprimer un attribut](../master-data-services/delete-an-attribute-master-data-services.md)&#41;&#40;Master Data Services


