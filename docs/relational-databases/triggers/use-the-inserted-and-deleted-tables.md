---
description: Utiliser les tables inserted et deleted
title: Utiliser les tables inserted et deleted | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: ''
ms.topic: conceptual
helpviewer_keywords:
- inserted tables
- UPDATE statement [SQL Server], DML triggers
- DELETE statement [SQL Server], DML triggers
- INSTEAD OF triggers
- deleted tables
- INSERT statement [SQL Server], DML triggers
- DML triggers, deleted or inserted tables
ms.assetid: ed84567f-7b91-4b44-b5b2-c400bda4590d
author: rothja
ms.author: jroth
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 6f78a502be14ffcf51937823d61bd61c144777f5
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99187264"
---
# <a name="use-the-inserted-and-deleted-tables"></a>Utiliser les tables inserted et deleted
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]
  Les instructions de déclenchement DML utilisent deux tables spéciales : la table deleted et la table inserted. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] crée et gère automatiquement ces tables. Ces tables temporaires résidant en mémoire servent à tester les effets de certaines modifications de données et à définir des conditions pour les actions de déclencheur DML. Vous ne pouvez pas modifier directement les données contenues dans les tables ou effectuer des opérations DDL (Data Definition Language) sur les tables, telles que CREATE INDEX.  
  
 Dans les déclencheurs DML, les tables inserted et deleted sont principalement utilisées pour exécuter les opérations suivantes :  
  
-   étendre l'intégrité référentielle entre les tables ;  
  
-   insérer ou mettre à jour des données dans des tables de base sous-jacentes d'une vue ;  
  
-   déceler la présence d'erreurs et prendre les mesures nécessaires ;  
  
-   détecter la différence entre l'état d'une table avant et après une modification des données, et prendre les mesures nécessaires en fonction de cette différence.  
  
 La table deleted stocke des copies des lignes affectées par les instructions DELETE et UPDATE. Pendant l'exécution d'une instruction DELETE ou UPDATE, certaines lignes sont supprimées de la table du déclencheur et déplacées vers la table deleted. La table deleted et la table du déclencheur n'ont habituellement pas de ligne en commun.  
  
 La table inserted stocke des copies des lignes affectées par les instructions INSERT et UPDATE. Pendant une transaction d'insertion ou de mise à jour, de nouvelles lignes sont ajoutées dans la table inserted et dans la table du déclencheur. Les lignes de la table inserted sont des copies des lignes créées dans la table du déclencheur.  
  
 D'un point de vue théorique, une transaction de mise à jour est une opération de suppression suivie d'une opération d'insertion ; les anciennes lignes sont d'abord copiées dans la table deleted, et les nouvelles lignes sont ensuite copiées dans la table du déclencheur et dans la table inserted.  
  
 Pour définir les conditions du déclencheur, utilisez les tables inserted et deleted de façon appropriée, en fonction de l'action qui a activé le déclencheur. Bien que vous puissiez, sans provoquer d'erreur, référencer la table deleted pendant le test d'une insertion (INSERT) ou la table inserted pendant le test d'une suppression (DELETE), ces tables de test du déclencheur ne contiendront alors aucune ligne.  
  
