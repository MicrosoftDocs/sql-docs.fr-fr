---
title: Objets pris en charge par l'Assistant Génération de scripts
description: Découvrez les types d’objets que l’Assistant générer et publier des scripts peut vous aider à publier.
ms.custom: seo-lt-2019
ms.date: 03/01/2017
ms.prod: sql
ms.technology: ssms
ms.reviewer: ''
ms.topic: conceptual
ms.assetid: 071eb2cb-f073-41ca-9f4d-11d3b8803495
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 5b078e1b5e0f76a565aa0bd2d686836d24d5ba98
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100270867"
---
# <a name="objects-supported-by-the-generate-scripts-wizard"></a>Objets pris en charge par l'Assistant Génération de scripts
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
  L'Assistant Générer et publier des scripts prend en charge un sous-ensemble des objets pris en charge par le [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)].  
  
## <a name="supported-objects"></a>Objets pris en charge  
 Le tableau suivant répertorie les objets qui peuvent être publiés et pris en charge par l'Assistant Générer et publier des scripts.  
  
:::row:::
    :::column:::
        Rôle d'application
    :::column-end:::
    :::column:::
        Rôle de base de données
    :::column-end:::
    :::column:::
        schéma
    :::column-end:::
    :::column:::
        Agrégat défini par l'utilisateur
    :::column-end:::
    :::column:::
        Affichage*
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
        Assembly
    :::column-end:::
    :::column:::
        Contrainte DEFAULT
    :::column-end:::
    :::column:::
        Procédure stockée*
    :::column-end:::
    :::column:::
        Type de données défini par l'utilisateur
    :::column-end:::
    :::column:::
        Collection de schémas XML
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
        Contrainte CHECK
    :::column-end:::
    :::column:::
        Catalogue de texte intégral
    :::column-end:::
    :::column:::
        Synonyme
    :::column-end:::
    :::column:::
        Fonction définie par l'utilisateur
    :::column-end:::
    :::column:::
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
        Procédure stockée CLR (Common Language Runtime)*
    :::column-end:::
    :::column:::
        Index
    :::column-end:::
    :::column:::
        Table de charge de travail
    :::column-end:::
    :::column:::
        Table définie par l'utilisateur
    :::column-end:::
    :::column:::
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
        Fonction CLR définie par l'utilisateur
    :::column-end:::
    :::column:::
        Règle
    :::column-end:::
    :::column:::
        Utilisateur**
    :::column-end:::
    :::column:::
        Type défini par l'utilisateur
    :::column-end:::
    :::column:::
    :::column-end:::
:::row-end:::

 *Publié sans chiffrement.  
  
 **Tous les utilisateurs non-système qui existent dans la base de données sont publiés en tant que rôles.  
  
  
