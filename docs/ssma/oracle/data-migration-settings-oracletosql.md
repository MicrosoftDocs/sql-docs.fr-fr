---
description: Paramètres de migration de données (OracleToSQL)
title: Paramètres de migration des données (OracleToSQL) | Microsoft Docs
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: 91f7f558-025d-4f4d-ac2c-aa095e7d1ace
author: nahk-ivanov
ms.author: alexiva
manager: alexiva
ms.openlocfilehash: 4bd91336e15f7d9404632aafd0b9cbc29e0dc2e0
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100058464"
---
# <a name="data-migration-settings-oracletosql"></a>Paramètres de migration de données (OracleToSQL)
  
## <a name="data-migration-settings"></a>Paramètres de migration de données  
Les **paramètres de migration des données** permettent à l’utilisateur d’écrire des requêtes personnalisées pour la migration des données.  
  
-   Cet onglet est disponible lorsque l' **option options de migration étendue des données** est définie sur **Afficher** et est masquée lorsque le paramètre est défini sur **Masquer** dans les paramètres du projet. Pour plus d’informations sur les paramètres de migration de projet, consultez [paramètres du projet (migration)](./project-settings-migration-oracletosql.md) .  
  
-   L’analyse des instructions SQL personnalisées sera implémentée dans l’onglet **paramètres de migration de données** du nœud de la table.  
  
-   Voici les deux cases à cocher disponibles dans les **paramètres de migration de données** , à savoir :  
  
    1.  Tronquer la table SQL Server  
  
    2.  Utiliser une sélection personnalisée  
  
1.  **Tronquer la table SQL Server :**  
     Cette option permet à l’utilisateur de disposer d’une vue claire des données migrées au niveau de la base de données cible.  
  
    -   Par défaut, cette zone de texte est cochée.  
  
    -   Si cette zone de texte est désactivée, les données qui sont migrées seront ajoutées aux données existantes dans la base de données cible.  
  
2.  **Utiliser la sélection personnalisée :**  
     Cette option permet à l’utilisateur de modifier l’instruction **Select** présente (l’instruction **Select** permet aux utilisateurs de sélectionner les données à afficher dans la base de données cible).  
  
    1.  Par défaut, cette zone de texte est désactivée.  
  
    2.  Si cette zone de texte est cochée, elle permet aux utilisateurs de modifier l’instruction **Select** présente.  
  
Deux boutons sont présents, à savoir :  
  
-   **Appliquer :** Cliquez sur **appliquer** pour appliquer les paramètres qui ont été modifiés.  
  
-   **Annuler :** Cliquez sur **Annuler** pour restaurer les paramètres présents avant l’établissement des modifications.  
  
## <a name="see-also"></a>Voir aussi  
[Migration des données Oracle vers SQL Server](migrating-oracle-data-into-sql-server-oracletosql.md)  
