---
description: sys. dm_os_enumerate_fixed_drives (Transact-SQL)
title: sys. dm_os_enumerate_fixed_drives (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 09/18/2019
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.dm_os_enumerate_fixed_drives
- sys.dm_os_enumerate_fixed_drives_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_os_enumerate_fixed_drives dynamic management view
ms.assetid: 2e27489e-cf69-4a89-9036-77723ac3de66
author: markingmyname
ms.author: maghan
ms.openlocfilehash: c35d783b8db1abe5803a34dd1a4c401444897207
ms.sourcegitcommit: dd36d1cbe32cd5a65c6638e8f252b0bd8145e165
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/08/2020
ms.locfileid: "89539308"
---
# <a name="sysdm_os_enumerate_fixed_drives-transact-sql"></a>sys. dm_os_enumerate_fixed_drives (Transact-SQL)

[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

Introduite dans SQL Server 2019.

Énumère les volumes montés sur des lettres de lecteur, par exemple `C:\` .

|Nom de la colonne|Type de données|Description|
|-----------------|---------------|-----------------|  
|`fixed_drive_path`|`nvarchar(512)`|Chemin d’accès au volume, par exemple `C:\` .|  
|`drive_type`|`int`|Code pour le type de lecteur. Consultez [ `GetDriveTypeW` Function](/windows/win32/api/fileapi/nf-fileapi-getdrivetypew).|
|`drive_type_desc`|`nvarchar(512)`|Description du type de lecteur. Consultez [ `GetDriveTypeW` Function](/windows/win32/api/fileapi/nf-fileapi-getdrivetypew).|
|`free_space_in_bytes`|`bigint`|Espace libre du disque en octets.|

## <a name="permissions"></a>Autorisations

L’utilisateur doit disposer `VIEW SERVER STATE` de l’autorisation sur le serveur.

## <a name="see-also"></a>Voir aussi  

 [Fonctions et vues de gestion dynamique &#40;Transact-SQL&#41;](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
 [Fonctions et vues de gestion dynamique liées aux e/s &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/i-o-related-dynamic-management-views-and-functions-transact-sql.md)  
