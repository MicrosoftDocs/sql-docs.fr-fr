---
description: Syntaxe de hiérarchie des objets (Transact-SQL)
title: Syntaxe des hiérarchies d’objets (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
dev_langs:
- TSQL
helpviewer_keywords:
- objects [SQL Server], hierarchy syntax
ms.assetid: 7ed8df86-9fd2-4e09-96bc-5381fec85f65
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 6ebfc2afdcbaa3ca5eddd5d311262ba19780e332
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99205259"
---
# <a name="object-hierarchy-syntax-transact-sql"></a>Syntaxe de hiérarchie des objets (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Le paramètre *PropertyName* de sp_OAGetProperty et sp_OASetProperty et le paramètre *MethodName* de sp_OAMethod prennent en charge une syntaxe de hiérarchie d’objets similaire à celle de [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[vbprvb](../../includes/vbprvb-md.md)] . Lorsque vous utilisez cette syntaxe spéciale, ces paramètres ont la forme générale suivante.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
'TraversedObject.PropertyOrMethod'  
```  
  
## <a name="arguments"></a>Arguments  
 *TraversedObject*  
 Objet OLE dans la hiérarchie sous le *jeton_d* 'objet spécifié dans la procédure stockée. Utilisez la syntaxe [!INCLUDE[vbprvb](../../includes/vbprvb-md.md)] pour spécifier une série de collections, de propriétés d'objet et de méthodes qui retournent des objets. Chaque identificateur d'objet de la série doit être séparé par un point (.).  
  
 Un élément dans la série peut être le nom d'une collection. Utilisez la syntaxe suivante pour indiquer une collection :  
  
 Collection («*Item*»)  
  
 Les guillemets doubles (") sont obligatoires. La syntaxe avec point d'exclamation (!) utilisée par [!INCLUDE[vbprvb](../../includes/vbprvb-md.md)] pour les collections n'est pas prise en charge.  
  
 *PropertyOrMethod*  
 Nom d’une propriété ou méthode du *TraversedObject*.  
  
 Pour spécifier tous les paramètres d'index ou de méthode à l'aide des paramètres sp_OAGetProperty, sp_OASetProperty ou sp_OAMethod (y compris la prise en charge des paramètres de sortie sp_OAMethod), utilisez la syntaxe suivante :  
  
 *PropertyOrMethod*  
  
 Pour spécifier tous les paramètres d'index ou de méthode à l'intérieur des parenthèses (avec pour résultat que tous les paramètres d'index ou de méthode de sp_OAGetProperty, sp_OASetProperty ou sp_OAMethod sont alors ignorés), utilisez la syntaxe suivante :  
  
 *PropertyOrMethod*([ *ParameterName*: =] "*paramètre*" [,...])  
  
 Les guillemets doubles (") sont obligatoires. Tous les paramètres nommés doivent être spécifiés après tous les paramètres positionnels.  
  
## <a name="remarks"></a>Notes  
 Si *TraversedObject* n’est pas spécifié, *PropertyOrMethod* est requis.  
  
 Si *PropertyOrMethod* n’est pas spécifié, *TraversedObject* est retourné en tant que paramètre de sortie de jeton d’objet à partir de la procédure stockée OLE Automation. Si *PropertyOrMethod* est spécifié, la propriété ou la méthode de *TraversedObject* est appelée, et la valeur de propriété ou la valeur de retour de la méthode est retournée en tant que paramètre de sortie à partir de la procédure stockée OLE Automation.  
  
 Si un élément de la liste *TraversedObject* ne retourne pas d’objet OLE, une erreur est générée.  
  
 Pour plus d'informations sur la syntaxe des objets [!INCLUDE[vbprvb](../../includes/vbprvb-md.md)], consultez la documentation de [!INCLUDE[vbprvb](../../includes/vbprvb-md.md)].  
  
 Pour plus d’informations sur les codes de retour HRESULT, consultez [sp_OACreate &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-oacreate-transact-sql.md).  
  
## <a name="examples"></a>Exemples  
 Voici des exemples de syntaxe de hiérarchie des objets qui utilisent un objet SQL-DMO SQLServer.  
  
```  
-- Get the AdventureWorks2012 Person.Address Table object.  
EXEC @hr = sp_OAGetProperty @object,  
   'Databases("AdventureWorks2012").Tables("Person.Address")',  
   @table OUT  
  
-- Get the Rows property of the AdventureWorks2012 Person.Address table.  
EXEC @hr = sp_OAGetProperty @object,  
   'Databases("AdventureWorks2012").Tables("Person.Address").Rows',  
   @rows OUT  
  
-- Call the CheckTable method to validate the   
-- AdventureWorks2012 Person.Address table.  
EXEC @hr = sp_OAMethod @object,  
   'Databases("AdventureWorks2012").Tables("Person.Address").CheckTable',  
   @checkoutput OUT  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Exemple de script OLE Automation](../../relational-databases/stored-procedures/ole-automation-sample-script.md)   
 [Procédures stockées OLE Automation &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/ole-automation-stored-procedures-transact-sql.md)  
  
  
