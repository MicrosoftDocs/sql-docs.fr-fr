---
description: FileTableRootPath (Transact-SQL)
title: FileTableRootPath (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- FileTableRootPath_TSQL
- FileTableRootPath
dev_langs:
- TSQL
helpviewer_keywords:
- FileTableRootPath function
ms.assetid: 0cba908a-c85c-4b09-b16a-df1cb333c629
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: f08273adcf261ecfdafaa70793d50b31360f7e40
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/11/2021
ms.locfileid: "98093860"
---
# <a name="filetablerootpath-transact-sql"></a>FileTableRootPath (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Retourne le chemin d'accès UNC au niveau racine pour un FileTable spécifique ou pour la base de données actuelle.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
FileTableRootPath ( [ '[schema_name.]FileTable_name' ], @option )  
```  
  
## <a name="arguments"></a>Arguments  
 *FileTable_name*  
 Nom du FileTable. *FileTable_name* est de type **nvarchar**. Il s'agit d'un paramètre facultatif. La valeur par défaut est la base de données actuelle. La spécification de *schema_name* est également facultative. Vous pouvez passer NULL pour que *FileTable_name* utilise la valeur de paramètre par défaut  
  
 *\@option*  
 Expression entière qui définit comment le composant serveur du chemin d'accès doit être mis en forme. l' *\@ option* peut prendre l’une des valeurs suivantes :  
  
|Valeur|Description|  
|-----------|-----------------|  
|**0**|Retourne le nom de serveur converti au format NetBIOS, par exemple :<br /><br /> `\\SERVERNAME\MSSQLSERVER\MyDocumentDatabase`<br /><br /> Il s’agit de la valeur par défaut.|  
|**1**|Retourne le nom de serveur non converti, par exemple :<br /><br /> `\\ServerName\MSSQLSERVER\MyDocumentDatabase`|  
|**2**|Retourne le chemin d'accès complet du serveur, par exemple :<br /><br /> `\\ServerName.MyDomain.com\MSSQLSERVER\MyDocumentDatabase`|  
  
## <a name="return-type"></a>Type de retour  
 **nvarchar(4000)**  
  
 Lorsque la base de données appartient à un groupe de disponibilité Always On, la fonction **FileTableRootPath** retourne le nom du réseau virtuel (VNN) à la place du nom de l’ordinateur.  
  
## <a name="general-remarks"></a>Remarques d'ordre général  
 La fonction **FileTableRootPath** retourne la valeur NULL lorsque l’une des conditions suivantes est remplie :  
  
-   La valeur de *FileTable_name* n’est pas valide.  
  
-   L'appelant ne dispose pas d'autorisations suffisantes pour référencer la table spécifiée ou la base de données actuelle.  
  
-   L’option FILESTREAM de *database_directory* n’est pas définie pour la base de données actuelle.  
  
 Pour plus d'informations, consultez [Work with Directories and Paths in FileTables](../../relational-databases/blob/work-with-directories-and-paths-in-filetables.md).  
  
## <a name="best-practices"></a>Bonnes pratiques  
 Pour garder le code et les applications indépendantes de l'ordinateur actuel et de la base de données, évitez d'écrire du code qui contient des chemins d'accès de fichier absolus. Au lieu de cela, récupérez le chemin d’accès complet d’un fichier au moment de l’exécution en utilisant conjointement les fonctions **FileTableRootPath** et **GetFileNamespacePath** , comme indiqué dans l’exemple suivant. Par défaut, la fonction **GetFileNamespacePath** retourne le chemin relatif du fichier sous le chemin racine de la base de données.  
  
```sql  
USE MyDocumentDatabase;  
  
@root varchar(100)  
SELECT @root = FileTableRootPath();  
@fullPath = varchar(1000);  
  
SELECT @fullPath = @root + file_stream.GetFileNamespacePath()  
FROM DocumentStore  
WHERE Name = N'document.docx';  
```  
  
## <a name="security"></a>Sécurité  
  
### <a name="permissions"></a>Autorisations  
 La fonction **FileTableRootPath** requiert les éléments suivants :  
  
-   Autorisation SELECT sur le FileTable pour obtenir le chemin d'accès racine d'un FileTable spécifique.  
  
-   **db_datareader** ou une autorisation plus élevée pour obtenir le chemin d’accès racine de la base de données actuelle.  
  
## <a name="examples"></a>Exemples  
 Les exemples suivants montrent comment appeler la fonction **FileTableRootPath** .  
  
```  
USE MyDocumentDatabase;  
-- returns "\\MYSERVER\MSSQLSERVER\MyDocumentDatabase"  
SELECT FileTableRootPath();  
  
-- returns "\\MYSERVER\MSSQLSERVER\MyDocumentDatabase\MyFileTable"  
SELECT FileTableRootPath(N'dbo.MyFileTable');  
  
-- returns "\\MYSERVER\MSSQLSERVER\MyDocumentDatabase\MyFileTable"  
SELECT FileTableRootPath(N'MyFileTable');  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Travailler avec des répertoires et des chemins d’accès dans des FileTables](../../relational-databases/blob/work-with-directories-and-paths-in-filetables.md)  
  
  
