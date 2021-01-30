---
description: sp_syscollector_update_collector_type (Transact-SQL)
title: sp_syscollector_update_collector_type (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_syscollector_update_collector_type_TSQL
- sp_syscollector_update_collector_type
dev_langs:
- TSQL
helpviewer_keywords:
- sp_syscollector_update_collector_type
- data collector [SQL Server], stored procedures
ms.assetid: 3c414dfd-d9ca-4320-81aa-949465b967bf
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 4ad2fefb206f417c4759f3e85d34a63f320e34d6
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99201869"
---
# <a name="sp_syscollector_update_collector_type-transact-sql"></a>sp_syscollector_update_collector_type (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Met à jour un type de collecteur pour un élément de collecte. Le nom et le GUID d'un type de collecteur étant fournis, met à jour la configuration du type de collecteur, y compris le package de collection et de téléchargement, le schéma de paramètres et le schéma du formateur de paramètres.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_syscollector_update_collector_type [ @collector_type_uid = ] 'collector_type_uid' OUTPUT  
          , [ @name = ] 'name'  
          , [ @parameter_schema = ] 'parameter_schema'  
          , [ @collection_package_id = ] collection_package_id  
          , [ @upload_package_id = ] upload_package_id  
```  
  
## <a name="arguments"></a>Arguments  
`[ @collector_type_uid = ] 'collector_type_uid'` GUID du type de collecteur. *collector_type_uid* est de type **uniqueidentifier** et, s’il a la valeur null, il sera automatiquement créé et retourné comme sortie.  
  
`[ @name = ] 'name'` Nom du type de collecteur. *Name* est de **type sysname** et doit être spécifié.  
  
`[ @parameter_schema = ] 'parameter_schema'` Est le schéma XML pour ce type de collecteur. *parameter_schema* est du **code XML** et peut être requis par certains types de collecteurs. S'il n'est pas requis, cet argument peut avoir la valeur NULL.  
  
`[ @collection_package_id = ] collection_package_id` Identificateur unique local qui pointe vers le [!INCLUDE[ssIS](../../includes/ssis-md.md)] package de collection utilisé par le jeu d’éléments de collecte. *collection_package_id* est **uniqueidentifer** et est obligatoire. Pour obtenir la valeur de *collection_package_id*, interrogez la vue système dbo.syscollector_collector_types dans la base de données msdb.  
  
`[ @upload_package_id = ] upload_package_id` Identificateur unique local qui pointe vers le [!INCLUDE[ssIS](../../includes/ssis-md.md)] package de téléchargement utilisé par le jeu d’éléments de collecte. *upload_package_id* est de type **uniqueidentifier** et est requis. Pour obtenir la valeur de *upload_package_id*, interrogez la vue système dbo.syscollector_collector_types dans la base de données msdb.  
  
## <a name="return-code-values"></a>Codet de retour  
 **0** (succès) ou **1** (échec)  
  
## <a name="permissions"></a>Autorisations  
 Requiert l’appartenance au rôle de base de données fixe **dc_admin** (avec autorisation EXECUTE).  
  
## <a name="example"></a>Exemple  
 Cet exemple met à jour le type de collecteur Requête T-SQL générique. (Dans l'exemple, le schéma par défaut du type de collecteur Requête T-SQL générique est utilisé.)  
  
```  
USE msdb;  
GO  
EXEC sp_syscollector_update_collector_type  
@collector_type_uid = '302E93D1-3424-4BE7-AA8E-84813ECF2419',  
@name = 'Generic T-SQL Query Collector Type',  
@parameter_schema = '<?xml version="1.0" encoding="utf-8"?>  
<xs:schema xmlns:xs="http://www.w3.org/2001/XMLSchema" targetNamespace="DataCollectorType">  
  <xs:element name="TSQLQueryCollector">  
<xs:complexType>  
  <xs:sequence>  
<xs:element name="Query" minOccurs="1" maxOccurs="unbounded">  
  <xs:complexType>  
<xs:sequence>  
  <xs:element name="Value" type="xs:string" />  
  <xs:element name="OutputTable" type="xs:string" />  
</xs:sequence>  
  </xs:complexType>  
</xs:element>  
<xs:element name="Databases" minOccurs="0" maxOccurs="1">  
  <xs:complexType>  
<xs:sequence>  
  <xs:element name="Database" minOccurs="0" maxOccurs="unbounded" type="xs:string" />  
</xs:sequence>  
<xs:attribute name="UseSystemDatabases" type="xs:boolean" use="optional" />  
<xs:attribute name="UseUserDatabases" type="xs:boolean" use="optional" />  
  </xs:complexType>  
</xs:element>  
  </xs:sequence>  
</xs:complexType>  
  </xs:element>  
</xs:schema>',  
@collection_package_id = '292B1476-0F46-4490-A9B7-6DB724DE3C0B',  
@upload_package_id = '6EB73801-39CF-489C-B682-497350C939F0';  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)   
 [Collecte de données](../../relational-databases/data-collection/data-collection.md)  
  
  
