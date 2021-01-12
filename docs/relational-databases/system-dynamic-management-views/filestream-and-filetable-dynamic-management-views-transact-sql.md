---
description: Vues de gestion dynamique liées à Filestream et FileTable (Transact-SQL)
title: Vues de gestion dynamique FileStream et filetable (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
dev_langs:
- TSQL
helpviewer_keywords:
- FileTables [SQL Server], dynamic management views
ms.assetid: e50a135d-6644-42a4-a0df-1c7a2b722051
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 3af7a4d72a93d2c44f470f83f6a0010737d3adcc
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/11/2021
ms.locfileid: "98099029"
---
# <a name="filestream-and-filetable-dynamic-management-views-transact-sql"></a>Vues de gestion dynamique liées à Filestream et FileTable (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Cette section décrit les vues de gestion dynamique en rapport avec les fonctionnalités FILESTREAM et FileTable.  
  
## <a name="filestream-dynamic-management-views-and-functions"></a>Fonctions et vues de gestion dynamique en rapport avec Filestream  
 [sys.dm_filestream_file_io_handles &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-filestream-file-io-handles-transact-sql.md)  
 Affiche les descripteurs de fichiers transactionnels actuellement ouverts.  
  
 [sys.dm_filestream_file_io_requests &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-filestream-file-io-requests-transact-sql.md)  
 Affiche les requêtes de sortie de fichier et d'entrée de fichier actuelles.  
  
## <a name="filetable-dynamic-management-views-and-functions"></a>Fonctions et vues de gestion dynamique en rapport avec FileTable  
 [sys.dm_filestream_non_transacted_handles &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-filestream-non-transacted-handles-transact-sql.md)  
 Affiche les descripteurs de fichiers non transactionnels actuellement ouverts aux données FileTable.  

## <a name="see-also"></a>Voir aussi
[FileStream](../../relational-databases/blob/filestream-sql-server.md)
<br>[Filetables](../../relational-databases/blob/filetables-sql-server.md)
<br>[Vues de catalogue Filestream et FileTable (Transact-SQL)](../system-catalog-views/filestream-and-filetable-catalog-views-transact-sql.md)
<br>[Procédures stockées Filestream et FileTable (Transact-SQL)](../system-stored-procedures/filestream-and-filetable-system-stored-procedures.md)
  
