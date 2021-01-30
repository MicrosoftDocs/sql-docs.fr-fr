---
description: sp_OACreate (Transact-SQL)
title: sp_OACreate (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_OACreate
- sp_OACreate_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_OACreate
ms.assetid: eb84c0f1-26dd-48f9-9368-13ee4a30a27c
author: markingmyname
ms.author: maghan
ms.openlocfilehash: bd6d44aedf1328234cf51366c3e81bcfb2d6dc94
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99191988"
---
# <a name="sp_oacreate-transact-sql"></a>sp_OACreate (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Crée une instance d'un objet OLE.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_OACreate { progid | clsid } , objecttoken OUTPUT [ , context ]   
```  
  
## <a name="arguments"></a>Arguments  
 *ProgID*  
 Identificateur de programme (ProgID) de l'objet OLE à créer. Cette chaîne de caractères décrit la classe de l’objet OLE et se présente sous la forme : **'**_OLEComponent_**.** _Objet_**'**  
  
 *OLEComponent* est le nom du composant du serveur OLE Automation, et *Object* est le nom de l’objet OLE. L’objet OLE spécifié doit être valide et doit prendre en charge l’interface **IDispatch** .  
  
 Par exemple, SQLDMO. SQLServer est le ProgID de l’objet SQL-DMO **SqlServer** . SQL-DMO a le nom de composant SQLDMO, l’objet **SqlServer** est valide et (comme tous les objets SQL-DMO) l’objet **SqlServer** prend en charge **IDispatch**.  
  
 *identificateur*  
 Identificateur de classe (CLSID) de l'objet OLE à créer. Cette chaîne de caractères décrit la classe de l’objet OLE et se présente sous la forme suivante : **' {**_nnnnnnnn-nnnn-nnnn-nnnn-nnnnnnnnnnnn_**} '**. L’objet OLE spécifié doit être valide et doit prendre en charge l’interface **IDispatch** .  
  
 Par exemple, {00026BA1-0000-0000-C000-000000000046} est le CLSID de l’objet SQL-DMO **SqlServer** .  
  
  **sortie** jeton_d'  
 Est le jeton d’objet retourné, et doit être une variable locale de type de données **int**. Ce jeton d’objet identifie l’objet OLE créé et est utilisé dans les appels aux autres procédures stockées OLE Automation.  
  
 *context*  
 Spécifie le contexte d'exécution dans lequel l'objet OLE nouvellement créé s'exécute. Si cet argument est spécifié, il doit avoir une des valeurs suivantes :  
  
 **1** = serveur OLE in-process (. dll) uniquement.  
  
 **4** = serveur OLE local (. exe) uniquement.  
  
 **5** = serveur OLE en cours et local autorisé  
  
 S’il n’est pas spécifié, la valeur par défaut est **5**. Cette valeur est transmise en tant que paramètre *dwClsContext* de l’appel à **CoCreateInstance**.  
  
 Si un serveur OLE in-process est autorisé (en utilisant une valeur de contexte de **1** ou **5** ou en ne spécifiant pas de valeur de contexte), il a accès à la mémoire et à d’autres ressources appartenant à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Un serveur OLE in-process peut endommager la mémoire ou les ressources [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et provoquer des résultats imprévisibles, comme une violation d'accès [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 Lorsque vous spécifiez une valeur de contexte de **4**, un serveur OLE local n’a accès à aucune [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ressource et il ne peut pas endommager la [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] mémoire ou les ressources.  
  
> [!NOTE]  
>  Les paramètres de cette procédure stockée sont spécifiés par position, et non pas par nom.  
  
## <a name="return-code-values"></a>Codet de retour  
 0 (succès) ou un nombre différent de zéro (échec), qui représente la valeur entière de HRESULT renvoyée par l'objet OLE Automation.  
  
 Pour plus d’informations sur les codes de retour HRESULT, consultez [codes de retour OLE Automation et informations sur les erreurs](../../relational-databases/stored-procedures/ole-automation-return-codes-and-error-information.md).  
  
## <a name="remarks"></a>Notes  
 Si les procédures OLE Automation sont activées, un appel à **sp_OACreate** démarrera l’environnement d’exécution partagé OLE Automation. Pour plus d’informations sur l’activation de l’automatisation OLE, consultez [procédures OLE Automation (option de configuration de serveur)](../../database-engine/configure-windows/ole-automation-procedures-server-configuration-option.md).  
  
 L'objet OLE créé est automatiquement détruit à la fin du lot d'instructions [!INCLUDE[tsql](../../includes/tsql-md.md)].  
  
## <a name="permissions"></a>Autorisations  
 Requiert l’appartenance au rôle serveur fixe **sysadmin** ou l’autorisation EXECUTE directement sur cette procédure stockée. `Ole Automation Procedures` la configuration doit être **activée** pour pouvoir utiliser toute procédure système liée à OLE Automation.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-using-progid"></a>R. Utilisation de ProgID  
 L’exemple suivant crée un objet **SqlServer** SQL-DMO à l’aide de son ProgID.  
  
```  
DECLARE @object int;  
DECLARE @hr int;  
DECLARE @src varchar(255), @desc varchar(255);  
EXEC @hr = sp_OACreate 'SQLDMO.SQLServer', @object OUT;  
IF @hr <> 0  
BEGIN  
   EXEC sp_OAGetErrorInfo @object, @src OUT, @desc OUT   
   raiserror('Error Creating COM Component 0x%x, %s, %s',16,1, @hr, @src, @desc)  
    RETURN  
END;  
GO  
```  
  
### <a name="b-using-clsid"></a>B. Utilisation de CLSID  
 L’exemple suivant crée un objet **SqlServer** SQL-DMO à l’aide de son CLSID.  
  
```  
DECLARE @object int;  
DECLARE @hr int;  
DECLARE @src varchar(255), @desc varchar(255);  
EXEC @hr = sp_OACreate '{00026BA1-0000-0000-C000-000000000046}',  
    @object OUT;  
IF @hr <> 0  
BEGIN  
   EXEC sp_OAGetErrorInfo @object, @src OUT, @desc OUT   
   raiserror('Error Creating COM Component 0x%x, %s, %s',16,1, @hr, @src, @desc)  
    RETURN  
END;  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Procédures stockées OLE Automation &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/ole-automation-stored-procedures-transact-sql.md)   
 [OLE Automation procedures (option de configuration de serveur)](../../database-engine/configure-windows/ole-automation-procedures-server-configuration-option.md)   
 [Exemple de script OLE Automation](../../relational-databases/stored-procedures/ole-automation-sample-script.md)  
  
  
