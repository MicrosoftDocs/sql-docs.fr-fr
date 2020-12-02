---
description: Sélectionner des tables Oracle pour capturer des modifications
title: Sélectionner des tables Oracle pour capturer des modifications | Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- selOraTabDia
ms.assetid: 2e295dc8-999d-4c4d-96cc-1519674b47a4
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 5417245afb872fac5ffc144c0d7a8b946e677e8b
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/26/2020
ms.locfileid: "88457625"
---
# <a name="select-oracle-tables-for-capturing-changes"></a>Sélectionner des tables Oracle pour capturer des modifications

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  Cette boîte de dialogue permet de sélectionner les tables incluses dans l'instance de capture de données modifiées. Les tables sélectionnées sont ajoutées à la liste dans la page **Sélectionner des tables et des colonnes** de l'Assistant Nouvelle instance. Cette boîte de dialogue permet d'effectuer les opérations suivantes :  
  
 Par défaut, aucune table n'est incluse dans la liste des tables de cette boîte de dialogue. Vous pouvez activer la case à cocher en haut de la colonne de case à cocher pour sélectionner toutes les tables ou rechercher des tables spécifiques.  
  
 **Pour rechercher des tables spécifiques**  
 Entrez les critères de recherche comme suit, puis cliquez sur **Rechercher**:  
  
-   **Schéma**: dans la liste, sélectionnez un schéma de base de données. Seules les tables qui ont ce schéma seront incluses dans la liste.  
  
-   **Modèle de nom de table**: entrez n'importe quelle chaîne de caractères. Seules les tables qui incluent la chaîne de caractères entrée s'affichent.  
  
> [!NOTE]  
>  Vous pouvez entrer des critères dans un des deux champs ou dans les deux.  
  
-   **Afficher les 1 000 premières tables correspondantes**: par défaut cette case à cocher est activée. Elle limite l'affichage aux 1 000 premières tables correspondantes. Si vous désactivez la case à cocher, toutes les tables qui correspondent aux critères s'affichent. S'il existe un grand nombre de tables, l'affichage de la liste peut prendre beaucoup de temps.  
  
 **Pour sélectionner les tables à inclure dans l'instance CDC**  
 Cochez la case en regard d’une table à inclure, puis cliquez sur **Ajouter**. Les tables sont ajoutées à la liste dans la page **Sélectionner des tables et des colonnes** de l'Assistant Nouvelle instance.  
  
 Cliquez sur **Fermer** pour fermer la boîte de dialogue sans ajouter de table supplémentaire.  
  
> [!NOTE]  
>  Si vous sélectionnez une table qui inclut un type de données non pris en charge, un message d'erreur s'affiche et la table n'est pas incluse.  
  
## <a name="see-also"></a>Voir aussi  
 [Procédure : créer l'instance SQL Server de base de données de modifications](../../integration-services/change-data-capture/how-to-create-the-sql-server-change-database-instance.md)   
 [Sélectionner des tables et des colonnes Oracle](../../integration-services/change-data-capture/select-oracle-tables-and-columns.md)  
  
  
