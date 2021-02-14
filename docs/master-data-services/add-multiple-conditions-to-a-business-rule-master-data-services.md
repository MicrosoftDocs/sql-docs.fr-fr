---
description: Ajouter plusieurs conditions à une règle d'entreprise (Master Data Services)
title: Ajouter des conditions à une règle d’entreprise
ms.custom: seo-lt-2019
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
helpviewer_keywords:
- business rules [Master Data Services], multiple conditions
ms.assetid: 5f0f6958-6cf2-444b-bdcd-05b887637a0b
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: e5529b10ef0a293a19c45eb8a0e904fcf1fc0f42
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100339694"
---
# <a name="add-multiple-conditions-to-a-business-rule-master-data-services"></a>Ajouter plusieurs conditions à une règle d'entreprise (Master Data Services)

[!INCLUDE [SQL Server - Windows only ASDBMI  ](../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  Dans [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)], ajoutez plusieurs conditions **AND** ou **OR** à une règle d'entreprise lorsque vous souhaitez une règle plus complexe.  
  
> [!NOTE]  
>  Si vous créez une règle d'entreprise qui utilise l'opérateur **OR** , créez une règle distincte pour chaque instruction conditionnelle qui peut être évaluée indépendamment. Vous pouvez alors exclure des règles si nécessaire, ce qui offre plus de souplesse et facilite la résolution des problèmes.  
  
## <a name="prerequisites"></a>Prérequis  
 Pour effectuer cette procédure :  
  
-   Vous devez avoir l'autorisation d'accéder à la zone fonctionnelle **Administration de système** .  
  
-   Vous devez être administrateur de modèle. Pour plus d’informations, consultez [administrateurs &#40;Master Data Services&#41;](../master-data-services/administrators-master-data-services.md).  
  
-   Une règle d'entreprise doit exister. Pour plus d’informations, consultez [Créer et publier une règle d’entreprise &#40;Master Data Services&#41;](../master-data-services/create-and-publish-a-business-rule-master-data-services.md).  
  
### <a name="to-add-multiple-conditions-to-a-business-rule"></a>Pour ajouter plusieurs conditions à une règle d'entreprise  
  
1.  Dans [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)], cliquez sur **Administration de système**.  
  
2.  Dans la barre de menus, pointez sur **Gérer** et cliquez sur **Règles d'entreprise**.  
  
3.  Dans la page **Règles d’entreprise/** , sélectionnez un modèle dans la liste déroulante **Modèle** .  
  
4.  Dans la liste déroulante **Entité** , sélectionnez une entité.  
  
5.  Dans la liste déroulante **Types de membres** , sélectionnez un type de membre.  
  
6.  Cliquez sur la ligne de la règle d'entreprise à modifier.  
  
7.  Cliquez sur **Modifier**.  
  
8.  Sous le bloc **If** à gauche de la liste déroulante, sélectionnez l’opérateur logique **AND/OR/NOT** dans la liste.  
  
9. Cliquez sur **Add**. Un panneau s’affiche.  
  
10. Dans la liste déroulante **Attribut** , sélectionnez un attribut.  
  
11. Dans la liste déroulante **Opérateur** , sélectionnez une condition.  
  
12. Complétez tous les champs obligatoires.  
  
13. Cliquez sur **Save**. Une nouvelle ligne est ajoutée à la grille **If** .  
  
14. Si vous le souhaitez, suivez les étapes 8 à 13 pour ajouter d’autres conditions.  
  
    > [!TIP]  
    >  Pour supprimer une condition, sélectionnez-la, cliquez dessus avec le bouton droit, puis cliquez sur **Supprimer**.  
  
    > [!TIP]  
    >  Vous pouvez sélectionner plusieurs conditions et, à l’aide d’un clic droit, les regrouper dans un opérateur logique ou les dissocier à l’intérieur d’un opérateur logique spécifique.  
  
## <a name="see-also"></a>Voir aussi  
 [&#40;des règles d’entreprise Master Data Services&#41;](../master-data-services/business-rules-master-data-services.md)   
 [Modifier le nom d’une règle d’entreprise &#40;Master Data Services&#41;](../master-data-services/change-a-business-rule-name-master-data-services.md)   
 [Configurer des règles d’entreprise pour envoyer des notifications &#40;Master Data Services&#41;](../master-data-services/configure-business-rules-to-send-notifications-master-data-services.md)  
  
  
