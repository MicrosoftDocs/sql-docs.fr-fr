---
description: sp_db_selective_xml_index (Transact-SQL)
title: sp_db_selective_xml_index (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
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
ms.openlocfilehash: afa61838ac7f5bdf24764564489882ae68a29af9
ms.sourcegitcommit: dd36d1cbe32cd5a65c6638e8f252b0bd8145e165
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/08/2020
ms.locfileid: "89541863"
---
# <a name="sp_db_selective_xml_index-transact-sql"></a>sp_db_selective_xml_index (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Active ou désactive la fonctionnalité d'index XML sélectif sur une base de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Appelée sans paramètres, la procédure stockée retourne 1 si l'index XML sélectif est activé sur une base de données particulière.  
  
> [!NOTE]  
>  Pour désactiver l’index XML sélectif à l’aide de cette procédure stockée, la base de données doit être placée en mode de récupération simple à l’aide des [options ALTER DATABASE SET &#40;commande Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql-set-options.md) .  
  
 ![Icône du lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
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
  
  
