---
description: sp_db_selective_xml_index (Transact-SQL)
title: sp_db_selective_xml_index (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_db_selective_xml_index_TSQL
- sp_db_selective_xml_index
dev_langs:
- TSQL
helpviewer_keywords:
- sp_db_selective_xml_index procedure
ms.assetid: 017301a2-4a23-4e68-82af-134f3d4892b3
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 17942cc1e4c68c844e78a2f25398a7e175035d01
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99201322"
---
# <a name="sp_db_selective_xml_index-transact-sql"></a>sp_db_selective_xml_index (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Active ou désactive la fonctionnalité d'index XML sélectif sur une base de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Appelée sans paramètres, la procédure stockée retourne 1 si l'index XML sélectif est activé sur une base de données particulière.  
  
> [!NOTE]  
>  Pour désactiver l’index XML sélectif à l’aide de cette procédure stockée, la base de données doit être placée en mode de récupération simple à l’aide des [options ALTER DATABASE SET &#40;commande Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql-set-options.md) .  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```sql  
  
      sys.sp_db_selective_xml_index[[ @db_name = ] 'db_name'],   
[[ @action = ] 'action']  
```  
  
## <a name="arguments"></a>Arguments  
`[ @ db_name = ] 'db_name'` Nom de la base de données sur laquelle activer ou désactiver l’index XML sélectif. Si *db_name* a la valeur null, la base de données actuelle est utilisée par défaut.  
  
`[ @action = ] 'action'` Détermine s’il faut activer ou désactiver l’index. Si une autre valeur à l’exception de’on', 'true', 'OFF’ou’false’est passée, une erreur est générée.  
  
```  
  
Allowed values: 'on', 'off', 'true', 'false'  
```  
  
## <a name="return-code-values"></a>Codet de retour  
 **1** si l’index XML sélectif est activé sur une base de données particulière.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-enable-selective-xml-index-functionality"></a>R. Activer la fonctionnalité d'index XML sélectif  
 L'exemple suivant active l'index XML sélectif sur la base de données active.  
  
```  
EXECUTE sys.sp_db_selective_xml_index  
    @db_name = NULL  
  , @action = N'on';  
GO  
```  
  
 L'exemple suivant active l'index XML sélectif sur la base de données AdventureWorks2012.  
  
```  
EXECUTE sys.sp_db_selective_xml_index  
    @db_name = N'AdventureWorks2012'  
  , @action = N'true';  
GO  
```  
  
### <a name="b-disable-selective-xml-index-functionality"></a>B. Désactiver la fonctionnalité d'index XML sélectif  
 L'exemple suivant désactive l'index XML sélectif sur la base de données active.  
  
```  
EXECUTE sys.sp_db_selective_xml_index  
    @db_name = NULL  
  , @action = N'off';  
GO  
```  
  
 L'exemple suivant désactive l'index XML sélectif sur la base de données AdventureWorks2012.  
  
```  
EXECUTE sys.sp_db_selective_xml_index  
    @db_name = N'AdventureWorks2012'  
  , @action = N'false';  
GO  
```  
  
### <a name="c-detect-if-selective-xml-index-is-enabled"></a>C. Détecter si l'index XML sélectif est activé  
 L'exemple suivant détecte si l'index XML sélectif est activé. Retourne 1 si l'index XML sélectif est activé.  
  
```  
EXECUTE sys.sp_db_selective_xml_index;  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Index XML sélectifs &#40;SXI&#41;](../../relational-databases/xml/selective-xml-indexes-sxi.md)  
  
  
