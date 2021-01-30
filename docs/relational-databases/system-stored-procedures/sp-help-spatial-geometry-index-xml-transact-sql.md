---
description: sp_help_spatial_geometry_index_xml (Transact-SQL)
title: sp_help_spatial_geometry_index_xml (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_help_spatial_geometry_index_xml_TSQL
- sp_help_spatial_geometry_index_xml
dev_langs:
- TSQL
helpviewer_keywords:
- sp_help_spatial_geometry_index_xml procedure
ms.assetid: 9668ae6d-9ed5-418e-bb9a-9e7b66f7dd16
author: markingmyname
ms.author: maghan
ms.openlocfilehash: f0630c37d00e6f9d8496d1fd0b0786987c46e96f
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99185395"
---
# <a name="sp_help_spatial_geometry_index_xml-transact-sql"></a>sp_help_spatial_geometry_index_xml (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Retourne les noms et les valeurs d’un jeu spécifié de propriétés à propos d’un index spatial **Geometry** . Vous pouvez choisir de retourner un jeu principal de propriétés ou toutes les propriétés de l'index.  
  
 Les résultats sont retournés dans un fragment XML qui affiche le nom et la valeur des propriétés sélectionnées.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_help_spatial_geometry_index [ @tabname =] 'tabname'   
     [ , [ @indexname = ] 'indexname' ]   
     [ , [ @verboseoutput = ]'{ 0 | 1 }]   
     [ , [ @query_sample = ] 'query_sample' ]   
     [ ,.[ @xml_output = ] 'xml_output' ]   
```  
  
## <a name="arguments"></a>Arguments  
 Consultez [les arguments et les propriétés des procédures stockées d’index spatial](../../relational-databases/system-stored-procedures/spatial-index-stored-procedures-arguments-and-properties.md).  
  
## <a name="properties"></a>Propriétés  
 Consultez [les arguments et les propriétés des procédures stockées d’index spatial](../../relational-databases/system-stored-procedures/spatial-index-stored-procedures-arguments-and-properties.md).  
  
## <a name="permissions"></a>Autorisations  
 L’utilisateur doit être membre du rôle **public** . Nécessite une autorisation READ ACCESS sur le serveur et l'objet.  
  
## <a name="remarks"></a>Notes  
 Les propriétés qui contiennent des valeurs NULL ne sont pas incluses dans le jeu de retour XML.  
  
## <a name="example"></a>Exemple  
 L’exemple suivant utilise `sp_help_spatial_geometry_index_xml` pour étudier l’index spatial **SIndx_SpatialTable_geometry_col2** défini sur la table **geometry_col** pour l’exemple de requête donné dans **\@ QS**. Cet exemple retourne les propriétés principales de l'index spécifié dans un fragment XML qui affiche le nom et la valeur des propriétés sélectionnées.  
  
 Un [XQuery](../../xquery/xquery-basics.md) est ensuite exécuté sur le jeu de résultats, retournant une propriété spécifique.  
  
```  
DECLARE @qs geometry  
        ='POLYGON((-90.0 -180.0, -90.0 180.0, 90.0 180.0, 90.0 -180.0, -90.0 -180.0))';  
DECLARE @x xml;  
EXEC sp_help_spatial_geometry_index_xml 'geometry_col', 'SIndx_SpatialTable_geometry_col2', 0, @qs, @x output;  
SELECT @x.value('(/Primary_Filter_Efficiency/text())[1]', 'float');  
```  
  
 Comme pour [sp_help_spatial_geometry_index](../../relational-databases/system-stored-procedures/sp-help-spatial-geometry-index-transact-sql.md), cette procédure stockée fournit un accès par programme plus simple aux propriétés d’un index spatial et signale le jeu de résultats en XML.  
  
## <a name="requirements"></a>Spécifications  
  
## <a name="see-also"></a>Voir aussi  
 [Arguments et propriétés des procédures stockées d’index spatial](../../relational-databases/system-stored-procedures/spatial-index-stored-procedures-arguments-and-properties.md)   
 [Procédures stockées d’index spatial](./spatial-index-stored-procedures-arguments-and-properties.md)   
 [sp_help_spatial_geometry_index](../../relational-databases/system-stored-procedures/sp-help-spatial-geometry-index-transact-sql.md)   
 [Vue d’ensemble des index spatiaux](../../relational-databases/spatial/spatial-indexes-overview.md)   
 [Données spatiales &#40;SQL Server&#41;](../../relational-databases/spatial/spatial-data-sql-server.md)   
 [Notions de base de XQuery](../../xquery/xquery-basics.md)   
 [Informations de référence sur le langage XQuery](../../xquery/xquery-language-reference-sql-server.md)  
  
