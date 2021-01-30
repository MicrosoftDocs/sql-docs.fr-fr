---
description: GetPathLocator (Transact-SQL)
title: Fonction getpathlocator (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- GetPathLocator
- GetPathLocator_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- GetPathLocator function
ms.assetid: 78b7e220-445b-4fdf-811b-7253f4f2b058
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: fbdf65d45374fe9c85adaade284ac6a8aab65dbf
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99207455"
---
# <a name="getpathlocator-transact-sql"></a>GetPathLocator (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Retourne la valeur d'ID de localisateur de chemin d'accès pour le fichier ou le répertoire spécifié d'un FileTable.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
GetPathLocator(filenamespace_path)  
```  
  
## <a name="arguments"></a>Arguments  
 *filenamespace_path*  
 Chemin d'accès à l'espace de noms dans le FileTable. Le chemin d’accès de l’espace de noms est de type **nvarchar (max)**.  
  
 Lorsque la base de données appartient à un groupe de disponibilité Always On, la fonction **fonction getpathlocator** accepte le nom du réseau virtuel (VNN) ou le nom de l’ordinateur.  
  
## <a name="return-type"></a>Type de retour  
 **hierarchyid**  
  
## <a name="general-remarks"></a>Remarques d'ordre général  
 Pour plus d'informations, consultez [Work with Directories and Paths in FileTables](../../relational-databases/blob/work-with-directories-and-paths-in-filetables.md).  
  
## <a name="examples"></a>Exemples  
 Vous pouvez utiliser la fonction **fonction getpathlocator** lors de la migration de fichiers à partir d’un serveur de fichiers vers un filetable. Dans ce cas, vous souhaitez déplacer les fichiers dans le FileTable, puis remplacer le chemin d'accès UNC d'origine de chaque fichier par le chemin d'accès UNC de FileTable. Pour obtenir un exemple complet, consultez [charger des fichiers dans FileTables](../../relational-databases/blob/load-files-into-filetables.md).  
  
## <a name="see-also"></a>Voir aussi  
 [Travailler avec des répertoires et des chemins d’accès dans des FileTables](../../relational-databases/blob/work-with-directories-and-paths-in-filetables.md)  
  
  
