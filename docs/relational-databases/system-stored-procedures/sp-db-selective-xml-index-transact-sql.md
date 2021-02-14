---
description: sp_db_selective_xml_index (Transact-SQL)
title: sp_db_selective_xml_index (Transact-SQL)
ms.custom: ''
ms.date: 02/11/2021
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
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 5df370a674026b4c6ebc7eb59985505821e3b028
ms.sourcegitcommit: e8c0c04eb7009a50cbd3e649c9e1b4365e8994eb
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/14/2021
ms.locfileid: "100489443"
---
# <a name="sp_db_selective_xml_index-transact-sql"></a>sp_db_selective_xml_index (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Active ou désactive la fonctionnalité d'index XML sélectif sur une base de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Appelée sans paramètres, la procédure stockée retourne 1 si l'index XML sélectif est activé sur une base de données particulière.  
  
> [!NOTE]  
>  Pour désactiver l’index XML sélectif à l’aide de cette procédure stockée, la base de données doit être placée en mode de récupération simple à l’aide des [options ALTER DATABASE SET &#40;commande Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql-set-options.md) .  
  
 ![Icône du lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql
  
      sys.sp_db_selective_xml_index[[ @dbname = ] 'dbname'],   
[[ @selective_xml_index = ] 'selective_xml_index']  
```  
  
## <a name="arguments"></a>Arguments  
`[ @ dbname = ] 'dbname'` Nom de la base de données sur laquelle activer ou désactiver l’index XML sélectif. Si *dbname* a la valeur null, la base de données active est utilisée par défaut. *@dbname* est de **type sysname**.


`[ @selective_xml_index = ] 'selective_xml_index'` Détermine s’il faut activer ou désactiver l’index. Valeurs autorisées : 'on', 'OFF', 'true', 'false'. Si une autre valeur à l’exception de’on', 'true', 'OFF’ou’false’est passée, une erreur est générée. *@selective_xml_index* est **de type varchar (6)**.

  
## <a name="return-code-values"></a>Codet de retour  
 **1** si l’index XML sélectif est activé sur une base de données particulière, **0** s’il est désactivé.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-enable-selective-xml-index-functionality"></a>R. Activer la fonctionnalité d'index XML sélectif  
 L'exemple suivant active l'index XML sélectif sur la base de données active.  
  
```sql
EXECUTE sys.sp_db_selective_xml_index  
    @dbname = NULL  
  , @selective_xml_index = N'on';  
GO  
```  
  
 L'exemple suivant active l'index XML sélectif sur la base de données AdventureWorks2012.  
  
```sql
EXECUTE sys.sp_db_selective_xml_index  
    @dbname = N'AdventureWorks2012'  
  , @selective_xml_index = N'true';  
GO  
```  
  
### <a name="b-disable-selective-xml-index-functionality"></a>B. Désactiver la fonctionnalité d'index XML sélectif  
 L'exemple suivant désactive l'index XML sélectif sur la base de données active.  
  
```sql
EXECUTE sys.sp_db_selective_xml_index  
    @dbname = NULL  
  , @selective_xml_index = N'off';  
GO  
```  
  
 L'exemple suivant désactive l'index XML sélectif sur la base de données AdventureWorks2012.  
  
```sql
EXECUTE sys.sp_db_selective_xml_index  
    @dbname = N'AdventureWorks2012'  
  , @selective_xml_index = N'false';  
GO  
```  
  
### <a name="c-detect-if-selective-xml-index-is-enabled"></a>C. Détecter si l'index XML sélectif est activé  
 L'exemple suivant détecte si l'index XML sélectif est activé. Retourne 1 si l'index XML sélectif est activé.  
  
```sql
EXECUTE sys.sp_db_selective_xml_index;  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Index XML sélectifs &#40;SXI&#41;](../../relational-databases/xml/selective-xml-indexes-sxi.md)  
   