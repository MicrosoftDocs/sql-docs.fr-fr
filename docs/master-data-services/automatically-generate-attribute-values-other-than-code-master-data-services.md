---
description: Générer automatiquement les valeurs des attributs autres que Code (Master Data Services)
title: Générer automatiquement les valeurs d’attribut
titleSuffix: Master Data Services
ms.custom: seo-lt-2019
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
ms.assetid: b82f6f81-6e9c-4918-9ea9-4ab5f5d11b15
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: 90302e701b8975d300e77cc7ca028c76825a79f6
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100339555"
---
# <a name="automatically-generate-attribute-values-other-than-code-master-data-services"></a>Générer automatiquement les valeurs des attributs autres que Code (Master Data Services)

[!INCLUDE [SQL Server - Windows only ASDBMI  ](../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  Dans [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)], générez automatiquement les valeurs des valeurs d’attribut d’une entité quand vous souhaitez qu’un entier soit attribué automatiquement comme valeur chaque fois que les règles d’entreprise sont appliquées.  
  
## <a name="prerequisites"></a>Prérequis  
 Pour effectuer cette procédure :  
  
-   Vous devez avoir l'autorisation d'accéder à la zone fonctionnelle **Administration de système** .  
  
-   Vous devez être administrateur de modèle. Pour plus d’informations, consultez [administrateurs &#40;Master Data Services&#41;](../master-data-services/administrators-master-data-services.md).  
  
-   Un attribut numérique doit exister. Pour plus d’informations, consultez [Créer un attribut numérique &#40;Master Data Services&#41;](../master-data-services/create-a-numeric-attribute-master-data-services.md).  
  
### <a name="to-automatically-generate-an-attribute-value"></a>Pour générer automatiquement une valeur d'attribut  
  
1.  Dans [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)], cliquez sur **Administration de système**.  
  
2.  Dans la barre de menus, pointez sur **Gérer** et cliquez sur **Règles d'entreprise**.  
  
3.  Dans la page **Maintenance des règles d’entreprise** , dans la liste **Modèle** , sélectionnez un modèle.  
  
4.  Dans la liste **Entité** , sélectionnez une entité.  
  
5.  Dans la liste **Type de membre** , sélectionnez un type de membre auquel appliquer la règle d’entreprise.  
  
6.  Dans la liste **Attribut** , conservez l’option par défaut **Tout**.  
  
7.  Cliquez sur **Ajouter une règle d’entreprise**.  
  
8.  Cliquez sur **Modifier la règle d’entreprise sélectionnée**.  
  
9. Dans le volet **Composants** , développez le nœud **Actions** .  
  
10. Dans le nœud Valeur par défaut, cliquez sur **prend par défaut une valeur générée** et faites-la glisser vers le libellé **Actions** du volet **THEN**.  
  
11. Dans le volet **Attributs** , cliquez sur l’attribut avec la valeur à générer et faites-le glisser vers le libellé **Sélectionner un attribut** du volet **Modifier l’action** .  
  
12. Tapez une valeur dans les zones **Démarrer avec** et **Incrémenter** . Si les membres existent déjà, la valeur sera définie en fonction de la valeur existante la plus élevée. Par exemple, si la valeur existante la plus élevée est 299 et que vous définissez **Incrémenter de** sur **1**, la valeur du membre suivant sera définie sur 300.  
  
13. Dans le volet **Modifier l’action** , cliquez sur **Enregistrer l’élément**.  
  
14. Cliquez sur **Précédent**.  
  
15. (Facultatif) Dans la page **Maintenance des règles d’entreprise** , sur la ligne qui contient votre règle d’entreprise, double-cliquez sur une cellule dans la colonne **Nom**, **Description** ou **Notification** pour mettre à jour la valeur.  
  
16. Cliquez sur **Publier les règles d’entreprise**.  
  
17. Dans la boîte de dialogue de confirmation, cliquez sur **OK**. L’état de la règle devient **Actif**.  
  
## <a name="next-steps"></a>Étapes suivantes  
  
-   [Valider des membres spécifiques par rapport aux règles d’entreprise &#40;Master Data Services&#41;](../master-data-services/validate-specific-members-against-business-rules-master-data-services.md)  
  
-   [Valider une version par rapport aux règles d’entreprise &#40;Master Data Services&#41;](../master-data-services/validate-a-version-against-business-rules-master-data-services.md)  
  
## <a name="see-also"></a>Voir aussi  
 [Création automatique de code &#40;Master Data Services&#41;](../master-data-services/automatic-code-creation-master-data-services.md)   
 [&#40;des règles d’entreprise Master Data Services&#41;](../master-data-services/business-rules-master-data-services.md)   
 [Validation &#40;Master Data Services&#41;](../master-data-services/validation-master-data-services.md)  
  
  
