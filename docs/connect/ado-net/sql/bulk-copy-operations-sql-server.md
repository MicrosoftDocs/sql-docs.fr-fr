---
title: Opérations de copie en bloc dans SQL Server
description: Décrit la fonctionnalité de copie en bloc pour le fournisseur de données .NET pour SQL Server.
ms.date: 06/15/2020
ms.assetid: 83a7a0d2-8018-4354-97b9-0b1d99f8342b
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
ms.reviewer: v-kaywon
ms.openlocfilehash: 4896bfdb419cfbd8e2cf6302a0a818407d6a596c
ms.sourcegitcommit: 6b3569977b034554883a94d73d1c4df6e2f74fe2
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/19/2020
ms.locfileid: "85107008"
---
# <a name="bulk-copy-operations-in-sql-server"></a>Opérations de copie en bloc dans SQL Server

[!INCLUDE[Driver_ADONET_Download](../../../includes/driver_adonet_download.md)]

Microsoft SQL Server inclut un utilitaire en ligne de commande connu, **bcp**, pour rapidement copier en bloc des fichiers volumineux dans des tables ou des vues de bases de données SQL Server. La classe <xref:Microsoft.Data.SqlClient.SqlBulkCopy> vous permet d’écrire des solutions de code managé, qui fournissent des fonctionnalités similaires. Il existe d’autres façons de charger des données dans une table SQL Server (instructions INSERT, par exemple) mais <xref:Microsoft.Data.SqlClient.SqlBulkCopy> offre de meilleures performances.  
  
À l'aide de la classe <xref:Microsoft.Data.SqlClient.SqlBulkCopy>, vous pouvez :  
  
- Une opération unique de copie en bloc  
  
- Plusieurs opérations de copie en bloc  
  
- Une opération de copie en bloc dans une transaction  
  
> [!NOTE]
>  Quand vous utilisez .NET Framework version 1.1 ou antérieure (ne prenant pas en charge la classe <xref:Microsoft.Data.SqlClient.SqlBulkCopy>), vous pouvez exécuter l’instruction SQL Server Transact-SQL **BULK INSERT** à l’aide de l’objet <xref:Microsoft.Data.SqlClient.SqlCommand>.  
  
## <a name="in-this-section"></a>Contenu de cette section  
[Configuration de l’exemple de copie en bloc](bulk-copy-example-setup.md)  
Décrit les tables utilisées dans les exemples de copie en bloc et fournit des scripts SQL pour créer les tables dans la base de données AdventureWorks.  
  
[Opérations uniques de copie en bloc](single-bulk-copy-operations.md)  
Décrit comment effectuer une seule copie en bloc de données dans une instance de SQL Server à l’aide de la classe <xref:Microsoft.Data.SqlClient.SqlBulkCopy> et comment effectuer l’opération de copie en bloc à l’aide d’instructions Transact-SQL et de la classe <xref:Microsoft.Data.SqlClient.SqlCommand>.  
  
[Plusieurs opérations de copie en bloc](multiple-bulk-copy-operations.md)  
Décrit comment effectuer plusieurs opérations de copie en bloc de données dans une instance de SQL Server à l’aide de la classe <xref:Microsoft.Data.SqlClient.SqlBulkCopy>.  
  
[Transaction et opérations de copie en bloc](transaction-bulk-copy-operations.md)  
Décrit comment effectuer une opération de copie en bloc dans une transaction, y compris la validation ou la restauration de la transaction.  

[Indicateurs d’ordre pour les opérations de copie en bloc](bulk-copy-order-hints.md)  
Explique comment utiliser les indicateurs d’ordre pour améliorer les performances de copie en bloc.
  
## <a name="next-steps"></a>Étapes suivantes
- [SQL Server et ADO.NET](index.md)
