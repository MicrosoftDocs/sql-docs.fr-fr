---
title: Créer et publier une règle d'entreprise
description: Découvrez comment créer une règle d’entreprise dans Master Data Services pour garantir la précision de vos données de référence. Après avoir créé une règle, publiez-la pour appliquer la règle.
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
helpviewer_keywords:
- business rules [Master Data Services], creating
- creating business rules [Master Data Services]
ms.assetid: 6961d636-4d69-468e-81f7-8d0be6a4a039
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: fbdb07d5a9bbff92b98fca913e2fcf142a1cbe6c
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100339094"
---
# <a name="create-and-publish-a-business-rule-master-data-services"></a>Créer et publier une règle d'entreprise (Master Data Services)

[!INCLUDE [SQL Server - Windows only ASDBMI  ](../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  Dans [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)], créez une règle d'entreprise pour garantir l'exactitude de vos données de référence. Après avoir créé une règle, vous devez la publier avant de pouvoir l'appliquer aux données.  
  
## <a name="prerequisites"></a>Prérequis  
 Pour effectuer cette procédure :  
  
-   Vous devez avoir l'autorisation d'accéder à la zone fonctionnelle **Administration de système** .  
  
-   Vous devez être administrateur de modèle. Pour plus d’informations, consultez [administrateurs &#40;Master Data Services&#41;](../master-data-services/administrators-master-data-services.md).  
  
### <a name="to-create-and-publish-a-business-rule"></a>Pour créer et publier une règle d'entreprise  
  
1.  Dans [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)], cliquez sur **Administration de système**.  
  
2.  Dans la barre de menus, pointez sur **Gérer** et cliquez sur **Règles d'entreprise**.  
  
3.  Dans la page **Règle d’entreprise** , sélectionnez un modèle dans la liste déroulante **Modèle** .  
  
4.  Dans la liste déroulante **Entité** , sélectionnez une entité.  
  
5.  Dans la liste **Types de membres** , sélectionnez un type de membre auquel appliquer la règle d’entreprise.  
  
6.  Cliquez sur **Add**.  
  
7.  Dans la zone **Nom** , tapez un nom de règle d’entreprise.  
  
8.  Si vous le souhaitez, dans le champ **Description** , tapez la description de la règle d’entreprise.  
  
9. Si vous le souhaitez, sélectionnez l’option **Envoyer des notifications** puis, dans la liste déroulante, sélectionnez un utilisateur ou un groupe auquel envoyer une notification par e-mail.  
  
    > [!NOTE]  
    >  Des notifications sont envoyées uniquement pour les règles qui incluent une action de validation.  
  
10. Sous le bloc **If** , cliquez sur le bouton **Ajouter**. Un panneau s’affiche.  
  
11. Dans la liste déroulante **Attribut** , sélectionnez un attribut.  
  
12. Dans la liste déroulante **Opérateur** , sélectionnez une condition.  
  
13. Complétez tous les champs obligatoires.  
  
14. Cliquez sur le bouton **Enregistrer** . Une nouvelle ligne est ajoutée à la grille **If** .  
  
    > [!TIP]  
    >  Vous pouvez supprimer des éléments de votre règle d’entreprise en cliquant avec le bouton droit et en choisissant **Supprimer**.  
  
15. Éventuellement, ajoutez plusieurs conditions à la règle. Pour plus d’informations, consultez [Ajouter plusieurs conditions à une règle d’entreprise &#40;Master Data Services&#41;](../master-data-services/add-multiple-conditions-to-a-business-rule-master-data-services.md).  
  
16. Sous le bloc **Then** , cliquez sur le bouton **Ajouter** . Un panneau s’affiche.  
  
17. Dans la liste déroulante **Attribut** , sélectionnez un attribut.  
  
18. Dans la liste déroulante **Opérateur** , sélectionnez une action.  
  
19. Complétez tous les champs obligatoires.  
  
20. Cliquez sur **Enregistrer**. Une nouvelle ligne est ajoutée à la grille **Then** .  
  
21. Si vous souhaitez ajouter une action **Else** , procédez comme suit.  
  
    1.  Sous le bloc **Else** , cliquez sur le bouton **Ajouter**. Un panneau s’affiche.  
  
    2.  Dans la liste déroulante **Attribut** , sélectionnez un attribut.  
  
    3.  Dans la liste déroulante **Opérateur** , sélectionnez une action.  
  
    4.  Complétez tous les champs obligatoires.  
  
    5.  Cliquez sur **Enregistrer**. Une nouvelle ligne est ajoutée à la grille **Else** .  
  
22. Cliquez sur **Enregistrer**. Une nouvelle ligne est ajoutée à la grille des règles d’entreprise.  
  
23. Cliquez sur **Tout publier**.  
  
24. Dans la boîte de dialogue de confirmation, cliquez sur **OK**. La valeur de la colonne **État de la règle d’entreprise** est **Active**.  
  
## <a name="grid-columns"></a>Colonnes de la grille  
 Pour chaque règle d’entreprise créée, une ligne comportant six colonnes est ajoutée dans la grille. Les différentes colonnes sont décrites ci-après.  
  
|Name|Description|  
|----------|-----------------|  
|Statut|Lorsque vous cliquez sur **Enregistrer** , l’image ci-après s’affiche pour indiquer que la règle d’entreprise est en cours de mise à jour.<br /><br /> ![mds_BR_refresh](../master-data-services/media/mds-br-refresh.png "mds_BR_refresh")<br /><br /> En cas d’erreur lors de la création ou de la modification d’une règle d’entreprise, l’image suivante apparaît.<br /><br /> ![mds_br_error](../master-data-services/media/mds-br-error.png "mds_br_error")<br /><br /> Si l’état présente la valeur OK, l’image ci-dessous s’affiche.<br /><br /> ![mds_BR_success](../master-data-services/media/mds-br-success.png "mds_BR_success")|  
|Name|Le nom de la règle d’entreprise.|  
|Description|La description de la règle d’entreprise.|  
|État de la règle d’entreprise|L’un des états de règle d’entreprise suivants : Règle non définie, Active, Exclue, Modifications en attente, Exclusion en attente et Suppression en attente.|  
|Exclu|Indique si la règle d’entreprise est exclue.|  
|Notification|Spécifie l’utilisateur ou le groupe sélectionné auquel envoyer la notification par courrier électronique.|  
  
## <a name="next-steps"></a>Étapes suivantes  
  
-   Appliquez des règles d'entreprise aux données en suivant l'une de ces procédures :  
  
    -   [Valider des membres spécifiques par rapport aux règles d’entreprise &#40;Master Data Services&#41;](../master-data-services/validate-specific-members-against-business-rules-master-data-services.md)  
  
    -   [Valider une version par rapport aux règles d’entreprise &#40;Master Data Services&#41;](../master-data-services/validate-a-version-against-business-rules-master-data-services.md)  
  
## <a name="see-also"></a>Voir aussi  
 [Configurez des règles d’entreprise pour envoyer des notifications &#40;Master Data Services&#41;](../master-data-services/configure-business-rules-to-send-notifications-master-data-services.md)   
 [Modifier le nom d’une règle d’entreprise &#40;Master Data Services&#41;](../master-data-services/change-a-business-rule-name-master-data-services.md)   
 [Ajouter plusieurs conditions à une règle d’entreprise &#40;Master Data Services&#41;](../master-data-services/add-multiple-conditions-to-a-business-rule-master-data-services.md)  
  
  
