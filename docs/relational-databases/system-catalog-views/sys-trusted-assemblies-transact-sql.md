---
description: sys.trusted_assemblies (Transact-SQL)
title: sys.trusted_assemblies (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/14/2017
ms.prod: sql
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- trusted_assemblies_TSQL
- trusted_assemblies
- sys.trusted_assemblies_TSQL
- sys.trusted_assemblies
dev_langs:
- TSQL
helpviewer_keywords:
- sys.trusted_assemblies
ms.assetid: ''
author: VanMSFT
ms.author: vanto
monikerRange: '>=sql-server-2017||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: f2b26f5e646d1997959ba4856a88463ae8b1b3ed
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99186153"
---
# <a name="systrusted_assemblies-transact-sql"></a>sys.trusted_assemblies (Transact-SQL)  
[!INCLUDE[SQL Server 2017](../../includes/applies-to-version/sqlserver2017.md)]

Contient une ligne pour chaque assembly approuvé pour le serveur.

 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  


|Nom de la colonne |Type de données |Description |
|--- |--- |--- |
|Hachage |varbinary(8000) |SHA2_512 hachage du contenu de l’assembly. |
|description |nvarchar(4000) |Description facultative définie par l’utilisateur de l’assembly. Microsoft recommande d’utiliser le nom canonique qui encode le nom simple, le numéro de version, la culture, la clé publique et l’architecture de l’assembly à approuver. Cette valeur identifie de façon unique l’assembly sur le côté common language runtime (CLR) et est identique à la valeur clr_name dans sys. Assemblies. |
|create_date |datetime2 |Date à laquelle l’assembly a été ajouté à la liste des assemblys approuvés. |
|created_by |nvarchar(128) |Nom de connexion du principal qui a ajouté l’assembly à la liste. |
| | | |

### <a name="permissions"></a>Autorisations  
 requièrent l'autorisation VIEW SERVER STATE sur le serveur.  
 
## <a name="remarks"></a>Notes  
Utilisez **[sys.sp_add_trusted_assembly](../../relational-databases/system-stored-procedures/sys-sp-add-trusted-assembly-transact-sql.md)** pour ajouter et **[sys.sp_drop_trusted_assembly](../../relational-databases/system-stored-procedures/sys-sp-drop-trusted-assembly-transact-sql.md)** pour supprimer des assemblys de `sys.trusted_assemblies` .

## <a name="see-also"></a>Voir aussi  
  [sys.sp_add_trusted_assembly](../../relational-databases/system-stored-procedures/sys-sp-add-trusted-assembly-transact-sql.md)  
  [sys.sp_drop_trusted_assembly](../../relational-databases/system-stored-procedures/sys-sp-drop-trusted-assembly-transact-sql.md)  
  [DROP ASSEMBLY &#40;Transact-SQL&#41;](../../t-sql/statements/drop-assembly-transact-sql.md)  
  [sys.assemblies](../../relational-databases/system-catalog-views/sys-assemblies-transact-sql.md)  
  [sys.dm_clr_loaded_assemblies](../../relational-databases/system-dynamic-management-views/sys-dm-clr-loaded-assemblies-transact-sql.md)  
