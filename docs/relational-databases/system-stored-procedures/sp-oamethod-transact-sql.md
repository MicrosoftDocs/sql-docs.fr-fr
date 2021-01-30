---
description: sp_OAMethod (Transact-SQL)
title: sp_OAMethod (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_OAMethod
- sp_OAMethod_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_OAMethod
ms.assetid: 1dfaebe2-c7cf-4041-a586-5d04faf2e25e
author: markingmyname
ms.author: maghan
ms.openlocfilehash: da84a59353baffa92ac0fa30e55bfa4b248379f7
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99182410"
---
# <a name="sp_oamethod-transact-sql"></a>sp_OAMethod (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Appelle une méthode d'un objet OLE.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_OAMethod objecttoken , methodname  
    [ , returnvalue OUTPUT ]   
    [ , [ @parametername = ] parameter [ OUTPUT ] [ ...n ] ]   
```  
  
## <a name="arguments"></a>Arguments  
 *jeton_d'*  
 Est le jeton d’objet d’un objet OLE créé précédemment à l’aide de **sp_OACreate**.  
  
 *MethodName*  
 Nom de la méthode de l'objet OLE à appeler.  
  
 **sortie** returnValue    
 Valeur renvoyée de la méthode de l'objet OLE. Si elle est spécifiée, il doit s'agir d'une variable locale du type de données approprié.  
  
 Si la méthode retourne une valeur unique, spécifiez une variable locale pour *returnValue*, qui retourne la valeur de retour de la méthode dans la variable locale, ou ne spécifiez pas *returnValue*, qui retourne la valeur de retour de la méthode au client sous la forme d’un jeu de résultats à une seule colonne et une seule ligne.  
  
 Si la valeur de retour de la méthode est un objet OLE, le paramètre *returnValue* doit être une variable locale de type de données **int**. Un jeton d’objet est stocké dans la variable locale, et ce jeton d’objet peut être utilisé avec d’autres procédures stockées OLE Automation.  
  
 Lorsque la valeur de retour de la méthode est un tableau, si *returnValue* est spécifié, il prend la valeur null.  
  
 Une erreur est générée lorsqu'un des événements suivants survient :  
  
-   *returnValue* est spécifié, mais la méthode ne retourne pas de valeur.  
  
-   La méthode renvoie un tableau qui possède plus de deux dimensions.  
  
-   La méthode renvoie un tableau comme paramètre de sortie.  
  
`[ _@parametername = ] parameter[ OUTPUT ]` Est un paramètre de méthode. S’il est spécifié, le *paramètre* doit être une valeur du type de données approprié.  
  
 Pour obtenir la valeur de retour d’un paramètre de sortie, le *paramètre* doit être une variable locale du type de données approprié, et la **sortie** doit être spécifiée. Si un paramètre de constante est spécifié, ou si **Output** n’est pas spécifié, toute valeur de retour d’un paramètre de sortie est ignorée.  
  
 S’il est spécifié, *ParameterName* doit être le nom du [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[vbprvb](../../includes/vbprvb-md.md)] paramètre nommé. Notez que **@** _parametername_is pas une [!INCLUDE[tsql](../../includes/tsql-md.md)] variable locale. L’arobase ( **@** ) est supprimée et *ParameterName* est passé à l’objet OLE comme nom de paramètre. Tous les paramètres nommés doivent être spécifiés après tous les paramètres positionnels.  
  
 *n*  
 Marque de réservation indiquant que plusieurs paramètres peuvent être spécifiés.  
  
> [!NOTE]
>  *\@ ParameterName* peut être un paramètre nommé, car il fait partie de la méthode spécifiée et est passé à l’objet. Les autres paramètres pour cette procédure stockée sont spécifiés par position, et non par nom.  
  
## <a name="return-code-values"></a>Codet de retour  
 0 (succès) ou un nombre différent de zéro (échec), qui représente la valeur entière de HRESULT renvoyée par l'objet OLE Automation.  
  
 Pour plus d’informations sur les codes de retour HRESULT, [les codes de retour et les informations d’erreur OLE Automation](../../relational-databases/stored-procedures/ole-automation-return-codes-and-error-information.md).  
  
## <a name="result-sets"></a>Jeux de résultats  
 Si la valeur renvoyée par la méthode est un tableau à une ou deux dimensions, le tableau sera renvoyé au client sous la forme d'un jeu de résultats :  
  
-   Un tableau à une dimension est retourné au client sous la forme d'un jeu de résultats d'une seule ligne avec autant de colonnes qu'il y a d'éléments dans le tableau. En d’autres termes, le tableau est retourné en tant que (colonnes).  
  
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
  
### <a name="a-calling-a-method"></a>R. Appel d'une méthode  
 L’exemple suivant appelle la `Connect` méthode de l’objet **SqlServer** créé précédemment.  
  
```  
EXEC @hr = sp_OAMethod @object, 'Connect', NULL, 'my_server',  
    'my_login', 'my_password';  
IF @hr <> 0  
BEGIN  
   EXEC sp_OAGetErrorInfo @object  
    RETURN  
END;  
```  
  
### <a name="b-getting-a-property"></a>B. Obtention d'une propriété  
 L’exemple suivant obtient la `HostName` propriété (de l’objet **SqlServer** créé précédemment) et la stocke dans une variable locale.  
  
```  
DECLARE @property varchar(255);  
EXEC @hr = sp_OAMethod @object, 'HostName', @property OUT;  
IF @hr <> 0  
BEGIN  
   EXEC sp_OAGetErrorInfo @object  
    RETURN  
END;  
PRINT @property;  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Procédures stockées OLE Automation &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/ole-automation-stored-procedures-transact-sql.md)   
 [Exemple de script OLE Automation](../../relational-databases/stored-procedures/ole-automation-sample-script.md)  
  
  
