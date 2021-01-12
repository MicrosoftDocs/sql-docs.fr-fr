---
description: Affichages catalogue FILESTREAM et FileTable (Transact-SQL)
title: Affichages catalogue FileStream et filetable (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
dev_langs:
- TSQL
helpviewer_keywords:
- FileTables [SQL Server], catalog views
ms.assetid: 2c83a4a7-720b-4435-a3b5-788c29f56949
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: f280208aa768997e63d0df2c9ee9fc4d8a079aca
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/11/2021
ms.locfileid: "98092599"
---
# <a name="filestream-and-filetable-catalog-views-transact-sql"></a>Affichages catalogue FILESTREAM et FileTable (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Cette section décrit les affichages catalogue relatifs à la fonction FileTable.  
  
## <a name="filestream-and-filetable-catalog-views-transact-sql"></a>Affichages catalogue FileStream et filetable (Transact-SQL)
 [sys.database_filestream_options &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-database-filestream-options-transact-sql.md)  
 Affiche des informations relatives au niveau d'accès non transactionnel aux données FILESTREAM dans les FileTables activés. Contient une ligne pour chaque base de données dans l'instance SQL Server.  
  
 [sys.filetable_system_defined_objects &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-filetable-system-defined-objects-transact-sql.md)  
 Affiche une liste des objets définis par le système en rapport avec les FileTables. Contient une ligne pour chaque objet défini par le système.  
  
 [sys.filetables &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-filetables-transact-sql.md)  
 Retourne une ligne pour chaque FileTable. Hérite de **sys. tables**.  

## <a name="see-also"></a>Voir aussi
[FileStream](../../relational-databases/blob/filestream-sql-server.md)
<br>[Filetables](../../relational-databases/blob/filetables-sql-server.md)
<br>[Vues de gestion dynamiques Filestream et FileTable (Transact-SQL)](../system-dynamic-management-views/filestream-and-filetable-dynamic-management-views-transact-sql.md)
<br>[Procédures stockées Filestream et FileTable (Transact-SQL)](../system-stored-procedures/filestream-and-filetable-system-stored-procedures.md)
  
  
