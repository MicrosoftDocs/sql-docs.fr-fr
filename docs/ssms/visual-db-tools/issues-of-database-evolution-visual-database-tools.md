---
description: Problèmes liés à l'évolution d'une base de données (Visual Database Tools)
title: Problèmes d’évolution des bases de données
ms.custom: seo-lt-2019
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- compatibility [SQL Server], multuser database changes
- database evolution [SQL Server]
ms.assetid: 1ed6ae10-d212-4ec2-8569-1b94ab1cba6d
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.openlocfilehash: d42c0bb0c18b7917ef0e8c8a29e9522e744ce2d2
ms.sourcegitcommit: 22dacedeb6e8721e7cdb6279a946d4002cfb5da3
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/14/2020
ms.locfileid: "92037767"
---
# <a name="issues-of-database-evolution-visual-database-tools"></a>Problèmes liés à l'évolution d'une base de données (Visual Database Tools)
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
Si vous modifiez la structure d'une base de données déployée, assurez-vous que les changements sont compatibles avec les données existantes et la structure de la base de données. Il est quelquefois nécessaire de prendre des mesures spéciales si les modifications appartiennent à une des catégories suivantes :  
  
-   **Ajout d’une contrainte** Quand vous ajoutez une contrainte, la base de données contient peut-être déjà des données qui ne la respectent pas. Quand vous essayez d’enregistrer la nouvelle contrainte, la [boîte de dialogue Notifications post-enregistrement &#40;Visual Database Tools&#41;](../../ssms/visual-db-tools/post-save-notifications-dialog-box-visual-database-tools.md) vous informe que le serveur de base de données n’a pas pu la créer. Pour obliger la base de données à accepter la nouvelle contrainte, vous pouvez décocher la case **Vérifier les données existantes à la création**.  
  
-   **Ajout d’une relation** Quand vous ajoutez une relation, la base de données contient peut-être déjà dans la table de clés étrangères des lignes sans correspondance dans la table de clés primaires. Ce qui signifie que les données existantes ne respectent peut-être pas l'intégrité référentielle. Quand vous essayez d’enregistrer la nouvelle relation, la [boîte de dialogue Notifications post-enregistrement &#40;Visual Database Tools&#41;](../../ssms/visual-db-tools/post-save-notifications-dialog-box-visual-database-tools.md) vous informe que le serveur de base de données n’a pas pu enregistrer la table de clé étrangère révisée. Pour obliger la base de données à accepter la modification, vous pouvez décocher la case **Vérifier les données existantes à la création**.  
  
-   **Modification d’une table contribuant à une vue indexée** Si vous modifiez une table contribuant à une vue Microsoft SQL Server indexée, vous perdez les index de la vue. Pour plus d'informations sur la reconstitution d'index, consultez la documentation en ligne de SQL Server.  
  
-   **Suppression d’un objet** Si vous supprimez un objet, tel qu’une colonne, une table ou une vue, vérifiez d’abord qu’il n’est pas référencé par un autre objet dans la base de données.  
  
Quelle que soit la façon dont vous modifiez le design de la base de données, gardez un historique de ces modifications. Une méthode consiste à conserver des scripts SQL de toutes les modifications que vous avez apportées à la base de données de production.  
  
## <a name="see-also"></a>Voir aussi  
[Utilisation des contraintes](../../relational-databases/tables/unique-constraints-and-check-constraints.md)  
[Environnements multi-utilisateurs &#40;Visual Database Tools&#41;](../../ssms/visual-db-tools/multiuser-environments-visual-database-tools.md)  
