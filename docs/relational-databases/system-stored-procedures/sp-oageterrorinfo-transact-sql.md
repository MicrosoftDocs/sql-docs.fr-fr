---
description: sp_OAGetErrorInfo (Transact-SQL)
title: sp_OAGetErrorInfo (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_OAGetErrorInfo_TSQL
- sp_OAGetErrorInfo
dev_langs:
- TSQL
helpviewer_keywords:
- sp_OAGetErrorInfo
ms.assetid: ceecea08-456f-4819-85d9-ecc9647d7187
author: markingmyname
ms.author: maghan
ms.openlocfilehash: d55126c7feb362b910e83034a66e4d691f1be84c
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99191973"
---
# <a name="sp_oageterrorinfo-transact-sql"></a>sp_OAGetErrorInfo (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Obtient des informations d'erreur OLE Automation.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_OAGetErrorInfo [ objecttoken ]  
    [ , source OUTPUT ]   
    [ , description OUTPUT ]   
    [ , helpfile OUTPUT ]   
    [ , helpid OUTPUT ]   
```  
  
## <a name="arguments"></a>Arguments  
 *jeton_d'*  
 Est le jeton d’objet d’un objet OLE créé précédemment à l’aide de **sp_OACreate** ou il a la valeur null. Si *jeton_don* est spécifié, les informations d’erreur pour cet objet sont retournées. Si NULL est spécifié, les informations d'erreur sont renvoyées pour l'ensemble du traitement.  
  
  **sortie** source  
 Source des informations d'erreur. S’il est spécifié, il doit s’agir d’une variable locale **char**, **nchar**, **varchar** ou **nvarchar** . Si nécessaire, la valeur renvoyée est tronquée pour s'adapter à la variable locale.  
  
  **sortie** de la description  
 Description de l’erreur. S’il est spécifié, il doit s’agir d’une variable locale **char**, **nchar**, **varchar** ou **nvarchar** . Si nécessaire, la valeur renvoyée est tronquée pour s'adapter à la variable locale.  
  
  **sortie** HelpFile  
 Fichier d'aide de l'objet OLE. S’il est spécifié, il doit s’agir d’une variable locale **char**, **nchar**, **varchar** ou **nvarchar** . Si nécessaire, la valeur renvoyée est tronquée pour s'adapter à la variable locale.  
  
  **sortie** HelpID  
 ID de contexte du fichier d'aide. S’il est spécifié, il doit s’agir d’une variable **int** locale.  
  
> [!NOTE]  
>  Les paramètres pour cette procédure stockée sont spécifiés par position et non pas par nom.  
  
## <a name="return-code-values"></a>Codet de retour  
 0 (succès) ou un nombre différent de zéro (échec), qui représente la valeur entière de HRESULT renvoyée par l'objet OLE Automation.  
  
 Pour plus d’informations sur les codes de retour HRESULT, consultez [codes de retour OLE Automation et informations sur les erreurs](../../relational-databases/stored-procedures/ole-automation-return-codes-and-error-information.md).  
  
## <a name="result-sets"></a>Jeux de résultats  
 Si aucun paramètre de sortie n'est spécifié, les informations d'erreur sont renvoyées au client sous la forme d'un ensemble de résultats.  
  
|Noms des colonnes|Type de données|Description|  
|------------------|---------------|-----------------|  
|**Error**|**binaire (4)**|Représentation binaire du numéro d'erreur.|  
|**Source**|**nvarchar (NN)**|Source de l'erreur.|  
|**Description**|**nvarchar (NN)**|Description de l'erreur.|  
|**HelpFile**|**nvarchar (NN)**|Fichier d'aide pour la source.|  
|**HelpID**|**int**|ID du contexte de l'aide dans le fichier source d'aide.|  
  
## <a name="remarks"></a>Notes  
 Chaque appel à une procédure stockée OLE Automation (à l’exception de **sp_OAGetErrorInfo**) réinitialise les informations d’erreur ; par conséquent, **sp_OAGetErrorInfo** obtient des informations d’erreur uniquement pour l’appel de procédure stockée OLE Automation le plus récent. Notez que dans la mesure où **sp_OAGetErrorInfo** ne réinitialise pas les informations d’erreur, il peut être appelé plusieurs fois pour recevoir les mêmes informations d’erreur.  
  
 Le tableau suivant donne la liste des erreurs OLE Automation et leurs causes courantes.  
  
|Erreur et HRESULT|Cause courante|  
|-----------------------|------------------|  
|**Mauvais type de variable (0x80020008)**|Le type de données d’une [!INCLUDE[tsql](../../includes/tsql-md.md)] valeur passée comme paramètre de méthode ne correspond pas au [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[vbprvb](../../includes/vbprvb-md.md)] type de données du paramètre de méthode, ou une valeur null a été passée comme paramètre de méthode.|  
|**Nom inconnu (0x8002006)**|La propriété ou le nom de méthode spécifié n'a pu être trouvé pour l'objet spécifié.|  
|**Chaîne de classe non valide (0x800401f3)**|Le ProgID ou le CLSID spécifié n'est pas inscrit en tant qu'objet OLE sur une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Les serveurs OLE Automation personnalisés doivent être inscrits avant de pouvoir être instanciés à l’aide de **sp_OACreate**. Pour ce faire, vous pouvez utiliser l’utilitaire Regsvr32.exe pour les serveurs in-process (. dll) ou le commutateur de ligne de commande **/regserver** pour les serveurs locaux (. exe).|  
|**Échec d'exécution du serveur (0x80080005)**|L'objet OLE spécifié est inscrit comme serveur OLE local (fichier .EXE), mais le fichier .EXE ne peut pas être trouvé ou démarré.|  
|**Le module spécifié est introuvable (0x8007007e)**|L'objet OLE spécifié est inscrit comme serveur OLE in-process (fichier .DLL), mais le fichier .DLL ne peut pas être trouvé ou chargé.|  
|**Discordance des types (0x80020005)**|Le type de données d'une variable locale [!INCLUDE[tsql](../../includes/tsql-md.md)] utilisée pour stocker une valeur de propriété renvoyée ou une valeur de retour de méthode ne correspond pas au type de données [!INCLUDE[vbprvb](../../includes/vbprvb-md.md)] de la valeur de retour de la propriété ou de la méthode. La raison peut aussi être que la valeur de retour d'une propriété ou d'une méthode était requise, mais qu'aucune valeur n'a été renvoyée.|  
|**Le type de données ou la valeur du paramètre’Context’de sp_OACreate n’est pas valide. (0x8004275B)**|La valeur du paramètre de contexte doit être : 1, 4 ou 5.|  
  
 Pour plus d’informations sur le traitement des codes de retour HRESULT, consultez [codes de retour OLE Automation et informations sur les erreurs](../../relational-databases/stored-procedures/ole-automation-return-codes-and-error-information.md).  
  
## <a name="permissions"></a>Autorisations  
 Requiert l’appartenance au rôle serveur fixe **sysadmin** ou l’autorisation EXECUTE directement sur cette procédure stockée. `Ole Automation Procedures` la configuration doit être **activée** pour pouvoir utiliser toute procédure système liée à OLE Automation.  
  
## <a name="examples"></a>Exemples  
 L'exemple suivant affiche des informations d'erreur OLE Automation.  
  
```  
DECLARE @output varchar(255);  
DECLARE @hr int;  
DECLARE @source varchar(255);  
DECLARE @description varchar(255);  
PRINT 'OLE Automation Error Information';  
EXEC @hr = sp_OAGetErrorInfo @object, @source OUT, @description OUT;  
IF @hr = 0  
BEGIN  
    SELECT @output = '  Source: ' + @source  
    PRINT @output  
    SELECT @output = '  Description: ' + @description  
    PRINT @output  
END  
ELSE  
BEGIN  
    PRINT '  sp_OAGetErrorInfo failed.'  
    RETURN  
END;  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Procédures stockées OLE Automation &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/ole-automation-stored-procedures-transact-sql.md)   
 [Exemple de script OLE Automation](../../relational-databases/stored-procedures/ole-automation-sample-script.md)  
  
  
