---
description: 'Présentation : Importation de données depuis Excel (Complément Master Data Services pour Excel)'
title: Importation de données à partir d’Excel
ms.custom: microsoft-excel-add-in
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
ms.assetid: ea84a9aa-aeec-411b-ab8d-bc1b14f864a3
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: df78497bc65d383a3dc44225971ab1df742f32b5
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/17/2020
ms.locfileid: "92257787"
---
# <a name="overview-importing-data-from-excel-mds-add-in-for-excel"></a>Présentation : Importation de données depuis Excel (Complément Master Data Services pour Excel)

[!INCLUDE [SQL Server Windows Only - ASDBMI ](../../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  Dans le [!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)][!INCLUDE[ssMDSXLS](../../includes/ssmdsxls-md.md)], publiez les données dans le référentiel MDS lorsque vous souhaitez les partager avec d’autres utilisateurs. Dès que les données sont publiées, elles peuvent être téléchargées par les autres utilisateurs du complément.  
  
 Quand vous publiez des données, toutes celles que vous avez ajoutées ou mises à jour sont publiées dans le référentiel MDS. Les données que vous avez supprimées ne sont pas publiées, vous devez les supprimer séparément. Pour plus d’informations, consultez [Supprimer une ligne &#40;Complément MDS pour Excel&#41;](../../master-data-services/microsoft-excel-add-in/delete-a-row-mds-add-in-for-excel.md).  
  
> [!NOTE]  
>  La publication ne peut pas être utilisée pour créer une entité. Pour plus d’informations sur la création d’entités, consultez [Créer une entité &#40;Complément MDS pour Excel&#41;](../../master-data-services/microsoft-excel-add-in/create-an-entity-mds-add-in-for-excel.md).  
  
## <a name="when-multiple-users-publish-at-the-same-time"></a>Lorsque plusieurs utilisateurs publient en même temps  
 Plusieurs utilisateurs peuvent publier des mises à jour sur les mêmes données. Lorsqu'un utilisateur publie, la mise à jour est enregistrée dans la base de données. Cela signifie qu'un utilisateur qui ne travaille pas sur des données récemment mises à jour peut toujours modifier leurs valeurs lorsqu'il les publie.  
  
 Seules les modifications que vous apportez sont publiées dans la base de données. Aucune donnée obsolète dans la feuille de calcul n'est publiée.  
  
## <a name="transactions-and-annotations"></a>Transactions et annotations  
 Chaque modification publiée est enregistrée comme une transaction. Si vous le souhaitez, vous pouvez ajouter des annotations (commentaires) à une transaction, pour expliquer pourquoi vous avez effectué la modification.  
  
-   Vous ne pouvez pas annoter les suppressions, bien que celles-ci soient enregistrées en tant que transactions qui peuvent être inversées par un administrateur.  
  
-   Si vous modifiez la valeur **Code** d’un membre, toutes les transactions précédentes de ce membre seront indisponibles. En redéfinissant **Code** sur sa valeur d’origine, vous pourrez accéder aux transactions précédentes.  
  
-   Vous pouvez afficher les transactions effectuées sur un membre par d'autres utilisateurs. Vous pouvez également afficher toutes les transactions que vous avez effectuées sur un membre, même si vous n’avez plus l’autorisation d’accès aux attributs spécifiques. Vous ne pouvez pas visualiser les transactions qui impliquent des attributs dans lesquels votre autorisation est définie sur Refuser.  
  
 Vous pouvez afficher toutes les transactions effectuées sur un membre. Pour plus d’informations, consultez [Afficher toutes les annotations ou transactions pour un membre &#40;Complément MDS pour Excel&#41;](../../master-data-services/microsoft-excel-add-in/view-all-annotations-or-transactions-for-a-member-mds-add-in-for-excel.md).  
  
> [!IMPORTANT]  
>  Si vous entrez une annotation de plus de 500 caractères, elle sera automatiquement tronquée.  
  
## <a name="business-rule-and-other-validation"></a>Règle d'entreprise et autres validations  
 Quand vous publiez des données, une validation est exécutée pour garantir que les données sont exactes avant d’être ajoutées au référentiel MDS. Si les données ne répondent pas aux critères spécifiés, elles ne seront pas publiées dans le référentiel. Pour plus d’informations, consultez [Validation des données &#40;Complément MDS pour Excel&#41;](../../master-data-services/microsoft-excel-add-in/validating-data-mds-add-in-for-excel.md).  
  
## <a name="related-tasks"></a>Tâches associées  
  
|Description de la tâche|Rubrique|  
|----------------------|-----------|  
|Publiez des données à partir de la feuille de calcul active vers le référentiel MDS.|[Publier des données d’Excel dans MDS &#40;Complément MDS pour Excel&#41;](../../master-data-services/microsoft-excel-add-in/import-data-from-excel-to-master-data-services-mds-add-in-for-excel.md)|  
|Supprimez une ligne du référentiel MDS et de la feuille de calcul en même temps.|[Supprimer une ligne &#40;Complément MDS pour Excel&#41;](../../master-data-services/microsoft-excel-add-in/delete-a-row-mds-add-in-for-excel.md)|  
|Visualisez les annotations (commentaires) et les transactions des lignes de données (membres) lorsque vous souhaitez consulter les mises à jour des données dans le temps.|[Afficher toutes les annotations ou transactions pour un membre &#40;Complément MDS pour Excel&#41;](../../master-data-services/microsoft-excel-add-in/view-all-annotations-or-transactions-for-a-member-mds-add-in-for-excel.md)|  
|Combinez les données de deux feuilles de calcul lorsque vous souhaitez comparer les données avant la publication.|[Combiner des données &#40;Complément MDS pour Excel&#41;](../../master-data-services/microsoft-excel-add-in/combine-data-mds-add-in-for-excel.md)|  

  
## <a name="related-content"></a>Contenu associé  
  
-   [Actualisation des données &#40;Complément MDS pour Excel&#41;](../../master-data-services/microsoft-excel-add-in/refreshing-data-mds-add-in-for-excel.md)  
  
-   [Complément Master Data Services pour Microsoft Excel](../../master-data-services/microsoft-excel-add-in/master-data-services-add-in-for-microsoft-excel.md)  
  
  
