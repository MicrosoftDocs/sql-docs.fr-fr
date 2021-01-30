---
description: sp_OAGetProperty (Transact-SQL)
title: sp_OAGetProperty (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_OAGetProperty_TSQL
- sp_OAGetProperty
dev_langs:
- TSQL
helpviewer_keywords:
- sp_OAGetProperty
ms.assetid: 240eeeb9-6d8b-4930-b912-1d273ca0ab38
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 0542ae7cf67fb5b05c63a4c8cdb9ef9fb31c62e2
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99174404"
---
# <a name="sp_oagetproperty-transact-sql"></a>sp_OAGetProperty (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Donne une valeur de propriété d'un objet OLE.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_OAGetProperty objecttoken , propertyname   
    [ , propertyvalue OUTPUT ]  
    [ , index...]   
```  
  
## <a name="arguments"></a>Arguments  
 *jeton_d'*  
 Est le jeton d’objet d’un objet OLE créé précédemment à l’aide de **sp_OACreate**.  
  
 *propertyname*  
 Nom de la propriété de l'objet OLE à renvoyer.  
  
  **sortie** PropertyValue  
 Valeur de la propriété renvoyée. Si elle est spécifiée, il doit s'agir d'une variable locale du type de données approprié.  
  
 Si la propriété retourne un objet OLE, *PropertyValue* doit être une variable locale de type de données **int**. Un jeton d’objet est stocké dans la variable locale, et ce jeton d’objet peut être utilisé avec d’autres procédures stockées OLE Automation.  
  
 Si la propriété retourne une valeur unique, spécifiez une variable locale pour *PropertyValue*, qui retourne la valeur de la propriété dans la variable locale ; ou ne spécifiez pas *PropertyValue*, qui retourne la valeur de la propriété au client sous la forme d’un jeu de résultats à une seule colonne et une seule ligne.  
  
 Lorsque la propriété retourne un tableau, si *PropertyValue* est spécifié, sa valeur est null.  
  
 Si *PropertyValue* est spécifié, mais que la propriété ne retourne pas de valeur, une erreur se produit. Si la propriété renvoie un tableau qui comprend plus de deux dimensions, une erreur se produit.  
  
 *index*  
 Paramètre d'index. S’il est spécifié, l' *index* doit être une valeur du type de données approprié.  
  
 Certaines propriétés possèdent des paramètres. Elles sont désignées sous le nom de propriétés indexées, et les paramètres s'appellent des paramètres d'index. Une propriété peut posséder plusieurs paramètres d'index.  
  
> [!NOTE]  
>  Les paramètres pour cette procédure stockée sont spécifiés par position et non pas par nom.  
  
## <a name="return-code-values"></a>Codet de retour  
 0 (succès) ou un nombre différent de zéro (échec), qui représente la valeur entière de HRESULT renvoyée par l'objet OLE Automation.  
  
 Pour plus d’informations sur les codes de retour HRESULT, consultez [codes de retour OLE Automation et informations sur les erreurs](../../relational-databases/stored-procedures/ole-automation-return-codes-and-error-information.md).  
  
## <a name="result-sets"></a>Jeux de résultats  
 Si la propriété renvoie un tableau à une ou deux dimensions, le tableau sera renvoyé au client sous la forme d'un jeu de résultats :  
  
-   Un tableau à une dimension est retourné au client sous la forme d'un jeu de résultats d'une seule ligne avec autant de colonnes qu'il y a d'éléments dans le tableau. Autrement dit, le tableau est renvoyé sous forme de colonnes.  
  
-   Un tableau à deux dimensions est retourné au client sous la forme d'un jeu de résultats qui contient autant de colonnes qu'il y a d'éléments dans la première dimension du tableau et autant de lignes qu'il y a d'éléments dans la seconde dimension du tableau. Autrement dit, le tableau est renvoyé sous la forme (colonnes, lignes).  
  
 Quand une valeur de retour de propriété ou une valeur de retour de méthode est un tableau, **sp_OAGetProperty** ou **sp_OAMethod** retourne un jeu de résultats au client. (Les paramètres de sortie de la méthode ne peuvent pas être des tableaux). Ces procédures analysent toutes les valeurs de données dans le tableau pour déterminer les types de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et les longueurs de données appropriés à utiliser pour chaque colonne du jeu de résultats. Pour une colonne particulière, ces procédures utilisent le type de données et la longueur requis pour représenter toutes les valeurs des données de cette colonne.  
  
 Lorsque toutes les valeurs de données d'une colonne partagent le même type de données, ce type est utilisé pour toute la colonne. Lorsque les valeurs de données d'une colonne utilisent des types de données différents, le choix du type pour l'ensemble de la colonne se fait sur la base du tableau suivant.  
  
||int|float|money|DATETIME|varchar|NVARCHAR|  
|------|---------|-----------|-----------|--------------|-------------|--------------|  
|**int**|**int**|**float**|**money**|**varchar**|**varchar**|**nvarchar**|  
|**float**|**float**|**float**|**money**|**varchar**|**varchar**|**nvarchar**|  
|**money**|**money**|**money**|**money**|**varchar**|**varchar**|**nvarchar**|  
|**datetime**|**varchar**|**varchar**|**varchar**|**datetime**|**varchar**|**nvarchar**|  
|**varchar**|**varchar**|**varchar**|**varchar**|**varchar**|**varchar**|**nvarchar**|  
|**nvarchar**|**nvarchar**|**nvarchar**|**nvarchar**|**nvarchar**|**nvarchar**|**nvarchar**|  
  
## <a name="remarks"></a>Notes  
 Vous pouvez également utiliser **sp_OAMethod** pour obtenir une valeur de propriété.  
  
## <a name="permissions"></a>Autorisations  
 Requiert l’appartenance au rôle serveur fixe **sysadmin** ou l’autorisation EXECUTE directement sur cette procédure stockée. `Ole Automation Procedures` la configuration doit être **activée** pour pouvoir utiliser toute procédure système liée à OLE Automation.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-using-a-local-variable"></a>R. Utilisation d'une variable locale  
 L’exemple suivant obtient la `HostName` propriété (de l’objet **SqlServer** créé précédemment) et la stocke dans une variable locale.  
  
```  
DECLARE @property varchar(255);  
EXEC @hr = sp_OAGetProperty @object, 'HostName', @property OUT;  
IF @hr <> 0  
BEGIN  
   EXEC sp_OAGetErrorInfo @object  
    RETURN  
END  
PRINT @property;  
```  
  
### <a name="b-using-a-result-set"></a>B. Utilisation d'un jeu de résultats  
 L’exemple suivant obtient la `HostName` propriété (de l’objet **SqlServer** créé précédemment) et la renvoie au client sous la forme d’un jeu de résultats.  
  
```  
EXEC @hr = sp_OAGetProperty @object, 'HostName';  
IF @hr <> 0  
BEGIN  
   EXEC sp_OAGetErrorInfo @object  
    RETURN  
END;  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Procédures stockées OLE Automation &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/ole-automation-stored-procedures-transact-sql.md)   
 [Exemple de script OLE Automation](../../relational-databases/stored-procedures/ole-automation-sample-script.md)  
  
  
