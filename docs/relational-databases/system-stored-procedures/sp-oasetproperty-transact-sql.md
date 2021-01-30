---
description: sp_OASetProperty (Transact-SQL)
title: sp_OASetProperty (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_OASetProperty
- sp_OASetProperty_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_OASetProperty
ms.assetid: 0fe7d554-6b67-4d55-9d3e-4096802c47f8
author: markingmyname
ms.author: maghan
ms.openlocfilehash: ff901b8f3aa7ad7c31680b8664fb753f9873cadc
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99195415"
---
# <a name="sp_oasetproperty-transact-sql"></a>sp_OASetProperty (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Affecte une nouvelle valeur à une propriété d'un objet OLE.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_OASetProperty objecttoken , propertyname , newvalue [ , index... ]  
```  
  
## <a name="arguments"></a>Arguments  
 *jeton_d'*  
 Est le jeton d’objet d’un objet OLE précédemment créé par **sp_OACreate**.  
  
 *propertyname*  
 Nom de la propriété de l'objet OLE à laquelle une nouvelle valeur doit être affectée.  
  
 *NewValue*  
 Nouvelle valeur de la propriété, ce doit être une valeur du type de données approprié.  
  
 *index*  
 Paramètre d'index. S’il est spécifié, l' *index* doit être une valeur du type de données approprié.  
  
 Certaines propriétés possèdent des paramètres. Elles sont désignées sous le nom de propriétés indexées, et les paramètres s'appellent des paramètres d'index. Une propriété peut posséder plusieurs paramètres d'index.  
  
> [!NOTE]  
>  Les paramètres pour cette procédure stockée sont spécifiés par position et non pas par nom.  
  
## <a name="return-code-values"></a>Codet de retour  
 0 (succès) ou un nombre différent de zéro (échec), qui représente la valeur entière de HRESULT renvoyée par l'objet OLE Automation.  
  
 Pour plus d’informations sur les codes de retour HRESULT, consultez [codes de retour OLE Automation et informations sur les erreurs](../../relational-databases/stored-procedures/ole-automation-return-codes-and-error-information.md).  
  
## <a name="permissions"></a>Autorisations  
 Requiert l’appartenance au rôle serveur fixe **sysadmin** ou l’autorisation EXECUTE directement sur cette procédure stockée. `Ole Automation Procedures` la configuration doit être **activée** pour pouvoir utiliser toute procédure système liée à OLE Automation.  
  
## <a name="examples"></a>Exemples  
 L’exemple suivant affecte `HostName` une nouvelle valeur à la propriété (de l’objet **SqlServer** créé précédemment).  
  
```  
EXEC @hr = sp_OASetProperty @object, 'HostName', 'Gizmo';  
IF @hr <> 0  
BEGIN  
   EXEC sp_OAGetErrorInfo @object  
    RETURN  
END'  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Procédures stockées OLE Automation &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/ole-automation-stored-procedures-transact-sql.md)   
 [Exemple de script OLE Automation](../../relational-databases/stored-procedures/ole-automation-sample-script.md)  
  
  
