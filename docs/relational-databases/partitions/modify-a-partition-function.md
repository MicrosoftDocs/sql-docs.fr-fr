---
description: Modifier une fonction de partition
title: Modifier une fonction de partition | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: ''
ms.topic: conceptual
ms.assetid: ae5bfc09-f27a-4ea9-9518-485278b11674
author: julieMSFT
ms.author: jrasnick
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: b200814dedac25809d1295146ff71d8908bce4da
ms.sourcegitcommit: 108bc8e576a116b261c1cc8e4f55d0e0713d402c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/25/2021
ms.locfileid: "98766111"
---
# <a name="modify-a-partition-function"></a>Modifier une fonction de partition
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
  Vous pouvez modifier le partitionnement d'une table ou d'un index dans [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] en ajoutant ou supprimant le nombre de partitions spécifié (par incréments de 1) dans la fonction de partition de la table ou de l'index à l'aide de [!INCLUDE[tsql](../../includes/tsql-md.md)]. Lorsque vous ajoutez une partition, vous fractionnez une partition existante en deux partitions dont vous redéfinissez les limites. Lorsque vous supprimez une partition, vous fusionnez les limites de deux partitions pour n'en définir qu'une. Cette action a pour effet de remplir à nouveau une partition et de laisser l'autre non affectée.  
  
> [!CAUTION]  
>  Une même fonction de partition peut être utilisée par plusieurs tables ou index. Lorsque vous modifiez une fonction de partition, vous affectez toutes les fonctions dans une transaction unique. Vérifiez les dépendances de la fonction de partition avant de la modifier.  
  
 **Dans cette rubrique**  
  
