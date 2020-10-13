---
title: Conseiller de compilation native | Microsoft Docs
description: Découvrez comment utiliser le conseiller de compilation native pour migrer une procédure stockée interprétée vers la compilation native dans le cadre de la migration vers l’OLTP en mémoire.
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: in-memory-oltp
ms.topic: conceptual
f1_keywords:
- sql13.swb.nativecompilationwizard.f1
- swb.nativecompilationwizard.f1
ms.assetid: d3898a47-2985-4a08-bc70-fd8331a01b7b
author: MightyPen
ms.author: genemi
monikerRange: =azuresqldb-current||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: efbd90e43c4f2bf7863106330b59f436c31b6238
ms.sourcegitcommit: 4d370399f6f142e25075b3714e5c2ce056b1bfd0
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/09/2020
ms.locfileid: "91868535"
---
# <a name="native-compilation-advisor"></a>Conseiller de compilation native
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  L’outil d’analyse des rapports de transaction indique quelles procédures stockées interprétées dans votre base de données tireront parti de l’utilisation de la compilation native. Pour plus d’informations, consultez [Déterminer si une table ou une procédure stockée doit être déplacée vers l’OLTP en mémoire](../../relational-databases/in-memory-oltp/determining-if-a-table-or-stored-procedure-should-be-ported-to-in-memory-oltp.md).  
  
 Après avoir identifié une procédure stockée que vous souhaitez déplacer pour utiliser la compilation native, vous pouvez utiliser le conseiller de compilation native pour migrer la procédure stockée interprétée vers la compilation native. Pour plus d'informations sur les procédures stockées compilées en mode natif, consultez [Natively Compiled Stored Procedures](./a-guide-to-query-processing-for-memory-optimized-tables.md).  
  
 Dans une procédure stockée interprétée donnée, le conseiller de compilation native vous permet d’identifier toutes les fonctionnalités qui ne sont pas prises en charge dans les modules natifs. Le conseiller de compilation native fournit des liens de documentation permettant de résoudre ou de contourner les problèmes rencontrés.  
  
 Pour plus d’informations sur les méthodologies de migration, consultez [OLTP en mémoire - Modèles de charge de travail courants et considérations relatives à la migration](/previous-versions/dn673538(v=msdn.10)).  
  
## <a name="walkthrough-using-the-native-compilation-advisor"></a>Procédure pas à pas d'utilisation du Conseiller de compilation native  
 Dans l' **Explorateur d'objets**, cliquez avec le bouton droit sur la procédure stockée à convertir, puis sélectionnez **Conseiller de compilation native**. Ce faisant, vous affichez la page d'accueil du **Conseiller compilation native de procédure stockée**. Cliquez sur **Suivant** pour continuer.  
  
### <a name="stored-procedure-validation"></a>Validation de la procédure stockée  
 Cette page signale si la procédure stockée utilise des éléments qui ne sont pas compatibles avec la compilation native. Vous pouvez cliquer sur **Suivant** pour afficher les détails. Si certains éléments ne sont pas compatibles avec la compilation native, vous pouvez cliquer sur **Suivant** pour afficher les détails.  
  
### <a name="stored-procedure-validation-result"></a>Résultats de la validation de la procédure stockée  
 Si certains éléments ne sont pas compatibles avec la compilation native, la page **Résultat de la validation de la procédure stockée** affiche les détails. Vous pouvez générer un rapport (cliquez sur **Générer le rapport**), quitter le **Conseiller de compilation native**et mettre à jour votre code afin qu’il soit compatible avec la compilation native.  
  
## <a name="code-sample"></a>Exemple de code  
 L’exemple suivant illustre une procédure stockée interprétée et la procédure stockée *équivalente* pour la compilation native. L'exemple suppose la présence d'un répertoire appelé c:\data.  
  
> [!NOTE]  
>  Comme d’habitude, l’élément **FILEGROUP** et l’instruction **USE** base_de_données s’appliquent à Microsoft SQL Server, mais pas à la base de données SQL Azure.  
  
```sql  
CREATE DATABASE Demo  
ON  
PRIMARY(NAME = [Demo_data],  
FILENAME = 'C:\DATA\Demo_data.mdf', size=500MB)  
, FILEGROUP [Demo_fg] CONTAINS MEMORY_OPTIMIZED_DATA(  
NAME = [Demo_dir],  
FILENAME = 'C:\DATA\Demo_dir')  
LOG ON (name = [Demo_log], Filename='C:\DATA\Demo_log.ldf', size=500MB)  
COLLATE Latin1_General_100_BIN2;  
go  
  
USE Demo;  
go  
  
CREATE TABLE [dbo].[SalesOrders]  
(  
     [order_id] [int] NOT NULL,  
     [order_date] [datetime] NOT NULL,  
     [order_status] [tinyint] NOT NULL  
     CONSTRAINT [PK_SalesOrders] PRIMARY KEY NONCLUSTERED HASH   
(  
     [order_id]  
) WITH ( BUCKET_COUNT = 2097152)  
) WITH ( MEMORY_OPTIMIZED = ON )  
go  
  
-- Interpreted.  
CREATE PROCEDURE [dbo].[InsertOrder] @id INT, @date DATETIME2, @status TINYINT  
AS   
BEGIN   
  INSERT dbo.SalesOrders VALUES (@id, @date, @status);  
END  
go  
  
-- Natively Compiled.  
CREATE PROCEDURE [dbo].[InsertOrderXTP]  
      @id INT, @date DATETIME2, @status TINYINT  
  WITH NATIVE_COMPILATION, SCHEMABINDING, EXECUTE AS OWNER  
AS   
BEGIN ATOMIC WITH   
     (TRANSACTION ISOLATION LEVEL = SNAPSHOT, LANGUAGE = N'us_english'  
     )  
  INSERT dbo.SalesOrders VALUES (@id, @date, @status);  
END  
go  
  
SELECT * from SalesOrders;  
go  
  
EXECUTE dbo.InsertOrder @id= 10, @date = '1956-01-01 12:00:00', @status = 1;  
EXECUTE dbo.InsertOrderXTP @id= 11, @date = '1956-01-01 12:01:00', @status = 2;  
  
SELECT * from SalesOrders;  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Migration vers OLTP en mémoire](./plan-your-adoption-of-in-memory-oltp-features-in-sql-server.md)   
 [Conditions requises pour l’utilisation des tables optimisées en mémoire](../../relational-databases/in-memory-oltp/requirements-for-using-memory-optimized-tables.md)  
  
