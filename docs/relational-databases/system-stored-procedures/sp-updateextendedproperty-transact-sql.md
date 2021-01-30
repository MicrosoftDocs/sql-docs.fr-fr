---
description: sp_updateextendedproperty (Transact-SQL)
title: sp_updateextendedproperty (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 04/12/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_updateextendedproperty_TSQL
- sp_updateextendedproperty
dev_langs:
- TSQL
helpviewer_keywords:
- sp_updateextendedproperty
ms.assetid: 7f02360f-cb9e-48b4-b75f-29b4bc9ea304
author: markingmyname
ms.author: maghan
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: a4ba9f50620dfb623edd3154600c956d1d168c16
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99189518"
---
# <a name="sp_updateextendedproperty-transact-sql"></a>sp_updateextendedproperty (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Met à jour la valeur d'une propriété étendue existante.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_updateextendedproperty  
    [ @name = ]{ 'property_name' }   
    [ , [ @value = ]{ 'value' }  
        [, [ @level0type = ]{ 'level0_object_type' }  
         , [ @level0name = ]{ 'level0_object_name' }  
              [, [ @level1type = ]{ 'level1_object_type' }  
               , [ @level1name = ]{ 'level1_object_name' }  
                     [, [ @level2type = ]{ 'level2_object_type' }  
                      , [ @level2name = ]{ 'level2_object_name' }  
                     ]  
              ]  
        ]  
    ]  
```  
  
## <a name="arguments"></a>Arguments  
 [ @name =] {'*property_name*'}  
 Nom de la propriété à mettre à jour. *property_name* est de **type sysname** et ne peut pas avoir la valeur null.  
  
 [ @value =] {'*valeur*'}  
 Valeur associée à la propriété. la *valeur* est **sql_variant**, avec NULL comme valeur par défaut. La taille de la *valeur* ne peut pas dépasser 7 500 octets.  
  
 [ @level0type =] {'*level0_object_type*'}  
 Type défini par l'utilisateur ou utilisateur. *level0_object_type* est de type **varchar (128)**, avec NULL comme valeur par défaut. Les entrées valides sont ASSEMBLy, CONTRACT, EVENT NOTIFICATION, GROUPE_DE_FICHIERS, MESSAGE TYPE, PARTITION FUNCTION, PARTITION SCHEMe, PLAN GUIDE, REMOTE SERVICE BINDING, ROUTE, SCHEMA, SERVICE, USER, TRIGGER, TYPE et NULL.  
  
> [!IMPORTANT]  
>  Les types de niveau 0 USER et TYPE seront éliminés dans une version ultérieure de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Évitez d'utiliser ces fonctionnalités dans une nouvelle tâche de développement et prévoyez de modifier les applications qui les utilisent actuellement. À la place de USER, utilisez SCHEMA en tant que type de niveau 0. Pour TYPE, utilisez SCHEMA comme type de niveau 0 et TYPE comme type de niveau 1.  
  
 [ @level0name =] {'*level0_object_name*'}  
 Nom du type d'objet de niveau 1 spécifié. *level0_object_name* est de **type sysname** , avec NULL comme valeur par défaut.  
  
 [ @level1type =] {'*level1_object_type*'}  
 Type d'objet de niveau 1. *level1_object_type* est de type **varchar (128)** avec NULL comme valeur par défaut. Les entrées valides sont AGGREGATE, DEFAULT, FUNCTION, LOGICAL FILE NAME, PROCEDURE, QUEUE, RULE, SYNONYM, TABLE_TYPE, TYPE, VIEW, XML SCHEMA COLLECTION et NULL.  
  
 [ @level1name =] {'*level1_object_name*'}  
 Nom du type d'objet de niveau 1 spécifié. *level1_object_name* est de **type sysname** , avec NULL comme valeur par défaut.  
  
 [ @level2type =] {'*level2_object_type*'}  
 Type d'objet de niveau 2. *level2_object_type* est de type **varchar (128)** avec NULL comme valeur par défaut. Les entrées valides sont COLUMN, CONSTRAINT, EVENT NOTIFICATION, INDEX, PARAMETER, TRIGGER et NULL.  
  
 [ @level2name =] {'*level2_object_name*'}  
 Nom du type d'objet de niveau 2 spécifié. *level2_object_name* est de **type sysname**, avec NULL comme valeur par défaut.  
  
## <a name="return-code-values"></a>Codet de retour  
 0 (réussite) ou 1 (échec)  
  
## <a name="remarks"></a>Notes  
 Dans le cadre de la spécification des propriétés étendues, les objets d’une [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] base de données sont classés en trois niveaux (0, 1 et 2). Le niveau 0 est le niveau le plus élevé et est composé d'objets relevant de l'étendue de la base de données. Les objets de niveau 1 figurent dans l'étendue du schéma ou de l'utilisateur tandis que les objets de niveau 2 se trouvent dans les objets de niveau 1. Vous pouvez définir des propriétés étendues pour les objets de tous ces niveaux. Les références à un objet d'un niveau donné doivent être qualifiées par les noms des objets de niveau supérieur possédant ou contenant l'objet en question.  
  
 Étant donné un *property_name* et une *valeur* valides, si tous les types et noms d’objet sont NULL, la propriété mise à jour appartient à la base de données active.  
  
## <a name="permissions"></a>Autorisations  
 Les membres des rôles de base de données fixes db_owner et db_ddladmin peuvent mettre à jour les propriétés étendues de n'importe quel objet, avec toutefois la restriction suivante : db_ddladmin ne peut pas ajouter de propriétés à la base de données elle-même, ni aux utilisateurs ou rôles.  
  
 Les utilisateurs peuvent mettre à jour les propriétés étendues des objets qu'ils possèdent ou pour lesquels ils disposent d'autorisations ALTER ou CONTROL.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-updating-an-extended-property-on-a-column"></a>R. Mise à jour d'une propriété étendue sur une colonne  
 L'exemple suivant met à jour la valeur de la propriété `Caption` sur la colonne `ID` de la table `T1`.  
  
```  
USE AdventureWorks2012;  
GO  
CREATE TABLE T1 (id int , name char (20));  
GO  
EXEC sp_addextendedproperty   
    @name = N'Caption'  
    ,@value = N'Employee ID'  
    ,@level0type = N'Schema', @level0name = dbo  
    ,@level1type = N'Table',  @level1name = T1  
    ,@level2type = N'Column', @level2name = id;  
GO  
--Update the extended property.  
EXEC sp_updateextendedproperty   
    @name = N'Caption'  
    ,@value = 'Employee ID must be unique.'  
    ,@level0type = N'Schema', @level0name = dbo  
    ,@level1type = N'Table',  @level1name = T1  
    ,@level2type = N'Column', @level2name = id;  
GO  
```  
  
### <a name="b-updating-an-extended-property-on-a-database"></a>B. Mise à jour d'une propriété étendue sur une base de données  
 L'exemple suivant crée une propriété étendue sur l'exemple de base de données [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] puis met à jour la valeur de cette propriété.  
  
```  
USE AdventureWorks2012;  
GO  
EXEC sp_addextendedproperty   
@name = N'NewCaption', @value = 'AdventureWorks2012 Sample OLTP Database';  
GO  
USE AdventureWorks2012;  
GO  
EXEC sp_updateextendedproperty   
@name = N'NewCaption', @value = 'AdventureWorks2012 Sample Database';  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Moteur de base de données des procédures stockées &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/database-engine-stored-procedures-transact-sql.md)   
 [sys.fn_listextendedproperty &#40;Transact-SQL&#41;](../../relational-databases/system-functions/sys-fn-listextendedproperty-transact-sql.md)   
 [sp_addextendedproperty &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addextendedproperty-transact-sql.md)   
 [sp_dropextendedproperty &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-dropextendedproperty-transact-sql.md)   
 [sys.extended_properties &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/extended-properties-catalog-views-sys-extended-properties.md)  
  
  