-   **Avant de commencer :**  
  
     [Limitations et restrictions](#Restrictions)  
  
     [Sécurité](#Security)  
  
-   **Pour modifier une fonction de partition à l'aide de :**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Avant de commencer  
  
###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> Limitations et restrictions  
  
-   Vous ne pouvez utiliser ALTER PARTITION FUNCTION que pour diviser une partition en deux ou pour fusionner deux partitions en une seule. Pour modifier le mode de partitionnement d'une table ou d'un index (par exemple, pour passer de 10 à 5 partitions), vous disposez de plusieurs options :  
  
    -   Créez une nouvelle table partitionnée avec la fonction de partition de votre choix, puis insérez les données de l'ancienne table dans la nouvelle à l'aide de l'instruction INSERT INTO ... SELECT FROM [!INCLUDE[tsql](../../includes/tsql-md.md)] ou de **l’Assistant gestion de partition** dans [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)].  
  
    -   Créez un index cluster partitionné sur un segment.  
  
        > [!NOTE]  
        >  La suppression d'un index cluster partitionné engendre un segment partitionné.  
  
    -   Supprimez et reconstruisez un index partitionné existant à l'aide de l'instruction [!INCLUDE[tsql](../../includes/tsql-md.md)] CREATE INDEX avec la clause DROP EXISTING = ON.  
  
    -   Exécutez une séquence d'instructions ALTER PARTITION FUNCTION.  
  
-   [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] n'assure pas la prise en charge de la réplication lors de la modification d'une fonction de partition. Si vous voulez apporter des modifications à une fonction de partition dans la base de données de publication, vous devez le faire manuellement dans la base d'abonnement.  
  
-   Tous les groupes de fichiers concernés par l’instruction ALTER PARTITION FUNCTION doivent être en ligne.  
  
###  <a name="security"></a><a name="Security"></a> Sécurité  
  
####  <a name="permissions"></a><a name="Permissions"></a> Autorisations  
 L'instruction ALTER PARTITION FUNCTION peut être exécutée avec les autorisations suivantes :  
  
-   Autorisation ALTER ANY DATASPACE. Cette autorisation est attribuée par défaut aux membres du rôle de serveur fixe **sysadmin** et des rôles de base de données fixes **db_owner** et **db_ddladmin** .  
  
-   Autorisation CONTROL ou ALTER sur la base de données dans laquelle la fonction de partition a été créée.  
  
-   Autorisation CONTROL SERVER ou ALTER ANY DATABASE sur le serveur de la base de données dans laquelle la fonction de partition a été créée.  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Utilisation de SQL Server Management Studio  
 **Pour modifier une fonction de partition :**  
  
 Cette action spécifique ne peut pas être exécutée à l'aide de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]. Afin de modifier une fonction de partition, vous devez d'abord supprimer la fonction puis en créer une nouvelle avec les propriétés souhaitées à l'aide de l'Assistant Création de partition. Pour plus d'informations, consultez la rubrique  
  
#### <a name="to-delete-a-partition-function"></a>Pour supprimer une fonction de partition  
  
1.  Développez la base de données dans laquelle vous souhaitez supprimer la fonction de partition, puis développer le dossier **Stockage** .  
  
2.  Développez le dossier **Fonctions de partition** .  
  
3.  Cliquez avec le bouton droit sur la fonction de partition à supprimer, puis sélectionnez **Supprimer**.  
  
4.  Dans la boîte de dialogue **Supprimer un objet** , assurez-vous que la fonction de partition est sélectionnée, puis cliquez sur **OK**.  

##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Utilisation de Transact-SQL  
  
#### <a name="to-split-a-single-partition-into-two-partitions"></a>Pour fractionner une partition unique en deux partitions  
  
1.  Dans l' **Explorateur d'objets**, connectez-vous à une instance du [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
2.  Dans la barre d'outils standard, cliquez sur **Nouvelle requête**.  
  
3.  Copiez et collez l'exemple suivant dans la fenêtre de requête, puis cliquez sur **Exécuter**.  
  
    ```  
    -- Look for a previous version of the partition function "myRangePF1" and deletes it if it is found.  
    IF EXISTS (SELECT * FROM sys.partition_functions  
        WHERE name = 'myRangePF1')  
        DROP PARTITION FUNCTION myRangePF1;  
    GO  
    -- Create a new partition function called "myRangePF1" that partitions a table into four partitions.  
    CREATE PARTITION FUNCTION myRangePF1 (int)  
    AS RANGE LEFT FOR VALUES ( 1, 100, 1000 );  
    GO  
    --Split the partition between boundary_values 100 and 1000  
    --to create two partitions between boundary_values 100 and 500  
    --and between boundary_values 500 and 1000.  
    ALTER PARTITION FUNCTION myRangePF1 ()  
    SPLIT RANGE (500);  
    ```  
  
#### <a name="to-merge-two-partitions-into-one-partition"></a>Pour fusionner deux partitions dans une partition  
  
1.  Dans l' **Explorateur d'objets**, connectez-vous à une instance du [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
2.  Dans la barre d'outils standard, cliquez sur **Nouvelle requête**.  
  
3.  Copiez et collez l'exemple suivant dans la fenêtre de requête, puis cliquez sur **Exécuter**.  
  
    ```  
    -- Look for a previous version of the partition function "myRangePF1" and deletes it if it is found.  
    IF EXISTS (SELECT * FROM sys.partition_functions  
        WHERE name = 'myRangePF1')  
        DROP PARTITION FUNCTION myRangePF1;  
    GO  
    -- Create a new partition function called "myRangePF1" that partitions a table into four partitions.  
    CREATE PARTITION FUNCTION myRangePF1 (int)  
    AS RANGE LEFT FOR VALUES ( 1, 100, 1000 );  
    GO  
    --Merge the partitions between boundary_values 1 and 100  
    --and between boundary_values 100 and 1000 to create one partition  
    --between boundary_values 1 and 1000.  
    ALTER PARTITION FUNCTION myRangePF1 ()  
    MERGE RANGE (100);  
    ```  
  
 Pour plus d’informations, consultez [ALTER PARTITION FUNCTION &#40;Transact-SQL&#41;](../../t-sql/statements/alter-partition-function-transact-sql.md).  
  
  
