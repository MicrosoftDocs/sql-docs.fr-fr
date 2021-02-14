---
title: Leaf Member Staging Table
description: Utilisez la table de mise en lots des membres feuille dans la base de données Master Data Services pour créer, mettre à jour, désactiver et supprimer des membres feuille.
ms.custom: ''
ms.date: 04/01/2016
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
helpviewer_keywords:
- members staging table [Master Data Services]
- database [Master Data Services], members staging table
ms.assetid: a8c953da-ec20-47dc-8656-ed5f0dfed89b
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: e38a38d14551b819db0e6ff7f8558448040a7049
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100338222"
---
# <a name="leaf-member-staging-table-master-data-services"></a>Table de mise en lots des membres feuille (Master Data Services)

[!INCLUDE [SQL Server - Windows only ASDBMI  ](../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  Utilisez la table de mise en lots des membres feuille (stg.name_Leaf) dans la base de données [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] pour créer, mettre à jour, désactiver et supprimer des membres feuille. Elle permet également de mettre à jour les valeurs d'attribut des membres feuille.  
  
##  <a name="table-columns"></a><a name="TableColumns"></a> Colonnes de table  
 Le tableau suivant explique la fonction de chacun des champs de la table de mise en lots Feuille.  
  
|Nom de la colonne|Description|Valeurs|  
|-----------------|-----------------|------------|  
|**Identifiant**|Identificateur automatiquement affecté.|N'entrez pas de valeur dans ce champ. Si le lot n'a pas été traité, ce champ est vide.|  
|**ImportType**<br /><br /> Obligatoire|Détermine ce qu'il convient de faire lorsque les données mises en lots correspondent aux données qui existent déjà dans la base de données [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] .|**0**: créer des membres. Remplacez les données MDS existantes par les données mises en lots, mais uniquement si les données mises en lots n'ont pas la valeur NULL. Les valeurs NULL sont ignorées. Pour remplacer une valeur d'attribut de chaîne par la valeur NULL, définissez-la sur **~NULL~**. Pour remplacer une valeur d'attribut numérique par la valeur NULL, définissez-la sur **-98765432101234567890**. Pour remplacer une valeur d'attribut datetime par la valeur NULL, définissez-la sur **5555-11-22T12:34:56**.<br /><br /> **1**: créer des nouveaux membres uniquement. Toutes les mises à jour des données MDS existantes échouent.<br /><br /> **2**: crée de nouveaux membres. Remplacez les données MDS existantes par les données mises en lots. Si vous importez des valeurs NULL, elles remplacent les valeurs MDS existantes.<br /><br /> **3**: désactiver le membre, en fonction de la valeur Code. Les attributs, les appartenances de hiérarchie et de collection et les transactions sont tous conservés mais ne sont plus disponibles dans l'interface utilisateur. Si le membre est utilisé comme valeur d'attribut basé sur un domaine d'un autre membre, la désactivation échoue. Consultez **ImportType5** pour une alternative.<br /><br /> **4**: supprimer définitivement le membre, en fonction de la valeur Code. Les attributs, les appartenances de hiérarchie et de collection et les transactions sont tous supprimés définitivement. Si le membre est utilisé comme valeur d'attribut basé sur un domaine d'un autre membre, la suppression échoue. Consultez **ImportType6** pour une alternative.<br /><br /> **5**: désactiver le membre, en fonction de la valeur **Code** . Les attributs, les appartenances de hiérarchie et de collection et les transactions sont tous conservés mais ne sont plus disponibles dans l'interface utilisateur. Si le membre est utilisé comme valeur d'attribut basé sur un domaine d'autres membres, les valeurs associées seront NULL. ImportType 5 est destiné à des membres feuille uniquement.<br /><br /> **6**: supprimer définitivement le membre, en fonction de la valeur **Code** . Les attributs, les appartenances de hiérarchie et de collection et les transactions sont tous supprimés définitivement. Si le membre est utilisé comme valeur d'attribut basé sur un domaine d'autres membres, les valeurs associées seront NULL. ImportType 6 est destiné à des membres feuille uniquement.|  
|**ImportStatus_ID**<br /><br /> Obligatoire|État du processus d'importation.|**0**, que vous spécifiez pour indiquer que l'enregistrement est prêt pour la mise en lots ;<br /><br /> **1**, qui est affecté automatiquement et qui indique que le processus de mise en lots pour l'enregistrement a réussi ;<br /><br /> **2**, qui est affecté automatiquement et qui indique que le processus de mise en lots pour l'enregistrement a échoué.|  
|**Batch_ID**<br /><br /> Requis par le service Web uniquement|Identificateur automatiquement affecté qui regroupe des enregistrements pour la mise en lots. Cet identificateur, affiché dans l'interface utilisateur [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)] dans la colonne **ID** , est affecté à tous les membres du lot.|Si le lot n'a pas été traité, ce champ est vide.|  
|**BatchTag**<br /><br /> Requis, sauf par le service Web|Nom unique du lot, jusqu'à 50 caractères.||  
|**ErrorCode**|Affiche un code d'erreur. Pour tous les enregistrements dont la valeur **ImportStatus_ID** est **2**, consultez [Erreurs du processus de mise en lots &#40;Master Data Services&#41;](../master-data-services/staging-process-errors-master-data-services.md).||  
|**Code**<br /><br /> Obligatoire, sauf quand les codes sont générés automatiquement pour **ImportType1** ou **2**. Pour plus d’informations, consultez [Création automatique de code &#40;Master Data Services&#41;](../master-data-services/automatic-code-creation-master-data-services.md).|Code unique du membre.||  
|**Nom**<br /><br /> Facultatif|Nom du membre.||  
|**NewCode**|À utiliser uniquement si vous modifiez le code du membre.||  
|\<Attribute name>|Il existe une colonne pour chaque attribut dans l'entité. Utilisez ceci avec un **ImportType** de valeur **0** ou **2**. Pour les attributs de forme libre, spécifiez le nouveau texte ou la nouvelle valeur de chaîne pour l'attribut. Pour les attributs basés sur un domaine, spécifiez le code du membre qui sera l'attribut. Pour les attributs de lien, l’URL doit commencer par **https://**.<br /><br /> Remarque : vous ne pouvez pas mettre en lots des attributs de fichier.||  
  
## <a name="see-also"></a>Voir aussi  
 [Vue d’ensemble : importation de données à partir de tables &#40;Master Data Services&#41;](../master-data-services/overview-importing-data-from-tables-master-data-services.md)   
 [Afficher les erreurs qui se produisent lors de la &#40;de la mise en lots Master Data Services&#41;](../master-data-services/view-errors-that-occur-during-staging-master-data-services.md)   
 [Erreurs du processus de mise en lots &#40;Master Data Services&#41;](../master-data-services/staging-process-errors-master-data-services.md)  
  
  
