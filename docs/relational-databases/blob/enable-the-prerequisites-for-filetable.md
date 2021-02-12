---
title: Activer les conditions préalables pour les FileTables | Microsoft Docs
description: Pour utiliser FileTables, commencez par activer FILESTREAM, spécifiez un répertoire, puis définissez certaines options et certains niveaux d’accès. Découvrez comment satisfaire tous les prérequis.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.technology: filestream
ms.topic: conceptual
helpviewer_keywords:
- FileTables [SQL Server], prerequisites
ms.assetid: 6286468c-9dc9-4eda-9961-071d2a36ebd6
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: ''
ms.openlocfilehash: d5e02aa025d65fd7f3db6d1f5bd8f43e44566ac9
ms.sourcegitcommit: 58e7069b5b2b6367e27b49c002ca854b31b1159d
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/04/2021
ms.locfileid: "99552658"
---
# <a name="enable-the-prerequisites-for-filetable"></a>Activer les conditions préalables pour les FileTables
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  Décrit la manière de satisfaire aux conditions préalables en vue de la création et de l'utilisation de FileTables.  
  
##  <a name="enabling-the-prerequisites-for-filetable"></a><a name="EnablePrereq"></a> Activation des conditions préalables pour les FileTables  
 Pour activer les conditions préalables en vue de créer et d'utiliser les FileTables, activez les éléments suivants :  
  
