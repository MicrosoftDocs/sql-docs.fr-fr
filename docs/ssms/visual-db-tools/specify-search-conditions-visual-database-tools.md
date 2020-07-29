---
title: Spécifier des conditions de recherche
ms.custom: seo-lt-2019
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- choosing search criteria
- search conditions [SQL Server], specifying
- search criteria [SQL Server], specifying conditions
ms.assetid: 18e2c759-68ec-4efe-b208-2f73418cd9bd
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.openlocfilehash: e1541a4602c53c96dfad36c549bc75c8cf76fae5
ms.sourcegitcommit: f3321ed29d6d8725ba6378d207277a57cb5fe8c2
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/06/2020
ms.locfileid: "85999260"
---
# <a name="specify-search-conditions-visual-database-tools"></a>Spécifier des conditions de recherche (Visual Database Tools)
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
Vous pouvez spécifier les lignes de données figurant dans votre requête en spécifiant des conditions de recherche. Par exemple, si vous formulez une requête sur une table `employee` , vous pouvez spécifier que vous ne souhaitez rechercher que les employés travaillant dans une région donnée.  
  
Vous spécifiez des conditions de recherche à l'aide d'une expression. La plupart du temps, cette expression comporte un opérateur et une valeur de recherche. Pour rechercher, par exemple, les employés d'une région commerciale donnée, vous pouvez spécifier le critère de recherche suivant pour la colonne `region` :  
  
```  
='UK'  
```  
  
> [!NOTE]  
> Si vous utilisez plusieurs tables, le Concepteur de requêtes et de vues étudie chaque condition de recherche, afin de déterminer si la comparaison que vous établissez donne lieu à une jointure. Si tel est le cas, le Concepteur de requêtes et de vues convertit alors automatiquement la condition de recherche en jointure. Pour plus d’informations, consultez [Joindre automatiquement des tables &#40;Visual Database Tools&#41;](../../ssms/visual-db-tools/join-tables-automatically-visual-database-tools.md).  
  
### <a name="to-specify-search-conditions"></a>Pour spécifier des conditions de recherche  
  
1.  Si vous ne l'avez pas encore effectué, ajoutez les colonnes ou les expressions que vous souhaitez utiliser dans votre condition de recherche au volet Critères.  
  
    Si vous créez une requête Select et si vous ne souhaitez pas que les expressions ou les colonnes de recherche figurent dans le résultat de la requête, effacez chaque expression ou colonne de recherche de la colonne **Sortie** pour les exclure des colonnes de sortie.  
  
2.  Localisez la ligne comportant l’expression ou la colonne de données à rechercher, puis entrez une condition de recherche dans la colonne **Filtres** .  
  
    > [!NOTE]  
    > Si vous n'entrez pas d'opérateur, le Concepteur de requêtes et de vues insère automatiquement l'opérateur d'égalité « = ».  
  
Le Concepteur de requêtes et de vues met à jour l’instruction SQL dans le [volet SQL](../../ssms/visual-db-tools/sql-pane-visual-database-tools.md) en ajoutant ou en modifiant la clause WHERE.  
  
## <a name="see-also"></a>Voir aussi  
[Règles pour l’entrée de valeurs de recherche &#40;Visual Database Tools&#41;](../../ssms/visual-db-tools/rules-for-entering-search-values-visual-database-tools.md)  
[Spécifier des critères de recherche &#40;Visual Database Tools&#41;](../../ssms/visual-db-tools/specify-search-criteria-visual-database-tools.md)  
  