> [!NOTE]  
>  Si les actions du déclencheur dépendent du nombre de lignes affectées par une modification de données, utilisez les tests (comme l’examen de @@ROWCOUNT) pour les modifications de données multilignes (une instruction INSERT, DELETE ou UPDATE basée sur une instruction SELECT), puis effectuez les opérations appropriées. Pour plus d’informations, consultez [Créer de déclencheurs DML pour gérer plusieurs lignes de données](../../relational-databases/triggers/create-dml-triggers-to-handle-multiple-rows-of-data.md).
  
 [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] n’autorise pas les références aux colonnes de type **text**, **ntext** ou **image** dans les tables inserted et deleted pour les déclencheurs AFTER. Cependant, ces types de données sont inclus à des fins de compatibilité ascendante uniquement. Pour le stockage des données volumineuses, il est préférable d’utiliser les types de données **varchar(max)** , **nvarchar(max)** et **varbinary(max)** . Les déclencheurs AFTER et INSTEAD OF prennent en charge les données **varchar(max)** , **nvarchar(max)** et **varbinary(max)** dans les tables inserted et deleted. Pour plus d’informations, consultez [CREATE TRIGGER &#40;Transact-SQL&#41;](../../t-sql/statements/create-trigger-transact-sql.md).  
  
 **Exemple de l'utilisation de la table insérée dans un déclencheur pour imposer des règles d'entreprise**  
  
 Les contraintes CHECK pouvant référencer uniquement les colonnes sur lesquelles des contraintes de niveau table ou colonne sont définies, toutes les contraintes entre tables (dans ce cas, des règles de gestion) doivent être définies sous la forme de déclencheurs.  
  
 L'exemple suivant crée un déclencheur DML. Ce déclencheur vérifie que les informations de conditions de crédit du fournisseur sont correctes lors d'une tentative d'insertion d'un nouveau bon de commande dans la table `PurchaseOrderHeader` . Pour obtenir les informations de conditions de crédit du fournisseur correspondant à la commande qui vient d'être insérée, la table `Vendor` doit être référencée et jointe à la table inserted. Si les conditions de crédit sont trop faibles, un message s'affiche et l'insertion n'a pas lieu.
  
 [!code-sql[TriggerDDL#CreateTrigger3](../../relational-databases/triggers/codesnippet/tsql/use-the-inserted-and-del_1.sql)]  
  
## <a name="using-the-inserted-and-deleted-tables-in-instead-of-triggers"></a>Utilisation des tables inserted et deleted dans les déclencheurs INSTEAD OF  
 Les tables inserted et deleted passées aux déclencheurs INSTEAD OF définis sur des tables suivent les mêmes règles que les tables inserted et deleted passées aux déclencheurs AFTER. Le format des tables inserted et deleted est le même que celui de la table sur laquelle est défini le déclencheur INSTEAD OF. Chaque colonne des tables inserted et deleted est directement mappée à une colonne de la table de base.  
  
 Qu'une table possède ou non un déclencheur INSTEAD OF, les règles suivantes qui régissent la fourniture de valeurs pour les colonnes par une instruction INSERT ou UPDATE faisant référence à la table sont les mêmes :  
  
-   Aucune valeur ne peut être spécifiée pour une colonne calculée ou de type de données **timestamp** .  
  
-   Aucune valeur ne peut être spécifiée pour les colonnes possédant une propriété IDENTITY, à moins que IDENTITY_INSERT ait la valeur ON pour cette table, auquel cas les instructions INSERT doivent fournir une valeur.  
  
-   Les instructions INSERT doivent fournir des valeurs pour toutes les colonnes NOT NULL pour lesquelles aucune contrainte DEFAULT n'est définie.  
  
-   Les valeurs sont facultatives pour toute colonne acceptant des valeurs NULL ou toute colonne NOT NULL avec valeur par défaut (DEFAULT), sous réserve qu'il ne s'agisse pas d'une colonne calculée, identité ou **timestamp** .  
  
 Lorsqu'une instruction INSERT, UPDATE ou DELETE fait référence à une vue possédant un déclencheur INSTEAD OF, le [!INCLUDE[ssDE](../../includes/ssde-md.md)] appelle le déclencheur au lieu d'effectuer une opération directe sur une table. Le déclencheur doit utiliser les informations présentées dans les tables inserted et deleted pour élaborer toute instruction nécessaire à l'implémentation de l'action requise dans les tables de base, même si le format des informations contenues dans les tables inserted et deleted conçues pour la vue diffère de celui des données stockées dans les tables de base.  
  
 Le format des tables inserted et deleted passées à un déclencheur INSTEAD OF défini sur une vue correspond à la liste de sélection de l'instruction SELECT définie pour la vue. Par exemple :  
  
```  
USE AdventureWorks2012;  
GO  
CREATE VIEW dbo.EmployeeNames (BusinessEntityID, LName, FName)  
AS  
SELECT e.BusinessEntityID, p.LastName, p.FirstName  
FROM HumanResources.Employee AS e   
JOIN Person.Person AS p  
ON e.BusinessEntityID = p.BusinessEntityID;  
```  
  
 Le jeu de résultats pour cette vue possède trois colonnes : une colonne **int** et deux colonnes **nvarchar** . Les tables inserted et deleted passées à un déclencheur INSTEAD OF défini sur la vue ont également une colonne **int** nommée `BusinessEntityID`, une colonne **nvarchar** nommée `LName`et une colonne **nvarchar** nommée `FName`.  
  
 La liste de sélection d'une vue peut également contenir des expressions qui n'établissent pas de mappage direct à une colonne de table de base unique. Certaines expressions de vue, telles que l'invocation d'une constante ou d'une fonction, peuvent ne pas référencer de colonne et être ignorées. Les expressions complexes peuvent référencer plusieurs colonnes, mais les tables inserted et deleted ne détiennent qu'une seule valeur pour chaque ligne insérée. Les mêmes considérations s'appliquent aux expressions simples d'une vue si elles font référence à une colonne calculée à laquelle est associée une expression complexe. Un déclencheur INSTEAD OF défini sur la vue doit gérer ces types d'expressions.  
  
  
