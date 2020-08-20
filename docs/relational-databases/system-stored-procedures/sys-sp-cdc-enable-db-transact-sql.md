---
description: sys.sp_cdc_enable_db (Transact-SQL)
title: sys. sp_cdc_enable_db (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sp_cdc_enable_db_TSQL
- sp_cdc_enable_db
- sys.sp_cdc_enable_db
- sys.sp_cdc_enable_db_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.sp_cdc_enable_db
- change data capture [SQL Server], enabling databases
- sp_cdc_enable_db
ms.assetid: 176d83b3-493d-43cd-800e-aa123c3bdf17
author: CarlRabeler
ms.author: carlrab
ms.openlocfilehash: 810e39bf9c4ff4626bf957978a12d77df6e60d54
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/17/2020
ms.locfileid: "88485505"
---
# <a name="syssp_cdc_enable_db-transact-sql"></a>sys.sp_cdc_enable_db (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Active la capture de données modifiées pour la base de données actuelle. Cette procédure doit être exécutée pour une base de données afin que des tables puissent être activées pour la capture de données modifiées dans cette base de données. Les enregistrements de capture de données modifiées insèrent, mettent à jour et suppriment l'activité appliquée aux tables activées, en rendant les détails des modifications disponibles dans un format relationnel simple à utiliser. Les informations sur la colonne qui reflètent la structure de colonne d'une table source suivie sont capturées pour les lignes modifiées, avec les métadonnées nécessaires à l'application des modifications à un environnement cible.  
  
> [!IMPORTANT]
>  La capture des modifications de données n’est pas disponible dans toutes les éditions de [!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Pour obtenir la liste des fonctionnalités prises en charge par les éditions de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], consultez [Fonctionnalités prise en charge par les éditions de SQL Server 2016](~/sql-server/editions-and-supported-features-for-sql-server-2016.md).  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sys.sp_cdc_enable_db  
```  
  
## <a name="return-code-values"></a>Codet de retour  
 0 (réussite) ou 1 (échec)  
  
## <a name="result-sets"></a>Jeux de résultats  
 None  
  
## <a name="remarks"></a>Notes  
 La capture de données modifiées ne peut pas être activée sur les bases [de données système](../../relational-databases/databases/system-databases.md) ou les bases de données de distribution.  
  
 sys.sp_cdc_enable_db crée les objets de capture de données modifiées qui ont une portée à l'échelle de la base de données, y compris les tables de métadonnées et les déclencheurs DDL. Il crée également le schéma CDC et l’utilisateur de la base de données CDC et définit la colonne is_cdc_enabled pour l’entrée de la base de données dans l’affichage catalogue [sys. databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md) sur 1.  
  
## <a name="permissions"></a>Autorisations  
 Nécessite l'appartenance au rôle serveur fixe sysadmin.  
  
## <a name="examples"></a>Exemples  
 L'exemple suivant active la capture des données modifiées.  
  
```  
USE AdventureWorks2012;  
GO  
EXECUTE sys.sp_cdc_enable_db;  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [sys. sp_cdc_disable_db &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sys-sp-cdc-disable-db-transact-sql.md)  
  
  
