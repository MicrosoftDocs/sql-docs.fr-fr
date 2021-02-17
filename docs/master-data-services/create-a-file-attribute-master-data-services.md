---
description: Créer un attribut de fichier (Master Data Services)
title: Créer un attribut de fichier
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
helpviewer_keywords:
- creating file attributes [Master Data Services]
- attributes [Master Data Services], creating file attributes
ms.assetid: d224886b-2ef1-4658-8b01-2213cc4b8df6
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: 25023eb11f9968f9edc3e7159f32252b0a116291
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100272680"
---
# <a name="create-a-file-attribute-master-data-services"></a>Créer un attribut de fichier (Master Data Services)

[!INCLUDE [SQL Server - Windows only ASDBMI  ](../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  Dans [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)], créez un attribut de fichier pour remplir les valeurs d'attribut avec des fichiers.

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

### <a name="to-create-a-file-attribute"></a>Pour créer un attribut de fichier

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

9. Dans la liste **Type d’attribut** , sélectionnez **Fichier**.

10. Dans la liste **Extension de fichier** , sélectionnez un type de fichier téléchargeable ou acceptez la valeur par défaut (*.\*) pour autoriser tous les types de fichiers.

11. Sélectionnez éventuellement **Activer le suivi des modifications** pour effectuer le suivi des modifications apportées aux groupes d'attributs. Pour plus d’informations, consultez [Ajouter des attributs à un groupe de suivi des modifications &#40;Master Data Services&#41;](../master-data-services/add-attributes-to-a-change-tracking-group-master-data-services.md).

12. Cliquez sur **Enregistrer**.

## <a name="see-also"></a>Voir aussi
 Les [attributs &#40;Master Data Services&#41;](../master-data-services/attributes-master-data-services.md) [modifier un nom d’attribut et un Type de données &#40;Master Data Services](../master-data-services/change-an-attribute-name-and-data-type-master-data-services.md)&#41;[créer un attribut](../master-data-services/create-a-domain-based-attribute-master-data-services.md) Domain-Based &#40;Master Data Services [créer un attribut de texte&#41;](../master-data-services/create-a-text-attribute-master-data-services.md) &#40;Master Data Services