-   **Au niveau de l'instance :**  
  
    -   [Activation de FILESTREAM au niveau de l'instance](#BasicsFilestream)  
  
-   **Au niveau de la base de données :**  
  
    -   [Fournir un groupe de fichiers FILESTREAM au niveau de la base de données](#filegroup)  
  
    -   [Activation de l'accès non transactionnel au niveau de la base de données](#BasicsNTAccess)  
  
    -   [Spécification d'un répertoire pour les FileTables au niveau de la base de données](#BasicsDirectory)  
  
##  <a name="enabling-filestream-at-the-instance-level"></a><a name="BasicsFilestream"></a> Activation de FILESTREAM au niveau de l'instance  
 Les FileTables étendent les fonctionnalités FILESTREAM de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Par conséquent, vous devez activer FILESTREAM pour l'accès d'E/S de fichier au niveau de Windows et sur l'instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] avant de pouvoir créer et utiliser des FileTables.  
  
###  <a name="how-to-enable-filestream-at-the-instance-level"></a><a name="HowToFilestream"></a> Procédure : activer FILESTREAM au niveau de l’instance  
 Pour plus d’informations sur l’activation de FILESTREAM, consultez [Activer et configurer FILESTREAM](../../relational-databases/blob/enable-and-configure-filestream.md).  
  
 Quand vous appelez **sp_configure** pour activer FILESTREAM au niveau de l’instance, vous devez affecter à l’option filestream_access_level la valeur 2. Pour plus d’informations, consultez [Niveau d’accès du flux de fichier (option de configuration de serveur)](../../database-engine/configure-windows/filestream-access-level-server-configuration-option.md).  
  
###  <a name="how-to-allow-filestream-through-the-firewall"></a><a name="firewall"></a> Procédure : activer FILESTREAM via le pare-feu  
 Pour plus d'informations sur l'activation de FILESTREAM via le pare-feu, consultez [Configure a Firewall for FILESTREAM Access](../../relational-databases/blob/configure-a-firewall-for-filestream-access.md).  
  
##  <a name="providing-a-filestream-filegroup-at-the-database-level"></a><a name="filegroup"></a> Fournir un groupe de fichiers FILESTREAM au niveau de la base de données  
 Avant de pouvoir créer des FileTables dans une base de données, cette dernière doit avoir un groupe de fichiers FILESTREAM. Pour plus d’informations sur cette condition préalable, consultez [Créer une base de données compatible FILESTREAM](../../relational-databases/blob/create-a-filestream-enabled-database.md).  
  
##  <a name="enabling-non-transactional-access-at-the-database-level"></a><a name="BasicsNTAccess"></a> Activation de l'accès non transactionnel au niveau de la base de données  
 Les FileTables permettent aux applications Windows d'obtenir un descripteur de fichier Windows aux données FILESTREAM sans requérir de transaction. Pour autoriser cet accès non transactionnel aux fichiers stockés dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], vous devez spécifier le niveau désiré d'accès non transactionnel au niveau de la base de données pour chaque base de données qui contiendra des FileTables.  
  
###  <a name="how-to-check-whether-non-transactional-access-is-enabled-on-databases"></a><a name="HowToCheckAccess"></a> Procédure : vérifier si l’accès non transactionnel est activé sur les bases de données  
 Interrogez l’affichage catalogue [sys.database_filestream_options &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-database-filestream-options-transact-sql.md) et vérifiez les colonnes **non_transacted_access** et **non_transacted_access_desc**.  

```sql
SELECT DB_NAME(database_id), non_transacted_access, non_transacted_access_desc  
    FROM sys.database_filestream_options;  
GO  
```

###  <a name="how-to-enable-non-transactional-access-at-the-database-level"></a><a name="HowToNTAccess"></a> Procédure : activer l’accès non transactionnel au niveau de la base de données  
 Les niveaux disponibles de l'accès non transactionnel sont FULL, READ_ONLY et OFF.  
  
 **Spécifier le niveau d'accès non transactionnel à l'aide de Transact-SQL**  
 - Quand vous **créez une base de données**, appelez l’instruction [CREATE DATABASE &#40;SQL Server Transact-SQL&#41;](../../t-sql/statements/create-database-transact-sql.md) avec l’option FILESTREAM **NON_TRANSACTED_ACCESS**.

   ```sql
   CREATE DATABASE database_name  
     WITH FILESTREAM ( NON_TRANSACTED_ACCESS = FULL, DIRECTORY_NAME = N'directory_name' )  
   ```

- Quand vous **modifiez une base de données existante**, appelez l’instruction [ALTER DATABASE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql.md) avec l’option FILESTREAM **NON_TRANSACTED_ACCESS**.

   ```sql
   ALTER DATABASE database_name  
     SET FILESTREAM ( NON_TRANSACTED_ACCESS = FULL, DIRECTORY_NAME = N'directory_name' )  
   ```

 **Spécifier le niveau d'accès non transactionnel à l'aide de SQL Server Management Studio**  
 Vous pouvez spécifier le niveau d’accès non transactionnel dans le champ **Accès non transactionnel FILESTREAM** de la page **Options** de la boîte de dialogue **Propriétés de la base de données** . Pour plus d’informations sur cette boîte de dialogue, consultez [Propriétés de la base de données &#40;page Options&#41;](../../relational-databases/databases/database-properties-options-page.md).  
  
##  <a name="specifying-a-directory-for-filetables-at-the-database-level"></a><a name="BasicsDirectory"></a> Spécification d'un répertoire pour les FileTables au niveau de la base de données  
 Lorsque vous activez l’accès non transactionnel aux fichiers au niveau de la base de données, vous pouvez éventuellement fournir en même temps un nom de répertoire à l’aide de l’option **DIRECTORY_NAME** . Si vous ne fournissez pas de nom de répertoire lorsque vous activez l'accès non transactionnel, vous devez le fournir ultérieurement avant de pouvoir créer des FileTables dans la base de données.  
  
 Dans l'arborescence du dossier FileTable, ce répertoire de base de données devient l'enfant du nom de partage spécifié pour FILESTREAM au niveau de l'instance, et le parent des FileTables créés dans la base de données. Pour plus d'informations, consultez [Work with Directories and Paths in FileTables](../../relational-databases/blob/work-with-directories-and-paths-in-filetables.md).  
  
###  <a name="how-to-specify-a-directory-for-filetables-at-the-database-level"></a><a name="HowToDirectory"></a> Procédure : spécifier un répertoire pour les FileTables au niveau de la base de données  
 Le nom que vous spécifiez doit être unique dans l'instance pour les répertoires de niveau base de données.  
  
**Spécifier un répertoire pour les FileTables à l'aide de Transact-SQL**  
- Quand vous **créez une base de données**, appelez l’instruction [CREATE DATABASE &#40;SQL Server Transact-SQL&#41;](../../t-sql/statements/create-database-transact-sql.md) avec l’option FILESTREAM **DIRECTORY_NAME**.

   ```sql
   CREATE DATABASE database_name  
     WITH FILESTREAM ( NON_TRANSACTED_ACCESS = FULL, DIRECTORY_NAME = N'directory_name' );  
   GO  
   ```

-   Quand vous **modifiez une base de données existante**, appelez l’instruction [ALTER DATABASE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql.md) avec l’option FILESTREAM **DIRECTORY_NAME**. Lorsque vous utilisez ces options pour modifier le nom de répertoire, la base de données doit être verrouillée en mode exclusif, sans descripteurs de fichiers ouverts.  
  
    ```sql  
    ALTER DATABASE database_name  
        SET FILESTREAM ( NON_TRANSACTED_ACCESS = FULL, DIRECTORY_NAME = N'directory_name' );  
    GO  
    ```  
  
-   Quand vous **attachez une base de données**, appelez l’instruction [CREATE DATABASE &#40;SQL Server Transact-SQL&#41;](../../t-sql/statements/create-database-transact-sql.md) avec l’option **FOR ATTACH** et l’option FILESTREAM **DIRECTORY_NAME**.  
  
    ```sql  
    CREATE DATABASE database_name  
        FOR ATTACH WITH FILESTREAM ( DIRECTORY_NAME = N'directory_name' );  
    GO  
    ```  
  
-   Quand vous **restaurez une base de données**, appelez l’instruction [RESTORE &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-transact-sql.md) avec l’option FILESTREAM **DIRECTORY_NAME**.  
  
    ```sql  
    RESTORE DATABASE database_name  
        WITH FILESTREAM ( DIRECTORY_NAME = N'directory_name' );  
    GO  
    ```  
  
 **Spécifier un répertoire pour les FileTables à l'aide de SQL Server Management Studio**  
 Vous pouvez spécifier un nom de répertoire dans le champ **Nom du répertoire FILESTREAM** de la page **Options** de la boîte de dialogue **Propriétés de la base de données** . Pour plus d’informations sur cette boîte de dialogue, consultez [Propriétés de la base de données &#40;page Options&#41;](../../relational-databases/databases/database-properties-options-page.md).  
  
###  <a name="how-to-view-existing-directory-names-for-the-instance"></a><a name="viewnames"></a> Procédure : afficher les noms de répertoires existants pour l’instance  
 Pour afficher la liste des noms de répertoire existant pour l’instance, interrogez l’affichage catalogue [sys.database_filestream_options &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-database-filestream-options-transact-sql.md) et vérifiez la colonne **filestream_database_directory_name**.  
  
```sql  
SELECT DB_NAME ( database_id ), directory_name  
    FROM sys.database_filestream_options;  
GO  
```  
  
###  <a name="requirements-and-restrictions-for-the-database-level-directory"></a><a name="ReqDirectory"></a> Exigences et restrictions pour le répertoire de niveau base de données  
  
-   La définition de **DIRECTORY_NAME** est facultative lorsque vous appelez **CREATE DATABASE** ou **ALTER DATABASE**. Si vous ne spécifiez pas de valeur pour **DIRECTORY_NAME**, le nom de répertoire reste Null. Cependant, vous ne pouvez pas créer de FileTables dans la base de données tant que vous n’avez pas spécifié de valeur pour **DIRECTORY_NAME** au niveau de la base de données.  
  
-   Le nom de répertoire que vous fournissez doit se conformer aux configurations requises du système de fichiers concernant les noms de répertoire valides.  
  
-   Quand la base de données contient des FileTables, vous ne pouvez pas redéfinir **DIRECTORY_NAME** sur une valeur Null.  
  
-   Lorsque vous attachez ou restaurez une base de données, l’opération échoue si la nouvelle base de données comporte une valeur pour **DIRECTORY_NAME** qui existe déjà dans l’instance cible. Spécifiez une valeur unique pour **DIRECTORY_NAME** lorsque vous appelez **CREATE DATABASE FOR ATTACH** ou **RESTORE DATABASE**.  
  
-   Lorsque vous mettez à niveau une base de données existante, la valeur de **DIRECTORY_NAME** est Null.  
  
-   Lorsque vous activez ou désactivez l'accès non transactionnel au niveau de la base de données, l'opération ne vérifie pas si le nom de répertoire a été spécifié ou s'il est unique.  
  
-   Lorsque vous supprimez une base de données activée pour les FileTables, le répertoire de niveau base de données et toutes les structures de répertoires de tous les FileTables situés au niveau inférieur sont supprimés.  
  
