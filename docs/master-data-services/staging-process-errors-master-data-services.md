---
title: Erreurs du processus de site
description: Cet article explique les codes d’erreur ajoutés à tous les enregistrements traités au cours du processus de site dans Master Data Services.
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
helpviewer_keywords:
- staging process [Master Data Services], error messages
ms.assetid: 0d9be0dd-638f-4dd4-92b2-253fda655455
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: 84a5a66cbb8bacb3e1df896fedfa3a051e2f4dbb
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100336267"
---
# <a name="staging-process-errors-master-data-services"></a>Erreurs du processus de site (Master Data Services)

[!INCLUDE [SQL Server - Windows only ASDBMI  ](../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  Lorsque le processus de mise en lots est terminé, tous les enregistrements traités dans les tables intermédiaires ont une valeur dans la colonne ErrorCode. Ces valeurs sont répertoriées dans le tableau suivant.  
  
|Code|Error|Se produit lorsque/détails|S'applique à la table|  
|----------|-----------|--------------------------|----------------------|  
|210001|Le même code de membre existe plusieurs fois dans la table de mise en lots.|Votre lot intermédiaire comporte plusieurs occurrences du même code de membre. Aucun membre n'est créé ou mis à jour.|Feuille<br /><br /> Consolidé<br /><br /> Relationship|  
|210003|La valeur d'attribut fait référence à un membre qui n'existe pas ou qui est inactif.|Lorsque vous mettez en lots des attributs basés sur un domaine, vous devez utiliser le code, au lieu du nom. S’applique à **ImportType0**, **1** et **2**.|Feuille<br /><br /> Consolidé|  
|210006|Le code du membre est inactif.|**ImportType**  =  **1** et vous avez spécifié un code de membre qui n’existe pas.|Feuille<br /><br /> Consolidé<br /><br /> Relationship|  
|210032|Le nom de hiérarchie est manquant ou n'est pas valide.|La hiérarchie explicite est introuvable ou la valeur de **HierarchyName** est vide.|Consolidé<br /><br /> Relationship|  
|210035|Comme il n’existe pas de règle d’entreprise de génération de code, le **MemberCode** est obligatoire.|Lors de la création ou de la mise à jour des membres, un **MemberCode** est toujours obligatoire, à moins d’utiliser la génération de code automatique. Pour plus d’informations, consultez [création automatique de Code &#40;Master Data Services&#41;](../master-data-services/automatic-code-creation-master-data-services.md).|Feuille<br /><br /> Consolidé|  
|210036|Comme il existe une règle d’entreprise de génération de code, le **MemberCode** n’est pas obligatoire.|Lors de la création ou de la mise à jour des membres, un **MemberCode** n’est pas obligatoire quand vous utilisez la génération de code automatique. Toutefois, vous pouvez spécifier le code si vous le choisissez. Pour plus d’informations, consultez [création automatique de Code &#40;Master Data Services&#41;](../master-data-services/automatic-code-creation-master-data-services.md).|Feuille<br /><br /> Consolidé|  
|210041|« ROOT » n’est pas un code de membre valide.|La valeur **MemberCode** contient le mot « root ».|Feuille<br /><br /> Consolidé<br /><br /> Relationship|  
|210042|« MDMUNUSED » n’est pas un code de membre valide.|La valeur **MemberCode** contient le mot « MDMUNUSED ».|Feuille<br /><br /> Consolidé<br /><br /> Relationship|  
|210052|MemberCode ne peut pas être désactivé, car il est utilisé comme valeur d'attribut basée sur un domaine.|Lorsque **ImportType**  =  **3** ou **4**, la mise en lots échoue si le membre est utilisé comme valeur d’attribut pour d’autres membres. Utilisez **ImportType5** ou **6** pour définir la valeur sur NULL ou modifiez les valeurs avant d’exécuter le processus de mise en lots.|Feuille<br /><br /> Consolidé|  
|300002|Le code de membre n'est pas valide.|Relations - Le code de membre parent ou enfant n'existe pas.<br /><br /> Feuille ou consolidé : **ImportType**  =  **3** ou **4** et le code de membre n’existe pas.|Feuille<br /><br /> Consolidé<br /><br /> Relationship|  
|300004|Le code de membre existe déjà.|**ImportType**  =  **1** et vous avez utilisé un code de membre qui existe déjà dans l’entité.|Feuille<br /><br /> Consolidé|  
|210011|Lorsque **RelationshipType** a la valeur **1**, **ParentCode** ne peut pas être un membre feuille.|Vérifiez que la valeur **ParentCode** est un code de membre consolidé.|Relationship|  
|210015|Le code de membre est présent plusieurs fois dans la table de mise en lots pour une hiérarchie et un lot.|Pour une hiérarchie explicite, vous avez spécifié l'emplacement du même membre plusieurs fois dans le même lot.|Relationship|  
|210016|Impossible de créer la relation car cela provoquerait une référence circulaire.|Ceci se produit lorsque vous essayez d'affecter un enfant en tant que parent.|Relationship|  
|210046|Le membre ne peut pas être un frère de Racine.|Cela se produit lorsque **RelationshipType**  =  **2** (frère) et **ParentCode** ou **ChildCode** sont **racines**. Les membres ne peuvent pas être au même niveau que le nœud racine ; ils peuvent uniquement être des enfants.|Relationship|  
|210047|Le membre ne peut pas être un frère d'Inutilisé.|Cela se produit lorsque **RelationshipType**  =  **2** (frère) et **ParentCode** ou **ChildCode** sont **inutilisés**. Les membres peuvent uniquement être des enfants du nœud Inutilisé.|Relationship|  
|210048|**ParentCode** et **ChildCode** ne peuvent pas être identiques.|La valeur **ParentCode** est identique à la valeur **ChildCode** . Ces valeurs doivent être différentes.|Relationship|  
  
## <a name="see-also"></a>Voir aussi  
 [Afficher les erreurs qui se produisent lors de la &#40;de la mise en lots Master Data Services&#41;](../master-data-services/view-errors-that-occur-during-staging-master-data-services.md)   
 [Présentation : Importation de données à partir de tables &#40;Master Data Services&#41;](../master-data-services/overview-importing-data-from-tables-master-data-services.md)  
  
  
